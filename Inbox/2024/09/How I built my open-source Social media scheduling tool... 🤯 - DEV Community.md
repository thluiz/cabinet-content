---
created: 2024-09-04T20:32:47 (UTC -03:00)
tags: [webdev,javascript,programming,tutorial,software,coding,development,engineering,inclusive,community]
source: https://dev.to/nevodavid/how-i-built-my-open-source-social-media-scheduling-tool-dih?context=digest
author: 
---

# How I built my open-source Social media scheduling tool... 🤯 - DEV Community

> ## Excerpt
> I published Postiz, my open-source social media scheduling tool, on Reddit, and received much...

---
I published [Postiz](https://postiz.com/), my open-source [social media scheduling tool](https://github.com/gitroomhq/postiz-app), on [Reddit](https://www.reddit.com/r/selfhosted/comments/1f4x806/postiz_opensource_social_media_scheduling_tool), and received much attention.

I guess it was super needed in open-source.  
I have received multiple questions from developers on how I built it.

So today, I will take you through the infrastructure and things you need to consider.

[![Social Media](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Flwjkbemvdketw9gx0zb8.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Flwjkbemvdketw9gx0zb8.png)

___

## [](https://dev.to/nevodavid/how-i-built-my-open-source-social-media-scheduling-tool-dih?context=digest#it-all-starts-with-oauth-oauth2)It all starts with oAuth (oAuth2)

The core of every social media scheduling tool is oAuth. Let me explain.

oAuth is a standard way for different platforms to give you access to their users to perform a limited-scope action.

For example, you can ask Facebook to give you access to post on a user timeline or read their analytics.

oAuth is limited by time but can be refreshed, and the core of every scheduling tool is:

1.  Collect oAuths of different platforms.
2.  Refresh them when needed.
3.  Use them to post on a user timeline.

Since you want to build a generic platform that you can easily add more and more social platforms, you want to build an interface like this (simplified version):  

```
<span>export</span> <span>interface</span> <span>Provider</span> <span>{</span>
  <span>generateAuthUrl</span><span>():</span> <span>Promise</span><span>&lt;</span><span>GenerateAuthUrlResponse</span><span>&gt;</span><span>;</span>
  <span>authenticate</span><span>(</span><span>params</span><span>:</span> <span>{</span><span>code</span><span>:</span> <span>string</span><span>;</span> <span>codeVerifier</span><span>:</span> <span>string</span><span>;</span> <span>refresh</span><span>?:</span> <span>string</span><span>;}):</span> <span>Promise</span><span>&lt;</span><span>AuthTokenDetails</span><span>&gt;</span><span>;</span>
  <span>refreshToken</span><span>(</span><span>refreshToken</span><span>:</span> <span>string</span><span>):</span> <span>Promise</span><span>&lt;</span><span>AuthTokenDetails</span><span>&gt;</span><span>;</span>
<span>post</span><span>(</span><span>token</span><span>:</span> <span>string</span><span>,</span> <span>info</span><span>:</span> <span>MessageInformation</span><span>):</span> <span>Promise</span><span>&lt;</span><span>PostDetails</span><span>&gt;</span><span>;</span>
<span>}</span>
```

`generateAuthUrl` — This function generates a URL for the user to authenticate on a platform (like LinkedIn). It gives you a code you can later convert to a token to post on a user's timeline.

`authenticate` - Takes the code of the user, converts it to a token, and creates the user in the database (and saves their token along with the token expiry date)

`refreshToken` - Takes the user token and expiry date and refreshes it if needed.

`post`— This function takes the user token and message details and posts them to the user's timeline (it can be a picture, text, video, etc.).

Once you have that, you must catch a user's POST request in an authentication request, map it to the right provider that implements IAuthenticator, and return the `generateAuthUrl.`

The `generateAuthUrl` gets a "return URL," which the user will return once they have completed authentication. In that URL, you use the `authenticate` function.

___

## [](https://dev.to/nevodavid/how-i-built-my-open-source-social-media-scheduling-tool-dih?context=digest#scheduling-the-posting)Scheduling the posting

Platforms mostly will not give you a way to schedule their posts with an API.

Even if they did, it would be hard to manage some who do and some who don't, so it's better to view it as a platform for those who don't.

To do this, you need to implement a queue.

**Example**: Post to Facebook: "I love Postiz" and process it in 1 week from now.

You have many options for queues.  
Some of the most popular are [RabbitMQ](https://www.rabbitmq.com/), [SQS](https://aws.amazon.com/sqs), and [ActiveMQ](https://activemq.apache.org/)

They are perfect when you have tons of jobs, and if you have streaming Petabytes of data, [Kafka](https://kafka.apache.org/) can probably help.

But in my case, I don't need a "real" queue.

So, I used Redis (Pub-sub). You push some key inside of Redis and add an expiry date. Once the expiry date passes, it triggers Redis and sends a notification (to some service)

There is a great company called [BullMQ](https://bullmq.io/) that simplifies the process with Redis and your app (it's free and fully open-sourced)

[![Archtecture](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F34dsfux8q48t4yb7c2a8.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F34dsfux8q48t4yb7c2a8.png)

Below are "Workers". They are microservices of your application that process and post the "job" on social media.

They are separated because they are smaller than the main application and might need to be scaled depending on your number of jobs.

We call this "Horizontal scaling," as we will need to add more workers as our application grows.

___

## [](https://dev.to/nevodavid/how-i-built-my-open-source-social-media-scheduling-tool-dih?context=digest#the-db)The DB

Usually, you would need to save a post in the DB with multiple statuses—`DRAFT,` `SCHEDULED,` `COMPLETED,` and `ERROR`—so you can track what happened to it.

In my case, I used [Prisma](https://www.prisma.io/).  
Prisma is an ORM that wraps and queries your database using terms like One-to-one, Many-to-many, and one-to-many.

It's a known approach that exists for you in all the languages.  
I like it because it keeps a consistent response structure and allows me to change to a different DB in the future (easy migration). I don't have even one raw query in my project (good or bad)

I am using PostgreSQL because it's trending now (according to a [StackOverflow survey](https://survey.stackoverflow.co/2024/technology#1-databases))

[![StackOverflow survey](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgccweu9mqautwij4b0co.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgccweu9mqautwij4b0co.png)

The main thing to be careful about when using SQL is deadlocks. You don't want to update a row from multiple places simultaneously, as this will throw a deadlock, and the update will fail.

___

## [](https://dev.to/nevodavid/how-i-built-my-open-source-social-media-scheduling-tool-dih?context=digest#help-me-out)Help me out

[Postiz](https://github.com/gitroomhq/postiz-app) is trending on GitHub. It's 100% free, and I will try to give you value as much as possible.

Already crafted a small [roadmap](https://github.com/gitroomhq/postiz-app/discussions/185) from all the requests.

I was hoping you could help me with a star that will help me (well, I can't even describe how much)

[Star Postiz](https://github.com/gitroomhq/postiz-app)
