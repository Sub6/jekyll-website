---
layout: post
title: Code highlight experiment
author: tung_thai
date: "2021-03-19 10:07:32"
intro_paragraph: ""
categories: misc coding
---

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

Some tests and examples to try syntax highlighting. In the following, you can see various languages used for testing.

{% highlight Python %}
#!/usr/bin/env python3
# -_- coding: utf-8 -_-

import math

a = 2.0
a = math.sqrt(a)

print(a)

{% endhighlight %}

Here is an example of a power function written in C++. To be honest, it is so old, I'm not sure what it does exactly. Not that I need this piece of code anyway.

{% highlight CPP %}
int powCalc(int putNum, float *ratio)
{
  int pow_num = 2;

  for (int i = 1; i < 11; i++)
  {
      if (pow_num >= putNum)
      {
          *ratio = ((float) putNum) / ((float) pow_num);
          return pow_num;
      }
      pow_num = pow_num << 1;
  }
  return -1;

}

{% endhighlight %}

Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborumo.
