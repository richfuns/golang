## **empty interface空接口**

---

前面学习的变量都要在声明时候确定类型，虽然用:=可以不用指定类型，但实际是类型推倒，在赋值后类型就确定下来不能改了。

空接口的作用：不用指定类型变量，并且类型可变。比如：

```text
var i interface{}
i = 42	//这个时候i就是int类型
i = "hello"	//这个时候i就是string类型
```

!!! note
	interface{}这么写比较方便，当然也写成2行：

	```text
	type I interface {
	}
	```

完整例子

```go
package main

import "fmt"

type I interface{}

func main() {
	var i I
	i = 42 //这个时候i就是int类型
	fmt.Printf("%v,%T\n", i, i)
	i = "hello" //这个时候i就是string类型
	fmt.Printf("%v,%T\n", i, i)
}
```

输出

```text
42,int
hello,string
```

## **空接口对切片的影响**

---

1. 若将一个array或slice赋值给空接口，这个空接口无法再进行切片

2. array或slice赋值给空接口的行为不是复制，而是类似指针效果，只不过无法再进行切片，但元素和原来的array、slice及其衍生的，都有关联

```go
package main

import "fmt"

func main() {
	sli := []int{2, 3, 5, 7, 11, 13}

	var e interface{}
	e = sli

	f := sli[0:3]
	f[2] = 55

	fmt.Printf("%T,%v\n", sli, sli)
	fmt.Printf("%T,%v\n", e, e)
	fmt.Printf("%T,%v\n", f, f)
}
```

输出

```text
[]int,[2 3 55 7 11 13]
[]int,[2 3 55 7 11 13]
[]int,[2 3 55]
```

若改为

```text
package main

import "fmt"

func main() {
	sli := []int{2, 3, 5, 7, 11, 13}

	var e interface{}
	e = sli

	g := e[1:3]
	fmt.Println(g)
}
```

报错

```text
./example.go:11: cannot slice e (type interface {})
```