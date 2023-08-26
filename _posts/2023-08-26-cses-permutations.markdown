---
layout: post
title:  "CSES Permutations"
date:   2023-08-26 22:23:58 +0530
categories: problem-solving learning
---
<nav>
  <h4>Table of Contents</h4>
  * this unordered seed list will be replaced by toc as unordered list
  {:toc}
</nav>

# Introduction
Hello There, In this post let's see how I tackled the `Permutations` problem from [CSES][cses-site].

`Permutations` is the `5th` problem from [CSES][cses-site]. I stored my solutions [here][github-permutations]. Let's explore
the solutions.

NOTE: To reduce character count shown in CSES I have used short variable names in the code.

[CSES Profile][cses-profile]


# Problem Statement
[Link][problem-link]

A permutation of integers 1,2,…,n is called beautiful if there are no adjacent elements whose difference is 1.

Given n, construct a beautiful permutation if such a permutation exists.


# Approach #1
We have `n`. So for the first approach I went on with the brute force method. That is to create all possible permutations
and find whether it is a solution or not.

We use the built-in `permutations` function from the `itertools`. 

For each of the permutations for `[1...n]`, we check whether all of the numbers from the permutation didn't contain the difference as `1` when compared to the adjacent elements.


## Observations
Obviously as expected for big permutations the code didn't stand. 

For short permutations it worked. From this I learned that 
there should be some other way to solve this problem. I thought for a little bit and searched the internet for possible ideas.


## Code
[Link][link-1]

{% highlight python %}
n = int(input())

from itertools import permutations

perm = permutations(list(range(1, n+1)))

for i in list(perm):
    flag = False
    for j in range(0, n):
        next_index = j + 1 if j != n -1 else None
        previous_index = j - 1 if j != 0 else None

        diff1 = None
        if next_index:
            diff1 = abs(i[next_index] - i[j])

        diff2 = None
        if previous_index:
            diff2 = abs(i[previous_index] - i[j])

        if diff1 == 1 or diff2 == 1:
            flag = True

        if flag:
            break

    if not flag:
        print(' '.join([str(k) for k in i]))
        break

    flag = False

print('NO SOLUTION')
{% endhighlight %}


# Approach #2
In the internet I found [this][idea-link].

From that blog I can formulate a solution for this problem. They are going with approach of the difference between `2` even or odd
numbers will always be `2`. 

So they take all of the even numbers in the first half of the permutation solution and rest will be the odd numbers. This way
we will always have difference between any `2` adjacent numbers as more than `1`.

For permutations of length `2` & `3` there are no solutions to make the difference more than `1`.


## Observations
The code this time succeeded. The [CSES][cses-site] max execution time was `~0.59 s`. But I was not satisfied with the execution time and I wanted to reduce it. As observed [here][so-gv-vs-lv], local variables perform better than global variables in `python`. So I updated the code accordingly that we will see in the next approach after this approach's code.


## Code
[Link][link-2]

{% highlight python %}
n = int(input())

if n in [2, 3]:
    print('NO SOLUTION')
    exit()

v = 2

while v <= n:
    print(v, end=' ')
    v += 2

v = 1

while v <= n:
    print(v, end=' ')
    v += 2
{% endhighlight %}


# Approach #3..5
As we have the working solution from `Approach #2`. Now it's time to optimize. 

In these approaches we converted the `global variable approach to local variable` approach.

Then I experimented with possible solutions for permutations for `2` & `3` lengths. Experimented with using `Set`, `==` & `List` for the `in` operation.

Also I tried to make a function for the inner `while` loop duplication and try to see how it affects the performance. Couldn't see any noticeable differences.

These approaches gave `~0.52 s`. But why stop. Let's optimize further.


## Observations
These approaches gave a little performance enhancement. I checked other python solution from CSES and found that my usage of `print` causes my program to slow down. Let's fix this in the next approach.


## Code
[Link #3][link-3]
[Link #4][link-4]
[Link #5][link-5]

{% highlight python %}
# 3
def a():
    n = int(input())

    if n in {2, 3}: 
        print('NO SOLUTION')
        exit()

    def b(v):
        while v <= n:
            print(v, end=' ')
            v += 2
    b(2)
    b(1)

a()

# 4
def a():
    n = int(input())

    if n == 2 or n == 3: 
        print('NO SOLUTION')
        exit()

    v = 2
    while v <= n:
        print(v, end=' ')
        v += 2
    
    v = 1
    while v <= n:
        print(v, end=' ')
        v += 2

a()

# 5
def a():
    n = int(input())

    if n in {2, 3}: 
        print('NO SOLUTION')
        exit()

    v = 2
    while v <= n:
        print(v, end=' ')
        v += 2
    
    v = 1
    while v <= n:
        print(v, end=' ')
        v += 2

a()
{% endhighlight %}


# Approach #6 and their friends
In this approach instead of `print`ing out the value each time we are storing it in an array and `print` it after all the loop ends.

This gave a very significant performance boost.


## Observations
This approach ran at `~0.34 s`. This is a major performance enhancement. Converting the duplicated `while` into a function made the runtime as `~0.36 s`, a little slow. 


## Code
[Link #6][link-6]
[Link #6-1][link-6-1]
[Link #6-2][link-6-2]

{% highlight python %}
# 6
def a():
    n = int(input())

    if n == 1:
        print('1')
    if n in {2, 3}: 
        print('NO SOLUTION')
    else:
        r = []
        v = 2
        while v <= n:
            r.append(v)
            v += 2
    
        v = 1
        while v <= n:
            r.append(v)
            v += 2

        print(' '.join(map(str, r)))

a()

# 6-with-str-called-at-append
def a():
    n = int(input())

    if n == 1:
        print('1')
    if n in {2, 3}: 
        print('NO SOLUTION')
    else:
        r = []
        v = 2
        while v <= n:
            r.append(str(v))
            v += 2
    
        v = 1
        while v <= n:
            r.append(str(v))
            v += 2

        print(' '.join(r))

a()

# 6-with-function-little-slow
def a():
    n = int(input())

    if n == 1:
        print('1')
    if n in {2, 3}: 
        print('NO SOLUTION')
    else:
        r = []
        
        def b(v):
            while v <= n:
                r.append(v)
                v += 2
    
            while v <= n:
                r.append(v)
                v += 2
        
        b(2)
        b(1)
        print(' '.join(map(str, r)))

a()
{% endhighlight %}


# Conclusion
In conclusion from this problem,
* I've learned the main performance optimization with local variables
* I've experimented with possibilities for the `in` operation data structures
* I've found multiple calls to `print` also affects performance


[cses-profile]: https://cses.fi/user/182121
[problem-link]: https://cses.fi/problemset/task/1070
[cses-site]: https://cses.fi/problemset/
[github-permutations]: https://github.com/Maheshkumar-novice/CSES/blob/main/Permutations
[idea-link]: https://japlslounge.com/posts/cses/permutations/1.htm
[so-gv-vs-lv]: https://stackoverflow.com/questions/12590058/performance-with-global-variables-vs-local
[link-1]: https://github.com/Maheshkumar-novice/CSES/blob/main/Permutations/1.py
[link-2]: https://github.com/Maheshkumar-novice/CSES/blob/main/Permutations/2.py
[link-3]: https://github.com/Maheshkumar-novice/CSES/blob/main/Permutations/3.py
[link-4]: https://github.com/Maheshkumar-novice/CSES/blob/main/Permutations/4.py
[link-5]: https://github.com/Maheshkumar-novice/CSES/blob/main/Permutations/5.py
[link-6]: https://github.com/Maheshkumar-novice/CSES/blob/main/Permutations/6.py
[link-6-1]: https://github.com/Maheshkumar-novice/CSES/blob/main/Permutations/6-with-function-little-slow.py
[link-6-2]: https://github.com/Maheshkumar-novice/CSES/blob/main/Permutations/6-with-str-called-at-append.py