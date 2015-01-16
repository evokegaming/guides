# CoffeeScript Style Guide
## General
* Use UTF-8 file encoding.
* No trailing semicolons. CoffeeScript adds them automatically.

  ```coffeescript
  # bad
  foo = bar;

  # good
  foo = bar
  ```

* Keep lines to at most 80 characters.
* Avoid embedded JavaScript in CoffeScript files. External code should be in the vendor directory and vendor script should remain untouched and should not be affected by these guidelines. Refactor any copy-pasted code into JavaScript. [js2coffe.org](http://js2coffee.org/) can help, but don't rely on it fully.

  ```coffeescript
  # bad
  `function greet(name) {
    return "Hello " + name;
  }`

  # good
  greet = (name)->
    "Hello " + name
  ```

## Whitespace
* Use Unix line endings. That is LF (\n) and not CR (\r) or CRLF (\r\n).
* Always use indentation with 2 spaces:

  ```coffeescript
  # bad
  if false
  ∙∙∙∙foo = 'bar'

  # bad
  if false
  ∙foo = 'bar'

  # good
  if true
  ∙∙foo = 'bar'
  ```

* No trailing whitespace.

  ```coffeescript
  # bad
  foo = 'bar'∙∙∙∙
  foo = 'bar'∙

  # good
  foo = 'bar'
  ```

* Use a space around operators:

  ```coffeescript
  # bad
  x=y+5

  # good
  x = y + 5
  ```

* Use one space after commas unless it's the last character at the end of the line:

  ```coffeescript
  # bad
  foo = [1,2,3]

  # bad
  bar =
    foo:   'bar',∙
    hello: 'world'

  # good
  foo = [1, 2, 3]

  # good
  bar =
    foo:   'bar',
    hello: 'world'
  ```

* No spaces after `(`, `[` or before `]`, `)`:

  ```coffeescript
  # bad
  foo( x, y )

  # good
  foo(x, y)
  ```

* Do not use empty lines between objects property definitions.

  ```coffeescript
  # bad
  foo =
    foo:   'bar',

    hello: 'world'

  # good
  foo =
    foo:   'bar',
    hello: 'world'
  ```

* Use a blank line between function definitions as well as between function and class definitions.

  ```coffeescript
  # bad
  class Cats
    foo: ()->
      'bar'
    sound: ()->
      'mjau'

  # bad
  sum = (a, b)->
    a + b
  power = (x, y)->
    Math.pow(x, y)

  # good
  class Cat

    foo: ()->
      'bar'

    sound: ()->
      'mjau'

  # good
  sum = (a, b)->
    a + b

  power = (x, y)->
    Math.pow(x, y)
  ```

* Prefer keeping different classes and different class extensions in different files.
* Align object properties if they span over multiple lines.

  ```coffeescript
  # bad
  foo =
    foo: 'bar',
    hello: 'world'

  # good
  foo =
    foo:   'bar',
    hello: 'world'
  ```

* Single newline at the end of files.

  ```coffeescript
  # bad
  lastCodeInFile()
  ```

  ```coffeescript
  # bad
  lastCodeInFile()↵
  ↵
  ```

  ```coffeescript
  # good
  lastCodeInFile()↵
  ```


## Naming Conventions
* Avoid single letter names. Be descriptive, but concise with your naming.

  ```coffeescript
  # bad
  r = 6371

  # good
  earthRadius = 6371 # km
  ```


* Use camelCase when naming variables, object attributes, functions and instances - never use snake_case.

  ```coffeescript
  # bad
  foo_bar = 'cats'
  hello_world()

  # good
  fooBar = 'cats'
  helloWorld()
  ```

* Use PascalCase when naming constructors or classes.

  ```coffeescript
  # bad
  class foobar
  class foo_bar
  class fooBar

  # good
  class FooBar
  ```

* Keep acronyms like HTTP, RFC, XML uppercase.

  ```coffeescript
  # bad
  httpRequest = $.get('example.com', (json_response)-> void())
  documentRfc = 'foo'

  # good
  HTTPrequest = $.get('example.com', (JSONresponse)-> void())
  documentRFC = 'foo'
  ```

* Use a leading underscore _ when naming private properties.

## Comments
* Write self-documenting code and ignore the rest of this section. Seriously!
* Always use `#` for both single and multiple line comments if they don't have to be preserved in generated JS.
* Comments that are sentences should be capitalized and use punctuation. Use one space after periods to divide sentences.

  ```coffeescript
  # bad
  # however something happens here

  # good
  # Magic
  # However, something happens here.
  ```

* Avoid superfluous comments.

  ```coffeescript
  # bad
  return 1 # returns the number 1
  ```

* Spell check your comments.

  ```coffeescript
  # bad
  # Swiches raound ariables

  # good
  # Switches around variables
  ```

* Keep existing comments up-to-date. An outdated is worse than no comment at all.

  ```coffeescript
  # bad
  # TODO: Change to true after the summer of '99
  foo = true

  # good
  foo = true
  ```

* Avoid writing comments to explain bad code. Refactor the code to make it self-explanatory. (Do or do not - there is no try. --Yoda).

  ```coffeescript
  # bad
  # Do it if we have cats
  if not !!noCats

  # bad
  # You are not expected to understand this.

  # good
  hasCats = Boolean(cats)
  unless hasCats
  ```

* Comments should usually be written on the line immediately above the relevant code. Exceptions can be single words like a unit.

  ```coffeescript
  # bad
  Math.pow(x, y) # It grows exponentially!

  # good
  # It grows exponentially!
  Math.pow(x, y)

  # acceptable
  earthRadius = 6371 # km
  ```

* Use `# FIXME:` to annotate problems.

  ```coffeescript
  # bad
  # FIXME: Something is wrong here

  # good
  # FIXME: Refactor this ugly code to make it readable.
  ```

* Use `# TODO:` to annotate solutions to problems.

  ```coffeescript
  # bad
  # TODO: Make this work.

  # bad
  # TODO: Houston, we have a problem.

  # good
  # TODO: Remove this function after Christmas 2014.
  ```

* Use `# OPTIMIZE:` to note slow or inefficient code that may cause performance problems.

  ```coffeescript
  # bad
  # OPTIMIZE: It's slow!

  # good
  # OPTIMIZE: This loop goes through all the posts in the system, while it
  #       should only loop through the ones on that are shown on this page.
  ```

* Use `# HACK:` to note code smells where questionable coding practices were used and should be refactored away.

  ```coffeescript
  # bad
  # HACK: Ha ha!

  # good
  # HACK: Since we are stuck on an old version of the wheel it's actually
  #     an ellipsis so we need to extend the wheel to make it round.
  ```

* In cases where the problem is so obvious that any documentation would be redundant, annotations such as TODO or FIXME may be left at the end of the offending line with no note. This usage should be the exception and not the rule.
* Only use `###` block comments if they need to be preserved in generated code.

  ```coffeescript
  # bad
  ###
  FIXME: This actually allows us to login without authentication.
  ###

  ###
  This function does this, that and a bit more or less something
  ###

  # good
  ###
  Copyright 1999 Big Corporation.
  We will sue if this copyright notice is changed or removed.
  ###
  ```


## Conditionals
* Prefer `unless` over `if!` or `if not`.
* Favor use of `if` with `else`:
  ```Ruby
  # bad
  unless true
    'failure'
  else
    'success'
  end

  # good
  if true
    'success'
  else
    'failure'
  end
  ```

* Only use then in one line conditional or switch statements

  ```coffeescript
  # bad
  if true then
    foo = 'bar'

  switch foobar
    when 'foo' then
      'bar'
    when 'bar' then
      'foo'

  # good
  if true then foo = 'bar'

  switch foobar
    when 'foo' then 'bar'
    when 'bar' then 'foo'
  ```

* Omit parentheses for conditional statements and loops.

  ```coffeescript
  # bad
  if(foo > bar)
    'cake'

  # bad
  catCount = 0
  while(hasCats())
    catCount++

  # good
  if foo > bar
    'cake'

  # good
  catCount = 0
  while hasCats()
    catCount++
  ```

* Indent content of multiple line conditionals.
* Use parentheses to separate conditions in multiple line/checks conditionals

  ```coffeescript
  # bad
  if
    foo is 'test'
    or
    foo is 'bar'
    and
    bar is 'for'

  # good
  if (
    foo is 'test'
    or
    (
      foo is 'bar'
      and
      bar is 'for'
    )
  )
  ```

* Prefer chained comparisons over separate ones.

  ```coffeescript
  # bad
  if 43 > life and life > 41

  # good
  if 43 > life > 41
  ```


## Logic
* Prefer `and` over `&&`.
* Prefer `or` over `||`.
* Prefer `is` over `==`.
* Prefer `isnt` over `!=`.
* Prefer `not` over `!`.

## Strings
* Prefer single quotes `'` over double quotes `"` unless string interpolation is used.

  ```coffeescript
  # bad
  foo = "Lorem ipsum sit dolor"
  bar = 'Lorem ipsum #{variabletext} dolor'

  # good
  foo = 'Lorem ipsum sit dolor'
  bar = "Lorem ipsum #{variabletext} dolor"
  ```

* Prefer string interpolation over string concatenation.

  ```coffeescript
  # bad
  foo = 'Lorem ipsum ' + variabletext + ' dolor'

  # good
  foo = "Lorem ipsum #{variabletext} dolor"
  ```

* No line concatenation for long stings that span multiple lines to keep lines at 80 characters unless no space between lines is desired, then use backslash `\`.

  ```coffeescript
  # bad
  errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.'

  # bad
  errorMessage = 'This is a super long error that was thrown because \
  of Batman. When you stop to think about how Batman had anything to do \
  with this, you would get nowhere \
  fast.'

  # bad
  errorMessage = 'This is a super long error that was thrown because ' +
    'of Batman. When you stop to think about how Batman had anything to do ' +
    'with this, you would get nowhere fast.'

  # good
  errorMessage = 'This is a super long error that was thrown because
    of Batman. When you stop to think about how Batman had anything to do
    with this, you would get nowhere fast.'
  ```

* When programmatically building up a string, use Array.join instead of string concatenation for [performance](http://jsperf.com/string-vs-array-concat/2).

  ```coffeescript
  messages = [
    state: 'success'
    message: 'This one worked.'
  ,
    state: 'success'
    message: 'This one worked as well.'
  ,
    state: 'error'
    message: 'This one did not work.'
  ]

  length = messages.length

  # bad
  inbox = (messages)->
    items = ''

    messages.forEach((message)->
      items += '<li>#{message.subject}</li>'
    )

    '<ul>#{items}</ul>'
  }

  # good
  inbox = (messages)->
    items = []

    messages.forEach((message, key)->
      items[key] = message.subject
    )

    "<ul><li>#{items.join('</li><li>')}</li></ul>"
  }
  ```


## Objects
* Prefer `{}` over `new Object()`.
* Do not use leading commas.

  ```coffeescript
  # bad
  hero =
    firstName: 'Kevin'
    ,lastName: 'Flynn'

  heroes = [
    'Batman'
    ,'Superman'
  ]

  # good
  hero =
    firstName: 'Kevin',
    lastName: 'Flynn'

  heroes = [
    'Batman',
    'Superman'
  ]
  ```

* Do not use additional trailing commas at the end of a array or object declaration, since it can cause problems in IE.

  ```coffeescript
  # bad
  hero =
    firstName: 'Kevin',
    lastName: 'Flynn',

  heroes = [
    'Batman',
    'Superman',
  ]

  # good
  hero =
    firstName: 'Kevin',
    lastName: 'Flynn'

  heroes = [
    'Batman',
    'Superman'
  ]
  ```

* Prefer dividing object definitions into multiple lines without commas.

  ```coffeescript
  # bad
  colors =
    red: '#f00',
    green: '#0f0',
    blue: '#00f'

  # good
  colors =
    red: '#f00'
    green: '#0f0'
    blue: '#00f'
  ```

* Omit braces for object definitions except in ambiguous places such as function parameters.

  ```coffeescript
  # bad
  colors = {
    red: '#f00'
    green: '#0f0'
    blue: '#00f'
  }

  # bad
  x(a, b: 1, c:2)

  # good
  colors =
    red: '#f00'
    green: '#0f0'
    blue: '#00f'

  # good
  x(a, {b: 1, c:2})

  # good
  x(a, {b: 1}, {c:2})
  ```

* Use dot notation when accessing properties.

  ```coffeescript
  luke =
    jedi: true
    age: 28

  # bad
  isJedi = luke['jedi']

  # good
  isJedi = luke.jedi
  ```

* Use subscript notation `[]` when accessing properties with a variable.

  ```coffeescript
  # good
  luke =
    jedi: true
    age: 28

  getProp = (prop)->
    luke[prop]

  isJedi = getProp('jedi')
  ```

* Avoid using prototype, but if used prefer shorthand notation (`::`) for accessing an object's prototype.

  ```coffeescript
  Person = (name)-> @name = name

  # bad
  Person.prototype.sayHello = ()-> 'Hello ' + @name

  # bad
  Dog.prototype = new Animal()

  # good
  Person::sayHello = ()-> 'Hello ' + @name

  # good
  Dog:: = new Animal()
  ```


## Arrays
* Use the literal syntax for array creation.

  ```coffeescript
  # bad
  items = new Array()

  # good
  items = []
  ```

* Use `Array.push` to add an item to the end of an array.

  ```coffeescript
  someStack = []

  # bad
  someStack[someStack.length] = 'test'

  # good
  someStack.push('test')
  ```

* When you need to copy an array use Array.slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

  ```coffeescript
  # bad
  itemsCopy = []

  items.forEach((item)->
    itemsCopy[i] = item
  )

  # good
  itemsCopy = items.slice()
  ```

* Always use `Array.forEach` to iterate through arrays.

  ```coffeescript
  numbers = [1, 4, 9]
  sum = 0

  # bad
  for i = 0; i < numbers.length; i++
    sum += numbers[i] if numbers[i] > 3

  # good
  numbers.forEach((number)->
    sum += number if number > 3
  )
  ```

* Use the most appropriate array method for what you are trying to accomplish among `forEach`, `map`, `reduce`, `some`, `every`, `filter` etc. It is often the one that requires the least amount of code.

  ```coffeescript
  # bad
  numbers = [1, 4, 9]
  doubles = []
  numbers.forEach((num, key)->
    doubles[key] = num * 2
  )

  # good
  numbers = [1, 4, 9]
  doubles = numbers.map((num)-> num * 2)
  ```


## Variables
* Never initialize variables without a value (CoffeeScript does that automatically).
* Prefer existential operator `variable?` over checking null and undefined with typeof.

  ```coffeescript
  # bad
  if typeof foo isnt "undefined" && foo isnt null

  # bad
  bar = 'test'
  if bar isnt null

  # good
  if foo?

  # good
  bar = 'test'
  if bar?
  ```


## Type casting
* Use preceding type coercion like `'' + string` to convert to string.
* Prefer `Number(value)` over parseInt(value) and parseFloat(value) to cast to number.
* Always use radix with `parseInt`, since `parseInt(010) === 8`.
* Prefer `!!` to cast to boolean, unless it makes it difficult to understand because of negation, then use `Boolean()`.

## Functions
* Use parentheses for function definitions and calls.

  ```coffeescript
  # bad
  foo x, bar y, z

  # good
  foo(x, bar(y, z))
  ```

* Use parentheses in function declarations even if the function lacks parameters.

  ```coffeescript
  # bad
  foo = -> Math.rand()

  # good
  foo = ()-> Math.rand()
  ```

* Do not put any space before function arrows.

  ```coffeescript
  # bad
  foo = () -> Math.rand()

  # good
  foo = ()-> Math.rand()
  ```

* No unnecessary fat arrows.

  ```coffeescript
  # bad
  later = ()=> 'go fishing'

  # bad
  class A
    constructor: (@msg)->
    thin: ()-> alert(@msg)
  x = new A('yo')
  fn = (callback) -> callback()
  fn(x.thin) # alerts 'undefined'

  # good
  later = ()-> 'go fishing'

  # good
  class A
    constructor: (@msg)->
    fat: ()=> alert(@msg)
  x = new A('yo')
  fn = (callback) -> callback()
  fn(x.fat) # alerts 'yo'
  ```

* Never declare a function in a non-function block (`if`, `while`, etc). Assign the function to a variable instead.

  ```coffeescript
  # bad
  while ()-> true

  # good
  condition = ()-> true
  while condition
  ```

* Never name a parameter arguments, this will take precedence over the arguments object that is given to every function scope.

  ```coffeescript
  # bad
  nope = (name, options, arguments)->
    # ...stuff...

  # good
  nope = yup(name, options, args)->
    # ...stuff...
  ```

* Prefer splats (`...`) over using `arguments` variable for functions that accept variable numbers of arguments.

  ```coffeescript
  # bad
  foobar = ()->
    a = arguments[0]
    b = arguments[1]
    c = arguments[2]
    if arguments.length > 3
      rest = arguments.slice(3)

  # bad
  console.log(arguments)

  # good
  (a, b, c, rest...)->

  # good
  console.log(args...)
  ```

* Prefer `@` over `this`.

  ```coffeescript
  # bad
  color: 'red'
  getColor: ()->
    this.color

  # bad
  view = this

  # good
  color: 'red'
  getColor: ()->
    @color

  # good
  view = @
  ```

* Avoid return where not required. Only use return when it is explicitly intended to return before the end of the function.

  ```coffeescript
  # bad
  foo = (x)->
    return x * 2

  # good
  foo = (x)->
    x * 2

  foo = (x)->
    return 0 if x < 0
    x * 2
  ```
