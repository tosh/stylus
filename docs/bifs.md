
## Built-in Functions

### red(color | hsl)

Return the red component of the given `color`.

     red(#c00)
     // => 204

### green(color | hsl)

Return the green component of the given `color`.

     green(#0c0)
     // => 204

### blue(color | hsl)

Return the blue component of the given `color`.

     red(#00c)
     // => 204

### alpha(color | hsl)

Return the alpha component of the given `color`.

      alpha(#fff)
      // => 1
      
      alpha(rgba(0,0,0,0.3))
      // => 0.3

### hue(color | hsl)

Return the hue of the given `color`.

    hue(hsl(50deg, 100%, 80%))
    // => 50deg

### saturation(color | hsl)

Return the saturation of the given `color`.

    hue(hsl(50deg, 100%, 80%))
    // => 100%

### lightness(color | hsl)

Return the lightness of the given `color`.

    hue(hsl(50deg, 100%, 80%))
    // => 80%

### typeof(node)

Return type of `node.` as a string.

      type(12)
      // => 'unit'

      typeof(12)
      // => 'unit'
      
      typeof(#fff)
      // => 'color'

      type-of(#fff)
      // => 'color'

Aliased as `type-of` and `type`.

### unit(unit[, type])

Return a string for the type of `unit` or an empty string,
or assign the given `type` without unit conversion.

    unit(10)
    // => ''
    
    unit(15in)
    // => 'in'
    
    unit(15%, 'px')
    // => 15px

    unit(15%, px)
    // => 15px

### match(pattern, string)

Test if `string` matches the given `pattern`.

    match('^foo(bar)?', foo)
    match('^foo(bar)?', foobar)
    // => true

    match('^foo(bar)?', 'foo')
    match('^foo(bar)?', 'foobar')
    // => true
    
    match('^foo(bar)?', 'bar')
    // => false

### abs(unit)

      abs(-5px)
      // => 5px

      abs(5px)
      // => 5px

### ceil(unit)

      ceil(5.5in)
      // => 6in

### floor(unit)

      floor(5.6px)
      // => 5px

### round(unit)

      round(5.5px)
      // => 6px

      round(5.4px)
      // => 5px

### min(a, b)

      min(1, 5)
      // => 1

### max(a, b)

      max(1, 5)
      // => 5

### even(unit)

      even(6px)
      // => true

### odd(unit)

      odd(5mm)
      // => true

### hsla(color | h[, s, l, a])

Convert the given `color` to an `HSLA` node,
or h,s,l,a component values.

     hsla(10deg, 50%, 30%, 0.5)
     // => HSLA

     hsla(#ffcc00)
     // => HSLA

### hsl(color | h[, s, l])

Convert the given `color` to an `HSLA` node,
or h,s,l component values.

     hsl(10, 50, 30)
     // => HSLA

     hsl(#ffcc00)
     // => HSLA

### rgba(color | r[, g, b, a])

Return a `Color` from the r,g,b,a channels or provide a color to tweak the alpha.

      rgba(255,0,0,0.5)
      // => rgba(255,0,0,0.5)
  
      rgba(255,0,0,1)
      // => #ff0000
  
      rgba(#ffcc00, 0.5)
      // rgba(255,204,0,0.5)

### rgb(color | r[, g, b])

Return a `Color` from the r,g,b channels or cast to a color.
    
    rgb(255,204,0)
    // => #ffcc00
    
    rgb(#fff)
    // => #fff

### lighten(color | hsl, amount)

Lighten the given `color` by `amount`.

    lighten(black, 50%)
    // => #808080

### darken(color | hsl, amount)

Darken the given `color` by `amount`.

    darken(white, 50%)
    // => #808080

### desaturate(color | hsl, amount)

Desaturate the given `color` by `amount`.

    desaturate(#f00, 40%)
    // => #c33

### saturate(color | hsl, amount)

Saturate the given `color` by `amount`.

    saturate(#c33, 40%)
    // => #f00

### unquote(str | ident)

  Unquote the given `str` and returned as a `Literal` node.
 
       unquote("sans-serif")
       // => sans-serif
 
       unquote(sans-serif)
       // => sans-serif

       unquote('1px / 2px')
       // => 1px / 2px

### operate(op, left, right)

  Perform the given `op` on the `left` and `right` operands:
  
      op = '+'
      operate(op, 15, 5)
      // => 20

### length([expr])

  Parenthesized expressions may act as tuples, the `length()` function returns the length of such expressions.

    length((1 2 3 4))
    // => 4

    length((1 2))
    // => 2
  
    length((1))
    // => 1
  
    length(())
    // => 0
  
    length(1 2 3)
    // => 1

    length(1)
    // => 1

    length()
    // => 0

### warn(msg)

  Warn with the given error `msg`, does not exit.

      warn("oh noes!")

### error(msg)

  Exits with the given error `msg`.

    add(a, b)
      unless a is a 'unit' and b is a 'unit'
        error('add() expects units')
      a + b

### p(expr)

 Inspect the given `expr`:
 
     fonts = Arial, sans-serif
     p('test')
     p(123)
     p((1 2 3))
     p(fonts)
     p(#fff)
     p(rgba(0,0,0,0.2))
     
     add(a, b)
       a + b
    
     p(add)

stdout:

     inspect: "test"
     inspect: 123
     inspect: 1 2 3
     inspect: Arial, sans-serif
     inspect: #fff
     inspect: rgba(0,0,0,0.2)
     inspect: add(a, b)

### opposite-position(positions)

 Return the opposites of the given `positions`.
  
     opposite-position(right)
     // => left

     opposite-position(top left)
     // => bottom right

     opposite-position('top' 'left')
     // => bottom right

### Undefined Functions

  Undefined functions will output as literals, so for example
  we may call `color-stop(50%, #fff)` within our css, and it will
  output as you would expect. We can use this within helpers as well.
  
  In the example below we simply define the function `stop()` which 
  returns the literal `color-stop()` call.
  
    stop(pos, color)
      color-stop(pos, color)

    stop(50%, orange)
    // => color-stop(50%, $ffa500)