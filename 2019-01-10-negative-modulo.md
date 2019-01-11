---
---

## The Problem

>[Don't give me five!](https://www.codewars.com/kata/5813d19765d81c592200001a) by: StevenVogel_79
>
>In this kata you get the start number and the end number of a region and should return the count of all numbers except numbers with a 5 in it. The start and the end number are both inclusive!
>
>Examples:
>```
>1,9 -> 1,2,3,4,6,7,8,9 -> Result 8
>
>4,17 -> 4,6,7,8,9,10,11,12,13,14,16,17 -> Result 12
>```
>The result may contain fives. ;-)
>
>The start number will always be smaller than the end number. Both numbers can be also negative!
>
>I'm very curious for your solutions and the way you solve it. Maybe someone of you will find an easy pure mathematics solution.
>
>Have fun coding it and please don't forget to vote and rank this kata! :-)
>
>I have also created other katas. Take a look if you enjoyed this kata!

What We Know:

- Only dealing with signed integers
- Don't count integers with a 5 digit in it
- Start int is always smaller than end int, and it's inclusive

```c++
// Initial Code
int dontGiveMeFive(int start, int end)
{
  return 0;
}
```
Questions I had:

- How do I figure out if an integer has a 5 digit in it?
- How do I traverse through all the digits in each integer?
- Is there anything important I need to know about negative numbers?

## My Initial Solution

```c++
int dontGiveMeFive(int start, int end)
{

  int count = 0;
  int temp;

  for (int i = start; i <= end; i++) {
    temp = i;

    while (temp > 0 || temp < 0) {
      if (temp % 5 == 0 && temp % 10 != 0) break;
      else temp /= 10;
    }

    if (temp == 0)
      count++;
  }

  return count;
}
```
I wanted to tackle this problem with a purely mathematical approach because I was able to answer all my questions with math:

- `temp % 5 == 0 && temp % 10 != 0` covers all the cases for a 5 digit
- `temp /= 10` allows us to traverse through all the digits in an int
- `temp > 0 || temp < 0` easy way to account for negative numbers

Some things I wondered about:

- I need to be able to exclude multiples of 10, hence `temp % 10 != 0` .
- Maybe I don't need to walk through the digits, but then I would fail in the case where both `% 5` and `% 10` is equivalent to 0 i.e. 50.
- Stepping through the digits eventually reduces the int to a 0 because a division operation on an int cuts off the remainder. That means if we end up with `temp == 0` we have an int to add to our count.
- Double check for cases with negative numbers and we see that there's not much to add since we are only dealing with two cases here: when `temp == 0` and when it's not. Just have to remember to include both sides in our while loop.

With that said, let's walk through the code.

1. Count variable to hold the number of integers without a 5 digit in it
2. Temp variable to hold the state of the current int
3. For loop to walk through the list of integers, inclusive
4. Store the current int into temp for processing
5. While loop to walk through all the digits in the int
6. Check to see if temp has a 5 digit, break out of while loop if so
7. If not then step to next digit
8. Exit out once we walk through all digits
9. Check to see if 5 digit was found, increment count if not
10. Go back to step 3 until we finish with list
11. Return our count of integers without a 5 digit

## Interesting Solutions By Other People

Here's a solution that I briefly considered following before scraping once I realized I could look through the integer with a division operation.

```c++
// by: siskin1, JígSāw , Elzei, Rauta
int dontGiveMeFive(int start, int end) {
  using namespace std;

  int j = 0;
  for (int i = start; i <= end; i++)
    if (to_string(i).find_first_of('5') == string::npos)
      j++;

  return j;
}
```
By converting the integer to a string and then using a function to find the first instance of a 5, this solution makes for a very readable one. If we take a look at the simplicity though, there's a lot lacking.

First you have to understand how the compiler compiles the function, which may not be pretty. Then if you also bring into the consideration that a simpler math operation does the same job, there is no need to call any of the string functions.

If an argument can be made that the mathematical operations needed lessens readability or if the overall language in the project follows the same style, then I can see this solution as fitting. But that's not the case here.

This next solution is one I love.
```c++
// by: jevans8190
bool containsFive(int num) {
  for (; num > 0; num /= 10) {
    if (num%10 == 5) { return true; }
  }
  return false;
}

int dontGiveMeFive(int start, int end) {
  int count = 0;
  for (int i = start; i <= end; ++i) {
    if (!containsFive(abs(i))) { count++; }
  }
  return count;
}
```

In comparison to my original solution, this author did a `num % 10 == 5` operation. It's a beautiful piece of code that covers all its bases. Now I'm not sure if you really needed to separate the two functions, but that comes down to the overall style of the project and whether or not you use the `containsFive` function in other scenarios.

One other thing to note about this solution is the use of the `abs()` function. It's useful here, albeit a little unnecessary, but we can see how it influences the `containsFive` function in a major way. It removes the case of negative integers, thereby allowing the function to simply iterate until it's 0 or below. This will be important once we deal with other types of numbers like floats and doubles.

## My Revised Solution

```c++
int dontGiveMeFive(int start, int end)
{

  int count = 0;
  int temp;

  // start to end, inclusive
  for (int i = start; i <= end; i++) {
    temp = i;

    // loop until 5 is found or integer becomes 0
    while (temp != 0) {
      if (temp % 10 == 5 || temp % 10 == -5) break;
      else temp /= 10;
    }

    // temp = 0 only if no 5 is found
    if (temp == 0)
      count++;
  }

  return count;
}
```
Using the knowledge gained, I've decided on two improvements.
- Changed the while condition to `temp != 0` which is really just a brain fart on my part because my original condition said the same thing in a stupid way.
- Implemented the expression `temp % 10 == 5` along with it's negative counterpart `temp % 10 == -5` since it's so much cleaner.

The major hiccup I experienced while revising the code occurred in the way C++ handles the `%` operation. I didn't remember that modding a negative number would return a negative number since the modulo operation on Google's calculator always returned a positive number.

To pinpoint exactly what was happening, I wrote a few test cases to narrow down the scenarios and included my favorite debugging tool; the print statement in key places `std::cout << "Test: " << temp << std::endl;` . And that's the story of how I figured out that I needed to include `temp % 10 == -5` in my expression.

I decided not to use the `abs()` function here because I didn't need it and it doesn't really make the code that much more readable. However, if I were to use it I would change the code in three places:
- `temp = i;` to `temp = abs(i);` so I avoid multiple calls to the function
- `temp != 0` to `temp > 0` one step into dealing with other types of numbers
- take out `temp % 10 == -5` because it's not needed anymore

## Additional Questions

- What if we were dealing with doubles or floats?
- How do things change if we were passed in a string of numbers?
- What if we allow the user to choose a range of numbers to ignore?
- What's the fastest way to solve this problem?
- Which way uses the least amount of memory?

## Contributors

- anthonychoi
