---
description: Main Page for the Rincon SDRCraft Project
---

# SDRCraft

## Getting Started

Starting the project is fairly simple as long as you have access to a terminal and python version 3. We recommend setting up an environment, shown below. If you already have an environment, skip to step \[Running the Application\]\(\#running-the-application\)

```
$ cd <location of project>
$ virtualenv -p python3 .venv
$ source .venv/bin/activate
$ pip3 install -r ./requirements.txt
```

{% hint style="info" %}
Dependencies are always being added, so if you find a dependency that does not exist, notify us via the issue tracker in github.
{% endhint %}

## Running the Application

It's as easy as running main.py in the application. Soon, we will export everything to a desktop icon / executable script, however, in development, a user must have python capability.

```bash
$ python3 main.py
```

## Folder Structure and Project Setup

The project is set up in one simple to use python module with individual aspects of the project stored inside the sdrcraft folder. Modules are stored in sdrcraft/blocks and resources \(ui files and images\) are located in sdrcraft/resources.

## Creating a New Module:

You can create a module by defining your transfer / source / sink function as a python function, or by a class. Whichever is easiest to you the developer. Below are detailed instructions for both.

### Using a Function

First, create a folder inside the directory project/sdrcraft/blocks. This folder can be named whatever you want and changing the name wont do anything. All of the following four files will live in that folder

A Module consists of 4 files. Here is an example for adding a node called GenericModuleFunc

* \_\_module\_\_ 

In order for the program to recognize that this directory is a module, simply make a new file called \_\_module\_\_. Nothing needs to be in this file, it just needs to exist.

* GenericModuleFunc.json 

This is the configuration file.

It needs four fields:

* Name - The name that appears inside the dropdown menu
* Module - The name of the python file \(if we name our python file GenericModule.py, this is GenericModule\)
* Type - The type of block which can be "Transition" "Sink" or "Source"
* Function - The function inside the python file \(if our class inside GenericModule.py is called GModule, this is GModule\)
* **Parameters** - All the editable parameters. This can be either str, float, int or dropdown \(dropdown is under development, refrain from this for now\).

**Parameters** are chosen by the user when they click on edit. The name of the parameter is the label and the user can enter the data type into that field in the editor window. When the user clicks ok, it returns a python dict of the parameters the user entered into a new module object

* FunctionParameters - Mapping of the Parameters to Function parameters for the function to reference. So Parameters are the labels for keys and function parameters are the actual function parameters. For example, in the function:

```python
def func(a):
        print(a)
```

The Parameter could be titled "Value of a" \(this would show up in the edit box\). The parameter is called "a", so the json would be:

```javascript
{
        ...
        "Parameters" :
        {
                "Value of a" : "str"
                ...
        }
        "FunctionParameters" :
        {
                "Value of a" : "a"
                ...
        }
        ...
}
```

Our example would look like this:

```javascript
{
    "Name" : "Generic Module Func",
    "Type" : "Transition",
    "Parameters" :
    {
        "Parameter 1" : "int",
        "Parameter 2" : "str",
        "Parameter 3" : "float"
    },
    "FunctionParameters" :
    {
        "Parameter 1" : "param1",
        "Parameter 2" : "param2",
        "Parameter 3" : "param3"
    },

    "Module" : "GenericModuleFunc",
    "Function" : "genericfunc"
}
```

1. GenericModuleFunc.py

This is a class file. The skeleton of the file is shown below:

```python
def <functionname>(input1, input2, ..., parameter1, parameter2, ...):
    ...
    return <only one output for now>
```

Our function would look like this:

```python
def genericfunc(input1, input2, param1, param2, param3):
    print(param1, param2)
    return input1 + input2
```

* \_\_init\_\_.py

just like the first file, this just needs to exist. Nothing needs to be inside it.

### Using a Class

irst, create a folder inside the directory project/sdrcraft/blocks. This folder can be named whatever you want and changing the name wont do anything.All of the following four files will live in that folder

A Module consists of 4 files. Here is an example for adding a node called GenericModule

* \_\_module\_\_ 

In order for the program to recognize that this directory is a module, simply make a new file called \_\_module\_\_. Nothing needs to be in this file, it just needs to exist.

* GenericModule.json 

This is the configuration file.

It needs four fields:

* Name - The name that appears inside the dropdown menu
* Module - The name of the python file \(if we name our python file GenericModule.py, this is GenericModule\)
* Class - The class inside the python file \(if our class inside GenericModule.py is called GModule, this is GModule\)
* Parameters - All the editable parameters. This can be either str, float, int or dropdown \(dropdown is under development, refrain from this for now\).

Parameters are chosen by the user when they click on edit. The name of the parameter is the label and the user can enter the data type into that field in the editor window. When the user clicks ok, it returns a python dict of the parameters the user entered into a new module object

Our example would look like this:

```javascript
{
    "Name" : "Generic Module",
    "Module" : "GenericModule",
    "Type" : "Transition",
    "Class" : "GModule",
    "Parameters" : 
    {
        "Type" : "str",
        "ExampleNumber" : "int",
        "Value" : "float",
        "Example2" : "float"
    }
}
```

* GenericModule.py

This is a class file. The skeleton of the file is shown below:

```python
from sdrcraft.blocks.block_interface import *

class <ClassName>(ModuleInterface):
        def __init__(self, config):
            ...
            self.register(self.<function_name>, <number inputs>, <number outputs>)

        def <funcion_name>(self, data ... <however many inputs>):
            ...
            return output, <however many outputs>
```

config is the exact same thing as "Parameters" in our configuration, so to get the user input for "Type", you'd type config\["Type"\]

data and output are both numpy arrays

Let's assume Generic Module has 1 input and 1 output, the class would look like this:

```python
from sdrcraft.blocks.block_interface import *

class GModule(ModuleInterface):
        def __init__(self, config):
            self.type = config["Type"]
            self.examplenumber = config["ExampleNumber"]
            self.value = config["Value"]
            self.example2 = config["Example2"]

            self.register(self.func, 1, 1)

        def func(self, data):
            ...
            return output
```

* \_\_init\_\_.py

just like the first file, this just needs to exist. Nothing needs to be inside it.



