.. raw:: html

    <style> .red {color:red} </style>
    <style> .blue {color:blue} </style>
    <style> .green {color:green} </style>
    <style> .cyan {color:cyan} </style>
    <style> .magenta {color:magenta} </style>
    <style> .orange {color:orange} </style>
    <style> .brown {color:brown} </style>
    
.. role:: red
.. role:: blue
.. role:: green
.. role:: cyan
.. role:: magenta
.. role:: orange
.. role:: brown

.. _mdf:

MDF
===

This class acts as a proxy for the MDF3 and MDF4 classes. 
All attribute access is delegated to the underlying *_file* attribute (MDF3 or MDF4 object). 
See MDF3 and MDF4 for available extra methods.

An empty MDF file is created if the *name* argument is not provided. 
If the *name* argument is provided then the file must exist in the filesystem, otherwise an exception is raised.

Best practice is to use the MDF as a context manager. This way all resources are released correctly in case of exceptions.

.. code::

    with MDF(r'test.mdf') as mdf_file:
        # do something
        

.. autoclass:: asammdf.mdf.MDF
    :members:
    
MDF3 and MDF4 classes
---------------------

.. toctree::
   :maxdepth: 1
   
   mdf3
   mdf4

Notes about *load_measured_data* argument
-----------------------------------------

By default when the *MDF* object is created the raw channel data is loaded into RAM. 
This will give you the best performance from *asammdf*. 

However if you reach the physical memmory limit *asammdf* gives you the option use the *load_measured_data* flag. 
In this case the raw channel data is not read. 


*MDF* defaults 
^^^^^^^^^^^^^^

Advantages

* best performance
    
Disadvantages

* higher RAM usage, there is the chance the file will exceed available RAM
    
Use case 

* when data fits inside the system RAM
  
  
*MDF* with *load_measured_data*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Advantages

* lowest RAM usage  
* can handle files that do not fit in the available physical memory
    
Disadvantages

* slow performance for getting channel data
* must call *close* method to release the temporary file used in case of appending.

.. note::

    it is advised to use the MDF context manager in this case
 
Use case 

* when *default* data exceeds available RAM
* it is advised to avoid getting individual channels when using this ioption.

Instead you can get performance close to load_measured_data=True if you use
the *select* method with the list of target channels.

.. note::

    See benchmarks for the effects of using the flag