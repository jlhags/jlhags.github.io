---
title: "Beginning Programming Lession 05"
subTitle: "Inheritance, Interfaces, and Type Assertion"
date: 2022-11-28T07:03:58-07:00
draft: true
tags: [go, golang, beginning programming, interfaces, inheritance, type assertion]
---

We created complex data types (structs/classes), but what if we wanted to create another `struct` that expands upon another? Or you wanted to create a number of different structs that play by a certain set of rules. The example code for today can be found [here](https://raw.githubusercontent.com/jlhags/Beginning_Programming_In_Go/main/Lesson_06/main.go).

```go
package main

import (
	"fmt"
	"math"
)

type Polygon interface {
	Shape() string
	Perimeter() float64
	Area() float64
}

type Rectangle struct {
	Length float64
	Height float64
}

func (r Rectangle) Perimeter() float64 {
	return 2*r.Height + 2*r.Length
}

func (r Rectangle) Area() float64 {
	return r.Height * r.Length
}
func (r Rectangle) Shape() string {
	return "rectangle"
}
func (r Rectangle) String() string {
	return fmt.Sprintf("Shape: %s, Dimensions: %fx%f, Perimeter: %f, Area: %f", r.Shape(), r.Length, r.Height, r.Perimeter(), r.Area())
}

type ColoredRectangle struct {
	Rectangle
	Color string
}

func (r ColoredRectangle) String() string {
	return fmt.Sprintf("Shape: %s, Dimensions: %fx%f, Perimeter: %f, Area: %f, Color: %s", r.Shape(), r.Length, r.Height, r.Perimeter(), r.Area(), r.Color)
}

type Triangle struct {
	Sides [3]float64
}

func (t Triangle) Perimeter() float64 {
	return t.Sides[0] + t.Sides[1] + t.Sides[2]
}

func (t Triangle) Area() float64 {
	p := t.Perimeter() / 2
	return math.Sqrt(p * (p - t.Sides[0]) * (p - t.Sides[1]) * (p - t.Sides[2]))
}

func (t Triangle) Shape() string {
	return "triangle"
}

func (t Triangle) String() string {
	return fmt.Sprintf("Shape: %s, Dimensions: %fx%fx%f, Perimeter: %f, Area: %f", t.Shape(), t.Sides[0], t.Sides[1], t.Sides[2], t.Perimeter(), t.Area())
}

func printSlice(s []interface{}) {
	for i, v := range s {
		fmt.Printf("%d - %v\n", i, v)
	}
}

func main() {
	rect := Rectangle{Length: 5, Height: 4}
	tri := Triangle{Sides: [3]float64{4, 5, 6}}

	fmt.Println(rect)

	fmt.Println(tri)

	var polys []Polygon

	polys = append(polys, rect)
	polys = append(polys, tri)

	var cr ColoredRectangle
	cr.Height = 7
	cr.Length = 8
	cr.Color = "red"

	polys = append(polys, cr)

	fmt.Println(polys)

	for _, p := range polys {
		fmt.Println(p)
	}

	fmt.Printf("Shape 1 type: %s\n", polys[0].Shape())
	random := []interface{}{1, "blue", rect, tri}

	printSlice(random)

	if _, ok := random[1].(Polygon); !ok {
		fmt.Println("Not a polygon")
	}

	fmt.Println(random[2].(Polygon).Shape())

}
```

## Interfaces
An interface in general provides a way of communication between two or more parties. A GUI (Graphical User Interface) provides a means communication between a computer and a human. In the case of Golang and many other programming languages, an interface is a definition of how a `struct` must behave in order to work with other portions of code that expect them to follow the definition. In our last lesson we created a `struct` `polygon`. It had a method to calculate the perimeter. What if we wanted to have it calculate area? Well, the area calculation varies from polygon to polygon. While there are a number of ways we could deal with this, let's see how we could use interfaces to help. 

### Defining an Interface
```go
type Polygon interface {
	Shape() string
	Perimeter() float64
	Area() float64
}
```

Here we created an interface `Polygon` and stated for a `struct` to be considered a `Polygon` it must have at least the three methods `Shape`, `Perimeter`, and `Area` implemented.

### Implementing an Interface
In many languages when you define as struct or class, you must explicitly state which interfaces it is implementing. This is not the case for Golang. 

```go
type Rectangle struct {
	Length float64
	Height float64
}

func (r Rectangle) Perimeter() float64 {
	return 2*r.Height + 2*r.Length
}

func (r Rectangle) Area() float64 {
	return r.Height * r.Length
}
func (r Rectangle) Shape() string {
	return "rectangle"
}
```

Here we created a struct called `Rectangle` and since it implements all the methods defined in the `Polygon` interface, it is considered as a `Polygon` as well.

### Using an Interface
So, we have a `Rectangle` now that is also a `Polygon`, but where did that get us? Lets say we wanted to create a function that performed some action on any `Polygon` for example:

```go
    func PrintPolygon(p Polygon) {
        fmt.Printf("type: %s, perimeter: %f, area: %f\n", p.Shape(), p.Perimeter(), p.Area() )
    }
```

You could pass any instance of a struct as long as it implements the `Polygon` interface to this function. While the above example works well to show a usage of interfaces, there is another way to achieve a similar result

### Stringer Interface
There is aa interface in the `fmt` package in Golang referred to as Stringer.

```go
type Stringer interface {
    String() string
}
```
If you pass a variable to a method in `fmt` and the expected result is a string, the package will try to call the `String` method on the variable automatically. We can make `Rectangle` a `Stringer` by adding:

```go
func (r Rectangle) String() string {
	return fmt.Sprintf("Shape: %s, Dimensions: %fx%f, Perimeter: %f, Area: %f", r.Shape(), r.Length, r.Height, r.Perimeter(), r.Area())
}
```

Now if we wanted to print `Rectangle` it might look like:

```go
rect := Rectangle{Length: 5, Height: 4}
fmt.Println(rect)
```

### Generic Interface
In Golang there is another type of interface which is referred to as simply `interface`. It is used when it doesn't matter what the `type` of a variable is. This can be useful for when the `type` of the variable is irrelevant to the actions being performed on it. For example, the `len` function is used to get the number of items in a slice, but it doesn't need to know what the `type` of the items are, or even if they are all the same `type`

#### Using interface{}

If you wanted to define a variable or argumetn to a function as a typeless interface, you can specify the type as `interface{}` Check out:

```go
func printSlice(s []interface{}) {
	for i, v := range s {
		fmt.Printf("%d - %v\n", i, v)
	}
}
```
This function takes a slice and prints out the position in the slice along with the contents at that position. It doesn't care it the slice holds, `Rectangle`, `Polygon`, `int`, etc, or any combination of types:

```go
random := []interface{}{1, "blue", rect, tri}
printSlice(random)
```

## Type Assertion
What happens if we have a variable that is an `interface{}` but want to treat is as a specific type, or we want to test if is is really a specific type?

```go
if _, ok := random[1].(Polygon); !ok {
	fmt.Println("Not a polygon")
}
```
Here we are checking to see if the element in position 1 of the `random` slice is a `Polygon` the `.(Polygon)` operation has two return values, one is the variable in question now assigned the type `Polygon` and the other is whether the assertion was successful. In this case we only care if the assertion is successful, since we only want to see if it is a `Polygon`

## Inheritance
What if we wanted to create a group of types that have something in common? For instance, if we wanted to create a `ColoredRectangle` isn't is also a `Rectangle` with some extra information? We can define an new `struct` that inherits the member variables and methods of another:

```go
type ColoredRectangle struct {
	Rectangle
	Color string
}
```

Now we have a new type called `ColoredRectangle` that is a Rectangle, but has an additional variable to store its color.

```go
var cr ColoredRectangle
cr.Height = 7
cr.Length = 8
cr.Color = "red"
```

Here we set the `Height` and `Width` that were inherited from `Rectangle`. We also set its `Color`.

## Conclusion
We learned how we can expand the usefulness and reusability of structs, along with how to start working with variables of "un-pre-determined" types. Look through the example code again. Does it seem more clear now? Can you think of a better way to implement the various `Polygon` structs? In the next lesson we will look at Recursion and Error Handling.

