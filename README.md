## Volcanic-Data-Visualizations-Plotly

Plotly is a technical computing company that develops online data analytics and visualization tools.

It can be easily installed via pip install plotly and then imported in our python notebook.

The dataset that we are going to use contains information about volcanos around the world.

```Python
import numpy as np
import pandas as pd
import plotly

# This line is important to display plots in Jupyter Notebook
plotly.offline.init_notebook_mode(connected=True)

df = pd.read_csv("https://raw.githubusercontent.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/master/data/volcano_db.csv", encoding="iso-8859-1")
df.head()
```
![alt text](https://github.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/blob/master/images/data_1.PNG)

A first element we might be interested to collect is the density of volcanos in each country. To do so, let’s start with plotting a histogram which counts the occurrences of each country in our dataset:
```Python
import plotly.express as px
fig = px.histogram(df, x="Country")
fig.show()
```
![alt text](https://github.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/blob/master/images/plot1.png)

For a cleaner visualization, we can sort countries in descending order like so:
```Python
fig = px.histogram(df, x="Country").update_xaxes(categoryorder="total descending")
fig.show()
```
![alt text](https://github.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/blob/master/images/plot2.png)

Furthermore, we can add new features to our histogram. 

Indeed, looking at our dataset we can see that there is one feature, ‘Status’, which might add relevant information to our graph. 

So let’s first visualize a pie chart of all the status and their importance (in terms of percentage):

```Python
import plotly.graph_objects as go
labels=df.Status.unique()
values=df['Status'].value_counts()
fig = go.Figure(data=[go.Pie(labels=labels, values=values)])
fig.show()
```
![alt text](https://github.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/blob/master/images/plot3.png)

```Python
fig = px.histogram(df, x="Country",color="Status").update_xaxes(categoryorder="total descending")
fig.show()
```
![alt text](https://github.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/blob/master/images/plot4.png)

With a first glimpse at our graph, we can see that the majority of volcanos are in either Holocene or Historical status.

We can also focus a bit more on our volcanos themselves and see, namely, which are the highest. For this purpose, we will use and customize a bar plot:

```Python
import plotly.express as px
fig = px.bar(df, x='Volcano Name', y='Elev', color='Type', height=400)
fig.show()
```
![alt text](https://github.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/blob/master/images/plot5.png)

Since we have 1454 volcanos, the barplot is hard to interpret. We can zoom in to see the individual volcano details.

Now let’s focus on a new section of our dataset, which is that of geolocation of our volcanos. Indeed, looking at our data we can see that there are two features, latitude and longitude, which allow us to plot our volcanos on maps. Let’s implement it with Plotly:

```Python
import plotly.graph_objects as go
fig = go.Figure(data=go.Scattergeo(
        lon = df['Longitude'],
        lat = df['Latitude'],
        mode = 'markers'
        ))
fig.update_layout(
        title = 'Volcanos',
        geo_scope='world',
    )
fig.show()
```
![alt text](https://github.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/blob/master/images/plot6.png)

The same result can be obtained with a different layout. The following code shows how to display the volcanos on a 3D globe map:

```Python
fig = go.Figure(data=go.Scattergeo(
        lon = df['Longitude'],
        lat = df['Latitude'],
        mode = 'markers',
        showlegend=False,
        marker=dict(color="crimson", size=4, opacity=0.8))
        )
fig.update_geos(
    projection_type="orthographic",
    landcolor="white",
    oceancolor="MidnightBlue",
    showocean=True,
    lakecolor="LightBlue"
)
fig.show()
```
![alt text](https://github.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/blob/master/images/plot7.png)


Getting new data for volcano and plotting a 3d graph using Plotly

df_v = pd.read_csv("https://raw.githubusercontent.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/master/data/volcano.csv")

We can plot a 3D reproduction of that volcano as follows:

```Python
fig = go.Figure(data=[go.Surface(z=df_v.values)])
fig.update_layout(title='Volcano', autosize=False,
                  width=500, height=500,
                  margin=dict(l=65, r=50, b=65, t=90))
fig.show()
```
![alt text](https://github.com/deepankarkotnala/Volcanic-Data-Visualizations-Plotly/blob/master/images/plot8.png)

