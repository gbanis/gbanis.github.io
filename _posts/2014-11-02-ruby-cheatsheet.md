---
layout:     post
title:      "Ruby Cheat Sheet"
description:  "A nifty little cheat sheat with the basics of the Ruby language."
date:       2014-11-02 12:00:00
tags:
  - coding
  - cheat sheets
  - ruby
author:     George Banis
---

## Data Types

Numbers:

```ruby
a = 123
```

```ruby
a = 123
b = 1_234
c = 123.45
d = 1.2e-3
```

Strings:

```ruby
'no interpolation'
"#{interpolation} and backslashes\n"
```

Symbols:

A symbol is an immutable name used for identifiers, variables, and operators.

```ruby
:symbol
```

Ranges:

```ruby
1..10
'a'..'z'
(1..10)  # inclusive
(1...10) # exclusive
```

Arrays:

```ruby
[1, 2, 3]
%w(foo bar baz)    # no interpolation
%W(foo #{bar} baz) # interpolation
```

Hashes:

```ruby
{ number: 1, string: 'hi' }
```

Variables and Constants:

```ruby
$global_variable
@instance_variable
[OtherClass::]CONSTANT # eg. Math::PI
local_variable
```

## Control Operations

```ruby
if a > b
  # body
elsif b > a
  # body
else
  # body
end
```

```ruby
unless a > b   # 'unless a == b' is same as 'if a != b'
  # body
else
  # body
end
```

```ruby
do_something if a > b
do_something unless a == b
```

```ruby
a > b ? do_something_if_true : do_something_if_false
```

```ruby
case grade
when "A"
  puts 'Well done!'
when "B"
  puts 'Try harder!'
when "C"
  puts 'You need help!!!'
else
  puts "You just making it up!"
end
```

## Loops

```ruby
while conditional [do]
   code
end

code while condition

begin
  code
end while conditional
```

```ruby
until conditional [do]
   code
end

code until conditional

begin
   code
end until conditional
```

```ruby
(expression).each do |variable[, variable...]|
  code
end
```

```ruby
for i in 0..5
   if i > 2 then
      break   # use break to exit the loop
   end
   puts "Value of local variable is #{i}"
end
```

## Methods

```ruby
# define
def method_name
   expr..
   # By default returns the evaluation of the last line of code
   # 'return' is optional
end

def method_name (var1, var2, optional_val = value)
   expr..
end

# invoke
method_name
method_name(var1, var2)
```

## Classes

```ruby
class Identifier < Superclass
  # Accessors
  attr_reader
  attr_writer
  attr_accessor

  def initializer (var1, var2)
    @var1 = var1
    @var2 = var2
  end

  def public_method
    # ...
  end

  protected
  # accessable only by instances of class and direct descendants

  private
  # accessable only by instances of class

end
```

```ruby
b = B.new(va1, var2)
b.class_method
```

## Modules

```ruby
module Identifier
  # ...
end
```

## Exceptions

```ruby
class InvalidUserInput < Exception
end

begin
  puts "Select a column to place an \'#{color}\' piece:"
  move = gets.chomp.to_i

  # checks if user tries to play a move outside the board
  raise InvalidUserInput.new("Invalid input. Select a number between 1 and #{@col_num}") unless move.between?(1, @col_num)
  # checks if user tries to play a move at a full column
  raise InvalidUserInput.new("You can't put another checker in row #{move}.") unless @board[move-1].length < @row_num

rescue InvalidUserInput => error
  puts error
  retry
end
```

## References

- [http://www.tutorialspoint.com](http://www.tutorialspoint.com)
- [http://overapi.com/](http://overapi.com)
- [http://www.skorks.com](http://www.skorks.com/)
