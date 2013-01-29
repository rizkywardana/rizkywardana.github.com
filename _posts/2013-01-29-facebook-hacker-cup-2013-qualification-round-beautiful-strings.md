---
layout: post
title: Facebook Hacker Cup 2013 Qualification Round - Beautiful Strings Problem
comments: true
---

{{ page.title }}
================

<p class="meta">29 Jan 2013 - West Java</p>

The problem can be found [here](https://www.facebook.com/hackercup/problems.php?pid=475986555798659&round=185564241586420)

## TLDR

Given a string (case insensitive), find the **maximum** possible beauty of the string. A beauty of each letter
is an integer between 1 and 26. For example:

{% highlight text %}
# Input
ABbCcc

# Output
152
{% endhighlight %}

Since we're asked to find the **maximum** possible, I think the solution below would be
acceptable:

1. Order the letter by count. Since 'C' and 'c' is the same, worth to lowercase
   all letters.
2. The most common letters weighted to 26, the 2nd one to 25, etc.

In the example above it would be:
{% highlight text %}
letter  count  weight (count*weight)
c       3      26      78
b       2      25      50
a       1      24      24
                       152 (sum)
{% endhighlight %}

## Example Input and Output

The input file consists of a single integer m followed by m lines.

{% highlight text %}
5
ABbCcc
Good luck in the Facebook Hacker Cup this year!
Ignore punctuation, please :)
Sometimes test cases are hard to make up.
So I just go consult Professor Dalves
{% endhighlight %}

Your output should consist of, for each test case, a line containing the string
"Case #x: y" where x is the case number (with 1 being the first case in the input file,
2 being the second, etc.) and y is the maximum beauty for that test case.

{% highlight text %}
Case #1: 152
Case #2: 754
Case #3: 491
Case #4: 729
Case #5: 646
{% endhighlight %}

## Solution in Python

First, you need to know how to read input from stdin and output to stdout with
given format. You can read [my post](/2013/01/23/read-from-stdin-in-python-for-common-algorithm-challenges.html)
on how to read from stdin in Python. It should be something like:

{% highlight python %}
m = int(sys.stdin.readline())
for i in range(m):
	# Read each test case here
	# and output the result

	# Assuming y will hold the result

	print "Case: %d %d" % (i+1, y)
{% endhighlight %}

On each iteration, read a test case with `readline` and strip the heading and trailing whitespaces.
Also, make sure we later process only alphabet that already in lowercase.

{% highlight python %}
s = [l for l in sys.stdin.readline().strip().lower() if l in ALPHABET]
{% endhighlight %}

Now we need to get the **counts** on each letter. There's `collections` module with `Counter` class
to easily count hashtable objects. Here's an example what `Counter` can do:

{% highlight text %}
>>> from collections import Counter
>>> Counter('abbccc')
Counter({'c': 3, 'b': 2, 'a': 1})
>>> Counter(['a', 'b', 'b', 'c', 'c', 'c'])
Counter({'c': 3, 'b': 2, 'a': 1})
>>> Counter(['a', 'b', 'b', 'c', 'c', 'c']).most_common()
[('c', 3), ('b', 2), ('a', 1)]
{% endhighlight %}

See, how easily is that! Now we need to iterate the `Counter` object, which sorted already, and
count the *beauty* of each letter and sum it. Inside the `for` loop, the code would be:

{% highlight python %}
b, y = 26, 0
	s = [l for l in sys.stdin.readline().strip().lower() if l in ALPHABET]
	s = Counter(s).most_common()
	for k, v in s:
		y = y + v * b
		b = b - 1
{% endhighlight python %}

The `y` is the **maximum-beauty** to be printed. Now the final result of the code is looked like:

{% highlight python %}
#!/usr/bin/env python
 
import string
import sys
 
from collections import Counter
 
ALPHABET = string.ascii_lowercase
 
def main():
	m = int(sys.stdin.readline())
	for i in range(m):
		b, y = 26, 0
		s = [l for l in sys.stdin.readline().strip().lower() if l in ALPHABET]
		s = Counter(s).most_common()
		for k, v in s:
			y = y + v * b
			b = b - 1
 
		print "Case #%d: %d" % (i+1, y)
 
 
if __name__ == '__main__':
	main()
{% endhighlight %}

Now `chmod +x` the file above. Download the input file (or, you can use example input above).
And run it:

{% highlight text %}
$ cat input.txt | ./beautiful_strings.py
{% endhighlight %}

Hope it helps!
