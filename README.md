# Shortest FizzBuzz Approaches in PHP

## Explanation
FizzBuzz is an old coding challenge given to interviewees during job interviews. It's a simple challenge with many different approaches to gauge the interviewee's problem solving style. The task is to create a piece of code that will produce a text output with an increasing number on each line, or replace said numbers with "Fizz" if it's a multiple of 3, "Buzz" if it's a multiple of 5, or "FizzBuzz" if it's a multiple of both 3 and 5.

Example output (condensed into 1 line separated by commas): 1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, FizzBuzz, 16, 17, Fizz, 19, Buzz, ...

## The Setup
First, we will establish a baseline environment so we know how the code will function and how the PHP engine will react.

- We will turn on error reporting:
```php
ini_set('display_errors', 1);
error_reporting(E_ALL);
```
This will notify us of any problems and help guide us to writing more strict, production-worthy code and adhering to the PHP syntax. This wouldn't be a concern for most people but we will really be pushing the limits of code reduction further on.

- For this document, I will omit the PHP start (`<?=`, `<?php `) and end (`?>`) tags.
- All code can safely be saved as ANSI or UTF-8 without any issues unless stated otherwise (which it will). 
- I will limit output examples to the first 20 lines as that is generally enough lines to see the code is working.
- Counting bytes on a Windows machine usually means a new line constitutes 2 bytes for "\r\n".

## A Walkthrough Example
```php
for($i = 1; $i <= 100; $i++) {
  if($i % 3 == 0) {
    echo 'Fizz';
  }

  if($i % 5 == 0) {
    echo 'Buzz';
  }

  if($i % 3 > 0 && $i % 5 > 0) {
    echo $i;
  }

  echo PHP_EOL;
}
```
The above code is 197 bytes.

This code uses a `for` loop using `$i` to store our increasing number and it stops when this number reaches 100.

The first `if` condition performs a modulo (`%`) expression on our number. Using modulo 3, our result will count up to `n-1` (2 in this case) and count again from 0. It calculates the remainder from dividing `$i` by 3. If the result is equal to `0` then `$i` must be perfectly divisible by 3, with no remainder, and we should output `Fizz`.

The second `if` condition performs an identical task as the first `if` condition, but we check to see if it's divisible by 5 instead of 3. If it is, then we output `Buzz`.

These two `if` conditions will perform independent of each other. This means if `$i` is divisble by both 3 and 5, the result will be `FizzBuzz`. They are not separated by a new line because the code does not instruct it to.

The third `if` statement checks if both of the above remainders are `more than` zero at the same time. This means the above two `if` conditions were **not** met because they are not perfectly divisible by 3 or by 5 so we know Fizz, Buzz, and FizzBuzz were not produced; therefore, we output the number.

Finally, we output a new line. This is typically served as `"\n"` but PHP predefines this as a constant of `PHP_EOL` where EOL stands for End Of Line, the control character name for a new line.

## Tightening the Code
In most languages, we can condense code a bit more while keeping it sufficiently manageable.

- We can remove lots of unnecessary whitespaces in between the expressions.
- We can remove the braces (`{` and `}`) where 1 line of code is inside it.
- We can replace `PHP_EOL` with just a regular `"\n"`

```php
for($i=1; $i<=100; $i++) {
  if($i%3==0)
    echo 'Fizz';

  if($i%5==0)
    echo 'Buzz';

  if($i%3>0 && $i%5>0)
    echo $i;

  echo "\n";
}
```
The above code is 153 bytes.

There are still some whitespaces in the code, none of it contributes any purpose to how it works. It only benefits the developer reading the code to make it appear clearer. We could remove every whitespace and put all the code on a single line and that would result in 106 bytes of code but it's ugly. It's not even worth showing.

## Conditional Ternary Operators
Conditional ternary operators are becoming a lot more commonplace in various languages. It's likely to be the only operator that takes three operands. It works similar to the `if` and `else` statements but the ternary expression works to return a result. A ternary operator follows the syntax of `condition ? expression_1 : expression_2`. This whole operand will return `expression_1` if the `condition` evaluates to true, or it will return `expression_2` if the `condition` evaluates to false.

For example:
```php
if($i > 5)
  echo 'Output A';
else
  echo 'Output B';
```

We're using `echo` in both cases in this example so we can easily use a ternary operator instead:

```php
echo $i > 5 ? 'Output A' : 'Output B';
```

Just for this example exclusively, we can also demonstrate how we can make it shorter. Since the text `Output` is present in both cases, we can put it before our ternary operator and the result will remain the same.

```php
echo 'Output '.$i > 5 ? 'A' : 'B';
```

Moving back to our FizzBuzz code. We can replace our first 2 `if` statements with a couple of ternary operators and use a blank string when we don't want to output anything, and join them together and use `echo` to output.

We surround each ternary operator with brackets (`(` and `)`) to stop them interfering with each other.

```php
for($i=1; $i<=100; $i++) {
  echo ($i%3==0?'Fizz':'') . ($i%5==0?'Buzz':'');

  if($i%3>0 && $i%5>0)
    echo $i;

  echo "\n";
}
```
The above code is 136 bytes.

Where the code is checking if each of the remainders and outputting Fizz and/or Buzz, now we can remove the `==0` comparator. Any number greater than zero automatically evaluates to true with `if` statements and ternary operators so if we swap the expressions around and simply check for any number greater than zero, it will work the same.

We can change:
```php
echo ($i%3==0?'Fizz':'') . ($i%5==0?'Buzz':'');
```
To...
```php
echo ($i%3?'':'Fizz') . ($i%5?'':'Buzz');
```
Now our total code is 130 bytes.

We still have one last ternary operator to deal with and we can wrap that into the same line by nesting them.

The above ternary operator outputs to an `echo`, but we can put that ternary operator inside another ternary operator! Because an empty string evaluates to false, if our operator does not output Fizz, Buzz, or FizzBuzz, then it means we will have an empty string.

Ternary operators also have a cool feature where if you just pass a non-empty string into the condition, it'll always evaluate to true and return the first expression, *unless* it's in the format of `condition ?: expression_2` where you omit the first expression, and the operator will return the result of the condition if it evaluates to true.

For example:
```php
echo 'This is a string.' ?: ':(';
// outputs "This is a string"

echo '' ?: ':(';
// outputs ":("
```

With this information, we can drop our ternary operators from earlier into this new ternary operator format.

```php
for($i=1; $i<=100; $i++) {
  echo ($i%3?'':'Fizz') . ($i%5?'':'Buzz') ?: $i;
  echo "\n";
}
```
The above code is 95 bytes.

The `echo` "function" (it's not technically a function but I won't go into details) allows for multiple arguments, meaning you can comma-separate values and they will all be output. We can do this with the second `echo` that outputs the new line. The `echo` also does not need a space after it unless it's followed by a alphanumeric value, so variables and symbols are acceptable.

Suddenly, our `for` loop has 1 line and we can remove the braces! Now we can take away all the whitespaces.

```php
for($i=1;$i<=100;$i++)
  echo($i%3?'':'Fizz').($i%5?'':'Buzz')?:$i,"\n";
```
The above code is 73 bytes.

## Error Suppression
This is where things get fun.

Error suppression techniques allows us to break conventional norms of the PHP syntax. This is done with the commerial "at" symbol (`@`) before certain expressions.

For example:
```php
echo something; // no error suppression
// produces a Warning before outputting "something"

echo @something; // error suppression
// no warning, only outputs "something"

echo $variable; // no error suppression
// produces a Notice, no visible output but evaluates to NULL

echo @$variable; // error suppression
// no warnings or notices, no visible output but evaluates to NULL
```

We can do something very clever with this information. Because making error-suppressed references to undefined variables essentially return NULL values, which, when outputted, is equal to an empty string in some circumstances, we can have an array with a single item and refer to it with the index of zero, if that index is greater than zero then we get an empty string. This means we can make our code shorter without using `if` statements or ternary operators.

We essentially tell the code "If remainder value is equal to 0, then output something. If it's greater than 0 then we don't care and output an empty string." Keep in mind we still want the ternary operator that outputs a numerical value if we don't get Fizz or Buzz values.

The single values in the two arrays are just the strings Fizz and Buzz but since we're error-suppressing the array indeces because an index of 1 or more don't exist, we can take advantage of it and remove the quotes from the strings as they will output the strings without errors.

This is what the code looks like now:

```php
for($i=1;$i<=100;$i++)
  echo@[Fizz][$i%3].@[Buzz][$i%5]?:$i,"\n";
```
The above code is 67 bytes.

Interestingly, in our `for` loop, we explicitly initialise the variable `$i` and increment it with `$i++`.

Increasing a variable with `$i++` is identical to `$i = $i + 1`.

Because undefined variables essentially return a NULL value, performing this expression is equivalent to `$i = NULL + 1` which evaluates to 1, this is why `$i++` produces an error the first execution only (without error-suppression) and then it becomes defined and works as normal afterwards.

With this information, we can change out our `for` loop with a slightly shorter `while` loop instead.

```php
while(@$i++<100)
  echo@[Fizz][$i%3].@[Buzz][$i%5]?:$i,"\n";
```
The above code is 61 bytes.

Remove the whitespaces and new line, we have a 1-liner:
```php
while(@$i++<40)echo@[Fizz][$i%3].@[Buzz][$i%5]?:$i,"\n";
```
The above code is 56 bytes.

## The Final Touch
Everything written above I understand fully, the behaviours and quirks of PHP are very well-known to me.

However, there is one final thing I saw a Reddit user share when tackling this challenge.

They replaced the final `"\n"` with something very clever. They managed to squeeze those 4 bytes into 3.

```php
while(@$i++<40)echo@[Fizz][$i%3].@[Buzz][$i%5]?:$i,@~õ;
```
The above code is 55 bytes.

What on Earth is `~õ` you're probably asking yourself, remembering the `@` is error-suppression. This works the same as `~"õ"` like you would a normal string.

The first character `~` is a bitwise NOT operand. It is used to flip all binary 1s and 0s around in numeric values. Usually it leads to positive numbers becoming negative and vice versa because of the way signed integers work in programming. However, that is not exactly what's happening here.

The second character `õ` is a Latin lowercase O with a tilde above it and can be used as a constant names or variable names perfectly fine so it's just considered ordinary text. The potential issues from using this "extended Latin" character is how text editors encode it. Most text editors and IDEs will use UTF-8 by default now and actually confuses the PHP interpreter. UTF-8 character encoding will convert the `õ` character into a multibyte character meaning that 2 "special" bytes are used to represent this `õ` character and the PHP interpreter can't handle it as it is used directly in the inner-workings of code and not just a PHP string.

In order to use this character in your code, you either have to set your editor to use ANSI text encoding, or, you can use UTF-8 (or other character encodings) as long as you can force your editor to represent `õ` as a single byte in that specific position in your code. For example, in Notepad++, using UTF-8 but forcing it to use a single byte, it will display this character as an inverted "symbol" with `xF5` inside it. If you can do this, you should be good to go.

So now we got that out of the way, what actually is going on?

As mentioned, the bitwise NOT operand will flip all ones and zeros in whatever the operand is being applied to. This is usually done on numbers, but it can also be done on strings and will return a string of the same length where each byte in the string has its binary representation of ones and zeros flipped. Because strings are kind of like an array of 8-bit numbers, this only works in an 8-bit capacity. When this is done on our `õ` character, the bits flip and actually evaluate to the byte for a new line character.

For example:
```php
echo ord('õ');  // decimal 245, hexadecimal 0xF5, or binary 1111 0101
echo ord("\n"); // decimal 10,  hexadecimal 0x0A, or binary 0000 1010
```

Notice the binary values are inverse of each other.

## The Cheat

We used error suppression techniques to get away with some syntax-bending practices. We do this because sometimes PHP errors show up when we don't want them to, we know what we're doing is wrong but we don't care in this exercise, so we used `@` in our code a number of times. We're doing this assuming our PHP interpreter is configured to show errors; however, if our PHP interpreter is configured to **not** show errors, we can strip all those `@` symbols out.

PHP error reporting can be turned off in `php.ini` with `display_errors = 0`.
A cheat is that it can also be turned off with `error_reporting(0);` at runtime.

PHP-only short-circuit FizzBuzz code:
```php
while($i++<100)echo[Fizz][$i%3].[Buzz][$i%5]?:$i,~õ;
```
The above code is 52 bytes.

Ok, so we should probably put this code in a file to execute. Unfortunately, to run this from a file, we need to increases our character count a little bit because PHP requires a start tag to interpret the parts of files containing actual PHP code even if it is the only thing in the file. If we use PHP version 7.4 or earlier, we can potentially get away with using PHP short tags which means we just need to prepend the code with `<?=` (no spaces necessary). This brings the minimum valid PHP file byte count to 54. The worse news is PHP short tags were removed in PHP 8.0 and require `<?php ` (followed by a space) before the code. This brings the minimum valid PHP file byte count to 58.

Because PHP 8.0 and above has established itself since 2020, the file in this directory called `fizzbuzz.php` contains 58 bytes with the long start tag.

## Last Thoughts
The final byte count is open to interpretation. Most people would only count the parts of the code that are the make-up of the actual task only and not include things like start tags, error report disabling, or tinkering with different versions of PHP interpreters. These are mostly just setup steps and not directly related to the FizzBuzz code.

While short-circuited code like this is very fun to work on, I strongly advise developers not to intentionally code like this especially when working as part of a team who are expected to maintain the code you write. It will be irritating for them and it would be very difficult to read or understand for most people.

Short-circuit coding can get unimaginably more complex using barely-documented behaviours and can include taking advantage of quirks and bugs present in the interpreter to squeeze every little byte out of code. As much as I enjoy exploring and learning the amazing behaviour of interpreters and baffling people with code that looks somebody had a seizure on the keyboard, because many coding languages evolve over time, including PHP, some practices, bugs, and behaviours may change and therefore some syntaxes may not be very sustainable. Some practices may potentially adversely affect the performance of code if executed a lot in a short amount of time (such as the `@` error suppression).

I wrote the initial FizzBuzz code at the top of this document in about a minute. Ordinarily I would move on, but the challenge of pushing myself and testing code and seeing what does what, where I can place symbols, running various parallel tests to figure out the behaviours. It keeps me interested and motivated to learn more every time. I may not have written this document for 6 hours if not for short circuiting.

I attempted to get ChatGPT to tackle this problem for a while. I requested it to produce PHP code to output FizzBuzz using the shortest possible code. It provided me with broken code. I requested and requested in a number of ways that it was broken, not short enough, or not producing a correct FizzBuzz sequence. I got it to produce 13 different pieces of code until I figured it simply wouldn't be able to do it.

## Other Approaches

I found [this Reddit comment](https://www.reddit.com/r/PHP/comments/2hz5l2/comment/ckxcldr/) showing another interesting method:

```php
while($i++<100)echo$i%3?!$$i=$i:Fizz,$i%5?$$i:Buzz,~õ;
```
The above code is 54 bytes.

Although it's just 2 more bytes than the code we've been working on, it works in a completely different way. We can undress it to see exactly what it's doing. Straight away we see the `echo` is taking 3 arguments: One responsible for outputting Fizz, one for Buzz, and the new line. If we separate those out, add some whitespaces, it makes it a little bit clearer but it's still logically hard to see what's going on.

```php
while($i++<100) {
  echo $i%3 ? !$$i=$i : Fizz;
  echo $i%5 ?  $$i    : Buzz;
  echo ~õ;
}
```
