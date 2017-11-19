.. title: Using Keras and TensorFlow with Nvidia gpus under Ubuntu
.. slug: using-keras-and-tensorflow-with-nvidia-gpus-under-ubuntu
.. date: 2017-11-19 09:55:28 UTC-03:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Using Keras and TensorFlow with Nvidia gpus under Ubuntu
========================================================

What are all these things???
----------------------------

* **Keras**: the Python library that knows how to build and train artificial neural networks.
* **TensorFlow**: the Python library that knows how to do heavy computations both under cpus and gpus, used by Keras.
* **CUDA + cuDNN**: Nvidia utilities to be able to run general purpose computations in the gpu.
* **Graphics drivers**: drivers that allow your linux to access and use the graphics card.


Graphics drivers
----------------

It might be possible to use CUDA without having the graphics drivers installed, but I'm not sure how easy and stable it is. 
So my recommendation is to install them first, and verify that they are working.

Usually it's just installing a package with ``apt``: 


.. code-block::

    ``sudo apt install nvidia-375``


But if you are using an Optimus-enabled graphics card (most laptops with Nvidia cards previous to the 10xx generation),
you might need to install the ``nvidia-prime`` package too.

The recommended version might be higher in the future, 375 is the one I'm using right now under Ubuntu 16.10.


CUDA installation
-----------------

Get both the CUDA installer and the cuDNN installer, from their official websites:
https://developer.nvidia.com/cuda-downloads and https://developer.nvidia.com/cudnn
(you will need to register in the website and fill a survey to be able to download cuDNN).

The versions you need to get depend on which versions does TensorFlow support. 
You can check this in the official website, at https://www.tensorflow.org/install/install_linux .

Once you have both installers, first run the cuda installer (replace the name of the file with the one you got): 


.. code-block::

    sudo sh ./cuda_8.0.61_375.26_linux.run --override


It will ask you a few things. Tell "no" to the installation of graphics drivers (you should already have them), 
and "yes" to the creation of the symbolic link.

Then uncompress the cuDNN installer (a file with a name similar to ``cudnn-8.0-linux-x64-v5.1.tgz``), 
and copy its files into the ``/usr/local/cuda-8.0`` folder (you should have it from the CUDA installation).
The tar file contains subfolders, be sure to copy the files into the same subfolders in the destination.


TensorFlow and Keras installation
---------------------------------

Once you have the graphics drivers and CUDA, then it's easy to install TensorFlow and Keras, they are just Python packages:

.. code-block::

    pip install tensorflow-gpu keras


If you aren't using virtualenvs (you should!), you should add ``--user`` to that command. 
I don't recommend installing the packages system-wide with ``sudo``, as with time you will probably need different versions of both 
``tensorflow`` and ``keras`` for different projects (they both evolve quite quickly).


Running the code
----------------


Finally, when running your code you may need to define the ``LD_LIBRARY_PATH`` environment variable, for TensorFlow to be able to 
find the needed CUDA libraries:


.. code-block::

    LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64 python your_awesome_code.py


This is true also for Jupyter notebooks:


.. code-block::

    LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64 jupyter notebook


If you are unsure if your code is actually using your gpu, you can paste this snippet into a ``test_device.py`` file:


.. code-block:: python

    import tensorflow as tf
    sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))


And then run it:


.. code-block::

    LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64 python test_device.py


It should print a lot of information, but in between you should see something with your graphics card name, like
``name: GeForce GTX 1070``.
