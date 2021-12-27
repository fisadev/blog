.. title: The simplest Virtualenv tutorial (Python 3)
.. slug: virtualenvs-simplest-tutorial
.. date: 2019-04-15 19:51:00 UTC-03:00
.. tags: 
.. category: 
.. link: 
.. description: The absolute fundamentals on how to use python virtualenvs.
.. type: text

Python virtualenvs allow you to have isolated environments, in which you can install python libs and run your programs. This is useful when you have different projects with different requirements, and also to avoid installing python libs at system level.

This is how to use them in modern versions of python.

What do I need?
---------------

Python 3.3 or newer (older versions do have virtualenvs, but they are used in a slightly different way). 

How do I create a new virtualenv?
---------------------------------

Open a terminal, and run this:

.. code:: bash

    python3 -m venv PATH_TO_YOUR_VIRTUALENV


The path it receives as a paramenter, is the location for a folder that will be created and will contain your virtualenv inside. It will only contain the virtualenv, **don't** add any files inside that folder. Treat it like a "system" folder.

Example:

.. code:: bash

    python3 -m venv /home/fisa/projects/my_blog/venv


(If you are using Windows, the path would instead look something like this: "C:\Users\fisa\projects\my_blog\venv")


How do I use the virtualenv?
----------------------------

Each time you open a new terminal (console) to work in your project, you need to **activate** the virtualenv. The command to activate the virtualenv is different for Linux/MacOS vs Windows. 

On Linux and MacOS, run:

.. code:: bash

    source PATH_TO_YOUR_VIRTUALENV/bin/activate


(if you are using fish shell, replace "activate" with "activate.fish" at the end of that command)

On Windows:


.. code::

    PATH_TO_YOUR_VIRTUALENV\Scripts\activate.bat


From now on, the prompt of the terminal should say something like ``(venv)`` at the begining (with the name of your virtualenv). This indicates that you are working inside your virtualenv.

With your virtualenv activated, if you now install libs with pip (example: ``pip install pandas``), they will be installed inside the virtualenv. If you run a program inside that terminal, it will be able to import any libs installed in the virtualenv.


That's it?
----------

Yep.

Well, there's more to it, but this is what you need to start using virtualenvs :)


Ok, but...
----------

- ... how do I deactivate the virtualenv? Just close that terminal. Or run ``deactivate``.
- ... how do I delete the virtualenv? Just delete the folder. Nothing else is created anywhere else.
- ... can I move the virtualenv? No. Just delete it, and create a new one in the new location. Virtualenvs are disposable, don't get attached to them :) (your project should define its dependencies either in a ``requirements.txt`` or in a ``setup.py``, so you can easily install all the dependencies at once in the new environment)
