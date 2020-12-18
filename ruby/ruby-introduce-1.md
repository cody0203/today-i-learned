# 1. A tour of Ruby

## 1.1. Ruby is Object-Oriented

- Ruby là ngôn ngữ hướng đối tượng.
- Tất cả mọi value trong Ruby đều là object, kể cả kí tự số đơn giản và những value như true, false và nil. (nil là null version của Ruby).
- Có thể dùng built-in method class để kiểm tra class hiện tại của value (Tương tự như sử dụng typeof để kiểm tra type của value trong JS). Ví dụ
    - 1.class `# => Interger: 1 có class là Interger`
    - 0.0.class `# => Float: 0.0 có class là Float`
    - true.class `# ⇒ TrueClass: true là ví dụ đơn lẻ của TrueClass`
    - false.class `# => FalseClass`
    - nil.class `# => NilClass`
    - "foo".class `# => String`
- Trong nhiều ngôn ngữ khác, để invoke function hoặc method thì cần sử dụng dấu ngoặc đơn (ví dụ như JS để gọi method class như trên thì sẽ là 1.class()). Tuy nhiên, việc sử dụng dấu ngoặc đơn để invoke method/function trong Ruby chỉ là optional và thường sẽ được bỏ qua.
- Ruby cũng rất strict về scope của object. Nếu muốn truy cập internal state của 1 object thì cần sử dụng method để thực hiện chứ không thực chọc thẳng vào trong object để lấy ra.

## 1.2. Blocks and Iterators

- Như bao ngôn ngữ lập trình khác, Ruby cũng có các Iterators (loop) method. 1 số ví dụ về Iterators method của Interger:
    - 3.times { print "Ruby! " } `# Prints "Ruby! Ruby! Ruby! "`
    - 1.upto(9) { |x| print x } `#Prints "123456789"`
- Ở đây, times và upto là 2 interators method implement bởi Interger Object. Đoạn code ở trong dấu ngoặc kép {} được gọi là 1 block. Block đóng vai trò là body của vòng lặp, logic sẽ được thực hiện trong đó.
- Mặc dù Ruby cũng có vòng lặp while nhưng việc sử dụng các iterators để thực hiện các vòng lặp thường hay được sử dụng hơn. (Khá giống với ES6, đa số thực hiện vòng lặp cũng không còn dùng for loop/while nhiều nữa).
- Arrays cũng là object có iterators, ví dụ như: each, map, collect,...

```ruby
# each iterator

nums = [3, 2, 1]   # Array literal
nums.each do |num| # each iterator. Block bắt đầu bằng do và có paratemer là num
	print num + 1    # Prints "432"
end                # Block bắt đầu = do thì sẽ kết thúc bằng end thay vì {}.
```

```ruby
# map iterator

nums = [1, 2, 3, 4]               # Array literal
mapResult = nums.map { |x| x*x  } # map iterator. Block nằm trong {}. x là từng
																	# value của nums, lấy từng value * với chính nó.
print mapResult                   # map iterator trả về array.
																	# mapResult sẽ có giá trị là [1, 4, 9, 16]
```

```ruby
# select iterator

nums = [100, 29, 541, 9, 2]                 # Array literal
selectResult = nums.select { |x| x <= 100 } # select iterator. x là từng value của
																					  # nums, lấy ra value nhỏ hơn 100.
print selectResult                          # select iterator trả về array.
																					  # selectResult có giá trị là [100, 29, 9, 2]
```

```ruby
#inject iterator

nums = [1, 2, 3, 4]                    # Array literal
injectResult = nums.inject do |sum, x| # inject iterator. Block bắt đầu bằng do
																			 # nhận vào 2 paratermeter là sum và x
	sum + x                              # lấy sum + x. inject tương đường với reduce trong JS.
end                                    # block bắt đầu = do thì kết thúc bằng end.
```

- `Hashes`, giống như `arrays`, `intergers`, ... cũng là 1 kiểu data căn bản của Ruby.` Nếu so sánh ra thì `hashes` có thể coi là object trong Js, cũng gồm các cặp key/value tương ứng tuy nhiên cách khai báo hơi có chút khác biệt. Cách query vào hashes thì ở Ruby chỉ có thể dùng cặp ngoặc vuông []. `Hashes` object cũng có built-in iterator là `each`.
- `Hashes` có thể sử dụng bất cứ object nào để làm key cũng được. Tuy nhiên `Symbol` thường được sử dụng nhất. `Symbols` là những string bất biến.

```ruby
# Hashes each iterator
hash = {                    # Đây là 1 hash
	:name => "Cody",          # dấu mũi tên => để thể hiện việc mapping key với value
	:age => 25                # dấu hai chấm : cho biết đây là 1 Symbol age.
}

hash.each do |key, value|   # each của hash có parameter là key và value
	print "#{key}:#{value}; " # Giống với template string của Js
end                         # print ra "name: Cody; age: 25; "
```

## 1.3. Expressions and Operators in Ruby

- Giống như những ngôn ngữ lập trình khác, những biểu thức của Ruby cũng bao gồm các giá trị và toán tử. Những toán tử của Ruby hầu như cũng không khác các ngôn ngữ khác là mấy, 1 số ví dụ:

```ruby
#Operators
1 + 2                        # => 3: biểu thức cộng
1 * 2                        # => 2: biểu thức nhân
1 + 2 == 3                   # => true: Ruby chỉ có 1 toán tử so sánh
1 + 2 == "3"                 # => false: toán tử so sánh cả value và type\
2 ** 3                       # => 8: biểu thức luỹ thừa
"Ruby" + " rocks!"           # => "Ruby rocks!": Nối chuỗi giống Js
"Ruby! " * 3                 # => "Ruby! Ruby! Ruby!": lặp lại chuỗi
max = x > y ? x : y          # => Ternary operator như Js
```

## 1.4. Methods

- Methods (function trong Js) được define bằng keyword def. Methods của Ruby không cần keyword return mà sẽ return ra giá trị của biểu thức cuối cùng trong body của nó.

```ruby
# Method

def square(x) # Define method square bằng keyword def. square nhận vào 1 param là x
	x * x       # return x squared
end           # Kết thúc method
```

- Như method square ở trên được define ngoài scope của `Object` hoặc 1 module thì sẽ có tác dụng như 1 global method hơn là 1 method được gọi bởi 1 object. Tuy nhiên, về mặt kĩ thuật thì 1 method như square nên trở thành private method của 1 `Object` class. Methods cũng có thể được define trên những object riêng lẻ bằng cách thêm prefix là tên object mà nó được define. Methods như này được gọi là singleton methods, và đây là cách mà Ruby define methods cho class,

```ruby
# Singleton method

def Math.square(x) # Define square class method cho Math module
	x * x
end
```

- `Math` module là 1 phần trong Ruby core mà đoạn code phía trên đã add thêm 1 method cho module này. Đây cũng là 1 key feature của Ruby - tất cả class và module đều `open` và có thể được modify và extend ở runtime. Param của method cũng có thể là default value (cách khai báo giống Js) hoặc là các giá trị được truyền vào.

## 1.5. Assignment

- Ruby sử dụng toán tử = để gán giá trị cho 1 biến.

```ruby
x = 1
```

- Có thể sử dụng toán tử assignment cùng với các toán tử khác như sau:

```ruby
x += 1   # Tương đương x = x + 1
y -= 2   # Tương đương y = y - 2
# Ruby không có toán tử ++ hoặc --
```

- Ruby cũng support parallel assignment - cho phép gán nhiều giá trị cho nhiều biến theo order tương ứng, khá giống với JS. Ví dụ:

```ruby
x, y = 1, 2     # Tương đương x = 1; y = 2.
a, b = b, a     # Swap value của 2 biến a, b với nhau
a,b,c = [1,2,3] # Với array, các biến sẽ được gán theo từng value của array
								# tương đương với array destructuring assignment của JS.
```

- Nếu function return nhiều value thì có thể return array rồi sử dụng parallel assignment cũng giúp lấy ra value trong array dễ dàng hơn:

```ruby
def polar(x, y)
	theta = Math.atan2(x, y)
	r = Math.hypot(x, y)
	[r, theta]                  # Trả về array gồm distance và angle.
end

distance, angle = polar(2, 2) # Sử dụng parallel assignment để lấy ra distance
															# và angle
```

- Với những method có tên kết thúc bằng dấu equal sign (=), Ruby cho phép invoke những method này bằng 2 cách như sau:

```ruby
class Obj
    def sum=(nums)
        @sum = nums.inject do |sum, x|
            sum + x
        end
        print @sum
    end
end

math = Obj.new

math.sum=([1, 2]) # Invoke method như bình thường.
math.sum = [1, 2] # Invoke method dạn assignment.
```

## 1.6. Suffixes and Prefixes

### 1.6.1. Suffixes

- Method kết thúc bằng equal sign (=) như ở ví dụ trên cũng là 1 suffix trong Ruby.
- Ngoài ra, Ruby còn 1 số loại suffix như:
    - ? - Dùng để đánh dấu cho những method sẽ trả về giá trị là Boolean. Array và Hash có method empty? để kiểm tra nếu không có value bên trong nó.
    - ! - Dùng để đánh dấu cho những method cần cẩn trọng khi sử dụng. Thông thường, method không có exclamation mark (!) sẽ trả về 1 bản copy của object mà nó được gọi, còn method có exclamation mark (!) thì sẽ mutate trực tiếp object mà nó được gọi, ví dụ:

    ```ruby
    nums = [1,3,4,2]
    sortedNum1 = nums.sort  # method sort sẽ copy Array nums ra để sử dụng.
    print nums              # Giá trị của Array nums vẫn như ban đầu [1,3,4,2]
    print sortedNum1        
    sortedNum2 = nums.sort! # method sort! sẽ mutate trực tiếp vào Array nums
    print nums              # Giá trị của Array nums đã bị thay đổi [1,2,3,4]
    print sortedNum2
    ```

### 1.6.2. Prefixes

- Prefixes trong Ruby được sử dụng ở trước tên biến, có tác dụng giúp xác định type và scope của biến đó.

```ruby
# Global variable
$name = "Cody"

def changeName name
	$name = name
end

changeName "Liin"
puts $name            # Liin - Global variable $name đã bị thay đổi bởi method
											# changeName.

# Instance variable
class Person
	def initialize(name)
		@name = name
	end

	def showName
		puts @name
	end
end

cody = Person.new('Cody')   # Khởi tạo 1 instance mới với instance var là Cody
cody.showName               # Cody

liin = Person.new('Liin')   # Khởi tạo 1 instance mới với instance var là Liin
liin.showName               # Liin

cody.showName               # Cody - Instance var @name không bị ảnh hưởng bởi những instance khác nhau hay còn có thể nói @name là local variable của mỗi instance.
# Class variable
class Person
    def initialize(name)
        @@name = name
    end
    
    def showName
        puts @@name
    end
end

cody = Person.new('Cody')   # Khởi tạo 1 instance mới với class var là Cody
cody.showName               # Cody

liin = Person.new('Liin')   # Khởi tạo 1 instance mới với class var là Liin
liin.showName               # Liin

cody.showName               # Liin - Class var @@name đã bị thay đổi sau khi instance liin gán lại value hay có thể nói @@name là global var trong class Person.
```