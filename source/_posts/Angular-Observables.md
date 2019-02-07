---
title: Angular Observables
tags: [Angular, Observable]
---

### Overview ###

[Observables in Angular](https://angular.io/guide/observables) are based on promises, which if you are not familiar with those we will give a quick break down.

Imagine you are at work and since your friend is getting coffee you ask him to get you some too and he agrees. While he is doing that you are going to continue working on your project since you have a hard deadline, but after a few minutes the boss calls you into her office to talk over a couple of details. He gets back with the coffee, sees that you aren't there, and so he leaves it at your desk for you which you had asked him to do.

Let's break down what happened, you asked for coffee (request) then had some other stuff to keep doing (making it asynchronous or async), and your buddy left the coffee on your desk for you (response). Neither of you lost time because you both kept doing everything you needed. That is the beauty of it -- no time wasted waiting. Imagine if your boss called you in and your response was "yeah, but wait until I have my coffee back".

The difference between an observable and promise is the observable has to be subscribed to, and it will begin publishing data once it has some and then will tell you once it is done.

#### The Good

Since the observable will begin publishing data once you have it you need an object called an **observer**. The observer is sitting there waiting for some information and is a watchful eye. If we go back to our coffee example used for promises, this is your assistant who immediately tells you when your coffee is there. So you tell your assistant what to do with your coffee when it is there, and then you can also choose to tell your assistant what to do if there is an error with your coffee, and also optionally what to do when your coffee request is complete.

In oberserver talk, these would be the three methods 

```
const theOberserver = {
    next: performThisOnEveryValueReceived(), 
    error: doThisIfWeReceiveAnErrorBack(), 
    complete: finallyDoThisToTellMeYouAreDone() };
```

The next is required, but error and complete are optional and these can be anonymous or named functions.
One quirk to note is that even after a complete() signal comes through to observer you can still receive more data and the observer will continue to fire the next() on those.

#### The Bad

One of the things that can be tough to get past is that the observable is subscribed to, and then you pass the observer to the observable. Let's go back to our coffee example

```
coffeeObservable.subscribe( myAssistantObserver );
```

or you can alternatively pass through anonymous functions through

```
coffeeObservable.subscribe(
    data => textMe(data),
    error => {
        console.log(error);
        return goMakeMeCoffee();
    },
    () => return true
);
```

The key to understanding why you subscribe to the observable is that you can have multiple observers subscribed to a single observable. Think of it like donuts getting put in the breakroom, everyone is going to want to know when that happens.

```
donutsInBreakRoom.subscribe(bossAssistant);
donutsInBreakRoom.subscribe(myObserver);
donutsInBreakRoom.subscribe(buddy);
```

So now everyone knows about the donuts in the breakroom, but behind the scenes it is actually telling each person separately about the donuts in the break room. This is great because it ensures that each observer get the exact same message.

Let's say though that the person who brought in donuts just wants to yell once to the whole office that there are donuts, this would be more akin to **multicasting**, since some people might hear the whole announcement while others might just get part of the message. We will deep dive into this in a different blog. So even though this looks like a bad, once it is understood what is going on it is actually pretty nice.

#### Recap

Observables are similar to a promise in that they can be delivered async, and they must be subscribed to so the message can be observed. You can create an observer object and pass that through or pass through anonymous functions to the `subscribe()` method subscribe that to the observerable and then when information is received it will perform the `next()`, `error()`<sub>1</sub>, `complete()`<sub>1</sub>. You can have multiple observes subscribed to an observable so they are all looking out for that data.

Let me know if you find any errors or confusing points in the information above, and keep coding.

---
 <sub>1 - if passed through</sub>