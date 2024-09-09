---
created: 2024-09-09T09:53:29 (UTC -03:00)
tags: []
source: https://graphite.dev/blog/how-to-write-better-unit-tests?ref=dailydev
author: Written by
---

# Going from 0 to 1: How to write better unit tests when there are none

> ## Excerpt
> When I joined Graphite, there were almost no tests in the entire codebase. Out of the team of five engineers, three had previously worked at Meta — and had internalized the poor testing culture practiced there.

---
![Background gradient](https://graphite.dev/_next/image?url=%2F_next%2Fstatic%2Fmedia%2F3.a626335d.png&w=3840&q=80)

#Engineering#Learning

June 13, 2024

![author](https://graphite.dev/_next/image?url=https%3A%2F%2Fwww.datocms-assets.com%2F85246%2F1676042393-img_1141-1.png&w=3840&q=75)

David Bradford

![bugs effect on legacy code](https://www.datocms-assets.com/85246/1718318026-frame-4.png)

## Testing at Graphite[](https://graphite.dev/blog/how-to-write-better-unit-tests#testing-at-graphite)

When I joined Graphite, there were almost no tests in the entire codebase. Out of the team of five engineers, three had previously worked at Meta — and had internalized the poor testing culture practiced there.

Slowly, over the course of two years, we’ve transformed the company culture towards testing. Now, happily, most PRs (but not all) are accompanied by tests.

As we write more and more tests however, a new problem has cropped up: poorly architected tests.

## The trap of poorly architected tests[](https://graphite.dev/blog/how-to-write-better-unit-tests#the-trap-of-poorly-architected-tests)

A common set of beliefs I find among engineers are:

1.  Some tests are better than no tests.
    
2.  Because users don’t interact with tests in production like they do with production code, the quality of test code matters less; as long as the test tests the code, it’s good enough.
    

The end result is that when tests are introduced (like they were at Graphite), it’s all too easy to end up with tests written with spaghetti code.

But so what? If the test code isn’t technically used in production why does the code-quality matter?

Recently I was re-reading _Clean Code_ and found a nice story by [Uncle Bob](http://www.cleancoder.com/products) that explains the trap here:

> Some years back I was asked to coach a team who had explicitly decided that their test code should not be maintained to the same standards of quality as their production code They gave each other license to break the rules in their unit tests. “Quick and dirty” was the watchword. So long as the test code worked, and so long as it covered the production code, it was good enough.

> What this team did not realize was that having dirty tests is equivalent to, if not worse than, having no tests. The problem is that tests must change as the production code evolves. The dirtier the tests, the harder they are to change. The more tangled the test code, the more likely it is that you will spend more time cramming new tests into the suite than it takes to write the new production code. As you modify the production code, old tests start to fail, and the mess in the test code makes it hard to get those tests to pass again. So the tests become viewed as an ever-increasing liability.

> From release to release the cost of maintaining my team’s test suite rose. When managers asked what their estimates were getting so large, the developers blamed the tests. In the end they were forced to discard the test suite entirely.

> But, without a test suite they lost the ability to make sure that changes to their code base worked as expected. So their defect rate began to rise. As the number of unintended defects rose, they started to fear making changes. Their production code began to rot. In the end they were left with no tests, tangled and bug-riddled production code, frustrated customers, and the feeling that their testing effort had failed them.

Revisiting the earlier beliefs, I still agree that some tests are better than no tests. What most engineers miss, however, is what the Uncle Bob story highlights: the key to those tests is the architecture and subsequent maintainability of the tests themselves.

## Towards better tests[](https://graphite.dev/blog/how-to-write-better-unit-tests#towards-better-tests)

As we’ve been adding more tests to our codebase at Graphite, I’ve found myself repeating many of the same recommendations over and over again in code review, and have started refactoring a number of our older tests based on that same feedback.

Here are some of the tips I’ve shared internally to help us improve our test architecture.

## Tip #1: Cleaning up underlying code can help create cleaner tests[](https://graphite.dev/blog/how-to-write-better-unit-tests#tip-1-cleaning-up-underlying-code-can-help-create-cleaner-tests)

Like many early-stage startups racing to ship a product, there was a time at Graphite where code was written without tests in mind. The result: lots of large functions that intertwined core business logic and side effects.

For example, consider the following simplified method which resembles what our code frequently used to look like. The method determines whether a PR should be merged and, if it should, calls the GitHub API to merge, and then sends a notification via Slack:

```javascript
async function mergePr(pr: Pr, context: Context) {

  if (!ciIsPassing(pr)) {

    await context.githubApi.comment.mergeFailed(pr, 'MISSING_REQUIRED_CI');

    throw new MergeFailedError();

  }

if (!prHasRequiredApprovers(pr)) {

  await context.githubApi.comment.mergeFailed(pr, 'MISSING_REQUIRED_APPROVERS');

  throw new MergeFailedError();

  }

  await context.githubApi.mergePr(pr);

  await context.sendMergedSlackNotification(pr);

}
```

A corresponding test case for the above method might look something like this:

```javascript
it("A PR with passing required CI and required approvers merges", () => {

  const pr = {

    org: "withgraphite",

    repo: "backend",

    number: 1,

    requiredCi: [...],

    requiredApprovers: [...],

  };

  const mergePr = sinon.fake(),

  const context = {

  githubApi: {

  comment: sinon.fake(),

  mergePr,

},

sendMergedSlackNotification,

  };

  await mergePr(context, pr);

  expect(mergePr.calledOnce).to.be.true;  

});
```

The test itself in this simplified case isn’t too bad, but there are a few smells:

-   There’s a lot of setup, particularly with the stubbing. This boilerplate detracts away from the focus of the test case and makes it harder for a reader to skim the test case and understand what it does with just a brief glance.
    
-   The stubbing also adds to the maintenance burden. Consider the case where we later extend the `mergePr` method to also send a Slack notification when a PR cannot be merged. Our test case above will need to add a new stub even though it doesn’t test this codepath.
    
-   Consider the case where we want to add an additional test case that tests what happens when a PR can be merged (i.e. `mergePr` and `sendMergedSlackNotification`). In this test case we’ll need to supply a test PR that passes all of our merge requirements; if we later extend the merge requirements, this PR and test case may need to be updated as well, even though the thing we really want to test are the merge side effects.
    

We can do better. As I alluded to earlier, the problem here is an architectural one; I often find that when tests are hard to write, it’s a smell that the design of the code itself is bad.

In the below version of the code, I’ve now refactored it to separate out our pure logic from our side effects using the [Command/Query separation pattern](https://www.martinfowler.com/bliki/CommandQuerySeparation.html) which also has the nice effect of making each method smaller and more focused.

```javascript
function prMergeBlockers(pr: Pr) {

  const mergeBlockers = [];

  if (!ciIsPassing(pr)) {

    mergeBlockers.push('MISSING_REQUIRED_CI');

  }

  if (!prHasRequiredApprovers(pr)) {

    mergeBlockers.push('MISSING_REQUIRED_APPROVERS');

  }

  return mergeBlockers;

}

function mergePrIfPossible(pr: Pr, context: Context) {

  const mergeBlockers = prMergeBlockers(pr);

  if (mergeBlockers.length > 0) {

    await githubApi.comment.mergeFailed(pr, mergeBlockers);

  }

  mergePr(context, pr);

}

function mergePr(pr: Pr, context: Context) {

  await context.githubApi.mergePr(pr);

  await context.sendMergedSlackNotification(pr);

}
```

Our earlier test case now looks like this:

```javascript
it("A PR with passing required CI and required approvers merges", () => {

  const pr = {

    org: "withgraphite",

    repo: "backend",

    number: 1,

    requiredCi: [...],

    requiredApprovers: [...],

  };

  const mergeBlockers = prMergeBlockers(pr);

  expect(mergeBlockers.length).to.be.equal(0);

});
```

The problems we identified with testing the previous version of this code have now disappeared:

-   Our `prMergeBlockers` function allows us to test the merge blocker logic without needing to stub anything.
    
-   If we wanted to test the set of merge side effects, we could do so by directly invoking `mergePr`.
    

As an additional bonus, the new code is also simpler understand.

In Graphite’s case, as we’ve refactored more production code towards this pattern we’ve also noticed some corresponding speedups in our test suite execution. Database-related side effects in our tests are tested against a real Postgres instance running in our test container. By refactoring our logic and tests to be pure when we can, we’ve been able to cut down on these roundtrip database calls.

## Tip #2: Consider using a domain-specific language[](https://graphite.dev/blog/how-to-write-better-unit-tests#tip-2-consider-using-a-domain-specific-language)

In our earlier, improved test case, something still bothers me: of the nine lines in the test case, seven are dedicated to creating our fake PR object.

```javascript
const pr = {

  org: "withgraphite",

  repo: "backend",

  number: 1,

  requiredCi: [...],

  requiredApprovers: [...],

};

const mergeBlockers = prMergeBlockers(pr);

expect(mergeBlockers.length).to.be.equal(0);
```

This detracts from the focus of the test; all the reader needs to care about is that this PR passes required CI and has the required set of approvers. The reader doesn’t actually care about the specific organization or repository the PR is in and at worst, these are red herrings — they don’t actually affect the outcome of the test.

One pattern I’ve been enjoying lately is to use lightweight domain-specific language to help construct these test objects.

In our particular case this resembles a basic builder that translates to our earlier object:

```javascript
const pr = new Pr()

  .withRequiredCi()

  .withRequiredApprovers();

const mergeBlockers = prMergeBlockers(pr);

expect(mergeBlockers.length).to.be.equal(0);
```

Our example here is relatively basic, but in more complex cases in our codebase where there are complex relationships between entities, this has helped us remove a significant amount of boilerplate code.

A common concept we test at Graphite involves creating a graph of PRs with dependencies between them. In our tests, we’ve simplified this to a list of nodes and edges, relying on helper functions to do the real work of constructing the actual graph.

```javascript
nodes: [{ prNumber: 123 }, { prNumber: 5000 }, { prNumber: 314 }],

edges: [

  [{ prNumber: 123 }, { prNumber: 5000, parentPrNumber: 123 }],

],
```

These small helpers also come in handy at the assertion layer as well. In a test we can just write:

```javascript
assertEntriesAre({

  actual: entries,

  expected: [

    { prNumber: 13, status: "PENDING" },

    { prNumber: 42, status: "PENDING" },

    { prNumber: 314, status: "PENDING" },

    { prNumber: 500, status: "PENDING" },

  ],

});
```

This allows the reader to focus on the high-level, hiding the less interesting implementation within.

```javascript
const assertEntriesAre = ({

  actual,

  expected,

}: {

  actual: TMergeQueue;

  expected: { prNumber: number; status: TMqEntryStatus }[];

}) => {

  expect(

    actual.map((entry) => ({

      prNumber: entry.githubPr.number,

      status: entry.status,

    }))

  ).to.deep.eq(expected);

};
```

## Tip #3: Try out parameterizing your tests[](https://graphite.dev/blog/how-to-write-better-unit-tests#tip-3-try-out-parameterizing-your-tests)

Our single test case is now concise and easy to read. It highlights what is unique about this particular test case, allowing the reader to focus solely on what they need to know.

```javascript
it("A PR with passing required CI and required approvers merges", () => {

  const pr = new Pr()

    .withRequiredCi()

    .withRequiredApprovers();

  const mergeBlockers = prMergeBlockers(pr);

  expect(mergeBlockers.length).to.be.equal(0);

});
```

Let’s consider adding another test case: this time we want to ensure that if a PR is failing CI, that it does not merge.

```javascript
it("PR with passing required CI and required approvers merges", () => {

  const pr = new Pr()

    .withRequiredCi()

    .withRequiredApprovers();

  const mergeBlockers = prMergeBlockers(pr);

  expect(mergeBlockers.length).to.be.equal(0);

});

it("PR with required approvers but *without* required CI does not merge", () => {

  const pr = new Pr()

    .withRequiredApprovers()

    .withoutRequiredCi();

  const mergeBlockers = prMergeBlockers(pr);

  expect(mergeBlockers).to.contain('MISSING_REQUIRED_CI');

});
```

This does the job, but there’s some code duplication and its a little harder to tell at a quick glance what is unique about each test case.

Let’s refactor our test to explicitly highlight this parametrization:

```javascript
[

  {

    desc: "A PR with passing required CI and required approvers merges",

    pr: new Pr()

      .withRequiredCi()

      .withRequiredApprovers(),

    mergeBlockers: [],

  },

  {

    desc: "PR with required approvers but *without* required CI does not merge",

    pr: new Pr()

      .withRequiredApprovers()

      .withoutRequiredCi(),

    mergeBlockers: ['MISSING_REQUIRED_CI'],

  },

].forEach((tc) => {

  it(

    tc.desc,

    () => {

      const mergeBlockers = prMergeBlockers(pr);

      expect(mergeBlockers).to.deep.eq(tc.mergeBlockers);

    }

  )

});
```

Now it’s easier to see what’s unique about each case (the parameters and expected output) as well as what’s shared (the invocation of `prMergeBlockers`).

Extending this test to add a new test case or modify the output of a given input is also now a small change, easy to create and review.

```javascript
    ...

+   {

+     desc: "PR with required CI but *without required approvers does not merge",

+     pr: new Pr()

+       .withRequiredCi()

+       .withoutRequiredApprovers(),

+     mergeBlockers: ['MISSING_REQUIRED_APPROVERS'],

+   },

    ...
```

## Tip #4: Design your tests with failure in mind[](https://graphite.dev/blog/how-to-write-better-unit-tests#tip-4-design-your-tests-with-failure-in-mind)

When tests pass, engineers mostly ignore them; it’s only once tests start failing that someone starts digging into what happened.

As a result, a common point of first contact is the test failure message. When writing tests, we should consider what failure message might display to a future engineer and contemplate how we can make that as clear and actionable as possible.

Consider the following set of checks which assert that an array contains two entries, PR #42 and PR #500:

```javascript
expect(entries.length).to.be.eq(2);

const entriesSet = new Set(entries.map((e) => e.githubPr.number));

expect(entriesSet).to.not.contain(childPrNumber);

expect(entriesSet).to.not.contain(childPrNumber2);

expect(entriesSet).to.contain(42);

expect(entriesSet).to.contain(500);
```

If the `entries` array returns three items instead of the expected two, as intended, the test will fail on the first assertion, notifying us that `entries` array length is three and not two.

An engineer debugging the failure would likely start by modifying the test to then output the list of returned entries (trying to understand which entries were in the array), re-run the test, and then start inspecting the results.

If we can anticipate that this is a common debugging pattern, we can and should do better in our test case here. Swapping the above set of checks for a more concise single check can help future engineers skip that extra debugging step:

```javascript
expect(entries.map(e => e.githubPr.number)).to.deep.eq([42, 500]);
```

## Conclusion[](https://graphite.dev/blog/how-to-write-better-unit-tests#conclusion)

During my time at Graphite, we’ve been able to steadily improve the testing culture through education efforts like these. Though this change has been years in the making, I’ve found that steady effort and patience has been rewarded over time. My hope with these tips is that it can help you, your team also improve your testing approach.

This post captures some of the common problems I’ve noticed at Graphite, but is certainly not exhaustive. If you’re curious, I recommend reading “Chapter 9: Unit Tests” of _Clean Code_ as a good starting point; while I don’t agree with all of [_Clean Code_,](https://ptgmedia.pearsoncmg.com/images/9780132350884/samplepages/9780132350884.pdf) this chapter is one that stands the test of time.

Happy testing!

___
