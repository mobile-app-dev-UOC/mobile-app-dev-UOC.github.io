---
layout: default
title: 6.1. Debugger
parent: 6. Debugging and testing
nav_order: 1
---

# 6.1. Debugger 

The debug system is one of the most important components of a development environment. Its main mission is to allow developers to run the code of our application line by line, being able to monitor the value of variables at all times.

To use a debugger, first we select in the environment the emulator or physical device where we are going to use the debugger. Then, we click the debugger button.

> ![Launching the debugger in Android Studio.](/images/06/debugger.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Launching the debugger in Android Studio.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We can tap on the grey strip to the left of the source code to enter breakpoints. A **breakpoint** is a location where we want the execution to stop because at that time we want to start a step-by-step execution or because we want to inspect the value of a particular variable. 

Running the debugger stops the execution at the first breakpoint and marks it with a blue line.

> ![Execution stops because a breakpoint has been reached.](/images/06/breakpoint.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Execution stops because a breakpoint has been reached.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then, we can add variables to our watchlist so that we can inspect their value dynamically. 

For example, we add the variable `u` which will initially be undefined. When we ask the debugger to step forward (execute the next line of code), an object of class User class is created and assigned to variable `u`, which then becomes defined. In the “Variables” area of the debugger, there is also a `this` variable that points to the class within which we are located. Using this variable, we can check the value of our class properties.

> ![Examining the values of variables using the debugger.](/images/06/debugger-variables.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Examining the values of variables using the debugger.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We can also display complex objects to see the value of their properties. For instance, in our example the `name` and `age` properties are filled in correctly.

Each time we step forward, we can decide whether we want to execute the next line of the current method (*Step Over*) or whether we want to enter inside the method executed by the next statement (*Step Into*).

In the bottom area there is a `Frames` section showing us the current execution stack.  All methods currently being executed are organized as a stack, with the innermost method at the top. Calling a new method adds it to the stack, while ending a method pops it from the top of the stack.

> ![Inspecting the execution stack.](/images/06/frames.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Inspecting the execution stack.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We are going to enter the `u.CalculateRisk()` method by pressing the `Step Into` button as seen in the image below. 

> ![Stepping into a method.](/images/06/step-into.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Stepping into a method.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

After entering the method `CalculateRisk`, the `Frames` section now displays that we are inside the method.

> ![The execution stack becomes updated after entering a method. ](/images/06/updated-frames.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *The execution stack becomes updated after entering a method.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

In the `Variables` section, we see that now variable `u` is not of type `User`, of type Int (the name `u` is reused as a local variable of the method. Moreover, `this` now points to the object of type User where we are executing the method `CalculateRisk`.

Clicking on a previous row of the `Frames` section (see image below) retrieves the position within the source code and the values of the available variables in that method. 

> ![Inspecting the code and variables in the methods that called the current method.](/images/06/back-frame.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Inspecting the code and variables in the methods that called the current method.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

If we want to continue the execution without going line by line, click on the `Play` button (see image below). At any time we can stop the execution by pressing the `Pause` button. If we stop the application randomly, we may find ourselves in the middle of executing code from a system library (rather than the code of our app). If this happens, we can always tap the `Play` button again to continue executing the app normally. 

> ![Play and Pause buttons in the debugger.](/images/06/pause-play.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Play and Pause buttons in the debugger.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Pressing the `Stop` button stops the app and ends the debug session.

One of the main goals of debug sessions is to check the code around breakpoints. We can view all active breakpoints by clicking on the button that gives us access to the breakpoint window. From this window, we can enable, disable or delete them.


> ![Inspecting the list of active breakpoints.](/images/06/active-breakpoints.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Inspecting the list of active breakpoints.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

