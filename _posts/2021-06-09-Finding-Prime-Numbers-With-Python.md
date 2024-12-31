---
layout: post
title: Finding Prime Numbers with Python
image: "/posts/primes_image.jpeg"
tags: [Python, Primes]
---

In this post, I’ll demonstrate a Python function designed to efficiently identify all prime numbers below a specified value. For instance, if you provide the function with the value 100, it will return all the prime numbers less than 50!

If you're unsure what a prime number is, it’s a number that can only be divided evenly by 1 and itself. For example, 5 is a prime number because no numbers other than 1 and 5 divide into it without leaving a remainder. On the other hand, 6 is not a prime number, as it can also be divided evenly by 2 and 3, in addition to 1 and 5.

Let's dive in!

---

First, let’s set up a variable to define the upper limit of numbers we want to search through. We’ll start with 20, meaning we want to find all prime numbers less than or equal to 20.

```ruby
n = 20
```

The smallest prime number is 2, so we’ll start by creating a collection of numbers to check—every integer from 2 up to the upper limit we defined earlier (in this case, 20). Since the range function in Python excludes the upper limit, we use n+1 to include 20 in the range.

Instead of using a list, we’ll use a set. Sets have special properties that will make it easier to remove non-prime numbers during our search. You’ll see how this works shortly!

```ruby
number_range = set(range(2, n+1))
```


Let’s also set up a place to store the prime numbers we find. A list will be ideal for this purpose.

```ruby
primes_list = []
```

We’ll use a while loop to iterate through our set and check for primes, but before constructing the loop, I find it useful to manually code the logic and iterate through the numbers first. This allows me to verify that the process is working correctly before automating the entire check.

So, we have our set of numbers (called number_range), which contains all integers between 2 and 20. Let’s start by extracting the first number from the set to check if it’s a prime. If the number is a prime, we’ll add it to our list called primes_list. If it isn’t a prime, we won’t keep it.

To remove an element from a set or list and retrieve its value, we can use the *pop* method.

```ruby
print(number_range)
>>> {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```

If we use the pop method and assign the result to an object called **prime**, it will remove the first element from the **number_range** set and assign it to **prime**.
```ruby
prime = number_range.pop()
print(prime)
>>> 2
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```

Since we know that the very first value in our range is a prime (as there’s nothing smaller than it to divide evenly), we can confidently add it to our list of primes.

```ruby
primes_list.append(prime)
print(primes_list)
>>> [2]
```

Now, we’re going to perform a special step to check the remaining numbers in our number_range for non-primes. For the prime we just checked (in this case, 2), we’ll generate all its multiples up to the upper limit (20 in our case).

We’ll use a set again, rather than a list, because sets offer some unique functionality that will be useful in this approach, and this is where the magic happens.
```ruby
multiples = set(range(prime*2, n+1, prime))
```

Remember, when creating a range, the syntax is range(start, stop, step). For the starting point, we don’t need to include our number since it has already been added as a prime. Instead, we’ll start the range of multiples at 2 * our number, which is the first multiple. For example, if our number is 2, the first multiple will be 4. If we were checking 3, the first multiple would be 6, and so on.

For the stopping point of the range, we want the range to go up to 20, so we use n+1 to ensure 20 is included.

Now, the **step** is crucial here. Since we’re looking for multiples of our number, we want to increment by the value of the number itself. Therefore, we’ll use **prime** for the step value.

Let’s take a look at our list of multiples…

```ruby
print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```


Here comes the magic I mentioned earlier: we're using the special set functionality, **difference_update**, which removes any values from our number_range that are multiples of the number we just checked. The reason we do this is that if a number is a multiple of anything other than 1 or itself, it is **not a prime number**, and we can safely remove it from the list of numbers to be checked.

Before applying **difference_update**, let’s take a look at our two sets

```ruby
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}

print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

**difference_update** works in a way that will update one set to only include the values that are *different* from those in a second set

To use this, we put our initial set and then apply the difference update with our multiples

```ruby
number_range.difference_update(multiples)
print(number_range)
>>> {3, 5, 7, 9, 11, 13, 15, 17, 19}
```

Now, when we look at our number_range, all the values that were also present in the multiples set have been removed, as we know they are not prime numbers.

This is fantastic! We've significantly reduced the pool of numbers that need to be tested, making the process much more efficient. It also means that the smallest number in our range is guaranteed to be a prime, as we know nothing smaller than it divides evenly into it. This allows us to repeat the logic from the top.

Whenever you need to repeat a process until a condition is met, a while loop is often a great solution.

Here’s the code, with a while loop doing the heavy lifting of updating the number list and extracting primes until the list is empty.

Let’s run it to find all primes below 1000…

```ruby
n = 1000

# number range to be checked
number_range = set(range(2, n+1))

# empty list to append discovered primes to
primes_list = []

# iterate until list is empty
while number_range:
    prime = number_range.pop()
    primes_list.append(prime)
    multiples = set(range(prime*2, n+1, prime))
    number_range.difference_update(multiples)
```

Let's print the primes_list to have a look at what we found!

```ruby
print(primes_list)
>>> [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997]
```

Let's now get some interesting stats from our list which we can use to summarise our findings, the number of primes that were found, and the largest prime in the list!

```ruby
prime_count = len(primes_list)
largest_prime = max(primes_list)
print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
>>> There are 168 prime numbers between 1 and 1000, the largest of which is 997
```

Amazing!

The next thing to do would be to put it into a neat function, which you can see below:

```ruby
def primes_finder(n):
    
    # number range to be checked
    number_range = set(range(2, n+1))

    # empty list to append discovered primes to
    primes_list = []

    # iterate until list is empty
    while number_range:
        prime = number_range.pop()
        primes_list.append(prime)
        multiples = set(range(prime*2, n+1, prime))
        number_range.difference_update(multiples)
        
    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
```

Now we can jut pass the function the upper bound of our search and it will do the rest!

Let's go for something large, say a million...

```ruby
primes_finder(1000000)
>>> There are 78498 prime numbers between 1 and 1000000, the largest of which is 999983
```

That is pretty cool!

I hoped you enjoyed learning about Primes, and one way to search for them using Python.

---

###### Important Note: Using pop() on a Set in Python
In practical applications, it's important to consider the behavior of the pop() method when used on a Set, as it can sometimes be inconsistent.

The pop() method typically extracts an arbitrary element from the Set. While it often retrieves the lowest element, Sets in Python are unordered. Internally, the items are stored based on their hash values, which allows for fast retrieval. This hashing mechanism means we can't fully rely on pop() returning the lowest value. In rare cases, the hash may cause a different element to be returned.

Although in this case we’re working on a fun exercise, it's worth noting this potential inconsistency when using Sets and pop() in Python going forward.

To ensure we always get the minimum value, we can replace the line...

```ruby
prime = number_range.pop()
```

...with the lines...

```ruby
prime = min(sorted(number_range))
number_range.remove(prime)
```

...where we first identify the lowest number in number_range and assign it to our prime variable, then remove it.

However, because we need to sort the set on each iteration of the loop to find the minimum value, this approach is slightly slower than using pop(), which would typically be faster due to its constant time complexity for removing an arbitrary element from a set.
