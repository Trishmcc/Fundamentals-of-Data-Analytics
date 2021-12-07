# Overview of the matplotlib.pyplot Python package and an overview of how to load CAO points information from the CAO website into a pandas data frame. 

***

This respository contains two Jupyter notebooks.

1.Overview of the matplotlib.pyplot Python package
2.Overview of how to load CAO points information from the CAO website into a pandas data frame. 

## You can view the two Jupyter notebooks in static form at the follwing links:

[![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg)]

1. https://github.com/Trishmcc/Fundamentals-of-Data-Analytics/blob/main/pyplot.ipynb
2. https://github.com/Trishmcc/Fundamentals-of-Data-Analytics/blob/main/cao.ipynb

## You can view the two Jupyter notebooks in dynamic form at the follwing links:

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/Trishmcc/Fundamentals-of-Data-Analytics/HEAD)

## How to run the notebook

1. Download [Anaconda](https://www.anaconda.com/)
2. Download [cmder](https://cmder.net/) if using Windows.
3. Run Jupyter Lab

## Overview of the matplotlib.pyplot Python package 

This overview is located in pyplot.ipyn

## How to load CAO points information from the CAO website into a pandas data frame

requests - import requests as rq
regular expression - import re

To begin this project, I needed to Google CAO points to view a webpage to view all the CAO points. Examine the data. There are 4 columns and 4 titles and each row has a course code. Python interacts with any html webpage by givng it the url. All webpages are text files. Chrome recognises html tags. HTML is semi-structured which means that you can write incorrect code and it will still run, so when Python reads the url it will have errors, therefore a python html parser is required. The easiest way to get the HTML webpage into a python data structure is to use a pandas dataframe!!!!!!!!!!!!!!???????????

The package called requests (rq) which is convenient for making http requests comes with the Anaconda package. Its method is resp = rq.get(url) which allows you to get the url from the browser. Is this a parser!!!!!!!??????????
iter_lines() is used to loop through the lines of the file [1] line is the name of the variable in the loop. It will loop or iterate through each line and if the regular expression matches the whole line, then a dd 1 line to to the number of lines we have matched. Pick out the relevant parts of the line , for example, substitute and return the first group\1 followed by the second group \2 and so forth.

for line in resp.iter_lines():

# Backing up data (pg 22)

1. Save the orignal html file
with open(pathhtml, 'w') as f:
    f.write(resp.text)

Create a file path for the original data. The module datetime from the standard library is imported to allow you to efficently work with dates and times in Python. It also gives a different file name each time so it wont overwrite the original file. strftime() turns dates into strings. Save the output in a variablle called now. The order of year/month/day is used as in Windows Explorer/Data alphabetical order corresponds to datetime order.

2. Create a file path for the csv file
path2021 = 'data/cao2021_csv_' + nowstr + '.csv'

# Regular Expression

A regular expression is needed to define a pattern in the text. Regular Expression comes as default with Python. A regular expression is a sequence of characters that specifies a search pattern.[2]
To match a whole line (whole string not partial string) with a regular expression use[3]

if re.fullmatch(). 

Each time you run this regular expression it calls everything and recomplies to do the matching everytime. To be more efficient, the regular expression should be compiled so it doesnt have to keep re-running so re_course is used to reuse the precomplield regular expression.[4] The complier builds a function in memeory that recognises this pattern. The brackets mean to group something together. The r means raw and is placed in front of the string so no characters like back slashes, for example \n will be evaluated by Python. [5] It will read it as a character not a command. 

re_course = re.compile(r'([A-Z]{2}[0-9]{3})(.*)')


[A-Z] This allows you to find any capital letter from A-Z. If you want to match two uppercase letters use {2} after it. Curly braces are quanitifiers. To match any digits use [0-9] If you want to match 3 numbers in the range of 0-9 use {3} Use the space bar to create two blank spaces as no quantifier is needed for this. The .* was used to match any character any number of times and spaces too until the next number. A space and a  wild card at the end matches zero or more spaces at the end.

if re_course.fullmatch('[A-Z]{2}[0-9]{3} .*[0-9]{3} *, line.decode('utf-8'))

An error occurs: Cannot use a string pattern on a bytes like object. To fix this decode('utf-8') [6] 

Some lines match the regular expression and others do not as some have extra characters like * etc
To include the * you need to include \* and to include everythin without an asterix put \* in brackets (the \ says not to treat the * as a quantifier) followed by ? (this is the quantifier 0 or 1 of)

(\*)?

# Accents: Unicode and special characters
To get rid of Fadas, you need to decode the unicode behind the scence. 

ASCII was before unicode. It only used 7 bits out of 1 byte (8 bits) There are no accents in the ASCII table.

Unicode uses 8 bits, ie 1 byte

An example:

q        Q         113         01110001 

The number behind the character, in this example 113, is called the code point. Binary representation of the code point 113 is 01110001 [7]

UTF 8 represents code points/integers in binary

Hexamals turn into binary easily.

# Dashes

\x96 is not being decoded correctly as an unside down question mark appears. Although the web browser states that the character encoding is ISO - 8859-1, it is not working correctly as an unside down question mark appears in places. Windows created cp1252 code points which have extra code points 128-159 (0x80-0x9F)[8] This fixes the problem. Its now in a unicode byte. 






# Troubleshooting:

1. Cannot use a string pattern on a bytes like object. To fix this use decode('utf-8') [6] 

2. Accents - To get rid of Fadas, you need to decode the unicode behind the scence. To do this, go to CAO website/rigt click/inspect/Network/Refresh. You      should see 2 http requests. Click on l8.php/Headers/Response headers/char set is iso-8859-1

3. To fix the dash problem - Windows created cp1252 code points which have extra code points 128-159 (0x80-0x9F)[8]  iso-8859-1 dont have these code points to decode the byte -.







The CAO Points are downloaded as a csv format.

The Pandas Library is imported.

The Pandas python toolkik is an open source library that is used for data analysis. It makes working with CSV files simplier and more effective as it can read and write data from different formats, i.e CSV (Common Seperated Values) which is the format of the CAO Points used for this analysis [?] The dataframe object is also useful for groupimg which will be demonstated in this project.
print(df) clarifies that df is the object name that contains the csv file

Data Screening

To confirm the file has been read correctly and identify information about the data:
print(df) To confirm that the csv file has been read correctly. It also identifies all column names and the number of rows and columns

Code
import pandas as pd

df2021 = pd.read_csv(path2021, encoding='cp1252')

print(df2021)


Troubeshooting:

## Detailed comparison of CAO points in 2019, 2020 and 2021 using the functionality in Python

## Three plots used from matplotlib.pyplot are 1.   2.   3.

## Other visualisations used are 



## References

1. https://stackoverflow.com/questions/16870648/python-read-website-data-line-by-line-when-available
2. https://en.wikipedia.org/wiki/Regular_expression
3. https://docs.python.org/3/library/re.html?highlight=match#re.match
4. https://docs.python.org/3/library/re.html
5. https://stackoverflow.com/questions/33729045/what-does-an-r-represent-before-a-string-in-python
6. https://stackoverflow.com/questions/31019854/typeerror-cant-use-a-string-pattern-on-a-bytes-like-object-in-re-findall
7. https://www.intel.com/content/dam/www/program/education/us/en/documents/the-journery-inside/digital/tji-digital-info-handout4.pdf

http://www.i18nqa.com/debug/table-iso8859-1-vs-windows-1252.html


https://pandas.pydata.org/about/index.html