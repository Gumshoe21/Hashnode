---
title: "A Quirk Regarding Type Assertions and Switch Statements in Go"
seoTitle: "A Quirk Regarding Type Assertions and Switch Statements in Go"
seoDescription: "During my journey of learning Go, I've come across a confusing yet interesting quirk regarding type assertions."
datePublished: Tue Jan 02 2024 03:56:56 GMT+0000 (Coordinated Universal Time)
cuid: clqvtimlj000208guh5fp6cfb
slug: a-quirk-regarding-type-assertions-and-switch-statements-in-go
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1704167912990/a2d4e9ae-7f9f-49cd-9d57-c60f7ef82538.webp
tags: software-development, go, programming, learning, coding

---

During my journey of learning Go, I've come across a confusing yet interesting quirk regarding type assertions.

I've learned that when running a `switch` statement on a value's type, **you can match a type that is different than the value's declared type if the value's underlying type satisfies the matched type.**

That might have been a mouthful, but I'll explain exactly what I mean in great detail.

Take the following code for example:

```go
package main

import "fmt"

type Color struct {
	name string
}

type Join interface {
	Mix(o Color)
}

type Combine interface {
	Mix(o Color)
}

func (c *Color) Mix(otherColor Color) {
	fmt.Printf("%v mixed with %v = %v-%v!", c, otherColor, c, otherColor)
}

func main() {
	var blue Combine = &Color{"blue"}

	switch v := blue.(type) {
	case Join: 
		fmt.Printf("%v is a Join.", v)
	case Combine: 
		fmt.Printf("%v is a Combine.", v)
	}
}
```

Here, we have a struct `Color` and two interfaces, `Join` and `Combine`, that both contain the same method signature, `Mix(o Color)`.

Below these definitions, we define a function `Mix` whose receiver type is `*Color`. This means that `*Color` satisfies both the `Join` and `Combine` interfaces due to its implementation of the `Mix` method.

Now, here's where things get confusing (and interesting):

```go
func main() {
	var blue Combine = &Color{"blue"}

	switch v := blue.(type) {
	case Join: 
		fmt.Printf("%v is a Join.", v)
	case Combine: 
		fmt.Printf("%v is a Combine.", v)
	}

}
```

What do you think the output of this code should be? This, right?

`&{blue} is a Combine`

Wrong! We get this instead:

`&{blue} is a Join`

But why? The answer lies in what I stated before:

"**You can match a type that is different than the value's declared type if the value's underlying type satisfies the matched type.**"

So let's go over this in greater detail:

Here are the facts:

* The value against whose type we're matching: `blue`
    
* The value's declared type: `Combine`
    
* The value's underlying type: `*Color`
    
* The type the value matched: `Join`
    

Due to the fact that the value's (`blue`) underlying type (`*Color`) satisfies the matched type (`Join`), it will match `Join` and not the expected type (i.e., the declared type `Combine`). It's really that simple.

I thought this was an interesting quirk and I'm doing my due diligence by documenting my learning process here so that others may avoid the headache of misunderstanding later on.

Happy coding!