.. title: Encrypt a dir with Ecryptfs
.. slug: encrypt-a-dir-with-ecryptfs
.. date: 2020-03-11 20:30:00 UTC-03:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

This is a simple tutorial on how to have an encrypted directory in your Linux, by using Ecryptfs.

There are many alternatives to do the same, and I'm not qualified to do a deep comparison of them.
But I've been using this solution for many years already, and it works like a charm. 
It relies on command line tools only which are easy to automate. 
And because it uses a mounted filesystem, it's pretty transparent to other tools (like backups, 
editors, etc). 
There's absolutely no need for specialized tools to work with your encrypted files.


Dependencies
============

.. code:: bash

    sudo apt install ecryptfs-utils


Initial setup (one time only)
=============================

You need to do this only once to setup your dir. 
After that you won't have to run these steps in your daily use.

First create two dirs:

- A dir where you will work with your files when the encrypted filesystem is mounted.
- A dir where the encrypted data will live. You should **never** edit its contents by yourself.


.. code:: bash

    mkdir super_secret_things
    mkdir super_secret_things_encrypted
 

Then, mount the filesystem for the first time:


.. code:: bash

    sudo mount super_secret_things_encrypted super_secret_things -t ecryptfs


This will ask you a bunch of questions, these are the answers I recommend, plus some explanations:

- **Passphrase**: whatever you want to use as password :)
- **Cipher**: aes (option 1. This is the algorithm used to encrypt. Aes is pretty good)
- **Key bytes**: 32 (option 2. The size of the key, bigger usually means harder to hack)
- **Plaintext passthrough**: no (this allows for non-encrypted files to be used along encrypted ones, 
  which I think is a bad idea. These are your super secret stuff, you need to be sure they are 
  always encrypted).
- **Filename encryption**: yes (fairly obvious. If the answer is no, people can see the names of the 
  files without having to know the encryption password)
- **Signature confirmation**: just press enter.
- A confirmation asking whether you are sure you typed everything right, because this is the first
  time ecryptfs sees you using this combination of parameters with this directory. Just say yes, 
  because doh, of course it's the first time.
- And Finally, whether you want ecryptfs to remember the answer to the previous question, so it 
  doesn't ask you "are you sure? this is the first time..." every time you mount the encrypted dir.
  Answer yes.

**You will need to remember the answers** you chose, to be able to re-mount the encrypted dir in 
the future! For me this became muscle memory: "1 2 n y enter".

And that's it! Encrypted dir created!

Daily use
=========

Whenever you need to work with your encrypted files, the steps are this:

1. Mount the encrypted dir 

.. code:: bash

    sudo mount super_secret_things_encrypted super_secret_things -t ecryptfs
    # answer "1 2 n y enter", or whatever you chose instead


2. Work with your files inside `super_secret_things` (REMEMBER!!! never edit the contents of `super_secret_things_encrypted` by yourself)
3. Un-mount the encrypted dir 

.. code:: bash

    sudo umount super_secret_things


Of course, you can automate these into scripts, alias in your shell, etc.

Hope this is as useful to you as it was for me :)
