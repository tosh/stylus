
## Functions

 Stylus features powerful in-language function definition. Function definition appears identical to mixins, however functions may return a value.

### Return Values

 Let's try a trivial example, creating a function that adds two numbers.

    add(a, b)
      a + b

 We may then utilize this function in conditions, as property values, etc.
 
     body 
       padding add(10px, 5)

 Rendering
     
     body {
       padding: 15px;
     }

### Argument Defaults

 Optional arguments may default to a given expression. With Stylus we may even default arguments to leading arguments! For example:
 
 
     add(a, b = a)
       a + b

     add(10, 5)
     // => 15
     
     add(10)
     // => 20

note that since argument defaults are assignments, we can also utilize function calls for defaults:

     add(a, b = unit(a, px))
       a + b

### Function Bodies

 We can take our simple `add()` function further, by casting all units passed as `px` via the `unit()` built-in. Re-assigning each argument and providing a unit type string (or identifier), which disregards unit conversion.
 
     add(a, b = a)
       a = unit(a, px)
       b = unit(b, px)
       a + b

     add(15%, 10deg)
     // => 25

### Multiple Return Values

 Stylus functions can return several values, just as you can assign several values to a variable. For example the following is a valid assignment:
 
       sizes = 15px 10px
     
       sizes[0]
       // => 15px 

Similarly we may return several values:

       sizes()
         15px 10px

       sizes()[0]
       // => 15px

One slight exception is when return values are identifiers, for example the following looks like a property assignment to Stylus since no operators are present.

     swap(a, b)
       b a

To disambiguate we may either wrap in parens, or utilize the `return` keyword.

      swap(a, b)
        (b a)

      swap(a, b)
        return b a

### Conditionals

 Let's say we want to create a function named `stringish()` to see if the value given can be transformed to a string. We check if `val` is a string, or an ident which is string-like. Because undefined identifiers yield themselves as the value, we may compare them to them-selves as shown below, where `yes` and `no` are used in place of `true` and `false`.
 
 
     stringish(val)
       if val is a 'string' or val is a 'ident'
         yes
       else
         no

usage:

     stringish('yay') == yes
     // => true
   
     stringish(yay) == yes
     // => true
   
     stringish(0) == no
     // => true

__note__: `yes` and `no` are not boolean literals, they are simply undefined identifiers in this case.

Another example:


    compare(a, b)
      if a > b
        higher
      else if a < b
        lower
      else
        equal

usage:

    compare(5, 2)
    // => higher

    compare(1, 5)
    // => lower

    compare(10, 10)
    // => equal

### Aliasing

  To alias a function we can simply assign a function's name to a new identifier. For example our previous `add()` function could be exposed as `plus()` as well, simply by:
  
      plus = add
      
      plus(1, 2)
      // => 3

### Variable Functions

  In the same way that we can "alias" a function, we can pass a function as well, here our `invoke()` function accepts a function, so we can pass it `add()` or `sub()`.

    invoke(a, b, fn)
      fn(a, b)

    add(a, b)
      a + b

    body
      padding invoke(5, 10, add)
      padding invoke(5, 10, sub)

yielding:

    body {
      padding: 15;
      padding: -5;
    }

### arguments

 The `arguments` local is available to all function bodies, and contains all the arguments passed. For example:
 
     sum()
       n = 0
       for num in arguments
         n = n + num

     sum(1,2,3,4,5)
     // => 15

### Complex Example

Below we construct two functions, one named `opposite()` which will give the opposite of each positional identifier passed in, for example `left` will become `right`, and `top right`, will become `bottom left`.

First our helper function `opposite-position()` performs the this condition, simply returning a new identifier. Secondly `opposite()` iterates the positions given, appending them to our previous or empty expression.

    opposite-position(pos)
      if pos == top
        bottom
      else if pos == bottom
        top
      else if pos == left
        right
      else if pos == right
        left
      else
        error('Invalid position ' + pos)

    opposite(positions)
      for pos in positions
        pos = opposite-position(pos)
        ret = ret is defined ? ret pos : pos

usage:

    opposite(top)
    // => bottom
    
    opposite(left)
    // => right
    
    opposite(top left)
    // => bottom right
