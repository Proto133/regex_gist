# REGEX Ellaboration

Regular Expressions (REGEX) are one of the most powerful tools available to web developers for parsing text. They can be used to extract, search and replace data from a large number of sources - including files such as CSV, HTML or JSON.  This gist will use one ornate example to show you how they work so, hopefully, become a bit more familiar with REGEX and how they work.

## Summary

 As previously mentioned, Regular expressions are a powerful and are used in many different areas of development. This gist will explore how the REGEX pattern used to parse Illinois driver's license numbers is constructed, but will also provide information on the significance of the sequences and characters used to do this through disecting the pattern.

 For clarity here is a brief explanation of how an Illion License Number is constructed:

There are 4 parts to IL drivers license numbers. It is of the form SSSS-FFFY-YDDD.  

As an example we'll use:
```
John Q Public, a nice man born January 1, 1980

DL number: P142-4758-0001
```


**Part 1** - SSSS
: The first part (four characters) is a [Soundex Code](https://en.wikipedia.org/wiki/Soundex) significant of the License holder's surname. *"P142" in our example.*

**Part 2** - FFF
: The second part (four characters) is a numeric value derived from the License holder's first name and middle initial. *"475" in our example.*

**Part 3** - YY
: This is the easiest part to pick out. This is a two digit representation of the License holder's birth year. *"8-0" in our example.*

**Part 4** - DDD
: This final part is a numeric value derived from the License holder's month and date of birth + gender modifier. *"001" in our example.*

Resulting in **P142-4758-0001** representing John Q Public in the eyes of the IL DMV.<sup><a href="#[^1]">^1</a></sup> <sup><a href="#[^2]">^2</a></sup>


---
|**So knowing the aforementioned formula, we can create a REGEX pattern that matches a IL Driver's License like this:**|
|:-------------------------------------------------------------:|
|<br><code style="background-color:#B8B8B8;color:white;">/^[A-Z]{1}[0-9]{3}\-[0-9]{4}\-[0-9]{4}$/gm</code><br><br>|

---

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components

### Anchors

What are REGEX Anchors?  A regular expression anchor matches at the beginning (^) or end (\$) of a line when it appears as one of the first characters on its own line; or at the start and end of each line (either ^$).

In our example:
<p>
<code style="background-color:#B8B8B8;color:white;">
/<strong style="color:#FF803C;">^</strong>[A-Z]{1}[0-9]{3}\-[0-9]{4}\-[0-9]{4}<strong style="color:#FF803C;">$</strong>/gm
</code>
</p>

### Quantifiers

The most basic of all regular expression operators is the quantifier, which typically comes in five flavors: “*”, “+”, “?”, {n}, and {m}.

| Quantifier | Description |
|:---:|:---:|
|`x*`| 0 or more repetitions of *x*|
|`x+`| 1 or more repetitions of *x*|
|`x?`| 0 or 1 instances of *x*|
|`x{m}`| **exactly** *m* instances of *x*|
|`x{m+}`| **at least** m instances of *x*|
|`x{m,n}`| between *m* and *n* (inclusive) instances of *x*|
|   |
|Citation| [MIT.edu PDF](https://web.mit.edu/hackl/www/lab/turkshop/slides/regex-cheatsheet.pdf)|


<p>
In our example:
<code style="background-color:#B8B8B8;color:white;">
/^[A-Z]<strong style="color:#3CFFD7;">{</strong>1<strong style="color:#3CFFD7;">}</strong>[0-9]<strong style="color:#3CFFD7;">{</strong>3<strong style="color:#3CFFD7;">}</strong>\-[0-9]<strong style="color:#3CFFD7;">{</strong>4<strong style="color:#3CFFD7;">}</strong>\-[0-9]<strong style="color:#3CFFD7;">{</strong>4<strong style="color:#3CFFD7;">}</strong>$/gm
</code>
</p>

So in the above example, we are saying that we want the first character to be a capital letter, and subsequent 3 characters to be numbers, followed by a "-"

### Grouping Constructs
As regular expressions grow more complex, you may check multiple parts of a string to determine that different sections fullfil different requirements. To break these sections up, use grouping constructs like parentheses (). Each section within parentheses are known as 'subexpression'.
By placing a part of the regular expression inside round brackets or parentheses, you can group that part and apply quantifiers to it. This includes limits on alternation so only specific parts are affected by whatever change is being made in your regex.

<p> A very good example for articulating this point would be finding `anchor` tags in an HTML document. To do this you would define the regular expression pattern as follows:
<br>
<code style="background-color:#B8B8B8;color:white;">

/<strong style="color:#DFE900;">(</strong><a href=<strong style="color:#DFE900;">)</strong><strong style="color:#DFE900;">(</strong>["']<strong style="color:#DFE900;">)</strong><strong style="color:#DFE900;">(</strong>.*?<strong style="color:#DFE900;">)</strong><strong style="color:#DFE900;">(</strong>["']<strong style="color:#DFE900;">)</strong>>   <strong style="color:#DFE900;">(</strong>.*?<strong style="color:#DFE900;">)</strong><strong style="color:#DFE900;">(</strong><\/a><strong style="color:#DFE900;">)</strong>/gmi<br>
</code>

Would find both:

<code style="background-color:#B8B8B8;color:white;"> 

\<a href='trashparty.xyz'>TrashParty\</a>
</code>
<p>and</p>
<code style="background-color:#B8B8B8;color:white;"> 

\<a href="trashparty.xyz">Trash Party\</a>
</code>

<sub>Side Note:<i>[TrashParty](trashparty.xyz) may be the single greatest web application ever created. You should check it out.</i></sub>

### Bracket Expressions

Bracket expressions are powerful tools for searching and selecting data. They don't require a string to meet every requirement in the pattern, they just need one of them! This means that [a-z0-9_-] searches for alphanumeric characters or two special symbols included within brackets (hyphens (-) and underscores (_)).

By putting any portion of our regular expression into square brackets [ ] we're able to define what's called a character class - this defines anything with those letters- which can make finding things much easier!

<p>
Our example is riddled with grouping, but here's a simple example:
The regular expression
<code style="background-color:#B8B8B8;color:white;">/ch[eiou]ck/igm</code> would find the following: <br><code style="background-color:#B8B8B8;color:white;"> The <strong style="color:#FF000F">chick </strong> is rich, I mean <strong style="color:#FF000F">chock</strong> full of <strong style="color:#FF000F">checks</strong>.Sometimes she even <strong style="color:#FF000F">chucks</strong> them at cashiers to show them she is rich.</code>

Now, that sentence was garbage, but it made for a good example.
</p>

### Character Classes

A character class in a regex defines a set of characters, any one of which can occur in an input string to fulfill the match. The bracket expressions outlined previously--including positive and negative groups--are considered character classes. 

Some other common ones are:
|Syntax| Description|
|:---:|:---:|
| . | Wildcard (matches any character)|
| \d | Matches any digit 0123456789 |
| \w | Matches "word" (letters, digits, and _)  |
| \W | Matches non-word characters |
| \t | Matches tab characters|
| \r | Matches return |
| \S | Matches non-whitespace |
|   |    |
|Citation| [MIT.edu PDF](https://web.mit.edu/hackl/www/lab/turkshop/slides/regex-cheatsheet.pdf)|

So in our example:
<p>This ➡️ <code style="background-color:#B8B8B8;color:white;">/^[A-Z]{1}<strong style="color:#FF3CFF">[\d]</strong>{3}\-<strong style="color:#FF3CFF">[\d]</strong>{4}\-<strong style="color:#FF3CFF">[\d]</strong>{4}$/gm</code></p><p>
is the same as</p><p>This ➡️ <code style="background-color:#B8B8B8;color:white;">/^[A-Z]{1}<strong style="color:#FF3CFF">[0-9]</strong>{3}\-<strong style="color:#FF3CFF">[0-9]</strong>{4}\-<strong style="color:#FF3CFF">[0-9]</strong>{4}$/gm</code></p>


### The OR Operator

Using the OR operator "|" in a regex pattern, tells the engine to look for multiple sets of criteria, but not mix.
*i.e The pattern <code style="background-color:#B8B8B8;color:white;"> /^cat|dog/gmi</code> would find <code style="background-color:#B8B8B8;color:white;"><strong style="color:#585C81">cat</strong>s play with yarn, but <strong style="color:#585C81">dog</strong>s don't.</code>
as well as <p><code style="background-color:#B8B8B8;color:white;"><strong style="color:#585C81">CAT</strong>S are terrible, <strong style="color:#585C81">DOG</strong>S RULE!</code></p>

<i>. . . but I thought REGEX was case-sensitive. . . . .</i>

Which brings us to [Flags](#flags)



### Flags
To make regular expressions more versatile, there are six<sup><a href="#[^3]">^3</a></sup> optional flags that can be used. 
These include: 
* /i for case-insensitive matching 
* /m Multi-line Search
* /g which tells the engine to find all occurrences of pattern in one or many strings instead of stopping at first match only (/g stands for global)
* /s allows . to match newline characters
* /u tells the engine to treat a pattern sequence as a string of unicode characters
* /d generates indices for substring matches

In our example, we are assuming you're looking for all occurrences and on multiple lines.
<p>
<code style="background-color:#B8B8B8;color:white;">/^[A-Z]{1}[\d]{3}\-[\d]{4}\-[\d]{4}$<strong style="color:#F4FF00">/gm</strong></code>
</p>

### Character Escapes

A backslash in a regex can be used to escape characters that would otherwise have special meaning. For example, the open curly brace is usually used as an indication of quantifiers but adding a backslash before it (\{) means that you are looking for the character and not beginning another type of identifier such as "{"

A slash in your regular expression signifies ending one set or pattern and starting another. There's no need to use this when searching strings with certain symbols within them though, because there will always only ever be one instance per string which needs escaping - so using { instead leaves more room behind for other identifiers like "+" if needed: \{\+

In our example:
<p>
<code style="background-color:#B8B8B8;color:white;">
/^[A-Z]{1}[0-9]{3}<strong style="color:#75FF3C;">\</strong>-[0-9]{4}<strong style="color:#75FF3C;">\</strong>-[0-9]{4}$/gm
</code>
</p>


## Author

A web development student. Peter loves learning new things and developing them into useful other things for people. His strengths lie in JS & CSS and seeing the "big picture" of almost everything in life. As a student of the world, he is committed to <strong>always</strong> learning.

Please, feel free to contact [✉️ Peter via email](mailto:support@peterroto.com) or visit his[ :octocat Github profile](https://github.com/proto133).


---
### Footnotes

<sub id="[^1]">1. *For more in depth analysis of how driver's license numbers work* [click here](http://www.highprogrammer.com/alan/numbers/dl_us_shared.html)</sub>

<sub id="[^2]">2. *For a fun generator and analyzer* [click here](http://www.highprogrammer.com/cgi-bin/uniqueid/dl_il)</sub>

<sub id="[^3]"> 3. According to [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags) there is an additional "y" flag that indicates a "sticky" search. There site says that there are 6 REGEX flags, but then proceed to show you a chart with 7 rows</sub>
