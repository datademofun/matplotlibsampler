# Matplotlib and Pandas Sampler


# Lessons

These short guides are meant to show you some practical examples of matplotlib and pandas, not serve as comprehensive walkthroughs.

- [Introduction to matplotlib visualizations](Introduction-to-matplotlib-visualizations.ipynb)
- [Basic matplotlib visualization of climate data](Basic-matplotlib-visualization-of-climate-data.ipynb)
- [Basic pandas data wrangling and matplotlib visualization of climate data](Basic-pandas-and-matplotlib-visualization-of-climate-data.ipynb)

# Further reading


### Matplotlib

- Nicolas P. Rougier [has an excellent and beautifully designed Matplotlib tutorial](http://www.labri.fr/perso/nrougier/teaching/matplotlib/matplotlib.html).
- [How to make beautiful data visualizations in Python with matplotlib](http://www.randalolson.com/2014/06/28/how-to-make-beautiful-data-visualizations-in-python-with-matplotlib/)
- [Matplotlib homepage](http://matplotlib.org/)
  + [Examples index](http://matplotlib.org/examples/index.html)

(Note: While the Matplotlib homepage is a place you eventually want to go to, some of the documentation may be more complicated for you than necessary...)


### Pandas

- [10 Minutes to pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html)
- [Pandas cookbook](http://pandas.pydata.org/pandas-docs/stable/cookbook.html#cookbook)
- [An Introduction to Pandas, via Michael Hansen](http://synesthesiam.com/posts/an-introduction-to-pandas.html)
- [12 Useful Pandas Techniques in Python for Data Manipulation](http://www.analyticsvidhya.com/blog/2016/01/12-pandas-techniques-python-data-manipulation/)
- [Things in Pandas I Wish I'd Known Earlier](http://nbviewer.jupyter.org/github/rasbt/python_reference/blob/master/tutorials/things_in_pandas.ipynb)


# Datasets

The [data](data) folder contains several datasets, extracted and somewhat normalized for your convenience:


#### Climate data

- [data/climate](data/climate)
  - Sources:
    + NASA-aggregated data on [global temperature and greenhouse gases](https://github.com/dannguyen/python-notebooks-data-wrangling/blob/master/Data-Extraction--NASA-Text.ipynb)
- [data/schools](data/schools)
  - Sources:
    + [2014 SAT scores for California schools](http://www.cde.ca.gov/ds/sp/ai/)
    + [2014 Free-and-reduced lunch (poverty) data](http://www.cde.ca.gov/ds/sd/sd/filessp.asp) for schools
- [data/stocks](data/stocks)
  - Source:
    - Daily closing prices for top tech stocks, via [Yahoo Finance](http://finance.yahoo.com).
- [data/congress](data/congress)
  - Sources:
    - [Legislator spreadsheet from Sunlight Foundation](https://sunlightlabs.github.io/congress/index.html#legislator-spreadsheet). 
    - [Twitter API and t-tool](https://github.com/sferik/t)



# Ad-hoc examples (to get their own notebook)


Typecasting dates during the pandas import:

~~~py
from os.path import join
import matplotlib.pyplot as plt 
import pandas as pd
fname = join('data', 'stocks', 'YHOO.csv')
# must specify that the 'Date' column is actually a date
# and pandas will try its best to convert it
df = pd.read_csv(fname, parse_dates=['Date'])
fig, ax = plt.subplots()
ax.plot(df['Date'], df['Adj Close'])
~~~

Without pandas, here's what that typecasting would look like:


~~~py
from os.path import join
from datetime import datetime
import csv
fname = join('data', 'stocks', 'YHOO.csv')
with open(fname, 'r') as rf:  
    data = list(csv.DictReader(fname))
    for d in data:
        d['Date'] = datetime.strptime(d['Date'], '%Y-%m-%d')
        d['Adj Close'] = float(d['Adj Close'])
# then the visualization code...
~~~

# Coercing numeric values with pandas

The [2014 SAT score data](data/schools/sat-2014.csv) is an example of annoyingly difficult dirty data. The columns contain a mix of numbers and things like asterisks, which need to be cleared out if pandas is to typecast a column as all numbers/floats/etc.

The coercion can be done when __read_csv()__ is called; [check out the documentation for all of its arguments](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html).

One argument is __na_values__, which let's us specify strings values that should be considered as "not-a-number" values. Such as `'NA'` or `'*'`:

Here's the import without specifying `na_values`:


~~~py
from os.path import join
import pandas as pd
fname = join('data', 'schools', 'sat-2014.csv')
adf = pd.read_csv(fname)
bdf = pd.read_csv(fname, na_values=['*'])
~~~

Compare the `dtypes` attributes of `adf` and `bdf` -- many more columns of the `bdf` dataframe are typecasted as numbers.

Now it's easy to filter the SAT results by schools that have a minimum number of test takers:

~~~py
cdf = bdf[bdf['number_of_test_takers'] >= 20]
~~~







