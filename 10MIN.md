pandas

python


10 Minutes to pandas
====================

> 10分钟 Pnadas 体验


This is a short introduction to pandas, geared mainly for new users. You
can see more complex recipes in the <span
role="ref">Cookbook&lt;cookbook&gt;</span>.

Customarily, we import as follows:

python

import pandas as pd import numpy as np import matplotlib.pyplot as plt

Object Creation
---------------

See the <span
role="ref">Data Structure Intro section &lt;dsintro&gt;</span>.

Creating a <span role="class">Series</span> by passing a list of values,
letting pandas create a default integer index:

python

s = pd.Series(\[1,3,5,np.nan,6,8\]) s

Creating a <span role="class">DataFrame</span> by passing a NumPy array,
with a datetime index and labeled columns:

python

dates = pd.date\_range('20130101', periods=6) dates df =
pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD')) df

Creating a `DataFrame` by passing a dict of objects that can be
converted to series-like.

python

df2 = pd.DataFrame({ 'A' : 1.,  
'B' : pd.Timestamp('20130102'), 'C' :
pd.Series(1,index=list(range(4)),dtype='float32'), 'D' :
np.array(\[3\] \* 4,dtype='int32'), 'E' :
pd.Categorical(\["test","train","test","train"\]), 'F' : 'foo' })

df2

The columns of the resulting `DataFrame` have different <span
role="ref">dtypes &lt;basics.dtypes&gt;</span>.

python

df2.dtypes

If you're using IPython, tab completion for column names (as well as
public attributes) is automatically enabled. Here's a subset of the
attributes that will be completed:

@verbatim In \[1\]: df2.&lt;TAB&gt; df2.A df2.bool df2.abs df2.boxplot
df2.add df2.C df2.add\_prefix df2.clip df2.add\_suffix df2.clip\_lower
df2.align df2.clip\_upper df2.all df2.columns df2.any df2.combine
df2.append df2.combine\_first df2.apply df2.compound df2.applymap
df2.consolidate df2.D

As you can see, the columns `A`, `B`, `C`, and `D` are automatically tab
completed. `E` is there as well; the rest of the attributes have been
truncated for brevity.

Viewing Data
------------

See the <span role="ref">Basics section &lt;basics&gt;</span>.

Here is how to view the top and bottom rows of the frame:

python

df.head() df.tail(3)

Display the index, columns, and the underlying NumPy data:

python

df.index df.columns df.values

<span role="func">~DataFrame.describe</span> shows a quick statistic
summary of your data:

python

df.describe()

Transposing your data:

python

df.T

Sorting by an axis:

python

df.sort\_index(axis=1, ascending=False)

Sorting by values:

python

df.sort\_values(by='B')

Selection
---------

Note

While standard Python / Numpy expressions for selecting and setting are
intuitive and come in handy for interactive work, for production code,
we recommend the optimized pandas data access methods, `.at`, `.iat`,
`.loc` and `.iloc`.

See the indexing documentation <span
role="ref">Indexing and Selecting Data &lt;indexing&gt;</span> and <span
role="ref">MultiIndex / Advanced Indexing &lt;advanced&gt;</span>.

### Getting

Selecting a single column, which yields a `Series`, equivalent to
`df.A`:

python

df\['A'\]

Selecting via `[]`, which slices the rows.

python

df\[0:3\] df\['20130102':'20130104'\]

### Selection by Label

See more in <span
role="ref">Selection by Label &lt;indexing.label&gt;</span>.

For getting a cross section using a label:

python

df.loc\[dates\[0\]\]

Selecting on a multi-axis by label:

python

df.loc\[:,\['A','B'\]\]

Showing label slicing, both endpoints are *included*:

python

df.loc\['20130102':'20130104',\['A','B'\]\]

Reduction in the dimensions of the returned object:

python

df.loc\['20130102',\['A','B'\]\]

For getting a scalar value:

python

df.loc\[dates\[0\],'A'\]

For getting fast access to a scalar (equivalent to the prior method):

python

df.at\[dates\[0\],'A'\]

### Selection by Position

See more in <span
role="ref">Selection by Position &lt;indexing.integer&gt;</span>.

Select via the position of the passed integers:

python

df.iloc\[3\]

By integer slices, acting similar to numpy/python:

python

df.iloc\[3:5,0:2\]

By lists of integer position locations, similar to the numpy/python
style:

python

df.iloc\[\[1,2,4\],\[0,2\]\]

For slicing rows explicitly:

python

df.iloc\[1:3,:\]

For slicing columns explicitly:

python

df.iloc\[:,1:3\]

For getting a value explicitly:

python

df.iloc\[1,1\]

For getting fast access to a scalar (equivalent to the prior method):

python

df.iat\[1,1\]

### Boolean Indexing

Using a single column's values to select data.

python

df\[df.A &gt; 0\]

Selecting values from a DataFrame where a boolean condition is met.

python

df\[df &gt; 0\]

Using the <span role="func">~Series.isin</span> method for filtering:

python

df2 = df.copy() df2\['E'\] = \['one',
'one','two','three','four','three'\] df2
df2\[df2\['E'\].isin(\['two','four'\])\]

### Setting

Setting a new column automatically aligns the data by the indexes.

python

s1 = pd.Series(\[1,2,3,4,5,6\], index=pd.date\_range('20130102',
periods=6)) s1 df\['F'\] = s1

Setting values by label:

python

df.at\[dates\[0\],'A'\] = 0

Setting values by position:

python

df.iat\[0,1\] = 0

Setting by assigning with a NumPy array:

python

df.loc\[:,'D'\] = np.array(\[5\] \* len(df))

The result of the prior setting operations.

python

df

A `where` operation with setting.

python

df2 = df.copy() df2\[df2 &gt; 0\] = -df2 df2

Missing Data
------------

pandas primarily uses the value `np.nan` to represent missing data. It
is by default not included in computations. See the <span
role="ref">Missing Data section
&lt;missing\_data&gt;</span>.

Reindexing allows you to change/add/delete the index on a specified
axis. This returns a copy of the data.

python

df1 = df.reindex(index=dates\[0:4\], columns=list(df.columns) + \['E'\])
df1.loc\[dates\[0\]:dates\[1\],'E'\] = 1 df1

To drop any rows that have missing data.

python

df1.dropna(how='any')

Filling missing data.

python

df1.fillna(value=5)

To get the boolean mask where values are `nan`.

python

pd.isna(df1)

Operations
----------

See the <span
role="ref">Basic section on Binary Ops &lt;basics.binop&gt;</span>.

### Stats

Operations in general *exclude* missing data.

Performing a descriptive statistic:

python

df.mean()

Same operation on the other axis:

python

df.mean(1)

Operating with objects that have different dimensionality and need
alignment. In addition, pandas automatically broadcasts along the
specified dimension.

python

s = pd.Series(\[1,3,5,np.nan,6,8\], index=dates).shift(2) s df.sub(s,
axis='index')

### Apply

Applying functions to the data:

python

df.apply(np.cumsum) df.apply(lambda x: x.max() - x.min())

### Histogramming

See more at <span
role="ref">Histogramming and Discretization &lt;basics.discretization&gt;</span>.

python

s = pd.Series(np.random.randint(0, 7, size=10)) s s.value\_counts()

### String Methods

Series is equipped with a set of string processing methods in the str
attribute that make it easy to operate on each element of the array, as
in the code snippet below. Note that pattern-matching in str generally
uses [regular expressions](https://docs.python.org/3/library/re.html) by
default (and in some cases always uses them). See more at <span
role="ref">Vectorized String Methods
&lt;text.string\_methods&gt;</span>.

python

s = pd.Series(\['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog',
'cat'\]) s.str.lower()

Merge
-----

### Concat

pandas provides various facilities for easily combining together Series,
DataFrame, and Panel objects with various kinds of set logic for the
indexes and relational algebra functionality in the case of join /
merge-type operations.

See the <span role="ref">Merging section &lt;merging&gt;</span>.

Concatenating pandas objects together with <span
role="func">concat</span>:

python

df = pd.DataFrame(np.random.randn(10, 4)) df

\# break it into pieces pieces = \[df\[:3\], df\[3:7\], df\[7:\]\]

pd.concat(pieces)

### Join

SQL style merges. See the <span
role="ref">Database style joining &lt;merging.join&gt;</span> section.

python

left = pd.DataFrame({'key': \['foo', 'foo'\], 'lval': \[1, 2\]}) right =
pd.DataFrame({'key': \['foo', 'foo'\], 'rval': \[4, 5\]}) left right
pd.merge(left, right, on='key')

Another example that can be given is:

python

left = pd.DataFrame({'key': \['foo', 'bar'\], 'lval': \[1, 2\]}) right =
pd.DataFrame({'key': \['foo', 'bar'\], 'rval': \[4, 5\]}) left right
pd.merge(left, right, on='key')

### Append

Append rows to a dataframe. See the <span
role="ref">Appending &lt;merging.concatenation&gt;</span> section.

python

df = pd.DataFrame(np.random.randn(8, 4), columns=\['A','B','C','D'\]) df
s = df.iloc\[3\] df.append(s, ignore\_index=True)

Grouping
--------

By "group by" we are referring to a process involving one or more of the
following steps:

> -   **Splitting** the data into groups based on some criteria
> -   **Applying** a function to each group independently
> -   **Combining** the results into a data structure

See the <span role="ref">Grouping section &lt;groupby&gt;</span>.

python

df = pd.DataFrame({'A' : \['foo', 'bar', 'foo', 'bar',  
'foo', 'bar', 'foo', 'foo'\],

'B' : \['one', 'one', 'two', 'three',  
'two', 'two', 'one', 'three'\],

'C' : np.random.randn(8),  
'D' : np.random.randn(8)})

df

Grouping and then applying the <span role="meth">~DataFrame.sum</span>
function to the resulting groups.

python

df.groupby('A').sum()

Grouping by multiple columns forms a hierarchical index, and again we
can apply the `sum` function.

python

df.groupby(\['A','B'\]).sum()

Reshaping
---------

See the sections on <span
role="ref">Hierarchical Indexing &lt;advanced.hierarchical&gt;</span>
and <span role="ref">Reshaping &lt;reshaping.stacking&gt;</span>.

### Stack

python

tuples = list(zip(\*\[\['bar', 'bar', 'baz', 'baz',  
'foo', 'foo', 'qux', 'qux'\],

\['one', 'two', 'one', 'two',  
'one', 'two', 'one', 'two'\]\]))

index = pd.MultiIndex.from\_tuples(tuples, names=\['first', 'second'\])
df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=\['A',
'B'\]) df2 = df\[:4\] df2

The <span role="meth">~DataFrame.stack</span> method "compresses" a
level in the DataFrame's columns.

python

stacked = df2.stack() stacked

With a "stacked" DataFrame or Series (having a `MultiIndex` as the
`index`), the inverse operation of <span
role="meth">~DataFrame.stack</span> is <span
role="meth">~DataFrame.unstack</span>, which by default unstacks the
**last level**:

python

stacked.unstack() stacked.unstack(1) stacked.unstack(0)

### Pivot Tables

See the section on <span
role="ref">Pivot Tables &lt;reshaping.pivot&gt;</span>.

python

df = pd.DataFrame({'A' : \['one', 'one', 'two', 'three'\] \* 3,  
'B' : \['A', 'B', 'C'\] \* 4, 'C' : \['foo', 'foo', 'foo', 'bar', 'bar',
'bar'\] \* 2, 'D' : np.random.randn(12), 'E' : np.random.randn(12)})

df

We can produce pivot tables from this data very easily:

python

pd.pivot\_table(df, values='D', index=\['A', 'B'\], columns=\['C'\])

Time Series
-----------

pandas has simple, powerful, and efficient functionality for performing
resampling operations during frequency conversion (e.g., converting
secondly data into 5-minutely data). This is extremely common in, but
not limited to, financial applications. See the <span
role="ref">Time Series section &lt;timeseries&gt;</span>.

python

rng = pd.date\_range('1/1/2012', periods=100, freq='S') ts =
pd.Series(np.random.randint(0, 500, len(rng)), index=rng)
ts.resample('5Min').sum()

Time zone representation:

python

rng = pd.date\_range('3/6/2012 00:00', periods=5, freq='D') ts =
pd.Series(np.random.randn(len(rng)), rng) ts ts\_utc =
ts.tz\_localize('UTC') ts\_utc

Converting to another time zone:

python

ts\_utc.tz\_convert('US/Eastern')

Converting between time span representations:

python

rng = pd.date\_range('1/1/2012', periods=5, freq='M') ts =
pd.Series(np.random.randn(len(rng)), index=rng) ts ps = ts.to\_period()
ps ps.to\_timestamp()

Converting between period and timestamp enables some convenient
arithmetic functions to be used. In the following example, we convert a
quarterly frequency with year ending in November to 9am of the end of
the month following the quarter end:

python

prng = pd.period\_range('1990Q1', '2000Q4', freq='Q-NOV') ts =
pd.Series(np.random.randn(len(prng)), prng) ts.index = (prng.asfreq('M',
'e') + 1).asfreq('H', 's') + 9 ts.head()

Categoricals
------------

pandas can include categorical data in a `DataFrame`. For full docs, see
the <span role="ref">categorical introduction &lt;categorical&gt;</span>
and the <span
role="ref">API documentation &lt;api.categorical&gt;</span>.

python

df = pd.DataFrame({"id":\[1,2,3,4,5,6\], "raw\_grade":\['a', 'b', 'b',
'a', 'a', 'e'\]})

Convert the raw grades to a categorical data type.

python

df\["grade"\] = df\["raw\_grade"\].astype("category") df\["grade"\]

Rename the categories to more meaningful names (assigning to
`Series.cat.categories` is inplace!).

python

df\["grade"\].cat.categories = \["very good", "good", "very bad"\]

Reorder the categories and simultaneously add the missing categories
(methods under `Series .cat` return a new `Series` by default).

python

df\["grade"\] = df\["grade"\].cat.set\_categories(\["very bad", "bad",
"medium", "good", "very good"\]) df\["grade"\]

Sorting is per order in the categories, not lexical order.

python

df.sort\_values(by="grade")

Grouping by a categorical column also shows empty categories.

python

df.groupby("grade").size()

Plotting
--------

See the <span role="ref">Plotting &lt;visualization&gt;</span> docs.

python

import matplotlib.pyplot as plt plt.close('all')

python

ts = pd.Series(np.random.randn(1000), index=pd.date\_range('1/1/2000',
periods=1000)) ts = ts.cumsum()

@savefig series\_plot\_basic.png ts.plot()

On a DataFrame, the <span role="meth">~DataFrame.plot</span> method is a
convenience to plot all of the columns with labels:

python

df = pd.DataFrame(np.random.randn(1000, 4), index=ts.index,  
columns=\['A', 'B', 'C', 'D'\])

df = df.cumsum()

@savefig frame\_plot\_basic.png plt.figure(); df.plot();
plt.legend(loc='best')

Getting Data In/Out
-------------------

### CSV

<span role="ref">Writing to a csv file. &lt;io.store\_in\_csv&gt;</span>

python

df.to\_csv('foo.csv')

<span
role="ref">Reading from a csv file. &lt;io.read\_csv\_table&gt;</span>

python

pd.read\_csv('foo.csv')

python

os.remove('foo.csv')

### HDF5

Reading and writing to <span
role="ref">HDFStores &lt;io.hdf5&gt;</span>.

Writing to a HDF5 Store.

python

df.to\_hdf('foo.h5','df')

Reading from a HDF5 Store.

python

pd.read\_hdf('foo.h5','df')

python

os.remove('foo.h5')

### Excel

Reading and writing to <span
role="ref">MS Excel &lt;io.excel&gt;</span>.

Writing to an excel file.

python

df.to\_excel('foo.xlsx', sheet\_name='Sheet1')

Reading from an excel file.

python

pd.read\_excel('foo.xlsx', 'Sheet1', index\_col=None,
na\_values=\['NA'\])

python

os.remove('foo.xlsx')

Gotchas
-------

If you are attempting to perform an operation you might see an exception
like:

``` sourceCode
>>> if pd.Series([False, True, False]):
    print("I was true")
Traceback
    ...
ValueError: The truth value of an array is ambiguous. Use a.empty, a.any() or a.all().
```

See <span role="ref">Comparisons&lt;basics.compare&gt;</span> for an
explanation and what to do.

See <span role="ref">Gotchas&lt;gotchas&gt;</span> as well.
