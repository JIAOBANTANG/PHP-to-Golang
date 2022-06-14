# PHP2GO
* [定义变量－Variables](#定义变量variables)
* [输出－Echo](#输出echo)
* [函数－Function](#函数function)
    * [多返回值－Multiple Value](#多返回值multiple-value)
* [匿名函数－Anonymous Function](匿名函数anonymous-function)
* [复合类型－Stores](#复合类型stores)
    * [数组－Array](#数组array)
    * [切片－Slice](#切片slice)
    * [映射－Map](#映射map)
    * [接口－Interface](#接口interface)
* [不定值－Mixed Type](#不定值mixed-type)
* [延迟处理－Defer](#延迟处理defer)
* [跳转－Goto](#跳转goto)
* [循环－Loops](#循环loops)
    * [遍历－Foreach](#遍历foreach)
    * [While](#while)
    * [Do While](#do-while)
* [日期－Date](#日期date)
* [切割字串－Split](#切割字串split)
* [关联数组－Associative Array](#关联数组associative-array)
* [是否存在－Isset](#是否存在isset)
* [指针－Pointer](#指针pointer)
* [错误处理－Error Exception](#错误处理error-exception)
	* [抛出和捕获异常－Try & Catch](#抛出和捕获异常try--catch)
* [包／引入／导出－Package / Import / Export](#包引入导出package--import--export)
    * [包－Package](#包package)
    * [引入－Import](#引入import)
    * [导出－Export](#导出export)
* [类－Class](#类class)
   * [构造方法－Constructor](#构造方法constructor)
   * [继承－Embedding](#继承embed)
   * [封装－Shadowing](#封装shadowing)
   * [多态－Polymorphism](#多态polymorphism)


# 定义变量－Variables
#### PHP
php是弱类型语言，使用`$`符定义变量，php7之后可以声明变量类型。类型转换较为混乱
```php
$a string = "foo";
$b = "bar";
```

#### Golang
go是强类型语言，可省略类型会自动推导类型，布尔类型无法使用0和1替代
```go
// 直接定义
var a string = "foo"
// 类型推导
var a = "foo"
//多个变量
var (
    a = "foo"
    ...
)

// 函数体内可使用简写
c := "bar"
```

# 输出－Echo
#### PHP
php一般使用`Echo``print`语句输出基本类型，使用`print_r``var_dump`函数输出基本类型以及复合类型和特殊类型

```
```php
echo "Foo"; // 输出：Foo
$A = "Bar"
echo $A; // 输出：Bar
$B = "Hello"
echo $B . ", world!"; // 输出：Hello, world!
$C = [1, 2, 3];
echo var_dump($C); // 输出：array(3) {[0]=>int(1) [1]=>int(2) [2]=>int(3)}
```

#### Golang

然而在 Golang 中你会需要 `fmt` 包，关于「什么是包」的说明你可以在文章下述了解。

```go
fmt.Println("Foo") // 输出：Foo

A := "Bar"
fmt.Println(A) // 输出：Bar

B := "Hello"
fmt.Printf("%s, world!", B) // 输出：Hello, world!

C := []int{1, 2, 3}
fmt.Println(C) // 输出：[1 2 3]
```

# 函数－Function

#### PHP
可以很随意，也可以使用严格模式定义参数类型与返回值类型
```php
function test(string $a):string {
    return $a;
}

echo test(); // 输出：Hello, world!
```

#### Golang

必须定义参数类型与返回值类型

```go
func test() string {
    return "Hello, world!"
}

fmt.Println(test()) // 输出：Hello, world!
```

## 多返回值－Multiple Value

#### PHP
不支持多返回值，可返回数组使用list函数解构
```php
function test() {
    return ['username' => 'YamiOdymel', 
            'time'     => 123456];
}
$data = test();
echo $data['username'], $data['time']; // 输出：YamiOdymel 123456
//或
list($username,$time) = test();
```

#### Golang

支持返回多个值

```go
func test() (string, int) {
    return "YamiOdymel", 123456
}
username, time := test()

fmt.Println(username, time) // 输出：YamiOdymel 123456
```


# 匿名函数－Anonymous Function

#### PHP
同样的写法也可以定义匿名类
```php
$a = function() {
    echo "Hello, world!";
};

$a(); // 输出：Hello, world!
```

#### Golang

```go
a := func() {
    fmt.Println("Hello, world!")
}
	
a() // 输出：Hello, world!
```

# 复合类型－Stores

主要是 PHP 的数组能做太多事情了，所以在 PHP 里面要存储什么用数组就好了。

#### PHP

```php
$array  = [1, 2, 3, 4, 5];
$array2 = ['username' => 'YamiOdymel', 
           'password' => '2016 Spring'];
```

在 Golang 里⋯⋯沒有这么万能的东西，首先要先了解 Golang 中有这些类型：`array`, `slice`, `map`, `interface`，

## 数组－Array

> 一个存放固定长度的数组。

先撇开 PHP 的「万能数组」不管，Golang 中的数组很单纯，在定义一个数组的时候，你你必须给一个长度还有其內容存放的数据类型，你的数组內容不一定要填满其长度，但是你的数组內容不能超过你当初定义的长度。

#### PHP

```php
$a = ["foo", "bar"];

echo $a[0]; // 输出：foo
```

#### Golang

```go
var a [2]string

a[0] = "foo"
a[1] = "bar"

fmt.Println(a[0]) // 输出：foo
```

## 切片－Slice

> 可供「裁切」而且供自由扩展的数组。

切片⋯⋯这听起來也许很奇怪，但是你确实可以「切」他，让我们先谈谈「切片」比起「数组」要好在哪里：「你不用定义其最大长度，而且你可以直接赋值」。

#### PHP

```php
$a = ["foo", "bar"];

echo $a[0]; // 输出：foo
```

#### Golang

```go
a := []string{"foo", "bar"}

fmt.Println(a[0]) // 输出：foo
```

我们刚才有提到你可以「切」他，记得吗？这有点像是 PHP 中的 `array_slice()`，但是 Golang 直接让 Slice「内建」了这个用法，其用法是：`slice[开始:结束]`。

#### Golang

```go
p := []int{1, 2, 3, 4, 5, 6}

fmt.Println(p[0:1]) // 输出：[1]
fmt.Println(p[1:1]) // 输出：[]  （！注意这跟 PHP 不一样！）
fmt.Println(p[1:])  // 输出：[2, 3, 4, 5, 6]
fmt.Println(p[:1])  // 输出：[1]
```

在 PHP 中倒是没有那么方便，在下列 PHP 案例中你需要不断地使用 `array_slice()`。

#### PHP

```php
$p = [1, 2, 3, 4, 5, 6];

echo array_slice($p, 0, 1); // 输出：[1]
echo array_slice($p, 1, 1); // 输出：[2]
echo array_slice($p, 1);    // 输出：[2, 3, 4, 5, 6]
echo array_slice($p, 0, 1); // 输出：[1]
```

## 映射－Map

> 有键名和键值的数组。

你可以把「映射」看成是有键名和键值的数组，但是记住：「你需要事先定义键名、键值的数据类型」，这仍限制你没办法在映射中存放多种不同数据的类型。

#### PHP

```php
$data["username"] = "YamiOdymel";
$data["password"] = "2016 Spring";

echo $data["username"]; // 输出：YamiOdymel
```

#### Golang

在 Golang 里可就没这么简单了，你需要先用 `make()` 宣告 `map`。

```go
data := make(map[string]string)

data["username"] = "YamiOdymel"
data["password"] = "2016 Spring"

fmt.Println(data["username"]) // 输出：YamiOdymel
```

## 接口－Interface

> 接口；一個可存放多中数据类型的数组。

这跟php面向对象里的接口有冲突

#### PHP

```php
$mixedData  = ["foobar", 123456];
$mixedData2 = ['username' => 'YamiOdymel', 
               'time'     => 123456];
```

#### Golang

现在你有福了！正因为 Golang 中的 `interface{}` 可以接受任何內容，所以你可以把它拿來存放任何数据类型。

```go
mixedData := []interface{}{"foobar", 123456}

mixedData2 := make(map[string]interface{})
mixedData2["username"] = "YamiOdymel"
mixedData2["time"]     = 123456
```

# 不定值－Mixed Type

未初始值的变量，在 PHP 里你可以直接一个变量定义成字符串、数值、空值。

#### PHP

```php
$mixed = 123;
echo $mixed; // 输出：123

$mixed = 'Moon, Dalan!';
echo $mixed; // 输出：Moon, Dalan!

$mixed = ['A', 'B', 'C'];
echo $mixed; // 输出：['A', 'B', 'C']
```

#### Golang

在 Golang 中你必须给变量一个类型，不过还记得刚才提到的：「Golang 中有个 `interface{}` 能够**存放任何事物**」？

```go
var mixed interface{}

mixed = 123
fmt.Println(mixed) // 输出：123

mixed = "Moon, Dalan!"
fmt.Println(mixed) // 输出：Moon, Dalan!

mixed = []string{"A", "B", "C"}
fmt.Println(mixed) // 输出：["A", "B", "C"]
```

# 延迟处理－Defer

当我们程序中不需要继续使用到某个资源或是发生错误的时候，我们索性会将其关闭或是抛弃来节省资源开销，例如 PHP 里的读取文档：

#### PHP

```php
$handle = fopen('example.txt', 'r');

if($errorA)
    errorHandlerA();

if($errorB)
    errorHandlerB();

fclose($handle); // 关闭文档
```

#### Golang

在 Golang 中，你可以使用 `defer` 来在函数结束的时候自动执行某些程序(其执行的方向为反向)。所以你就不需要在函数最后面结束最前面的资源。

`defer` 可以被称为「延迟执行」，实际上就是在函数结束后会「反序」执行的东西，例如你按照了这样的顺序定义 `defer`： `A->B->C->D`，那么执行的顺序其实会是 `D->C->B->A`，這这用在程序结束时还蛮有用的，让我们看看 Golang 如何编写上述案例。

```go
handle := file.Open("example.txt")
defer file.Close() // 关闭文档但「延迟执行」，所有程序结束后才会执行到这里

if errorA {
    errorHandlerA()
}
if errorB {
    errorHandlerB()
}
```


# 跳转－Goto

跳转执行，在某些重试处理里很好用，与go语言写法基本一致

#### PHP

```php
goto a;
echo 'foo';
 
a:
echo 'bar'; // 输出：bar
```

#### Golang

```go
goto a
fmt.Println("foo")

a:
fmt.Println("bar") // 输出：bar
```


# 循环－Loops

Golang 中僅有 `for` 一种循环但却能够达成 `foreach`、`while`、`for` 多种用法。普通 `for` 循环写法在两种语言中都十分相近。

#### PHP

```php
for($i = 0; $i < 3; $i++)
    echo $i; // 输出：012
    
$j = 0;
for($j; $j < 5; $j++)
    echo $j; // 输出：01234
```

#### Golang

在 Golang 请记得：如果你的 `i` 先前并不存在，那么你就需要定义它，所以下面这个案例你会看见 `i := 0`。

```go
for i := 0; i < 3; i++ {
    fmt.Println(i) // 输出 012
}

j := 0
for ; j < 5 ; j++ {
    fmt.Println(j) // 输出：01234
}
```

## 遍历－Foreach

在 PHP 裡，`foreach()` 能夠直接給你值和鍵名，用起來十分簡單。

#### PHP

```php
$data = ['a', 'b', 'c'];

foreach($data as $index => $value)
    echo $index . $value . '|' ; // 输出：0a|1b|2c|

foreach($data as $index => $value)
    echo $index . '|' ; // 输出：0|1|2|
    
foreach($data as $value)
    echo $value . '|' ; // 输出：a|b|c|
```

#### Golang

Golang 裡面雖然僅有 `for()` 但卻可以使用 `range` 達成和 PHP 一樣的 `foreach` 方式。

```go
data := []string{"a", "b", "c"}

for index, value := range data {
    fmt.Printf("%d%s|", index, value)  // 输出：0a|1b|2c|
}

for index := range data {
    fmt.Printf("%d|", index)  // 输出：0|1|2|
}

for _, value := range data {
    fmt.Printf("%s|", value)  // 输出：a|b|c|
}
```

## －While

一個 `while(條件)` 循环在 PHP 裡面可以不斷地執行區塊中的程序，直到 `條件` 為 `false` 為止。

#### PHP

```php
$i = 0;

while( $i < 3 ) {
    $i++;
    echo $i; // 输出：123
}

while(true)
    echo "WOW" // 输出：WOWWOWWOWWOWWOW...
```

#### Golang

在 Golang 裡也有相同的做法，但仍是透過 `for` 循环，請注意這個 `for` 循环並沒有任何的分號（`;`），而且一個沒有條件的 `for` 循环會一直被執行。

```go
i := 0

for i < 3 {
    i++
    fmt.Println(i) // 输出：123
}

for {
    fmt.Println("WOW") // 输出：WOWWOWWOWWOWWOW...
}
```

## 做 .. －Do While

PHP 中有 `do .. while()` 循环可以先做區塊中的動作。

#### PHP

```php
$i = 0;

do {
    $i++;
    echo $i; // 输出：123
} while($i < 3);
```

#### Golang

在 Golang 中則沒有相關函数，但是你可以透過一個無止盡的 `for` 循环加上條件式來讓他結束循环。

```go
i := 0

for {
    i++
    fmt.Println(i) // 输出：123
    
    // 注意這個條件式和 PHP 有所不同
    if i > 2 {
        break
    }
}
```

#### Golang

要是你真的希望完全符合像是 PHP 那樣的設計方式，或者你可以在 Golang 中使用很邪惡的 `goto`。

```go
i := 0

LOOP:
    i++
    fmt.Println(i) // 输出：123
    
    if i < 3 {
        goto LOOP
    }
```

&nbsp;

# 日期－Date

在 PHP 中我們可以透過 `date()` 像這樣取得目前的日期。

#### PHP

```php
echo date("Y-m-d H:i:s"); // 输出：2016-07-13 12:59:59
```

#### Golang

在 Golang 就稍微有趣點了，因為 Golang 中並不是以 `Y-m-d` 這種格式做為定義，而是 `1`、`2`、`3`，這令你需要去翻閱文件，才能夠知道 `1` 的定義是代表什麼。

```go
fmt.Println(time.Now().Format("2006-2-1 03:04:00"))          // 输出：2016-07-13 12:59:59
fmt.Println(time.Now().Format("Mon, Jan 2, 2006 at 3:04pm")) // 输出： Mon, Jul 13, 2016 at 12:59pm
```

&nbsp;

# 切割字串－Split

俗話說：「爆炸就是藝術」，可愛的 PHP 用詞真的很大膽，像是：`explode()`（爆炸）、`die()`（死掉），回歸正傳，如果你想在 PHP 裡面將字串切割成数组，你可以這麼做。

#### PHP

```php
$data  = 'a, b, c, d';
$array = explode(', ', $data);
```

#### Golang

簡單的就讓一個字串給「爆炸」了，那麼 Golang 呢？

```go
data  := "a, b, c, d"
array := strings.Split(data, ", ")
```

對了，記得引用 `strings` 包。

&nbsp;

# 关联数组－Associative Array

這真的是很常用到的功能，就像物件一樣有著鍵名和鍵值，在 PHP 裡面你很簡單的就能靠数组（Array）辦到。

#### PHP

```php
$data = ['username' => 'YamiOdymel',
         'password' => '2016 Spring'];
         
echo $data["username"]; // 输出：YamiOdymel
```

#### Golang

真是太棒了，那麼 Golang 呢？用 `map` 是差不多啦。如果有必要的話，你可以稍微複習一下先前提到的「复合类型－Stores」。

```go
data := map[string]string{
           "username": "YamiOdymel", 
           "password": "2016 Spring"}

fmt.Println(data["username"]) // 输出：YamiOdymel
```

&nbsp;

# 是否存在－Isset

你很常會在 PHP 裡面用 `isset()` 檢查一個索引是否存在，不是嗎？

#### PHP

```php
// 如果 $data['username'] 存在
if(isset($data['username'])) {
    $username = $data['username'];
}
```

#### Golang

在 Golang 裡面很簡單的能夠這樣辦到（僅適用於 `map`）。

```go
username, exists := data["username"]

if !exists {
    fmt.Printf("你要找的資料不存在。")
}
```
&nbsp;

# 指针－Pointer

指针（有時也做參照）是一個像是「變數別名」的方法，這種方法讓你不用整天覆蓋舊的變數，讓我們假設 `A = 1; B = A;` 這個時候 `B` 會複製一份 `A` 且兩者不相干，倘若你希望修改 `B` 的時候實際上也會修改到 `A` 的值，就會需要指针。

指针比起複製一個變數，他會建立一個指向到某個變數的記憶體位置，這也就是為什麼你改變指针，實際上是在改變某個變數。

#### PHP

```php
function zero(&$number) { // & 即是指针
    $number = 0;
}

$A = 5;
zero($A);

echo $A; // 输出：0
```

#### Golang

在 Golang 你需要用上 `*` 還有 `&` 符號。

```go
func zero(number *int) {
    number = 0
}

func main() {
    A := 5;
    zero(&A)

    fmt.Printf("%d", A) // 输出：0
}
```

&nbsp;

# 错误处理－Error Exception

有些時候你會回傳一個数组，這個数组裡面可能有資料還有錯誤代號，而你會用條件式判斷錯誤代號是否非空值。

#### PHP

```php
function foo($number) {
    if($number !== 1)
        return ['number' => -1, 
                'error'  => '$number is not 1'];
    
    return ['number' => $number, 
            'error'  => null];
}

$bar = foo(0);

if($bar['error'])
    echo $bar['number'], $bar['error']; // 输出：-1
                                        //      $number is not 1
```

#### Golang

在 Golang 中函数可以一次回傳多個值。為此，你不需要真的回傳一個数组，不過要注意的是你將會回傳一個屬於 `error` 資料型態的錯誤，所以你需要引用 `errors` 包來幫助你做這件事。

該注意的是 Golang 沒有 `try .. catch`，因為 **Golang 推薦這種错误处理方式**，你應該在每一次執行可能會發生錯誤的程序時就處理錯誤，而非後來用 `try` 到處包覆你的程序。

```go
import "errors"

func foo(number int) (int, error) {
    if number != 1 {
        return -1, errors.New("$number is not 1")
    }
    return number, nil
}

if bar, err := foo(0); err != nil {
    fmt.Println(bar, err) // 输出：-1
                          //      $number is not 1
}
```

在 `if` 條件式裡宣告變數會讓你只能在 `if` 內部使用這個變數，而不會污染到全域範圍。

## 抛出和捕获异常－Try & Catch

也許你在 PHP 中更常用的會是 `try .. catch`，在大型商業邏輯時經常看見如此地用法，實際上這種用法令人感到聒噪（因為你會需要一堆 `try` 區塊）：

* [Too many try/catch block for PDO](http://stackoverflow.com/questions/7620305/too-many-try-catch-block-for-pdo)
* [Too many try/catch blocks. Is this proper?](http://stackoverflow.com/questions/23295953/too-many-try-catch-blocks-is-this-proper)
* [Is this too many lines and too many nested blocks?](http://stackoverflow.com/questions/7620305/too-many-try-catch-block-for-pdo)

#### PHP

```php
function foo($number) {
    if($number < 10)
        throw new Exception('$number is less than 10');
    else if($number > 10)
        throw new Exception('$number is greater than 10');
}

try {
    foo(9);
} catch(Exception $e) {
    echo $e->getMessage(); // 输出：$number is less than 10
}

try {
    foo(11);
} catch(Exception $e) {
    echo $e->getMessage(); // 输出：$number is greater than 10
}
```

#### Golang

Golang 中並沒有 `try .. catch`，實際上 Golang 也**不鼓勵這種行為**（Golang 推薦逐一處理錯誤的方式），倘若你真想辦倒像是捕捉異常這樣的方式，你確實可以使用 Golang 中另類處理錯誤的方式（可以的話盡量避免使用這種方式）：`panic()`, `recover()`, `defer`。

你可以把 `panic()` 當作是 `throw`（丟出錯誤），而這跟 PHP 的 `exit()` 有 87% 像，一但你執行了 `panic()` 你的程序就會宣告而終，但是別擔心，因為程序結束的時候會呼叫 `defer`，所以我們接下來要在 `defer` 停止 `panic()`。

關於 `defer` 上述已經有提到了，他是一個反向執行的宣告，會在函数結束後被執行，當你呼叫了 `panic()` 結束程序的時候，也就會開始執行 `defer`，所以我們要在 `defer` 內使用 `recover()` 讓程序不再繼續進行結束動作，這就像是捕捉異常。

`recover()` 可以看作 `catch`（捕捉），我們要在 `defer` 裡面用 `recover()` 解決 `panic()`，如此一來程序就會回歸正常而不會被結束。

```go
// 建立一個模仿 try&catch 的函数供稍後使用
func try(fn func(), handler func(interface{})) {
    // 這不會馬上被執行，但當 panic 被執行就會結束程序，結束程序就必定會呼叫 defer
    defer func() { 
        // 透過 recover 來從 panic 狀態中恢復，並呼叫捕捉函数
        if err := recover(); err != nil {
            handler(err)
        }
    }()
    // 執行可能帶有 panic 的程序
    fn()
}

func foo(number int) {
    if number < 10 {
        panic("number is less than 10")
    }
    if number > 10 {
        panic("number is greater than 10")
    }
}

func main() {
    try(func() {
        foo(9)
    }, func(e interface{}) {
        fmt.Println(e) // 输出：number is less than 10
    })
    
    try(func() {
        foo(11)
    }, func(e interface{}) {
        fmt.Println(e) // 输出：number is greater than 10
    })
}

```

&nbsp;

# 包／引入／导出－Package / Import / Export

還記得在 PHP 裡要引用一堆檔案的日子嗎？到處可見的 `require()` 或是 `include()`？到了 Golang 這些都不見了，取而代之的是「包（Package）」。現在讓我們來用 PHP 解釋一下。

#### PHP

```php
// a.php
<?php
    $foo = "bar";
?>
```
```php
// index.php
<?php
    include "a.php";
    
    echo $foo; // 输出：bar
?>
```

這看起來很正常對吧？但假設你有一堆檔案，這馬上就成了 `Include Hell`，讓我們看看 Golang 怎麼透過「包」解決這個問題。

#### Golang

```go
// a.go
package main

var foo string = "bar"
```

```go
// main.go
package main

import "fmt"

func main() {
    fmt.Println(foo) // 输出：bar
}
```

`main.go` 中除了引用 `fmt` 包（*為了要输出結果用的包*）之外完全沒有引用到 `a.go`。



既然沒有引用其他檔案，為什麼 `main.go` 可以输出 `foo` 呢？注意到了嗎，**兩者都是屬於 `main` 包**，因此**他們共享同一個區域**，所以接下來要介紹的是什麼叫做「包」。

## 包－Package

包是每一個 `.go` 檔案都必須聲明在 Golang 原始碼中最開端的東西，像下面這樣：

```go
package main
```

這意味著目前的檔案是屬於 `main` 包（*你也可以依照你的喜好命名*），那麼要如何讓同個包之間的函数溝通呢？

#### PHP

```php
// a.php
<?php
    function foo() {
        // ...
    }
?>
```
```php
// index.php
<?php
    include "a.php";
    
    foo();
?>
```

#### Golang

接著是 Golang；注意！你不需要引用任何檔案，因為下列兩個檔案同屬一個包。

```go
// a.go
package main

func foo() {
    // ...
}
```

```go
// main.go
package main

func main() {
    foo()
}
```

一個由「包」所掌握的世界，比起 PHP 的 `include()` 和 `require()` 還要好太多了，對嗎？

## 引入－Import

在 Golang 中沒有引用單獨檔案的方式，你必須引入一整個包，而且你要記住：「一定你引入了，你就一定要使用它」，像下面這樣。

```go
package main

import (
    "fmt"                           // 引用底層包
    "time"                          // 這也是底層包
    "github.com/yamiodymel/teameow" // 來自 Github 的 "teameow" 包
)

func main() {
    // 然後像下面這樣使用你剛引入的包
    fmt.XXX()
    time.XXX()
    teameow.XXX()
}
```

假如你不希望使用你引入的包，你只是為了要觸發那個包的 `main()` 函数而引用的話⋯⋯，那麼你可以在前面加上一個底線（`_`）。

```go
import (
    _ "fmt"
)
```

如果你的包出現了名稱衝突，你可以在包來源前面給他一個新的名稱。

```go
import (
    "github.com/karisu/teameow"
    neko "github.com/yamiodymel/teameow"
)

func main() {
    teameow.XXX()
    neko.XXX()
}
```

## 导出－Export

現在你知道可以引入包了，那麼什麼是「导出」？同個包內的函数還有共享變數確實可以直接用，但那**並不表示可以給其他包使用**，其方法取決於**函数／變數的「開頭大小寫」**。

是的。**Golang 依照一個函数／變數的開頭大小寫決定這個東西是否可供「导出」**。

```go
// a.go
package hello

// 注意：這裡的 Foo 的開頭字母是大寫！
var Foo string = "bar"

// 注意：這個 World 函数的開頭字母是大寫！
func World() {
    // ...
}
```

```go
// b.go
package test

import (
    "hello"
    "fmt"
)

func main() {
    fmt.Println(hello.Foo) // 输出：bar
    
    hello.World()
}
```

這用在區別函数的時候格外有用，因為小寫開頭的任何事物都是不供导出的，反之，大寫開頭的任何事物都是用來导出供其他包使用的。

一開始可能會覺得這是什麼奇異的規定，但寫久之後，你就能發現比起 JavaScript 和 Python 以「底線為開頭的命名方式」還要來得更好；比起成天宣告 `public`、`private`、`protected` 還要來得更快。

&nbsp;

# 类－Class

在 Golang 中沒有类，但有所謂的「建構體（Struct）」和「接口（Interface）」，這就能夠滿足幾乎所有的需求了，這也是為什麼我認為 Golang 很簡潔卻又很強大的原因。

讓我們先用 PHP 建立一個类，然後看看 Golang 怎麼解決這個問題。

#### PHP

```php
class Foobar {
    public $a = "hello, world";
    
    public function test() {
        echo $this->a;
    }
}

$b = new Foobar();
$b->test(); // 输出：hello, world!
```

#### Golang

雖然 Golang 沒有类，但是「建構體（Struct）」就十分地堪用了，首先你要知道在 Golang 中「类」的成員還有方法都是在「类」外面所定義的，這跟 PHP 在类內定義的方式有所不同，在 Golang 中還有一點，那就是他們沒有 `public`、`private`、`protected` 的種類。

```go
// 先定義一個 Foobar 建構體，然後有個叫做 a 的字串成員
type Foobar struct {
    a string
}

// 定義一個屬於 Foobar 的 test 方法
func (f *Foobar) test () {
    // 接收來自 Foobar 的 a（等同於 PHP 的 `$this->a`）
    fmt.Println(f.a)
}

b := &Foobar{a: "hello, world!"}
b.test() // 输出：hello, world!
```

## 构造方法－Constructor

在 PHP 中，當有一個类被 `new` 的時候會自動執行該类內的构造方法（`__construct()`），通常你會用這個來初始化一些类內部的值。

#### PHP

```php
class Test{
    public $a;
    
    function __construct() {
        $this->a = "foobar";
    }
    
    function show() {
        echo $this->a;
    }
}

$b = new Test();
$b->show(); // 输出：foobar
```

#### Golang

但是在 Golang 裡因為沒有类，也就沒有构造方法，不巧的是建構體本身也不帶有构造方法的特性，這個時候你只能自己在外部建立一個建構用函数。

```go
type Test struct {
    a string
}

func (t *Test) show() {
    fmt.Println(t.a)
}

// 用來建構 Test 的假构造方法
func newTest() (test *Test) {
    test = &Test{a: "foobar"}
    
    // 這裡會回傳一個型態是 *Test 建構體的 test 變數
    return
}

b := newTest()
b.show() // 输出：foobar
```

## 继承－Embed

讓我們假設你有兩個类，你會把其中一個类傳入到另一個类裡面使用，廢話不多說！先上個 PHP 範例（為了簡短篇幅我省去了換行）。

#### PHP

```php
class Foo {
    public $msg = "Hello, world!";
}

class Bar {
    public $foo;
    
    function __construct($foo){ $this->foo = $foo;    }
    function show()           { echo $this->foo->msg; }
}

$a = new Foo();
$b = new Bar($a);
$b->show(); // 输出：Hello, world!
```

#### Golang

在 Golang 中你也有相同的用法，但是請記得：「**任何東西都是在「类」外完成建構的**」。

```go
type Foo struct {
    msg string
}

type Bar struct {
    *Foo
}

func (b *Bar) show() {
    // Foo 中的 msg 會直接暴露在 Bar 底下
    // 所以你可以直接使用 b.msg
    fmt.Println(b.msg)
}

a := &Foo{msg: "Hello, world!"}
b := &Bar{a}
b.show() // 输出 Hello, world!
```

## 封装－Shadowing

在 PHP 中沒有相關的範例，這部分會以剛才「继承」章節中的 Golang 範例作為解說對象。

你可以看見 Golang 在進行 `Foo` 继承 `Bar` 的時候，會自動將 `Foo` 的成員暴露在 `Bar` 底下，那麼假設「雙方之間有相同的成員名稱」呢？

這個時候被继承的成員就會被「封装」，下面是個實際範例，還有你如何解決封装問題：

```go
type Foo struct {
    msg string
}

type Bar struct {
    *Foo
    msg string // 封装了 Foo 的 msg
}

a := &Foo{msg: "Hello, world!"}
b := &Bar{Foo: a, msg: "Moon, Dalan!"}

fmt.Println(b.msg)     // 输出：Moon, Dalan!
fmt.Println(b.Foo.msg) // 输出：Hello, world!
```

## 多态－Polymorphism

雖然都是呼叫同一個函数，但是這個函数可以針對不同的資料來源做出不同的舉動，這就是多态。你也能夠把這看作是：「訊息的意義由接收者定義，而不是傳送者」。

目前 PHP 中沒有真正的「多态」，不過你仍可以做出同樣的東西。

#### PHP

```php
class Foo{ public $msg = "hello";  }
class Bar{ public $msg = "world!"; }

class Handler {
    public function process($class) {
        switch(get_class($class)) {
            // 依照不同的資料類型做出不同的舉動
            case 'Foo':
                echo '處理 Foo | ' . $class->msg . ', world!';
                break;
                
            case 'Bar':
                echo '處理 Bar | ' . 'hello, ' . $class->msg;
                break;
        }
    }
}

$foo = new Foo();
$bar = new Bar();
$handler = new Handler();

// 雖然都是同個函数，但是可以處理不同資料
$handler->process($foo); // 输出：處理 Foo | hello, world!
$handler->process($bar); // 输出：處理 Bar | hello, world!
```

#### Golang

嗯⋯⋯那麼 Golang 呢？實際上更簡單而且更有條理了，在 Golang 中有 `interface` 可以幫忙完成這個工作。

```go
type Foo struct {
    msg string
}

type Bar struct {
    msg string
}

// 透過 Handler 實作 process
type Handler interface {
    process()
}

// 處理 Foo 資料的 process
func (f Foo) process() {
    fmt.Printf("處理 Foo | %s, world!", f.msg)
}

// 處理 Bar 資料的 process
func (b Bar) process() {
    fmt.Printf("處理 Bar | hello, %s", b.msg)
}

foo := Foo{msg: "hello"}
bar := Bar{msg: "world!"}

// 雖然都是同個函数，但是可以處理不同資料
Handler.process(foo) // 输出：處理 Foo | hello, world!
Handler.process(bar) // 输出：處理 Bar | hello, world!
```

如果你對 Interface 還不熟悉，可以試著查看「[解釋 Golang 中的 Interface 到底是什麼](https://yami.io/golang-interface/)」文章。

謝謝你看到這裡，可惜這篇文章卻沒有說出 Golang 最重要的賣點：「Goroutine」和「Channel」。簡單說就是讓你一秒變成多執行緒的超好用功能，這部分就⋯⋯改天見吧。
