---
layout: post
title: "Type in Swift"
description: "Type in Swift"
category: swift
tags: [swift]
---
{% include JB/setup %}

## Type in Swift

Swift is a strongly Typed Language. Meaning that it will consider each and every variable along with its type.
If some operation causes type mis match it causes an error.

Before getting into this let us see the ways of making swift compiler understand the type of 
the variable.

**Explicit indication**

var \<variableName\>: \<variableType\>

variableType tells the compiler that the variable is of this type.
so for eg.

var name: String

**Type Inference**

var \<variableName\> = \<variableValue\>

so from the variableValue it figures out the type of the variable.

So having known this, where comes strong typing ? Ya, having known the type of the variable, if the varible is 
assigned a value of a different type then the compiler cannot tolerate as the expected type did not match the
input value type. It flings an error in such a scenario. 

For eg. 

```swift
var name = "Hello"
name = 1

<stdin>:3:8: error: cannot assign a value of type 'Int' to a value of type 'String'
name = 1

```

This intolerance to mismatching variable types makes 
it a Strongly Typed Language.
