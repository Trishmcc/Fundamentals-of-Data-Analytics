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
3. Run Jupyter Lab - it runs on my local machine and is an ipynb file. Also, Jupyter lab is a web application so opens in my web browser. 

## Overview of the matplotlib.pyplot Python package 

This overview is located in pyplot.ipynb

## How to load CAO points information from the CAO website into a pandas data frame

To begin this project, Google was used to locate data for 2019, 2020 and 2021 CAO points.  After the data was examined it was identified that there are 4 columns, 4 titles and each row has a course code. Chrome recognises html tags and Python interacts with any html webpage (as all webpages are text files) by giving it the url.  HTML is semi-structured which means that you can write incorrect code and it will still run, so when Python reads the url it will have errors, therefore a python html parser is required. The easiest way to get the HTML webpage into a python data structure is to use a pandas dataframe.

The package called requests (rq) which is convenient for making http requests comes with the Anaconda package. Its method is resp = rq.get(url) which allows you to get the content from the url.

iter_lines() is used to loop through the lines of the file [1] line is the name of the variable in the loop. It will loop or iterate through each line and if the regular expression matches the whole line, then it will add 1 line to to the number of lines that have matched. It picks out the relevant parts of the line , for example, substitute and return the first group\1 followed by the second group \2 and so forth. This will be demonstrated in the project.

```for line in resp.iter_lines():```

# 2021 Data

## Backing up data 

To back up data, a file path needs to be created for the csv file. 

```path2021 = 'data/cao2021_csv_' + nowstr + '.csv'```

The server was using the wrong encoding. To fix this problem a variable was created called original coding, and the reponse encoding was changed from ISO-8859-1 to cp1252

Reponse.text means that the request library takes the response and turns it into some text using the coding standard that was used. The encoding is correct, however, the server is sending back the wrong encoding. The request library tries to figure out the encoding from the data that it receives, ie resp._encoding which is ISO-8859-1  so this needs to be changed. 

```original_encoding = resp.encoding```
```resp.encoding = 'cp1252'```

The orignal html file was saved by opening the path with write permissions

```with open(pathhtml, 'w') as f:```
    ```f.write(resp.text)```

A file path needed to be created for the original data. The module datetime from the standard library is imported to efficently work with dates and times in Python. It also gives a different file name each time so it wont overwrite the original file.[8] strftime() turns dates into strings. The output was saved in a variable called now. The order of year/month/day is used in this format, because the alphabetical order in Windows Explorer/Data corresponds to datetime order.

## Regular Expression

A regular expression is needed to define a pattern in the text. Regular Expression comes as default with Python. A regular expression is a sequence of characters that specifies a search pattern.[2]
To match a whole line (whole string not partial string) with a regular expression use[3]

```if re.fullmatch()``` 

Each time a regular expression is run, it calls everything and recomplies to do the matching everytime. To be more efficient, the regular expression should be compiled so it doesnt have to keep re-running so re_course is used to reuse the precomplield regular expression.[4] The complier builds a function in memory that recognises this pattern. The brackets mean to group something together. The r means raw and is placed in front of the string so no characters like back slashes, for example \n will be evaluated by Python. [5] It will read it as a character not a command. 

```re_course = re.compile(r'([A-Z]{2}[0-9]{3})(.*)')```

[A-Z] This locates any capital letter from A-Z. To match two uppercase letters {2} is used after it. Curly braces are quanitifiers. To match any digits use [0-9] If three numbers are to be matched in the range of 0-9 use {3} The space bar is used to create two blank spaces as no quantifier is needed for this. The .* was used to match any character any number of times and spaces too until the next number. A space and a wild card at the end matches zero or more spaces at the end.

```if re_course.fullmatch('[A-Z]{2}[0-9]{3} .*[0-9]{3} *, line.decode('utf-8'))```

An error occurs: Cannot use a string pattern on a bytes like object. To fix this decode('utf-8') [6] 

Some lines match the regular expression and others do not as some have extra characters like * etc
To include the * a backslash needed to be included \* To include everything without an asterix an \* needed to be put in brackets (the \ says not to treat the * as a quantifier) followed by ? (this is the quantifier 0 or 1 of)

```(\*)?```

## Accents: Unicode and special characters
To get rid of Fadas, the Unicode behind the scence needed to be decoded. 

ASCII was before unicode. It only used 7 bits out of 1 byte (8 bits) There are no accents in the ASCII table.

Unicode uses 8 bits, ie 1 byte

An example:

q        Q         113         01110001 

The number behind the character, in this example 113, is called the code point. Binary representation of the code point 113 is 01110001 [7]

UTF 8 represents code points/integers in binary. Hexamals turn into binary easily.

## Dashes

\x96 is not being decoded correctly as an unside down question mark appears. Although the web browser states that the character encoding is ISO - 8859-1, it is not working correctly as an unside down question mark appears in places. Windows created cp1252 code points which have extra code points 128-159 (0x80-0x9F)[8] This fixes the problem. Its now in a unicode byte. 

## Errors fixed

It will loop through iterate lines and decode the line and if it matches the lines from the html, the csv version will be returned.

Two regular expression are used in the if statement. The one below is used to replace the lines with a csv format. The dline.split function is used to split spaces between different fields/columns.[10] When spliting on two or more spaces a 'space' and a '+' are used. The blank lines in between each row wont be effected as it doesnt match the original regular expression. Each part of the row becomes its own mini string. Lots of groupings from the original regular expression are not required, eg ([A-Z]{2}[0-9]{3}) It is not a good idea to split on one space as there is one space allocated between each word, for example a title 'Computer Science'. You can rejoin the mini strings so that they only have a comma between them.

```linesplit = [course_code, course_title, course_points[0], course_points[1]]```
           
```f.write(','.join(linesplit) + '\n')```

# 2020 Data

## Data Cleaning

The 2020 data was in the format of an Excel document. The Excel file was downloaded and the Excel spreadsheet parsed. In the function signature there is an option skiprows=10. This parameter can be passed in to skip the first 10 rows of the Excel spredsheet.[11] All the column names in the dataframe are now visbile.

``` df2020 = pd.read_excel(url2020, skiprows=10) ```

Like for the 2021 data the orignal html file was saved by opening the path with write permissions

```with open(pathhtml, 'w') as f:``
    f.write(resp.text)```

A spot check was completed to make sure the data has been read in correctly.

Code was used to check the data is correct in a particular row. Indexes need to be considered, ie, Excel row 1 is indexed 1 whereas Python row 1 is indexed as 0.[12] iloc retrieves the row locations. So row 753 and the last row of the dataframe are checked to confirm data has read correctly.

``` df2020.iloc[753] ```

```df2020.iloc[-1]```

From the 2021 file, the string for the path was copied. The now time date doesnt need to be regenerated (Before the date time package was used, to get the current date and time, they were formatted as a string before using strftime to create a path for the 2021 data ) The original data was saved [13]

``` pathxlsx = 'data/cao2020_' + nowstr + '.xlsx' ```
``` urlrq.urlretrieve(url2020, pathxlsx) ```

Its good practice to save original file to a data folder. A timestamp was created so each time it is ran, it redownloads the CAO page and its backed up everytime so there is a  history of all the analysis performed.

## 2019 ??? PDF DATA  

2021 and 2020 data was downloaded from the original web server and were parsed through Python, ie to extract data and to get it into a reasonable format, which helped with reproducibility, that is to back up the original data. However,PDFs can be difficult to work with so tabula was imported to help scrap the data from the pdf using Python.[14] Java is required and the path for tabula needs to be set too. The pdf needs to be read into a list in the DataFrame, dfs = tabula.read_pdf(path, pages='all')[15]

```tabula.convert_into("http://www2.cao.ie/points/lvl8_19.pdf", path, output_format="csv", pages='all')```

The pdf was opened in a word document and saved as a .docx and regular expressions were used to extract the data. 

Each course code was mapped to each college name. CK is for example Cork

isnull was used to check course code to see if its empty, empty means that this cell belongs to the college name [16]

astype was used to make them strings [17]

the zip joins the two dataframes [18]

notna allowed to delete empty cells only and include everything else [19]


```college_names = pd.DataFrame(df2019[df2019['Course Code'].isnull()]['INSTITUTION and COURSE'].astype(str))```
```course_codes = pd.DataFrame(df2019['Course Code'].astype(str).str[:2].unique().astype(str)[1:])```
```coursecode_and_names = list(zip(course_codes.values, college_names.values))```
```df2019 = df2019[df2019['Course Code'].notna()]```
```coursecode_and_names```

Now, that all CAO points have been imported and the data cleaned for each year, the columns were renamed for all three years. The data is then joined using the concat function in order to get meaning from the data. All duplicated data is removed too.

## Querying Data

Courses that increased in points every year were then identified. Courses that decreased in points were also identified. Seven main colleges were seperated from the 'all courses' dataframe. A histogram graph was drawn for courses in Cork. It identified the required CAO points for courses in Cork. The maximum points required in each college are identified. Minimum points required in each college are then identified. A function was created that takes any course title and a given amount of points,to return courses that a student can select based on their exact points that they received.

# Conclusion

In conclusion, insights and analyses are only as good as the data someone works with. In other words, if a person works with rubbish data, you cannnot expect that person to perform excellant analysis. A crucial tasks for any organization is to build a culture around data cleaning, and quality data decision-making.

In the above exercise, CAO data for the years 2019, 2020 and 2021 were used to demonstrate how to load different types of data formats into a pandas dataframe. Data was in the form of urls, Excel documents and pdfs. Once the data was successfully loaded into the dataframe it then needed to be examined and cleaned before analysis could begin. Loading and cleaning the data proved quite challenging. However, data cleansing is imperative if correct analysis is to be performed and meaningful graphs produced. 

## Troubleshooting:

1. Cannot use a string pattern on a bytes like object. To fix this use decode('utf-8') [6] 

2. Accents - To get rid of Fadas, you need to decode the unicode behind the scence. To do this, go to CAO website/rigt click/inspect/Network/Refresh. You      should see 2 http requests. Click on l8.php/Headers/Response headers/char set is iso-8859-1

3. To fix the dash problem - Windows created cp1252 code points which have extra code points 128-159 (0x80-0x9F)[8]  iso-8859-1 dont have these code points to decode the byte -.

## References

1.  https://stackoverflow.com/questions/16870648/python-read-website-data-line-by-line-when-available
2.  https://en.wikipedia.org/wiki/Regular_expression
3.  https://docs.python.org/3/library/re.html?highlight=match#re.match
4.  https://docs.python.org/3/library/re.html
5.  https://stackoverflow.com/questions/33729045/what-does-an-r-represent-before-a-string-in-python
6.  https://stackoverflow.com/questions/31019854/typeerror-cant-use-a-string-pattern-on-a-bytes-like-object-in-re-findall
7.  https://www.intel.com/content/dam/www/program/education/us/en/documents/the-journery-inside/digital/tji-digital-info-handout4.pdf
8.  http://www.i18nqa.com/debug/table-iso8859-1-vs-windows-1252.html
9.  https://docs.python.org/3/library/datetime.html
10. https://docs.python.org/3/library/re.html?highlight=split#re.split
11. https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_excel.html
12. https://www.marsja.se/how-to-use-iloc-and-loc-for-indexing-and-slicing-pandas-dataframes/
13. https://stackoverflow.com/questions/19602931/basic-http-file-downloading-and-saving-to-disk-in-python
14. https://pypi.org/project/tabula-py/
15. https://tabula-py.readthedocs.io/en/latest/tabula.html
16. https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.isnull.html
17. https://pandas.pydata.org/docs/reference/api/pandas.Series.astype.html
18. https://stackoverflow.com/questions/33282119/pandas-filter-dataframe-by-another-dataframe-by-row-elements/33282617
19. https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.notna.html
20. https://pandas.pydata.org/about/index.html