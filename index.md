## Always Judge a Book By Its Cover

### Abstract
The well-known famous English idiom "you should never judge a book by its cover" applies to many things - but not books! Compelling cover art often reveals a lot about a novel while drawing in readers by piquing their interest. A natural question arises: can we train a machine to extract this subtle information in the ways that humans do?

Unfortunately, book covers don't make for good input to machine learning models. Book cover images vary too much in shape and size, while text on the cover (e.g. the title or author) can use uniquely different fonts. In comes Audible, an online audiobook and podcast streaming service. Audible helps match authors to professional narrators in order to convert books into audiobooks. During this process, they hire artists to modify book cover art into a square form. By scraping 11,958 modified book covers and title text from Audible, we have created a dataset that we hope to use to "judge a book by its cover".

### Video
<div style="position: relative; padding-bottom: 56.25%; height: 0;"><iframe src="https://www.loom.com/embed/aee68385b401431494e32500c27c40e3" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe></div>

### Problem Statement
Given 256x256 colored images of book covers and a string representing the title of the book, predict which of 22 categories a book falls under in Audible.

**Note:** For convenience, this is multi-class classification NOT multi-label classification. Each book can only have one category. To handle cases where a book fits in many categories, we simply choose the book's alphabetically last category.

### Related Work
I produced my own dataset by scraping images and title text from Audible using [a simple Python notebook](https://colab.research.google.com/drive/1CjkhO3SELTK_KpOblS4pAcoaqMuzP1gC?usp=sharing).

Admittedly, I did not use as much related work as I would have liked. However, I found one excellent survey paper titled [Benchmarking Deep Learning Models for Classification of Book Covers](https://link.springer.com/article/10.1007/s42979-020-00132-z) which helped me compare my architecture to modified versions of pre-trained models. My dataset differs from the public dataset described in the paper in that my approach (1) trains on 5x less data and (2) contains additional text information. Although it is not an exact match, this paper helped me measure the effectiveness of my model with other approaches.

### Methodology
Since this is a mult-input problem, I thought it would be apt to use a CNN-like structure to handle book cover image input and an RNN-like structure for the book title input. I initially thought combining these would be difficult, but some prior work on multi-input models indicates that it is usually sufficient to just flatten the output of each of these architectures and concatenate them before passing them into a fully-connected layer.

###### Learning from images
The images were originally 256x256 pixels - however some testing revealed that I could halve the image dimensions with almost no loss to accuracy. This helped me speed up training a lot! I also played around with different image transformations. Randomly jittering pixels and rotating the images seemed to prevent some overfitting on the validation set. I also experimented with color modifications (e.g. random color jitter or grayscaline) but these significantly hurt the performance. This could be explained by color serving as a valuable signal to what category a book belongs to (e.g. pink => romance, green => nature). My actual model was implemented through simple CNNs like we learned. I considered using pre-trained models, but decided against since many pre-trained image networks are designed for identifying the cotents of an image, but book covers frequently have "abstract" art. I ended up getting a ~24.47% accuracy using only image data on the test set.

###### Learning from titles
Book titles also contain useful information about the category. To that end, I considered hand-crafting some RNN-like structures and using pre-trained models. My preprocessing step was essentiall the same for all models - pad the title based on the maximum title length which was 54. My hand crafted models included RNNs, LSTMs, and GRUs followed by a linear layer to reduce. I wanted to try GloVe representations of the titles, but unfortunately I couldn't get it to work. So I only consider pre-trained variations of BERT from the NLP hugging face library. The pre-trained model that worked the best as the BERT for Sequence Classification which got 26.03% accuracy on the test set.

###### Hyperparameter Search
Once I found my preferred models for working with the image and text data independently, I combined them using a fully-connected layer to connect their output. This also served as they layer that would consolidate my weights into a final classification output. I used the [Ax platform](https:\\ax.dev) in order to do hyperparameter search via Bayesian optimization. Specifically, I searched through learning rate, momentum, weight decay, whether or not Nesterov was used, the number of output channels in my CNN, the kernel size in my CNN, and the size of my max pool. I ran 10 iterations of search (because I am poor and bounded by the usage limits of Google Colab). This ended up with a test accuracy of 33.92%!

### Experiments/Evaluation
Experiments/evaluation - how are you evaluating your results
I split the dataset into training (60%), validation (20%), and test (20%) to run experiments. I chose the best performing architecture on the validation set, ran hyperparameter search, and then trained a final model on the training + validation sets and test it on the test set.

*Only images*:

| Model Name | Training Accuracy | Validation Accuracy |
|-------|--------|---------|
| Simple Convolution | 21.98% | 22.56% |
| 1 Full Convolution Layer | 28.77% | 26.03% |
| 2 Full Convolution Layer | 30.04% | 26.11% |

Here, full convolution layer includes dropout, maxpool, and batch norm. Although 2 Full Convolution layers performed better, the difference with 1 full convolution layer was negligible and it took WAY more time to run.

*Only text*:

| Model Name | Training Accuracy | Validation Accuracy |
|-------|--------|---------|
| RNN | 14.28% | 15.17% |
| LSTM | 14.50% | 14.23% |
| BERT | 28.49% | 25.91% |
| BERT for Sequence Generation | 28.38% | 26.03% |

I think there was relatively no difference between LSTM and RNN because the sentences were short and relied less on context. BERT somehow blew these out of the water, but I'm not immediately sure why other than BERT is trained on a lot of data. 

I ended up combining 1 Full Convolution Layer (good performance + speed) with BERT for Sequence Generation.

### Results
Combining these resulted in a model that I ran hyperparameter searh over. The best search netted the following:

| Parameter | Value |
|-------|--------|
| LR | 0.0009222003494347612 |
| Momentum | 0.037071920931339264 |
| Weight Decay | 2.3331490406026315e-06 |
| M | 70 | 
| k | 7 | 
| N | 14 | 

where M is the number of output channels, k is the kernel size, and N is the pool size. Retraining this model on both the test set and the validation set resulted in an overall test accuracy of 33.958%.

### Examples
To use the demo, you'll need to follow these steps:

1. You'll need access to the dataset in order to use the notebook. Add a copy or a link of the public Google Driver folder in your "My Drive" (i.e. the root folder) of the email you are using. [Audible Dataset Folder](https://drive.google.com/drive/folders/19H5GsRGsLvwm350dbMAYuPO3_JWwijDu?usp=sharing)
2. Now, simply run the demo! You can access it here: [https://colab.research.google.com/drive/1QNwIC_1pvyY7X-FMYfB5Tsg0Wc6U7L5u?usp=sharing](https://colab.research.google.com/drive/1QNwIC_1pvyY7X-FMYfB5Tsg0Wc6U7L5u?usp=sharing)


### Project Links
To see all of the work in my projects, you can view these notebooks and slides.
- [Presentation Slides](https://docs.google.com/presentation/d/1K6__rhH25OgbwqZvlMlfX75R-YHsyYvxKiPKS7OOMAI/edit?usp=sharing)
- [Dataset Downloader](https://colab.research.google.com/drive/1CjkhO3SELTK_KpOblS4pAcoaqMuzP1gC?usp=sharing)
- [Project Notebook](https://colab.research.google.com/drive/1E1GKQjZ1hweY2ZTR4AzIuwKaTHbdjhCD?usp=sharing)