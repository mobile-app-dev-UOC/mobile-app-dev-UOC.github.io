---
layout: default
title: 10. Concurrency
nav_order: 10
has_children: true
---

# 10. Concurrency

Concurrent programming is a key concept in mobile application development. Let us start by reviewing some fundamental concurrent programming concepts. 

Mobile apps sometimes need to take care of several tasks that are independent between them. Executing those tasks **sequentially** (one after the other) might be inefficient if some of the tasks require waiting for user input, receiving data through the network or solving a computationally complex problem. Thus, it is best to define these tasks in a way that allows them to be executed **concurrently**: 

- If there are sufficient hardware resources (a CPU core for each task), they can be executed in parallel.
- If the hardware resources are insufficient, we can at least interleave the execution of these tasks (executing some instructions from one task and then switching to another one), making sure that idle tasks do not delay the others and that each task has the opportunity to progress and does not become stalled. 

Thus, even with a single processor, it makes sense to define concurrent tasks as it can greatly improve the performance of our app.   

The mechanism we use to support a concurrent execution is called **thread**. Each thread is a flow of execution of the code of our mobile app. In a mobile app there can be multiple threads of execution (**multithreading**). The number of threads used in an app does not depend on the number of cores of the mobile device where the app will run. Instead, the mobile operation system manages the hardware and assigns threads to cores, switching the current thread when necessary to achieve the best overall performance.

Every application has a primary thread that handles user interface management. This thread should never stall performing expensive input/output operations, or performing complex calculations. In order to do this optimally, we create other threads that operate in the background.

All threads share the properties of the classes they use but each thread has its own local variables.

> ![Concurrent threads in a mobile app.](/images/10/concurrency.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Concurrent threads in a mobile app.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

{:toc:}
