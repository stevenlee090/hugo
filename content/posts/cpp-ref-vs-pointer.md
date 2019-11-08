---
title: "References and Pointers in C++"
date: 2019-11-08T16:07:56+11:00
draft: true
---

Here I will try to go over the differences between reference and pointers in C++. When I first encountered these concepts, I was not sure about the the difference between `reference` and `pointer`, and found them quite confusing. As I am going over the [C++ Primer book ](https://www.goodreads.com/book/show/768080.C_Primer), I thought it would be a good idea to make some notes on these concepts.

## Compound Types
Specifically, reference and pointer were discussed in section 2.3 compound types.

> Compound type is a type that is defined in terms of another type.

### Reference

* A reference defines an alternative name for an object.
* A reference type "refers to" another type.
* We can define a reference type by writing a declarator of the form `&d`.
* Essentially, a reference is an alias.

```
int value = 1024;
int &refVal = value; // refVal refers value
```

Some properties to keep in mind:
* A reference must be initialised, so we cannot simply write `int &refVal2;`.
* We bind the reference to its initialiser. Once initialised, reference remains bound, and cannot rebind to a reference.

### Pointer
A pointer is a compound type that "points to" another type. Like references, pointers are used for indirect access to other objects.

* Unlike a reference, a pointer is an object in its own right. Pointers can be assigned and copied.
* Unlike a reference, a pointer need not be initialised at the time it is defined.

We define a pointer type by writing a declarator of the form `*d`.
```
int *ip1, *ip2; // both ip1 and ip2 are pointers to int
```

A pointer holds the address of another object. We can get the address of an object by using address-of operator `&`.
```
int value = 42;
int *p = &value; // p holds address of value; p is a pointer to value
```

When assigning pointer variables, the data types must match, as the type of pointer is used to infer type of object.

The value (i.e., the address) stored in a pointer can be in one of four states:
1. It can point to an object.
2. It can point to the location just immediately past the end of an object.
3. It can be a null pointer, indicating that it is not bound to any object.
4. It can be invalid; values other than the preceding three are invalid.

It is an error to copy or otherwise try to access the value of an invalid pointer. Such an error is one that compiler is unlikely to detect.

#### Using a pointer to access an object

We can use the dereference operator `*` to access an object which a points to.

```
int value = 42;
int *p = &value;
std::cout << *p << std::endl;
```

#### Null Pointers
A null pointer does not point to any object. We can obtain a null pointer by:

```
int *p1 = nullptr;      // equivalent to int *p1 = 0;
```

> Modern C++ programs generally should avoid using `NULL` and use `nullptr` instead.

We should always initialise all the pointers.

* Define pointer only after the object to which it should point has been defined.
* Initialise pointer to `nullptr` or `0` if we are not binding any object.

#### Comparing two pointers
> Note that it is possible for a pointer to an object and a pointer one past the end of a different object to hold the same address.

See this [stackoverflow post](https://stackoverflow.com/questions/21850108/what-is-pointer-past-the-end-of-an-object-means) for more details, but essentially what this means is that, if we have:

```
int a[] = {1, 2};
float b;
```

It is possible to have the following expression to be true.

```
(void *) &a[2] == (void *) &b;
```



### Combining the two concepts

For example, we can have:

```
int j = 42, *p2 = &j;   // j is an int, p2 is an int pointer to j
int *&pref = p2;        // pref is a reference to the pointer p2
```

### Differences

The most important difference between pointer and reference is that, reference is not an object, whereas pointer is an object on its own.

* To re-iterate, once a reference has been initialised, it cannot be bound to another object.
* On the other hand, a pointer can point to different objects, can can be left uninitialised. However, this is generally not recommended in practice.
