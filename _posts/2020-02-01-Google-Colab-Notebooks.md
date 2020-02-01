---
layout: post
title: Playing around with Google Colab and VEs
---
Simply type Google Colab Notebooks into the search engine and click on 'Show notebooks in Drive - Google' to get started. Let us also see how to create a virtual environment and install packages according to a requirements.txt file.

<hr>

### Google Colabs and TensorFlow 2
We may want to import tensforflow and this is simply done by saying:

```python
import tensorflow as tf
```

However, you may get a warning message saying that the default TensorFlow will soon be switching to version 2.x. You will also see that if you type:

```python
tf.__version__
```

It will most likely be version 1.15.0 that has been imported. To get around this, restart the runtime by going up to the top and selecting Runtime and the necessary option.

Then type:
```python
%tensorflow_version 2.x
```

And then run:
```python
import tensorflow as tf
```

Now if you check the version, it will be version 2.1 or higher.

### Getting a GPU setup
To get a GPU setup, simply go to Edit at the top and select Notebook settings. From here, you can then select the Hardware accelerator to be GPU.

### Virtual environments
Instead of using Google Colab, we can also use our own local machine. And we can use a requirements list to install necessary packages if we want to run someone's algorithm.

Let us first create the virtual environment - in the command prompt (or terminal) type:
`conda create -n tf2 python=3.7`

Now activate the virtual environment by typing:
`activate tf2`

It may be that what you may need to type is different dependent on your operating system; e.g., for Linux, it may be `source activate <name of your virtual environment>`.

And then maybe we have a requirements.txt file that contains certain packages that we want. Such a requirements.txt file would be a simple list such as:

matplotlib==3.1.1
pandas==0.25.2
tensorflow==2.1.0

Simply navigate to the area containing your requirements.txt and do:
`pip install -r requirements.txt`


That is all!
