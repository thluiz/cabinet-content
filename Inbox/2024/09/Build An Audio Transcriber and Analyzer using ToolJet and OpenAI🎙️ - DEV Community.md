---
created: 2024-08-30T21:24:45 (UTC -03:00)
tags: [javascript,webdev,ai,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/tooljet/build-an-audio-transcriber-and-analyzer-using-tooljet-and-openai-1109?context=digest
author: 
---

# Build An Audio Transcriber and Analyzer using ToolJet and OpenAI🎙️ - DEV Community

> ## Excerpt
> In this hands-on tutorial, we’ll learn how to build a powerful audio transcriber and analyzer using...

---
In this hands-on tutorial, we’ll learn how to build a powerful audio transcriber and analyzer using ToolJet and Open AI. We'll quickly design an intuitive UI using ToolJet's pre-built [components](https://docs.tooljet.com/docs/tooljet-concepts/what-are-components/), and then use the platform's [query builder](https://docs.tooljet.com/docs/tooljet-concepts/what-are-queries) to interact with Open AI for audio transcription and analysis.

By the end of this tutorial, we'll have a fundamental structure to build more sophisticated transcription and audio analysis applications.

___

**Prerequisites**:

-   **ToolJet** ([https://github.com/ToolJet/ToolJet](https://github.com/ToolJet/ToolJet)) : An open-source, low-code platform designed for quickly building and deploying internal tools. Sign up for a free ToolJet cloud account [here](https://app.tooljet.com/).
-   **Open AI Account** : Register for an Open AI account to utilize AI-powered features in your ToolJet applications. Sign up [here](https://openai.com/).

___

Here's a quick preview of what we are going to build:

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ffvahkytka6o04f3d76g7.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ffvahkytka6o04f3d76g7.png)

Before you begin, go to the [Open AI Console](https://platform.openai.com/api-keys), and copy your secret key. Next, login to your [ToolJet account](https://app.tooljet.com/), locate the **Data Sources** section on the left sidebar, and configure Open AI as a data source using the secret key.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fnu5xh4zzjm076lsonfdn.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fnu5xh4zzjm076lsonfdn.png)

Once the data source is configured, create a new app called "Speech Insight" from the dashboard. And with that, we are ready to start building our application.

___

## [](https://dev.to/tooljet/build-an-audio-transcriber-and-analyzer-using-tooljet-and-openai-1109?context=digest#step-1-building-the-ui-for-the-audio-transcriber)Step 1: Building the UI for the Audio Transcriber

Let's use ToolJet's visual app builder to design our UI.

-   For the app header, drag and drop an **Icon** component on the canvas. Navigate to its properties panel on the right, and select the `IconBrandDingtalk` icon.
-   Drop a **Text** component next to it, and enter "Speech Insight" under its Data property.
-   Change the color of both components to blue (`#3E63DD`). This will be the primary color scheme of our app, update the color scheme of the remaining components accordingly.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftft7as3w2n2hydid22ll.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftft7as3w2n2hydid22ll.png)

-   Place a **Container** component below the header. We will organize the upcoming components inside the Container component.
-   Rename it to `mainContainer`.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fpxfzmcowcuthl26rgz0w.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fpxfzmcowcuthl26rgz0w.png)

-   On the top left of the Container component, place a **Text** component with the label "Output".
-   Below it, add another **Container** and place two **Text** components inside it. We will use these components to display the transcribed text and feedback. Name them `transcribedText` and `feedback` respectively.
-   Add a **File Picker** component below it and rename it to `uploader`. Change its `Accept file types` property to `"audio/*"`.
-   Place a **Button** component below it and rename it to `analyzeButton` and add an "Analyze Button" label to it.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F1hn33z3rpcx6v5omuchx.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F1hn33z3rpcx6v5omuchx.png)

_Note:_ We are renaming key components to make them easier to reference in other parts of our application.

-   Finally, place four **Statistics** components on the right for Fluency, Pronunciation, Intonation, and Vocabulary scores.
-   Drop a **Button** component below it labeled "Copy Output" and rename it to `copyButton`.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbl7w02dtioa3ibwqga8t.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbl7w02dtioa3ibwqga8t.png)

The UI is now ready! Time to configure the interactions with Open AI.

___

## [](https://dev.to/tooljet/build-an-audio-transcriber-and-analyzer-using-tooljet-and-openai-1109?context=digest#step-2-interact-with-open-ai)Step 2: Interact With Open AI

In the below steps, we will go through the configuration to interact with Open AI using both REST API and ToolJet's native integration.

-   Expand the query panel at the bottom and click on the **Add** button to create a new REST API query. Rename the query to `transcribe`.
-   Enter the Open AI URL under the `URL` property: `https://api.openai.com/v1/audio/transcriptions`.
-   Add a new row under **Header**, enter `Content-Type` as the key and `multipart/form-data` as the value.
-   Add another key for `Authorization`. Enter `Bearer <OPEN_AI_KEY>` in the value.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fg1wpbxyluwy29otqhipr.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fg1wpbxyluwy29otqhipr.png)

-   Under **Body**, add `file` as the key and value as `&#123;&#123; components.uploader.file[0] &#124;&#124;`. This will ensure the audio file selected in our uploader/File Picker component is sent.
-   Add `model` as the key and enter `whisper-1` as the value.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fd4pgec9dp0htbqp0jfbw.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fd4pgec9dp0htbqp0jfbw.png)

Now if we select an audio file in the uploader/File Picker component and click on the **Run** button, we will see the transcribed audio as the output.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fuwgdw0sq52bsbixc1wfs.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fuwgdw0sq52bsbixc1wfs.png)

Once the audio is transcribed, we need to analyze it to provide a score. Let's use the native Open AI integration for this query.

-   Click on the **Add** button and add a new query. Select **Open AI** as the data source for this query. This is the same data source that we had set up at the beginning. Rename it to `analyze`.
-   Select **Chat** as the operation and **Message** as input, and enter the below prompt:

```
<span>Based</span> <span>on</span> <span>the</span> <span>transcribed</span> <span>audio</span> <span>below</span><span>,</span> <span>provide</span> <span>a</span> 
<span>JSON</span> <span>object</span> <span>in</span> <span>response</span> <span>with</span> <span>the</span> <span>following</span> <span>details</span><span>:</span>

   <span>-</span> <span>Fluency </span><span>(</span><span>out</span> <span>of</span> <span>10</span><span>)</span>
   <span>-</span> <span>Pronunciation </span><span>(</span><span>out</span> <span>of</span> <span>10</span><span>)</span>
   <span>-</span> <span>Vocabulary </span><span>(</span><span>out</span> <span>of</span> <span>10</span><span>)</span>
   <span>-</span> <span>Intonation </span><span>(</span><span>out</span> <span>of</span> <span>10</span><span>)</span>
   <span>-</span> <span>A</span> <span>paragraph</span> <span>that</span> <span>gives</span> <span>general</span> <span>feedback</span> 
<span>on</span> <span>the</span> <span>transcribed</span> <span>text</span><span>'</span><span>s quality and overall improvement suggestions.

   Return the object in the following format:

   {fluency: "...", pronunciation: "...", 
vocabulary: "...", intonation: "...", feedback: "..."}

   Transcribed text:
   &#123;&#123;queries.transcribe.data.text&#124;&#124;
</span>
```

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fe2qacepaqbz6z5jozaaj.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fe2qacepaqbz6z5jozaaj.png)

In this prompt, we are using Open AI to perform a detailed analysis of the audio transcription. We are referencing the data returned by the `transcribe` query in the prompt along with other scoring criteria.

Running this query will result in the following output:

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fcfkw4y3qw3gmgrvmh6y1.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fcfkw4y3qw3gmgrvmh6y1.png)

Both the queries are ready. As a final step, let's automate the process of triggering the `analyze` query every time the `transcribe` query is successfully executed.

-   Go back to the `transcribe` query, navigate to **Events** and add a new [event](https://docs.tooljet.com/docs/tooljet-concepts/what-are-events/) handler.
-   Select **Query Success** as the Event, **Run Query** as the Action, and `analyze` as the Query.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdcbwgj5whjp2u3lzkeso.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdcbwgj5whjp2u3lzkeso.png)

By using events, we have set up the process of triggering the `analyze` query after the `transcribe` query is triggered and is ready with the output for analysis.

___

## [](https://dev.to/tooljet/build-an-audio-transcriber-and-analyzer-using-tooljet-and-openai-1109?context=digest#step-3-binding-the-transcripts-and-analysis-to-components)Step 3: Binding the Transcripts and Analysis to Components

Onto the final step. We have built our UI and also built queries to interact with Open AI. Now we can connect it all together and see the app in action.

-   Select the **Analyze Audio** button, navigate to its properties panel on the right and add a new event.
-   Select **On click** as the Event, **Run Query** as the Action, and `transcribe` as the Query.

Now every time the Analyze Audio button is clicked, it will trigger the `transcribe` query.

-   Select the **Copy Output** button and add a new event to it.
-   Select **On click** as the Event, **Copy to clipboard** as the Action, and `&#123;&#123;queries.analyze.data&#124;&#124;` as the Text.

This configuration will ensure that the analyzed output gets copied when you click on the Copy Output button.

-   Select the **Text** component that we had placed to display the transcript. Enter the following value under its Data property:  
    **Transcript:** `&#123;&#123;queries.transcribe.data.text&#124;&#124;`
    
-   Select the **Text** component to display the feedback. Enter the following value under its Data property:  
    **Feedback:** `&#123;&#123;JSON.parse(queries.analyze.data).feedback&#124;&#124;`
    

_Note:_ We received a JSON string in response to our `analyze` query. Therefore, we need to parse it to construct a JavaScript object before displaying it.

-   Select the **Statistics** component for "Fluency" and enter the below value under its `Primary value` property:  
    `&#123;&#123;JSON.parse(queries.analyze.data).fluency&#124;&#124;`
    
-   Update the rest of the Statistics components using the same logic.
    

Our audio transcriber and analyzer is now fully complete. Upload an audio file and click on the Analyze Audio button to see the transcription, feedback, and scores getting populated in the UI.

[![Image description](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fh4qq0bzys8izun9tvfng.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fh4qq0bzys8izun9tvfng.png)

___

## [](https://dev.to/tooljet/build-an-audio-transcriber-and-analyzer-using-tooljet-and-openai-1109?context=digest#conclusion)Conclusion

In this tutorial, we learned how to create a complete audio transcription and analysis tool using ToolJet and OpenAI. We walked through designing an intuitive user interface, setting up API queries to interact with OpenAI, and binding the results to display transcriptions, feedback, and speech analysis scores.

To further customize the application, experiment with different UI components to enhance the user experience or integrate additional APIs to analyze other aspects of the audio, such as emotion detection or language translation.

To learn more, check out ToolJet's official [documentation](https://docs.tooljet.com/docs/) or connect on [Slack](https://tooljet.slack.com/) for questions or queries.
