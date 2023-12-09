# File: arrays-and-sets.md

# Check if a value exists in an array in Ruby
[Reference](https://stackoverflow.com/questions/1986386/check-if-a-value-exists-in-an-array-in-ruby)

- You can use `*` to check array membership in a `case` expression.

``` ruby
case element
when *array
  ...
else
  ...
end
```

- Use `Set` when calling it an `include?` on an equivalent Array.
- There is an `in?` method in `ActiveSuport`.
- The methods to find elements in an array:

``` ruby
array.include?(element) # preferred method
array.member?(element)
array.to_set.include?(element)
array.to_set.member?(element)
array.index(element) > 0
array.find_index(element) > 0
array.index { |each| each == element } > 0
array.find_index { |each| each == element } > 0
array.any? { |each| each == element }
array.find { |each| each == element } != nil
array.detect { |each| each == element } != nil
```

# Advantages of Set in Ruby
[Reference](https://stackoverflow.com/questions/36548938/advantages-of-set-in-ruby)

- When doing an `include?`, Set and Hash is way more efficient than Array.
- Set also has things like `superset?`, `intersect?`, `subset?`.
- Arrays:
  - Can have duplicated elements
  - Maintains order
  - Can be iterated over in order
  - Searching for element is slower
  - Maintaining uniqueness of elements is slow
- Sets:
  - Can't have duplicated elements
  - Don't have ordering
  - Searching for element is fast, and unique
- ***If you want to enforce uniqueness and you don't need any ordering - sets are your best choice. When you don't really care about uniqueness and ordering is important - Array is your choice.***

# 7 daily use cases of Ruby Array
[Link](http://blog.8thcolor.com/en/2014/02/7-daily-use-cases-of-ruby-array/)

1. Check if one Array has all elements of another? `array_of_imported_emails - existing_emails`.empty?
2. Elements common to both arrays? `first_array & second_array`
3. Merging two Arrays without duplicating entries? `first | second`
4. Sort by a key, ex: location? `data.sort_by {|hsh| hsh[:location]}`
5. Keep a product unique with respect to one attribute? `products.uniq &:category_id`
6. Filter an Array with a String? `books.grep(/[Rr]ails/)`
7. How to always get an array?

Need to return [], [1], or [1,2]. Always an array.

    def method
       ...
       [*products] or Array(products)
    end


Get last element:  A[-1] or A.last

#### Construction:

## Array.new(initial size, default object) (default object is the same object referenced)

## Create an array with separate objects: a block can be passed instead
## Multi-dimensional: Array.new(3) {Array.new(3)}
## If you don't od this, then you will have the same value for all the elements of the array

#### Accessing
```ruby
    a = [1, 3, 5, 7, 9]

    # Ranges: Two periods = include ending
    a[1..3] = [3, 5, 7]
    # Three periods = don't include ending
    a[1...3] = [3, 5]

    arr[2], arr[2, 4], arr[-9, 3], arr[1..4]
    arr.at(0) #this won't raise an error

    arr.fetch(100) => raises an error if out of bounds
    arr.first, arr.last

    arr.take(3) => returns the first n elements
    arr.drop(3) => gets all ofthe elements after n elements have been dropped

#### Obtaining Information

    arr.length, count, size = same thing
    arr.empty? => check if empty
    arr.include?('konqueror') => check if item is included

#### Adding

    arr << 5
    arr.push(5), arr << 5 => add to end
    arr.unshift(0) => add to any position
    arr.insert (3, 'apple') => add at position 3
```
#### Removing

[What is the easiest way to remove the first element from an array?](http://stackoverflow.com/questions/3615700/ruby-what-is-the-easiest-way-to-remove-the-first-element-from-an-array)

    head, *tail = [1, 2, 3, 4, 5]
    #==> head = 1, tail = [2, 3, 4, 5]

    arr.pop => remove last element and returns it
    arr.shift => retrieve and remove the first item
    arr.delete_at(index) => deletes
    arr.delete(delete somewhere)
    [1,2,3,4,5,6,7,8,9].delete_if{|i| i % 2 == 0}         # delete if
    arr.compact and arr.compact! => removes ni value
    arr.uniq and arr.uniq! => removes duplicates

#### Iterating

    arr.each {|a|} => leaves the array unchanged
    arr.reverse_each {|a|} => reverse order
    arr.map and arr.map! => modifies array

#### Selection

    arr.select {|a| a > 3}
    arr.reject {|a| a > 3}
    arr.drop_while {|a| a < 4}
    arr.delete_if
    arr.keep_if

#### Updating

    a = [1, 3, 5, 7, 9]
    a[2, 2] = 'cat' -> [1, 3, "cat", 9] #The 2 elements at the 2 position become 'cat'
    a[2, 0] = 'dog' -> [1, 3, "dog", "cat", 9] #Since 0, replace 2 position with 'dog'
    a[1, 1] = [9, 8, 7] -> [1, 9, 8, 7, "dog", "cat", 9] #Replace 1 position (length 1) with the array
    a[0..3] = [] -> ["dog", "cat" 9] #Clear from 0 to 3 (inclusive)
    a[5..6] = 99, 98 -> ["dog", "cat", 9, nil, nil, 99, 98] #Pad with null if elements in begin don't exist yet

#### Destructive
    arr.select! {|a| a > 3}
    arr.reject!

#### Public instance methods
    ary & other_ary => combines the elements common to the two arrays, excluding duplicates

    ary * int => triples the array size and fills
    [1, 2, 3] * 3 => [1, 2, 3, 1, 2, 3, 1, 2, 3]

    ary * str => "join"
    [1, 2, 3] * ',' => "1,2,3"

    ary + other_ary => concatenates the arrays
    ary - other_ary => subtracts the arrays

    arry << obj -> push to end of array, returns the array itself to several appends may be chained together


# File: attr_reader_writer_accessor.md

# What is `attr_accessor` in Ruby?
[link](http://stackoverflow.com/questions/4370960/what-is-attr-accessor-in-ruby)

    class Person
      def greet
        @name
      end
    end

`attr_reader` means you can get the `name` variable inside. Right now we cannot do `person = Person.new; person.name` since there is no access. But with the reader,

    class Person
      attr_reader :name

      def greet
        @name
      end
    end

We can do `person.name` (no errors), but we cannot edit what is inside the `name` since it doesn't have the `attr_writer` yet. So we do this:

    class Person
      attr_writer :name

      def greet
        @name
      end
    end

Now we can set `person.name = "yolo"` and have `person.greet` return "yolo". However we cannot do `person.name` since we don't have the `attr_reader`. To be able to do things, you should include both:

    attr_writer :name
    attr_reader :name

Or you could just do:

    attr_accessor :name

Why? Ruby, like Smalltalk, does not allow instance variables to be accessed outside of methods for that object (by default). Instance variables cannot be accessed in the `x.y` form, in Ruby `y` is always taken as a message to send. Thus the `attr_*` methods create wrappers which proxy the instance `@variable access through dynamically created methods.`

`attr_accessor` is just a method, what it does is create more methods for you. These are equivalent:

    class Foo
      attr_accessor :bar
    end

    class Foo
      def bar
        @bar
      end

      def bar=(new_value)
        @bar = new_value
      end
    end



# File: classes-and-methods.md

## RubyMonk

	x.class, 1.class, "".class
	x.is_a?("Integer")
	x.is_a?("String")
	1.class.class 				# Class

For a class to justify its existence, it needs to have two distinct features:

1. State: It defines the attributes of its instances.
2. Behavior: It must do something meaningful.

Example:

		class Rectangle
		  def initialize(length, breadth)
		    @length = length
		    @breadth = breadth
		  end

		  def perimeter
		    2 * (@length + @breadth)
		  end
		  
		  def area
		    @length * @breadth
		  end

		  #write the 'area' method here
		end

#### Methods

Methods are also objects. 

	1.method("next") #<Method: Fixnum(Integer)#next>

Even a method that does nothing at all and has no return produces an object - `nil`.

`Return` returns `nil` if no object is specified.

Splat: Used to handle methods which has a variable parameter list.

	def add(*numbers)
	  numbers.inject(0) { |sum, number| sum + number }
	end

	def add_with_message(message, *numbers)
	  "#{message} : #{add(*numbers)}"
	end

	puts add_with_message("The Sum is", 1, 2, 3)

3rd parameter hash example

	def add(a_number, another_number, options = {})
	  sum = a_number + another_number
	  sum = sum.abs if options[:absolute]
	  sum = sum.round(options[:precision]) if options[:round]
	  sum
	end

	puts add(1.0134, -5.568)
	puts add(1.0134, -5.568, absolute: true)
	puts add(1.0134, -5.568, absolute: true, round: true, precision: 2)

Injects and shit

	def add(*numbers)
		numbers.inject(0) { |sum, number| sum + number }  
	end

	def subtract(*numbers)
	  sum = numbers.shift
	  numbers.inject(sum) { |sum, number| sum - number }  
	end

	def calculate(*arguments)
	  # if the last argument is a Hash, extract it 
	  # otherwise create an empty Hash
	  options = arguments[-1].is_a?(Hash) ? arguments.pop : {}
	  options[:add] = true if options.empty?
	  return add(*arguments) if options[:add]
	  return subtract(*arguments) if options[:subtract]
	end





























# File: combined_doc.md

# File: arrays-and-sets.md

# Check if a value exists in an array in Ruby
[Reference](https://stackoverflow.com/questions/1986386/check-if-a-value-exists-in-an-array-in-ruby)

- You can use `*` to check array membership in a `case` expression.

``` ruby
case element
when *array
  ...
else
  ...
end
```

- Use `Set` when calling it an `include?` on an equivalent Array.
- There is an `in?` method in `ActiveSuport`.
- The methods to find elements in an array:

``` ruby
array.include?(element) # preferred method
array.member?(element)
array.to_set.include?(element)
array.to_set.member?(element)
array.index(element) > 0
array.find_index(element) > 0
array.index { |each| each == element } > 0
array.find_index { |each| each == element } > 0
array.any? { |each| each == element }
array.find { |each| each == element } != nil
array.detect { |each| each == element } != nil
```

# Advantages of Set in Ruby
[Reference](https://stackoverflow.com/questions/36548938/advantages-of-set-in-ruby)

- When doing an `include?`, Set and Hash is way more efficient than Array.
- Set also has things like `superset?`, `intersect?`, `subset?`.
- Arrays:
  - Can have duplicated elements
  - Maintains order
  - Can be iterated over in order
  - Searching for element is slower
  - Maintaining uniqueness of elements is slow
- Sets:
  - Can't have duplicated elements
  - Don't have ordering
  - Searching for element is fast, and unique
- ***If you want to enforce uniqueness and you don't need any ordering - sets are your best choice. When you don't really care about uniqueness and ordering is important - Array is your choice.***

# 7 daily use cases of Ruby Array
[Link](http://blog.8thcolor.com/en/2014/02/7-daily-use-cases-of-ruby-array/)

1. Check if one Array has all elements of another? `array_of_imported_emails - existing_emails`.empty?
2. Elements common to both arrays? `first_array & second_array`
3. Merging two Arrays without duplicating entries? `first | second`
4. Sort by a key, ex: location? `data.sort_by {|hsh| hsh[:location]}`
5. Keep a product unique with respect to one attribute? `products.uniq &:category_id`
6. Filter an Array with a String? `books.grep(/[Rr]ails/)`
7. How to always get an array?

Need to return [], [1], or [1,2]. Always an array.

    def method
       ...
       [*products] or Array(products)
    end


Get last element:  A[-1] or A.last

#### Construction:

## Array.new(initial size, default object) (default object is the same object referenced)

## Create an array with separate objects: a block can be passed instead
## Multi-dimensional: Array.new(3) {Array.new(3)}
## If you don't od this, then you will have the same value for all the elements of the array

#### Accessing

    a = [1, 3, 5, 7, 9]

    # Ranges: Two periods = include ending
    a[1..3] = [3, 5, 7]
    # Three periods = don't include ending
    a[1...3] = [3, 5]

    arr[2], arr[2, 4], arr[-9, 3], arr[1..4]
    arr.at(0) #this won't raise an error

    arr.fetch(100) => raises an error if out of bounds
    arr.first, arr.last

    arr.take(3) => returns the first n elements
    arr.drop(3) => gets all ofthe elements after n elements have been dropped

#### Obtaining Information

    arr.length, count, size = same thing
    arr.empty? => check if empty
    arr.include?('konqueror') => check if item is included

#### Adding

    arr << 5
    arr.push(5), arr << 5 => add to end
    arr.unshift(0) => add to any position
    arr.insert (3, 'apple') => add at position 3

#### Removing

[What is the easiest way to remove the first element from an array?](http://stackoverflow.com/questions/3615700/ruby-what-is-the-easiest-way-to-remove-the-first-element-from-an-array)

    head, *tail = [1, 2, 3, 4, 5]
    #==> head = 1, tail = [2, 3, 4, 5]

    arr.pop => remove last element and returns it
    arr.shift => retrieve and remove the first item
    arr.delete_at(index) => deletes
    arr.delete(delete somewhere)
    [1,2,3,4,5,6,7,8,9].delete_if{|i| i % 2 == 0}         # delete if
    arr.compact and arr.compact! => removes ni value
    arr.uniq and arr.uniq! => removes duplicates

#### Iterating

    arr.each {|a|} => leaves the array unchanged
    arr.reverse_each {|a|} => reverse order
    arr.map and arr.map! => modifies array

#### Selection

    arr.select {|a| a > 3}
    arr.reject {|a| a > 3}
    arr.drop_while {|a| a < 4}
    arr.delete_if
    arr.keep_if

#### Updating

    a = [1, 3, 5, 7, 9]
    a[2, 2] = 'cat' -> [1, 3, "cat", 9] #The 2 elements at the 2 position become 'cat'
    a[2, 0] = 'dog' -> [1, 3, "dog", "cat", 9] #Since 0, replace 2 position with 'dog'
    a[1, 1] = [9, 8, 7] -> [1, 9, 8, 7, "dog", "cat", 9] #Replace 1 position (length 1) with the array
    a[0..3] = [] -> ["dog", "cat" 9] #Clear from 0 to 3 (inclusive)
    a[5..6] = 99, 98 -> ["dog", "cat", 9, nil, nil, 99, 98] #Pad with null if elements in begin don't exist yet

#### Destructive
    arr.select! {|a| a > 3}
    arr.reject!

#### Public instance methods
    ary & other_ary => combines the elements common to the two arrays, excluding duplicates

    ary * int => triples the array size and fills
    [1, 2, 3] * 3 => [1, 2, 3, 1, 2, 3, 1, 2, 3]

    ary * str => "join"
    [1, 2, 3] * ',' => "1,2,3"

    ary + other_ary => concatenates the arrays
    ary - other_ary => subtracts the arrays

    arry << obj -> push to end of array, returns the array itself to several appends may be chained together


# File: attr_reader_writer_accessor.md

# What is `attr_accessor` in Ruby?
[link](http://stackoverflow.com/questions/4370960/what-is-attr-accessor-in-ruby)

    class Person
      def greet
        @name
      end
    end

`attr_reader` means you can get the `name` variable inside. Right now we cannot do `person = Person.new; person.name` since there is no access. But with the reader,

    class Person
      attr_reader :name

      def greet
        @name
      end
    end

We can do `person.name` (no errors), but we cannot edit what is inside the `name` since it doesn't have the `attr_writer` yet. So we do this:

    class Person
      attr_writer :name

      def greet
        @name
      end
    end

Now we can set `person.name = "yolo"` and have `person.greet` return "yolo". However we cannot do `person.name` since we don't have the `attr_reader`. To be able to do things, you should include both:

    attr_writer :name
    attr_reader :name

Or you could just do:

    attr_accessor :name

Why? Ruby, like Smalltalk, does not allow instance variables to be accessed outside of methods for that object (by default). Instance variables cannot be accessed in the `x.y` form, in Ruby `y` is always taken as a message to send. Thus the `attr_*` methods create wrappers which proxy the instance `@variable access through dynamically created methods.`

`attr_accessor` is just a method, what it does is create more methods for you. These are equivalent:

    class Foo
      attr_accessor :bar
    end

    class Foo
      def bar
        @bar
      end

      def bar=(new_value)
        @bar = new_value
      end
    end



# File: classes-and-methods.md

## RubyMonk

	x.class, 1.class, "".class
	x.is_a?("Integer")
	x.is_a?("String")
	1.class.class 				# Class

For a class to justify its existence, it needs to have two distinct features:

1. State: It defines the attributes of its instances.
2. Behavior: It must do something meaningful.

Example:

		class Rectangle
		  def initialize(length, breadth)
		    @length = length
		    @breadth = breadth
		  end

		  def perimeter
		    2 * (@length + @breadth)
		  end
		  
		  def area
		    @length * @breadth
		  end

		  #write the 'area' method here
		end

#### Methods

Methods are also objects. 

	1.method("next") #<Method: Fixnum(Integer)#next>

Even a method that does nothing at all and has no return produces an object - `nil`.

`Return` returns `nil` if no object is specified.

Splat: Used to handle methods which has a variable parameter list.

	def add(*numbers)
	  numbers.inject(0) { |sum, number| sum + number }
	end

	def add_with_message(message, *numbers)
	  "#{message} : #{add(*numbers)}"
	end

	puts add_with_message("The Sum is", 1, 2, 3)

3rd parameter hash example

	def add(a_number, another_number, options = {})
	  sum = a_number + another_number
	  sum = sum.abs if options[:absolute]
	  sum = sum.round(options[:precision]) if options[:round]
	  sum
	end

	puts add(1.0134, -5.568)
	puts add(1.0134, -5.568, absolute: true)
	puts add(1.0134, -5.568, absolute: true, round: true, precision: 2)

Injects and shit

	def add(*numbers)
		numbers.inject(0) { |sum, number| sum + number }  
	end

	def subtract(*numbers)
	  sum = numbers.shift
	  numbers.inject(sum) { |sum, number| sum - number }  
	end

	def calculate(*arguments)
	  # if the last argument is a Hash, extract it 
	  # otherwise create an empty Hash
	  options = arguments[-1].is_a?(Hash) ? arguments.pop : {}
	  options[:add] = true if options.empty?
	  return add(*arguments) if options[:add]
	  return subtract(*arguments) if options[:subtract]
	end





























# File: control-structures.md

## [Making sense with Ruby's "unless"](http://37signals.com/svn/posts/2699-making-sense-with-rubys-unless) and [Unless, The Abused Ruby Conditional](http://www.railstips.org/blog/archives/2008/12/01/unless-the-abused-ruby-conditional/) and [If and Else](http://ruby.bastardsbook.com/chapters/ifelse/)

The words `true` and `false` have special meaning in programming languages. In Ruby, they have the datatypes of `TrueClass` and `FalseClass`, respectively.

These two values – true and false – are not Strings. 

		true == "true" 		# false
		false == "false"	# false

Everything except `false` and `nil` evaluates as `true` by an `if` statement.

#### Variations on `if`

Use `if = ` if the right side either returns something or `nothing at all`. 

	# remember that puts returns nil, so this code block will not execute.
	if x = (puts 'hello world') 
	   puts "Successful assignment. x is now #{x}"
	end





#### Using `unless`

Don't use `unless` with more than a single logical condition.

Avoid negation (`unless !thingie`) because `unless` is already a negate.

Never ever use an `else` with an `unless` statement.

`unless` actually reads better than if ! when used as a statement modifier. 

	raise InvalidFormat unless AllowedFormats.include?(format) # instead of
	raise InvalidFormat if !AllowedFormats.include?(format)

testing `nil?` is bad.

	if foo ... # instead of
	unless foo.nil? ...



# File: enumerable.md

## Ruby Enumerable Magic: The Basics
[link](http://kconrails.com/2010/11/30/ruby-enumerable-primer-part-1-the-basics/)

Ruby's arrays use Enumerable. To use this, we define the `each` method:

    class Team
      include Enumerable

      attr_accessor :members

      def initialize
        @members = []
      end

      def each(&block)
        @members.each(&block)
      end
    end

Basically what happens is that every time you call a `Team.each`, you iterate over the `@members`. (It is really common to just do this with the `Enumerable` module, forwarding the each over to an instance variable.) You are also able to use the other collection class methods (`map`, `filter`, etc...) because you defined the `each` method.

#### Unary Ampersand

There is no `&:` operator in Ruby. *What you see in `&:capitalize` is `&` and `:capitalize` pushed together. The first character is the unary ampersand, the second is a Ruby symbol being passed to the operator.*

When Ruby sees the unary ampersand on the last argument of a method, it tries to convert it to a proc and run it.

    class Translator
      def speak &language
        language.call(self)
      end

      protected

      def french
        'bon jour'
      end

      def spanish
        'hola'
      end

      def turkey
        'gobble'
      end

      def method_missing *args
        'awkward silence'
      end
    end

    translator.speak(&:spanish) #=> "hola"
    translator.speak(&:turkey) #=> "gobble"
    translator.speak(&:italian) #=> "awkward silence"

## Enumerable API

> The `Enumerable` mixin provides collection classes with several traversal and searching methods, and with the ability to sort. The class must provide a method `each`, which yields successive members of the collection.

#### Filtering

`all?`: Returns true if block never returns false or nil.

    %w[ant bear cat].all? { |word| word.length >= 3 } #=> true

`any?`: Returns true if block HAS a true

    %w[ant bear cat].any? { |word| word.length >= 4 } #=> true

`chunk`: Better to use if the array is sorted. Literally creates chunks nung mga magkakatabi na pasok sa condition.

`detect(ifnone = nil){|obj| block }`: Return first match, if no object matches, call ifnone. Aliased as find.

`find_all{|obj| block}`: Filter out all that follows the condition.

    (1..10).find_all { |i| i % 2 == 0 } # Get all evens

`find_index(value), find_index {|obj| block }`: Returns the index of the first matcher.

`first, first(n)` (aliased as `take(n)`): Get first n elements.

`grep(pattern)`: Returns an array of very element for which Pattern === element.

    IO.constants.grep(/SEEK/) #=> [:SEEK_SET, :SEEK_CUR, :SEEK_END]

`include?(obj)`: Return if enum contains obj, equality is tested via `==`.

`reject{ |obj| block }`: Return all elements for which the given block returns false.

`partition { |obj| block }`: Returns two arrays, the first containing the elements of enum for which the block evaluates to true, the second containing the rest.

`select { |obj| block }`: Return array containing elements who satisfy condition.

`take_while { |arr| block }`: Pass element to the block until the block returns `nil` or `false`, then stops iterating and returns an array of all prior elements.

    a = [1, 2, 3, 4, 5, 0]
    a.take_while { |i| i < 3 }   #=> [1, 2]

#### Iterate over

`collect` (aliased as `map`): Returns a new array with the results of the block once for every element in the enum.

`collect_concat` (Aliased as `flat_map`): Returns a new array with the concatenated results of running block once.

    [1, 2, 3, 4].flat_map { |e| [e, -e] } #=> [1, -1, 2, -2, 3, -3, 4, -4]

`cycle(n=nil){}`: Continuously call the block for each element of enum repeatedly n times or forever if none or nil is given as n. If no block is given, an enumerator is returned.

    [a, b, c].cycle { |x| puts x }  # print, a, b, c, a, b, c,.. forever.
    [a, b, c].cycle(2) { |x| puts x }  # print, a, b, c, a, b, c.

`each_cons(n){}`: Iterate the given block for each element and the next n elements.

    (1..10).each_cons(3) { |a| p a }
    # outputs below
    [1, 2, 3], [2, 3, 4], ... [8, 9, 10]

`each_slice(n){}`: Iterate over each slice of `<n>` elements. If no block is given, return an enumerator.

    (1..10).each_slice(3){|a| p a} [1, 2, 3] [4, 5, 6] [7, 8, 9] [10]

`each_with_index`: each but has an index passed in

`each_with_object(obj){|elem, obj| a << i * 2}`: Iterate over the given block for each element with an arbitrary object, and reutrn the initially given object.

    (1..10).each_with_object([]) { |i, a| a << i*2 }
    [2, 4, 6 ... 20]

>What happens is that the object a, which is initially a [], has each of the indices multiplied by 2 appended to i.

`reverse_each(*args){ |item| block }` # Builds a temporary array and traverses that array in reverse order.

#### Finding out something

`count`, `count {|obj| block }`: Count objects that qualify

`max`, `max{ |a, b| block }`: Return the object in enum using the comparator.

`minmax {|a, b| block }`, `minmax_by { |a, b| block }`: Return 2 element array which contains the minimum and maximum value in the enumerable.

`none?[{ |obj| block }]`: Return `true` if the block never returns `true` for all elements.

    %w{ant bear cat}.none? { |word| word.length == 5 } #=> true
    [nil].none?                                        #=> true

`one?[{ |obj| block }]`: Return `true` if the block returns `true` exactly once.

#### Perform operation

    (1..7).to_a ????

`inject(initial){ |memo, obj| block }`: My personal favorite! So verbatim that shit. :)

Combines all elements of enum by applying a binary operation, specified by a block or a symbol that names a method or operator.

If you specify a block, then for each element in enum the block is passed an accumulator value (memo) and the element. If you specify a symbol instead, then each element in the collection will be passed to the named method of memo. In either case, the result becomes the new value for memo. At the end of the iteration, the final value of memo is the return value for the method.

If you do not explicitly specify an initial value for memo, then the first element of collection is used as the initial value of memo.

    # Sum some numbers
    (5..10).reduce(:+)                             #=> 45
    # Same using a block and inject
    (5..10).inject { |sum, n| sum + n }            #=> 45
    # Multiply some numbers
    (5..10).reduce(1, :*)                          #=> 151200
    # Same using a block
    (5..10).inject(1) { |product, n| product * n } #=> 151200
    # find the longest word
    longest = %w{ cat sheep bear }.inject do |memo, word|
       memo.length > word.length ? memo : word
    end
    longest                                        #=> "sheep"

`reduce(initial){ |memo, obj| block }`: Near to reduce.

    # Sum some numbers
    (5..10).reduce(:+)                             #=> 45
    # Same using a block and inject
    (5..10).inject { |sum, n| sum + n }            #=> 45

`sort { |a, b| block }`, `sort_by`: Return a sorted array, block should return -1, 0, and +1, depending on the comparison between a and b.

    %w(rhea kea flea).sort          #=> ["flea", "kea", "rhea"]
    (1..10).sort { |a, b| b <=> a }  #=> [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]

`zip(arg, ...)`: An array of array.

    a = [ 4, 5, 6 ]
    b = [ 7, 8, 9 ]

    [1, 2, 3].zip(a, b)      #=> [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
    [1, 2].zip(a, b)         #=> [[1, 4, 7], [2, 5, 8]]
    a.zip([1, 2], [8])       #=> [[4, 1, 8], [5, 2, nil], [6, nil, nil]]

#### Grouping

`group_by{|obj| block}`: Groups the collection by result of the lock and returns a hash wehre the keys are the evaluated result from the block and the values are arrays of elements in the collection that correspond to the key.

    (1..6).group_by { |i| i%3 }   #=> {0=>[3, 6], 1=>[1, 4], 2=>[2, 5]}

#### Delete stuff

`drop(n)`: Drop first n elements and return the rest in an array.

`drop_while{|arr| block}`: Drop element up to, but not including, the first element for which the block returns nil or false and returns an array containing the remaining elements.

    a = [1, 2, 3, 4, 5, 0]
    a.drop_while { |i| i < 3 }   #=> [3, 4, 5, 0]

#### Misc

`lazy`. Read this shit muna.

`to_a`, `to_h`: Conversion and shit to hashes.

WTF

each_entry (PI this)
slice_before()


# File: equality.md

## [What's the difference between equal?, eql?, ===, and ==?](http://stackoverflow.com/questions/7156955/whats-the-difference-between-equal-eql-and)

	== 		-> generic "equality", if they are the same objet. Overriden.
	=== 	-> commonly overriden, check the === of Range, Regex, and Proc
	eql?	-> generic/alternative equality
	equal?	-> identity comparison. It's like a pointer comparison. Don't override.


# File: exceptional-ruby.md

# Exceptional Ruby

In most test suites, happy path tests predominate, and there may only be a few failure case tests. Most mature codebases are riddled with the telltale times of hastily-patched failure cases--business logic that is interrupted again band again by `nil`-checkes and `begin...rescue...end` blocks.

An *exception* is the occurence of an abnormal condition during the execution of a software element.

A *failure* is the inability of a software element to satisfy its purpose.

An *error* is the presence in the software of some element not satisfying its specification.

Bertrand Meyer: All methods have a contract, either implicit or explicit, with their callers. A method can be said to have failed when it has failed to fulfill this contract.

*Contract: "Given the following inputs, I promise to return certain outputs and/or cause certain side-effects." It is the caller's responsibility to ensure that the method's preconditions (the inputs it depends on) are met. It is the method's responsibility to ensure that its postconditions (outputs and side effects) are fulfilled.*

It is also the methods' responsibility to maintain the invariant of the object it is a member of. The invariant is the set of conditions that must be met for the object to be in a consistent state.

*When a method's preconditions are met, but is unable to either deliver on its promised postconditions, or to maintain the object invariant, then it is in breach of its contract; it has failed.*

## Reasons for Failure

- Mistake in implementation (ex: hash indexed by String instead of Symbol).
- Program runs out of resources.
- Web service fails.

# Exception Life Cycle

`raise` and `fail` are synonyms. Jim Weirich: Use `fail` for first time and `raise` for second time.

    raise [EXCEPTION_CLASS], [MESSAGE], [BACKTRACE]
    raise RuntimeError # RuntimeError is raised when nothing is passed in as an EXCEPTION_CLASS

`raise` can be overrode because it is implemented by `Kernel`.

# `raise` Internals

1. Call `#exception` to get the exception object.
2. Set the backtrace.
3. Set the global exception variable. The `$!` and `$ERROR_INFO` global variable always contains the currently active exception if any.
4. Raise the exception object up the call stack. Exception will look for `ensure` or `rescue`.

`ensure` classes will always be executed, whether an exception is raised or not. BTW a return from a method inside an `ensure` takes precedence over any exception being raised, and the method will return as if no exception has been raised at all.

# Coming to the `rescue`

It doesn't capture: `NoMemoryError`, `LoadError`, `NotImplementedError`, `SignalException`, `Interrupt`, `ScriptError`. Exceptions outside of `StandardError` (these are conditions that can't reasonably be handled by a generic catch-all `rescue`).

You can use `rescue` in the same way that you use `if` or `unless`.

    f = open("nonesuch.txt") rescue nil

`retry`

    tries = 0
    begin
      tries += 1
      puts "Trying #{tries}"
      raise "Didn't work"
    rescue
      retry if tries < 3
      puts "I give up"
    end

`raise` during exception handling: This throws away the original exception. There is no way to retrieve the original exception. Use Nested exceptions.

[TODO]: NESTED_EXCEPTIONS.

`else` after a `rescue` clause is the opposite of `rescue`; where the `rescue` clause is only hit when an exception is raise, `else` is only hit when no exception is raised by the preceding code block.

## Uncaught exceptions

When an exception is never rescued, the stack will unwind completely and Ruby will handle the un-rescued exception by printing a stack trace and terminating the program. However, before the program ends, Ruby will execute various exit hooks:

    trap
    at_exit
    END

Crash logger implemented with `at_exit`:

    at_exit do
      if $! # Check if the exit is because of an exception
        open('crash.log', 'a') do |log|
          error = {
            :timestamp => Time.now,
            :message => $!.message,
            :backtrace => $!.backtrace,
            :gems => Gem.loaded_specs.inject({}){m, (n,s)| m.merge(n => s.version)}
            }
          }
          YAML.dump(error, log)
        end
      end
    end

## Are Ruby exceptions slow?

There is no penalty simply for having exception-handling code lying around dormant. On the other hand, writing code that uses exceptions as part of its logic can have a significant performance cost.

# Responding to Failures

*Failure flags and benign values.* A `nil` isn't very communicative but in some cases it may be all that's needed.

When the system's success doesn't depend on the outcome of the method in question, using a benign value may be the right choice.

    begin
      response = HTTP.get_response(url)
      JSON.parse(response.body)
    rescue Net::HTTPError
      { "stock_quote" => "<Unavailable>" }
    end

*Remote failure reporting.* These can be in a central log server, an email to the developers, or a post to a third-party exception-reporting service such as Hoptoad, Exceptional, or New Relic RPM.

*Bulkhead.* By partitioning your systems, you can separate a system's errors from the other parts. Ex: `rescue` nothing.

    begin
      SomeExternalService.some_request
    rescue Exception => error
      logger.error "Exception at..."
      logger.error error.message
      logger.error error.backtrace.join("\n")
    end

*The Circuit Breaker pattern.*

This is a mechanism that controls the operation of a software subsystem and behaves like a physical circuit breaker. Thee states:

- *Closed.* Subsystem is allowed to operate normally. However, a counter tracks the number of failures that have occurred, and when the number exceeds a threshold, the breaker trips, and enters the open state.
- *Open.* The subsystem is not permitted to open.
- *Half-open.* The system can run, but a single failure will send it back into the "open" state.

# Alternatives to Exceptions

There are situations when failing fast may not yield the best results. Ex: When proceeding with a script to set up a server, you may fail on a "keyfile download" exception when you can't access the key data. What can happen is you need a way to proceed through the steps of the process then get a report at the end telling you what parts succeeded and what parts failed.

*Sideband data.* A Every method has a primary channel of communicating data back to the user, the return value. The side band is a secondary channel of communication for reporting meta-information about the status and disposition of a process.

*Multiple return values.* `return [result, succcess]` where `success` can be true or false. Problem is that receivers need to expect an array.

*Output parameters.* These aren't output parameters in the traditional sense of a parameter.

    def make_user_accounts(host, transcript = StringIO.new)
      transcript.puts "* Making user accounts..."
    end

    def install_packages(host, transcript = StringIO.new)
      transcript.puts "* Installing packages..."
    end

    def provision_host(host, transcript)
      make_user_accounts(host, transcript)
      install_packages(host, transcript)
    end

    transcript = StringIO.new
    provision_host("192.168.1.123", transcript)
    puts "Provisioning"
    puts transcript.string

*Caller-supplied fallback strategy.* If we're not sure we want to terminate the execution of a long process by raising an exception, we can inject a failure policy into the process in the same way that we injected a transcript above.

    def make_user_accounts(host, failure_policy=method(:raise))
      # ...
      rescue => error
        failure_policy.call(error)
    end

    def install_packages(host, failure_policy=method(:raise))
      # ...
      raise "Package ’foo’ install failed on #{host}"
      rescue => error
        failure_policy.call(error)
    end

    def provision_host(host, failure_policy)
      make_user_accounts(host, failure_policy)
      install_packages(host, failure_policy)
    end

    policy = lambda {|e| puts e.message}
    provision_host("192.168.1.123", policy)

*Global variables.* We can designate a global variable to contain the error code for the most recently executed function.

    class Provisioner
      def provision
        # ...
        (Thread.current[:provisioner_errors ||= []) << "Error getting key file..."
      end
    end

    p = Provisioner.new
    p.provision

    if Array(Thread.current[:provisioner_errors]).size > 0 # Coerce the thread into an array
      # handle failures
    end

*Process reification.* The final/cleanest solution is to represent the process itself as an object, and give the object an attribute for collecting status data.

    class Provisionment
      attr_reader :problems

      def initialize
        @problems = []
      end

      def perform
        @problems << "Failure..."
      end
    end

    p. Provisionment.new
    p.perform
    if p.problems.size > 0
      ...
    end

This avoids multiple return values and global namespace pollution of using thread-local variables. The drawback is adding another class to the system.

# Your failure handling strategy

*Exceptions shouldn't be expected.* Use exceptions only for exceptional situations. Ex: `ActiveRecord#save` doesn't raise an exception when the record is invalid, because invalid user input isn't that unusual.

The Pragmatic Programmer: "Will this code still run if I remove all exception handlers? If the answer is "no", then maybe exceptions are being used in non-exceptional circumstances."

[TODO]: USE_THROW_FOR_EXPECTED_CASES

*What constitutes an exceptional case?* (EOF? Missing key in a hash? 404 status code from a web service?) It depends! When we raise an Exception, we force the caller to treat the condition as an exceptional case, whether the caller considers it to be one or not.

## Caller-supplied fallback strategy.

*In most cases, the caller should determine how to handle an error, not the callee.*

Ex: `Array#fetch`. If the collection is not able to find a value, it yields to the provided block.

    h.fetch(:optional_key) { DEFAULT_VALUE } # Use default value if key is not found
    h.fetch(:required_key) { raise "Required key not found!" }

Other ex: `Enumerable#detect`. Since block as already taken, it uses a parameter for its caller-specified feedback handler.

    arr.detect(lambda({"None found"}) {|x| ... } # The lambda param is yielded when nothing is detected.

Ex in Rails:

    def render_user(user)
      if user.name && user.lname
        "#{user.lname}, #{user.fname}"
      else
        yield
      end
    end

    # Fall back to a benign placeholder value:
    render_user(u) { "UNNAMED USER" }

    # Fall back to an exception
    render_user(u) { raise "User missing a name" }

## Questions to ask before raising

*Is the situation truly unexpected?* Do we really need to raise an exception when the user fails to answer YES or NO to a question? Maybe we can just loop until we get a sensible response. *User input is a classic example of a case where we expect mistakes. Is this really an unexpected case, or can you reasonably predict it will happen during normal operation?*

*Am I prepared to end my program?* An exception, if it goes unhandled, can potentially end the program (or in web apps, end the request). Maybe you can substitute some kind of benign value for the unexpected result.

    @ug = UserGreeting.find_by_name!("winter")

    @ug UserGreeting.find_by_name("winter")
    unless @ug
      logger.error "Someone forgot to run db:populate!"
      @ug = OpenStruct.new(:welcome => "Hello")
    end

*Can I punt the decision up the call chain?* Do I want to capture and log failure instead of aborting?

*Am I throwing away valuable diagnostics?A When you have just received the results of a long, expensive operation, that's probably not a good time to raise an exception because of a trivial formatting error. You want to preserve as much of that information as much as possible. See ifyou can enable the code to continue normally while noting the fact that there was a problem, using a kind of sideband.*

*Would continuing result in a less informatiive exception?* Sometimes, failing to raise an exception just results in things going wrong in less easy-to-diagnose ways down the road. In cases like this, it's better to raise earlier than later.

Compare this:

    response_code = might_return_nil()
    message = codes_to_messages[response_code]
    response_code = "Status: #{message}" # We can get a can't convert nil to string error here

...to this:

    response_code = might_return_nil() or raise "No response code"

*Isolate exception handling code.* An exception represents an immediate, non-local transfer of control (it's a kind of cascading `goto`). Programs that use exceptions as part of their normal processing suffer from all the readability and maintainability problems of classic spaghetti code.

I consider `begin` keyword to be a code smell in Ruby. The better way is to use Ruby's implicit begin blocks.

    def foo
      ... mainline logic
    rescue
      ... failure handling
    end

Isolating exception policy with a Contingency Method. A contingency method cleanly separates business logic from failure handling. Ex, in a code base which makes numerous calls which might raise an `IOError`:

    def with_io_error_handling
      yield
    rescue
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }
    with_io_error_handling { something_else_that_might_fail }

## Exception Safety

When it is vital that a method operate reliably, it becomes useful to have a good understanding  of how that method will behave in the face of unexpected Exceptions being raise.

There are three defined levels of exception safety:

- The weak guarantee. If an exception is raised, the object will be left in a consistent state.
- The strong guarantee. If an exception is raised, the object will be rolled back to its beginning state.
- The nothrow guarantee. No exceptions will be raised from the method. If an exception is raised during the execution, it will be handled internally.
- Implicit fourth level: No guarantee lol.

*Be specific when eating Exceptions.*

Don't do this:

    begin
    rescue Exception
    end

Sometimes you're forced to catch an overly broad exception type, but code such as the above example is prone to bugs. You'll often spend a long time trying to figure out why the code is not doing what it's supposed to do, only to discover that an exception is being raise early--and then being caught and hidden by the over-eager `rescue` clause.

If you can't match the class of an Exception, try to at least match the message.

    begin
      # ...
    rescue => error
      raise unless error.message =~ /foo bar/
    end

*Namespace your own exceptions.*

Every library codebase should have, at the very least, a definition such as the following:

    module MyLibrary
      class Error < StandardError; end
    end

That gives client code something to match on when calling into the library. It is suggested to put code in the top-level API calls,

[TODO]: TAGGING_EXECPTIONS_WITH_MODULES

## Three essential exception classes

- We could divide Exceptions up by which module or subsystem they come from.
- We might break them down by software layer--e.g. separate exceptions for UI level failures and for model-level failures.
- We could try basing them on severity levels: separate exceptions for fatal problems and nonfatal problems.

Why different exception classes?

- Want tot attach extra information to the exception, like an error code.
- Want to handle the exception differently. "Handling" means specific actions or presenting the failure to the user in a particular way.

Let's look at the different ways failures are presented to the user:

1. "You did something wrong." This message tells the user that the only way to fix the problem is for them to do something differently.
2. "Something went wrong inside the app." This tells the user that there is nothing they can do to fix the problem; it's an internal error.
3. "We are temporarily over capacity." Nothing is broken, but the system is temporarily over capacity and the problem will most likely resolve itself eventually.

## Three classes of exception

1. User error.
2. Logic error. Error in the system.
3. Transient failure. Something is over capacity or temporarily offline. Usually handled by giving the user a hint about when to come back and try again, or in the case of batch jobs, by arranging to retry the failed operation a little layer.

*I find that if a library or app defines these three exception types, they are sufficient for 80% of the cases where an exception is warranted.*

Ex:

    failures = 0
    begin
      ...
    rescue MyLib::UserError => e
      puts e.message
      puts "Please try again"
      retry
    rescue MyLib::TransientFailure => e
      failures += 1
      if failures < 3
        warn e.message
        sleep 10
        retry
      else
        abort "Too many failures"
      end
    rescue MyLib::LogicError => e
      log_error(e)
      abort "Internal error! #{e.message}"


# File: exceptions.md

# Why is it bad style to `rescue Exception => e` in Ruby?
[Reference](https://stackoverflow.com/questions/10048173/why-is-it-bad-style-to-rescue-exception-e-in-ruby?noredirect=1&lq=1)

- `Exception` is the root of Ruby's exception hierarchy, so when you `rescue Exception`, you rescue from everything, including subclasses such as `SyntaxError`, `LoadError`, and `Interrupt`.
  - Rescuing `Interrupt` prevents the user from using CTRLC to exit the program.
  - Rescuing `SignalException` prevents the program from responding correctly to signals. It will be unkillable except by `kill -9`.
  - Ruby's default behavior is to rescue from `StandardError`.
- The case for rescuing from `Exception`: for logging/reporting purposes, just so you can re-raise it.

``` ruby
begin
  # iceberg?
rescue Exception => e
  # do some logging
  raise e  # not enough lifeboats ;)
end
```

- There are gems that inherit from Exception (Why?).
- Rescuing `Exception` will hide bugs such as `NameError` or `NoMethodError` if you mistyped a method name.

# Catch all exceptions in a rails controller
[Reference](https://stackoverflow.com/questions/3694153/catch-all-exceptions-in-a-rails-controller?noredirect=1&lq=1)

# List of Exceptions - Exceptional Ruby

- `StandardError`: This class and all its subclasses will be rescued by a default `rescue` clause.
- `RuntimeError`: The exception class we get when we call `raise` or `fail` without an explicit class. In a sense, `RuntimeError` is Ruby's miscellaneous exception. It says nothing about the failure except that there was one.
- `NoMemoryError`
- `ScriptError`: We have subclasses `LoadError` and `SyntaxError` to indicate failures in loading and executing Ruby scripts.
- `SignalException`: These are raised when a Ruby process is signaled by the OS. (A big infrequent.)
- `IOError`
- `ArgumentError, RangeError, TypeError, IndexError`: Method was called incorrectly.
- `SystemCallError`: You'll never see this, what you will see are its descendants, each of which are named `Errno::<ERROR SYMBOL>`, where `<ERROR SYMBOL>` is the symbolic name for a system error code.

# Jim Weirich on Exceptions
[link](http://devblog.avdi.org/2014/05/21/jim-weirich-on-exceptions/)

"When you call a method, you have certain expectations about what the method will accomplish. Formally, these expectations are called post-conditions. Formally, these expectations are called post-conditions. A method should through an exception whenever it fails to meet its postconditions."

This implies a small understanding of Design by Contract and the meaning of pre- and post-conditions.

    model.save!

If the model is not saved for some reason, then an exception must be raised because the post-condition is not met. If save doesn't actually save, then the returned result will be false, but the post-condition is still met, so no exception.

The only time you would want to rescue/rethrow is when you have a job half-way done and you want to undo something to avoid a partially complete state. Your strategic rescue points should be chosen carefully so that the program can continue with other work even if the current operation failed. A Rails app should recover and be ready to handle the next HTTP request.

*Most exception handlers should be generic. Since exceptions indicate a failure of some type, then the handler needs only make a decision on what to do in case of failure.* Detailed recovery operations for very specific exceptions are generally discouraged unless the handler is very close to the point of the exception.

*Exceptions should not be used for flow control. Use throw/catch for that. Reserve exceptions for true failure conditions.*


# File: forwardable.md

## Shorter, Simpler Code with Forwardable
[link](http://www.saturnflyer.com/blog/jim/2014/11/15/shorter-simpler-code-with-forwardable/)

    class Person
      def street
        address.street
      end

      def city
        address.city
      end

      def state
        address.state
      end
    end

Ruby has a built-in way to bring the information out of this code: Forwardable.

    require 'forwardable'
    class Person
      extend Forwardable

      delegate [:street, :city, :state] => :address
    end

*With this code, the concept of forwarding a message to a collaborating object is so clear that it reads like configuration. Good configuration is fast and easy to understand.*

#### Simplified Implementation

    module Forwardable
      def delegate(hash)
        hash.each{ |methods, accessor|
          methods.each{ |method|
            instance_eval %{
              def #{method}(*args, &block)
                #{accessor}.__send__(:#{method}, *args, &block)
              end
            }
          }
        }
      end
    end

The "delegate" method accepts a hash where the keys are the method names to forward, and the values are the object names to receive the forwarded messages.


# File: hashes.md

# Ruby API, Pickaxe Book, RubyMonk

# Tipz
[Link](http://blog.8thcolor.com/en/2014/03/7-daily-use-cases-of-ruby-hash/)

1. JSON -> Hash: `JSON.parse(data)`
2. Hash -> JSON: `require json; .to_json()`
3. Set default value: `contacts['Jane'][:email] = "jane@doe.com"`. Your default value for `[:`

        contacts.default_proc = Proc.new do |hsh, key|
                hsh[key] = {
                name: key,
                email: ''
            }
        end

4. Merging two nested Hashes: Use `deep_merge` from ActiveSupport.
5. Filtering out some keys using ActiveSupport: `SOME_ARRAY.except(:saturday, :sunday)`
6. Sorting: `sort_by`.
7. Finding the differences between two Hashes: `new_entries = updated_entries.reject{|k, _| entries.include? k}`.

## API

- Hashes enumerate their values in the order that the corresponding keys were inserted.
- We can create hashes via strings and via symbols. `grades = { "Jane Doe" => 10, "Jim Doe" => 6 }` or `options = { :font_size => 10, :font_family => "Arial" }`.
- Hashes also have a default value, declared on `grades = Hash.new(0)`. This is accessible via the `grades.default` method.

*Equality method*
```ruby
  def ==(other)
    self.class === other and
      other.author == @author and
    other.title == @title
  end

*Instance Methods*

  h = {"colors"  => ["red", "blue", "green"], "letters" => ["a", "b", "c" ]}

    h.assoc("letters")                          # => ["letters", ["a", "b", "c"]]
    h.clear                                     # => {}
    h.default                                   # => {}
    h.default = 5                               # => 5
    h.default_proc Hash.new{|h,k| h[k] = k * k} # =>                                                                # Proc If Hash::new was invoked with a block, return that block.
    h.delete("colors")                          # => ["red", "blue", "green"]
    H.delete("dqdq"){|e1| "                     # {e1} not found"}                                                  # => "dqdq not found"                                            # return block if key doesn't exist
    h.delete_if{|k, v| k >= "b"}                # => delete all where block evaluates to true

    h.each{|k,v| block}
    h.each_pair{|k, v| block}
    h.each                                      # => an_enumerator
    h.each_pair                                 # => an_enumerator

    h.each_key{|k| block}                       # => calls block once for each key in hsh, passing the key as param
    h.each_pair{|k, v| block}
    h.each_value{|v| block}                     # => calls block once for each key, passing in value
    h.empty?
    h.equal?                                    # => same content
  h.fetch(key, [, default])               # => ret default if not exist
  h.flatten

`each_with_index` awesomeness

  hash.each_with_index{|(k, v), index}

## Initialization

  # Unorthodox

  chuck_norris = Hash[:punch, 99, :kick, 98, :stops_bullets_with_hands, true]
  def artax
    a = [:punch, 0]
    b = [:kick, 72]
    c = [:stops_bullets_with_hands, false]
    key_value_pairs = a, b, c
    Hash[key_value_pairs]
  end

`each` has to be done in-place...

  restaurant_menu = { "Ramen" => 3, "Dal Makhani" => 4, "Coffee" => 2 }
  restaurant_menu.each do |item, price|
    restaurant_menu[item] = price + (price * 0.1)
  end

NOT
  restaurant_menu = { "Ramen" => 3, "Dal Makhani" => 4, "Coffee" => 2 }
  restaurant_menu.each do |item, price|
    price = price + (price * 0.1)
  end
```


Also known as associative arrays, maps, dictionaries, and are similar to arrays in that they are indexed collections of object references.

You can use any object as an index, but their elements are not ordered, so you cannot easily use a hash as a stack or a queue.

You can’t have multiple keys with the same value.

For the song list, we can define the [] method, to get the songs inside.
```ruby

class SongList(index)
  def [](index)
    @songs[index]
  end
end
Iterators
def with_title(title)
  songs.find {|song| title == song.name}
end

Each vs. collect: Collect takes each element from the collection and passes it to the block. The results returned by the block are used to construct a new array.

  Inject: Set sum’s initial value, and iterate over the array using the inject.
  [1, 3, 5, 7].inject(0) {|sum, element| sum + element}
  [1, 3, 5, 7].inject(1) {|product, element| product * element}

# Blocks as code blocks

    songlist = SongList.new
    class JukeboxButton < Button
        def initialize(label, &action)
            super(label) 
            @action = action
        end
        def button_pressed
            @action.call(self)
        end
    end

start_button = JukeboxButton.new("Start") { songlist.start } pause_button = JukeboxButton.new("Pause") { songlist.pause }
```

You can essentially pass a closure or a function inside. When using an ampersand, you are telling Ruby to look for a code block whenever that method is called.

restaurant_menu = { "Ramen" => 3, "Dal Makhani" => 4, "Coffee" => 2 }
restaurant_menu.each do |item, price|
  restaurant_menu[item] = price + (price * 0.1)
end


# File: hook_methods.md

## Ruby's Important Hook Methods
[link](http://www.sitepoint.com/rubys-important-hook-methods/)


# File: io.md

## RubyMonk, Pickaxe

"Pure" code is code without side-effects (code that just performs calculations).

	# open the file "new-fd" and create a file descriptor:
	fd = IO.sysopen("new-fd", "w")

	# create a new I/O stream using the file descriptor for "new-fd":
	p IO.new(fd)

There are a bunch of I/O streams that Ruby initializes when the interpreter gets loaded.

	io_streams = Array.new
	ObjectSpace.each_object(IO) { |x| io_streams << x }

	p io_streams # Medyo marami

Ruby defines constants STDOUT, STDIN and STDERR that are IO objects pointing to your program's input, output and error streams that you can use through your terminal, without opening any new files. 

	p STDOUT.class
	p STDOUT.fileno
	  
	p STDIN.class
	p STDIN.fileno

	p STDERR.class 
	p STDERR.fileno

Whenever you call `puts`, the output is sent to the `IO` object that `STDOUT` points to. It is the same for `gets`, where the input is captured by the `IO` object for `STDIN` and the `warn` method which directs to `STDERR`.

The Kernel module provides us with global variables $stdout, $stdin and $stderr as well, which point to the same IO objects that the constants STDOUT, STDIN and STDERR point to. We can see this by checking their object_id.

	p $stdin.object_id is the same as p STDIN.object_id
	p $stdout.object_id is the same as p STDOUT.object_id
	p $stderr.object_id is the same as p STDERR.object_id

Whenever you call `puts`, you're actually calling `Kernel.puts` (methods in `Kernel` are accessible everywhere in Ruby), which in turn calls `$stdout.puts`.


[TODO]


	gets: reads a line from standard input
	file.gets: reads a line from the file object file

Iterators

Spit out exceptions if the files don’t exist.

	File.open("testfile") do |file|
		file.each_byte {|ch| putc ch; print "." }
	end

#### Per line

File.open("testfile") do
      file.each_line {|line|
end

#### Entire file: String, or array of lines

	str = IO.read("testfile")
	str.length → 66
	arr = IO.readlines("testfile")
	arr.length → 4
	Talking to Networks ???
	require 'socket'
	client = TCPSocket.open('127.0.0.1', 'finger')
	client.send("mysql\n", 0) # 0 means standard packet puts client.readlines
	client.close


# File: keyword-arguments.md

## RubyConf 2016 - Keyword Args — the killer Ruby feature you aren't using by Guyren G. Howe
[Reference](https://www.youtube.com/watch?v=4e-_bbFjPRg)

``` ruby
def foo(x, y=3, *rest, opt2:, opt1: nil, **c, &block)

Positional required
Positional optional
Positional arg splat
Kwarg required
Kwarg argument
Kwarg splat
Block
```

- Keyword arg can refer to an earlier keyword arg for its value.
- Positional args before kwargs, if your last positional arg is a hash, Ruby can get confused.
- Clearer: Unless you work with `Net::HTTP.new('server', 732, 'other_server.com', 872, 'admin', 'monkey123')`, you won't know what this is.
- Programming Ruby book: because we all use the conventions in this book, it is easy for us to communicate with other Ruby programmers. Problem is, it's an old (15 years) book.

``` ruby
Net::HTTP.new(
  server: 'server.com',
  port: '732',
  proxy: 'other_server.com'
)
```

- The clear advantage to kwargs = they are more flexible.
- Adding arguments to easier in kwargs.
- "Loose coupling" and "dependency injection".
- "Context funnel": Receive contexts with this type of method signature:

``` ruby
def (required_argument, **context) -> This context can be passed and passed in the call chain.
```


# File: lambdas-and-blocks.md

## RubyMonk Dive, Ascent, [Blocks and Yields in Ruby](http://stackoverflow.com/questions/3066703/blocks-and-yields-in-ruby?rq=1), [Understanding Ruby Blocks, Procs and Lambdas](http://www.robertsosinski.com/2008/12/21/understanding-ruby-blocks-procs-and-lambdas/)

A lambda is just a function without a name. They can be assigned to variables.

	l = lambda { "Do or do not" }
	puts l.call # Call the thing!

Function that increments things

	Increment = lambda {|x| x.next }
	# You can call stuff on it now.
	Increment.call(1) => 2
	Increment.call(-1) => 0

Lambdas vs. Blocks: A lambda is a piece of code you can store in a variable and is an object. A block _can't_ be stored in a variable and isn't an object.

The use of lambdas: Passing one block to a method which in turn uses it to get some work done.

Ex:

	def demonstrate_block(number)
	  yield(number)
	end

	puts demonstrate_block(1) { |number| number + 1 }

No `lambda`, but there is `yield`.

	def calculate (*args)
	  yield(*args)
	end

	calculate(2, 3) {|a, b| a + b} returns 5
	calculate(2, 3) {|a, b| a - b} returns -1

#### What are blocks?

"A block is code that you can store in a variable like any other object and run on demand." 

	addition = lambda {|a, b| return a + b }
	puts addition.call(5, 6)

Blocks are objects, and are members of class `Proc`, which is what a block is called in Ruby.

A method is simply a block bound to an object, with access to the object's state.

	Addition 		= lambda {|a, b|  a + b }
	Subtraction 	= lambda {|a, b| a - b }
	Multiplication 	= lambda {|a, b| a * b }
	Division 		= lambda {|a, b| a / b  }

#### Yield

The most common usage of blocks involves passing exactly one block to a method.

Old method:

	def calculation(a, b, operation)
	  operation.call(a, b)
	end

	puts calculation(5, 6, lambda { |a, b| a + b }) # addition

With `yield`, this is the same thing.

	def calculation(a, b)
	  yield(a, b)
	end

	puts calculation(5, 6) { |a, b| a + b } # addition
	puts calculation(5, 6) { |a, b| a - b } # subtraction

Differences between `yield` and the normal approach:

- The block is no longer a parameter to the method. It has been _implicitly passed_ to the method.
- `yield` makes executing the block feel like a method invocation within the method invocation (compare with doing `call` again).

Yield is not a method. `yield.class = ???`

A `yield` can provide a custom sort algorithm.

	days.sort do |x,y|
	    x.size <=> y.size
	 end

	=> ["monday", "friday", "tuesday", "thursday", "wednesday"]

Optional yield:

	yield(value) if block_given?

## Explicit blocks

Sometimes, the performance benefits of implicit block invocation are outweighed by the need to have the block accessible as a concrete object.

Explicit f(x), implicit block: 

	# We explicitly define that a block should be passed in via the &block parameter.
	def calculation(a, b, &block) # &block is an explicit (named) parameter
	 block.call(a, b)
	end

	puts calculation(5, 5) { |a, b| a + b } 

Implicit f(x), explicit block: 

	# Nothing is implied based on the function signature, but a block is def. required.
	def calculation(a, b)
	  yield(a, b) # yield calls an implicit (unnamed) block 
	end

	addition = lambda {|x, y| x + y}
	puts calculation(5, 5, &addition)

The block should be the last parameter passed to a method. Placing an ampersand (&) before the name of the last variable triggers the conversion.

	def filter(array, block)
	  return array.select {|x| block.call(x)}
	end

	# is the same as 

	Filter = lambda do |array, &block|
	 array.select(&block)
	end

#### Explanation of WTF blocks actually do:

If we try to implement blocks on our own as an `iterate` method, it would look like this:

	class Array
	  def iterate!
	    self.each_with_index do |n, i|
	      self[i] = yield(n)
	    end
	  end
	end

	array.iterate! do |n|
	  n ** 2
	end

blocks are of class `Proc`, but they do not have a name. If you want to name a block, you have to do this:

blocky = Proc.new {|x| x * 2}

This is useful when you have to have two functions as a callback or something. Can't use a block here.

	def callbacks(procs)
	  procs[:starting].call

	  puts "Still going"

	  procs[:finishing].call
	end

	callbacks(:starting => Proc.new { puts "Starting" },
	          :finishing => Proc.new { puts "Finishing" })

Block vs Proc.

1. Block if the method breaks an object down into smaller pieces and you want the users to interact with the pieces.
2. Block if you want to run multiple expressions atomically.
3. Proc if you want to reuse a block of code multiple times.
4. Proc if your method will have one or more callbacks.

`Lambda`s are almost the same as `Proc`s, except they are actually methods. Their differences are that `lamba` actively checks the number of arguments, and their `return`s are different.

	def args(code)
	  one, two = 1, 2
	  code.call(one, two)
	end

	args(Proc.new{|a, b, c| puts "Give me a #{a} and a #{b} and a #{c.class}"})

	args(lambda{|a, b, c| puts "Give me a #{a} and a #{b} and a #{c.class}"})

lambda will raise an ArgumentError here because it requires the third argument.

	def proc_return
	  Proc.new { return "Proc.new"}.call
	  return "proc_return method finished"
	end

	def lambda_return
	  lambda { return "lambda" }.call
	  return "lambda_return method finished"
	end

	puts proc_return		# => Proc.new
	puts lambda_return		# => lambda_return method finished

*`proc` is a code snippet*, copy and paste, so you actually exit immediately when you hit the `return` statement. `*Lambda` is an actual method* that is executed, so it produces the "lambda" and carries on with the method.

Think of `lambda`s as a way of writing anonymous methods.

*`Proc.new` is something that’s hardly ever used to explicitly create blocks because of these surprising return semantics. It is recommended that you avoid using this form unless absolutely necessary.*

Method Objects
	
Passing methods into methods, you use the `method` cast (different from `:method` method.

	def square(n)
	  n ** 2
	end

	> aw = [1, 2, 3].each{method(:square)}
	> aw = [1, 4, 9]

API def of `method`: Looks up the named method as a receiver in obj, returning a Method object (or raising NameError). The Method object acts as a closure in obj’s object instance, so instance variables and the value of self remain available.

	class Demo
	  def initialize(n)
	    @iv = n
	  end
	  def hello()
	    "Hello, @iv = #{@iv}"
	  end
	end

	k = Demo.new(99)
	m = k.method(:hello)
	m.call   #=> "Hello, @iv = 99"

	l = Demo.new('Fred')
	m = l.method("hello")
	m.call   #=> "Hello, @iv = Fred"


# File: loops.md

	# add a loop inside this method to ring the bell 'n' times
	def ring(bell, n)
	  n.times do
	      bell.ring
	  end
	end  


# File: modules.md

# Jumpstart Lab
[link](http://tutorials.jumpstartlab.com/topics/models/modules.html)

Rails -- modules can be used to namespace a group of related Rails models. These would usually be stored in a subfolder of models with the name of the namespace.

In `ActiveRecord`, inheritance leads to STI. STI sounds like a good idea, then you end up ripping it out as the project matures. It just isn't a strong design practices. We can mimic inheritance using modules and allow each model to have its own table.

We can do this:

    module TextContent
      def word_count
        body.split.count
      end
    end

Then to make use of the class:

    class Article < AR::Base
      includ TextContent
    end

We can also use modules to share class methods.

## `self.included`

We can modify `self.included` like so:

    def self.included(including_class)
      including_class.extend ClassMethods
      including_class.send(:has_one, :moderator_approval, {as: :content})
    end

`send` allows us to trigger a private method inside another object.


# Pickaxe 9 -- Modules

### Namespaces

Modules define a namespace. Module constants begin with a capital letter.

When you `include` a module within a class definition, it gets `mixed_in`.

A module is basically a class that cannot be instantiated. Like a class, its body is executed during definition and the resulting Module object is stored in a constant.

  class|module name
    include expr
  end

A module may be included within the definition of another module or class using the `include` method.

If a module is included within a class definition, the module’s constants, class variables, and instance methods are effectively bundled into an anonymous (and inaccessible) superclass for that class.

##  RubyMonk

*Modules only hold behaviour, unlike classes, which hold both behaviour and state.*

    module WarmUp
      def push_ups
        "Phew, I need a break!"
      end
    end

    class Gym
      include WarmUp

      def preacher_curls
        "I'm building my biceps."
      end
    end

    puts Gym.new.push_ups

`Module` is the superclass of `Class`, so *classes can be used as modules.*

### Modules as Namespaces

    module Perimeter
      class Array
        def initialize
          @size = 400
        end
      end
    end

    our_array = Perimeter::Array.new
    # our_array.class is Perimeter::Array

Contrived example of extending the `Array` class hehe.

Multiple scopes:

    module Dojo
      A = 4
      module Kata
          B = 8
        module Roulette
          class ScopeIn
            def push
              15
            end
          end
        end
      end
    end

    A = 16
    B = 23
    C = 42

    puts "A - #{A}"
    puts "Dojo::A - #{Dojo::A}"					# Scoped within Dojo

    puts

    puts "B - #{B}"
    puts "Dojo::Kata::B - #{Dojo::Kata::B}"		# Scoped within Dojo::Kata

    puts

    puts "C - #{C}"
    puts "Dojo::Kata::Roulette::ScopeIn.new.push - #{Dojo::Kata::Roulette::ScopeIn.new.push}"
                                                                                          # Scoped within something huge and even in the method itself

  If you prepend a constant with :: without a parent, the scoping happens on the topmost level.

      ::A

## Ruby Best Practices
[link](http://blog.rubybestpractices.com/posts/gregory/037-issue-8-uses-for-modules.html)

    class Blog
      class Comment
      end
    end

A class nested within another class looks the same as a class nested within a module. This would only be really useful when you have a desired namespace for your library that also happens to match one of your class names. *In all other situations, it makes sense to use a module for namespacing as it would prevent your users from creating instances of an empty and meaningless class.*

In general, having to use absolute lookups may be a sign that there is an unnecessary name conflict within your application.

A class is a module that can be instantiated, and you can't mix in a class.

## Practicing Ruby, Modules PDF

#### Modules Features

- You can create nested constants, which allows you to organize your code into namespaces.
- You can `include` a module into a class, mixing in the functionality into all instances of the class.
- You can `extend` the functionality of objects using modules.
- You can define methods and instance variables directly on modules.

#### Core Ruby Mixins

`Comparable`:

    def <=>(other)
      return 0 if result == other.result
      return 1 if result > other.result
      return -1 if result < other.result
    end

By `include Comparable` and adding an implementation for the spaceship (`<=>`) method, you get the other comparison operators (`<`, `>`...) for free.

Ruby's `Enumerable` gives you `select()`, `map()`, `reduce()` for free once you define `each()`.

Mixins don't explicitly distinguish between methods whether they are mixed in at the class or instance level.

    module Wow
      def huh
        puts wewertz
      end
    end

    class WowEx
      extend Wow
      def self.wewertz
        '???'
      end
    end

    puts WowEx.huh #=> '???'

    class WowExDin
      include Wow
      def wewertz
        '???'
      end
    end

    puts WowExDin.new.huh #=> '???'

#### Singleton objects

*When an object doesn't really need to be instantiated at all because it has no data in common between its behaviors, the functional approach often works best.* There are some cases though when a single object is all we need, in particular configuration systems.

    AccessControl.configure do
      role "basic",
        :permissions => [:read_answers, :answer_questions]
      role "premium",
        :parent      => "basic",
        :permissions => [:hide_advertisements]
      role "manager",
        :parent      => "premium",
        :permissions => [:create_quizzes, :edit_quizzes]
      role "owner",
        :parent      => "manager",
        :permissions => [:edit_users, :deactivate_users]
    end

While it is easy to imagine that roles will get added and removed as needed, it's hard to imagine having more than one `AccessControl` object. By modelling `AccessControl` as a module rather than a class, it becomes impossible to create new instances of the object, and so all the state needs to be stored within the module itself.

    module AccessControl
      extend self
      def configure(&block)
        instance_eval(&block)
      end
      def definitions
        @definitions ||= {}
      end

      # Role definition omitted, replace with a stub if you want to test # or refer to Practicing Ruby Issue #4
      def role(level, options={})
        definitions[level] = Role.new(level, options)
      end

      def roles_with_permission(permission)
        definitions.select { |k,v| v.allows?(permission) }.map { |k,_| k }
      end

      def [](level)
        definitions[level]
      end
    end

With this pattern we are able to add and modify definitions with a single object. Because `AccessControl` is an ordinary Ruby object, it has ordinary instance variables and can make use of `instance_eval` just like any other object. The key difference is that `AccessControl` is a module, not a class, and so cannot be used as a factory for creating more instances.


# File: require.md

## [What is the difference between include and require in Ruby?](http://stackoverflow.com/questions/318144/what-is-the-difference-between-include-and-require-in-ruby)

*Require: Copy and paste the file. A file thing. It doesn't let you require the same file twice. Use to import libraries.*

*Load: Use this to execute code.*

*Include: Takes all the methods from another module and includes them into the current module. A language thing. Use this to "extend classes" with other modules (mixins).*

*Extend: You're bringing in the module's methods as _class_ methods.*

	module A
	   def say
	     puts "this is module A"
	   end
	 end

	class B
	   include A
	end

	class C
	   extend A
	end

	B.say => undefined method 'say' for B:Class
	B.new.say => this is module A
	C.say => this is module A
	C.new.say => undefined method 'say' for C:Class






# File: self.md

# Self -- The Current/Default Object
[link](http://rubylearning.com/satishtalim/ruby_self.html)

`self` = displays `main`, a special term the default self object uses to refer to itself.

In a class or module definition, the class is the class or module object.

In an instance method, `self` is the object itself.



# File: struct.md

## Ruby data object comparison
[Reference](http://palexander.posthaven.com/ruby-data-object-comparison-or-why-you-should-never-ever-use-openstruct)

- OpenStruct is really bad performance wise.
- 2015: OpenStruct is still really bad, Hash/Struct/Class are very near to each other (Class-Struct-Hash fastest to slowest).

## When to use or not use structs?
[Reference](https://www.reddit.com/r/ruby/comments/6a4j7x/when_to_use_or_not_use_structs/)

- Classes where the only methods are the accessors.
- Structs suggest a data container.
- Structs are not as performant as hashes.
- No keyword-argument initializers, no clear definition if structs can be converted to hashes and back or not.
- Structs are lightweight data containers with some convenient data manipulation possibilities.

## Struct inheritance is overused
[Reference](https://thepugautomatic.com/2013/08/struct-inheritance-is-overused/)

- *Struct arguments are not required.* If this is not what you want, then don't inherit from struct.
  - Struct creates public accessors.
  - Data container--It has `members`, `values`, `length`.
  - Structs are equal if their attributes are equal.
  - Structs don't want to be subclassed.


# File: symbols.md

## [The Difference Between Ruby Symbols and Strings](http://www.robertsosinski.com/2009/01/11/the-difference-between-ruby-symbols-and-strings/)

*Symbols are Strings, just with an important difference, Symbols are immutable.* Mutable objects can be changed after assignment while immutable objects can only be overwritten. Ruby is quite unique in offering mutable Strings, which adds greatly to its expressiveness. However mutable Strings can have their share of issues in terms of creating unexpected results and reduced performance. It is for this reason Ruby also offers programmers the choice of Symbols.

Symbols are as flexible as Strings in expressing data. You can interpolate symbols.

	"hello world#{bang}" # => "hello world!"
	:"hello world#{bang}" # => :"hello world!"

Conversion between Strings to Symbols and back:

	:"hello world".to_s # => "hello world"
	"hello world".intern # => :"hello world"

Symbols cannot change.

	puts "hello" << " world"	# => hello world
	puts :hello << :" world"	# => *.rb:4: undefined method `<<' for :hello:Symbol (NoMethodError)

#### Mutability: You can introduce bugs with Strings.

	status = "peace"

	buggy_logger = status

	print "Status: "
	print buggy_logger << "\n" # <- This insertion is the bug.

	def launch_nukes?(status)
	  unless status == 'peace'
	    return true
	  else
	    return false
	  end 
	end

	print "Nukes Launched: #{launch_nukes?(status)}\n"

	# => Status: peace
	# => Nukes Launched: true

The `<<` statement allows a nuke to be launched because the String has been edited.

*Frozen Strings:* You can use this to make a String un-editable.

	> aw = "hello"
	> aw.freeze
	> aw.upcase! # can't modify frozen string (TypeError)

#### Performance

Because Strings are mutable, the Ruby interpreter never knows what that String may hold in terms of data. As such, every String needs to have its own place in memory.

	puts "hello world".object_id 		# => 3102960
	puts "hello world".object_id 		# => 3098410

	puts :"hello world".object_id		# => 239518
	puts :"hello world".object_id		# => 239518

Ruby does not mark Symbols for destruction, they are actually kep track off in an optimized dictionary. (Check it out via `Symbol.all_symbols.inspect`). Ruby creates and keeps track of the Symbols for the dict.

	>> Symbol.all_symbols.collect{|sym| sym.to_s}.include?("new_symbol")
	=> false
	>> :new_symbol
	=> :new_symbol
	>> Symbol.all_symbols.collect{|sym| sym.to_s}.include?("new_symbol")
	=> true

Symbols are also faster than Strings!
```ruby
	require 'benchmark'
	str = Benchmark.measure do
	  10_000_000.times do
	    "test"
	  end
	end.total

	sym = Benchmark.measure do
	  10_000_000.times do
	    :test
	  end
	end.total

	puts "String: " + str.to_s
	puts "Symbol: " + sym.to_s
	puts

	$ ruby benchmark.rb
	String: 2.24
	Symbol: 1.32

```
	


On average, we get a 40% increase solely by using Symbols. Symbols are also faster than Strings in how they are compareed, because of the sharing same space on the heap thing.





# File: tap.md

# Ruby - `#tap` that!
[Link](http://blog.endpoint.com/2012/04/deconstructing-oo-blog-designs-in-ruby.html)

    class Object
      def tap
        yield self
        self
      end
    end

Tap allows you to do something with an object inside of a block, and always have that block return the object itself.

`#tap` was created for "tapping" into method chains.

> No tap

    def something
        result = operation
        do_something_with_result
        result
    end

> With tap

    def something
        operation.tap do |op|
            do_something_with op
        end
    end

We can use it for putting debugging calls, but there's so much more than that.
    
    arr.reverse
    arr.tap { |a| puts a}.reverse # After, has a debugger on it

#### Other uses

> Assigning a property to an object, especially if single attribute. Btw this is the same as `User.new(key: "value")`

    # Traditional
    object = SomeClass.new
    object.key = "value"
    object

    # Tapped
    object = SomeClass.new.tap do |obj| 
        obj.key = "value"
    end

    # Tapped shortcut
    object = SomeClass.new.tap { |obj| obj.key = "value" }

> Ignoring method return. Btw this is also the same as `Model.create!`

    # Traditional
    object = Model.new
    object.save!
    object

    # Tapped
    object = Model.new.tap do |model|
        model.save!
    end

    # Tapped shortcut
    object = Model.new.tap(&:save!)

> 


# File: timeout.md

# Ruby Timeouts
[Reference](https://github.com/ankane/the-ultimate-guide-to-ruby-timeouts)

- PG: Statement timeouts: Prevent single queries from taking up all of your database resources.
  - `database.yml`

``` yml
production:
  variables:
    statement_timeout: 250 # ms
```

- ActiveRecord:: `ActiveRecord::Base.establish_connection`
- ElasticSearch: `Elasticsearch::Client.new(transport_options: {request: {timeout: 1}}, ...)`
- Mongo: `Mongo::Client.new([host], connect_timeout: 1, socket_timeout: 1, server_selection_timeout: 1, ...)`
- Redis: `Redis.new(connect_timeout: 1, timeout: 1, ...)`
- Kafka: `Kafka.new(connect_timeout: 1, socket_timeout: 1)`
- Faraday:

```
Faraday.get(url) do |req|
  req.options.open_timeout = 1
  req.options.timeout = 1
end

Faraday.new(url, request: {open_timeout: 1, timeout: 1}) do |faraday|
  # ...
end
```

- Puma: `# config/puma.rb worker_timeout 15`
- ActionMailer:

``` ruby
ActionMailer::Base.smtp_settings = {
  open_timeout: 1,
  read_timeout: 1
}
```

- AWS:

```
Aws.config = {
  http_open_timeout: 1,
  http_read_timeout: 1
}
```

- `Gibbon`, `geocoder`, `Google-Cloud`, `hipchat`, `koala`, `kubeclient`, mail, `mechanize`, `net-dns`, `stripe`, `twilio-ruby`, `zendesk-api`.
- On rescuing exceptions:
  - `rescue Timeout::Error`




# [[Ruby IO]] 
