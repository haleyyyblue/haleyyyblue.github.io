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
정규표현식 공부를 미루다가 이번기회에 익혀보려고 정리해보았다. 
출처는 _파이썬 프로그래밍으로 지루한 작업 자동화하기_
# Useful Methods
## re.compile()
Regular expression uses lots of back slash(\). 
However, Python uses back slash(\) as **escape** which means we need one more back slash for back slash for regex (i.e \\d). we can simply write regex by using re.compile()
{% highlight python %}
phoneNumRegex = re.compile(r'(\d\d\d)-\d\d\d-\d\d\d\d')
{% endhighlight %}
### re.DOTALL
Make dot(.) can match all characters including **newline**
{% highlight python %}
newlineRegex = re.compile('.*', re.DOTALL)
{% endhighlight %}

### re.I (re.IGNORECASE)
Ignore upper/lowercase
{% highlight python %}
robocop = re.compile(r'robocop', re.I)
{% endhighlight %}

### re.VERBOSE
Ignore space and comment-out
{% highlight python %}
phoneRegex = re.compile(r'''(
    (\d{3}\(\d{3}\))?     # area code
    (\s|-|\.)?            # separator
    )''', re.VERBOSE)
{% endhighlight %}

### Combination
{% highlight python %}
someRegexValue = re.compile('foo', re.IGNORECASE | re.DOTALL | re.VERBOSE )
{% endhighlight %}

## search()
Return only first matched pattern.
1. If no pattern matches a regex, search() method will return None
2. If yes, search() method will return Match object.
{% highlight python %}
mo = phoneNumRegex.search('My number is 123-456-7777.')
print(mo.group(0))
123-456-7777
print(mo.group(1))
123
{% endhighlight %}

## findall()
Return all matched pattern.
{% highlight python %}
phoneNumRegex = re.compile(r'(\d\d\d)-(\d\d\d-\d\d\d\d)')
phoneNumRegex.findall('Cell: 123-456-7777 Work: 050-555-5555')
[('123', '456-7777'), ('050', '555-5555')]
{% endhighlight %}

## sub()
Substitute
{% highlight python %}
agentNamesRegex = re.compile(r'Agent (\w)\w*')
agentNamesRegex.sub(r'\1****', 'Agent Alice told Agent Carol that Agent Eve knew Agent Bob was a double agent.')
'A**** told C**** that E**** knew B**** was a double agent.'
{% endhighlight %}

# Useful Regex
## Pipe(|)
or
{% highlight python %}
batRegex = re.compile(r'Bat(man|mobile)')
mo = batRegex.search('Batmobile lost a wheel')
mo.group()
'Batmobile'
{% endhighlight %}

## Question(?)
Optional
{% highlight python %}
batRegex = re.compile(r'Bat(wo)?man')
mo = batRegex.search('The Adventures of Batman')
mo.group()
'Batman'
{% endhighlight %}

## Asterisk(*)
Repeat pattern 0~?
{% highlight python %}
batRegex = re.compile(r.'Bat(wo)*man')
mo = batRegex.search('The Adventures of Batman')
mo.group()
'Batman'
mo = batRegex.search('The Adventrues of Batwowoman')
mo.group()
'Batwowoman'
{% endhighlight %}

## Plus(+)
Repeat pattern 1~?
{% highlight python %}
batRegex = re.compile(r.'Bat(wo)+man')
mo = batRegex.search('The Adventures of Batman')
mo == None
True
{% endhighlight %}

## Character Class
{% highlight python %}
# 0-9
\d
# All except 0-9
\D
# a-z A-Z 0-9 _
\w
# All except a-z A-Z 0-9 _
\W
# space, tab, newline
\s
# All except space, tab, newline
\S
{% endhighlight %}

## Carrot(^) and Dollar($)
Start(^)
End($)

## Wildcard (.)
one character except newline

## greedy and Non greedy(?)
nongreedy(?) will return minimum
greedy will return maximum

# Sample Program
><Practical Programming for total beginners>
[Link](http://www.inventwithpython.com/)

## import necessary module
{% highlight python %}
import pyperclip, re
{% endhighlight %}

## Example for Phone Number Regex

{% highlight python %}
phoneRegex = re.compile(r'''(
    (\d{3}|\(\d{3}\))?              # area code
    (\s|-|\.)?                      # separator
    (\d{3})                         # first 3 digits
    (\s|-|\.)                       # separator
    (\d{4})                         # last 4 digits
    (\s*(ext|x|ext.)\s*(\d{2,5}))?  # extension
    )''', re.VERBOSE)
{% endhighlight %}

## Example for email Number Regex
{% highlight python %}
emailRegex = re.compile(r'''(
    [a-zA-Z0-9._%+-]+       # username
    @                       # @ symbol
    [a-zA-Z0-9.-]+          # domain name
    (\.[a-zA-Z]{2,4})       # dot-something
    )''', re.VERBOSE)
{% endhighlight %}

## Find matches in clipboard text.
{% highlight python %}
text = str(pyperclip.paste())
matches = []
for groups in phoneRegex.findall(text):
    phoneNum = '-'.join([groups[1], groups[3], groups[5]])
    if groups[8] != '':
        phoneNum += ' x' + groups[8]
    matches.append(phoneNum)
for groups in emailRegex.findall(text):
    matches.append(groups[0])
{% endhighlight %}

## Copy results to the clipboard.
{% highlight python %}
if len(matches) > 0:
    pyperclip.copy('\n'.join(matches))
    print('Copied to clipboard:')
    print('\n'.join(matches))
else:
    print('No phone numbers or email addresses found.')
{% endhighlight %}

# Useful Website
[Regex Test](http://regexpal.com/)
[Official python doc](http://docs.python.org/3/library/re.html)
[Regex Tutorial](http://www.regular-expressions.info/)
