.. title: Separate IO from algorithms
.. slug: separate-io-from-algorithms
.. date: 2022-02-14 17:55:50 UTC-03:00
.. tags: python
.. category: 
.. link: 
.. description: 
.. type: text

This is an old post I wrote for the Machinalis blog. Machinalis was a company I worked at some years ago, that later on got acquired by Mercado Libre. The old post is no longer online, so I replicated it here to keep it somewhere on the web :)

Separate IO from algorithms
===========================

Being able to write clean, easy to maintain code is one of the most important skills a developer should have. And it isn’t an easy task to accomplish. We will often be presented with complex problems in which there is no clear “clean” solution. But at the same time, there are some simple practices that can help a lot in the path to better code.

In this post we will talk about one of those practices: separating IO code from algorithms. It’s not rocket science, and many will probably find this obvious. But experience shows that it’s something too often overlooked, and when that happens, the code tends to become messy quite fast.

A not-so-real example
---------------------

Let’s start with a hypothetical task (later on we will look at a more real example). It will be something quite simple, but bear in mind I’m using it as a vehicle to present some ideas. In real life I would probably just use ``collections.Counter`` and the csv module :)

Imagine we have a .csv file, in which each line has the name of a developer and the language he uses:

.. code-block::

    Guido Van Rossum,Python
    Dennis Ritchie,C 
    Armin Ronacher,Python 
    Larry Wall,Perl 
    ...


And we are asked to develop a small program that counts how many developers each language has. It must produce these results via standard output:


.. code-block::


    Python: 2
    Perl: 1
    C: 1
    ...


The code we would write to solve the task could be something like this:


.. code-block:: python

    def count_developers(file_path):
        quantities = {}
        with open(file_path) as developers_file:
            for line in developers_file:
                developer, language = line.strip().split(',')
                if language not in quantities:
                    quantities[language] = 0
                quantities[language] += 1

        for language, quantity in quantities.items():
            print('{l}: {q}'.format(l=language, q=quantity))


And it works, it gets the job done. Even more: it looks like simple code, clean code.

But it has some not-so-obvious problems:

- What if we want to write tests for it? That would be a problem: the tests would either have to create a file to use as input, and capture the standard output to check the results, or use lots of complex mocking to avoid the interaction with real files and real standard output.
- What if at some point we need to count developers from a different source, like a json API response? We would need to create a .csv file just to be able to feed it into this function, even if our input data doesn’t come in a file.
- What if we need to use the output in a different way instead of showing it to the user via standard output? This function forces the results to be shown in that particular way.

All these issues have the same root: our code is doing two things at the same time, that should be separated. Our program is dealing with the IO logic (reading files, showing results) and the algorithms itself (the “business logic”) in a single piece of code.

In this simple example it would be trivial to refactor the code to solve any of those issues. But that kind of refactors (changes to the input and output formats of a piece of business logic) tends not to be so trivial in real life code.

A better approach is then to follow that simple rule we mentioned in the beginning: to separate IO code from algorithms. Following that rule, our solution would look more like this:

.. code-block:: python

    def read_developers_file(file_path):
        with open(file_path) as developers_file:
            return [line.strip().split(',')
                    for line in developers_file]

    def count_developers(developers):
        quantities = {}
        for developer, language in developers:
            if language not in quantities:
                quantities[language] = 0
            quantities[language] += 1
        return quantities

    def show_report(quantities):
        for language, quantity in quantities.items():
            print('{l}: {q}'.format(l=language, q=quantity))


In this new solution, we clearly divided our code in three blocks: the code dealing with the input file, the counting algorithm itself (business logic), and the code dealing with the output of the results. We can easily test the business logic without mocking or doing real IO. We can easily reuse the business logic in scenarios where the input or output formats are different. Even if we have to support input data coming from a stream, something quite difficult with the previous approach, we could achieve that with simple refactors. This separation leaves the door open for changes in a way the old code didn’t.

A real example
--------------

A very common scenario in which this rule is neglected, leading to really ugly code, slow and complex tests, and overall difficult to maintain code, are Django views. Developers too often write much of the business logic of their web apps right into the views. At first sight this doesn’t look “that bad”, the code is clean, simple. It’s just a view doing business stuff. But as we saw before, problems start to arise when we need to write tests, or reuse that business logic in slightly different scenarios.

When writing the tests, people usually just rely on the ``django.test.client`` to solve the “I need to do IO to test this logic” issue. The test client is great, it really solves the need of having to test a view. But the problem is: we shouldn’t be testing a view, when we just need to test a piece of business logic. We are doing lots of unnecessary extra work (url resolving, middlewares, etc), and complicating the test code, when it could have been just a function call.

And as you can imagine, things get really messy when we need to reuse that business logic that’s buried inside the view.

So, instead of writing views like this:

.. code-block:: python

    def update_score(request, username):
        # logic to get the current score
        # logic to get the matches won
        # score = a little extra code calculating the new score
        # some more score updating
        # the last bits of the score update
        returnrender(request, 'score.html', {'score': score})


We should always try to write views more similar to this:

.. code-block:: python

    import score_logic

    def update_score(request, username):
        score = score_logic.update_score(username)
        returnrender(request, 'score.html', {'score': score})


Conclusion
----------

Separating IO from algorithms might sound like an obvious advice, but it isn’t, it’s a principle that is often overlooked. And specially in web apps, leading to test suites that take too much time to run, and code that is indeed very hard to maintain.

It’s a simple rule, easy to follow, and it does prevent serious maintainability problems. So this is my advice: never again miss a chance to separate that function (or view) into dedicated IO and algorithms blocks. Your future self will be thankful :)
