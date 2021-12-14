## Always Judge a Book By Its Cover

### Abstract
The well-known famous English idiom "you should never judge a book by its cover" applies to many things - but not books! Compelling cover art often reveals a lot about a novel while drawing in readers by piquing their interest. A natural question arises: can we train a machine to pick up on this subtle information on book covers in the ways that humans do?

Unfortunately, book covers don't make for good input to machine learning models. Book cover images vary too much in shape and size, while text on the cover (e.g. the title or author) can use uniquely different fonts. In comes Audible, an online audiobook and podcast streaming service. Audible helps match authors to professional narrators in order to convert books into audiobooks. During this process, they hire artists to modify book cover art into a square form. By scraping ~12,000 modified book covers and title text from Audible, we have created a dataset that we hope to use to "judge a book by its cover".

### Problem Statement
Given 256x256 colored images of book covers and a string representing the title of the book, predict which of 22 categories a book falls under in Audible.

Note: This is multi-class classification NOT multi-label classification, so each book only keeps the label of its alphabetically last category.

### Related Work
Related work - what papers/ideas inspired you, what datasets did you use, etc

![Image](10.jpg)


### Methodology
Methodology - what is your approach/solution/what did you do?


### Experiments/Evaluation
Experiments/evaluation - how are you evaluating your results


### Results
Results - How well did you do


### Examples
Examples - images/text/live demo, anything to show off your work (note, demos get some extra credit in the rubric)

### Video
Video - a 2-3 minute long video where you explain your project and the above information


### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/SohamPardeshi/490g1-final-project/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
