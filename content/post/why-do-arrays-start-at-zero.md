---
title: "Why Do Arrays Start at Zero?"
date: 2022-11-11T12:29:33-07:00
draft: false
description: In most programming languages arrays and other list type things start counting at zero. Why?
categories: history
tags: [array, index]
keywords: [programming, array, index, zero]
---

A common point of confusion for new programmers arises when trying to access a item in an array/list/slice/string etc. The "first" item is not located in position 1, but rather it is in position 0, or so it seems. The reason for this is all thanks to the programming language C. But why is C like that? For this we need to understand [pointers](/post/beginning-programming-lesson-02/). A pointer is a [variable](/post/beginning-programming-lesson-02/) that stores a memory location holding a value. More specifically it is the beginning position in memory. For example, say we have a string "abcdefg" and using ACSII each character is 1 byte. Lets also say it is stored in memory starting at location 42. I will be using the decimal representation for the sake of simplicity:

|a|b|c|d|e|f|g|`null`|
| - | - | - | - | - | - | - | - | 
|42|43|44|45|46|47|48|49|

C strings end will a `null` in oder for it to know where the end of the string is. Lets say we have a points `p` that points the string, so it's value would be 42. But what if we wanted to reference the third character, what would it's memory location be relative to the beginning? In this particular case it is super easy since each character is exactly 1 byte, so it's just:

$$ p+2 $$

but what if we were using unicode characters? Each character is 2 bytes long. So the same string looks like starting in the same location:

|a|b|c|d|e|f|g|`null`|
| - | - | - | - | - | - | - | - | 
|42-43|44-45|46-47|48-49|50-51|52-53|54-55|56|


The third character in this case starts at location 44 or:

$$ p+4 $$

How would we write this formula in a generic way to get any position in the string give `size` is the size in bytes of a single character

$$ p + position * size  $$

Now what would that look like in the beginning days of C? Assuming `p` is already defined:

```c
int pos = 2;
char c2 = *(p + pos * sizeof(char));
```

So here we get the location of the second character then use `*` to dereference it, aka get its value, not it's location. Ugh, thats a lot to do just to access a specific item. It starts to look even worse for multi-dimensional arrays. Knowing this some shorthand was created. The following is the equivalent of above using the shorthand:

```c
int pos = 2;
char c2 = p[pos];
```
Now if we use `p[1]` what memory location do we get? Well plug it into the long form equation:
$$ 42 + 1 * 2 = 44$$

The character at position 44 is "b". To get "a" we use `p[0]`:
$$ 42+1*0 = 42$$

And there you have it. The `[]` notation is shorthand for something slightly more complicated than it initially appears. Languages that proceeded C, even if they don't have pointer arithmetic, followed suit either out of convenience, consistency, nostalgia, or a combination.

