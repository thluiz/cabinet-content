---
created: 2024-07-24T09:14:20 (UTC -03:00)
tags: [devops,beginners,productivity,learning,software,coding,development,engineering,inclusive,community]
source: https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest
author: 
---

# 20 Life hacks for DevOps Engineers - DEV Community

> ## Excerpt
> Tricks of the trade, hacks, trade secrets, cheat sheets, best practices, call them what you will....

---
Tricks of the trade, hacks, trade secrets, cheat sheets, best practices, call them what you will. Every industry has them, and anyone who sticks around long enough builds an arsenal of techniques and finely tuned tools to excel at their job.

Some things simply take time to master. My dad, a retired builder, could tile a medium-sized bathroom in under an astonishing three hours, while it would take me a full day just to do the grouting afterwards. Experience is key for certain skills, but there are also tips that don’t require years of practice to acquire.

![gif](https://res.cloudinary.com/practicaldev/image/fetch/s--iMS8bpDp--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://media3.giphy.com/media/SXZj5WFLxiSdy/200.webp%3Fcid%3D790b761164na8hke0pfgznqogjrmxfnl9fulp4fhqyybt7o8%26ep%3Dv1_gifs_search%26rid%3D200.webp%26ct%3Dg)

DevOps is no different. There are no shortcuts to becoming a god-tier DevOps savant, put the years in and you might just get there. Having said that, there are some tricks of the trade, life hacks and useful tooling that are sure to give you an instant boost in productivity.

Here is my non-comprehensive list of life hacks guaranteed to make any DevOps engineer’s life easier.

The list is broken down into:

-   Tooling 🧰
-   Skills 🤸
-   Habits 🔁
-   Scripts, configs and extensions 💻

## [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#tooling)Tooling: 🧰

Did you know that if you really want to get somebodies attention in Germany, [sending a fax](https://www.npr.org/2024/05/09/1250136510/germany-has-a-reputation-for-efficiency-so-why-do-fax-machines-remain-popular) is your best bet?  
Also, in Japan up until this year, [floppy disks](https://www.abc.net.au/news/2024-07-10/japan-eliminates-use-of-floppy-disks/104077114) were still in use by government institutions.

Knowing which context you find yourself in is essential to then choose the best tools for the job. Even though it’s important not to obsess over having the best, newest, or shiniest tool on the market. The following tools can be real game-changers for DevOps engineers.

> "You don’t need to be the sharpest tool in the shed to use the sharpest tools in the shed" - Anonymous (I might have made it up)

![gif-2](https://res.cloudinary.com/practicaldev/image/fetch/s--yXKpPU96--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExbHd3NjR5OTA3cXB3c3NkbjBibGFzaWc5MTE2cTNrZWM4YmxmOWN3cCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/IelxugxenjdyU/giphy.gif)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#1-k9s)1\. k9s

[K9s](https://k9scli.io/) is a terminal-based UI for interacting with your Kubernetes clusters. It takes very little time to get accustomed to navigating, observing, and managing your live applications through the tool. And once you do, you might never go back. K9s continually monitors Kubernetes for changes and offers many useful commands to interact with your observed resources.

Link to [instal](https://k9scli.io/topics/install/).

[![k9s](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5qk2ahx3y75ho015a0mo.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5qk2ahx3y75ho015a0mo.png)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#2-tmux)2\. tmux

[tmux](https://github.com/tmux/tmux/wiki) is a powerful terminal multiplexer that enhances productivity by allowing session persistence, window and pane management, and customization through key bindings and configuration files. It supports scripting for automation, facilitates collaboration with shared sessions, and integrates well with various shells and tools.

[![tmux](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5g9z9v1la2z1is08ti6x.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F5g9z9v1la2z1is08ti6x.png)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#3-glasskube)3\. Glasskube

[Glasskube](https://github.com/glasskube/glasskube) is an Open Source package manager for Kubernetes. It makes deploying, updating, and configuring packages on Kubernetes **20 times faster** than tools like **Helm or Kustomize**. Inspired by the simplicity of Homebrew and npm. You can decide if you want to use the Glasskube UI, CLI, or directly deploy packages via GitOps.

[![glasskube-dash](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxzyje56hrgu3i75phal5.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxzyje56hrgu3i75phal5.png)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#care-to-support-us)Care to support us?

If this is the first time you've heard of Glasskube, we are working to build the next generation `Package Manager for Kubernetes`.

If you like our content and want to support us on this mission, we'd appreciate it if you could give us a star ⭐️ on [GitHub](https://github.com/glasskube/glasskube).

![star-gif](https://res.cloudinary.com/practicaldev/image/fetch/s--_E1k546n--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExOHVuenNmNGJnYWhzam43MmkxemNrNzloOHJ3ZzZmbGFyb3lseGNteSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/l3q2wJsC23ikJg9xe/giphy.gif)

⭐️ Star us on [GitHub](https://github.com/glasskube/glasskube) 🙏

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#4-ripgrep)4\. ripgrep

[Riggrep](https://github.com/BurntSushi/ripgrep) is a powerful search tool known for its speed, flexibility, and user-friendly output. It quickly processes large codebases using advanced algorithms, supports a wide range of search patterns, and presents clear, highlighted results. Riggrep integrates well with other tools, is cross-platform, and customizable.

[![ripgrep](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fx33jvnbgbdzd4li38p9x.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fx33jvnbgbdzd4li38p9x.png)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#5-firefox-containers-for-multi-cloud-account-access)5\. Firefox containers for multi cloud account access

[Firefox Multi-Account Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/) is a such an underestimated browser extension for it's utility. It helps manage online activity by separating websites into different containers or tabs, preventing sessions tracking across sites. It’s most useful feature is that it allows users to log into multiple accounts simultaneously within the same browser. By isolating sessions through cookie separation, it protects personal data and improves the overall browsing experience. Have multiple AWS accounts? Not an issues, you can log in to all of them from the same browser window.

[![firefox-tabs](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxr7uji0zrxekr3bt0arq.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fxr7uji0zrxekr3bt0arq.png)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#6-vpa)6\. VPA

Vertical Pod Autoscaler (VPA) frees users from the necessity of setting up-to-date resource limits and requests for the containers in their pods. When configured, it will set the requests automatically based on usage and there after allow proper scheduling onto nodes so that appropriate resource amounts are available for each pod.

Install it [here](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler#install-command).

Example config:  

```
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: my-app-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind:       Deployment
    name:       my-app
  updatePolicy:
    updateMode: "Auto"
```

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#7-kctx-and-kubens)7\. Kctx and Kubens

Is just hand’s down the most useful CLI tool for Kubernetes context and Namespace switching.

Install it [here](https://github.com/ahmetb/kubectx)

![kubectx](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fnfovv5rex6xirw0o7ltx.gif)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#8-chatgpt-for-guidance)8\. ChatGPT for guidance

Use [ChatGPT](https://chatgpt.com/) as if it were a senior member on your team who is never busy and always happy to answer all of your questions. Make sure your intention is deeper understanding and not blind task resolution.

Prime ChatGPT to be a senior team member with a prompt like this:

> You will act as a Senior DevOps Engineer to provide life hacks and tips on how to excel in the field of DevOps. You will also be ready to help junior team members with any questions they might have. Please include practical advice, recommended tools, and best practices for managing infrastructure and continuous integration/continuous deployment (CI/CD) pipelines. Write the output using my communication style, which is clear, concise, and practical. Here are examples of my communication style:
> 
> -   "Focus on automating repetitive tasks to save time and reduce errors."
>     
> -   "Leverage tools like Docker and Kubernetes for containerization and orchestration."
>     
> -   "Always monitor system performance and be proactive in identifying potential issues."
>     
> -   "When mentoring juniors, be patient and explain concepts in simple terms."
>     

**Here are some questions you can ask ChatGPT to further fine tune the prompt:**

1.  Do you want the advice and explanations to cater more to beginner-level juniors or those with some experience in DevOps?
    
2.  Could you provide more detailed examples of your communication style, especially in scenarios where you explain complex concepts to juniors?
    
3.  Are there specific challenges or areas of focus within DevOps (e.g., automation, monitoring, security) that you want to prioritize for the advice and junior support?
    

## [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#skills)Skills: 🤸

It's no surprise that skills can't be absorbed instantly, they require time and effort. In the ever-evolving tech world, determining which skills to cultivate can be confusing. However, as DevOps engineers, you can't go wrong with mastering scripting and prioritizing documentation.

> “Build your skills not your resume.” — [Sheryl Sandberg](https://quotefancy.com/sheryl-sandberg-quotes)

![skills](https://res.cloudinary.com/practicaldev/image/fetch/s--FbEuX1ck--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://media2.giphy.com/media/i7EXEMgsP7oSk/200.webp%3Fcid%3Decf05e47axxswg5q93zogb1hjld9wyxerrlxa0rwhhdzebue%26ep%3Dv1_gifs_search%26rid%3D200.webp%26ct%3Dg)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#9-scripting)9\. Scripting

Scripting is like a swiss army knife for DevOps engineers because it automates repetitive tasks, provides essential glue between processes, and ensures consistency across environments. Learning and practicing scripting involves familiarizing yourself with tools like [Makefiles](https://opensource.com/article/18/8/what-how-makefile), regular expressions ([regex](https://support.smartbear.com/testcomplete/docs/scripting/regular-expressions.html)) for efficient text processing, and [Bash](https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/) scripting for powerful command-line operations.

Don't feel like you have to master scripting before implementing it at work. Look for manual steps in your current software or infrastructure delivery processes and try to script automations that make sense. Leverage an LLM for help if needed.

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#10-documentation)10\. Documentation

Write everything down. It’s the best way to take care of your future you.

Try out a few note taking solutions here are a few:

-   [Notion](https://www.notion.so/)
    
-   [Outline](https://www.getoutline.com/)
    
-   [Obsidian](https://obsidian.md/)
    
-   [Google Keep](https://keep.google.com/)
    
-   [Joplin](https://joplinapp.org/)
    

It doesn’t matter which one you choose as long as you only choose one and you stick to it.

> 🤔 Don’t fall into the trap of spending too much time organising and optimising your notes. Notes don’t have to be perfect only functional. Spending too much time on note maintenance is `meta-work`.

## [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#habits)Habits: 🔁

You will never get far if you rely solely on pure motivation to get things done and improve. Well-defined and consistent habits are the glue that keep you growing and effective, even when your motivation wanes. Habits seamlessly integrate tools and skills, ensuring you are effective at your job.

> "Motivation is what gets you started. Habits is what keeps you going." - Jim Ryun

![practice](https://res.cloudinary.com/practicaldev/image/fetch/s--5fw7BZWC--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExc3E0OXFnNnVqZnJxajRka21jcTJsNXY2bnlnbW8wa280NnJoNnlzeSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/8AfVHQbGG8dxGCC7ES/giphy.gif)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#11-dont-write-todo-lists-block-time)11\. Don’t write to-do lists, block time.

I’ve been heavily influenced by Cal Newport’s writing on Deep Work and I’m quite convinced that efforts to preserve a certain amount of time per week dedicated to uninterrupted **deep work** is crucial for an individual who wants to contributor meaningfully to a team.

To-do list on their own are just wish lists. Once you plot them on a calendar, that’s when you have a plan.

> It’s important to point out that I know very few people who execute on their time block plans with 100% fidelity every day of the week. Use them as north stars, schedule rest time when you need it. Update the list throughout the day even. But at least hold yourself to a plan.

[![time-block](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fajpplmlajhjl8bpcx05a.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fajpplmlajhjl8bpcx05a.png)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#12-reciprocal-meeting-blocks)12\. Reciprocal meeting blocks

Unless you are your own boss, you’re most likely not 100% in charge of the meetings and commitments you might be obliged to participate in. Reciprocal meeting blocks is a way to counteract unexpected time commitments that just pop up in your calendar.  
The idea here to reserve an equivalent deep work block every time a new meeting is added to you calendar. That way you can stay flexible and relatively available without having to compromise on your weekly deep work quota.

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#13-have-a-shutdown-routine)13\. Have a shutdown routine

Especially useful if you are a remote worker. A shutdown routine is a series of questions and steps your run through to finish your working day. Ideally once you complete the checklist you should be able to forget about your work until the next day.

What I track is the following:

-   Did I exercise?
    
-   Did I address all of the miscellaneous tasks I had?
    
-   Do I have any open conversations?
    
-   Do I need to push any tasks to the next day?
    
-   Did I write a diary entry? (not work related but I like to get it done before I leave the desk)
    
-   How many deep hours did I do?
    
-   Did I check the metrics I’m tracking one last time before I close the laptop for the day?
    

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#14-take-notes-during-meetings)14\. Take notes during meetings

Taking notes during meetings and sharing them afterward should ideally be a common practice in your organization. If it isn't, this is a perfect opportunity for you to start. Not only does it ensure that no important details slip through your fingers, but it also provides a great service to your team.

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#15-test-run-outages)15\. Test run outages

Test run outages are essential for DevOps engineers to prepare for real incidents. You have to know exactly how to swiftly and efficiently connect to your clusters or VMs before an unexpected 1 a.m. alert for example, you can’t be caught off guard.

Familiarize yourself with moving files, retrieving container logs, and other critical tasks. Setting up SSH keys, kubeconfigs, and other access tools in advance will save valuable time and reduce stress during actual outages. Proactively testing and optimizing these processes ensures that you are ready to respond effectively to any disruption.

## [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#scripts-configs-and-extensions)Scripts, configs and extensions: 💻

If you have to do something more than once, automate it. Why write out a full command when you can save time by using an alias? Over a long career, it might be hard to calculate exactly how much time is saved by typing "k" instead of "kubectl." One thing is for sure: it's a lot, and it's well worth it.

> “You’re either the one that creates the automation or you’re getting automated.” — Tom Preston-Werner

![automation](https://res.cloudinary.com/practicaldev/image/fetch/s--I9mpQaQd--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://media3.giphy.com/media/PlLanl8Bzcvr14IfjJ/200.webp%3Fcid%3D790b7611yt1u5wcvmarfszupicy0wiilvzaf1meik7a1knl8%26ep%3Dv1_gifs_search%26rid%3D200.webp%26ct%3Dg)

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#16-use-helpful-aliases)16\. Use helpful aliases

Don’t waste time typing out the full commands you write every day.

Here is a small snippet of a few of my configured aliases:  

```
k=kubectl
kctx='kubectl ctx'
kgp='kubectl get pods'
kns='kubectl ns'
l='ls -lah'
la='ls -lAh'
ll='ls -lh'
ls='ls -G'
lsa='ls -lah'
md='mkdir -p'
rd=rmdir
run-help=man
```

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#17-efficiently-cleaning-up-finished-jobs-with-ttl-controller)17\. Efficiently cleaning up finished jobs with TTL controller

You can specify the lifespan of finished jobs or pods before they are automatically removed by setting the `.spec.ttlSecondsAfterFinished` field. If you working in a job heavy environment finished jobs can quickly accumulate and become quite resource heavy.

Example config:  

```
apiVersion: batch/v1
kind: Job
metadata:
  name: test-ttl-job
spec:
  ttlSecondsAfterFinished: 100
  ...
```

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#18-git-script-to-keep-synced-with-the-upstream)18\. Git script to keep synced with the upstream

```
git remote add upstream &lt;upstream-url&gt;
git fetch upstream
git rebase upstream/main
git push --force-with-lease
```

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#19-kubectl-auto-complete)19\. Kubectl auto complete

Kubectl autocompletion lets you create an alias for kubectl. This feature saves time by reducing the need for cheat sheets and is especially useful for managing Kubernetes clusters. It's also recommended for the CKA exam due to its time efficiency.

Set up for Linux:  

```
# install bash-completion
sudo apt-get install bash-completion

# Add the completion script to your .bashrc file
echo 'source &lt;(kubectl completion bash)' &gt;&gt;~/.bashrc

# Apply changes
source ~/.bashrc

```

Check out other installation methods [here](https://komodor.com/learn/kubectl-autocomplete-enabling-and-using-in-bash-zsh-and-powershell/).

### [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#20-visual-studio-code-remote-ssh)20\. **Visual Studio Code Remote – SSH**

The Remote - SSH extension lets you use any SSH-enabled remote machine for development, making it easier to develop on the same OS you deploy to, use powerful hardware, switch between environments, and debug applications remotely.

Install it [here](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh).

## [](https://dev.to/glasskube/20-life-hacks-for-devops-engineers-3dn7?context=digest#final-thoughts)Final thoughts

There’s no magic formula for becoming a top 1% DevOps engineer. Like in most professions, it’s a matter of time, dedication, and experience that will gradually shape you into a highly effective professional. Over time, you'll get much better at recognizing patterns, recalling past situations, and finding quick solutions to recurring issues. So, don’t expect any single life hack from this list to instantly earn you a 50% raise and a promotion.

However, if you consistently focus on refining your tools, honing your skills, refusing to break good habits, and implementing smart automations, you’ll be well on your way to rapidly evolving and surpassing your current self. And who knows, maybe that promotion is just around the corner.

___

If you like our content and want to support us on this mission, we'd appreciate it if you could give us a star ⭐️ on [GitHub](https://github.com/glasskube/glasskube).

![giff](https://res.cloudinary.com/practicaldev/image/fetch/s--_E1k546n--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExOHVuenNmNGJnYWhzam43MmkxemNrNzloOHJ3ZzZmbGFyb3lseGNteSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/l3q2wJsC23ikJg9xe/giphy.gif)

[⭐️ Star us on GitHub 🙏](https://github.com/glasskube/glasskube)
