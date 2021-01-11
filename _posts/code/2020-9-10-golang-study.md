---
layout: blog
comments: true
code: true
title:  "Golang Learning"
tags:
- Golang
background-image: https://cdn.learnku.com/uploads/images/201805/11/1/ZnrA2VK0SN.png!/both/400x400
date:   2020-09-10 18:11:54
category: code
---
```
package main

import (
	"errors"
	format "fmt"
	"math/rand"
	"strconv"
	"sync"
)

// single way channel
type Sender chan<- int
type Receiver <-chan int

type Animal interface {
	Grow()
	Move(string) string
}

type Cat struct {
	Name string
	Age int
	Status string
}

func (cat *Cat) Grow() {

}

func (cat *Cat) Move(x string) string {
	return "nil"
}

func main() {

	// variable
	var x int = 3

	// array
	var numbers [2]int
	numbers[0] = 0
	numbers[1] = 1
	// alternative initialize method
	var ori = [5]int{1, 2, 3, 4, 5}
	tmpSlice := ori[2:]
	format.Print("xxx")
	format.Print(x)
	format.Print(len(numbers))
	format.Print(tmpSlice)

	// slice
	var numbers3 = [5]int{1, 2, 3, 4, 5}
	slice3 := numbers3[2 : len(numbers3) - 1]
	length := (2) // length is the size
	capacity := (3) // capacity is the span from the starting index of the index to the final index of the original array
	format.Printf("%v, %v\n", (length == len(slice3)), (capacity == cap(slice3)))

	// map
	mm2 := map[string]int{"golang": 42, "java": 1, "python": 8}
	mm2["scala"] = 25
	mm2["erlang"] = 50
	mm2["python"] = 0
	format.Printf("%d, %d, %d \n", mm2["scala"], mm2["erlang"], mm2["python"])

	// channel
	ch2 := make(chan string, 1)
	go func() {
		ch2 <- ("Arrived!")
	}()
	var value string = "Data"
	getFromChannel, ok := <- ch2
	value = value + (getFromChannel) + strconv.FormatBool(ok)
	format.Println(value)
	// declare buffer as 0 to make a block channel
	var myChannel = make(chan int, (0))
	var number = 6
	var wg sync.WaitGroup
	wg.Add(2)
	go func() {
		var sender Sender = myChannel
		sender <- number
		format.Println("Sent!")
		wg.Done()
	}()
	go func() {
		var receiver Receiver = myChannel
		format.Println("Received!", <-receiver)
		wg.Done()
	}()
	wg.Wait()
	// until 'Received', the 'Sent' go routine stops

	myCat := Cat{"Little C", 2, "In the house"}
	// type assert
	animal, ok := interface{}(&myCat).(Animal)
	format.Printf("%v, %v\n", ok, animal)

	// logic control
	var num int = 5
	if num += 4; 10 > num {
		// 标识符重声明
		var num int
		num += 3
		format.Print(num) // print 3 here
	} else if 10 < num {
		num -= 2
		format.Print(num)
	}
	// inner num variable life cycle ends, here print the outer num variable, 5 + 4 = 9
	format.Println(num) // print 9 here

	// slice of empty interface, which could be used to refer to any variable
	ia := []interface{}{byte(6), 'a', uint(10), int32(-4)}
	switch v := ia[rand.Intn(4) % 2]; interface{}(v).(type) {
	case rune:
		format.Println("Case A.")
	case byte :
		format.Println("Case B.")
	default:
		format.Println("Unknown!")
	}

	// iteration
	map1 := map[int]string{1: "Golang", 2: "Java", 3: "Python", 4: "C"}
	for k, v := range(map1) {
		format.Printf("%d: %s\n", k, v)
	}

	// select clause combine with case to control channel behavior
	selectTest()

	// defer clause, make the referring statement to be executed at the end of the method
	// if multiple statements, should be executed from down to top
	downTop(3)
	//time.Sleep(time.Second)
	format.Println("Enter main")
	// recover must run with defer
	defer globalRecover()
	outerFunc()
	format.Println("Quit main")

	/**
	缓冲的channel：保证往缓冲中存数据先于对应的取数据，简单说就是在取的时候里面肯定有数据，否则就因取不到而阻塞。所以缓冲的channel可能在取数据的时候发生阻塞。
	非缓冲的channel：保证取数据先于存数据，就是保证存的时候肯定有其他的goroutine在取，否则就因放不进去而阻塞。所以非缓冲的channel可能在存数据的时候会发生阻塞。
	 */

}

func downTop(n int) {
	for i := 0; i < n; i++ {
		defer format.Println("down top" + strconv.Itoa(i))
	}
}

func selectTest() {
	ch4 := make(chan int, 1)
	for i := 0; i < 4; i++ {
		select {
		case e, ok := <-ch4:
			if !ok {
				format.Println("End.")
				return
			}
			format.Println(e)
			// close the channel to make
			// e, ok := <-ch4:
			// ok = false
			close(ch4)
		default:
			format.Println("No Data!")
			ch4 <- 1
		}
	}
}

func innerFunc() {
	format.Println("Enter innerFunc")
	panic(errors.New("Occur a panic!"))
	format.Println("Quit innerFunc")
}

func outerFunc() {
	format.Println("Enter outerFunc")
	innerFunc()
	format.Println("Quit outerFunc")
}

func globalRecover() {
	if p := recover(); p != nil {
		format.Printf("Fatal error: %s\n", p)
	}
}
```