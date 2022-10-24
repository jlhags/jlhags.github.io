---
title: "Beginning Programming Lesson 04"
subtitle: "Control Structures and Array-Like Things"
date: 2022-10-19T11:13:23-07:00
draft: true
tags: [go, golang, beginning programming, control structures, arrays, slices, for loop, switch statement, if else ]
---

We now know how to create and use variables, structs, and functions. But thus far, all the examples have been fairly linear. What if we want do things only when certain criteria are met? Or what if we want to do the same thing many times in a row? What if we want want store a bunch of a rectangles from the previous lesson? This is where control structures, arrays and slices come in handy. You can download this lessons code [here](https://raw.githubusercontent.com/jlhags/Beginning_Programming_In_Go/main/Lesson_02/main.go).
```go
package main
import "fmt"

func main() {
	var s1 []int
	s2 := []int{4, 5, 6}

	fmt.Println(s1)
	fmt.Println(s2)

	fmt.Printf("s1 Length: %d\n", len(s1))
	fmt.Printf("s2 Length: %d\n", len(s2))

	s1 = append(s1, 1)
	fmt.Printf("s1 Length: %d\n", len(s1))
	s1 = append(s1, 2)
	s1 = append(s1, 3)
	fmt.Println(s1)

	fmt.Println(s1[1])
	fmt.Println(s1[0])
	fmt.Printf("Max s2: %d\n", max(s2))

	p := polygon{Sides: []float64{1, 1, 1.4142}}

	fmt.Printf("Perimeter: %f\n", p.Perimeter())
	fmt.Println(p.Type())

	p2 := polygon{Sides: []float64{1, 1, 1, 1, 1, 1, 1}}
	fmt.Println(p2.Type())
}

func max(s []int) int {
	var ret int
	// for i := 0; i < len(s); i++ {
	// 	if s[i] > ret {
	// 		ret = s[i]
	// 	}
	// }
	for i, v := range s {
		if v > ret {
			ret = v
		}
	}
	return ret
}

type polygon struct {
	Sides []float64
}

func (p polygon) Perimeter() float64 {
	var ret float64

	for _, v := range p.Sides {
		ret += v
	}
	return ret
}

func (p polygon) IsRegular() bool {
	ret := true

	if len(p.Sides) < 3 {
		ret = false
	} else {
		for i := 1; i < len(p.Sides); i++ {
			if p.Sides[i-1] != p.Sides[i] {
				ret = false
				break
			}
		}
	}

	return ret
}

func (p polygon) Type() string {
	var ret string
	switch len(p.Sides) {
	case 3:
		ret = "triangle"
	case 4:
		ret = "quadrangle"
	case 5:
		ret = "pentagon"
	default:
		ret = "unknown"
	}
	return ret
}
```

## Arrays and Slices
You can think of arrays and slices as variables that are a list of values. If you wanted to store a grocery list, you may want to use an array or slice of type string. But what are the differences between the two?

Arrays in Golang are of a predetermined length. If we use less space than what was requested, the extra space is wasted. If we need more space, we need to create a new variable. Slices on the other hand, can grow or shrink depending on how much space you need. Which one you use will depend on the use case.

### Defining an Array
You can define array as follows:
```go
var a [10] int
```
Here we created an array of 10 integers, each one being set to 0, the default value

### Defining a Slice
You can define a slice as follows:

```go
var s [] int
```
Here we created a slice of integers. Since nothing has been assigned to it, it currently holds no values.
Accessing Elements of an Array or Slice
If you wanted to access a specific element of an array or slice:

```go
s2 := []int{4, 5, 6}
fmt.Println(s2[2])
```

In this case the value in position 2 of s2 would be printed. Note that I didn't say the 2nd element. Most programming languages start counting at 0 (Article on why to come later). So, 6 would be printed, not 5.

### Getting the Length of a Slice
Since slices can change in length, it will be useful to know just how long it is. For this, we can use the function len:
```go
fmt.Printf("s1 Length: %d\n", len(s1))
```
Here were are printing out the length of s1. The len function can be used for more that just slices too. For example you can use it to get the number of characters in a string. For more info see here.

### Boolean Logic
We need to spend a moment to talk about Boolean Logic, as most of the control structures to follow will use them to some degree. Think of it as a logic formula that results in either true or false. You use them everyday without realizing it. For instance if you were trying to determine if you needed an umbrella or not you would consider, "Is it raining and will I be going out side?" If both parts of that sentence are true, then the whole statement is true and you should have an umbrella. You can read more about it [here](https://en.wikipedia.org/wiki/Boolean_algebra). If that is too confusion, let me know in the comments and I will write an article in it.

In terms of Golang we need to know at least few comparative and logical operators.

#### Comparative Operators
These are symbols used to compare to things.
  
| Symbol	| Meaning |
| ----------- | ----------- |
| == | test equality |
| > | greater than |
| < | less than |

Note to test equality it is a double equals. Using a single "=" in a condition can produce unexpected results.

#### Logical Operators
These are symbols to represent the boolean logic operators "AND", "OR", and "NOT" 

| Symbol	| Meaning |
| ----------- | ----------- |
| && | AND |
| \|\| |	OR |
| !	| NOT |

## Control Structures
Control structures define how the code flows. Since we just talked about arrays, lets looks at some control structures that allow for "looping" through the elements of an array
### for Loops
Many languages have for loops or at least something like them. Most for loops follow the same structure:
for(<initialization of a variable>;<condition in which loop should continue>;<action to perform at end of iteration>). For example:

```go
s:=[]int{1,2,3,4,5,6}
for i := 0; i < len(s); i++ {
    fmt.Printf("%d:%d\n",i,s[i])

}
```
The initialization part is creating a variable `i` and giving it a value of 0. As long as `i` is less than the length of the slice s we should continue looping. At the end of each iteration we add 1 to i (`i++` is shorthand for `i = i+1`). In Golang it is not required that all components of the for loop need to be there. In fact, none of them need to be there:

```go
for ; ; {
    fmt.Println("this will get printed over and over indefinitely")
}	
```

The example above would cause an "infinite loop" meaning once the program got here, it would be stuck here forever, until program is forced to close. Why would we want to do this? Well we probably wouldn't at least not as is. Maybe the condition for determining when to stop is not easily captured in the for loop conditional. Another way to end a for loop is by using the `break` command.
```go
for ; ; {
   fmt.Println("This only gets printed once")
   break
}
```

There is one more example of a for loop that is particularly useful when looping through arrays/slices:

```go
s:=[]int{1,2,3,4,5,6}
for i, v := range s {
	fmt.Printf("%d:%d\n",i,v)
}
```
This would effectively do the same thing as our first for loop example. range returns 2 values, the index, and the value at that index. If for some reason you didn't need the index, you can assign it to `_`, meaning ignore it:
```go
s:=[]int{1,2,3,4,5,6}
for _, v := range s {
	fmt.Printf("d\n",v)
}
```

### if-else
What if you wanted to only execute certain bit of code if a condition was true? 

```go
if v > ret {
    ret = v
}
```

In this case ret will get set to `v` only if the current value of `ret` is less than `v`. That great but what if we wanted to do something different if it is not true

```go
if v > ret {
    ret = v
} else {
    //do something else
}
```
We can also chain ifs and elses together:
```go
    if [some condition] {
	ret = v
    } else if [some other condition] {
        //do something else
    } else {
        // do another thing
    }
```
## switch
Switch statements are great for when you want execute something based on the value of something else. You could achieve it by creating a long if-else-if-else chain or:

```go
func (p polygon) Type() string {
	var ret string
	switch len(p.Sides) {
	case 3:
		ret = "triangle"
	case 4:
		ret = "quadrangle"
	case 5:
		ret = "pentagon"
	default:
		ret = "unknown"
	}
	return ret
}
```

Here the polygon `Type()` method gets the number of sides then based on that value returns the name of the polygon. Let's look at this as a series of if-else-if-else:

```go
func (p polygon) Type() string {
	var ret string
        l := len(p.Sides)
        if l == 3{
            ret = "triangle"
        } else if l == 4 {
            ret = "quadrangle"
        } else if l == 5 {
            ret = "pentagon"
        } else {
            ret = "unknown"
        }
	return ret
}
```

## Conclusion
Now we have enough tools at our disposal to create mildly useful programs. Take a look at the example code for today. How might you improve it? What is `IsRegular()` doing? If you had a method to calculate the perimeter, could you utilize it in in `IsRegular()`? Could you rewrite the Type() method without using switch and only one if-else clause? 

In our next lesson we will cover Interfaces, Inheritance, and Type Assertion.
