---
created: 2024-07-07T12:55:18 (UTC -03:00)
tags: []
source: https://aws.amazon.com/pt/blogs/machine-learning/build-your-multilingual-personal-calendar-assistant-with-amazon-bedrock-and-aws-step-functions/?ref=dailydev
author: 
---

# Build your multilingual personal calendar assistant with Amazon Bedrock and AWS Step Functions | AWS Machine Learning Blog

> ## Excerpt
> This post shows you how to apply AWS services such as Amazon Bedrock, AWS Step Functions, and Amazon Simple Email Service (Amazon SES) to build a fully-automated multilingual calendar artificial intelligence (AI) assistant. It understands the incoming messages, translates them to the preferred language, and automatically sets up calendar reminders.

---
Foreigners and expats living outside of their home country deal with a large number of emails in various languages daily. They often find themselves struggling with language barriers when it comes to setting up reminders for events like business gatherings and customer meetings. To solve this problem, this post shows you how to apply AWS services such as [Amazon Bedrock](https://aws.amazon.com/bedrock/), [AWS Step Functions](https://aws.amazon.com/step-functions/), and [Amazon Simple Email Service (Amazon SES)](https://aws.amazon.com/ses/) to build a fully-automated multilingual calendar artificial intelligence (AI) assistant. It understands the incoming messages, translates them to the preferred language, and automatically sets up calendar reminders.

[Amazon Bedrock](https://aws.amazon.com/bedrock/) is a fully managed service that makes foundation models (FMs) from leading AI startups and Amazon available through an API, so you can choose from a wide range of FMs to find the model that’s best suited for your use case. With Amazon Bedrock, you can get started quickly, privately customize FMs with your own data, and easily integrate and deploy them into your applications using AWS tools without having to manage any infrastructure.

[AWS Step Functions](https://aws.amazon.com/step-functions/) is a visual workflow service that helps developers build distributed applications, automate processes, orchestrate microservices, and create data and machine learning (ML) pipelines. It lets you orchestrate multiple steps in the pipeline. The steps could be [AWS Lambda](https://aws.amazon.com/lambda/) functions that generate prompts, parse foundation models’ output, or send email reminders using [Amazon SES](https://aws.amazon.com/ses/). Step Functions can interact with over 220 AWS services, including [optimized integrations](https://aws.amazon.com/blogs/aws/build-generative-ai-apps-using-aws-step-functions-and-amazon-bedrock/) with Amazon Bedrock. Step Functions pipelines can contain loops, map jobs, parallel jobs, conditions, and human interaction, which can be useful for AI-human interaction scenarios.

This post shows you how to quickly combine the flexibility and capability of both Amazon Bedrock FMs and Step Functions to build a generative AI application in a few steps. You can reuse the same design pattern to implement more generative AI applications with low effort. Both Amazon Bedrock and Step Functions are serverless, so you don’t need to think about managing and scaling the infrastructure.

The source code and deployment instructions are available in the [Github repository](https://github.com/aws-samples/build-multilingual-calendar-assistant-with-amazon-bedrock-and-aws-step-functions).

## Overview of solution

[![Figure 1: Solution architecture](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2024/06/25/ML-16234-Architecture.png)](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2024/06/25/ML-16234-Architecture.png)

Figure 1: Solution architecture

As shown in Figure 1, the workflow starts from the [Amazon API Gateway](https://aws.amazon.com/api-gateway), then goes through different steps in the Step Functions state machine. Pay attention to how the original message flows through the pipeline and how it changes. First, the message is added to the prompt. Then, it is transformed into structured JSON by the foundation model. Finally, this structured JSON is used to carry out actions.

1.  The original message ([example in Norwegian](https://github.com/aws-samples/build-multilingual-calendar-assistant-with-amazon-bedrock-and-aws-step-functions/blob/main/doc/sample-inputs/norsk1.json)) is sent to a Step Functions state machine using API Gateway.
2.  A [Lambda function](https://github.com/aws-samples/build-multilingual-calendar-assistant-with-amazon-bedrock-and-aws-step-functions/blob/main/src/lambda/prompt_generator/prompt_generator.py) generates a prompt that includes system instructions, the original message, and other needed information such as the current date and time. (Here’s the [generated prompt](https://github.com/aws-samples/build-multilingual-calendar-assistant-with-amazon-bedrock-and-aws-step-functions/blob/main/src/lambda/prompt_generator/generated_prompt_example.json) from the example message).
    -   Sometimes, the original message might not specify the exact date but instead says something like “please RSVP before this Friday,” implying the date based on the current context. Therefore, the function inserts the current date into the prompt to assist the model in interpreting the correct date for _this Friday_.
3.  Invoke the Bedrock FM to run the following tasks, as outlined in the prompt, and pass the output to the next step to the parser:
    -   Translate and summarize the original message in English.
    -   Extract events information such as subject, location, and time from the original message.
    -   Generate an action plan list for events. For now, the instruction only asks the FM to generate action plan for sending calendar reminder emails for attending an event.
4.  [Parse](https://github.com/aws-samples/build-multilingual-calendar-assistant-with-amazon-bedrock-and-aws-step-functions/blob/main/src/lambda/llm_output_parser/llm_output_parser.py) the FM output to ensure it has a valid schema. (Here’s the [parsed result](https://github.com/aws-samples/build-multilingual-calendar-assistant-with-amazon-bedrock-and-aws-step-functions/blob/main/src/lambda/llm_output_parser/parsed_result_example.json) of the sample message.)
    -   Anthropic Claude on Amazon Bedrock can [control the output format](https://docs.anthropic.com/claude/docs/control-output-format) and generate JSON, but it might still produce the result as “this is the json {…}.” To enhance robustness, we implement an output parser to ensure adherence to the schema, thereby strengthening this pipeline.
5.  Iterate through the action-plan list and perform step 6 for each item. Every action item follows the same schema:
6.  Choose the right tool to do the job:
    -   If the `tool_name` equals `create-calendar-reminder`, then run sub-flow A to send out a calendar reminder email using [Lambda Function](https://github.com/aws-samples/build-multilingual-calendar-assistant-with-amazon-bedrock-and-aws-step-functions/blob/main/src/lambda/send_calendar_reminder/send_calendar_reminder.py).
    -   For future support of other possible jobs, you can expand the prompt to create a different action plan (assign different values to `tool_name`), and run the appropriate action outlined in sub-flow B.
7.   Done.

## Prerequisites

To run this solution, you must have the following prerequisites:

-   An [AWS account](https://signin.aws.amazon.com/signup?request_type=register)
-   Enable model access to the Anthropic Claude 3 Sonnet model on Amazon Bedrock in the deployment AWS Region by following the steps in [Amazon Bedrock access setup](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access.html).
-   [Create and verify identities in Amazon SES](https://docs.aws.amazon.com/ses/latest/dg/creating-identities.html): If your Amazon SES is in sandbox mode, you must verify the email addresses of the sender and recipient.

## Deployment and testing

Thanks to [AWS Cloud Development Kit (AWS CDK)](https://aws.amazon.com/cdk/), you can deploy the full stack with a single command line by following the deployment instructions from the [Github repository](https://github.com/aws-samples/build-multilingual-calendar-assistant-with-amazon-bedrock-and-aws-step-functions). The deployment will output the API Gateway endpoint URL and an API key.

Use a tool such as curl to send messages in different languages to API Gateway for testing:

Within 1–2 minutes, email invitations should be sent to the recipient from your sender email address, as shown in Figure 2.

[![Figure 2: Email generated by the solution](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2024/06/25/ML-16234-Screenshot-Norsk.png)](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2024/06/25/ML-16234-Screenshot-Norsk.png)

Figure 2: Email generated by the solution

## Cleaning up

To avoid incurring future charges, delete the resources by running the following command in the root path of the source code:

`$ cdk destroy`

## Future extension of the solution

In the current implementation, the solution only sends out calendar reminder emails; the prompt only instructs the foundation model to generate action items where `tool_name` equals `create-calendar-reminder`. You can extend the solution to support more actions. For example, automatically send an email to the event originator and politely decline it if the event is in July (summer vacation for many):

1.  Modify the prompt instruction: If the event date is in July, create an action item and set the value of `tool_name` to `send-decline-mail`.
2.  Similar to the sub-flow A, create a new sub-flow C where `tool_name` matches `send-decline-mail`:
    1.  Invoke the Amazon Bedrock FM to generate email content explaining that you cannot attend the event because it’s in July (summer vacation).
    2.  Invoke a Lambda function to send out the decline email with the generated content.

In addition, you can experiment with different foundation models on Amazon Bedrock, such as [Meta Llma 3](https://aws.amazon.com/bedrock/llama/) or [Mistral AI](https://aws.amazon.com/bedrock/mistral/), for better performance or lower cost. You can also explore [Agents for Amazon Bedrock](https://aws.amazon.com/bedrock/agents/), which can orchestrate and run multistep tasks.

## Conclusion

In this post, we explored a solution pattern for using generative AI within a workflow. With the flexibility and capabilities offered by both Amazon Bedrock FMs and AWS Step Functions, you can build a powerful generative AI assistant in a few steps. This assistant can streamline processes, enhance productivity, and handle various tasks efficiently. You can easily modify or upgrade its capacity without being burdened by the operational overhead of managed services.

You can find the solution source code in the [Github repository](https://github.com/aws-samples/build-multilingual-calendar-assistant-with-amazon-bedrock-and-aws-step-functions) and deploy your own multilingual calendar assistant by following the deployment instructions.

Check out the following resources to learn more:

-   Visit [aws community](https://community.aws/generative-ai) to discover how our builder communities are using Amazon Bedrock in their solutions.
-   Learn more about [Generative AI on AWS](https://aws.amazon.com/generative-ai/)
-   Learn more about [Amazon Bedrock](https://aws.amazon.com/bedrock/)

___

### About the Author

[![](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2024/06/25/ML-16234-Author-lufng.jpeg)](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2024/06/25/ML-16234-Author-lufng.jpeg)[Feng Lu](https://www.linkedin.com/in/linkcd/) is a Senior Solutions Architect at AWS with 20 years professional experience. He is passionate about helping organizations to craft scalable, flexible, and resilient architectures that address their business challenges. Currently, his focus lies in leveraging Artificial Intelligence (AI) and Internet of Things (IoT) technologies to enhance the intelligence and efficiency of our physical environment.
