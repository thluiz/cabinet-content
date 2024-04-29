---
created: 2024-04-18T19:26:54 (UTC -03:00)
tags: [softwareengineering,career,software,coding,development,engineering,inclusive,community]
source: https://dev.to/moozzyk/write-good-commit-messages-to-accelerate-your-dev-career-2mjf
author: 
---

# Write good commit messages to accelerate your dev career - DEV Community

> ## Excerpt
> “’Fixing a bug’ is a great commit message!” said no one.  Why? Because commit messages like this make...

---
“’Fixing a bug’ is a great commit message!” said no one.

Why? Because commit messages like this make our jobs frustrating.

Good commit messages on the other hand, make our jobs easier and save everyone time as they:

-   give more clarity to the diff author - if you, as the diff author, have a hard time writing a good commit message maybe your change is not ready for review?
-   help understand what the person who wrote the code was thinking which makes it much easier to grasp reasons why the code was written the way it was written and helps avoid introducing regressions
-   provide the context for code reviewers and as a result facilitate faster code reviews by lowering the barrier to entry for anyone willing to review the change
-   allow finding commits quicker

Note that this is not about making commit messages longer. Long commit messages are not necessarily good commit messages. Good commit messages are short but informative. In many cases a single line explaining what the change is about is sufficient. More complex changes, especially bug fixes, often require more context:

-   What exactly the change is about. This information is a must, and should be always included in the title.
-   Why you are making this change. What scenario it unblocks. What issue it fixes.
-   Short description of implementation details.
-   If fixing a bug, how to reproduce it.
-   How the change was tested - especially useful for any validation beyond unit tests
-   Links to related issues or tasks

You can find an expanded version of this post here: [https://growingdev.substack.com/p/how-to-remove-unnecessary-friction](https://growingdev.substack.com/p/how-to-remove-unnecessary-friction)
