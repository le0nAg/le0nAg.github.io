---
layout      :   post
title       :   "Binary search and SQLi"
date        :   2025-05-05 23:09:22 +0100
categories  :   scripting
---

# The importance of writing your own exploit: <br />Binary search and SQLi

# ! [writing in progress]

## Introduction

Like many others, I started my journey in web application hacking through CTFs (and I seriously recommend them, it's a valuable and fun way to learn!). After googling some details of CTFs that I was unable to solve, as happens when you don't know something, I found myself reading write-ups of the solutions.

In most cases, you will find a brief explanation of the vulnerability and a very specific one of the exploitation process used to get the flag. Also, in most cases, these writeups will mention pre-built tools that do almost all of the work for you, giving you the flag or a pre-built exploit.

Using them is like watching a magician pulling a rabbit from his hat, but I think that the really cool part of hacking is the curiosity, the willingness to understand the magic behind that trick. With this article, and generally with this blog, I want to try to go deeper into the white rabbit hole.

Today we are going to explore two important concepts of computer science and cybersecurity: time complexity and SQL injections.

## What is time complexity? - a brief introduction

We can say that **time complexity** is *"a way to measure the efficiency of an algorithm over time"*.

What does that mean? Let's take a classical search problem as an example to explain the concept:
> Given an ordered set of data D and an element E, find its position. If that element is not in the given set, answer with "-1".
>
> Examples:
>
> - Given the set D = {1, 5, 8, 15} and the element E = 8, the expected output is 3 (or 2 if the set is 0-indexed)
> - Given the set D = {"a", "d", "l", "z"} and the element E = "b", the expected output is -1 since it's not an element in the set.

The most basic approach to solve the problem can be done by looking at each element of the set:

```text
int search(D, E):
    for (int i = 1; i < length(D); i++):
        if(D[i] == E):
            return i
    return -1
```

<br/>
![simulation of linear search](/assets/images/linear_search.gif){:width="500px"}
<br/>

Another approach is the so-called binary search.
The idea is to repeatedly divide the search interval in half:

1. Start with the middle element of the array D.
2. If it matches the target E, return the index.
3. If E is smaller, repeat the search on the left half.
4. If E is larger, repeat on the right half.
5. If the range becomes empty, the element is not found.

<br/>

```text
int binarySearch(D, E):
    low = 0
    high = length(D) - 1

    while low <= high:
        mid = (low + high) // 2

        if D[mid] == E:
            return mid
        else if D[mid] < E:
            low = mid + 1
        else:
            high = mid - 1

    return -1
```

<br/>
![simulation of binary search](/assets/images/bin_search.gif){:width="500px"}
<br/>

Now: we have two different algorithms to solve the same problem, which one should we choose?
Generally speaking, the answer to this question depends on the choice criteria. In this case, we want the most efficient one, or in other words, the algorithm that gives us the correct output as quickly as possible.

Going back to our example, we can count how many comparisons (steps) we need to find the target number E.

```text
Example 1:
D = {1, 5, 8, 15}
E = 8

|   Algorithm       | # steps   |
|-------------------+-----------|
|   Linear search   |    3      |
|   Binary search   |    2      |

Example 2:
D = {1, 3, 7, 9, 10, 11, 13, 15, 16, 17, 18, 20, 21, 22, 24, 25, 26, 28, 30}
E = 24

|   Algorithm       | # steps   |
|-------------------+-----------|
|   Linear search   |    15     |
|   Binary search   |    5      |

```

As we can see, binary search is much faster than linear search.
We formalize this concept through the *asymptotic notation*, in particular through the *Big-O notation*.

The Big-O notation describes the upper bound of an algorithm's time complexity in the worst-case scenario. It helps us understand how the algorithm's performance scales with increasing input size.

For our examples:

- **Linear search**: O(n) - The time complexity grows linearly with the input size. In the worst case, we might need to check every element.
- **Binary search**: O(log n) - The time complexity grows logarithmically with the input size. With each step, we eliminate half of the remaining elements.

This logarithmic growth is what makes binary search significantly more efficient for large datasets. Consider a dataset with 1,000,000 elements:

- Linear search might require up to 1,000,000 comparisons in the worst case
- Binary search would require at most 20 comparisons (log₂ 1,000,000 ≈ 20)

![Alt: a graph comparing the growth of linear vs logarithmic](/assets/images/bin_lin_graph.png){:width="500px"}

This concept of algorithmic efficiency is crucial not only for general programming but also for security-related tasks, cryptography, for example, relies on this concept and we'll discuss later how to use it to optimize our exploitation process.

## What is a SQLi? - Types, exploits & mitigations

SQL injection are a particular subset of code injection vulnerabilities and like all code injection vulnerabilities it arises when the input of a program is not well sanitized and happens that it is intepreted as (injected) code.

SQL injection are common in web applications and allow an attacker to execute SQL code 

## Blind SQLi: two approaches burpsuite vs scripting

We are going to use for this part [this lab](https://portswigger.net/web-security/sql-injection/blind/lab-conditional-responses).

### Burpsuite usage

The first and most common approach to this challenge we can take is using the

### Scripting a solution

## Bonus tip: parallel code runs faster
