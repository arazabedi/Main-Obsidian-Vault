##### Enumerable.reduce | Enumerable.inject 
___
Combine all elements of an eneumerable (e.g. `array`) by passing a symbol (e.g. :+, :-, :*, :/) or block ( e.g. |accumulator, current_value| accumulator + current_value). 

##### gsub
___
Return a copy of a passed string with substituted characters:

```ruby
"hello".gsub(/[aeiou]/, '*')                  #=> "h*ll*"
"hello".gsub(/([aeiou])/, '<\1>')             #=> "h<e>ll<o>"
"hello".gsub(/./) {|s| s.ord.to_s + ' '}      #=> "104 101 108 108 111 "
"hello".gsub(/(?<foo>[aeiou])/, '{\k<foo>}')  #=> "h{e}ll{o}"
'hello'.gsub(/[eo]/, 'e' => 3, 'o' => '*')    #=> "h3ll*"
```
