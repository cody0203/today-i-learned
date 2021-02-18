# 8. Collection

## 8.1. Array

### 8.1.1. Introduction

- Cơ bản là giống Array của Js.
- Đều là tập hợp của những object, có index là integer, index start từ 0 là negative để select từ cuối của array.

### 8.1.2. Creating array

- Có 1 số cách để create array như sau:

```ruby
arr = [1, 2, 3, 4]

Array.new            # => []
Array.new [1, 2, 3]  # => [1, 2, 3]
Array.new 1          # => [nil]
Array.new 1, 1       # => [1]
Array.new 3, "Hello" # => ["Hello", "Hello", "Hello"]

arr2 = %w(1, 2, 3)   # => ["1", "2", "3"]
```

### 8.1.3. Accessing array

- Có thể sử dụng cặp ngoặc vuông [] hoặc 1 số method để access value của array bằng index:

```ruby
# Using []
arr = [1, 2, 3, 4, 5]

p arr[2]                 # 3
p arr[5]                 # nil
p arr[-1]                # 5
p arr[0, 2]              # [1, 2]
p arr[0, 0]              # []
p arr[0..3]              # [1, 2, 3, 4]
```

```ruby
# Using methods
arr = [1, 2, 3, 4, 5]

arr.at 0               # 1
arr.first              # 1
arr.last               # 5
arr.take 2             # [1, 2]
```

### 8.1.4. Get information about an array

```ruby
numbers = ["one", "two", "three", "four"]
numbers.length                            # => 4
numbers.empty?                            # => false
numbers.include? "ten"                    # => false
```

### 8.1.5. Array manipulation

- Có thể add thêm item vào cuối array bằng method push hoặc <<
- Push và << sẽ trả về array mới sau khi được add thêm item, khác với Js là sẽ trả về độ dài của array mới.

```ruby
arr = [1, 2, 3]
p arr.push 4      # [1, 2, 3, 4]
p arr             # [1, 2, 3, 4]
p arr.push 5, 6   # [1, 2, 3, 4, 5, 6]
p arr << 7        # [1, 2, 3, 4, 5, 6, 7]
p arr << 8 << 9   # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- Có thể add item vào đầu array thằng method unshift.
- Giống như push, unshift cũng trả về array mới sau khi được add thêm item.

```ruby
arr = [1, 2, 3]
p arr.unshift 0    # [0, 1, 2, 3]
```

- Với method insert, có thể add thêm item với vào array ở bất kì vị trí nào, điều kiện là cần pass index muốn add vào method.

```ruby
arr = [1, 2, 4]
p arr.insert 2, 3 # [1, 2, 3, 4]
```

- Với method pop, có thể dùng để remove item cuối cùng trong array.
- Method pop sẽ trả về item vừa được lấy ra trước đó, giống Js.

```ruby
arr = [1, 2, 3, 4, 5]
p arr.pop                # 5
p arr                    # [1, 2, 3, 4]
```

- Với shift thì là remove item ở vị trí đầu tiên của array, còn lại behavior giống pop.

```ruby
arr = [0, 1, 2, 3]
p arr.shift              # 0
p arr.shift              # [1, 2, 3]
```

- Để xoá item ở 1 ví trị bất kì trong array, có thể sử dụng delete_at, delete_at nhận vào vị trí muốn delete.
- delete_at cũng trả về item bị delete đó.

```ruby
arr = [1, 2, 4, 3]
p arr.delete_at(2)   # 4
p arr                # [1, 2, 3]
```

### 8.1.6. Iterating over array

- Ruby cung cấp 1 số iterator method cho array tương tự Js như each, map, find, select (filter), ...
- Tham khảo thêm các method khác của Array tại: [https://ruby-doc.org/core-2.7.1/Array.html](https://ruby-doc.org/core-2.7.1/Array.html)

## 8.2. Hash

### 8.2.1. Introduction

- Hash tương tự object của Js, gồm các cặp key value.

### 8.2.2. Creating hash

- Có 1 số cách để create hash như sau:

```ruby
hash = Hash.new                  # {}

hash = Hash["a": 100, "b": 200]  # { :a => 100, :b => 200 }
hash = { a: 300, b: 400 }        # { :a => 400, :b => 400 }
```

- Ở đây, nếu không sử dụng string thì mặc định key của hash sẽ là symbols.

### 8.2.3. Accessing hash

- Để access hash thì cũng dùng cặp ngoặc vuông [].
- Tuy nhiên vì key của hash là symbol nên cần gọi đúng format.

```ruby
hash = { a: 300, b: 400 }
hash[:a]                     # 300
hash[a]                      # undefined local variable a
hash[:c]                     # nil
hash.keys                    # [:a, :b]
hash.values                  # [300, 400]
```

### 8.2.4. Converting to hash

- Có thể sử dụng method try_convert của Hash class để convert 1 object thành hash. Nếu thành công sẽ trả về 1 hash mới, nếu không sẽ trả về nil.
- Convert cũng cần chú ý tới giá trị của key.

```ruby
Hash.try_convert({1 => 100})   # { 1 => 100 }
Hash.try_convert({a => 200})   # undefinded local variable a
Hash.try_convert({:a => 300})  # { :a => 300}
Hash.try_convert("1=>200")     # nil
```

### 8.2.5. Equality hashes

- Có thể sử dụng các operator như ==, ≠, >, <, ≥, ≤ để compare các hash với nhau.
- 2 hash cùng có các cặp key/value giống nhau nhưng vị trí khác nhau thì cũng sẽ bằng nhau.
- Ruby không có khái niệm shallow/deep nên khá dễ để compare các hash.

```ruby
h = Hash["a": 100, "b": 200, "c": 300]
h1 = Hash["a": 100, "b": 200, "c": 300, "d": 400]
h2 = Hash["b": 200, "c": 300, "a": 100]
h3 = Hash["a": 100, "b": 200, "c": 400]

puts "h == h1 #=> #{h == h1}"                      # false
puts "h == h2 #=> #{h == h2}"                      # true
puts "h1 == h2 #=> #{h1 == h2}"                    # false
puts "h > h1 #=> #{h > h1}"                        # false
puts "h1 > h #=> #{h1 > h}"                        # true
puts "h1 != h #=> #{h1 != h}"                      # true
puts "h > h3 #=> #{h > h3}"                        # false
puts "h <= h3 #=> #{h <= h3}"                      # false
puts "h != h3 #=> #{h != h3}"                      # true
```

### 8.2.6. Element assignment

- Giống như cách access, cũng có thể dùng cặp ngoặc vuông [] để access và assign value cho element trong hash.
- Nếu assign vào key chư tồn tại trong hash thì key đó sẽ được tạo mới.

```ruby
h = {"a": 100, "b": 200}
h["a"] = 10                # h ⇒ {"a"=>10, "b"=>200}
h["c"] = 300               # h ⇒ {"a"=>10, "b"=>200, "c"=> 300}
h.store "d", 400           # h ⇒ {"a"=>10, "b"=>200, "c"=> 300, "d"=>400}
```

### 8.2.7. Iterating over hash

- Với hash thì phải dùng method để iterator.
- Có 1 số method như sau: each, each_key, each_value, each_pair, ...

```ruby
hash = {a: 1, b: 2}
hash.each { |key, value| p "key is: #{key}, value is: #{value}" } # "key is: a, value is: 1" "key is: b, value is: 2"
hash.each_key { |key| p "key is #{key}"} # "key is a" "key is b"
hash.each_value { |value| p "value is #{value}"} # "value is 1" "value is 2"
```

- Tham khảo thêm các method khác của hash tại: [https://ruby-doc.org/core-2.7.1/Hash.html](https://ruby-doc.org/core-2.7.1/Hash.html)

# 9. OOP

## 9.1. Classes

### 9.1.1. Class definition

- Ruby sử dụng class để define các tính chất và hành vi của object.
- Convention name của class sẽ là PascalCase thay vì snake_case như method.

```ruby
class GoodDog
end

sparky = GoodDog.new
```

### 9.1.2. Initialize method

- Initialize method là method tiêu chuẩn của class trong Ruby, hoạt động gần giống với constructor của các ngôn ngữ khác.
- Initialize method được sử dụng khi muốn khởi tạo 1 số class variable tại thời điểm khởi tạo class.
- Initialize method cũng có tham số như method bình thường và cũng được khởi tạo bằng keyword `def`.

```ruby
class Box
    def initialize w, h
        @width, @height = w, h
    end
end
```

### 9.1.3. Accesstor & Setter (Getter & Setter)

- Accesstor method dùng để make variable của class available ở bên ngoài class. Accesstor sẽ return về class variable.
- Tương tự với accesstor, Ruby sử dụng Setter để set lại giá trị cho variable bằng setter methods.

```ruby
class Box
    def initialize w, h
        @width, @height = w, h
    end
    
    def getWidth
        @width
    end
    
    def getHeight
        @height
    end
    
    def setWidth=(newWidth)
        @width = newWidth
    end
    
    def setHeight=(newHeight)
        @height = newHeight
    end
end

box = Box.new 10, 20
p box.getWidth                  # 10
p box.getHeight                 # 20

# Using setter
box.setWidth = 15
p box.getWidth                  # 15
box.setHeight = 25
p box.getHeight                 # 25
```

- Ngoài cách sử dụng def keyword để khai báo còn có thể sử dụng accessor methods để generate accesstor/setter method. Tuy nhiên các method này phải có cùng tên với variable.
- Có 3 loại accessor method:
    - attr_reader: Dùng để generate getter method.
    - attr_writer: Dùng để generate setter method.
    - attr_accessor: Dùng để generate cả getter và setter method.

```ruby
class Box
    def initialize w, h
        @width, @height = w, h
    end
    
    attr_reader :width
    attr_writer :width
    
    attr_accessor :height
end

box = Box.new 10, 20

# attr_reader & attr_writer
p box.width                  # 10
box.width = 15
p box.width                  # 15

# attr_accessor
p box.height                 # 20
box.height = 30
p box.height                 # 30
```

### 9.1.4. Instance method

- Được khai báo như 1 method bình thường với keyword def. Chỉ có thể sử dụng được với các instance của class.
- Instance method không thể được gọi bằng class và ngược lại

```ruby
class Box
    def initialize w, h
        @width, @height = w, h
    end
    
    # instance method
    def getArea
        @width * @height
    end
end

box = Box.new 10, 20
p box.getArea                     # 200
p Box.getArea                     # undefined method getArea
```

### 9.1.5. Class method

- Các instance của method không thể gọi class method.
- Class method được khởi tạo bằng syntax `def self.methodName`

```ruby
class Box
    @@count = 10
    def self.printCount
        @@count
    end
end

p Box.printCount

box = Box.new                   # 10
box.printCount                  # undefined method printCount
```

## 9.2. Abstraction (Tính trừu tượng)

- Tính trừu tượng làm giảm sự phức tạp của các công việc bằng cách ẩn đi những chi tiết không liên quan trực tiếp tới user, giúp công việc vẫn hoàn thành mà user không cần hiểu, không cần nghĩ tới công việc đó cần những gì để hoàn thành.
- Tính trừu tượng không chỉ áp dụng trong programing mà còn được áp dụng trong thực tế cuộc sống. Ví dụ khi gửi mail thì user chỉ cần soạn nội dung và ấn gửi chứ không cần quan tâm việc hệ thống xử lý việc gửi mail ra sao hoặc khách hàng chỉ cần order và trả tiền cho món ăn chứ không cần quan tâm nó được làm thế nào.

## 9.3. Encapsulation (Tính đóng gói)

- Tính đóng gói được thể hiện qua việc gói gọn những data có liên quan tới nhau cùng với những method hoạt động dựa trên những data đó vào chung 1 class.
- Tính đóng gói còn có cơ chế ngăn ngừa việc truy xuất hoặc sử dụng method private của class nên Encapsulation còn thường bị hiểu lầm là data hiding.
- Data hiding trong Ruby cũng được thể hiện thông qua keyword private và protected.
- Những method được khai báo trong protected có thể hiểu đơn giản là trong class nó sẽ giống như public method còn bên ngoài class thì nó tương tự như private method.
- Ở 1 số ngôn ngữ khác thì có thể set private/public cho attribute (variable) của class nhưng ở Ruby thì attribute mặc định sẽ là private.

### 9.3.1. Instance method: Bên ngoài class

```ruby
class Box
    # public method
    def public_method
        p "This is public method of class Box"
    end
    
    # protected method
    protected
    def protected_method
        p "This is protected method of class Box"
    end
    
    # private method
    private
    def private_method
        p "This is private method of class Box"
    end
end

box = Box.new

box.public_method    # "This is public method of class Box"

box.protected_method # NoMethodError
box.private_method   # NoMethodError
```

- Những instance method thuộc protected và private thì không thể gọi được ở bên ngoài class.

### 9.3.2. Instance method: Bên trong class

```ruby
class Box
    # public method
    def public_method
        p "This is public method of class Box"
        protected_method
        private_method
    end
    
    # protected method
    protected
    def protected_method
        p "This is protected method of class Box"
    end
    
    # private method
    private
    def private_method
        p "This is private method of class Box"
    end
end

box = Box.new

box.public_method    # "This is public method of class Box"
                     # "This is protected method of class Box"
                     # "This is private method of class Box"
```

- Tuy nhiên có thể gọi bên trong class như bình thường.

### 9.3.3. Class method: Bên ngoài class

```ruby
class Box
    # public method
    class << self
        def public_method
            p "This is public method of class Box"
        end
        
        # protected method
        protected
        def protected_method
            p "This is protected method of class Box"
        end
        
        # private method
        private
        def private_method
            p "This is private method of class Box"
        end
    end
end

Box.public_method    # "This is public method of class Box"
Box.protected_method # NoMethodError
Box.private_method   # NoMethodError
```

- Với class method thì cũng tương tự.

### 9.3.4. Class method: Bên trong class

```ruby
class Staff
    class << self
        def public_class
        puts "public_class is public method!!!!"
        self.protected_class
        self.private_class                       # NoMethodError
        Staff.protected_class
        Staff.private_class                      # NoMethodError
        end
        
        protected
        def protected_class
        puts "protected_class is protected method!!!!"
        end
        
        private
        def private_class
        puts "private_class is private method!!!!"
        end
    end
end
Staff.public_class
```

- Để execute class method ở bên trong class sẽ hơi khác 1 chút so với instance method.
- Với protected method, ta cần sử dụng keyword self hoặc dùng thẳng tên class để execute method đó.
- Với private method thì không cần, có thể gọi bình thường như với private instance method.

## 9.4. Inheritance

- Tính kế thừa (Inheritance) giúp sử dụng lại các đoạn code 1 cách hiệu quả. Khi tạo 1 class thay vì viết lại 1 class mới hoàn toàn thì có thể kế thừa lại 1 số attribute hoặc method từ 1 class khác đã tồn tại.
- Class tồn tại từ trước được gọi lá base class, class kế thừa từ base class gọi là derived class.

### 9.4.1. Inheritance với instance method

- Derived class có thể kế thừa những method từ base class. Nếu derived class cũng có method trùng tên với base class thì sẽ override method của base class.

```ruby
class Animal
    def speak
        p "Hello"
    end
end

class Dog < Animal
    attr_accessor :name
    
    def initialize n
        self.name = n
    end
    
    def speak
        p "Hello my name is #{self.name}"
    end
end

class Cat < Animal
end

pluto = Dog.new "Pluto"
tom = Cat.new

pluto.speak
tom.speak
```

- Ở đây, class Dog đã override method speak của base class Animal.

### 9.4.2. Inheritance với public/private/protected instance method

- Với public instance method, derived class có thể gọi nó ở bên ngoài class bình thường.
- Với private và protected instance method thì không thể gọi trực tiếp bên ngoài class, behavior cơ bản là giống với method của chính derived class.

```ruby
class Box
    def public_instance
    puts " public_instance is public method!!!!"
    end

    protected
    def protected_instance
    puts " protected_instance is protected method!!!!"
    end

    private
    def private_instance
    puts " private_instance is private method!!!!"
    end
end

class BigBox < Box
	def big_box_public_instance
		public_instance
		protected_instance
		private_instance
	end

	def big_box_public_instance_1
		self.protected_instance
		self.private_instance             # NoMethodError
	end
end

big_box = BigBox.new
big_box.public_instance               # public_instance is public method!!!!
big_box.protected_instance            # NoMethodError
big_box.private_instance              # NoMethodError
big_box.big_box_public_instance       # public_instance is public method!!!!
                                      # protected_instance is protected method!!!!
																			# private_instance is private method!!!!
big_box.big_box_public_instance_1     # protected_instance is protected method!!!!
																			
```

### 9.4.2. Inheritance với public/private/protected class method

- Behavior tương tự với base class.

```ruby
class Staff
    class << self
        def public_class
        puts " public_class is public method!!!!"
        end

        protected
        def protected_class
        puts " protected_class is protected method!!!!"
        end

        private
        def private_class
        puts " private_class is private method!!!!"
        end
    end
end

class Manager < Staff
    def self.manager_public_class
    protected_class
    private_class
    self.protected_class
    self.private_class
    end
end

Manager.public_class
Manager.manager_public_class

# public_class is public method!!!!
# protected_class is protected method!!!!
# private_class is private method!!!!
# protected_class is protected method!!!!
# NoMethodError
```

## 9.5. Polymorphism

- Tính đa hình (Polymorphism) là tính chất đặc trung cuối cùng của OOP.
- Tính đa hình nói về việc tuỳ theo context mà đối tượng sẽ thể hiện 1 cách khác nhau.
- Trong cuộc sống bình thường, 1 người khi ở công ty sẽ là sếp, khi đi nhậu sẽ là chiến hữu, khi về nhà sẽ là chồng/cha.
- Trong Ruby, tư tưởng Polymorphism cũng tương tự như vậy, triển khai code sao cho các object có chung method tuy nhiên mỗi method lại có cách triển khai các nhau để phù hợp với context.

```ruby
# Polymorphism using Inheritance
class Vehicle 
    def tyreType 
        puts "Heavy Car"
    end
end
   
# Using inheritance  
class Car < Vehicle 
    def tyreType 
        puts "Small Car"
    end
end
   
# Using inheritance  
class Truck < Vehicle 
    def tyreType 
        puts "Big Car"
    end
end
  
# Creating object  
vehicle = Vehicle.new
vehicle.tyreType()          # Heavy Car
   
vehicle = Car.new
vehicle.tyreType()          # Small Car
   
vehicle = Truck.new
vehicle.tyreType()          # Big Car
```

- Ở đây thấy derived class đã override method typeType của base class và mỗi method sẽ xử lý và trả về 1 result khác nhau.

```ruby
# Polymorphism using Duck-Typing
class Hotel 

  def type(customer) 
    customer.type 
  end
   
  def room(customer) 
    customer.room 
  end
   
end
   
# Creating class with two methods  
class Single 
   
  def type 
    puts "Room is on the fourth floor."
  end
   
  def room 
    puts "Per night stay is 5 thousand"
  end
   
end
   
   
class Couple 
   
 # Same methods as in class single 
  def type 
    puts "Room is on the second floor"
  end
   
  def room 
    puts "Per night stay is 8 thousand"
  end
   
end
   
# Creating Object 
# Performing polymorphism  
hotel= Hotel.new
puts "This visitor is Single."
customer = Single.new
hotel.type(customer) 
hotel.room(customer) 
   
   
puts "The visitors are a couple."
customer = Couple.new
hotel.type(customer) 
hotel.room(customer)
```

- Ở đây có 3 class gồm Hotel, Single, Couple có 2 method cùng tên là type và room. Cách xử lý ở đây tương tự như việc sử dụng callback trong Js.

# 10. Module

## 10.1. Introduction

- Module là 1 nhóm các methods, constants và class variables.
- Module object là instance của class Module.
- 1 module được define bằng keywork `module`.
- Module không có instance, không có phân lớp (subclass) và không có kế thừa.
- Có thể sử dụng `include` hoặc `mix` để sử dụng các method của module trong các class khác.

```ruby
module Person
    def eat
        p "I'm eating"
    end
end

class Woman
    include Person
    
    def walk
        p "I'm waking"
    end
end

class Man
    include Person
    
    def work
        p "I'm working"
    end
end

Woman.new.eat               # "I'm eating"
Man.new.eat                 # "I'm eating"
```

- Class Woman, Man include module Person và có thể sử dụng method của module này.

```ruby
module Person
    def self.eat
        p "I'm eating"
    end
end

Person.eat              # "I'm eating"
```

- Ngoài ra còn có thể sử dụng module độc lập (module function).

## 10.2. Mixins

- Có 3 dạng mixins của module là: include, extends và prepend.
- Khi sử dụng include thì các method của module sẽ được sử dụng trong class dưới dạng instance method.
- Khi sử dụng extends thì các method của module sẽ được sử dụng trong class dưới dạng class method.
- Khi sử dụng prepend thì cơ bản behavior giống với include, tuy nhiên prepend sẽ làm cho các method của module có độ ưu tiên cao hơn method của bản thân class đó.

```ruby
# exclude
module Foo1
	def module_method
		puts "Module Method invoked"
	end
end

class Bar1
	extend Foo1
end

Bar1.module_method #=> Module Method invoked

# include
module FooBar
  def say
    puts "2 - Module"
  end
end

class Foo
  include FooBar

  def say
    puts "1 - Implementing Class"
    super
  end
end

Foo.new.say # =>
            # 1 - Implementing Class
            # 2 - Module

# prepend
module FooBar
  def say
    puts "2 - Module"
    super
  end
end

class Foo
  prepend FooBar

  def say
    puts "1 - Implementing Class"
  end
end

Foo.new.say # =>
            # 2 - Module
            # 1 - Implementing Class
```
