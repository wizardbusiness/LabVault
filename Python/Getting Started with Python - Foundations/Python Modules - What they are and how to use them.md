
> [!info] ***Q: What is a Module?***
> 
> - **A module is an object that maps names to objects (functions, classes, values)**
> - **A module's source defines what objects (functions, classes, values) it includes**
> - **Any `.py` file can be a module source**
> - **Modules grouped together in a directory form a *package*.** 
>  - **You can make your own packages, or download them via a *package manager* like *pip*** 
> 	 - **Note:** *Package Manager packages aren't strictly just python packages. They include additional stuff like metadata, install scripts and dependencies which is used for package deployment.* 
> - **Under the hood modules are instances of python's `ModuleType` *type object*. You can inspect the `ModuleType` object at `types.Moduletype`**
> 
> ***Sources***
>   - https://claude.ai/chat/644f0086-e3ed-4b70-b3db-fdb3dc16d4cc
>   


> [!info] ***Q: How do I create a module?***
> 
> - **To create a module in Python 3.3+ simply create a new `.py` file inside your project directory.**   
> - **To Create a module in pre 3.3 releases you must include an `__init__.py` file in the same directory. - This defines any module files in the directory as a *package.***
>   - **Note:** *Even though Python 3.3+ doesn't need an `__init__.py` file, if you want to control **what** modules are exported from a package you'll still need to add one and write your export definitions inside.* 
> - **Modules can be created directly using a `ModuleType` object instance**
>   
>   ```python
>   import types
>   your_custom_module = types.ModuleType("yourcustommodule")
>   your_custom_module.someValue = 42
>   sys.modules["yourcustommodule"] = your_custom_module
>   ```
>    
>***Sources***
>  - [https://claude.ai/chat/644f0086-e3ed-4b70-b3db-fdb3dc16d4cc](https://claude.ai/chat/644f0086-e3ed-4b70-b3db-fdb3dc16d4cc)
>  - https://claude.ai/chat/5135db7c-6658-4ad0-aad4-e9450160e111  
>   


> [!info] ***Q: What's the difference between a package and a module?***
> 
> ***Packages group related modules together***
> 
> |                   | **Module**                                   | **Package**                      |
> | ----------------- | -------------------------------------------- | -------------------------------- |
> | *What it is*        | A single `.py` file                          | A directory *containing* modules |
> | *Has `__init.py__`* | No                                           | Yes (implicit in python 3.12+)   |
> | *Purpose*           | Grpup related code - Functions, classes, etc | Group related *modules*          |
> | *Dot access*        | `<module>.name`                              | `<package>.<module>.<name>`      |
> 
> ---
>   
>**Sources**
>- https://claude.ai/chat/9e38ac0c-7e7a-4ae4-bfa7-132167ceaa7e
>   


> [!code] `code` *jot*
> 
> ####  Methods of Importing
> 
> ##### 1. **Basic Import**
> ~~~python
> # import <module>
> import numpy
> ~~~
> 
> ##### 2. **Import with Alias**
> ~~~python
> # import <module> as <abbr>
> import numpy as np
> ~~~
> 
> ##### 3. **From Import**
> ~~~python
> # from <module>.path import <submodule a>, <submodule b>, ... 
> from os.path import join, exists
> ~~~
> 
> ##### 4. **Star Import**
> ~~~python
> # from <module>.path import * 
> from os.path import join, exists
> ~~~
> 
> ##### 5. **Relative Import**
> ***Imports from the package at the current system path***
> ~~~python
> # . import <submodule a>, <submodule b>, ... 
> from . import utils
> ~~~
> 
>    
> **Sources**
> - https://claude.ai/chat/9e38ac0c-7e7a-4ae4-bfa7-132167ceaa7e
>  
> 


