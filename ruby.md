# The Ruby Style Guide

Forked from https://github.com/bbatsov/ruby-style-guide

You can generate a PDF or an HTML copy of this guide using
[Transmuter](https://github.com/TechnoGate/transmuter).

## Table of Contents

* [Source Code Layout](#source-code-layout)
* [Syntax](#syntax)
* [Naming](#naming)
* [Comments](#comments)
* [Annotations](#annotations)
* [Classes](#classes)
* [Exceptions](#exceptions)
* [Collections](#collections)
* [Strings](#strings)
* [Regular Expressions](#regular-expressions)
* [Percent Literals](#percent-literals)
* [Metaprogramming](#metaprogramming)
* [Misc](#misc)

## Source Code Layout

1. Use `UTF-8` as the source file encoding.
2. Use two **spaces** per indentation level.

    ```Ruby
    # good
    def some_method
      do_something
    end

    # bad - four spaces
    def some_method
        do_something
    end
    ```

3. Use Unix-style line endings. (*BSD/Solaris/Linux/OSX users are covered by default,
  Windows users have to be extra careful.)
    * If you're using Git you might want to add the following
    configuration setting to protect your project from Windows line
    endings creeping in:

        ```$ git config --global core.autocrlf true```

4. Use spaces around operators, after commas, colons and semicolons, around `{`
  and before `}`. Whitespace might be (mostly) irrelevant to the Ruby
  interpreter, but its proper use is the key to writing easily
  readable code.

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

    The only exception is when using the exponent operator:

    ```Ruby
    # bad
    e = M * c ** 2

    # good
    e = M * c**2
    ```

5. No spaces after `(`, `[` or before `]`, `)`.

    ```Ruby
    some(arg).other
    [1, 2, 3].length
    ```

6. Indent `when` as deep as `case`. I know that many would disagree
  with this one, but it's the style established in both the "The Ruby
  Programming Language" and "Programming Ruby".

    ```Ruby
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end
    ```

7. Use empty lines between `def`s and to break up a method into logical
  paragraphs.

    ```Ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_method
      result
    end
    ```

8. Align the parameters of a method call if they span over multiple lines.

    ```Ruby
    # starting point (line is too long)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # bad (normal indent)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end

    # bad (double indent)
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # good
    def send_mail(source)
      Mailer.deliver(
        to:      'bob@example.com',
        from:    'us@example.com',
        subject: 'Important message',
        body:    source.text
      )
    end
    ```

9. Use RDoc and its conventions for API documentation.  Don't put an
   empty line between the comment block and the `def`.
10. Keep lines fewer than 80 characters.
11. Avoid trailing whitespace.
12. Single newline at EOF.

## Syntax

1. Use `def` with parentheses when there are arguments. Omit the
  parentheses when the method doesn't accept any arguments both in calling and
  declaration.

     ```Ruby
     def some_method
       # body omitted
     end

     def some_method_with_arguments(arg1, arg2)
       # body omitted
     end
     ```

2. Never use `for`, unless you know exactly why. Most of the time iterators
  should be used instead. `for` is implemented in terms of `each` (so
  you're adding a level of indirection), but with a twist - `for`
  doesn't introduce a new scope (unlike `each`) and variables defined
  in its block will be visible outside it.

    ```Ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do
      puts elem
    end

    # good
    arr.each { |elem| puts elem }
    ```

3. Never use `then` for multi-line `if/unless`.

    ```Ruby
    # bad
    if some_condition then
      # body omitted
    end

    # good
    if some_condition
      # body omitted
    end
    ```

4. Favor the ternary operator(`?:`) over `if/then/else/end` constructs.
  It's more common and obviously more concise.

    ```Ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

5. Use one expression per branch in a ternary operator. This
  also means that ternary operators must not be nested. Prefer
  `if/else` constructs in these cases.

    ```Ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

6. Never use `if x: ...` - it is removed in Ruby 1.9. Use
  the ternary operator instead.

    ```Ruby
    # bad
    result = if some_condition: something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

7. Never use `if x; ...`. Use the ternary operator instead.

8. Use `when x then ...` for one-line cases. The alternative syntax
  `when x: ...` is removed in Ruby 1.9.

9. Never use `when x; ...`. See the previous rule.

10. Use `&&/||` for boolean expressions, `and/or` for control flow.  (Rule
  of thumb: If you have to use outer parentheses, you are using the
  wrong operators.)

    ```Ruby
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    user_id = params[:user_id] or return 'MISSING_USER_ID'
    ```

11. Avoid multi-line `?:` (the ternary operator), use `if/unless` instead.

12. Favor modifier `if/unless` usage when you have a single-line
  body. Another good alternative is the usage of control flow `and/or`.

    ```Ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition and do_something
    ```

13. Favor `unless` over `if` for negative conditions (or control
  flow `or`).

    ```Ruby
    # bad
    do_something if !some_condition

    # good
    do_something unless some_condition
    ```

14. Favor use of `if` with `else`.

    ```Ruby
    # bad
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # good
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

15. Don't use parentheses around the condition of an `if/unless/while`,
  unless the condition contains an assignment (see "Using the return
  value of `=`" below).

    ```Ruby
    # bad
    if (x > 10)
      # body omitted
    end

    # good
    if x > 10
      # body omitted
    end

    # ok
    if (x = self.next_value)
      # body omitted
    end
    ```

16. Favor modifier `while/until` usage when you have a single-line
  body.

    ```Ruby
    # bad
    while some_condition
      do_something
    end

    # good
    do_something while some_condition
    ```

17. Favor `until` over `while` for negative conditions.

    ```Ruby
    # bad
    do_something while !some_condition

    # good
    do_something until some_condition
    ```

18. Omit parentheses around parameters for methods that are part of an
  internal DSL (e.g. Rake, Rails, RSpec), methods that are with
  "keyword" status in Ruby (e.g. `attr_reader`, `puts`) and attribute
  access methods. Use parentheses around the arguments of all other
  method invocations.

    ```Ruby
    class Person
      attr_reader :name, :age

      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)
    ```

19. Prefer `{...}` over `do...end` for single-line blocks.  Avoid using
  `{...}` for multi-line blocks (multiline chaining is always
  ugly). Always use `do...end` for "control flow" and "method
  definitions" (e.g. in Rakefiles and certain DSLs).  Avoid `do...end`
  when chaining.

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # good
    names.each { |name| puts name }

    # bad
    names.each do |name|
      puts name
    end

    # good
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }

    # bad
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }
    ```

    Some will argue that multiline chaining would look OK with the use of {...}, but they should
    ask themselves - it this code really readable and can't the blocks contents be extracted into
    nifty methods.

20. Avoid `return` where not required.

    ```Ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```

21. Do not use spaces around the `=` operator when assigning default values to method parameters:

    ```Ruby
    # bad, difficult to see how many args
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end

    # good, easy to see that its 3 args
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end
    ```

    While several Ruby books suggest the first style, the second is much more prominent
    in practice (and arguably a bit more readable).

22. Avoid line continuation (\\) where not required. In practice, avoid using
  line continuations at all.

    ```Ruby
    # bad
    result = 1 - \
             2

    # good (but still ugly as hell)
    result = 1 \
             - 2
    ```

23. Using the return value of `=` (an assignment) is ok, but surround the
  assignment with parenthesis.

    ```Ruby
    # good - shows intented use of assignment
    if (v = array.grep(/foo/)) ...

    # bad
    if v = array.grep(/foo/) ...

    # also good - shows intended use of assignment and has correct precedence.
    if (v = self.next_value) == 'hello' ...
    ```

24. Use `||=` freely to initialize variables.

    ```Ruby
    # set name to Bozhidar, only if it's nil or false
    name ||= 'Bozhidar'
    ```

25. Don't use `||=` to initialize boolean variables. (Consider what
would happen if the current value happened to be `false`.)

    ```Ruby
    # bad - would set enabled to true even if it was false
    enabled ||= true

    # good
    enabled = true if enabled.nil?
    ```

26. Avoid using Perl-style special variables (like `$0-9`, `$``,
  etc. ). They are quite cryptic and their use in anything but
  one-liner scripts is discouraged.

27. Never put a space between a method name and the opening parenthesis.

    ```Ruby
    # bad
    f (3 + 2) + 1

    # good
    f(3 + 2) + 1
    ```

28. If the first argument to a method begins with an open parenthesis,
  always use parentheses in the method invocation. For example, write
`f((3 + 2) + 1)`.

29. Always run the Ruby interpreter with the `-w` option so it will warn
you if you forget either of the rules above!

30. When the keys of your hash are symbols use the short syntax.

    ```Ruby
    # bad
    hash = { :one => 1, :two => 2 }

    # good
    hash = { one: 1, two: 2 }
    ```

31. Use the new lambda literal syntax.

    ```Ruby
    # bad
    lambda = lambda { |a, b| a + b }
    lambda.call(1, 2)

    # good
    lambda = ->(a, b) { a + b }
    lambda.(1, 2)
    ```

32. Use `_` for unused block parameters.

    ```Ruby
    # bad
    result = hash.map { |k, v| v + 1 }

    # good
    result = hash.map { |_, v| v + 1 }
    ```

## Naming

1. Use `snake_case` for methods and variables.
2. Use `CamelCase` for classes and modules.  (Keep acronyms like HTTP,
  RFC, XML uppercase.)
3. Use `SCREAMING_SNAKE_CASE` for other constants.
4. The names of predicate methods (methods that return a boolean value)
  should end in a question mark. (i.e. `Array#empty?`).
5. The names of potentially "dangerous" methods (i.e. methods that modify `self` or the
  arguments, `exit!` (doesn't run the finalizers like `exit` does), etc.) should end with an exclamation mark if
  there exists a safe version of that *dangerous* method.

    ```Ruby
    # bad - there is not matching 'safe' method
    class Person
      def update!
      end
    end

    # good
    class Person
      def update
      end
    end

    # good
    class Person
      def update!
      end

      def update
      end
    end
    ```

6. Define the non-bang (safe) method in terms of the bang (dangerous)
  one if possible.

    ```Ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

7. When using `reduce` with short blocks, name the arguments `|a, e|`
  (accumulator, element). Unless more clear names can be used, the
  point being to avoid `|a, b|`.
8. When defining binary operators, name the argument `other`.

    ```Ruby
    def +(other)
      # body omitted
    end
    ```

9. Prefer `map` over `collect`, `find` over `detect`, `select` over
  `find_all`, `reduce` over `inject` and `size` over `length`. This is
  not a hard requirement; if the use of the alias enhances
  readability, it's ok to use it. The rhyming methods are inherited from
  Smalltalk and are not common in other programming languages. The
  reason the use of `select` is encouraged over `find_all` is that it
  goes together nicely with `reject` and its name is pretty self-explanatory.

10. Avoid `get_` and `set_` method names.

    ```Ruby
    # bad
    def get_currency
      @currency
    end

    def set_currency(name)
      @currency = name
    end

    # good
    def currency
      @currency
    end

    def currency=(name)
      @currency = name
    end

    # even better (unless custom getters and setters are needed)
    attr_reader   :currency
    attr_writer   :currency
    # attr_accessor :currency # read *and* write shortcut
    ```

## Comments

1. Write self-documenting code and ignore the rest of this section. Seriously!
2. Comments longer than a word are capitalized and use punctuation. Use [one
  space](http://en.wikipedia.org/wiki/Sentence_spacing) after periods.
3. Avoid superfluous comments.

    ```Ruby
    # bad
    counter += 1 # increments counter by one
    ```

4. Keep existing comments up-to-date. An outdated is worse than no comment
at all.

5. Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory. (Do or do not - there is no try. --Yoda)

## Annotations

1. Annotations should usually be written on the line immediately above
  the relevant code.
2. The annotation keyword is followed by a colon and a space, then a note
  describing the problem.
3. If multiple lines are required to describe the problem, subsequent
  lines should be indented two spaces after the `#`.

    ```Ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #   be related to the BarBazUtil upgrade.
      baz(:quux)
    end
    ```

4. In cases where the problem is so obvious that any documentation would
  be redundant, annotations may be left at the end of the offending line
  with no note. This usage should be the exception and not the rule.

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

5. Use `TODO` to note missing features or functionality that should be
  added at a later date.
6. Use `FIXME` to note broken code that needs to be fixed.
7. Use `OPTIMIZE` to note slow or inefficient code that may cause
  performance problems.
8. Use `HACK` to note code smells where questionable coding practices
  were used and should be refactored away.
9. Use `REVIEW` to note anything that should be looked at to confirm it
  is working as intended. For example: `REVIEW: Are we sure this is how the
  client does X currently?`
10. Use other custom annotation keywords if it feels appropriate, but be
  sure to document them in your project's `README` or similar.

## Classes

1. When designing class hierarchies make sure that they conform to the
  [Liskov Substitution Principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle).
2. Try to make your classes as
  [SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\))
  as possible.
3. Always supply a proper `to_s` method for classes that represent
  domain objects.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#{@first_name} #{@last_name}"
      end
    end
    ```

4. Use the `attr` family of functions to define trivial accessors or
mutators.

    ```Ruby
    # bad
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```
5. Prefer instance variables over accessors *within the class* for clarity

    ```Ruby
    # bad
    class Person
      attr_reader :first_name

      def initialize(first_name)
        @first_name = first_name
      end

      def first_name_reversed
        first_name.reverse # missing @
      end
    end

    # good
    class Person
      attr_reader :first_name

      def initialize(first_name)
        @first_name = first_name
      end

      def first_name_reversed
        @first_name.reverse
      end
    end
    ```
6. Consider using `Struct.new`, which defines the trivial accessors,
constructor and comparison operators for you.

    ```Ruby
    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    class Person < Struct.new (:first_name, :last_name)
    end
    ````

7. Consider adding factory methods to provide additional sensible ways
to create instances of a particular class.

    ```Ruby
    class Person
      def self.create(options_hash)
        # body omitted
      end
    end
    ```

8. Prefer [duck-typing](http://en.wikipedia.org/wiki/Duck_typing) over inheritance.

    ```Ruby
    # bad
    class Animal
      # abstract method
      def speak
      end
    end

    # extend superclass
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # extend superclass
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    # good
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

9. Avoid the usage of class (`@@`) variables due to their "nasty" behavior
in inheritance.

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    As you can see all the classes in a class hierarchy actually share one
    class variable. Class instance variables should usually be preferred
    over class variables.

10. Assign proper visibility levels to methods (`private`, `protected`)
in accordance with their intended usage. Don't go off leaving
everything `public` (which is the default). After all we're coding
in *Ruby* now, not in *Python*.
11. Indent the `public`, `protected`, and `private` methods as much the
  method definitions they apply to. Leave one blank line above and below them.

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end
    end

12. Use `def self.method` to define singleton methods. This makes the methods
  more resistant to refactoring changes.

    ```Ruby
    class TestClass
      # bad
      def TestClass.some_method
        # body omitted
      end

      # good
      def self.some_other_method
        # body omitted
      end

      # Also possible and convenient when you
      # have to define many singleton methods.
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```

## Exceptions

1. Signal exceptions using the `raise` keyword. Do not use the `fail` keyword.

    ```Ruby
    begin
      raise 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

2. Never return form an `ensure` block. If you explicitly return from a
  method inside an `ensure` block, the return will take precedence over
  any exception being raised, and the method will return as if no
  exception had been raised at all. In effect, the exception will be
  silently thrown away.

    ```Ruby
    def foo
      begin
        raise
      ensure
        return 'very bad idea'
      end
    end
    ```

3. Use *implicit begin blocks* when possible.

    ```Ruby
    # bad
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # good
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

4. Mitigate the proliferation of `begin` blocks via the use of
  *contingency methods* (a term coined by Avdi Grimm).

    ```Ruby
    # bad
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # good
    def with_io_error_handling
       yield
    rescue
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

5. Don't suppress exceptions.

    ```Ruby
    # bad
    begin
      # an exception occurs here
    rescue SomeError
      # the rescue clause does absolutely nothing
    end

    # bad
    do_something rescue nil
    ```

6. Don't use exceptions for flow of control.

    ```Ruby
    # bad
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # good
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

7. Avoid rescuing the `Exception` class.  This will trap signals and calls to
  `exit`, requiring you to `kill -9` the process.

    ```Ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # good
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # better
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```

8. Put more specific exceptions higher up the rescue chain, otherwise
  they'll never be rescued from.

    ```Ruby
    # bad
    begin
      # some code
    rescue Exception => e
      # some handling
    rescue StandardError => e
      # some handling
    end

    # good
    begin
      # some code
    rescue StandardError => e
      # some handling
    rescue Exception => e
      # some handling
    end
    ```

9. Release external resources obtained by your program in an ensure
block.

    ```Ruby
    f = File.open('testfile')
    begin
      # .. process
    rescue
      # .. handle error
    ensure
      f.close unless f.nil?
    end
    ```

10. Favor the use of exceptions for the standard library over
introducing new exception classes.

## Collections

1. Prefer literal array and hash creation notation (unless you need to
pass parameters to their constructors, that is).

    ```Ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    hash = {}
    ```

2. Prefer `%w` to the literal array syntax when you need an array of
strings.

    ```Ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w[draft open closed]
    ```

3. Avoid the creation of huge gaps in arrays.

    ```Ruby
    arr = []
    arr[100] = 1 # now you have an array with lots of nils
    ```

4. Use `Set` instead of `Array` when dealing with unique elements. `Set`
  implements a collection of unordered values with no duplicates. This
  is a hybrid of `Array`'s intuitive inter-operation facilities and
  `Hash`'s fast lookup.
5. Use symbols instead of strings as hash keys.

    ```Ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

6. Avoid the use of mutable object as hash keys.
7. Prefer the hash syntax introduced in 1.9 over hash rockets.

    ```Ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

8. Never modify a collection while traversing it.

## Strings

1. Prefer string interpolation instead of string concatenation:

    ```Ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

2. Don't pad string interpolation code with space.

    ```Ruby
    # bad
    "#{ user.last_name }, #{ user.first_name }"

    # good
    "#{user.last_name}, #{user.first_name}"
    ```

3. Prefer single-quoted strings when you don't need string interpolation or
  special symbols such as `\t`, `\n`, `'`, etc.

    ```Ruby
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```

4. Use `{}` around instance variables being interpolated into a string.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # bad
      def to_s
        "#@first_name #@last_name"
      end

      # good
      def to_s
        "#{@first_name} #{@last_name}"
      end
    end
    ```

5. Avoid using `String#+` when you need to construct large data chunks.
  Instead, use `String#<<`. Concatenation mutates the string instance in-place
  and is always faster than `String#+`, which creates a bunch of new string objects.

    ```Ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

## Regular Expressions

1. Don't use regular expressions if you just need plain text search in string:
  `string['text']`
2. For simple constructions you can use regexp directly through string index.

    ```Ruby
    match = string[/regexp/]             # get content of matched regexp
    first_group = string[/text(grp)/, 1] # get content of captured group
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```

3. Use non capturing groups when you don't use captured result of parenthesis.

    ```Ruby
    /(first|second)/   # bad
    /(?:first|second)/ # good
    ```

4. Avoid using $1-9 as it can be hard to track what they contain. Named groups
  can be used instead.

    ```Ruby
    # bad
    /(regexp)/ =~ string
    ...
    process $1

    # good
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

5. Character classes have only few special characters you should care about:
  `^`, `-`, `\`, `]`, so don't escape `.` or brackets in `[]`.

6. Be careful with `^` and `$` as they match start/end of line, not string endings.
  If you want to match the whole string use: `\A` and `\z` (not to be
  confused with `\Z` which is the equivalent of `/\n?\z/`).

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # matches
    string[/\Ausername\z/] # don't match
    ```

7. Use `x` modifier for complex regexps. This makes them more readable and you
  can add some useful comments. Just be careful as spaces are ignored.

    ```Ruby
    regexp = %r{
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    }x
    ```

8. For complex replacements `sub`/`gsub` can be used with block or hash.

## Percent Literals

1. Use `%w` freely.

    ```Ruby
    STATES = %w[draft open closed]
    ```

2. Use `%[]` for single-line strings which require both interpolation
  and embedded double-quotes. For multi-line strings, prefer heredocs.

    ```Ruby
    # bad (no interpolation needed)
    %[<div class="text">Some text</div>]
    # should be '<div class="text">Some text</div>'

    # bad (no double-quotes)
    %[This is #{quality} style]
    # should be "This is #{quality} style"

    # bad (multiple lines)
    %[<div>\n<span class="big">#{exclamation}</span>\n</div>]
    # should be a heredoc.

    # good (requires interpolation, has quotes, single line)
    %[<tr><td class="name">#{name}</td>]
    ```

3. Use `%r` only for regular expressions matching *more than* one '/' character.

    ```Ruby
    # bad
    %r[\s+]

    # still bad
    %r[^/(.*)$]
    # should be /^\/(.*)$/

    # good
    %r[^/blog/2011/(.*)$]
    ```

4. Avoid `%q`, `%Q`, `%x`, `%s`, and `%W`.

5. Prefer `[]` as delimiters for all `%` literals.

## Metaprogramming

1. Do not mess around in core classes when writing libraries. (Do not monkey
patch them.)

* The block form of `class_eval` is preferable to the string-interpolated form.
  - when you use the string-interpolated form, always supply `__FILE__` and `__LINE__`, so that your backtraces make sense:

    ```ruby
    class_eval 'def use_relative_model_naming?; true; end', __FILE__, __LINE__
    ```

  - `define_method` is preferable to `class_eval{ def ... }`

2. When using `class_eval` (or other `eval`) with string interpolation, add a comment block showing its appearance if interpolated (a practice I learned from the rails code):

    ```ruby
    # from activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1

          # Result:
          # def capitalize(*args, &block)
          #   to_str.capitalize(*args, &block)
          # end
          def #{unsafe_method}(*args, &block)
            to_str.#{unsafe_method}(*args, &block)
          end

          # Result:
          # def capitalize!(*args)
          #   @dirty = true
          #   super
          # end
          def #{unsafe_method}!(*args)
            @dirty = true
            super
          end
        EOT
      end
    end
    ```

3. avoid using `method_missing` for metaprogramming. Backtraces become messy; the behavior is not listed in `#methods`; misspelled method calls might silently work (`nukes.launch_state = false`). Consider using delegation, proxy, or `define_method` instead.  If you must, use `method_missing`,
  - be sure to [also define `respond_to?`](http://devblog.avdi.org/2011/12/07/defining-method_missing-and-respond_to-at-the-same-time/)
  - only catch methods with a well-defined prefix, such as `find_by_*` -- make your code as assertive as possible.
  - call `super` at the end of your statement
  - delegate to assertive, non-magical methods:

    ```ruby
    # bad
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # good
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # best of all, though, would to define_method as each findable attribute is declared
    ```

## Misc

1. Write `ruby -w` safe code.
2. Avoid hashes as optional parameters. Does the method do too much?
3. Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will be shorter than
  5 LOC. Empty lines do not contribute to the relevant LOC.
4. Avoid parameter lists longer than three or four parameters.
5. If you really have to, add "global" methods to Kernel and make them private.
6. Use class instance variables instead of global variables.

    ```Ruby
    #bad
    $foo_bar = 1

    #good
    class Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

7. Avoid `alias` when `alias_method` will do.
8. Use `OptionParser` for parsing complex command line options and
`ruby -s` for trivial command line options.
9. Code in a functional way, avoiding mutation when that makes sense.
10. Avoid needless metaprogramming.
11. Do not mutate arguments unless that is the purpose of the method.
12. Avoid more than three levels of block nesting.
13. Be consistent. In an ideal world, be consistent with these guidelines.
14. Use common sense.

# Contributing

Nothing written in this guide is set in stone. It's my desire to work
together with everyone interested in Ruby coding style, so that we could
ultimately create a resource that will be beneficial to the entire Ruby
community.

Feel free to open tickets or send pull requests with improvements. Thanks in
advance for your help!

# Spread the Word

A community-driven style guide is of little use to a community that
doesn't know about its existence. Tweet about the guide, share it with
your friends and colleagues. Every comment, suggestion or opinion we
get makes the guide just a little bit better. And we want to have the
best possible guide, don't we?
