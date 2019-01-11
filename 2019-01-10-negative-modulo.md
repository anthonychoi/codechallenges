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

The first

## Initial Solution

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

## Interesting Solutions

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

## Revised Solution

```c++
int dontGiveMeFive(int start, int end)
{

  int count = 0;
  int temp;

  // start to end, inclusive
  for (int i = start; i <= end; i++) {
    temp = i;

    // loop until 5 is found or integer becomes 0
    while (temp > 0 || temp < 0) {
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
