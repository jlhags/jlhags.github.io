---
title: "Beginning Programming Lesson 03"
subtitle: "Complex Data Types (Structs/Classes)"
date: 2022-11-08T10:03:18-07:00
draft: false
categories: tutorials
tags: [go, golang, beginning programming, complex data types, structs]
keywords: [go, golang, beginning programming, complex data types, structs]
---

In the last installment of this series we learned about variables, and covered some simple variable types. Now we are going to learn about complex data types. In short, they store more than a single data point of information. For example, what if we wanted to have a variable that stored information about a rectangle? There is more than one bit of information to describe it. At minimum you would probably want to store its width and length.  Depending on the language you use, there are different terms for the data types that can hold such information. The main two are classes and structs (short for structures). Some languages use both, but in the case of Golang it uses structs. You can download the example code [here](https://raw.githubusercontent.com/jlhags/Beginning_Programming_In_Go/main/Lesson_03/main.go), or see below:
```go
package main
import "fmt"

type rectangle struct {
	Width  int
	Length int
}

func (rectangle) Area() int {
	return r.Length * r.Width
}

func (r rectangle) Perimeter() int {
	return 2 * (r.Length + r.Width)
}

func (r *rectangle) Scale2X() {
	r.Length = r.Length * 2
	r.Width = r.Width * 2
}

func main() {
	rect := rectangle{Length: 4, Width: 5}
	fmt.Printf("Rectangle: %+v\n", rect)
	fmt.Printf("Area: %d\n", rect.Area())
	fmt.Printf("Perimeter: %d\n", rect.Perimeter())

	rect.Scale2X()

	fmt.Printf("Rectangle: %+v\n", rect)
}
```

## Defining a Struct
When creating a variable, as you recall, we have to specify the type. If we want to use a new type, we must define it. Note the following definition of a new type called `rectangle`:
```go
type rectangle struct {
	Width  int
	Length int
}
```
Here we say the new type is a struct with Width and Length as components, called fields, of it. Both will be `int` values. 

## Declaring and Using Structs
Now that the `rectangle` type is defined, we can utilize it as noted in first line of function main:
```go
rect := rectangle{Length: 4, Width: 5}
```
`rect` will be a rectangle with a Length of 4 and a Width of 5. If we wanted to specifically look at an individual component/field like `Width`, you could do so by using `rect.Width`. What if we wanted to know / keep track of the rectangle's area. We could add a field to the struct to store it, but that would require the user of the struct to calculate it and keep it up to date. We can create special functions that are specific to a struct called methods.

## Methods
Methods are functions that are used on structs and have access to the calling variables fields and other methods. Going back to the rectangle and its area, we could create a method called `Area` that calculates the area and returns it back:
```go
func (r rectangle) Area() int {
	return r.Length * r.Width
}
```
The main difference you will see in this definition from a normal function is after the func keyword, we see `(r rectangle)` meaning only rectangles have access to this function/method. To call it we would use the same kind of dot notation we used to access its fields, `rect.Area()`. When inside the method we can access the calling structs fields by using `r`, as defined in the beginning of the function definition. 

Remember when learning about functions we can either pass by reference or by value. The same is not only true for the arguments of the function, but also the struct itself. In this case since it is passed by value (note the lack of an *), if we were to modify r in the function it wouldn't actually modify `r` in the main function since it only has access to the value of `r`. In other words, it made a copy of `r` from main. But what if we wanted to be able to actually manipulate `r`? If we wanted to have a function that resized/scaled the rectangle up we could do:
```go
func (r *rectangle) Scale2X() {
	r.Length = r.Length * 2
	r.Width = r.Width * 2
}
```
Here `r` is of type `*rectangle`. So we have access to it directly rather than a copy. When we look at the rect after calling `Scale2X`, Length will be 8 and height will be 10:
```go
rect.Scale2X()
fmt.Printf("Rectangle: %+v\n", rect)
```
 Try removing the `*` from `(r *rectangle)` and run the program again. See the difference?

## Conclusion
Now we know how to store information about entities that can't be stored as a single value. What other fields and methods do you think would be useful for the rectangle struct?  Maybe a method that allows you to specify how much you want to scale it up or down? In our next lesson we will talk about [Control Structures and Array-like Things!](/post/beginning-programming-lesson-04)
