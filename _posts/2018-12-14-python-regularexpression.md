---
layout: post
title:  "Examples for Regular Expression of Python"
date:   2018-12-14
excerpt: "Regex Tips and memo for Python Programming"
tag:
- Python 
- regex
comments: true
--- 

## Example for Phone Number Regex

{% highlight python %}
import pyperclip, re

phoneRegex = re.compile(r'''(
    (\d{3}|\(\d{3}\))?              # area code
    (\s|-|\.)?                      # separator
    (\d{3})                         # first 3 digits
    (\s|-|\.)                       # separator
    (\d{4})                         # last 4 digits
    (\s*(ext|x|ext.)\s*(\d{2,5}))?  # extension
    )''', re.VERBOSE)
{% endhighlight %}
