---
title: Installation of Sip for PyQt
layout: page
comments: no
---

Introduction
----------

One of the features of Python that makes it so powerful is the ability to take existing libraries, written in C or C++, and make them available as Python extension modules. Such extension modules are often called bindings for the library.

`SIP is a tool that makes it very easy to create Python bindings for C and C++ libraries.` It was originally developed to create PyQt, the Python bindings for the Qt toolkit, but can be used to create bindings for any C or C++ library.

SIP comprises a code generator and a Python module. The code generator processes a set of specification files and generates C or C++ code which is then compiled to create the bindings extension module. The SIP Python module provides support functions to the automatically generated code.

[Intro](http://www.riverbankcomputing.com/software/sip/intro) | [Download](http://www.riverbankcomputing.com/software/sip/download)

----------

Installation
---------

{% highlight bash %}
$ cd ~/Downloads/sip-4.15.5/
$ python configure.py
$ make
$ sudo make install
{% endhighlight %}
