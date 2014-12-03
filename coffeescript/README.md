# CoffeeScript Styleguide

*A conversion to CoffeeScript of a mostly reasonable approach to JavaScript*

Built upon the [AirBnb JavaScript Styleguide](https://github.com/airbnb/javascript).

## <a name='TOC'>Table of Contents</a>

  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [This](#this)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Operators](#operators)
  1. [Conditional Expressions](#conditionals)
  1. [Blocks](#blocks)
  1. [Parenthesis](#parenthesis)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#leading-commas)
  1. [Type Casting & Coercion](#type-coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [jQuery](#jquery)
  1. [Modules](#modules)
  1. [Performance](#performance)
  1. [The JavaScript Style Guide Guide](#guide-guide)
  1. [License](#license)


## <a name='objects'>Objects</a>

  - Use the literal syntax for object creation.

    ```coffeescript
    # bad
    item = new Object()

    # good
    item = {}
    ```

  - Don't use [reserved words](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Reserved_Words) as keys.

    ```coffeescript
    # bad
    superman =
      class: 'superhero'
      default:
        clark: 'kent'
      private: true

    # good
    superman =
      klass: 'superhero'
      defaults:
        clark: 'kent'
      hidden: true
    ```
    **[[⬆]](#TOC)**

  - Don't use commas when writing properties and avoid using braces unless
    you're instantiating an empty object

    ```coffeescript
    # bad
    item = {
      foo: 'bar',
      biz: 'baz'
    }

    # good
    item = 
      foo: 'bar'
      biz: 'baz'

    # good
    emptyObject = {}
    ```

## <a name='arrays'>Arrays</a>

  - Use the literal syntax for array creation

    ```coffeescript
    # bad
    items = new Array()

    # good
    items = []
    ```

  - Always use array#push when appending new values to an array.

    ```coffeescript
    someStack = []

    # bad
    someStack[someStack.length] = 'abracadabra'

    # good
    someStack.push 'abracadabra'
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```coffeescript
    items = ['hello', 'world']

    # bad
    itemsCopy[i] = item for item, i in items

    # good
    itemsCopy = items.slice()
    ```
    **[[⬆]](#TOC)**


## <a name='strings'>Strings</a>

  - Use single quotes `''` for strings

    ```coffeescript
    # bad
    name = "Sheryl Lee"

    # good
    name = 'Sheryl Lee'

    # bad
    firstName = "<a href=\"/name\">Sheryl</a>"

    # good
    firstName = '<a href="/name">Sheryl</a>'
    ```

  - Use double quotes `""` for string interpolation.

    ```coffeescript
    first = 'J'
    last = 'Doe'

    # bad
    greeting = 'Welcome back, ' + first + ' ' + last + '.'

    # good
    greeting = "Welcome back, #{ first } #{ last }."
    ```

  - Strings longer than 80 characters should be converted to block strings.
  - Note: If overused, long strings with concatenation could impact performance.
    [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

    ```coffeescript
    # bad
    errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.'

    # bad
    errorMessage = 'This is a super long error that ' +
      'was thrown because of Batman.' +
      'When you stop to think about ' +
      'how Batman had anything to do ' +
      'with this, you would get nowhere ' +
      'fast.'

    # good
    errorMessage = """
      This is a super long error that
      was thrown because of Batman.
      When you stop to think about
      how Batman had anything to do
      with this, you would get nowhere
      fast.
      """
    ```

  - When programatically building up a string, use Array#join instead of string
    concatenation. Mostly for IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```coffeescript
    words = ['The', 'owls', 'are', 'not', 'what', 'they', 'seem']

    # bad
    createList = ->
      list = '<ul>'
      list += "<li>#{ word }</li>" for word in words
      list += '</ul>'
      list

    # good
    createList = ->
      "<ul><li>#{ words.join('</li><li>') }</li></ul>"
    ```

    **[[⬆]](#TOC)**


## <a name='this'>This</a>

  - Use the `@` shortcut when accessing and setting properties.

    ```coffeescript
    # bad
    Lodge = (interdimensional) ->
      this.redRoom = interdimensional

    # good
    Lodge = (interdimensional) ->
      @redRoom = interdimensional
    ```

  - Avoid the `@` shortcut when just referencing `this` without a property.

    ```coffeescript
    # bad
    pie.call @, 'cherry', 2, 'slices'

    # good
    pie.call this, 'cherry', 2, 'slices'

    # bad
    haveSome = (what) ->
      console.log "Have some #{ what }."
      @

    # good
    haveSome = (what) ->
      console.log "Have some #{ what }."
      this
    ```

  - Try to use the fat arrow (`=>`) sparingly. Avoid using it if you don't need
    to save the current value of `this` further into the scope.

    **[[⬆]](#TOC)**


## <a name='functions'>Functions</a>

  - Function expressions:

    ```coffeescript
    # anonymous function expression
    anonymous = ->
      return true

    # immediately-invoked function expression (IIFE)
    do ->
      console.log 'Welcome to the Internet. Please follow me.'
    ```

  - Don't declare parenthesis if the function doesn't take any arguments.

    ```coffeescript
    # bad
    test = () ->
      console.log 'Nope.'

    # good
    test = ->
      console.log 'Yep.'
    ```

  - Never declare a function in a non-function block.
  - **Note:** ECMA-262 defines a `block` as a list of statements. A function
    declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```coffeescript
    # bad
    if currentUser
      test = ->
        console.log 'Nope.'
    ```

  - Never name a parameter `arguments`, as it will take precedence over the
    `arguments` object that is given to every function scope.

    ```coffeescript
    # bad
    nope = (name, options, arguments) ->
      # ...stuff...

    # good
    yup = (name, options, args) ->
      # ...stuff...
    ```

    **[[⬆]](#TOC)**


## <a name='properties'>Properties</a>

  - Use dot notation when accessing properties.

    ```coffeescript
    agent =
      fbi: true
      age: 31

    # bad
    isFbi = agent['fbi']

    # good
    isFbi = agent.fbi
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```coffeescript
    users =
      jane: {}
      john: {}

    userName = getUsername()
    userObject = users[userName]
    ```

    **[[⬆]](#TOC)**


## <a name='variables'>Variables</a>

  - Assign variables at the top of their scope. This helps avoid issues with
    variable declaration and assignment hoisting related issues.

    ```coffeescript
    # bad
    sample = ->
      test()
      console.log 'doing stuff..'

      # ..other stuff..

      name = getName()

      if name == 'test'
        return false

      name

    # good
    sample = ->
      name = getName()

      test()
      console.log 'doing stuff..'

      # ..other stuff..

      if name == 'test'
        return false

      name
    ```

  - If you need to return false on a method, declare your variables after so as
    not to waste an unnecessary assignment.

    ```coffeescript
    # bad
    sample = ->
      name = getName()

      if (!arguments.length)
        return false

      true

    # good
    sample = ->
      if (!arguments.length)
        return false

      name = getName()

      true
    ```

    **[[⬆]](#TOC)**


## <a name='operators'>Operators</a>

  - Avoid CoffeeScript's operator "shortcuts".

    ```coffeescript
    # bad
    if foo is bar

    # good
    if foo == bar

    # bad
    if foo isnt on

    # good
    if foo != on

    # bad
    if pie and not flavorless

    # good
    if pie && !flavorless

    # bad
    if coffee then sipCoffee()

    # good
    if coffee
      sipCoffee()

    # good
    sipCoffee() if coffee
    ```

  - If returning `this` at the end of a block, use `this`, rather than
    the `@` shortcut.

    ```coffeescript
    # bad
    Agent = (name, options) ->
      @fbi = options.fbi

      @

    # good
    Agent = (name, options) ->
      @fbi = options.fbi

      this
    ```

    **[[⬆]](#TOC)**


## <a name='conditionals'>Conditional Expressions</a>

  - Conditional expressions are evaluated using coercion with the `ToBoolean`
    method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evalute to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```coffeescript
    if [0]
      # true
      # An array is an object, objects evaluate to true
    ```

  - Use shortcuts.

    ```coffeescript
    # bad
    if name != ''
      # ...stuff...

    # good
    if name
      # ...stuff...
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

    **[[⬆]](#TOC)**


## <a name='blocks'>Blocks</a>

  - Always put blocks on their own lines.

    ```coffeescript
    # bad
    bad = -> false

    # good
    good = ->
      false
    ```

    **[[⬆]](#TOC)**


## <a name='parenthesis'>Parenthesis</a>

  - Avoid using parenthesis in conditionals.

    ```coffeescript
    # bad
    if (coffee)
      console.log 'on my way as soon as I finish my coffee.'
    else if (pie)
      console.log 'join us for cherry pie!'
    else
      console.log 'i want 16 cups of coffee!'

    # good
    if coffee
      console.log 'on my way as soon as I finish my coffee.'
    else if pie
      console.log 'join us for cherry pie!'
    else
      console.log 'i want 16 cups of coffee!'
    ```

  - Do not use parenthesis when passing arguments to a single function.

    ```coffeescript
    # bad
    logLady.say('Someday my log has something to say about this.')

    # good
    logLady.say 'Someday my log has something to say about this.'

    # bad
    $.ajax(
      type: 'GET'
      url: resource
      data: donuts
    )

    # good
    $.ajax
      type: 'GET'
      url: resource
      data: donuts
    ```

  - Do use parenthesis on **all** methods when chaining.

    ```coffeescript
    # bad
    $cafe
      .on('order', placeOrder)
      .on 'eat', enjoyMeal

    # good
    $cafe
      .on('order', placeOrder)
      .on('eat', enjoyMeal)
    ```

    **[[⬆]](#TOC)**


## <a name='comments'>Comments</a>

  - Documentation is highly recommended on Utility Modules. It is optional on
    all other types of Modules.
  - Don't over document. Opt for descriptive names of variables, functions, and
    returns over documenting the obvious.
  - Use [JSDoc](http://usejsdoc.org) notation for commenting guidelines.
  - Use `### ... ###` for multiline comments.

    ```coffeescript
    # bad
    # make() returns a new element
    # based on the passed in tag name
    #
    # @param {String} tag
    # @return {Element} element
    make = (tag) ->
      # ...stuff...

      element

    # good
    ###
    make() returns a new element
    based on the passed in tag name

    @param {String} tag
    @return {Element} element
    ###
    make = (tag) ->
      # ...stuff...

      element
    ```

  - Use `#` for single line comments. Always place single line comments on a
    newline above the subject of the comment. Put an empty line before the comment.

    ```coffeescript
    # bad
    active = true  # is current tab

    # good
    # is current tab
    active = true

    # bad
    getType = ->
      console.log 'fetching type...'
      # set the default type to 'no type'
      type = this._type || 'no type'

      type

    # good
    getType = ->
      console.log 'fetching type...'

      # set the default type to 'no type'
      type = this._type || 'no type'

      type
    ```

  - Always specify types and values for all parameters and return values.
    Ideally, provide a description for the function and parameters as well.

    ```coffeescript
    # bad
    make = (tag) ->
      # ...stuff...

    # good
    ###
    @param {String} tag
    @return {Element} element
    ###
    make = (tag) ->
      # ...stuff...
    }

    # best
    ###
    Returns a new element based on the passed in tag name.

    @param {String} tag
      Tag to create. -eg 'a', 'span', 'strong'
    @return {Element} element
      DOM element object.
    ###
    make = (tag) ->

      # ...stuff...

      element
    ```

    **[[⬆]](#TOC)**


## <a name='whitespace'>Whitespace</a>

  - Use soft tabs set to 2 spaces

    ```coffeescript
    # bad
    foo = ->
    ∙∙∙∙name = 'bar'

    # bad
    foo = ->
    ∙name = 'bar'

    # good
    foo = ->
    ∙∙name = 'bar'
    ```

  - When using `=` function assignment, place spaces around the `=`, and after
    any arguments.

    ```coffeescript
    # bad
    test=->
      console.log 'test'

    # bad
    test = (arg)->
      console.log arg

    # good
    test = ->
      console.log 'test'

    # good
    test = (arg) ->
      console.log arg
    ```

  - When using `:` function assignment, place 1 space after the `:` and after
    any arguments.

    ```coffeescript
    # bad
    test:->
      console.log 'test'

    # bad
    test : ->
      console.log 'test'

    # bad
    test: (arg)->
      console.log arg

    # good
    test: ->
      console.log 'test'

    # good
    test: (arg) ->
      console.log arg
    ```

  - Use indentation when making long method chains.

    ```coffeescript
    # bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    # good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    # bad
    var leds = stage.selectAll('.led').data(data).enter().append("svg:svg").class('led', true)
        .attr('width',  (radius + margin) * 2).append("svg:g")
        .attr("transform", "translate(" + (radius + margin) + "," + (radius + margin) + ")")
        .call(tron.led);

    # good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append("svg:svg")
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append("svg:g")
        .attr("transform", "translate(" + (radius + margin) + "," + (radius + margin) + ")")
        .call(tron.led);
    ```

    **[[⬆]](#TOC)**

## <a name='commas'>Commas</a>

  - No leading commas.

    ```coffeescript
    # bad
    myArray = [
        0
      , 1
      , 2
      , 3
    ]

    # good
    myArray = [
      0,
      1,
      2,
      3
    ]
    ```

  - No commas for objects.

    ```coffeescript
    # bad
    hero =
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'

    # good
    hero =
      firstName: 'Bob'
      lastName: 'Parr'
      heroName: 'Mr. Incredible'
      superPower: 'strength'
    ```

    **[[⬆]](#TOC)**


## <a name='type-coercion'>Type Casting & Coercion</a>

  - Perform type coercion at the beginning of the statement.
  - Strings:

    ```coffeescript
    #  => this.reviewScore = 9;

    # bad
    totalScore = this.reviewScore + ''

    # good
    totalScore = '' + this.reviewScore

    # bad
    totalScore = '' + this.reviewScore + ' total score'

    # good
    totalScore = "#{ this.reviewScore } total score"
    ```

  - Numbers:
  - Use `parseInt` for Numbers and always with a radix for type casting.
  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```coffeescript
    inputValue = '4'

    # bad
    val = new Number(inputValue)

    # bad
    val = +inputValue

    # good
    val = parseInt(inputValue, 10)
    ```

    **[[⬆]](#TOC)**


## <a name='naming-conventions'>Naming Conventions</a>

  - Avoid single letter names. Be descriptive with your naming. These all end up
    getting Uglified anyways.

    ```coffeescript
    # bad
    q = ->
      # ...stuff...

    # good
    query = ->
      # ..stuff..
    ```

  - Use PascalCase when naming Views, Constructors, Classes, Models, and Collections.

    ```coffeescript
    # bad
    user = (options) ->
      @name = options.name

    bad = new user
      name: 'nope'

    # good
    User = (options) ->
      @name = options.name

    good = new User
      name: 'yup'
    ```

  - Use lower camelCase for everything else (e.g.- objects, functions, and instances).

    ```coffeescript
    # bad
    OBJEcttsssss = {}
    this_is_my_object = {}
    this-is-my-object = {}

    # good
    thisIsMyObject = {}
    thisIsMyFunction = ->
    ```

  - If you need to save a reference to `this` use `self`.

    ```coffeescript
    # bad
    that = this

    bad = ->
      coffee (response) ->
        console.log that, this

    # good
    self = this

    good = ->
      coffee (response) ->
        console.log self, this
    ```

    **[[⬆]](#TOC)**


## <a name='accessors'>Accessors</a>

  - Accessor functions for properties are _not_ required
  - If you do make accessor functions, always prepend with `get` and `set`. Eg-
    getVal() and setVal('hello').

    ```coffeescript
    # bad
    agent.age()

    # good
    agent.getAge()

    # bad
    agent.age 31

    # good
    agent.setAge 31
    ```

  - If the property is a boolean, use isVal() or hasVal()

    ```coffeescript
    # bad
    if !agent.age()
      false

    # good
    if !agent.hasAge()
      false
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```coffeescript
    class Agent
      constructor: (options = {}) ->
        hasCoffee = options.hasCoffee || 'true'
        @set 'coffee', hasCoffee

      set: (key, val) =>
        @[key] = val

      get: (key) =>
        @[key]
    ```

    **[[⬆]](#TOC)**


## <a name='constructors'>Constructors</a>

  - Feel free to utilize CoffeeScript's support for building out and extending
    a basic class structure.

    ```coffeescript
    class Agent
      constructor:
        console.log 'new agent'

      drinkCoffee: ->
        @drinkingCoffee = true

      talkAboutCoffee: (height) ->
        console.log 'Damn good coffee!'
    ```

  - Methods can return `this` to help with method chaining.

    ```coffeescript
    class Agent
      constructor:
        console.log 'new agent'

      drinkCoffee: ->
        @drinkingCoffee = true
        this

      talkAboutCoffee: (height) ->
        console.log 'Damn good coffee!'
        this

    dale = new Agent()

    dale.drinkCoffee()
      .talkAboutCoffee()
    ```

    **[[⬆]](#TOC)**


## <a name='jquery'>jQuery</a>

  - Prefix jQuery object variables with a `$`.

    ```coffeescript
    # bad
    sidebar = $('.sidebar')

    # good
    $sidebar = $('.sidebar')

    # bad
    $primary_nav = $('#primary-nav')

    # good
    $primaryNav = $('#primary-nav')
    ```

  - If a jQuery lookup is performed more than once, cache the jQuery object.

    ```coffeescript
    # bad
    setSidebar = ->
      $('.sidebar').hide()

      # ...stuff...

      $('.sidebar').css
        'background-color': 'pink'

    # bad
    getSideBarHeight = ->
      $sidebar = $('.sidebar')

      $sidebar.height()

    # good
    getSideBarHeight = ->
      $('.sidebar').height()

    # good
    setSidebar = ->
      $sidebar = $('.sidebar')
      $sidebar.hide();

      # ...stuff...

      $sidebar.css
        'background-color': 'pink'
    ```

  - For DOM queries use Cascading `$('.sidebar ul')` or parent > child
    `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` with scoped jQuery object queries.

    ```coffeescript
    # bad
    $('.sidebar', 'ul').hide()

    # bad
    $('.sidebar').find('ul').hide()

    # good
    $('.sidebar ul').hide()

    # good
    $('.sidebar > ul').hide()

    # good (slower)
    $sidebar.find('ul')

    # good (faster)
    $($sidebar[0]).find('ul')
    ```

  - Don't combine elements with a class or ID as part of the selector. If this
    results in selecting elements you don't want, you probably need to refactor
    your DOM.

    ```coffeescript
    # bad
    $('form#new_user').submit()

    # bad
    $('li.user').hide()

    # good
    $('#new_user').submit()

    # good
    $('.user').hide()
    ```

  - Always use `#on` for attaching event handlers. You'll get the benefit of
    having a single syntax rather than dozens of aliases and can also take
    advantage of delegated events.

    ```coffeescript
    # bad
    $('form').click doSomething

    # bad
    $('li').live 'click', doSomething

    # good
    $('form').on 'submit', doSomething

    # good
    $('ul').on 'click', 'li', doSomething
    ```

    **[[⬆]](#TOC)**


## <a name='modules'>Modules</a>

  - We use the JavaScript Module Pattern, as described by Ben Cherry.
    [Read up on it here](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html).
  - Always declare `'use strict'` at the top of the module.
  - Always return the original `module` object.
  - If you need to reference any other global objects, pass it through to the
    module's arguments to help provide faster lookups and prevent linting errors.

    ```coffeescript
    ###
    @namespace rootNamespace
    @memberOf rootNamespace.subNamespace
    ###
    root = exports ? this

    root.rootNameSpace = do (module = root.rootNameSpace || {}, $) ->
      'use strict'

      # Module Namespace Extension
      utils = module.utils ?= {}
      utilName = utils.utilName ?= {}

      # Private Variables
      foo = 'bar'

      # Private Method
      privateMethod = ->
        # ...

      # Public API
      utilName.publicMethod = ->
        # ...

      # Return the extended module
      module
    ```

    **[[⬆]](#TOC)**


## <a name='performance'>Performance</a>

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

  **[[⬆]](#TOC)**


## <a name='guide-guide'>The JavaScript Style Guide Guide</a>

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)


## <a name='license'>License</a>

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[[⬆]](#TOC)**

