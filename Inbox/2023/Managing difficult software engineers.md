---
created: 2023-08-30T19:41:43 (UTC -03:00)
tags: []
source: https://vadimkravcenko.com/shorts/managing-bad-engineers/
author: Vadim Kravcenko
---

# Managing difficult software engineers

> ## Excerpt
> Effectively managing difficult employees in a software engineering context hinges on three core principles: fostering trust by empowering autonomy, promoting growth through challenges and constructive feedback, and ensuring a comfortable work environment with streamlined processes and minimal disruptions.

---
_Hello dear reader, I will be improving this article based on the feedback that I received. I don't see myself as an expert, more like ever-learning amateur, so if you point out something, I will try to understand your point of view and correct any mistakes. Feel free to comment the article below, any and all feedback is welcome, just stay civil._

In the grand scheme of a software engineering path, there's a thread that weaves through every project, every failure, and every challenge. That thread is people. As a person with an engineering background, I do enjoy solving hard puzzles and fixing problems. But the most complex, intriguing, and ultimately rewarding aspect of my journey has always been managing people. I wasn’t a people person before — It’s not something I could do naturally — I had to read a lot, learn a lot, try different methodologies, and figure stuff out on the fly, but it’s still the most rewarding aspect of my job.

This guide is born out of those countless interactions, conversations, and experiences. Things that I’ve failed at. It's a distillation of lessons learned, a roadmap to help you, the engineering managers of today, navigate the choppy waters of managing difficult software engineers.

_I'm not an expert, and am not trying to position myself as one, so if you disagree with anything here, let me know in the comments and we'll have a healthy discussion._

```
🏄 Some people call developers snowflakes — they’re brilliant, opinionated, and unique in many ways. It’s easy to manage those that don’t deviate too much from all the other engineers. Still, I will focus more on those difficult employees rather than the general management of all developers.
```

Why focus on the difficult ones, you ask? Well, it's simple. When everything is smooth sailing, when your team is balanced like a well-oiled machine, you don't need a guide. It's when you have conflicts, ego clashes, lone wolves, or when you're faced with a team member who's not pulling their weight, causing friction, or just not fitting in that's when you need a compass to guide you. And that's what this guide aims to be.

In the following sections, we'll delve into the various types of difficult employees you might encounter - from procrastinators and lone wolves to negative Nancies and over-promisers. We'll explore typical scenarios, describe their behaviors, and, most importantly, provide strategies for managing difficult employees effectively.

I will also touch a bit on the importance of communication techniques like Nonviolent Communication. We'll walk through the steps of implementing a Performance Improvement Plan and when it might be time to escalate issues and part ways.

```
🏄 This guide is not a magic wand that will solve all your management challenges. But it's a starting point. It's also very subjective. Your mileage may vary. It's a collection of insights and advice that I hope will help you become a better manager to your team, even if some of them are not the most pleasant people to work with.
```

So, let's embark on this journey together. Let's navigate these rough seas with your crew and turn challenges into opportunities for growth. As any seasoned sailor will tell you, it's not the calm seas but the storms that make a good captain.

## [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#h-effectively-managing-the-norm)Effectively Managing the Norm

Before diving deep into managing the difficult engineers, let’s focus first on managing those that are not as atypical. The "norm" refers to the majority of your team members who come in daily, do their job, and contribute to the team’s success — they’re competent professionals with healthy mental states and won't require any special attention. No issues, no conflicts, no special needs. These individuals form the backbone of your team and will probably represent 90% of the people you meet and work with.

I use a simple strategy to make sure these kinds of employees are happy — Trust, Growth, Comfort.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#h-trust)Trust

**Trusting Them to Do the Right Things**

-   **Assume Intelligence**: Always start with the assumption that your team members are intelligent and will make informed decisions. This mindset fosters an environment of respect and empowers individuals to take initiative. It’s even better to assume they’re smarter than you and might know something you don’t. You probably hired them, so unless you’re hiring people who are worse than yourself (and I sure hope you don’t), you should assume they have better ideas than you do.
-   **Accountable Autonomy**: Give them the freedom to make decisions and act on them. Keeping them accountable for those decisions is far more productive than questioning their every move. Autonomy comes with the price of accountability and should be stated explicitly. Do whatever you want, but you’re responsible for the fuck-ups as well.
-   **Allow fixable mistakes to happen**: Trust means allowing room for errors. Of course, I’m not talking about senior developers with privileged access running rm -rf on production or dropping production database and deleting backups, and saying whoops — that’s a cause for corrective action. The mistakes can be divided into two parts: Mistakes that can be fixed and mistakes that cannot. Mistakes that can be fixed is anything where you write a postmortem, do a debriefing and continue on your way with lessons learned. Mistakes that cannot be fixed is people dying, something blowing up or your company going bankrupt. Allow them to make mistakes from which the company can recover and help them learn from them. _**Update:** I suggest you also read the comments, there's a big discussion there regarding this point._

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#h-growth)Growth

**Challenging Them to Become Better**

-   **Challenge Regularly**: Push your team members out of their comfort zones. Not talking about dropping them cold into projects where they have zero knowledge and expecting them to perform. More of — encouraging them to take on tasks that stretch their abilities, a bit more complicated than they’re used to, fostering their growth.
-   **Praise and Recognition**: Recognize their achievements. Show them the milestones they can reach and celebrate with them when they do — be it with monetary incentives or a public announcement of their achievements. This not only boosts morale but also motivates them to aim higher.
-   **Constructive Feedback and clear Path**: While praise is essential, it's equally important to address areas where they’re lacking. Nobody is perfect, and some of us are better at different things. However, the focus should be on improving where we excel and pulling up those things we’re bad at. They’re good at algorithms but bad at databases — time to deep dive into DBs. They’re good at small tasks but struggle with bigger things — time to learn software architecture. In any way, the feedback should be a tool for growth, not a weapon for criticism.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#h-comfort)Comfort

**Keeping It Comfortable**

-   **Limit Interruptions**: Respect their time and focus. Avoid unnecessary meetings and interruptions. Embrace asynchronous communication, allowing team members to respond when it's most convenient, fostering efficiency and respect for individual work rhythms. If the wheels are turning, there’s no need to run around interrupting them constantly. Let people work.
-   **Streamlined Processes**: Ensure that all auxiliary processes, from software tools to administrative tasks, are efficient, user-friendly, and non-interrupting (see above). A smooth operational environment reduces frustration and allows team members to focus on what needs to be done without constant notifications.
-   **Less thinking, more creating**: As software engineers are more creative, the fewer decisions they have to make outside of their project, the better decisions they make inside of the project. Instead of them figuring out, “Is tomorrow a non-working day or not” — make the info easily accessible. Instead of them figuring out where to get snacks — have snacks ready. You get the idea.

The list above can be the catalyst that propels your team to a new steady height of productivity. These are low-hanging fruits that can create a positive atmosphere that permeates every line of code. It can enhance the quality of the software your team produces, ensuring that every product you deliver is as good as it possibly can be.

But let's not sugarcoat it - being an effective leader in a software engineering team is not without its challenges. You're dealing with people, each with their unique personalities, strengths, weaknesses, and quirks.

```
🏄 One of the most common - and often most daunting - challenges is dealing with difficult employees. Whether it's the brilliant coder who can't meet a deadline, the seasoned engineer who struggles with teamwork, or the talented newcomer who's always negative — there will be many, many individuals that will not conform to the normal management described above, and you’ll need to adapt.
```

Let's explore the challenging types of software engineers, understand their behaviors, and learn practical strategies to manage them. Because at the end of the day, it's not just about building great software; it's about having fun while building great teams.

## [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#h-difficult-software-engineers)Difficult Software Engineers

_I use the term "difficult" to differentiate between those employees where everything is going smoothly, and those where more effort is required._

Why is it important to understand different types of difficult employees? Well, think of it this way - if you're trying to debug a piece of code, you first need to understand what each line of code is doing, right? Similarly, to address and manage problematic behaviors effectively, you first need to understand these behaviors, their motivations, and their impact on the team.

In this section, we'll explore a variety of difficult employee types that you might encounter in a software engineering team. Each of these types presents unique challenges and opportunities for growth and improvement. By understanding these behaviors, you'll be better equipped to manage them effectively, turning potential obstacles into stepping stones toward a more harmonious and productive team.

_I want to note that some scenarios are hyperbolized, and I’m not a communications guru, so if you have better examples— please put them in the comments; much appreciated._

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-procrastinator)The Procrastinator

Imagine this: It's a week before a major software release. Your team is buzzing with activity, tying up loose ends, and running final tests. But there's one team member, let's call him Joe, who's yet to complete a crucial piece of the project. Despite repeated reminders, Joe always seems to have an excuse — the build is taking too long, or it works on my machine but is failing on staging — always promises to get it fixed by “tomorrow". This is a classic example of a Procrastinator.

Procrastinators tend to delay tasks, often focusing on less important tasks while ignoring the critical ones. They may have poor time management skills, often underestimating the time required to complete tasks. This behavior can lead to missed deadlines, increased stress for the team, and potential risks to the project.

Managing a Procrastinator requires a balance of firmness and support. Set clear deadlines and regularly check in on their progress. Make them accountable for those deadlines. Help them prioritize tasks and offer time management training if necessary. Ensure they update the Project Management tools daily with the progress made and things finished.

**Remember, the goal is not micromanaging but guiding them toward better work habits.**

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-lone-wolf)The Lone Wolf

Picture this: Your team is brainstorming ideas for a new feature. Ideas are flying around, and the energy is high. But there's one person, let's call her Lisa, who's sitting quietly, not participating. Later, you find out that Lisa has gone ahead and started working on the feature without discussing her approach with the team. She assumed she knows best how to tackle the problem. This is a typical Lone Wolf scenario.

Lone Wolves assume they’re the most competent person and prefer to work alone, often resisting collaborative efforts — I mean, why would you need someone else’s help if they’re far inferior to you? Usually, they do not participate in team discussions unless to tell everyone that they “could’ve done that in 3 days”. While their self-reliance can sometimes be an asset, it can also lead to a lack of cohesion and potential misalignment with team goals. And most importantly — people don’t enjoy working with Lone Wolves.

```
🏄 I see Lone Wolves as a teenager mentality. They’re stuck in this mindset that they’re the most brilliant badass coders ever.
```

There are two ways to manage the Lone Wolf — to embrace or make them collaborate. Embracing the Lone Wolves means that this person is of very high value to your business, and keeping him comfortable is more important than the overall team morale. Not recommended, but still a possible tradeoff if the Lone Wolf is a 100x Engineer in your eyes.

Managing a Lone Wolf involves forcing a culture of collaboration. Encourage them to share their ideas in the meetings and involve them in team decisions more directly. It would be great if you could put them in a team where they would not be the smartest. Assign them a mentor to help them see themselves in a different light. Give them tasks that require collaboration and, most important of all — provide constructive criticism of their behavior and set ground rules for collaboration — they can either stay on the same level as they are now or grow out of this lone wolf mentality and be an even more awesome coder who people enjoy working with.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-negative-nancy)The Negative Nancy

Let's set the scene: Your team is in a meeting discussing a new project. There's a sense of excitement in the air, a buzz of anticipation. But then there's one person, let's call him Mark, who seems to find a problem with every idea, a flaw in every plan. He constantly focuses on the negatives, rarely offers solutions, and thinks everything is impossible and the status quo is just fine. This is your typical Negative Nancy.

First of all — seeing issues in ideas is not a problem per se. It’s mostly the negative way the issue is portrayed. Suppose there’s a challenge that needs to be overcome to achieve the idea. In that case, Negative Nancies usually tend to say it’s impossible because of X, not that “we can make it happen if we do X and Y, which will require significant effort, but it’s still doable.”

Negative Nancies tend to overly focus on the negative aspects of situations, overlooking potential solutions. They may complain frequently, resist change, and spread negativity within the team. This behavior can dampen team morale and hinder creativity and innovation. “Why are they forcing us to do X? Urgh, I like how things are now.”

Managing a Negative Nancy involves understanding where they’re coming from — from a position of “nothing should affect my comfort, the more issues I find, the less I need to do and the less I need to move out of my comfort zone”. This negativity should be addressed directly and privately, asking them to bring solutions instead of problems. Encourage positive interactions and remind them that change will happen regardless of them and that things will move on without them if they continue to focus on the negatives. If the behavior continues, involving HR or considering whether this employee is a good fit for the team may be necessary.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-over-promiser)The Over-Promiser

Imagine this: You're planning a new software release. One of your team members, let's call her Sarah, promises to deliver a complex functionality within a month. You're impressed by her confidence, but two weeks after she told you that “everything is fine, I’m handling it,” the functionality is nowhere near complete; even worse, it’s about 20% done. This is a classic Over-Promiser scenario.

Over-Promisers tend to be overly optimistic about their capabilities or the time required to complete tasks and secretive when things don’t go as planned. Even during status updates, they can say, “It’s going great,” even though they’re struggling to find a solution and face some blockers — because they still hope to finish it on time. They often fail to deliver on their promises, which leads to frustration and mistrust within the team and with the Product Owners.

```
🏄 Over-Promisers are usually the younger developers who have not missed deadlines that often yet, and they’re still getting the hang of how much they can do and how fast.
```

Managing an Over-Promiser involves setting realistic expectations together with the teammates — figuring out a proper deadline for this big thing that needs to be built and splitting it up into meaningful tasks (also together with the teammates) — after that, everyone agrees on how much time each of those parts will take. I don’t think there’s a way to help them make more accurate commitments besides getting more experience. If it’s a senior dev — make them accountable for the deadlines that they set. Encourage them to communicate early and openly about any challenges they're facing.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-know-it-all)The Know-It-All

Possible Scenario: You're in a team meeting, discussing a complex problem. Different team members suggest solutions, debating pros and cons, and a healthy collaboration is happening. But then, all of a sudden, someone, let’s call him Alex, speaks up and says that he’s heard enough, everyone is stupid, all the solutions are bullshit, and his solution is the only correct one. No way you should question or poke holes in his suggestion, as nobody is more brilliant than him. He vehemently dismisses others' ideas and resists feedback. This is your typical Know-It-All.

```
🏄 It’s very similar to lone wolf, but they don’t insist on working alone — they insist on their solution being built and praised.
```

Know-It-Alls tend to be arrogant and closed-minded. They often lack the ability to listen to others and are highly resistant to change or new ideas. As you can imagine, this behavior is quite annoying for others who want to spar with ideas and figure out the best way to build something. Overall this stifles creativity, hinders team collaboration, and creates a hostile work environment.

Nobody enjoys working with the know-it-alls. They tend to shut down any, even small improvement suggestions. Imagine someone with a superiority complex — any hint that their idea is imperfect leads to a defensive discussion.

How to handle the know-it-all — the first step is addressing the behavior directly in a 1:1 meeting — they are likely unaware of their shortcomings, as they think everything they do is perfect. Send them this article, and tell them if they see themselves in the description above. After the confrontation — emphasize mutual respect and open-mindedness — all ideas need to be discussed/sparred/disassembled, even if they seem perfect; only then can anyone be sure the solution is correct. Encourage them to ask for feedback all the time.

**If the behavior continues, give a formal warning and consider involving HR in the meeting.**

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-silent-types)The Silent Type

Scenario: Your team is in a lively discussion, brainstorming ideas for a new awesome feature. Everyone is participating except for one person. Let’s call her Emily. She sits quietly, not contributing to the discussion. Later, you find out that she had a great idea but didn't feel comfortable sharing it. This is a typical Silent Type scenario.

> I’m writing about this type irrespective of the gender of the person. Still, I’d like to point out that female developers often have a more challenging time voicing their opinion when the team culture is similar to the frat house and values the loudest voices the most.

Silent Types, in general, tend to be introverted and may struggle with communication. Even though they might immediately see the issues with the solution, they might think, “It’s not worth it to share the concerns, to antagonize someone.” They often keep their thoughts to themselves, which can lead to misunderstandings or missed opportunities for collaboration.

```
🏄 The silent type is usually a by-product of a toxic culture where the conflicts don’t get resolved in a healthy way; hence everyone avoids them instead of dealing with them head-on. This also means other “bad” archetypes are present on the team, e.g., the know-it-all.
```

Managing a Silent Type involves creating a safe and inclusive environment and encouraging others to speak up even when there will be conflicts of ideas. Healthy debates must happen regularly so everyone sees that voicing their opinion is not something to be afraid of. Provide various channels/platforms for communication — one-on-one meetings, meetings in a smaller group, group chats, email discussion — which may be more comfortable for them.

**But most important, recognize and value their contributions to make them feel more included even if they’re not the loudest voice in the room.**

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-perfectionist)The Perfectionist

So, let’s set the scene: You see a Pull Request pop up from one of your team members. Let’s call him Tom. You start reviewing a piece of code — The code is well-written and does the job, but Tom is not satisfied. He's been tweaking and refining it for days, trying to make it "perfect,” eventually turning it into a one-liner. The problem? Other tasks are piling up, deadlines are looming, code looks cool only to Tom, and Tom's quest for perfection is causing delays. This is your typical Perfectionist.

Very often, Perfectionists are recent graduates. They tend to focus on minute details, often at the expense of the bigger picture. They may struggle with the KISS principles and delegation and have high, sometimes unrealistic, standards for themselves and others. In their mind, the code is art, and they should not be rushed while creating an art piece. This behavior leads to inefficiencies, missed deadlines, and increased stress for the whole team.

So how can you manage a perfectionist? **Easy. Lower their expectations of themselves, help them focus on what's most important — a working solution that is not-overengineered, does not read like a poem, and is maintainable in 2 years.** Encourage them to prioritize simple solutions and set realistic standards. Ask them for a second opinion on the “readiness” of the code.

If necessary, provide coaching on time management and book suggestions to build effective coding habits.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-unreliable-one)The Unreliable One

Scenario: You're managing a project with a tight deadline. One of your team members, let's call her Anna, is responsible for a critical part of the project. Despite her assurances that she'll complete the task on time, the deadline comes and goes, and the task is still incomplete. This is a classic Unreliable One scenario.

Unreliable employees are identified by their tardiness — constantly late, submitting unfinished work, and skipping meetings. They lack commitment, discipline, or time management skills. Or all three combined. Their inconsistent performance can lead to mistrust and frustration within the team and potentially (highly likely) jeopardize project timelines.

Managing an Unreliable One involves setting clear expectations and holding them accountable — meaning giving formal warning after the first incident. During the first incident, provide feedback on the impact of their behavior on the team and the project. If the habit continues, set up a performance improvement plan and agree transparently on future disciplinary action. Remember, reliability is a key component of a high-performing team, and addressing issues promptly and effectively is important.

**If you don’t handle the unreliable employees, you’re doing a disservice to your team, who are trying to be effective and rely on them.** They will need to spend time thinking about ways to work around them rather than work with them.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-conflict-instigator)The Conflict Instigator

Picture this: Your team is working harmoniously on a project, but there's one person, let's call him Jake, who seems to thrive on conflict. He makes fun of others’ code, pokes at people, and often instigates arguments, spreads rumors, or creates a hostile environment. This is your typical Conflict Instigator.

Conflict Instigators tend to be argumentative, aggressive, and disruptive. They thrive on drama and create a toxic work environment. This behavior can lead to decreased team morale, increased stress, and reduced productivity.

In my opinion (Of course, you can correct me in the comments) — There’s only one way to handle such employees — with immediate addressing of the actions in private, emphasizing the importance of staying professional and a zero-tolerance policy for such behavior in the future. If it continues — cut fast and don’t look back. It’s not worth the effort to keep them on the team. Remember, a healthy team is a productive team, and toxic conflict should be addressed promptly and effectively.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#the-burned-out-employee)The Burned-Out Employee

Scenario: One of your top performers, let's call her Lisa, has been showing signs of burnout. She's been great the last year, hitting milestone after milestone, but recently she’s been working long hours, missing deadlines, and seems disengaged. She's not the enthusiastic, motivated team member she used to be. This is a typical Burned-Out Employee scenario.

```
🏄 I don’t consider a burned-out employee as a “bad employee,” but they can still have a negative impact — they are the ones you need to help and make sure they can thrive again.
```

Burned-out employees exhibit signs of exhaustion, cynicism, and reduced professional efficacy. They struggle to meet deadlines, have the lowest output on the team, and make more mistakes than over the previous observable period.

Managing a Burned-Out Employee involves addressing the issue with as much empathy and concern as you have. Suggest they take a vacation or sabbatical. Go all-in on work-life balance and provide resources for stress management.

Remember, your team members are your most valuable asset, and their well-being should always be a priority.

## [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#communication-tips)Communication Tips

Managing difficult employees means figuring out ways to ensure you and your employees thrive, even if the circumstances are against you. One tool stands out as particularly crucial: communication. In a software engineering team, clear and effective communication is not just a nice-to-have; it's a must-have. Bad communication is the reason teams fail.

Think about it. How often have you seen a project go off track due to a simple misunderstanding? How many conflicts could have been avoided with better communication? How many great ideas have been lost in the noise of poor communication?

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#nonviolent-communication)Nonviolent Communication

One powerful communication tool I've found particularly useful is Nonviolent Communication (NVC). Developed by psychologist Marshall Rosenberg, NVC is a method of communication that promotes empathy and understanding.

At its core, NVC involves four steps: Observation, Feeling, Need, and Request.

-   **Observation** involves stating the facts we observe that are affecting our well-being.
-   **Feeling** is about sharing how we feel in relation to what we observe.
-   **Need** is expressing what we need or value that is causing our feelings.
-   **Request** involves clearly requesting what we want without demanding it.

In a management context, NVC can be a game-changer. It can help you communicate effectively with your team, constructively address issues, and build strong, positive relationships.

I’m not going to talk much about it, but I’m going to give you some links to read:

-   [https://www.dave-bailey.com/blog/nonviolent-communication](https://www.dave-bailey.com/blog/nonviolent-communication)
-   [https://www.amazon.com/Nonviolent-Communication-Language-Life-Changing-Relationships/dp/189200528X](https://www.amazon.com/Nonviolent-Communication-Language-Life-Changing-Relationships/dp/189200528X)

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#active-listening-and-empathy)Active Listening and Empathy

As a manager, your role is not just to talk; it's also to listen. Active listening is a crucial skill that involves fully focusing on the speaker, understanding their message, and responding thoughtfully. It's not just about hearing the words; it's about understanding the emotions, ideas, and thoughts behind those words.

```
🏄 My business partner, who’s a great salesperson, has this rule — after someone finishes talking to you — wait a few seconds before responding, organize your thoughts, and process what they said, only after you’re sure they’ve said everything they wanted to do you respond to them.
```

Active listening can be improved through paraphrasing, asking open-ended questions, and showing empathy. Empathy, the ability to understand and share the feelings of others, plays a crucial role in management. Cultivating compassion involves being open to others' perspectives, showing genuine interest in their experiences, and responding with kindness and understanding. **Simply put, before you judge anyone as a "difficult" employee, you put yourself in their shoes and see from their side.** Talk with them, listen, we're all humans.

Your role is not just to lead; it's also to support, understand, and inspire.

## [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#what-to-do-next)What to do next?

Once you've identified and understood the types of difficult employees in your team and have communicated effectively with them, the question arises - what's the next step? What are the options? This section will guide you through the subsequent stages, from implementing Performance Improvement Plans to escalating issues when necessary.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#performance-improvement-plans)Performance Improvement Plans

**If, after many 1:1s with the person, you could not find a solution that helps both — the employee and the company — and they still continue behavior, it's time to make it formal.**

A Performance Improvement Plan (PIP) is a formal document that outlines an employee's performance issues and the steps they need to take to improve. It's an escalation tool that can help managers address performance issues in a structured, supportive manner.

A PIP may be necessary when an employee's performance is consistently not meeting expectations, despite feedback and coaching. It's a step that should be taken when other, less formal interventions have not resulted in any improvement.

Implementing a PIP involves several steps — first, clearly outline the performance issues and the expected improvements. Then, set realistic, measurable goals and a timeline for achieving them. Provide support and resources to help the employee improve, and schedule regular check-ins to discuss progress.

Monitoring progress is crucial. Regular feedback can help the employee understand where they're improving and where they still need to work. Remember, the goal of a PIP is not to punish but to support the employee in their transformation and to improve their performance.

To illustrate, let's consider a case study. An engineer in our team was consistently missing deadlines and marking tickets as done that would later be returned from the QA as unfinished. We implemented a PIP, setting clear expectations and defining strict rules how the behavior needed to change. Regular check-ins helped keep the engineer on track, and within a few months, their performance had significantly improved. The key lessons? Making it formal — helps a lot. It’s like getting a police ticket; people tend to react more seriously when you make it clear that it’s already escalating.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#time-to-escalate)Time to Escalate

Sometimes, despite your best efforts, an issue may need to be escalated. This could be due to persistent performance issues, serious misconduct, or if the employee's behavior negatively affects the team, e.g., with the conflict instigator.

Documenting the issue is crucial. Keep a record of the performance issues, the steps taken to address them, and the employee's response. This documentation can provide a clear picture of the problem and the efforts made to resolve it.

When escalating an issue, follow your organization's procedures. This may involve discussing the matter with HR or higher management. Remember, escalation is not a failure on your part as a manager. It's a step towards resolving an issue that's beyond your control. It's about ensuring a positive, productive work environment for your entire team.

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#time-to-part-ways)Time to Part Ways

There will come a time when, despite all efforts to improve a situation, it becomes clear that an employee is not a good fit for the team or the organization. This is a difficult realization and one that should not be taken lightly.

Termination should be considered when an employee's behavior or performance is consistently and significantly below expectations, despite repeated feedback, coaching, and support. Other reasons might include severe misconduct or an evident lack of alignment with the company's values or culture.

Termination is a last resort and should be considered only when all other avenues have been exhausted. It's a decision that can significantly impact the individual and the team, so it's crucial to handle it with care.

The process should be clear, fair, and transparent. The employee should be informed of the decision in a private, face-to-face meeting, and the reasons for the termination should be explained clearly. Treat the employee, even if you think they’re a “bad employee,” with respect and dignity throughout the process. Offer support where possible, and provide feedback to help them in their future careers.

Managing difficult employees is not just about addressing problematic habits; it's about understanding these behaviors, communicating effectively, and guiding your team members toward improvement.

Everyone has to have a second chance at becoming better.

___

Other [Newsletter](https://vadimkravcenko.com/newsletter/) Issues:

-   [Rules of Thumb for Software Development Estimations](https://vadimkravcenko.com/shorts/project-estimates/)
-   [The Surprising Power of Documentation](https://vadimkravcenko.com/shorts/proper-documentation/)
-   [Being a good mentor – a developers guide](https://vadimkravcenko.com/shorts/good-mentor/)
-   [Contracts you should never sign](https://vadimkravcenko.com/shorts/contracts-you-should-never-sign/)

_P.S I'm pretty sure there are more types of employees that can be mentioned here as well different ways YOU would've handled the scenarios, let me know in the comments below._

### [](https://vadimkravcenko.com/shorts/managing-bad-engineers/#reactions-new)Reactions New!

If you’re finding my articles valuable, consider sharing it with friends, or [buying me a coffee](https://vadimkravcenko.com/support).  
Also, [ask me anything](https://vadimkravcenko.com/support-ask) in a personalized question, if you want my expertise.
