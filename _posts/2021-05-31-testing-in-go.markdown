---
layout: post
title:  "Unit testing in Go"
date:   2021-05-31
categories: go
tags: go code
description: Unit testing is paramount to writing stable, production ready code. In this post, we will talk a bit about what unit tests should cover and table driven tests in Go. Let's make Go tests our best friends!
---

{% include read_time.html %}

Unit testing is paramount to writing stable, production ready code. In this post, we will talk a bit about what unit tests should cover and table driven tests in Go.

**Let's make Go tests our best friends!**

{% include tests_friends.html %}

## Basics
For the sake of this testing discussion, let's say we have created a new `Calculator` that prints out formatted results and we want to ensure that it is working as expected.
```go
type Calculator struct{}

func (c Calculator) Add(x, y int) string {
	if y > 0 {
		return fmt.Sprintf("%d+%d=%d", x, y, x+y)
	}

	return fmt.Sprintf("%d%d=%d", x, y, x+y)
}
```
Go provides inbuilt support for testing using by using the <a href="https://golang.org/pkg/testing/" target="_blank">testing package</a>.
A test is created by writing a function with a name beginning with `Test` and being followed by the name of the method under test. The test signature takes exactly one parameter from the `testing` package as seen below. 
```go
func TestAdd(t *testing.T)
```

The parameter `t` that is provided in the function signature is used to signal errors to the test runner by using `Fail` and `Errorf`. One simple test for our `Add` function can be:
```go
func TestAdd(t *testing.T) {
    want := "2+3=5"
    c := Calculator()
    got := c.Add(2,3)
    if got != want {
        t.Errorf("Add(2,3) = %s; want %s", got, want)
    }
}
```
## The unit under test
Unit testing aims to test individual units or components of a piece of software. The purpose is to validate that each unit of the code performs as expected. In other words, unit tests are for verifying independent behaviours that we want to expose to outside of the component under test. Functions and structs are exported from Go **packages** so unit tests should verify the functionality that your packages expose.

In Go, tests live together with the source code in files that match the name of the source code file and ending in `_test`. While not enforced, Go also encourages tests to live in a separate `_test` package which has the dual purpose of making sure test functions do not leak into source code as well ensuring that only exported functions/behaviours are available in the test code. (*Note:* Go does not allow multiple packages to live in the same directory, but `_test` packages are exempt from this rule.)

The eagle eyed readers might have noticed that the packages in the `Calculator` example conveniently left out package names. We can now revisit this example and include our packages. The `Calculator` can now live in the `nice_math` package in a `calculator.go` file.
```go
package nice_math

import (
    "fmt"
) 

type Calculator struct{}

func (c Calculator) Add(x, y int) string {
	if y > 0 {
		return fmt.Sprintf("%d+%d=%d", x, y, x+y)
	}

	return fmt.Sprintf("%d%d=%d", x, y, x+y)
}
```

The test can live in the `nice_math_test` package in a `calculator_test.go` file. The test then imports anything it uses from the `nice_math` package explicitly. 

```go
package nice_math_test

import (
    "fmt"
    "github.com/addetz/nice_math"
)

func TestAdd(t *testing.T) {
    want := "2+3=5"
    c := nice_math.Calculator()
    got := c.Add(2,3)
    if got != want {
        t.Errorf("Add(2,3) = %s; want %s", got, want)
    }
}
```
## Table driven tests
**Table driven tests** have been growing in popularity amongst Go developers. This technique allows the same test setup to cover a variety of scenarios and ensure that the code is working as expected. 

The outline of a table driven test is: 
- Declare a test case structure that holds inputs and expected outputs for the function. Optionally, a name for the test case can be stored as well in the structure as well
- Construct an list/map with the test cases we want to test. If using a map, the name of the function can be the map key. *Note:* I, personally, prefer to add the name of the test in the test case structure as it's easier to understand conceptually than using the map key for the test name
- Range over the list/map of test cases and run any assertions necessary in a subtest using `t.Run`

We can now expand our previous test to cover all of the logic of our calculator `Add` method to test out those negative inputs that we had not looked at until now
```go
package nice_math_test

import (
    "fmt"
    "github.com/addetz/nice_math"
)

func TestAdd(t *testing.T) {
	tests := []struct {
		name string
		x, y int
		want string
	}{
		{name: "two positives", x: 1, y: 2, want: "1+2=3"},
		{name: "one positive one negative", x: 1, y: -2, want: "1-2=-1"},		
		{name: "two negatives", x: -1, y: -2, want: "-1-2=-3"},
	}
	c := nice_math.Calculator{}

	for _, tc := range tests {
		t.Run(tc.name, func(t *testing.T) {
			got := c.Add(tc.x, tc.y)
			if got != tc.want {
				t.Errorf("got %s, want %s", got, tc.want)
			}
		})
	}
}
```

Subtests allow an itemised view of each test and their outcome, making it easy to detect and fix failures. 
```
=== RUN   TestAdd
=== RUN   TestAdd/two_positives
=== RUN   TestAdd/one_positive_one_negative
=== RUN   TestAdd/two_negatives
--- PASS: TestAdd (0.00s)
    --- PASS: TestAdd/two_positives (0.00s)
    --- PASS: TestAdd/one_positive_one_negative (0.00s)
    --- PASS: TestAdd/two_negatives (0.00s)
PASS

All tests passed.
```

## Assertions and libraries
When it comes to testing, there are a few libraries that are worth mentioning: 
- The <a href="https://golang.org/pkg/testing/" target="_blank">testing package</a> comes with the standard library. It is easy to start with, but it quickly becomes tedious and repetitive when used on more complex code. It also provides the ability to write benchmarks for our functions as well
- The <a href="https://github.com/stretchr/testify" target="_blank">testify library</a> provides easy to use assertions which come in super handy when asserting on objects. It also provides mocks for substituting dependencies (I've not really talked about mocks in this little post, as they deserve their very own discussion)
- The <a href="https://github.com/onsi/ginkgo" target="_blank">ginkgo library</a> provides BDD style tests using the given/when/then pattern and is usually used with the <a href="https://github.com/onsi/gomega" target="_blank">gomega matcher library</a>

## Parting words
Unit testing in Go is easy when making use of table driven tests to test the behaviours that your Go packages export. The `testing` package allows us to write subtests and write assertions for our code, allowing one test to cover multiple scenarios, while keeping traceability in test results.

Other libraries such as `testify` and `ginkgo` provide synctactic sugar on top of the standard testing package, but many voices in the Go community promote writing simple functions and tests, for which the `testing` package should be sufficient. 

I frequently use the `testify` library for its assertions and mocks, but the structure of your tests and what they cover is more important than the library you choose for testing. I would definitely recommend starting out with just the standard testing library, keeping things simple for as long as possible and then moving to other alternatives as the need arises.

**Happy Go coding!**