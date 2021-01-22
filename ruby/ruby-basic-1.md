# 1. Variables

## 1.1. Ruby variables

- Global variable: Bắt đầu bằng kí tự $.
    - Có thể sử dụng ở bất kì đâu trong code.
- Local variable
    - Chỉ được sử dụng trong nội tại của scope của 1 class, 1 module, hay 1 method.
- Instance variable: Bắt đầu bằng kí tự @.
    - Mỗi 1 instance của class sẽ có 1 instance variable riêng và không liên quan gì đến nhau.
- Class variable: Bắt đầu bằng 2 kí tự @@.
    - Class variable có thể được sử dụng ở bất kì đâu trong 1 class và các instance cũng có thể sử dụng chung class variable.
- Constants
    - Được bắt đầu bằng kí tự viết hoa.
    - Constants chỉ có thể được assign 1 lần.

## 1.2. Ruby variable scope

- Dựa trên 4 loại variable ở trên thì Ruby cũng có 4 scope lần lượt: Global → Class → Instance → Local.
- Với local variable thì bắt buộc phải gán giá trị cho nó nếu không khi gọi đến sẽ lỗi undefined.
- Nếu đặt tên local variable và method giống nhau nhưng khi gọi method theo cách thông thường (không sử dụng () hoặc self.method_name) thì sẽ xảy ra việc conflict giữa gọi local variable mới method đó.
- Sự khác nhau giữa local variable và instance variable là instance variable sẽ tồn tại trong từng instance khác nhau còn local variable tồn tại trong từng scope khác nhau.
- Chỉ có class, module, method được coi là variable scope, do/end chỉ được coi là block đơn thuần và trong block do/en vẫn có thể access local variable của cha nó.

# 2. String

## 2.1. String interpolation

- Có 2 cách căn bản là giống như Js tuy nhiên với interpolation thì có thể dùng với cả single/double quote.

```ruby
# String interpolation
str = 'The quick brown #{2 + 2}'
```

## 2.2. Ruby accessing string elements

- Có thể access vào elements trong string bằng string, index hoặc 1 khoảng index.

```ruby
str = "Ruby is awesome language"
p str          # Ruby is awesome language
p str['Ruby']  # Ruby
p str[0]       # R
p str[0...4]   # Ruby
p str[-1]      # e
p str[0] = 'L' # L
p str          # Luby is awesome language
```

## 2.3. Ruby concatenating strings

- Ruby có 1 số cách nối string như sau:

```ruby
p "Ruby" + " is awesome" + " language"
p "Ruby" " is awesome" " language"
p "Ruby" << " is awesome" << " language"
p "Ruby".concat(" is awesome").concat( " language")
```

## 2.4. Ruby comparing strings

- Có thể so sánh 2 string bằng == operator hoặc eql? method.

```ruby
p "Cody" == "Cody"   # true
p "Cody" == "Liin"   # false
p "Cody".eql? "Cody" # true
p "Cody".eql? "Liin" # false
```

## 2.5. String methods

### 2.5.1. gsub

- gsub tương tự như replace của Js, cũng có thể sử dụng regex để làm pattern.
- Syntax: gsub(pattern, replacement) hoặc gsub(pattern) {|match| block}.

```ruby
str = "Ruby is awesome language"

p str.gsub('a', '*')                   # "Ruby is *wesome l*ngu*ge"
p str.gsub('a') { |e| "#{e}#{e}" }     # "Ruby is aawesome laanguaage"
p str.gsub(/[ae]/, 'a' => 1, 'e' => 2) # "Ruby is 1w2som2 l1ngu1g2"
```

### 2.5.2. gsub!

- Giống với gsub nhưng gsub! sẽ trả về nil nếu không tìm thấy giá trị cần replace.
- 1 điểm khác nữa là gsub! sẽ làm mutate string ban đầu.

```ruby
str = "Ruby is awesome language"

p str.gsub!('a', '*')                   # "Ruby is *wesome l*ngu*ge"
p str.gsub!('a') { |e| "#{e}#{e}" }     # nil
p str.gsub!(/[ae]/, 'a' => 1, 'e' => 2) # "Ruby is 1w2som2 l1ngu1g2"
p str.gsub!('t', '*')                   # nil
```

### 2.5.3. split

- Tác dụng tương tự split của Js tuy nhiên giá trị return lại hơi khác 1 chút nếu sử dụng cùng 1 pattern. Không làm mutate string ban đầu.

```ruby
str = " Ruby is awesome language "

p str.split         # ["Ruby", "is", "awesome", "language"]
p str.split(' ')    # ["Ruby", "is", "awesome", "language"]
p str.split(/ /)    # ["", "Ruby", "is", "awesome", "language"]
p str.split(' ', 1) # [" Ruby is awesome language "]
p str.split(' ', 3) # ["Ruby", "is", "awesome language "]
```

### 2.5.4. strip

- Tương tự như trim của Js, trả về 1 string mới và bỏ đi whitespace ở đầu và cuối string đó.

# 3. Symbol

## 3.1. Introduction

- Symbol trong Ruby được sử dụng để làm thằng nhận diện cho 1 resource nhất định. Resource đó có thể là 1 method, 1 biến, 1 key của hash, 1 state, ...
- Có thể generate 1 symbol bằng 3 cách:

```ruby
sym1 = :hello
sym2 = :"hello"
sym3 = "hello".to_sym
```

- Mỗi symbol sẽ là unique vì mỗi 1 instance của class Symbol chỉ được sử dụng để khởi tạo cho 1 symbol. Đây cũng là điểm khác biệt của symbol với string.

```ruby
p :pending.object_id  # => 1764508
p :pending.object_id  # => 1764508
p :success.object_id  # => 1297308

p 'pending'.object_id # => 47256600737660
p 'pending'.object_id # => 47256600737540
```

- Ngoài ra, symbol cũng là immutate, khác với string và so sánh performance giữa string và symbol thì symbol cũng nhỉnh hơn.

```ruby
str = "hello"
sym = :hello

str[0] = 'k'
p str        # "kello"
sym[0] = 'k'
p sym        # `<main>': undefined method `[]=' for :hello:Symbol (NoMethodError)
             # Did you mean?  []
```

## 3.2. When should use Symbol?

- Dùng làm key của hash

```ruby
user = {
	name: "Cody",
	age: 26
}

=> {
	:name: "Cody",
	:age: 26
}
```

- Dùng làm parameter

```ruby
class Cat
 attr_accessor :name
end

kitty = Cat.new
kitty.name = "Liin"
p kitty.name        # "Liin"
```

# 4. Number

## 4.1. Introduction

- Có 2 loại number trong Ruby là integer và float.
- Integer là những number không có decimal points và ngược lại.

```ruby
Integer: 1, 3, 5, 6, 100, 100_000_000
Float: 3.5, 10.0
```

## 4.2. Arithmetic functions

- Ruby cũng có 1 số arithmetic functions giống Js và các ngôn ngữ lập trình khác.

```ruby
5 + 5  # addition
5 - 5  # subtraction
5 * 5  # multiplication
5 / 5  # division
5 ** 5 # exponentiation
```

## 4.3. Operations precedence

- Tương tự như Js.
- Trong ngoặc tròn → Exponentiation → multiplication/division → addition/subtraction

## 4.4. Difference between integers and floats

- Với 1 phép tính mà tất cả là số integer, khi thực hiện tính toán mà kết quả trả về là float thì sẽ được parse về integer.
- Ruby cũng có `floating point math` như Js :v.

```ruby
puts 9 / 2      # 4
puts 9.0 / 2.0  # 4.5
puts 5 / 2.0    # 2.5
puts 2 ** 4     # 16
puts 0.4 - 0.3  # 0.10000000000000003
```

# 5. Iterators and Loops

## 5.1. Loop - while

### 5.1.1. Introduction

- Có 2 cách sử dụng while loop là while do và end while.

### 5.1.2. Syntax

```ruby
while condition do
	# condition here
end

or

begin
	# code here
end while condition
```

### 5.1.3. Example

```ruby
p "While do"

i = 0
while i < 10 do
	p "Current i: #{i}"
	i += 1
end                       # 0 1 2 3 4 5 6 7 8 9

p "End while"

x = 0
begin
	p "Current x: #{x}"
	x += 1
end while i < 10         # # 0 1 2 3 4 5 6 7 8 9
```

## 5.2. Loop - Until

### 5.2.1. Introduction

- Giống như while, until cũng có 2 cách viết là until do và end until.
- Tuy nhiên về cách sử dụng thì lại ngược lại so với while.

### 5.2.2. Syntax

```ruby
until condition do
	# condition here
end

or
begin
	# code here
end until condition
```

### 5.2.3. Example

```ruby
p "until do"
i = 0
until i > 10 do
    p "Current i: #{i}"
    i += 1
end                       # 0 1 2 3 4 5 6 7 8 9 10

p "end until"

x = 0
begin
    p "Current x: #{x}"
    x += 1
end until x > 10          # 0 1 2 3 4 5 6 7 8 9 10
```

## 5.3. Each iterator

### 5.3.1. Syntax

```ruby
collection.each{|item| #code here}

or

collection.each do |item|
	#code here
end
```

### 5.3.2. Example

```ruby
[1, 2, 3, 4].each{|num| p num * 2} # 2 4 6 8

[1, 2, 3, 4].each do |num|
    p num * 2
end                                # 2 4 6 8

(1..4).each do |num|
	p num * 2
end                                # 2 4 6 8
```

## 5.4. Loop - "For in"

### 5.4.1. Introduce

- For in cũng được dùng để tạo 1 vòng lặp trong Ruby.
- For in cũng có thể dùng được 1 số keyword như break, next để xử lý logic trong loop.
- Nếu dùng redo thì phải có logic xử lý để end được cái redo đấy nếu k sẽ xảy ra tình trạng infinite loop.

### 5.4.2. Syntax

```ruby
for variable [, variable ...] in expression do
	# code here
end
```

### 5.4.3. Example

```ruby
# Normal case
for i in 0..4 do
    p "Current i: #{i}"
end                      # 0 1 2 3 4

# Break for in
for i in 0..4 do
    if i == 2
        break
    end
    p "Current i: #{i}"
end                      # 0 1

# Next for in
for i in 0..4 do
    if i == 2
        next
    end
    p "Current i: #{i}"  # 0 1 3 4
end

# Redo for in
for i in 0..4 do
	p "Current i: #{i}"
	i += 1 and redo if i == 2   # 0 1 2 3 3 4
end
```

# 6. Conditionals

## 6.1. Introduction

- Condition sẽ false khi giá trị expression là false hoặc nil.
- Số 0 trong Ruby cũng sẽ là true.

## 6.2. If statement

### 6.2.1. Syntax

```ruby
if condition
	# code here
elsif condition
	# code here
else
	# code here
end

# shorthand
[code here] if condition
```

### 6.2.2. Example

```ruby
x = 1
if x > 2
    p "x is greater than 2"
elsif x < 1
    p "x is smaller than 1"
else
    p "x is 1"
end                         # "x is 1"

y = 2
p "y is 2" if y == 2        # "y is 2"
```

## 6.3. Unless statement

### 6.3.1. Introduction

- Best practice là chỉ nên sử dụng unless với condition không có chứa operator.

### 6.3.2. Syntax

```ruby
unless condition
	# code here
else
	# code here
end

# shorthand
[code here] unless condition
```

### 6.3.3. Example

```ruby
x = false
unless x
    p "x is false"
else
    p "x is true"
end                     # "x is false"

p "x is false" unless x # "x is false"
```

## 6.4. Case when

### 6.4.1. Introdution

- Giống switch case của Js, case when không có default mà thay = else.

### 6.4.2. Syntax

```ruby
case expression
when expression
	# code here
else
	# code here
end
```

### 6.4.3. Example

```ruby
age = 5
case age
when 0..2
    p "baby"
when 3..6
    p "little child"
when 7..12
    p "child"
when 13..18
    p "youth"
else
    p "adult"
end                  # "little child"
```

# 7. Methods

## 7.1. Introduction

- Method của Ruby tương tự so với function của Js, cũng có tên, nhận vào 1 số input, thực hiện 1 số logic và trả về kết quả.
- Method thường được đặt tên theo dạng snake_case và viết thường.

## 7.1.1. Syntax

```ruby
def method_name(arg)
	# code here
end
```

### 7.1.2. Example

```ruby
def print_name(name)
	p "My name is #{name}"
end

print_name("Cody")       # "Cody"
```

## 7.2. Method arguments

### 7.2.1. Index arguments

- Arguments của Ruby có 1 số kiểu như sau: index arguments, keyword arguments, rest arguments.
    - Tương tự Js.

    ```ruby
    def sumNum(num1, num2)
    	p num1 + num2
    end
    sumNum(1, 2)            # 3
    ```

    - Khai báo default value argument của index arguments:

    ```ruby
    def sumNum(num1 = 2, num2 = 4)
    	p num1 + num2
    end
    sumNum(1)               # 5
    ```

### 7.2.2. Keyword arguments

- Giống như việc truyền 1 param là object vào method của Js, tuy nhiên cách khai báo lại hơi khác. Nếu sử dụng keyword argument thì bắt buộc phải khai báo default value cho keyword, nếu không thì behavior sẽ sai
- Cách khai báo default value của keyword arguments cũng khác với index arguments.
- Như example dưới, nếu không khai báo default value thì argument của method sẽ nhận vào là 1 hash.

```ruby
def foo(bar: 'foo')
  bar
end

p foo(bar: 'baz') # => 'baz'

def foo2(bar)
  bar
end

p foo2(bar: 'baz') # => {:bar=>"baz"}
```

### 7.2.3. Rest argument

- Cũng tương tự như bên Js, nếu có nhiều argument thì rest argument sẽ khai báo ở cuối cùng.
- Có 2 cách gọi restArgs trong method, mỗi cách sẽ return 1 kiểu value khác nhau.

```ruby
def foo(a, b, *restArgs)
    p *restArgs          # 3 4 5 6
		p restArgs           # [3, 4, 5, 6]
end
foo(1, 2, 3, 4, 5, 6)    
```

## 7.3. Method returning value

### 7.3.1. Introduction

- Method của Ruby sẽ return giá trị của câu lệnh cuối cùng trong method, keyword return là optional nên có thể dùng hoặc không.
- Trong trường hợp muốn kết thúc method thì có thể dùng return như js.

### 7.3.2. Example

```ruby
def sumNum(x, y)
    p x + y
end

sumNum(1, 2)         # 3

def sumNum2(x, y)
    return p x + y
end

sumNum2(3, 4)        # 7

def sumNum3(x, y)
    return p "Return here" if x < 3
    p x + y
end

sumNum3(2, 5)       # "Return here"
```

## 7.4. Class method and instance method

### 7.4.1. Introduction

- Class method là method của 1 class, class đó có thể execute trực tiếp method.
- Instance method là method của instance của 1 class, class không thể execute trực tiếp được method đó mà phải thông qua 1 instance.
- Nếu gọi method của class thông qua instance hoặc ngược lại thì sẽ báo lỗi undefined vì method không tồn tại.
- Class method và instance method cũng có 1 vài cách khai báo khác nhau.

### 7.4.2. Example

```ruby
# Class method
class Foo
    def self.bar_one
        p "This is class method 1"
    end
    
    class << self
        def bar_two
            p "This is class method 2"
        end
    end
end

Foo.bar_one # "This is class method 1"
Foo.bar_two # "This is class method 2"
```

```ruby
class Foo
    attr_accessor :baz
    def bar_one
        p "This is instance method 1"
    end
end

foo = Foo.new
foo.bar_one   # "This is instance method 1"

foo.baz = "This is instance method 2"
p foo.baz     # "This is instance method 2"
```

## 7.5. Block, Proc, Lambda

### 7.5.1. Block

**7.5.1.1. Introduction**

- Block được khởi tạo bằng do, kết thúc bằng end hoặc trong 1 cặp ngoặc nhọn {}.
- Block có thể có nhiều args.
- Tên của args được khai báo bên trong cặp pipe | |.
- Block thường được dùng với các iterators.

```ruby
1.upto(10) { |x| p x } # 1 2 3 4 5 6 7 8 9 10
1.upto(10) do |x|
    p x * 2            # 2 4 6 8 10 12 14 16 18 20
end
```

**7.5.1.2. Implicit block**

- Block có thể được truyền vào làm param là method. Block ở trong method có thể được Ruby ngầm hiểu (implicit) và có thể được execute bằng keyword yield mà không cần khai báo args.

```ruby
def hello
    yield
end

hello do
    puts " Implicit block"
end

def sumNum
    p yield + 3  # 6
end

sumNum do
    1 + 2
end
```

**7.5.1.3. Explicit block**

- Ngược lại với implicit, explicit block sẽ là kiểu khai báo arg của block cho method.
- Khai báo arg cho block sẽ có dạng `def method_name(&block)` .
- Nếu param truyền vào là 1 block thường thì Ruby sẽ convert block này sang Proc object bằng `to_proc` nên khi sử dụng .class thì lúc này sẽ thấy result là Proc và không cần sử dụng yield nữa mà có thể dùng call để gọi block này.

```ruby
def hello(&block)
    p block.class          # Proc
    block.call             # Implicit block
end

hello do
    puts " Implicit block"
end
```

### 7.5.2. Proc

- Proc là 1 instance của class Proc, chứa 1 block code.
- Proc có thể assign vào biến, block thì k thể.

```ruby
sumNum = Proc.new { | x, y | x + y }

p sumNum.call(1, 2)
```

### 7.5.3. Lambda

- Lambda là 1 anonymous function (không có tên).
- Lambda cũng là 1 instance của class Proc, chức năng tương tự như proc.

```ruby
square = lambda {|n| n ** 2}
p square.call(2)  # 4
p square.class    # Proc
```

### 7.5.4. Proc vs Lambda

- Cả Proc và Lambda đều là instance của class Proc.
- Lambda sẽ kiểm tra số lượng arg truyền vào trong khi Proc thì không.

```ruby
lam = lambda { | x | p x }
lam.call(2)                # 2
lam.call                   # wrong number of args error
lam.call(1,2,3)            # wrong number of args error

proc = Proc.new { | x | p x }
proc.call(2)               # 2
proc.call                  # nil
proc.call(1,2,3)           # 1
```

- Return của lambda với proc cũng có behavior khác nhau. Ví dụ cùng trong method thì nếu return trong lambda, code bên dưới lambda vẫn sẽ được execute còn với proc thì không.

```ruby
def lambda_test
    lam = lambda { return p "Lambda return" }
    lam.call
    p "End lambda test method"
end

def proc_test
    proc = Proc.new { return p "Proc return" }
    proc.call
    p "End proc test method"
end

lambda_test
proc_test

# Result
"Lambda return"
"End lambda test method"
"Proc return"
```
