
# REFPROP for MATLAB

Note: As of REFPPROP 10, the only interface between REFPROP and MATLAB that is officially supported is via Python.  The previous interface, which still works in version 10, can be found in the legacy folder relative to this file.  No official support is provided for the legacy interface due to the challenges of maintaining the interface with various versions of MATLAB.  If you still want to use the old wrapper and you run into troubles, please open an issue here and hopefully another MATLAB user can help you: [New Issue](https://github.com/usnistgov/REFPROP-wrappers/issues/new).  Pull requests updating and fixing the legacy interface will be considered and are welcomed.

The Python wrapper is written and maintained by Ian Bell.  For questions, bugs, or issues, please open an issue in this repository on github: https://github.com/usnistgov/REFPROP-wrappers/issues/new

## Setup

You will need to acquire a Python interpreter, the easiest method to do so if you do not already have Python installed on your computer is to download the installer from python.org: [link to downloads](https://www.python.org/downloads/).  There is also information on [the Mathworks website](https://www.mathworks.com/help/matlab/matlab_external/install-supported-python-implementation.html).  The Python wrapper of REFPROP only uses methods that are in the Python standard library, so the standard installation of Python would work fine.  If you would like to have a more full-featured Python installation that will work with the MATLAB interface of REFPROP, you can install a full-fledged Python installation from Anaconda: [Anaconda download](https://www.anaconda.com/download/).

In your MATLAB shell, you can inquire about what Python version MATLAB intends to use with a command like:

```
>> pyversion

       version: '2.7'
    executable: 'D:\Anaconda\python.exe'
       library: 'D:\Anaconda\python27.dll'
          home: 'D:\Anaconda'
      isloaded: 0
```
Good.  It found Python, and has not loaded it yet.  You are ready!

If you have multiple copies of Python on your computer already, then you can tell MATLAB which one you want it to use by passing the absolute path to the python executable to ``pyversion``.  For instance:

```

>> pyversion d:\Anaconda\envs\py36\python.exe
>> pyversion

       version: '3.6'
    executable: 'd:\Anaconda\envs\py36\python.exe'
       library: 'd:\Anaconda\envs\py36\python36.dll'
          home: 'd:\Anaconda\envs\py36'
      isloaded: 0
```
Finally, you need to install the ctREFPROP package (a ``ctypes``-based interface to REFPROP) into your given copy of python.  This one-liner calls the ``pip`` program of Python to install the ctREFPROP package from the PYPI package index.  Watch out for the spaces in the arguments, they are important!:
``` MATLAB
[v,e] = pyversion; system([e,' -m',' pip',' install',' --user',' -U',' ctREFPROP'])
```

## Use

At the beginning of your code you should add the import statement (similar to what you would do in Python):
```
import py.ctREFPROP.ctREFPROP.REFPROPFunctionLibrary
```
then to initialize REFPROP, you tell it what the root path of your REFPROP installation is:
```
RP = REFPROPFunctionLibrary('C:\Program Files (x86)\REFPROP')
```
and to confirm that everything is working correctly, let's print out the version of REFPROP that you have loaded:
```
>> RP.RPVersion()

ans = 

  Python str with no properties.

    10.0
```
As the first practical example of the use of the MATLAB interface, let's calculate the normal boiling point temperature of water at one atmosphere (1 bar = 101.325 kPa). We will revisit the other arguments soon

```
>> r = RP.REFPROPdll('Water','PQ','T',int8(0),int8(0),int8(0),101.325,0.0,{1.0})

ans = 

  Python REFPROPdlloutput with properties:

    Output
    hUnits
    herr
    iUCode
    ierr
    q
    x
    x3
    y
    z
```
Multiple outputs are returned in a data structure ([see the documentation for the REFPROPdll function](http://refprop-docs.readthedocs.io/en/latest/DLL/high_level.html#f/_/REFPROPdll)), and you can obtain the first output by indexing the ``Output`` list.  This interface calls directly into the DLL (via Python), so the documentation is exactly what you need to know for MATLAB too.

The normal boiling point temperature is then obtained from the Output:
```
>> r.Output(1)

ans = 

  Python list with no properties.

    [373.1242958476959]
```

Success!

## Additional reading

Mini-tutorial available in Python as a jupyter notebook: [Link to tutorial](https://nbviewer.jupyter.org/github/usnistgov/REFPROP-wrappers/blob/master/wrappers/python/notebooks/Tutorial.ipynb) .  The notebook can be accessed at [wrappers/python/notebooks](https://github.com/usnistgov/REFPROP-wrappers/tree/master/wrappers/python/notebooks)