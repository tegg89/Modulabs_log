## Demystifying Python 'import' statements
### Table of contents
1. [Overview](#1.-overview)
2. Module system in Python
3. The **import** statement
4. Package, Subpackage, \_\_init\_\_.py
5. Interactive execution (REPL mode)
6. Execute python file as a **module** or as a **standalone** script

### 1. Overview

#### Single Responsibility Principle:
>The single responsibility principle is a computer programming principle that states that every module or class should have responsibility over a single part of the functionality provided by the software, and that responsibility should be entirely encapsulated by the class. (Wikipedia)  

I would like to begin this short tutorial by quoting one of the well-known best practices on the development side.  
To put it simply, a single module or a function should take exactly one responsibility so that the project can keep growing without having the common maintanence issues.  
Likewise, we have hundreds of reasons to keep our code logic separable by its role, regardless of which programming language we prefer to code with.  

blah blah blah  
blah blah blah  
blah blah blah  
blah blah blah  
### 2. Module system in Python
In Python planet, 
- Every file ends with ```.py``` is considered as **a module.**  
- Every module lives in their own small world, we refer to them as the **module scope**, or **namespace**
- **Package** is simply a directory that contains one or more python modules, or extra subpackage(s) organized with hierarchical structure.  
In python 2, **you should explicitly tell python interpreter** that we want this directory be a python pacakge by putting \_\_init\_\_.py file in the root of the package.  
Since python 3, every directory that has module(s) inside of it will become a pacakge.  
  
Please check [official docs](https://docs.python.org/3/tutorial/modules.html) for more elaborated examples.

### 3. The **import** statement
```python
import tensorflow
```
### 4. Package, Subpackage, \_\_init\_\_.py
### 5. Interactive execution (REPL mode)
### 6. Execute python file as a **module** or as a **standalone** script
