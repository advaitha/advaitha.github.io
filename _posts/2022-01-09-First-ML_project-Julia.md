# My First Machine Learning Project in Julia

This year, I planned to learn **```Julia```**. As a kick of project, I decided to build a ML model using Julia. you may ask why Julia?

A few reasons which I reasonated with are:
* Julia is fast
* Julia supports multiple dispatch as a first class citizen 
* Custom functions which can executed at speed 
* Julia is a dynamically typed language which does not require compiling in advance like C++
* Julia does not suffer from "two-langauge" problem like python. 
* Amazing package management
* Julia is easy to learn like python
* Julia has a good ecosystem of packages for Machine Learning, Deep learning, Optimizations, simulation etc. In future I would like to learn more about simulation and optimization. <mark>Thers is no use of ML model predictions if they are not used for decision making. Optimization will help change that.</mark>


Years before as part of the interviewing process, a company provided me data to build a model. The data is related to telecom industry. The objective was to predict the churn of the customers. I used the same data to kick-off my first ML project in Julia. This will be a two-part blog post. In the first part, I will explain the steps I followed to prepare the data. In the second part, I will build different models and show the results.

To start with, I downloaded Julia, setup VS code, and configured the IDE to use Julia in the VS Code notebooks. Followed by creating an environment for the project. All of the steps required a fair amount of googling. Once the environment is set up, then the project was ready to go.

Load the required packages

```julia
using GLMNet
using MLBase
using Plots
using DecisionTree
using NearestNeighbors
using Random
using LinearAlgebra
using DataStructures
using DataFrames
using LIBSVM
using CSV
using CategoricalArrays
```

Read the data

```julia
data = CSV.read("telecom_churn.csv",DataFrame)
```

![data](j_read_data.png)

```julia
# Check the shape of the data
println(summary(data))
```
3333×21 DataFrame

```julia
# Look at the data types and stats
describe(telecom_data)
```
![describe](j_describe.png)


```julia
# Remove phone number as it does not have an information value
data = select!(telecom_data, Not(:"phone number"))
```

There were string columns in the data. I converted them to categorical values. This took a lot of googling. Also, I din't know if the ML models in Julia will accept categorical data or it will expect one-hot encoded data. I provided the categorical data as input, but it was not accepted by the model. Then I converted the categorical data to one-hot encoded data.

```julia
# Convert string to categorical data
area_code = categorical(data[!,:"area code"];)
data[!,:"area code"] = area_code
```

I have converted all the string columns to categorical data in the same fashion.

I tried to create a function and use, but was not able to get it to work. I need to learn more about writing functions in Julia.

```julia
# The code I used for the function
function change_string_column(df, col_name)
    cat_array = categorical(df[!,col_name])
    df[!,:col_name] = cat_array
    df
end
```
The function was executing succesfully,but the string was not getting converted to categorical data.

I wanted to check the number of unique values in categorical columns.

```julia
countmap(data."state")
```
![Unique states](j_unique.png)

There were 51 unique values in the state column. It means 51 new columns will be added to the data when this column is one-hot encoded.

```julia
ux = unique(data."state"); transform!(data, @. :"state" => ByRow(isequal(ux)) .=> Symbol(:"state_", ux))
```

I used the above code for all the categorical columns.

Now, I need to remove the redundant original categorical columns.

```julia
select!(data,Not(:"voice mail plan"))
```

Again, I was not able to remove all the redundant columns at once. I tried different methods but none of them worked.

I created a vector for the churn column
and removed churn column from the dataframe.

```julia
churn = data[!,:churn]
```

Created a matrix for all the predictors.

```julia
X = Matrix(data[:,1:73])
```

```
churnlabelsmap = labelmap(churn)
y = labelencode(churnlabelsmap, churnlabels)
```

With this, my data is ready to be used for training the Machine learning models.

**Scope for improvement**
* I need to learn more about writing functions in Julia, there by avoid writing the same code again and again.
* Learn operating on multiple columns at once, like deleting multiple columns etc.
* Learn more dataframe operations and ofcourse to explore and visualize the data. May be this is a topic for a future blog post

In the next part, I will be using this data to build different ML models and analyze the results.















