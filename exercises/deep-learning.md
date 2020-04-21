# Deep Learning Tutorial with Keras
based on [this tutorial](https://www.shirin-glander.de/2018/06/keras_fruits/)

https://keras.rstudio.com

### Step 1: Install Keras in R
Installation only has to be performed once, afterwards, the `Keras` package can be loaded each time in R by simply using `library(keras)`

```r
# Install the R package Keras
devtools::install_github("rstudio/keras")
library(keras)
```

### Step 3:  Download fruit data
This dataset is from [Kaggle](www.kaggle.com) and can be downloaded here: https://www.kaggle.com/moltean/fruits/data.  The dataset in total contains 82,213 images of 120 different fruits.  However, for the sake of time we will only be analyzing a few.  You will need to unzip the file you downloaded by either double-clicking on the file, using the UNIX command `unzip fruits.zip`, or in R as follows:
```R
unzip("fruits.zip")
```

