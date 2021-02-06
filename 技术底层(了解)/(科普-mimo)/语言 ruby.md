印象：和python很相像

---

```ruby
action = "victory"
print "Onwards, to " + action + "!"
puts	#额外添加换行

#注意除法
9 / 4	#结果是2 
9 / 4.0	#结果是2.25
```



```ruby
print * 1 .. 5	#12345
print * 1 .. 5	#1234
```





# 语句|控制流

## 条件

```ruby
if <true condition>
elsif
else
end


# 条件表达式
<bool> ? (True):(False)


unless <false condition>
else
end
```





## 循环

```ruby
while <condition>
end


until <condition>
end


for <var> in 1..5
end
```







# 数据类型

数据类型转换

```ruby
<number>.to_s
<string>.to_i	#数字类的string，比如"5"
```





## 字符串

#{}：可以计算，或者替换变量

```ruby
shoes = "sandals"
wear = "I wear #{1 + 1} #{shoes}!"
print wear	#I wear 2 sandals


<string> * <num>
    
#其他方法
.length
.reverse
.capitalize
.upcase
.downcase
```





## 布尔值

```ruby
true
false

=begin
==
!=
> <
>= <=
=end
```

逻辑运算符：

- and &&
- or ||
- !<var>





# 数据结构

## 数组

```ruby
array = []

# 索引@python

# 常用方法
.push()
.length
.sort

<array>.each do |piece|
    puts piece.capitalize
end


3.times {print "Go! "}	#Go! Go! Go!


say = ["I", "love", "arrays"]
say.each {|word| print word + " "}	#I love arrays
```





## Hash

无序的列表：key, value

```ruby
hash = {}
hash = { "name" => "Dipper"}
print hash["name"]

origins = {"Columbus" => "Italian", "Cousteau" => "French"}
origins.each do |key, value|
    puts "#{key} was #{value}"
end


<hash>.each {|tool, action| puts "The #{tool} #{action}"}
<hash>.each_value do |value|
    <exp>
end

#其他方法
.has_key?(<key>)
.has_value?(<key>)
```







# 方法method

```ruby
def puts_numbers
    1..5.each {|num| print num}
end



def greet(greeting, *explorers)	#*是splat operator, 类似python中的*args
    explorers.each do |explorer|
        puts "#{greeting} #{explorer}!"
    end
end
greet("Yo", "Bob", "Maria")



def triple(number)
    return number * 3
end


#返回布尔值的method
def is_gem?(stone)
    if stone == "ruby"
        true
    else
        false
    end
end
```







# 类

- state => attribute
- behavior => method

```ruby
class Explorer
    attr_reader :name	#可读
    def initialize(name)
        @name = name
    end
    
    def eat
        print "Munch!"
    end
end

explorer = Explorer.new("Bob")
print explorer.name	#Bob
explorer.eat



class Explorer
    attr_reader :name, :age, :origin
    def initialize(name, age, origin)
        @name = name
        @age = age
        @origin = origin
    end
    
    def eat
        print "Munch!"
    end
    
    def grow
        @age += 1	#在内部可以改，在外部实例中不能改(因为是只读的state)
end

explorer = Explorer.new("Bob", 30, "Atlantis")
print explorer.name	#Bob
explorer.eat
```







# 模块

```ruby
module Noise
    def make_noise(sound)
        print "#{sound}!"
    end
end

Noise.make_noise("Boo")


class Duck
    include Noise
end
duckling = Duck.new
duckling.make_noise("Quack")


# 定义常数
module Quadruped
    LEGS = 4
end

class Dog
    include Quadruped	#类似python中的继承，只不过这里推广到继承module
    def stand
        "Has #{Quadruped::LEGS} legs!"	#通过::访问module中的变量
    end
end
puppy = Dog.new
puppy.stand
```





组合其他的class, constant或者module => 可以作为mixins, containers, namespaces

```ruby
module Adventure
    TOOLS = ["maps", "compass"]
    class Person
    	attr_reader :name	#可读
    	def initialize(name)
        	@name = name
        end
        def use(tool)
            "#{@name} used the #{tool}!"
        end
    end
end


p = Adventure::Person.new("Bob")
p.use(Adventure::TOOLS[0])
```



```ruby
module Circle
    include Math
    def area(radius)
        radius * radius * PI
    end
end
# 实际使用
include Circle	#类似python中from <module> import <>
print(area(2))	#或者不用include：使用Circle::area(2)

```

