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

### Step 4:  xxxxxxx

```r
# list of fruits to include
fruit_list <- c("Kiwi", "Banana", "Apricot", "Avocado", "Cocos", "Clementine", "Mandarine", "Orange",
   "Limes", "Lemon", "Peach", "Plum", "Raspberry", "Strawberry", "Pineapple", "Pomegranate")

# Number of output classes (i.e. types of fruits)
output_n <- length(fruit_list)

# Image size to scale down to (original images are 100 x 100 px)
img_width <- 20
img_height <- 20
target_size <- c(img_width, img_height)

# RGB = 3 channels
channels <- 3

# path to image folders
train_image_files_path <- "./fruits-360_dataset/fruits-360/Training/"
valid_image_files_path <- "./fruits-360_dataset/fruits-360/Validation/"
```


## Notes
https://github.com/hussius/deeplearning-biology
https://bradleyboehmke.github.io/HOML/
