---
created: 2024-07-29T17:20:37 (UTC -03:00)
tags: []
source: https://www.theverge.com/2024/7/26/24206529/intel-13th-14th-gen-crashing-instability-cpu-voltage-q-a
author: 
---

# There is no fix for Intel’s crashing 13th and 14th Gen CPUs — any damage is permanent - The Verge

> ## Excerpt
> We got some answers from Intel, and more are on the way.

---
On Monday, it initially seemed like the [beginning of the end for Intel’s desktop CPU instability woes](https://www.theverge.com/2024/7/22/24203959/intel-core-13th-14th-gen-cpu-crash-update-patch) — the company confirmed a patch is coming in mid-August that should address the “root cause” of exposure to elevated voltage. But if your 13th or 14th Gen Intel Core processor is already crashing, that patch apparently won’t fix it.

Citing unnamed sources, [_Tom’s Hardware_ reports](https://www.tomshardware.com/pc-components/cpus/intel-finally-announces-a-solution-for-cpu-crashing-errors-claims-elevated-voltages-are-the-root-cause-fix-coming-by-mid-august) that any degradation of the processor is irreversible, and an Intel spokesperson did not deny that when we asked. Intel is “confident” the patch will keep it from happening in the first place. (As another preventative measure, you should update your motherboard BIOS ASAP.) But if your defective CPU has been damaged, your best option is to replace it instead of tweaking BIOS settings to try and alleviate the problems.

And, Intel confirms, too-high voltages aren’t the only reason some of these chips are failing. Intel spokesperson Thomas Hannaford confirms it’s _a_ primary cause, but the company is still investigating. Intel community manager Lex Hoyos also revealed some instability reports can be [traced back to an oxidization manufacturing issue](https://www.reddit.com/r/intel/comments/1e9mf04/comment/lefz09c/) that was fixed at an unspecified date last year.

This raises lots of questions. Will Intel recall these chips? Extend their warranty? Replace them no questions asked? Pause sales [like AMD just did](https://www.theverge.com/2024/7/24/24205416/amd-zen-5-ryzen-9000-desktop-delay) with its Ryzen 9000? Identify faulty batches with the manufacturing defect?

We asked Intel these questions, and I’m not sure you’re going to like the answers.

Why are these still on sale without so much as an extended warranty?

Intel has not halted sales or clawed back any inventory. It will not do a recall, period. The company is not currently commenting on whether or how it might extend its warranty. It would not share estimates with _The Verge_ of how many chips are likely to be irreversibly impacted, and it did not explain why it’s continuing to sell these chips ahead of any fix.

Intel’s not yet telling us how warranty replacements will work beyond trying customer support again if you’ve previously been rejected. It did not explain how it will contact customers with these chips to warn them about the issue.

But Intel _does_ tell us it’s “confident” that you don’t need to worry about invisible degradation. If you’re not currently experiencing issues, the patch “will be an effective preventative solution for processors already in service.” (If you don’t know if you’re experiencing issues, Intel currently suggests [the Robeytech test](https://youtu.be/wkrOYfmXhIc?si=XzCym6UjLcF3_yW2&t=497).)

And, perhaps for the first time, Intel has confirmed just how broad this issue could possibly be. The elevated voltages could potentially affect any 13th or 14th Gen desktop processor that consumes 65W or more power, not just the highest i9-series chips that [initially seemed to be experiencing the issue](https://www.theverge.com/2024/4/9/24125036/intel-game-crash-13900k-14900k-fortnite-unreal-engine-investigation).

Here are the questions we asked Intel and the answers we’ve received by email from Intel’s Hannaford:

**How many chips does Intel estimate are likely to be irreversibly impacted by these issues?**

Intel Core 13th and 14th Generation desktop processors with 65W or higher base power – including K/KF/KS and 65W non-K variants – could be affected by the elevated voltages issue. However, this does not mean that all processors listed are (or will be) impacted by the elevated voltages issue.

Intel continues validation to ensure that scenarios of instability reported to Intel regarding its Core 13th and 14th Gen desktop processors are addressed.

For customers who are or have been experiencing instability symptoms on their 13th and/or 14th Gen desktop processors, Intel continues advising them to reach out to Intel Customer Support for further assistance. Additionally, if customers have experienced these instability symptoms on their 13th and/or 14th Gen desktop processors but had RMA \[return merchandise authorization\] requests rejected we ask that they reach out to Intel Customer Support for further assistance and remediation.

**Will Intel issue a recall?**

No.

**Will Intel proactively warn buyers of these chips about the warning signs or that this update is required? If so, how will it warn them?**

Intel targets to release a production microcode update to OEM/ODM customers by mid-August or sooner and will share additional details on the microcode patch at that time.

Intel is investigating options to easily identify affected processors on end user systems. In the interim, as a general best practice Intel recommends that users adhere to Intel Default Settings on their desktop processors, along with ensuring their BIOS is up to date.

**Has Intel halted sales and / or performed any channel inventory recalls while it validates the update?**

No.

**Does Intel anticipate the fix will be effective for chips that have already been in service but are not yet experiencing symptoms (i.e., invisible degradation)? Are those CPUs just living on borrowed time?**

Intel is confident that the microcode patch will be an effective preventative solution for processors already in service, though validation continues to ensure that scenarios of instability reported to Intel regarding its Core 13th/14th Gen desktop processors are addressed.

Intel is investigating options to easily identify affected or at-risk processors on end user systems.

It is _possible_ the patch will provide some instability improvements to currently impacted processors; however customers experiencing instability on their 13th or 14th Generation desktop processor-based systems should contact Intel customer support for further assistance.

**Will Intel extend its warranty on these 13th Gen and 14th Gen parts, and for how long?** 

\[No answer yet.\]

**Given how difficult this issue was for Intel to pin down, what proof will customers need to share to obtain an RMA? (How lenient will Intel be?)**  

\[No answer yet.\]

**What will Intel do for 13th Gen buyers after supply of 13th Gen parts runs out? Final shipments were set to end last month,** [**I’m reading**](https://www.tomshardware.com/pc-components/cpus/intel-axes-13th-gen-core-i5-i7-i9-k-series-cpus-lineup-will-be-discontinued-by-may-24th-2024)**.**

Intel is committed to making sure all customers who have or are currently experiencing instability symptoms on their 13th and/or 14th Gen desktop processors are supported in the exchange process. This includes working with Intel’s retail and channel customers to ensure end users are taken care of regarding instability symptoms with their Intel Core 13th and/or 14th Gen desktop processors.

**What will Intel do for 14th Gen buyers after supply of 14th Gen parts run out?** 

Same as above.

**Will replacement / RMA’d chips ship with the microcode update preapplied beginning in August? Is Intel still shipping replacement chips ahead of that update?**

Intel will be applying to microcode to 13th/14th Gen desktop processors that are not yet shipped once the production patch is released to OEM/ODM partners (targeting mid-August or sooner). For 13th /14th Gen desktop processors already in service, users will need to apply the patch via BIOS update once available.

**What, if anything, can customers do to slow or stop degradation ahead of the microcode update?**

Intel recommends that users adhere to Intel Default Settings on their desktop processors, along with ensuring their BIOS is up to date. Once the microcode patch is released to Intel partners, we advise users check for the relevant BIOS updates.

**Will Intel share specific manufacturing dates and serial number ranges for the oxidized processors so mission-critical businesses can selectively rip and replace?** 

Intel will continue working with its customers on Via Oxidation-related reports and ensure that they are fully supported in the exchange process.

**Why does Intel believe the instability issues** [**do not affect mobile laptop chips**](https://www.reddit.com/r/intel/comments/1e9mf04/comment/lekoj61/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)**?** 

Intel is continuing its investigation to ensure that reported instability scenarios on Intel Core 13th/14th Gen processors are properly addressed.

This includes ongoing analysis to confirm the primary factors preventing 13th / 14th Gen mobile processor exposure to the same instability issue as the 13th/14th Gen desktop processors.  

That’s all we’ve heard from Intel so far, though Hannaford assured us more answers are on the way and that the company is working on remedies.

Again, if your CPU is already damaged, you need to get Intel to replace it, and if Intel won’t do so, please let us know. In the meanwhile, you’ll want to update your BIOS as soon as possible because your processor could potentially be invisibly damaging itself — and if you know your way around a BIOS, you may want to adjust your motherboard to Intel’s default performance profiles, too.

Lastly, here is that Robeytech video that Intel is recommending to Redditors to potentially help them identify if their chip has an issue. Intel says it’s looking into other ways to identify that, too.
