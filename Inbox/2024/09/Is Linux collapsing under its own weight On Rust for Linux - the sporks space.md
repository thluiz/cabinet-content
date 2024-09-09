---
created: 2024-09-06T11:30:43 (UTC -03:00)
tags: []
source: https://sporks.space/2024/09/05/is-linux-collapsing-under-its-own-weight-on-rust-for-linux/?utm_source=tldrnewsletter
author: cb
---

# Is Linux collapsing under its own weight? On Rust for Linux - the sporks space

> ## Excerpt
> Recently, one of the developers of the Rust for Linux project, Wedson Almeida Filho, resigned from the project. In his parting message, he linked a video of a filesystem maintainer shouting at him. Afterward, Asahi Lina, developer of the Apple… Continue reading →

---
Recently, one of the developers of the Rust for Linux project, Wedson Almeida Filho, resigned from the project. In his [parting message](https://lore.kernel.org/lkml/20240828211117.9422-1-wedsonaf@gmail.com/), he linked a video of a filesystem maintainer shouting at him. Afterward, Asahi Lina, developer of the Apple GPU drivers for Linux (which have not been upstreamed yet), posted a series of threads on Mastodon ([first one](https://vt.social/@lina/113045455229442533)), expressing sympathy for Wedson and her own frustrations with maintainers and Rust from the DRM perspective. While it’s tempting to look at this as just “Rust haters vs Rust pushers” drama, I think these signal deeper issues in Linux, both technical and cultural (which often feed into each other). In this article, I’ll summarize the issues for those unfamiliar and speculate where it could lead.

## Technical

### Documentation and interfaces

One of the big problems the Rust people were dealing with is getting documentation for interfaces. You would need this documentation for writing things in C, but for the Rust parts of the kernel, it’s even more critical. Since Rust encodes things like resources lifetimes as part of the type, knowing this is critical to be able to use the interface at all safely. The lack of this information beyond having to read the implementation or have it in your head hurts all new consumers of the APIs, refactors, and of course, Rust. If the subsystem maintainers aren’t helpful with questions like this, Rust or not, it makes it very hard to use the interface with any confidence.

### Testing and CI

Unit tests have a critical part of software engineering for decades. Continuous integration has recently became popular, but complements testing. Without tests, how can you know your changes still work, and without CI, how will you know until it’s too late? For replacing subsystems or drivers, it’s especially important if you’re employing things like the [strangler pattern](https://martinfowler.com/bliki/StranglerFigApplication.html).

Unfortunately, the kernel has only recently gained unit testing in the form of things like [kunit](https://lwn.net/Articles/780985/). CI has also [only recently begun to show up](https://lwn.net/Articles/972713/), but is inconsistent throughout the project for factors we’ll get into later. It is understandable for some things; unit tests for a driver may be hard without hardware and the ability to observe it. There are some progress with refractors to enable testings with other things though, such as [memory management](https://social.kernel.org/objects/9b8e2a26-4a43-407b-b927-1c116fc6f1c5).

### Development workflow

Yes, it’s about email. The Linux Kernel Mailing List is notorious for being a holdout with email based development workflows, though there has been mumbling about doing things differently (and some leeway for subsystems to do things their own way). This does mean that contribution is harder (setting up `git-send-email` is one thing, but using it for i.e. iterating on review is another), and things are likely to fall out of sight in an inbox without tooling to make it easier.

## Culture

### A collective

Arguably, Linux isn’t one project – it’s a collection of several different cliques. A friend joked that it’s not a kernel – it’s the single largest collection of open source device drivers on the planet. Filesystems, graphics, and so on are all their own communities with their own preferences. While Linus is arguably the one that holds the most power, in practice he rarely speaks up and prefers to trust and delegate to his lieutenants. While his rants when he’s displeased are notorious, they’re quite rare. Linus’ power is based on his reputation and respect less than any hard rules.

This also means the practices of each subsystem are quite different. Some have their own forges with CI and issue tracking, some are more particular about style than others, and so on. You could send emails only to the maintainer of one subsystem, and if they merge it into their branches, they’ll deal with merging it to the upstream source of truth. This makes the kernel again, less one project, and more many projects in a trenchcoat.

### Size of community

The Linux kernel community isn’t growing as fast as the workload. It’s the largest free software project in the world, yet they’re short of reviewers and maintainers. Arguably, development practices and the kernel community’s reputation makes people hesitant to join. Regardless, it does mean maintainers do not want to take on additional workload, like reviewing code in a language they’re unfamilar with. More maintainers, be it for the Rust-specific aspects or the subsystem in general could help here, both by diffusing workload and power of maintainers. Improving things like the kernel’s testing story or autoformatters too would help, by having automation reduce maintainer workloads on each patch.

### Collective conservatism

Because Linux is a bunch of communes in a trenchcoat, big changes are hard. This does have some mixed blessings; some subsystems could be easier to convince than others if you only need to convince a few, after all. Otherwise, the default speed of anarchism is incremental change, unless the problem is glaringly obvious to everyone. Leadership or organization (in the structural sense) could force resolutions (be it Linus being very angry at a maintainer, or appointing new maintainers), but these tend to be exceptional cases.

## What’s next?

I think Rust for Linux as a project is in danger as a project, not because of technical reasons (though larger kernel ones don’t help matters), but because of social ones. It’s trivial for a maintainer who doesn’t want Rust to sandbag integration efforts for their subsystem, for whatever reason (not liking it, not wanting the workload, etc.) via refusing to help. You need to either convince them otherwise, work around them (i.e. focus on other subsystems), or get some kind of leadership/organizational action. The first option seems hard  to convince everyone (if some seem set against it to the point of calling it “a religion”), the second option is presumably already being done, and the third is rare.

It might be what it comes to though. Linus seems interested in RfL as a project, so seeing his lieutenants resist might inspire action – be it making moves to prevent it from being blocked, or declaring RfL as incompatible with the project culturally. Both will have long lasting consequences.

### Long term

Rust for Linux is perhaps serving a a symbol for how the kernel handles big changes in general. Industry trends are pushing for languages that make it easier to write large-scale systems and spreading to existing large-scale systems (and of course, the whole memory safety thing). How does the kernel react to such things beyond refactoring existing things? The RfL saga shows it may involve someone stepping up to highlight the problems, or make things happen with pressure – meaning that it requires external stimulus beyond the usual flow of kernel development.

There’s also the question if this fractures (or hits existing cracks) in the kernel community. Linus as a unifying figure isn’t forever (he could get hit by a bus, piss people off, etc.), and who knows if Greg K-H could command the same respect. There are a lot of factions, be it by subsystem, corporations funding contributors, etc – they might not have the same interests and could use maintainer processes to sabotage each others’ efforts. It’s possible a fork happens, be it like egcs (fork due to technical issues, may get folded into the upstream project or take its own path), OpenBSD (cultural/personal issues that gains a direction afterward), io.js (fork that folds immediately), or something new.
