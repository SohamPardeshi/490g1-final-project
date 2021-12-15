## Always Judge a Book By Its Cover

### Abstract
The well-known famous English idiom "you should never judge a book by its cover" applies to many things - but not books! Compelling cover art often reveals a lot about a novel while drawing in readers by piquing their interest. A natural question arises: can we train a machine to extract this subtle information in the ways that humans do?

Unfortunately, book covers don't make for good input to machine learning models. Book cover images vary too much in shape and size, while text on the cover (e.g. the title or author) can use uniquely different fonts. In comes Audible, an online audiobook and podcast streaming service. Audible helps match authors to professional narrators in order to convert books into audiobooks. During this process, they hire artists to modify book cover art into a square form. By scraping 11,958 modified book covers and title text from Audible, we have created a dataset that we hope to use to "judge a book by its cover".

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


### Results
Results - How well did you do


### Examples
Examples - images/text/live demo, anything to show off your work (note, demos get some extra credit in the rubric)

### Video

