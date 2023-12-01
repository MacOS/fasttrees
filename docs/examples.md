---
layout: default
theme: just-the-docs
title: "fasttrees docs"
---
# Examples


## Credict approval data set
Let's walk through an example of using the Fast-and-Frugal Tree classifier.
First, we import the Fast-and-Frugal Tree classifier.

```python
from fasttrees.fasttrees import FastFrugalTreeClassifier
```

Now let’s get some data to fit our classifier to. Fast-and-Frugal Trees tend to do well on real-world data prone to (human) error, as they disregard information that doesn’t seem very predictive of the outcome. A typical use case is in an operational setting where humans quickly have to take decisions. You could then fit a fast-and-frugal tree to the data in advance, and use the simple resulting tree to quickly make decisions.

As an example of this, let’s have a look at credit decisions. UCI provides a credit approval dataset. Download the crx.data file from the data folder.

Let’s load the data as CSV to a Pandas dataframe:

```python
import pandas as pd
data = pd.read_csv('crx.data', header=None)
```

As there is no header, the columns are simply numbered 1, 2, 3 etc. Let’s make clear they’re attributes by naming them A1, A2, A3 etc.

```python
data.columns = ['A{}'.format(nr) for nr in data.columns]
```

The fasttrees implementation of fast-and-frugal trees can only work with categorical and numerical columns, so let’s assign the appropriate dtype to each column:

```python
import numpy as np

cat_columns = ['A0', 'A3', 'A4', 'A5', 'A6', 'A8', 'A9', 'A11', 'A12']
nr_columns = ['A1', 'A2', 'A7', 'A10', 'A13', 'A14']

for col in cat_columns:
    data[col] = data[col].astype('category')

for col in nr_columns:
    # only recast columns that have not been correctly inferred
    if data[col].dtype != 'float' and data[col].dtype != 'int':
        # change the '?' placeholder to a nan
        data.loc[data[col] == '?', col] = np.nan
        data[col] = data[col].astype('float')
```

The last column is the variable we want to predict, the credit decision. It’s denoted by + or -. For our FastFrugalTreeClassifier to work we need to convert this to boolean:

```python
data['A15'] = data['A15'].apply(lambda x: True if x=='+' else False).astype(bool)
```

Your data should now look something like this:

```
	A0 	A1 	A2 	A3 	A4 	A5 	A6 	A7 	A8 	A9 	A10 	A11 	A12 	A13 	A14 	A15
0 	b 	30.83 	0 	u 	g 	w 	v 	1.25 	t 	t 	1 	f 	g 	202 	0 	True
1 	a 	58.67 	4.46 	u 	g 	q 	h 	3.04 	t 	t 	6 	f 	g 	43 	560 	True
2 	a 	24.5 	0.5 	u 	g 	q 	h 	1.5 	t 	f 	0 	f 	g 	280 	824 	True
3 	b 	27.83 	1.54 	u 	g 	w 	v 	3.75 	t 	t 	5 	t 	g 	100 	3 	True
4 	b 	20.17 	5.625 	u 	g 	w 	v 	1.71 	t 	f 	0 	f 	s 	120 	0 	True
```

Now let’s do a train test split (we use two thirds of the data to train on):

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(data.drop(columns='A15'), data['A15'], test_size=0.33, random_state=0)

We can now finally instantiate our fast-and-frugal tree classifier. Let’s use the default parameters:

fc = FastFrugalTreeClassifier()
```

Let’s fit the classifier to our training data (this can take a few seconds):

```python
fc.fit(X_train, y_train)
```

We can take a look at the resulting tree, which can be used for decision making:

```python
fc.get_tree()
```

```
	IF NO 	feature 	direction 	threshold 	IF YES
0 	decide NO 	A8 	in 	(‘t’,) 	↓
1 	↓ 	A10 	> 	1 	decide YES
2 	↓ 	A9 	in 	(‘t’,) 	decide YES
3 	decide NO 	A7 	> 	1.25 	decide YES
```

Now somebody making a decision can simply look at the 3 central columns, which read, for example A8 in ('t',) and have a look whether this is the case. If it isn’t, they take the action in IF NO, which would be to decide NO (in this case that would mean not to grant this specific person a credit). If it is, they take the action in IF YES, which is to look at the next feature, for which they then repeat the process.

How well does this simple tree classifier perform? Let’s score it against the test data. By default the balanced accuracy score is used:

```python
fc.score(X_test, y_test)
```

This returns a balanced accuracy of 0.86, pretty good!

We can also have a look at how much information it actually used to make its decisions:

```python
fc.get_tree(decision_view=False)
```

```
	feature 	direction 	threshold 	type 	balanced_accuracy_score 	fraction_used 	exit
0 	A8 	in 	(‘t’,) 	categorical 	0.852438 	1 	0
1 	A10 	> 	1 	numerical 	0.852438 	0.528139 	1
2 	A9 	in 	(‘t’,) 	categorical 	0.852438 	0.238095 	1
3 	A7 	> 	1.25 	numerical 	0.852438 	0.192641 	0.5
```

While the first cue is used for all decisions, the second is only used for 52% of all decisions. This means that 48% of decisions could be made by just looking at one single feature.

Hence, fast and frugal trees provide a very easy way to generate simple decision criteria from a large dataset, which often perform better than more advanced machine learning algorithms, and are much more transparent.
