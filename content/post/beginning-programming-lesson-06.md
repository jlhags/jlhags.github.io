---
title: "Beginning Programming Lesson 06"
subTitle: "Recursion and Error Handling"
date: 2022-12-07T11:20:36-07:00
draft: false
categories: tutorials
tags: [go, golang, beginning programming, recursion, errro handling]
keywords: [programming, recursion, error handling]
---
When I was in college my professor explained recursion as "Pretend you already have a function that does what you need to do." The problem with that explanation is, it only makes sense after you understand recursion. I think we can do better here. While we are at it, let's look at error handling too. You can find the code for today's lesson [here](https://raw.githubusercontent.com/jlhags/Beginning_Programming_In_Go/main/Lesson_06/main.go).

```go
package main

import (
	"fmt"
	"log"
)

type MinInputError struct {
}

func (e *MinInputError) Error() string {
	return "invalid input, number must be greater than 0"
}

type MaxInputError struct {
}

func (e *MaxInputError) Error() string {
	return "invalid input, number must be less than 100"
}

func Factorial(n int) int {
	if n == 1 {
		return n
	}
	return n * Factorial(n-1)
}

func Factorial2(n int) (int, error) {
	if n < 0 {
		return 0, &MinInputError{}
	}
	if n > 100 {
		return 0, &MaxInputError{}
	}
	ret := n
	for i := n - 1; i > 0; i-- {
		ret = ret * i
	}
	return ret, nil
}

func main() {
	fmt.Println(Factorial(10))
	fmt.Println(Factorial2(10))
	fmt.Println(Factorial2(-10))

	f, err := Factorial2(-13)
	if err != nil {
		log.Print(err)
	} else {
		fmt.Println(f)
	}

	f, err = Factorial2(100000000000)

	if _, ok := err.(*MaxInputError); ok {
		fmt.Println("Do something special")
	} else {
		fmt.Println(f)
	}

}

```
## Recursion
If you want to understand recursion, check out my tutorial on [recursion](https://raw.githubusercontent.com/jlhags/Beginning_Programming_In_Go/main/Lesson_06/main.go). (:-p) (Sorry I couldn't help myself). Anyway, a recursive function/method is one that calls itself in its definition. It's probably best to look at an example. Let's create a function that calculates the factorial of a number. In case you don't recall:

$$f(n) = n! = 1 * 2 * 3...*(n-2) * (n-1) * n$$

So if we wanted to know 5!:
$$5! = 1 * 2 * 3 * 4 * 5$$

What about 4!:
$$4! = 1 * 2 * 3 * 4$$

Do you see that 5! is just 4! * 5? Oh and 4! is 3! * 4... Noticing a trend? We could write the equation as:
$$ x=n! = (n-1)! * n $$
That might not seem very helpful, but is helpful for writing a function to calculate factorial:

```go
func Factorial(n int) int {
	return n * Factorial(n-1)
}
```
Now if we were to use this method, it wouldn't know to stop when n = 1. So, it would either keep going until there wan't enough memory in the [stack](https://en.wikipedia.org/wiki/Call_stack). You see I made an error in the final mathematical function that lead to an error in our code. We have to make sure n > 0:
```go
func Factorial(n int) int {
	if n == 1 {
		return n
	}
	return n * Factorial(n-1)
}
```

If we run it now, it will know when to stop the recursion, but is it still correct? No. Factorial should only apply to positive integers. You might be thinking why not make `n` an `uint`, "unsigned integer", while it probably should be, it doesn't solve all the problems, nor does it prevent a negative value from being passed to it, if passed as a literal for example:

```go
Factorial(-10)
```
This would still compile and run even it we changed the definition to require a `uint`. Due to the way number are stored, a negative `int` and a large `uint` could be the same value in memory. If you would like more explanation of that, please leave a comment.  

In the next section we will look to solve it using Error Handling.

## Error Handling
What do you do when you encounter an error? A number of programming languages use a concept called `Exceptions`. But since Golang can return multiple values the standard way to handle errors is to return an `error` type along with the standard return values. Let's rewrite the function in a way that is more similar to the original equation and doesn't use recursion:

```go
func Factorial2(n int) int {
	ret := n
	for i := n - 1; i > 0; i-- {
		ret = ret * i
	}
	return ret
}
```

We still have the problem with trying to calculate the Factorial of a negative number. We could just do something like return -1 when a negative number is passed, and make the user of the function validate the return value accordingly. But without documentation, how would they know, or even if it is documented who says they will read it. Also, what if there are other things that could go wrong? Let's have the function return an error that can explain what went wrong, and if nothing went wrong we will return `nil` or no value for the error.


```go
func Factorial2(n int) (int,error) {
    if n < 0 {
        return 0, errors.New("value must be a positive integer")
    }
	ret := n
	for i := n - 1; i > 0; i-- {
		ret = ret * i
	}
	return ret
}
```

Golang has an `errors` package that make it easy to create generic errors which is of type `error`, by using `error.new` method. While this is useful, if you want to create more robust error handing, custom error types can be created by implementing the error interface. It is defined simply as:

```go
type error interface {
    Error() string
}
```
Using this we can create different types of errors thus allowing the user to check the type an react accordingly, rather than parsing the error string to deduce the issue.

We already know that one type of error we could have is if somebody passed as negative value to `Factorial2`. So, let's create a new error type for that situation.

```go
type MinInputError struct {
}

func (e *MinInputError) Error() string {
	return "invalid input, number must be greater than 0"
}
```

Now we may want to limit how big a value is passed for performance reasons:
```go
type MaxInputError struct {
}

func (e *MaxInputError) Error() string {
	return "invalid input, number must be less than 100"
}
```
Now let's add them to `Factorial2`:
```go
func Factorial2(n int) (int, error) {
	if n < 0 {
		return 0, &MinInputError{}
	}
	if n > 100 {
		return 0, &MaxInputError{}
	}
	ret := n
	for i := n - 1; i > 0; i-- {
		ret = ret * i
	}
	return ret, nil
}
```

Now the user of `Factorial2` can write some code using [type assertion](/post/beginning-programming-lesson-05) to handle each type of error differently:

```go
f, err = Factorial2(n)

if _, ok := err.(*MaxInputError); ok {
	fmt.Println("Do something special")
} else {
	fmt.Println(f)
}
```

## Conclusion
Recursion can be a powerful tool, but remember to be careful not to create infinite recursion. Also ,while a recursive solution to a problem might be the simplest, it may not be the most efficient. Which of the two methods of calculating factorial is the fastest? Which uses the least memory?

Error handling is easy to over look, but almost always regretted not handling it sooner that later. Remember you might not be the only one using your code. How might some of the previous lessons benefitted from more robust error handling? In the next lesson we will look at how to be more modular with our code by created packages/modules.