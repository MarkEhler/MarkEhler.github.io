---
layout: post
title:      "RegEx For Text"
date:       2019-02-23 21:43:27 +0000
permalink:  regex_for_text
---


Regular Expression, or RegEx, is a sequence of charaters that form a search pattern.  
Regular Expression can be used to check if a string contains the specified search pattern.  RegEx is like a code or a group of symbols used to filter through strings and this brief tutorial will not even cover the depth of it, but it will serve as an introduction to the concept.

```
import re

txt = "The rain in Spain"
x = re.search("^The,*Spain$", txt)
```


functions
findall - returns a list containing all matches
search - returns a [match object] if there is a match in the string
split - returns a list where the string has been slipt at each match
sub - replaces one or many matches with a string


metacharaters
charaters with special meaning
[] - a set of charaters - "[a-m]"
\ - Signals a special sequence - "\d"
. - any charater (except newline charater) - "he..o"
^ - Starts with - "^hello"
$ - Ends with - "world$"
* - Zero or more occurrences "aix*"
+ - one or more occurencecs "aix+"
{} - Exactly the specified number of occurrences "al{2}"
| - Either or - "falls|stays"
() - Capture and group



I am given a blig block of text using bs4.soup.all_text() and want to filter for only the
part of the page I want.  For this, lets try a couple expressions using Regular Expression.

```
import re
import bs4 as soup

song = 'https://www.azlyrics.com/lyrics/marsvolta/eunuchprovocatuer.html'
song_page = requests.get(song)
soup = BeautifulSoup(song_page.content, 'html.parser'

all_txt = soup.get_text()
lyrics = []
soup = with open('') 
txt = soup.all_text()
for row in txt:
Counter = 0
if (row = re.search("^[Lyrics] "$):
for row == "/n" counter += 1
when counter <= 4
lyrics.append(row)
if counter <=8
break
```

^this is a mess of pseudocode that might loosely represent my thought process.

what is a match object?
a match object is an object containing information about the search and the result.

match objects have properties.
.span() - returns a tuple containing the start and end positions of a match
.string - returns the string passed into the function
.group() - returns the part of the string where there was a match
