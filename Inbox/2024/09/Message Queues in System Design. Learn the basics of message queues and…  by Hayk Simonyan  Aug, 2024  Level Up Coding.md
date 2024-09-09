---
created: 2024-09-06T14:28:49 (UTC -03:00)
tags: []
source: https://levelup.gitconnected.com/message-queues-in-system-design-0440a1221023
author: Hayk Simonyan
---

# Message Queues in System Design. Learn the basics of message queues and… | by Hayk Simonyan | Aug, 2024 | Level Up Coding

> ## Excerpt
> Doing all of this immediately, especially during peak traffic times, could slow down the customer’s experience. Of course, we could scale up our servers to handle these large amounts of application…

---
[

![Hayk Simonyan](https://miro.medium.com/v2/resize:fill:88:88/1*shFuXtkBu9viAG261Fg2iA.png)



](https://hayk-simonyan.medium.com/?source=post_page-----0440a1221023--------------------------------)[

![Level Up Coding](https://miro.medium.com/v2/resize:fill:48:48/1*5D9oYBd58pyjMkV_5-zXXQ.jpeg)



](https://levelup.gitconnected.com/?source=post_page-----0440a1221023--------------------------------)

![](https://miro.medium.com/v2/resize:fit:700/1*QnEHTb57iUU8KPQ-gBzw6w.png)

Imagine an online store where every time a customer places an order, you need to:

1.  Process the payment.
2.  Update inventory.
3.  Send a confirmation email.

Doing all of this immediately, especially during peak traffic times, could slow down the customer’s experience.

In this case, we have a large number of application events, and we can’t handle all of them at once.

![](https://miro.medium.com/v2/resize:fit:700/1*5TbadXlJNU3ufSiTK0Wt7Q.png)

Of course, we could scale up our servers to handle these large amounts of application events, but if we don’t have to handle all of them at once, it’s better to queue these events and handle them later.

## Basic Architecture of a Message Queue

A message queue is a durable component stored in memory that supports asynchronous communication. It serves as a buffer and distributes asynchronous requests.

The basic architecture of a message queue is simple. Input services, called producers or publishers, create and publish messages to a message queue. Other services, called consumers or subscribers, connect to the queue and perform actions defined by the messages.

![](https://miro.medium.com/v2/resize:fit:700/1*MH0AbBSdZi5BgFFhX_5DpQ.png)

In a real-world scenario, there can be many apps writing to the queue and many servers reading from the queue.

![](https://miro.medium.com/v2/resize:fit:700/1*WeikGShMW69Dwxrn53ooqQ.png)

## Back to the example

So, in our case, instead of handling each task immediately, you can add it to the back of the queue, and from this queue, they are sent to our servers.

1.  **Order placed:** The order details are put into a message.
2.  **Message sent:** The message is added to the queue.
3.  **Workers process:** Separate processes (workers) pull messages from the front of the queue and handle the tasks.

![](https://miro.medium.com/v2/resize:fit:700/1*CsMA8OML1pbYE1SHqmDewA.png)

Also, our server acknowledges that it received and processed one message, and the queue removes it so that it is not sent for the second time.

![](https://miro.medium.com/v2/resize:fit:700/1*cAo5HW6rb1HeLflCtUUFBQ.png)

## Benefits of using Message Queues

The main advantage is that we **decouple** these events, and this message queue will allow us to process these events asynchronously. We can queue them until we can process them.

With the message queue, the producer can post a message to the queue when the consumer is unavailable to process it.

![](https://miro.medium.com/v2/resize:fit:700/1*JP9HOn98r9hZAkUWQH34rg.png)

Also, the consumer can read messages from the queue even when the producer is unavailable.

![](https://miro.medium.com/v2/resize:fit:700/1*1WWY_M5MdsNdPorpEatXZA.png)

Another great benefit is that they are **durable**. If the queue crashes, that data will not be lost as it’s not stored in RAM but in Disk.

![](https://miro.medium.com/v2/resize:fit:700/1*6jLcDI42TdCpCdWm1EXkGQ.png)

If a worker crashes while processing a message, no problem! The message is still in the queue and will be picked up by another worker.

![](https://miro.medium.com/v2/resize:fit:700/1*D1x0P9zMDjVxF_SvE92Cvw.png)

Message queues also provide **scalability**. If you receive a flood of orders, the queue will just get longer. You can add more workers to handle the extra load without affecting the website.

## Different Queue Types

There are multiple types of message queues. The most common ones are:

-   **FIFO (First-In-First-Out):** Just like a regular line, messages are processed in the order they arrive. This is important for things like payment processing.
-   **Priority Queues:** Some messages might be more important than others. You can prioritize these so they get processed sooner.

![](https://miro.medium.com/v2/resize:fit:700/1*rEo7STMHIpLSnePwK7JpcQ.png)

## Push vs Pull

Some queues wait for workers to ask for messages (**pull-based queue**), while others actively send messages to workers (**push-based queue**).

![](https://miro.medium.com/v2/resize:fit:700/1*o3Dz-ven854KSSZcY0Zh-A.png)

## Examples

Here are some popular examples of message queues:

![](https://miro.medium.com/v2/resize:fit:700/1*LiCdmn63OHwRlPJ_Go_dBg.png)

-   **RabbitMQ:** A versatile queue that’s good for many use cases.
-   **Kafka:** Built for high throughput and real-time data streaming. Great for things like logging and event-driven architectures.
-   **Amazon SQS (Simple Queue Service):** A fully managed cloud-based queue service offered by AWS. It’s scalable and reliable, with features like delay queues and dead-letter queues.
