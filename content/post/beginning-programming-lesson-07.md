---
title: "Beginning Programming Lesson 07"
subTitle: "Packages and Modules"
date: 2023-01-16T11:20:36-07:00
draft: false
categories: tutorials
tags: [go, golang, beginning programming, packages, modules]
keywords: [programming, beginning programming,packages, modules]
---
In previous lesson we put all of out code in one file and one package, `main`. However, aside from very small projects, this won't scale very well, and does make your code very reusable apart from copying and pasting. We have already seen the usefulness of packages through using the `fmt` and `errors` packages. So let's look at how to create our own. You can find the code for today's lesson [here](https://github.com/jlhags/Beginning_Programming_In_Go/tree/main/Lesson_07). There is more than one file this time!

main.go:
```go
package main

import (
	"fmt"

	"github.com/jlhags/Beginning_Programming_In_Go/Lesson_07/polygons"
)

func main() {
	rect := polygons.Rectangle{Length: 5, Height: 4}
	tri := polygons.Triangle{Sides: [3]float64{4, 5, 6}}

	fmt.Println(rect)

	fmt.Println(tri)
	rect.Area()
	//rect.internalUseOnly()
}

```

polygons/polygons.go:
```go
package polygons

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

func (r Rectangle) internalUseOnly() {
	fmt.Printf("Only can be used from inside polygon package")
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
```
go.mod
```go
module github.com/jlhags/Beginning_Programming_In_Go/Lesson_07

go 1.16
```

Take a look at `main.go`. In the imports section you will notice `"github.com/jlhags/Beginning_Programming_In_Go/Lesson_07/polygons"` This is saying we will be using a package called "polygons", but also where it can be found, "github.com/jlhags/Beginning_Programming_In_Go/Lesson_07". There is not requirement that it has to be on github, it could be gitlab, or any other git hosting server. In fact in this case since the polygons package is really just a sub-package "Lesson 7" we don't even need to have it remotely available. If we wanted to make the `polygons` package usable by other projects, we would probably set it up differently. In this use case we are really just creating different package as as way of organizing our code. 

What if we wanted to make the `polygons` package available for other projects to use? Well, a simple way would be to initialize it as as stand alone module. In the `polygons` folder we can run:

```bash
go mod init github.com/jlhags/Beginning_Programming_In_Go/Lesson_07/polygons
```

This will create a go.mod file very similar the one we have for our Lesson_07 project. Note that the above way of dealing with modules is a newer concept in Go. More in depth documentation of it can be found at https://go.dev/blog/using-go-modules



## Exported or Not?
Many programming languages have a concept of public v.s. private. Public means everyone has access, and private means only the owner has access. This access to apply to methods, member variables, etc. In Golang, it uses slight different terminology. They are either "exported" or not. Rather and using keywords like many languages, exported methods and variables start with a capital letter. In our `polygons` package most of the methods are exported, except `internalUseOnly`. This methods cannot be used outside the `polygons` package. If you try, you will get a complier error. Why might we want to not export certain methods and variables? This will become more clear in future examples.


## Conclusion
Packages are a great way to keep our code organized and reusable. We covered the very basics of how they are handled in Golang. We've now reviewed enough of the basics to write some useful code! In the next lesson we will be looking at a [practical example](/post/beginning-programming-lesson-08).
