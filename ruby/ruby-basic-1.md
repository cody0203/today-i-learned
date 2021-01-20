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
