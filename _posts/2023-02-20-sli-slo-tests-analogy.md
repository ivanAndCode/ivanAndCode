---
title:  "Observability: SLIs and alerts - parallels with tests"
layout: post
excerpt_separator: <!--more-->
---

![Observability](../assets/posts/2023_02_20_o11y.jpg)  
Photo by <a href="https://unsplash.com/it/@mvdheuvel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Maarten van den Heuvel</a> on <a href="https://unsplash.com/photos/s9XMNEm-M9c?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## Context
Today I've attended the "brown bag" session about observability aka o11y.  
At first, the [speaker](https://www.linkedin.com/in/simon-w-0b7a201a/) (hi, Simon!) was talking about alerts. Do they provide good insight into what is going on with the system? Are they enough?  

For example, a situation when you did `run of the disk space` on the instance that runs your host.  
Do you need to alert somebody about it? Yes! Is it enough? No! 😅

Why though? 🤔
<!--more-->
Can we not create all sorts of alerts for our infrastructure metrics + alerts on the logs of our system? We can!   

## What to do?
Now imagine the engineer on call receives an alert - `low disk space`, what should they do? 
- drop everything and attempt to fix it
- raise an incident
- do nothing and wait
- ...something else?

This choice is hard in itself, and different people would choose different answer.  
Now, multiply this by `X alert` and `Y services` and you'll see how it snowballs out of control.

What do we do then?

## SLIs
To answer this question, let's talk about another thing - `service level indicators` (SLIs)
> SLI is a defined quantitative measure of some aspect of the system

Typical SLI:
 - request latency
 - error rate
 - throughput
 - etc.

If you have this data, whether it's a dashboard, or alert, or both, you would be able to correlate `low level signal` (aka out of disk space) to the `impact on system behaviour` (these specific endpoints suddenly started to be slow, error rates are growing)  

Still, where does the tests of different types come to the picture?

## Tests are observability too?
Not in a traditional sense, but they have some aspects of it. When they run, they provide you data about your system.

### Unit test
Let's look at the unit test that checks `validateUser(user)` function
> validateUser() function  
> should return true  
> when called with valid user

⬆ if this test fails, how much data do you get? You will know exactly `what` is wrong, but not `why is it a problem?`.

### API test
Imagine we had another test that checks the endpoint `POST baseUrl/addUser` works well
> Calls to POST baseUrl/addUser  
> When called with valid payload (user)  
> Should return 200 OK

This gives you more of a `system behaviour` vibe. Let's go up a level and look at the UI test!

### UI test
> When user visits /register page  
> And enters valid user details  
> Then new user account is created   
> And user is redirected to the /index page

This test is describing a behaviour from the user perspective.

### Which tests do I need?
You need all of them. They work well together and provide different perspectives on the system under test.  
Same like for `low-level alert` and `SLI`, you need to have both to make an assessment of the impact and choose the right course of action. 

Unit test tells you `exctly what is wrong`, UI-level test will tell you `why is it a problem to your user`.

## Summary
Write tests!  
Think about observability!   

This all should be part of your `quality strategy`.  
And don't forget to pay attention to [test pyramid](https://ikaraman.github.io/ivanAndCode/test-pyramid/) too! 😉  