## Always Judge a Book By Its Cover

### Abstract
The well-known famous English idiom "you should never judge a book by its cover" applies to many things - but not books! Compelling cover art often reveals a lot about a novel while drawing in readers by piquing their interest. A natural question arises: can we train a machine to extract this subtle information in the ways that humans do?

Unfortunately, book covers don't make for good input to machine learning models. Book cover images vary too much in shape and size, while text on the cover (e.g. the title or author) can use uniquely different fonts. In comes Audible, an online audiobook and podcast streaming service. Audible helps match authors to professional narrators in order to convert books into audiobooks. During this process, they hire artists to modify book cover art into a square form. By scraping 11,958 modified book covers and title text from Audible, we have created a dataset that we hope to use to "judge a book by its cover".

### Problem Statement
Given 256x256 colored images of book covers and a string representing the title of the book, predict which of 22 categories a book falls under in Audible.

**Note:** For convenience, this is multi-class classification NOT multi-label classification. Each book can only have one category. To handle cases where there are many, we keep only a book's alphabetically last category.

### Related Work
I produced my own dataset by scraping images and title text from Audible using [a simple Python notebook](https://colab.research.google.com/drive/1CjkhO3SELTK_KpOblS4pAcoaqMuzP1gC?usp=sharing).

Admittedly, I did not use as much related work as I would have liked. However, I found one excellent survey paper titled [Benchmarking Deep Learning Models for Classification of Book Covers](https://link.springer.com/article/10.1007/s42979-020-00132-z) which helped me compare my architecture to modified versions of pre-trained models. My dataset differs from the public dataset described in the paper in that my approach (1) trains on 5x less data and (2) contains additional text information. Although it is not an exact match, this paper helped me measure the effectiveness of my model with other approaches.

### Methodology
Methodology - what is your approach/solution/what did you do?

Since this is a mult-input problem, I thought it would be apt to use a CNN-like structure to handle book cover image input and an RNN-like structure for the book title input. I initially thought combining these would be difficult, but some prior work on multi-input models indicates that it is usually sufficient to just flatten the output of each of these architectures and concatenate them before passing them into a fully-connected layer.

Since training these can be difficult (as I am poor and bounded by the usage limits of Google Colab), I first experimented with models that only considered image data. Then, I experimented with models that only considered text data. Picking the best architecture that I found for each, I incorporated them into my final architecture. Finally, I ran a Bayesian hyperparameter search (evaluating against a validation set) to pick my hyperparameters.

### Experiments/Evaluation
Experiments/evaluation - how are you evaluating your results


### Results
Results - How well did you do


### Examples
Examples - images/text/live demo, anything to show off your work (note, demos get some extra credit in the rubric)

### Video

