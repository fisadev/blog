.. title: Customer and code quality
.. slug: customer-and-code-quality
.. date: 2017-09-17 16:28:30 UTC-03:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

I hear this a lot in the software development community: "the customer doesn't care about the quality of the code, just about features, usability, user experience, etc.".
It has many variations, but the idea remains the same: code quality isn't a factor in the decisions of the customer (internal or external).

Sometimes it's even used to justify writing, or not refactoring, bad code.
Because the main thing is to give your customer some value, and code quality just doesn't do that.

I currently believe this is false.
I believe that **customers do care** about the quality of the code, even if they don't understand what code is or how does it work. 

Cars
----

When buying a car, would you ask how are the wheels attached to the rest of the car? 
You probably wouldn't, you don't care about that.
But what if car X had wheels so special that each time you must change a tire, you need to spend 10.000$?
Suddenly you care about wheel attachment systems.
You would even feel cheated if someone sold you this car without warning you about its wheels.

Well, no, you don't care about wheel attachment systems specifically. 
But you don't want a car that costs lots of money each time it requires some maintainance.
You care about **maintainance costs**.

Software
--------

You probably get my point already.

Software, code, will need maintainance in most projects.
And we know that one of the major factors defining the time (money) required to do code maintainance, is how good it is.
Bad code costs more to maintain because it tends to have more bugs, to spawn new bugs more easily when modifying it, to be harder to read/understand how to change it, etc.

So the customer might have zero idea regarding what code is. 
But they still **care**, they want software with reasonable maintainance costs and that means code that is decent enough.
And giving them bad code without warning them of its impacts on maintainance, is a way of cheating them.

Finally, customers tend to be unable to see this relation by themselves, and contractors usually don't want to tell them "this is taking too long to modify because we gave you bad quality code".
This may be the reason we got the idea that customers don't care about code, developers just usually fail to explain (or even hide) to them the strong relation between code quality and maintainance costs.
So they never come back asking for "better code".
