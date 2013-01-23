---
layout: post
title: Read from stdin in Python for Common Algorithm Challenges
comments: true
---

{{ page.title }}
================

<p class="meta">23 Jan 2013 - West Java</p>

There are many sites dedicated to host algorithm challenges (e.g [UVa Online Judge](http://uva.onlinejudge.org/),
[Top Coder](http://www.topcoder.com/tc), [Sphere online judge (spoj)](http://www.spoj.com/)). I love spoj
because they offer various languages for solution submission. Previously, I learned
algorithm in UVa online judge using C++ (i.e. the most popular one in UVa beside
Java). Common problem encourage us to read from stdin and there two common
patterns:

- Read each line as a test case
- Read first line as N and N test cases follow.

For the first pattern, a typical problem might be looked like:

{% highlight text %}
# Input. Read each line as x, y and print the sum
3 2
7 8
10 15

# Expected output
5
15
25
{% endhighlight %}

And you can solve the problem in C++ using the following:

{% highlight cpp %}
#include <cstdio>

int main() {
	int x, y;

	while (scanf("%d", &x) != EOF) {
		scanf("%d", &y);
		printf("%d\n", x + y);
	}
}
{% endhighlight %}

So how we can do the above in Python? Well, fortunately we
can use `sys.stdin` like below:

{% highlight python %}
import sys

def main():
  for line in sys.stdin:
    x, y = line.split(' ')
    print int(x) + int(y)


if __name__ == '__main__':
  main()
{% endhighlight %}

I'm not sure whether this approach is the Pythonic-way. But worked
for me pretty well. Let me know in case if you have a better
approach.

For the last problem pattern, below is a typical one:

{% highlight text %}
# Input. Read first line as N, N lines follow
# for each test case. For example we still
# asked to sum two ints on each test case.
3
3 2
7 8
10 15

# Expected outupt
5
15
25

{% endhighlight %}

It's possible to use previous approach and checking the singular value
per line. Or, you can read the first line, which is N, once and loop until N.

{% highlight python %}
import sys

def main():
  n = int(sys.stdin.readline())

  for i in range(n):
    x, y = sys.stdin.readline().strip().split(' ')
    print int(x) + int(y)


if __name__ == '__main__':
  main()
{% endhighlight %}

Hope it helps!
