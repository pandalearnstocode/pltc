---
title: Python type checking using mypy
author: Aritra Biswas
date: '2019-06-25'
slug: python-type-checking-using-mypy
categories: []
tags: []
cover: "/img/python_img/mypy.png"
draft: false
---

Cut out summary from your post content here.

<!--more-->
# Type checking in Python

## Python is strongly, dynamically typed.

* __Strong__ typing means that the type of a value doesn't change in unexpected ways. A string containing only digits doesn't magically become a number, as may happen in Perl. Every change of type requires an explicit conversion.

* __Dynamic__ typing means that runtime objects (values) have a type, as opposed to static typing where variables have a type.

For details check [this](https://stackoverflow.com/questions/11328920/is-python-strongly-typed).

<!--more-->

Let us write a function which will return addition of two numbers.
```python
def example_addition(a,b):
    return a + b
```

This function looks and works absolutely fine for expected inputs.

```python
a = 3
b = 4
print('Addition of',a, ' and',b, 'is:',example_addition(a,b))

```


```python
def example_addition(a,b):
    return a + b

# This one works fine.
a = 3
b = 4

print('Addition of',a,'and',b, 'is:',example_addition(a,b))
print('data type of a:',type(a))
print('data type of b:',type(b))
```

    Addition of 3 and 4 is: 7
    data type of a: <class 'int'>
    data type of b: <class 'int'>



```python
# But let us consider due to some bud this a and b becomes of datatype string then,

a = '3'
b = '4'

# Then the same function call will return,
print('Addition of',a,'and',b, 'is:',example_addition(a,b))
print('data type of a:',type(a))
print('data type of b:',type(b))
```

    Addition of 3 and 4 is: 34
    data type of a: <class 'str'>
    data type of b: <class 'str'>


Now in case of a small program it is still possible to check and debug the code and figure out that where the problem has occured but imagine a large system with multiple variables or some API where these inputs are coming from a POST call. Suppose some validation breaks and variables values are passed as string. It will not throw a compilation error or syntax error but runtime error will take place. In case of a large system it is almost impossible to debug and figure out these type of errors. So, in oder to be sure that any such scenario does not occur we can use mypy package. In the following section we will explore how we can use mypy to avoid these type of errors.




```python
%%writefile mypy_example1.py

def add_add_this_with_mypy(a:int,b:int)->int:
    return a + b

a = 3
b = 4

print('Addition of',a,'and',b, 'is:',add_add_this_with_mypy(a,b))
print('data type of a:',type(a))
print('data type of b:',type(b))
```

    Writing mypy_example1.py



```python
!python mypy_example1.py
```

    Addition of 3 and 4 is: 7
    data type of a: <class 'int'>
    data type of b: <class 'int'>



```python
!mypy mypy_example1.py
```

Since there is not error we can be sure that everything has worked has per plan. But now we will change the data type of the variables and will see how mypy can be useful to prevent the above mentioned scenario.


```python
%%writefile mypy_example1.py

def add_add_this_with_mypy(a:int,b:int)->int:
    return a + b

a = 3
b = 4

print('Addition of',a,'and',b, 'is:',add_add_this_with_mypy(a,b))
print('data type of a:',type(a))
print('data type of b:',type(b))

a = '3'
b = '4'

# Then the same function call will return,
print('Addition of',a,'and',b, 'is:',add_add_this_with_mypy(a,b))
print('data type of a:',type(a))
print('data type of b:',type(b))
```

    Overwriting mypy_example1.py



```python
!python mypy_example1.py
```

    Addition of 3 and 4 is: 7
    data type of a: <class 'int'>
    data type of b: <class 'int'>
    Addition of 3 and 4 is: 34
    data type of a: <class 'str'>
    data type of b: <class 'str'>



```python
!mypy mypy_example1.py
```

    mypy_example1.py:12: error: Incompatible types in assignment (expression has type "str", variable has type "int")
    mypy_example1.py:13: error: Incompatible types in assignment (expression has type "str", variable has type "int")


As you can see that mypy has thrown an error. 

__Note:__ mypy can catch this form of error only if you run the python file from console. It you just type the datatype but do not compile the function in mypy mode it will not do anything special. So, in order to be sure when in doubt compile it with mypy and always has the datatype of variable in case of definition of a function or variables. It also increases the readability of the code.

## Example of  variable


```python
def add_add_this_with_mypy(a:int,b:int)->int:
    return a + b

a = 3
b = 4

print('Addition of',a,'and',b, 'is:',add_add_this_with_mypy(a,b))
print('data type of a:',type(a))
print('data type of b:',type(b))

a = '3'
b = '4'

# Then the same function call will return,
print('Addition of',a,'and',b, 'is:',add_add_this_with_mypy(a,b))
print('data type of a:',type(a))
print('data type of b:',type(b))

```


## Example of list

```python
# Example of lists
from typing import List

def get_a_list(start:int,end:int)->List[int]:
    result = []
    for i in range(start,end):
       result.append(i)
    return result

print(get_a_list(8,22))
```

## Example of tuple

```python
# Example with tuple
from typing import Tuple

def get_me_a_tuple(name:str,age:float,height:float)->Tuple[str, int, float]:
    result:Tuple[str, int, float] = (name, age, height)
    return result

print(get_me_a_tuple("Aritra",23,95))

```

## Example of dictionary 

```python
# Example of using a type checking with dict
from typing import Dict
def getname(firstname:str,familyname:str)->Dict[str, str]:
    fullname:Dict[str, str] = {
    "first_name": firstname,
    "last_name": familyname
    }
    return fullname

print('My name is ',getname(firstname = 'Aritra',familyname = 'Biswas'))

```


## Example of list of dictionaries
```python
from typing import List, Dict
def get_me_a_list_of_dicts(name_1:Dict[str, int],name_2:Dict[str, int])->List[Dict[str, int]]:
    result = []
    result.append(name_1)
    result.append(name_2)
    return result

name_1 = {"Aritra":27}
name_2 = {"Arghya":21}

print(get_me_a_list_of_dicts(name_1,name_2))
```

## Example of list of tuples

```python
from typing import List, Tuple
def get_me_cordinates(x1:float,y1:float,x2:float,y2:float)->List[Tuple[float, float]]:
    coordinates = List[Tuple[float, float]]
    result:coordinates = [(x1,y1),(x2,y2)]
    return result

print(get_me_cordinates(x1 = 3.2,y1 = 3.22,x2 = 4.1 ,y2 = 8.9))
```

__Note:__
* This is possible to use pd.DataFrame or np.ndarray as datatype.
* mypy will treat the above mentioned data types as others. There is a numpy project which was about to be done by end of 2018. But there is not much development on that front.

__Links:__

* [Type-Checking Python Programs With Type Hints and mypy](https://www.youtube.com/watch?v=2xWhaALHTvU&t)
* [Carl Meyer - Type-checked Python in the real world - PyCon 2018](https://www.youtube.com/watch?v=pMgmKJyWKn8)
* [Learn how to use static type checking in python](https://medium.com/@ageitgey/learn-how-to-use-static-type-checking-in-python-3-6-in-10-minutes-12c86d72677b)
