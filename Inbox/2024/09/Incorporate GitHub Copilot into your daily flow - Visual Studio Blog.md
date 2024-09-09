---
created: 2024-09-05T14:48:51 (UTC -03:00)
tags: []
source: https://devblogs.microsoft.com/visualstudio/incorporate-github-copilot-into-your-daily-flow/
author: Rhea Patel
---

# Incorporate GitHub Copilot into your daily flow - Visual Studio Blog

> ## Excerpt
> Streamlining Workflow with GitHub Copilot  Have you ever received code completions that are too large to manage or ones that need slight tweaks, but you must accept all the code to make those changes? To address these pain points, in Visual Studio 17.11 we’ve introduced a new feature that allows you to refine your completions […]

---
## **Streamlining Workflow with GitHub Copilot** 

Have you ever received code completions that are too large to manage or ones that need slight tweaks, but you must accept all the code to make those changes? To address these pain points, in Visual Studio 17.11 we’ve introduced a new feature that allows you to refine your completions by adding extra context or asking clarifying questions. Now, you can move directly into Inline Chat and view the suggested code in a more accessible way, without having to accept and modify everything in the editor.

### **Refining GitHub Copilot Completions with Inline Chat**

[![Image completionstoinline](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/completionstoinline.png)](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/completionstoinline.png)

You now have more control over the suggestions provided by GitHub Copilot. Instead of merely accepting or ignoring a suggestion, you can now modify and retry! This feature allows you to fine-tune the suggestions given by GitHub Copilot, by adding context or tweaking the suggestion.

[![Image newinlinerefinement](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/newinlinerefinement.png)](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/newinlinerefinement.png)

### **Promoting Inline Chat to the Chat Window for More Context**

Preserve the history of your Inline Chat by promoting it to the Chat Window. This feature enables you to maintain a record of the conversation and continue the Chat Window at your convenience on a larger screen. To do this, simply select ‘Continue in chat window…’.

[![Image continuetochat](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/continuetochat.png)](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/continuetochat.png)

We’ve adjusted the length of responses from Copilot to ensure they are easily accessible and readable in Inline Chat. When you want to learn more or expand, you can switch to the Chat Window for a larger screen real estate. This will easily enable you to refer back to things later and chat outside of the context of your file.

## Understand your symbols right from your editor

GitHub Copilot can now help you understanding descriptions of various symbols at different invocations within their codebase right from your editor. and select **Generate Copilot summary** to get provide AI-generated summaries of the selection. This is available for both C# and C++ developers.

[![Image ontheflydocs](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/ontheflydocs.png)](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/ontheflydocs.png)

Leveraging LLMs, GitHub Copilot enhances existing or lacking code documentation by providing insightful explanations and context within hover tooltips.

## **(Coming Soon) Fix code with GitHub Copilot**

Now integrated into the lightbulb and error list, GitHub Copilot provides fixes and explanations for code issues in both C# and C++ development. This feature helps developers understand and resolve various code issues within their codebase. By invoking the lightbulb and selecting **Fix with Copilot**, GitHub Copilot will open inline chat with an explanation and available fix. Additionally, selecting the Copilot icon from the error list will take you to the chat panel with an explanation and available fix.

[![Image lightbulbtofix](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/lightbulbtofix.png)](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/09/lightbulbtofix.png)

Leveraging LLMs, GitHub Copilot enhances code fixes by providing insightful explanations and solutions directly within the lightbulb and error list.

These changes make it easier to refer back to previous conversations and continue discussions outside of your file. We hope this new feature enhances your experience with GitHub Copilot in Visual Studio. As always, we value your feedback and look forward to hearing your thoughts. Please use the thumbs up and down in chat to provide feedback.

[![Image send us feedback](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/07/send-us-feedback.png)](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/07/send-us-feedback.png)

We hope you enjoy this update to Visual Studio and all the new developments happening within GitHub Copilot, and we look forward to hearing what you think. You can share feedback with us by using the thumbs up or down within the Chat, via [Developer Community](https://developercommunity.visualstudio.com/home), by reporting issues via [report a problem](https://learn.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio?view=vs-2022) and [share your suggestions](https://developercommunity.microsoft.com/VisualStudio/suggest) for new features or improvements to existing ones.

Stay connected with the Visual Studio team by following us on [Twitter](https://twitter.com/VisualStudio), [YouTube](https://www.youtube.com/user/VisualStudio), and [LinkedIn](https://www.linkedin.com/showcase/microsoft-visual-studio/) and on [Microsoft Learn](https://learn.microsoft.com/en-us/visualstudio/?view=vs-2022).

Thank you for using Visual Studio and **happy coding!**

## Author

![Rhea Patel](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2024/01/dp-1-96x96.jpg)

![Sinem Akinci](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2023/10/i-WmBC8vP-X4-2-96x96.jpg)

C++ Product Manager working on Copilot, CMake, and Linux experiences in Visual Studio and VS Code

![Mika Dumont](https://devblogs.microsoft.com/visualstudio/wp-content/uploads/sites/4/2021/06/Mika-Headshot2-150x150.jpg)

Mika is a Product Manager on the .NET and GitHub Copilot developer experience.
