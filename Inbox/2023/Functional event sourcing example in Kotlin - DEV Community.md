---
created: 2023-11-15T06:52:55 (UTC -03:00)
tags: [kotlin,functional,eventsourcing,ddd,software,coding,development,engineering,inclusive,community]
source: https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245
author: Jakub Zalas
---

# Functional event sourcing example in Kotlin - DEV Community

> ## Excerpt
> Last time we discussed a functional modeling approach to event sourcing. Let's now consider an...

---
Last time we discussed a [functional modeling approach to event sourcing](https://dev.to/jakub_zalas/functional-event-sourcing-1ea5). Let's now consider an implementation example in Kotlin.

The problem we'll be working with is the [Mastermind game](https://github.com/jakzal/mastermind). If you're unfamiliar with the game, you can learn about it from Wikipedia or this [GitHub repository](https://github.com/jakzal/mastermind).

As before, we're still only focusing on the domain model.

[![Domain command handler](https://res.cloudinary.com/practicaldev/image/fetch/s--tK6bY25C--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eu2obot7gn6u33llkg01.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--tK6bY25C--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eu2obot7gn6u33llkg01.png)

Domain command handler

We're interested in the behaviour and types of the `execute()` function that we discussed in the [previous post](https://dev.to/jakub_zalas/functional-event-sourcing-1ea5). Here's how the function signature will look like for our game:  

```
fun execute(command: GameCommand, game: Game)
  : Either<GameError, NonEmptyList<GameEvent>> = TODO()
```

Enter fullscreen mode Exit fullscreen mode

Domain command handler

___

  
**Note**: The solution presented here has been developed with TDD. However, our goal now is to show the final solution rather than how we have arrived at it with refactoring. This is an important note, as I wouldn't like to introduce confusion that what we do here is TDD. TDD is better shown in videos than in the written word.  

  
Furthermore, since we've started the series with the model, it might appear as if we're working inside-out. In practice, it's usually the other way around. We work outside-in, starting at the boundaries of the system.  

___

## [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#event-model)Event model

Let's start with the output of an event modeling workshop to visualise what needs to be done.

> Event Modeling is a visual analysis technique that focuses heavily on identifying meaningful events happening in a business process.
> 
> Yves Goeleven, [What is Event Modeling? (with example)](https://www.goeleven.com/blog/event-modeling/)

Diagrams below reveal the commands (blue), events (orange), and error cases (red) the Mastermind game might need. We can also see the views (green), but we're not concerned with them on this occasion.

Playing the game means we join a new game and make guesses.

[![Mastermind game event model (playing the game)](https://res.cloudinary.com/practicaldev/image/fetch/s--hGO154Db--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gpp8uajf76i3zbxt9j0u.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--hGO154Db--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gpp8uajf76i3zbxt9j0u.png)

Playing the game

Eventually, we might make a guess that matches the secret and win the game.

[![Mastermind game event model (winning the game)](https://res.cloudinary.com/practicaldev/image/fetch/s--vSGB8-4S--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mx4f9betjjcyrv7gxwiv.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--vSGB8-4S--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mx4f9betjjcyrv7gxwiv.png)

Winning the game

Or, we might run out of available attempts and lose the game.

[![Mastermind game event model (losing the game)](https://res.cloudinary.com/practicaldev/image/fetch/s--65HpkyOT--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zwvn3djkchkzvxwp2ma9.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--65HpkyOT--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zwvn3djkchkzvxwp2ma9.png)

Losing the game

There are also several scenarios where our guess might be rejected.

[![Mastermind game event model (error scenarios)](https://res.cloudinary.com/practicaldev/image/fetch/s--ZM1WahkC--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j94hnskwjet4ecl44lfm.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--ZM1WahkC--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j94hnskwjet4ecl44lfm.png)

Error scenarios

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#commands)Commands

In the event modeling workshop, we have identified two commands: `JoinGame` and `MakeGuess`.

[![Mastermind game commands](https://res.cloudinary.com/practicaldev/image/fetch/s--V8ohdWNZ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xbkom3c2ai4xnhy6swby.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--V8ohdWNZ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xbkom3c2ai4xnhy6swby.png)

Mastermind game commands

Commands are implemented as [data classes](https://kotlinlang.org/docs/data-classes.html) with immutable properties. What's common between the two commands is a game identifier.  

```
sealed interface GameCommand {
    val gameId: GameId

    data class JoinGame(
        override val gameId: GameId,
        val secret: Code,
        val totalAttempts: Int,
        val availablePegs: Set<Code.Peg>
    ) : GameCommand

    data class MakeGuess(
      override val gameId: GameId,
      val guess: Code
    ) : GameCommand
}
```

Enter fullscreen mode Exit fullscreen mode

Mastermind game commands implementation

All the commands that are handled by the same aggregate implement the same sealed interface. Later, we'll use the same pattern with events and errors.

___

  
**Note**: Isn't it curious how we continue to use the term "aggregate" while there's no single entity that represents the aggregate? I like to think that, conceptually, the aggregate is still there. It's represented by commands, events, and the behaviour of the `execute()` function.  

___

The key benefit of [sealed types](https://kotlinlang.org/docs/sealed-classes.html#sealed-classes-and-when-expression) comes into play with the [`when`](https://kotlinlang.org/docs/control-flow.html#when-expression) expression. It lets us rely on exhaustive matching:  

```
fun execute(command: GameCommand, game: Game) =
  when(command) {
    is JoinGame -> TODO()
    is MakeGuess -> TODO()
  }
```

Enter fullscreen mode Exit fullscreen mode

Exhaustive matching example

Let's notice there's no _else_ branch in the above example. Since our type is sealed we can be sure we covered all the possible cases. The compiler will tell us otherwise.

If we ever add a new command implementation, the compiler will point out all the places we matched on the command that need to be updated with a new branch.

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#events)Events

In the workshop, we have also identified four events: `GameStarted`, `GuessMade`, `GameWon`, and `GameLost`. _GameStarted_ will only be published in the beginning, while _GameWon_ and _GameLost_ are our terminal events.

[![Mastermind game events](https://res.cloudinary.com/practicaldev/image/fetch/s--t4nxxg7c--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vmpwoxa3cv9ct21wwfin.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--t4nxxg7c--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vmpwoxa3cv9ct21wwfin.png)

Mastermind game events

Similarly to commands, we use a sealed interface to implement all the events.  

```
sealed interface GameEvent {
    val gameId: GameId

    data class GameStarted(
        override val gameId: GameId,
        val secret: Code,
        val totalAttempts: Int,
        val availablePegs: Set<Code.Peg>
    ) : GameEvent

    data class GuessMade(
        override val gameId: GameId,
        val guess: Guess
    ) : GameEvent

    data class GameWon(override val gameId: GameId) : GameEvent

    data class GameLost(override val gameId: GameId) : GameEvent
}
```

Enter fullscreen mode Exit fullscreen mode

Mastermind game events implementation

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#errors)Errors

To be honest, not all of these error cases were identified in the modeling workshop. Many were discovered in the implementation phase and added to the diagram later.

[![Mastermind game errors](https://res.cloudinary.com/practicaldev/image/fetch/s--2umtBvy3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l0hjggi21rfjmvir3biu.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--2umtBvy3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l0hjggi21rfjmvir3biu.png)

Mastermind game errors

Yet again, sealed interfaces come in handy. This time the hierarchy is a bit more nested. This way we'll be able to perform a more fine-grained matching later on when it comes to handling errors and converting them to responses.  

```
sealed interface GameError {
  val gameId: GameId

  sealed interface GameFinishedError : GameError {
    data class GameAlreadyWon(
      override val gameId: GameId
    ) : GameFinishedError

    data class GameAlreadyLost(
      override val gameId: GameId
    ) : GameFinishedError
  }

  sealed interface GuessError : GameError {
    data class GameNotStarted(
      override val gameId: GameId
    ) : GuessError

    data class GuessTooShort(
      override val gameId: GameId,
      val guess: Code,
      val requiredLength: Int
    ) : GuessError

    data class GuessTooLong(
      override val gameId: GameId,
      val guess: Code,
      val requiredLength: Int
    ) : GuessError

    data class InvalidPegInGuess(
      override val gameId: GameId,
      val guess: Code,
      val availablePegs: Set<Code.Peg>
    ) : GuessError
  }
}
```

Enter fullscreen mode Exit fullscreen mode

Mastermind game errors implementation

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#other-domain-types)Other domain types

Notice how we tend to avoid primitive types in our commands, events, and errors.

[![Other domain types supporting commands, events, and errors](https://res.cloudinary.com/practicaldev/image/fetch/s--YirE9tIU--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cysnwujogbz36x7vc84n.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--YirE9tIU--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/cysnwujogbz36x7vc84n.png)

Other domain types supporting commands, events, and errors

Rich types help us to express our domain better and prevent us from making simple mistakes (i.e. is this string a game ID or a player ID?).  

```
@JvmInline
value class GameId(val value: String)

data class Code(val pegs: List<Peg>) {
  constructor(vararg pegs: String) : this(pegs.map(::Peg))

  data class Peg(val name: String)

  val length: Int get() = pegs.size
}

data class Guess(val code: Code, val feedback: Feedback)

data class Feedback(val outcome: Outcome, val pegs: List<Peg>) {
  constructor(outcome: Outcome, vararg pegs: Peg) :
    this(outcome, pegs.toList())

  enum class Peg { BLACK, WHITE }

  enum class Outcome { IN_PROGRESS, WON, LOST }
}
```

Enter fullscreen mode Exit fullscreen mode

Implementation of remaining domain types

## [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#behaviours)Behaviours

With the right workshop, we can discover a lot of types and behaviours upfront. It's nearly impossible to think of everything upfront though. We fear not, as when it comes to the implementation, the tests we write tend to guide us and fill in the gaps.

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#joining-the-game)Joining the game

Joining the game should result in the _GameStarted_ event.

[![Joining the game](https://res.cloudinary.com/practicaldev/image/fetch/s--ZNpe_qEV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0dkz0wdsd7tc8umo0avj.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--ZNpe_qEV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0dkz0wdsd7tc8umo0avj.png)

Joining the game

Test cases for domain model behaviours could not be simpler. We need to arrange fixtures, execute the tested behaviour, and verify the outcome. The good old Arrange/Act/Assert, a.k.a Given/When/Then. Meszaros calls it a [four-phase test](http://xunitpatterns.com/Four%20Phase%20Test.html), but in our case, the fourth phase will be implicitly done by the test framework and there's nothing left to tear down.

[![Four-Phase Test](https://res.cloudinary.com/practicaldev/image/fetch/s--ZFMEPf1y--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/67bdfqr7zgmvt3cm8ofu.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--ZFMEPf1y--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/67bdfqr7zgmvt3cm8ofu.png)

Four-phase test

Since we planned for our domain model to be purely functional, it's easy to stick to this structure to keep the tests short and readable.

In the test case below, the [test fixture](http://xunitpatterns.com/test%20fixture%20-%20xUnit.html) (test context) is defined as properties of the test class. The test case creates the command, executes it, and verifies we got the expected events in response. This is how all our tests for the domain model will look like.  

```
class GameExamples {
  private val gameId = anyGameId()
  private val secret = Code("Red", "Green", "Blue", "Yellow")
  private val totalAttempts = 12
  private val availablePegs = setOfPegs("Red", "Green", "Blue", "Yellow", "Purple", "Pink")

  @Test
  fun `it starts the game`() {
    val command = JoinGame(gameId, secret, totalAttempts, availablePegs)

    execute(command) shouldSucceedWith listOf(
      GameStarted(
        gameId,
        secret,
        totalAttempts,
        availablePegs
      )
    )
  }
}
```

Enter fullscreen mode Exit fullscreen mode

Joining the game test case

The actual result of _execute()_ is of type `Either<GameError, NonEmptyList<GameEvent>>`, so when verifying the outcome we need to confirm whether we received the left or the right side of it. _Left_ is by convention used for the error case, while _Right_ is for success.

[![Either a GameError or a list of GameEvents](https://res.cloudinary.com/practicaldev/image/fetch/s--Ie82-sN---/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fim9hzjf41jx4hxzt3l5.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Ie82-sN---/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fim9hzjf41jx4hxzt3l5.png)

Either a GameError or a list of GameEvents

We created the `shouldSucceedWith()` and `shouldFailWith()` extension functions in the hope of removing some noise from the tests.  

```
infix fun <A, B> Either<A, B>.shouldSucceedWith(expected: B) =
  assertEquals(expected.right(), this, "${expected.right()} is $this")

infix fun <A, B> Either<A, B>.shouldFailWith(expected: A) =
  assertEquals(expected.left(), this, "${expected.left()} is $this")
```

Enter fullscreen mode Exit fullscreen mode

Custom assertions

  
Read more about [Either](https://arrow-kt.io/learn/typed-errors/either-and-ior/) and [typed errors](https://arrow-kt.io/learn/typed-errors/) in Arrow's documentation.  

Since for this first case, there are no error scenarios or any complex calculations, we only need to return a list with the _GameStarted_ event inside. Matching the command with `when()` gives us access to command properties for the matched type thanks to smart casts.  

```
typealias Game = List<GameEvent>

fun notStartedGame(): Game = emptyList()

fun execute(
  command: GameCommand,
  game: Game = notStartedGame()
): Either<GameError, NonEmptyList<GameEvent>> = when (command) {
  is JoinGame -> joinGame(command),
  is MakeGuess -> TODO()
}

private fun joinGame(command: JoinGame) =
  either<Nothing, NonEmptyList<GameStarted>> {
    nonEmptyListOf(
      GameStarted(
        command.gameId,
        command.secret,
        command.totalAttempts,
        command.availablePegs
      )
    )
  }
}
```

Enter fullscreen mode Exit fullscreen mode

Starting the game

Notice the `either {}` [builder](https://arrow-kt.io/learn/typed-errors/either-and-ior/#using-builders) above. It makes sure the value we return from its block is wrapped in an instance of `Either`, in this case, the `Right` instance. `either { nonEmptyListOf(GameStarted())) }` is an equivalent of `nonEmptyListOf(GameStarted())).right()`.

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#making-a-guess)Making a guess

Making a valid guess should result in the _GuessMade_ event.

[![Making a guess](https://res.cloudinary.com/practicaldev/image/fetch/s--bTbZNnxN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5d6hq4z9hbk3nz8s280k.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--bTbZNnxN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5d6hq4z9hbk3nz8s280k.png)

Making a guess

This time, we additionally need to prepare the state of the game, since _MakeGuess_ is never executed as the first command. The game must've been started first.  

```
@Test
fun `it makes a guess`() {
  val command = MakeGuess(gameId, Code("Purple", "Purple", "Purple", "Purple"))
  val game = gameOf(
    GameStarted(gameId, secret, totalAttempts, availablePegs)
  )

  execute(command, game) shouldSucceedWith listOf(
    GuessMade(
      gameId,
      Guess(
        Code("Purple", "Purple", "Purple", "Purple"),
        Feedback(IN_PROGRESS)
      )
    )
  )
}
```

Enter fullscreen mode Exit fullscreen mode

Making a guess test case

The state is created based on past events. We delegated this task to the `gameOf()` function. Since for now, we treat the list of events as the state, we only need to return the list. Later on, we'll see how to convert it to an actual state object.  

```
fun gameOf(vararg events: GameEvent): Game = listOf(*events)
```

Enter fullscreen mode Exit fullscreen mode

Creating the game state

Now we need to return the _GuessMade_ event for the second `when()` branch.  

```
fun execute(
  command: GameCommand,
  game: Game = notStartedGame()
): Either<GameError, NonEmptyList<GameEvent>> = when (command) {
  is JoinGame -> joinGame(command)
  is MakeGuess -> makeGuess(command, game)
}

private fun makeGuess(command: MakeGuess, game: Game) =
  either<GameError, NotEmptyList<GuessMade>> {
    nonEmptyListOf(
      GuessMade(
        command.gameId,
        Guess(command.guess, game.feedbackOn(command.guess))
      )
    )
  }

private fun Game.feedbackOn(guess: Code) = Feedback(IN_PROGRESS)
```

Enter fullscreen mode Exit fullscreen mode

Making a guess

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#receiving-feedback)Receiving feedback

Feedback should be given on each valid guess with the _GuessMade_ event.

[![Receiving feedback on the guess](https://res.cloudinary.com/practicaldev/image/fetch/s--bTbZNnxN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5d6hq4z9hbk3nz8s280k.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--bTbZNnxN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5d6hq4z9hbk3nz8s280k.png)

Receiving feedback on the guess

Here's one of the test cases for receiving feedback.  

```
fun `it gives feedback on the guess`() { 
  val secret = Code("Red", "Green", "Blue", "Yellow")
  val guess = Code("Red", "Purple", "Blue", "Purple")
  val game = gameOf(
    GameStarted(gameId, secret, totalAttempts, availablePegs)
  )

  execute(MakeGuess(gameId, guess), game) shouldSucceedWith listOf(
    GuessMade(gameId, Guess(
      guess,
      Feedback(IN_PROGRESS, BLACK, BLACK))
    )
  )
}
```

Enter fullscreen mode Exit fullscreen mode

Receiving feedback test case

We'll need a few more similar test cases to develop the logic for giving feedback. They're hidden below if you're interested.

Receiving feedback test cases  

```
@TestFactory
fun `it gives feedback on the guess`() = guessExamples { (secret: Code, guess: Code, feedback: Feedback) ->
  val game = gameOf(
    GameStarted(gameId, secret, totalAttempts, availablePegs)
  )

  execute(MakeGuess(gameId, guess), game) shouldSucceedWith listOf(
    GuessMade(gameId, Guess(guess, feedback))
  )
}

private fun guessExamples(block: (Triple<Code, Code, Feedback>) -> Unit) = mapOf(
  "it gives a black peg for each code peg on the correct position" to Triple(
    Code("Red", "Green", "Blue", "Yellow"),
    Code("Red", "Purple", "Blue", "Purple"),
    Feedback(IN_PROGRESS, BLACK, BLACK)
  ),
  "it gives no black peg for code peg duplicated on a wrong position" to Triple(
    Code("Red", "Green", "Blue", "Yellow"),
    Code("Red", "Red", "Purple", "Purple"),
    Feedback(IN_PROGRESS, BLACK)
  ),
  "it gives a white peg for code peg that is part of the code but is placed on a wrong position" to Triple(
    Code("Red", "Green", "Blue", "Yellow"),
    Code("Purple", "Red", "Purple", "Purple"),
    Feedback(IN_PROGRESS, WHITE)
  ),
  "it gives no white peg for code peg duplicated on a wrong position" to Triple(
    Code("Red", "Green", "Blue", "Yellow"),
    Code("Purple", "Red", "Red", "Purple"),
    Feedback(IN_PROGRESS, WHITE)
  ),
  "it gives a white peg for each code peg on a wrong position" to Triple(
    Code("Red", "Green", "Blue", "Red"),
    Code("Purple", "Red", "Red", "Purple"),
    Feedback(IN_PROGRESS, WHITE, WHITE)
  )
).dynamicTestsFor(block)

fun <T : Any> Map<String, T>.dynamicTestsFor(block: (T) -> Unit) =
  map { (message, example: T) -> DynamicTest.dynamicTest(message) { block(example) } }
```

Enter fullscreen mode Exit fullscreen mode

All receiving feedback test cases

Eventually, we arrived at the solution below.  

```
private fun Game.feedbackOn(guess: Code): Feedback =
  feedbackPegsOn(guess)
    .let { (exactHits, colourHits) ->
      Feedback(IN_PROGRESS, exactHits + colourHits)
    }

private fun Game.feedbackPegsOn(guess: Code) =
  exactHits(guess).map { BLACK } to colourHits(guess).map { WHITE }
```

Enter fullscreen mode Exit fullscreen mode

Calculating feedback on the guess

We've hidden `Game.exactHits()` and `Game.colourHits()` below as they're not necessarily relevant. The point is we identify pegs on the correct and incorrect positions and convert them to BLACK and WHITE pegs respectively.

Exact hits and colour hits  

```
private fun Game.exactHits(guess: Code): List<Code.Peg> = 
  this.secretPegs
    .zip(guess.pegs)
    .filter { (secretColour, guessColour) -> secretColour == guessColour }
    .unzip()
    .second

private fun Game.colourHits(guess: Code): List<Code.Peg> =
  this.secretPegs
    .zip(guess.pegs)
    .filter { (secretColour, guessColour) -> secretColour != guessColour }
    .unzip()
    .let { (secret, guess) ->
        guess.fold(secret to emptyList<Code.Peg>()) { (secretPegs, colourHits), guessPeg ->
            secretPegs.remove(guessPeg)?.let { it to colourHits + guessPeg } ?: (secretPegs to colourHits)
        }.second
    }

/**
 * Removes an element from the list and returns the new list, or null if the element wasn't found.
 */
private fun <T> List<T>.remove(item: T): List<T>? = indexOf(item).let { index ->
    if (index != -1) filterIndexed { i, _ -> i != index }
    else null
}
```

Enter fullscreen mode Exit fullscreen mode

Detailed calculations of exact and colour hits

What's more interesting is how we accessed the state of the game - namely the secret code and its pegs. The secret is available with the first _GameStarted_ event. Since we have access to the whole event history for the given game, we could filter it for the information we need. We achieved it with extension functions on the `Game` type (which is actually a `List<GameEvent>` for now).  

```
private val Game.secret: Code?
    get() = filterIsInstance<GameStarted>().firstOrNull()?.secret

private val Game.secretPegs: List<Code.Peg>
    get() = secret?.pegs ?: emptyList()
```

Enter fullscreen mode Exit fullscreen mode

Deriving state properties

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#enforcing-invariants)Enforcing invariants

> invariants: rules that have to be protected at all times
> 
> "Learning Domain-Driven Design" by Vladik Khononov.

[![Rejecting a command](https://res.cloudinary.com/practicaldev/image/fetch/s--lfFZWfsE--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7zp14gjka7gwq1hp7zfg.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--lfFZWfsE--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7zp14gjka7gwq1hp7zfg.png)

Rejecting a command

Commands that might put our game in an inconsistent state should be rejected. For example, we can't make a guess for a game that hasn't started or has finished. Also, guesses with a code that's of a different length to the secret are incomplete. Finally, we can't make guesses with pegs that are not part of the game.

> An invariant is a state that must stay transactionally consistent throughout the Entity life cycle.
> 
> "Implementing Domain-Driven Design" by Vaughn Vernon

#### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#game-not-started)Game not started

The game should not be played if it has not been started.  

```
@Test
fun `the game cannot be played if it was not started`() {
  val code = Code("Red", "Purple", "Red", "Purple")
  val game = notStartedGame()

  execute(MakeGuess(gameId, code), game) shouldFailWith
    GameNotStarted(gameId)
}
```

Enter fullscreen mode Exit fullscreen mode

Game not started test case

Our approach to validate an argument is to pass it to a validation function that returns the validated value or an error. That's the job for `startedNotFinishedGame()` below. Once the game is returned from the validation function, we can be sure that it was started. We can then pass it to the next function with `map()`. In case of an error, _map_ will short-circuit by returning the error, and the next function won't be called.  

```
private fun makeGuess(command: MakeGuess, game: Game) =
  startedNotFinishedGame(command, game).map { startedGame ->
    nonEmptyListOf(
      GuessMade(
        command.gameId,
        Guess(command.guess, startedGame.feedbackOn(command.guess))
      )
    )
  }

private fun startedNotFinishedGame(command: MakeGuess, game: Game): Either<GameError, Game> {
  if (!game.isStarted()) {
    return GameNotStarted(command.gameId).left()
  }
  return game.right()
}
```

Enter fullscreen mode Exit fullscreen mode

Validating if the game is started

```
private fun Game.isStarted(): Boolean =
    filterIsInstance<GameStarted>().isNotEmpty()
```

Enter fullscreen mode Exit fullscreen mode

Deriving state properties for validation

#### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#guess-is-invalid)Guess is invalid

The guess code needs to be of the same length as the secret. Furthermore, it should only contain pegs that the game has been started with.  

```
@Test
fun `the guess length cannot be shorter than the secret`() {
  val secret = Code("Red", "Green", "Blue", "Yellow")
  val code = Code("Purple", "Purple", "Purple")
  val game = gameOf(GameStarted(gameId, secret, 12, availablePegs))

  execute(MakeGuess(gameId, code), game) shouldFailWith 
    GuessTooShort(gameId, code, secret.length)
}

@Test
fun `the guess length cannot be longer than the secret`() {
  val secret = Code("Red", "Green", "Blue", "Yellow")
  val code = Code("Purple", "Purple", "Purple", "Purple", "Purple")
  val game = gameOf(GameStarted(gameId, secret, 12, availablePegs))

  execute(MakeGuess(gameId, code), game) shouldFailWith 
    GuessTooLong(gameId, code, secret.length)
}

@Test
fun `it rejects pegs that the game was not started with`() {
  val secret = Code("Red", "Green", "Blue", "Blue")
  val availablePegs = setOfPegs("Red", "Green", "Blue")
  val game = gameOf(GameStarted(gameId, secret, 12, availablePegs))
  val guess = Code("Red", "Green", "Blue", "Yellow")

  execute(MakeGuess(gameId, guess), game) shouldFailWith
    InvalidPegInGuess(gameId, guess, availablePegs)
}
```

Enter fullscreen mode Exit fullscreen mode

Guess validation test cases

Here, we use the same trick with a validation function to validate the guess. Notice we have to use `flatMap()` instead of `map()` in the outer call. Otherwise an _Either_ result would be placed inside another _Either_.  

```
private fun makeGuess(command: MakeGuess, game: Game) =
  startedNotFinishedGame(command, game).flatMap { startedGame ->
    validGuess(command, startedGame).map { guess ->
      nonEmptyListOf(
        GuessMade(
          command.gameId,
          Guess(command.guess, startedGame.feedbackOn(guess))
        )
      )
    }
  }

private fun validGuess(command: MakeGuess, game: Game): Either<GameError, Code> {
  if (game.isGuessTooShort(command.guess)) {
    return GuessTooShort(command.gameId, command.guess, game.secretLength).left()
  }
  if (game.isGuessTooLong(command.guess)) {
    return GuessTooLong(command.gameId, command.guess, game.secretLength).left()
  }
  if (!game.isGuessValid(command.guess)) {
    return InvalidPegInGuess(command.gameId, command.guess, game.availablePegs).left()
  }
  return command.guess.right()
}
```

Enter fullscreen mode Exit fullscreen mode

Guess validation

```
private fun Game.isGuessTooShort(guess: Code): Boolean =
  guess.length < this.secretLength

private fun Game.isGuessTooLong(guess: Code): Boolean =
  guess.length > this.secretLength

private fun Game.isGuessValid(guess: Code): Boolean =
  availablePegs.containsAll(guess.pegs)
```

Enter fullscreen mode Exit fullscreen mode

Deriving state properties for guess validation

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#winning-the-game)Winning the game

Winning the game means publishing the _GameWon_ event in addition to the usual _GuessMade_.

[![Winning the game](https://res.cloudinary.com/practicaldev/image/fetch/s--i6rkX5ql--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/s9pbn94pp3k206dx8bz1.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--i6rkX5ql--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/s9pbn94pp3k206dx8bz1.png)

Winning the game

```
@Test
fun `the game is won if the secret is guessed`() {
  val game = gameOf(GameStarted(gameId, secret, totalAttempts, availablePegs))

  execute(MakeGuess(gameId, secret), game) shouldSucceedWith listOf(
    GuessMade(gameId, Guess(secret, Feedback(WON, BLACK, BLACK, BLACK, BLACK))),
    GameWon(gameId)
  )
}
```

Enter fullscreen mode Exit fullscreen mode

Winning the game test case

We now need to decide if the game is won based on whether the number of guessed pegs is equal to the secret length.  

```
private fun Game.outcomeFor(exactHits: List<Feedback.Peg>) = when {
  exactHits.size == this.secretLength -> WON
  else -> IN_PROGRESS
}
```

Enter fullscreen mode Exit fullscreen mode

Winning the game implementation

Again, that requires a bit of state.  

```
private val Game.secretLength: Int
  get() = secret?.length ?: 0
```

Enter fullscreen mode Exit fullscreen mode

Deriving state properties

We've taken care of deciding whether the game is won. We also need to publish an additional event. To do this, we make `makeGuess()` return a single event. Next, we create the `withOutcome()` function that converts the result of `makeGuess()` to a list and optionally appends the `GameWon` event.  

```
fun execute(
    command: GameCommand,
    game: Game = notStartedGame()
): Either<GameError, NonEmptyList<GameEvent>> =
  when (command) {
    is JoinGame -> joinGame(command)
    is MakeGuess -> makeGuess(command, game)
                      .withOutcome()
  }

private fun makeGuess(command: MakeGuess, game: Game) =
  startedNotFinishedGame(command, game).flatMap { startedGame ->
    validGuess(command, startedGame).map { guess ->
      GuessMade(command.gameId, Guess(command.guess, startedGame.feedbackOn(guess)))
    }
  }

private fun Either<GameError, GuessMade>.withOutcome(): Either<GameError, NonEmptyList<GameEvent>> =
  map { event ->
    nonEmptyListOf<GameEvent>(event) +
      when (event.guess.feedback.outcome) {
        WON -> listOf(GameWon(event.gameId))
        LOST -> listOf(GameLost(event.gameId))
        else -> emptyList()
      }
  }
```

Enter fullscreen mode Exit fullscreen mode

Determining the game-terminating events

Once the game is won it shouldn't be played anymore.  

```
@Test
fun `the game can no longer be played once it's won`() {
  val game = gameOf(GameStarted(gameId, secret, totalAttempts, availablePegs))

  val update = execute(MakeGuess(gameId, secret), game)
  val updatedGame = game.updated(update)

  execute(MakeGuess(gameId, secret), updatedGame) shouldFailWith 
    GameAlreadyWon(gameId)
}

private fun Game.updated(update: Either<GameError, Game>): Game = this + update.getOrElse { emptyList() }
```

Enter fullscreen mode Exit fullscreen mode

Playing a won game test case

This requires an additional validation rule.  

```
private fun startedNotFinishedGame(command: MakeGuess, game: Game): Either<GameError, Game> {
  if (!game.isStarted()) {
    return GameNotStarted(command.gameId).left()
  }
  if (game.isWon()) {
    return GameAlreadyWon(command.gameId).left()
  }
  return game.right()
}
```

Enter fullscreen mode Exit fullscreen mode

Validation for a won game

The game is won if there has been a _GameWon_ event published.  

```
private fun Game.isWon(): Boolean =
  filterIsInstance<GameWon>().isNotEmpty()
```

Enter fullscreen mode Exit fullscreen mode

Deriving state properties

### [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#losing-the-game)Losing the game

Losing the game means publishing the _GameLost_ event in addition to the usual _GuessMade_.

[![Losing the game](https://res.cloudinary.com/practicaldev/image/fetch/s--krpAGxwK--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7p8x4nopt1t99g47bc5g.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--krpAGxwK--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7p8x4nopt1t99g47bc5g.png)

Losing the game

```
@Test
fun `the game is lost if the secret is not guessed within the number of attempts`() {
  val secret = Code("Red", "Green", "Blue", "Yellow")
  val wrongCode = Code("Purple", "Purple", "Purple", "Purple")
  val game = gameOf(
    GameStarted(gameId, secret, 3, availablePegs),
    GuessMade(gameId, Guess(wrongCode, Feedback(IN_PROGRESS))),
    GuessMade(gameId, Guess(wrongCode, Feedback(IN_PROGRESS))),
  )

  execute(MakeGuess(gameId, wrongCode), game) shouldSucceedWith listOf(
    GuessMade(gameId, Guess(wrongCode, Feedback(LOST))),
    GameLost(gameId)
  )
}
```

Enter fullscreen mode Exit fullscreen mode

Losing the game test case

We now need to decide if the game is lost based on whether we have run out of attempts.  

```
private fun Game.outcomeFor(exactHits: List<Feedback.Peg>) = when {
  exactHits.size == this.secretLength -> WON
  this.attempts + 1 == this.totalAttempts -> LOST
  else -> IN_PROGRESS
}
```

Enter fullscreen mode Exit fullscreen mode

Outcome calculation for a lost game

The following state-accessing functions turned out to be handy for this task.  

```
private val Game.attempts: Int
  get() = filterIsInstance<GuessMade>().size

private val Game.totalAttempts: Int
  get() = filterIsInstance<GameStarted>().firstOrNull()?.totalAttempts ?: 0
```

Enter fullscreen mode Exit fullscreen mode

Deriving state properties

One last rule is that we cannot continue playing a lost game.  

```
@Test
fun `the game can no longer be played once it's lost`() {
  val secret = Code("Red", "Green", "Blue", "Yellow")
  val wrongCode = Code("Purple", "Purple", "Purple", "Purple")
  val game = gameOf(GameStarted(gameId, secret, 1, availablePegs))

  val update = execute(MakeGuess(gameId, wrongCode), game)
  val updatedGame = game.updated(update)

  execute(MakeGuess(gameId, secret), updatedGame) shouldFailWith
    GameAlreadyLost(gameId)
}
```

Enter fullscreen mode Exit fullscreen mode

Playing a lost game test case

One more condition in the validation function will do the job.  

```
private fun startedNotFinishedGame(command: MakeGuess, game: Game): Either<GameError, Game> {
  if (!game.isStarted()) {
    return GameNotStarted(command.gameId).left()
  }
  if (game.isWon()) {
    return GameAlreadyWon(command.gameId).left()
  }
  if (game.isLost()) {
    return GameAlreadyLost(command.gameId).left()
  }
  return game.right()
}
```

Enter fullscreen mode Exit fullscreen mode

Validation for a lost game

The game is lost if there has been a _GameLost_ event published.  

```
private fun Game.isLost(): Boolean =
  filterIsInstance<GameLost>().isNotEmpty()
```

Enter fullscreen mode Exit fullscreen mode

Deriving state properties

## [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#summary)Summary

We explained the implementation details of an event-sourced functional domain model in Kotlin. In the process, we attempted to show how straightforward testing of such a model can be and how it doesn't require any dedicated testing techniques or tools. The model itself, on the other hand, remains a rich core of business logic.

Hopefully, dear reader, you found it useful.

If you prefer to see a full example in one place, here's the gist: [https://gist.github.com/jakzal/3f0ee968fe8073ee81e328ecbd59fe8b](https://gist.github.com/jakzal/3f0ee968fe8073ee81e328ecbd59fe8b)

Next time, we'll [continue the example](https://dev.to/jakub_zalas/deriving-state-from-events-1plj) by replacing extension functions that derive state with an actual state data class.

## [](https://dev.to/jakub_zalas/functional-event-sourcing-example-in-kotlin-3245#references)References

-   [Mastermind game rules](https://github.com/jakzal/mastermind)
-   [What is Event Modeling? (with example)](https://www.goeleven.com/blog/event-modeling/) by Yves Goeleven
-   [CQRS](https://martinfowler.com/bliki/CQRS.html) by Martin Fowler
-   [Learning Domain-Driven Design](https://www.goodreads.com/book/show/59383590-learning-domain-driven-design) by Vladik Khononov.
-   [Implementing Domain-Driven Design](https://www.goodreads.com/book/show/15756865-implementing-domain-driven-design) by Vaughn Vernon
-   [Functional Programming Ideas for the Curious Kotliner](https://leanpub.com/fp-ideas-kotlin) by Alejandro Serrano Mena
-   [Typed errors](https://arrow-kt.io/learn/typed-errors/) in Arrow documentation
-   [Mastermind game example gist](https://gist.github.com/jakzal/3f0ee968fe8073ee81e328ecbd59fe8b)
