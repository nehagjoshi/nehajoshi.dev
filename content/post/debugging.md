+++
author = "Neha Joshi"
title = "Debugging : An under-rated superpower"
date = 2020-04-26T21:05:08+05:30
draft = false
description = "My thoughts about the art of debugging"
tags = [
    "Programming",
    "Debugging"
]
+++

Let us give a simple thought to this question:
>"How many times have you debugged some issue today?"

You would be surprised that the number will be quite large than what you had initially expected. Debugging is such an important skill to possess. 
#### May it be:
1. -The setup which is not working
2. -Unable to connect to Zoom meetings (this has become a new way of working in this quarantine period)
3. -VNC session not getting connected
4. -Bluetooth not getting connected to the speaker

And countless other experiences! Debugging is so important in not just a computer programmer's life, but even in general. When I think about my journey in the **"Debugging world"**, I vividly remember an experience from my early days of career as a programmer. I was working on this new project with my mentor and I was adding some Perl/C code for the first time. When I compiled the code and executed the program, I saw the most infamous "segfault" on my screen. I went in a state of panic. My Mentor noticed this and very calmly asked me the reason of my panic. My reply was that "my code has hit a segfault!". I was surprised at what came next :) My Mentor replied with the same calmness: "This is a very good sign. This means your code is getting executed! Now you are free to debug what is wrong with it :)". That was my first lesson in debugging a production code.
Since then, I have never forgotten this art and only kept polishing it, as and when possible. A programmer is an artist who can develop only with the right knowledge about the tools which he/she needs. Now you may write a C code by opening a simple notepad. But if you take the efforts to learn tmux, vim, cscope, your life will be 10 times easier. There is a very famous (and indeed true) quote by Bill Gates: **"I choose a lazy person to do a hard job. Because a lazy person will find an easy way to do it"**. We should not just focus on getting the job done. We should also think about how well we can do this job. Being with the CUDA developer tools team for the longest time in my career, I have come to highly appreciate the difference which the tools bring in the life of a programmer. The CUDA developer tools consists of debugger, profiler and memcheck.
I feel that a person's ability to debug is directly proportional to the level of curiosity and the fact that how self driven he/she is. Ironically enough, I remember spending more time on a working code, which I felt should not work, than the broken code which I expected to work.
In my 9+ years of career with NVIDIA, after working for ~6 years with the CUDA developer tools team, I switched to the Grid-SW team, which had completely new product/stack. I did not know anything about the working of this product. I was new to the virtualization world. The only known friend which I had was the C-Language and debugging skills. So if you really think about new teams, new companies, all that you are going to work on, is some piece of **code** and your **debugging skills**. That is the greatest strength which any computer programmer can possess.

On that note, cheers to all those wonderful programmers out there! Have a great one :)

~Your's truly,

A debugger!