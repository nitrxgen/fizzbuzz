# Shortest FizzBuzz Approaches in PHP

#### Explanation
FizzBuzz is an old coding challenge given to interviewees during job interviews. It's a simple challenge with many different approaches to gauge the interviewee's problem solving style. The task is to create a piece of code that will produce a text output with an increasing number on each line, or replace said numbers with "Fizz" if it's a multiple of 3, "Buzz" if it's a multiple of 5, or "FizzBuzz" if it's a multiple of both 3 and 5.

Example output (condensed into 1 line separated by commas): 1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, FizzBuzz, 16, 17, Fizz, 19, Buzz, ...

#### The Setup
First, we will establish a baseline environment so we know how the code will function and how the PHP engine will react.

- We will turn on error reporting:
```php
ini_set('display_errors', 1);
error_reporting(E_ALL);
```
This will notify us of any problems and help guide us to writing more strict, production-worthy code and adhering to the PHP syntax. This wouldn't be a concern for most people but we will really be pushing the limits of code reduction further on.

- For this document, I will omit the PHP start (`<?`) and end (`?>`) tags.
- All code can safely be saved as ANSI or UTF-8 without any issues unless stated otherwise (which it will). 
- I will limit output examples to the first 20 lines as that is generally enough lines to see the code is working.

#### A Walkthrough Example
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

This code uses a `for` loop using `$i` as where our increasing number is stored and it stops when this number reaches 100.

The first `if` condition performs a modulo expression on our number. Using modulo 3, our result will count up to `n-1` (2 in this case) and count again from 0. It calculates the remainder if you divided `$i` by 3. If the result is equal to `0` then `$i` must be perfectly divisible by 3, with no remainder, and we should output `Fizz`.

The second `if` condition performs an identical task as the first `if` condition, but we check to see if it's divisible by 5 instead of 3. If it is, then we output `Buzz`.

These two `if` conditions will perform independent of each other. This means if `$i` is divisble by both 3 and 5, the result will be `FizzBuzz`. They are not separated by a new line because the code does not instruct it to.

The third `if` statement checks if both of the above remainders are `more than` zero at the same time. This means the above two `if` conditions were **not** met because they are not perfectly divisible by 3 nor 5 so we know Fizz, Buzz, and FizzBuzz were not produced; therefore, we output the number.

Finally, we output a new line. This is typically served as `"\n"` but PHP predefines this as a constant of `PHP_EOL` where EOL stands for End Of Line, the control character name for a new line.

#### Cleaning the Code
In most languages, we can condense code a bit more while keeping it sufficiently manageable.

- We can remove lots of unnecessary whitespaces in between the expressions.
- We can remove the braces (`{` and `}`) where 1 line of code is inside it.

```php
for($i=1; $i<=100; $i++) {
  if($i%3==0)
    echo 'Fizz';

  if($i%5==0)
    echo 'Buzz';

  if($i%3>0 && $i%5>0)
    echo $i;

  echo PHP_EOL;
}
```

