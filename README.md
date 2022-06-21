# Cryptocurrencies

## Overview

Accountability Accounting, a prominent investment bank, is interested in offering a new cryptocurrency investment portfolio for its customers. This project uses unsupervised learning models to group cryptocurrencies that are currently on the trading market to create a classification system for this new investment.

The 'crypto_data.csv' was sourced from [CryptoCompare](https://min-api.cryptocompare.com/data/all/coinlist).
A clustering algorithm was used to group the cryptocurrencies. 

## Results
#### Deliverable 1: Preprocessing the Data for PCA (Principal Component Analysis)

I processed the data set to keep all the cryptocurrencies that are being traded. Then I removed any rows that contain at least one null value. The `crypto_df` Dataframe was further filtered to keep only rows where coins have been mined. 

I created a new DataFrame `names_df` that holds only the cryptocurrency names and used the `crypto_df` DataFrame index as its index:

![names](https://github.com/stephperillo/Cryptocurrencies/blob/main/Resources/names.png)

Next I removed the `CoinName` column the `crypto_df` DatafFrame since it will not be used on the clustering algorithm.

This is what `crypto_df` looks like at this point:

![crypto_df](https://github.com/stephperillo/Cryptocurrencies/blob/main/Resources/crypto_df.png)

In the next step in preprocessing, I used the `get_dummies()` method to create variables for the two text features, `Algorithm` and `ProofType`, and stored the resulting data in a new DataFrame named `X`.

Finally, I used StandardScaler `fit_transform()` to standardize the features from `X`.
The StandardScaler function standardizes features by removing the mean and scaling to unit variance.[^1] The `fit_transform()` function is used to fit the scaled data to the DataFrame. 

After preprocessing was completed, there were 532 cryptocurrencies to group.

#### Deliverable 2: Reducing Data Dimensions Using PCA (Principal Component Analysis)

In this deliverable, I applied the Principal Component Analysis algorithm to the preprocessed data from Deliverable 1. This reduced the number of dimensions to three principal components.

The result is the `pcs_df` DataFrame:

![PCS](https://github.com/stephperillo/Cryptocurrencies/blob/main/Resources/PCS.png)

#### Deliverable 3: Clustering Cryptocurrencies Using K-means

Next I created an elbow curve using `hvPlot` to find the best value for `K` from `pcs_df`. 

![elbow_curve](https://github.com/stephperillo/Cryptocurrencies/blob/main/Resources/elbow_curve.png)

The elbow of the line, where the slope drastically changes clearly occurs at K = 4 as an optimal amount of clusters. If K is too high the model is at risk for overfitting, which would give undue importance to patterns within this dataset that are not found in other, similar datasets.

Then I ran the K-means algorithm to predict the clusters. This algorithm grouped the data around four centroids. Centroids denote the mean position of all the points in each cluster.

I added these predictions as a new column, `Class` and concatenated `crypto_df` and `pcs_df` to create `clustered_df`.

The first 10 results of `clustered_df`:

![clustered_df](https://github.com/stephperillo/Cryptocurrencies/blob/main/Resources/clustered_df.png)

#### Deliverable 4: Visualizing Cryptocurrencies Results

I created a 3D scatterplot using Plotly Express `scatter_3d()` showing the three clusters from `clustered_df`:

![3d](https://user-images.githubusercontent.com/99934391/174871242-53726cdc-b965-40e3-844f-b2c273013859.gif)

If you hover over a point on the graph, a pop up displays the Coin Name, Algorithm, Total Coins Mined, and Total Coin Supply.

I created a table showing the tradable cryptocurrencies using `hvplot.table()`:

!(hvplot.table)[https://github.com/stephperillo/Cryptocurrencies/blob/main/Resources/hvplot.table.png]

I wanted to plot the data on a 2D scatterplot so I scaled the data. `MinMaxScaler` is a tool used to scale features to fall between a minimum and maximum value. In this case we scaled the data between zero and one. 

![scatterplot](https://github.com/stephperillo/Cryptocurrencies/blob/main/Resources/scatterplot.png)

This scatterplot shows the different classes and how they vary in the amount of coins mined vs. the total coin supply for each cryptocurrency. It is also interactive and displays the CoinName when you hover over a data point.

## Summary 

This analysis and its visualizations resulted in four different groups of cryptocurrencies based on this dataset. Accountability Accounting will be able to take a closer look at the differences in groups.   


[^1]: https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler   
