# Python Library Tutorial Blog

In statistics, we are often interested in looking at the density of data across different variables. This is easy to do when looking at probability density functions (PDFs). However, a PDF only helps us look at the density of the data over one variable, and is less efficient when we hope to look at the density of the data between two variables. In this scenario, we need a third dimension to our graph that can let us view the density of the data in a clean, simple fashion. A great way to accomplish this is using a heat map.

## What is a heat map?
Imagine we have two variables in a dataset. To give an example, we can look at a dataset commonly used for data analysis practice, the Palmer Penguins dataset from Seaborn in Python. In this example, we might be interested in the relationship between a penguin's different body measurements. We can plot different scatterplots of the data as linear relationships, but there is a wide distribution of values for the variables we want to use. What if we want to view the density of the data, rather than the linear relationship? A heat map can help us view it. A heat map is a graphic that uses a color scale to show the density of data. We will look at how heat maps look in python and the coding options we have when making them.

## The Data
```python
import seaborn as sns
df = sns.load_dataset('penguins').dropna()
df.head()
```

This is the list of libraries we will be using, as well as code to load in the dataset. I chose to drop all missing values in this case. For this and all code, it may be copied for your own use.

There are a list of values that we might be interested in, such as the relationship between a penguin's bill length and its body mass. The code make a heat map for the density between the two is below.

```python
sns.histplot(
    data=df,
    x="bill_length_mm",
    y="flipper_length_mm",
    bins=12,
    cmap="viridis",
    cbar=True  
)
```
![](docs/hist_heatmap.png)

Let's break it down step by step. The `histplot()` function from seaborn is going to tally up the values from the variables `bill_length_mm` and `flipper_length_mm`. 

Then, it is going to place them in `bins`, grouping by every 10 mm.

Finally, we chose the color pallette viridis, and added `cbar`, which displays the color key.

### First Analysis
In our first heat map of the penguin dataset, we can see not only the linear relationship between flipper length and bill length, but also the density of the data. It seems that many penguins had a flipper length of around 190mm with a bill length of around 35-40mm, while it was much more rare to have a penguin with a flipper length of 230mm and bill length of 55mm, even though it seems to be on the line of best fit. This is the benefit of a heat map- seeing aspects of the data that simple linear models struggle to display. 

As great as that is, it feels sloppy in a way. The penguin dataset especially points out flaws in the code we chose, as there are not too many observations and we needed to make the bin size rather small in order to get it to look nice. A better way of doing this, so that we get a smoother feeling graph, is a `kdeplot()`.

## `kdeplot()`
Here is an example of how to use the `kdeplot()` instead of the histplot.
```python
sns.kdeplot(
    data=df,
    x="bill_length_mm",
    y="flipper_length_mm",
    fill=True,         
    cmap="viridis",     
    thresh=0.05,        
    levels=100          
)
```
![](docs/kde_heatmap.png)

Look at how much smoother the graph has become! The new plot uses many of the previous parameters, but also introduces some new ones. The `thresh` parameter is essentially what threshold of data do we allow to influence our smoothed graph. Do we listen to those rare cases or do we just rely on the most common penguin measurements? A high threshold will exclude more of the less common data points. `levels` is the number of slices we make as we compile the density plot. Think of our plot like a topography map, where a steep mountain has lots of lines densely together. Choosing a high level makes the color breaks indistiguishable, whereas a low level will allow rings of discrete colors to show density. 
