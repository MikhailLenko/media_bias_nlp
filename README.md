Acknowledgements:
- [Andrew Thompson](https://www.kaggle.com/snapcrack) for providing articles from genuine news sources in [All the news](https://www.kaggle.com/snapcrack/all-the-news)
- [Megan Risdal](https://www.kaggle.com/mrisdal) for providing articles from fake news sites in "[Getting Real about Fake News](https://www.kaggle.com/mrisdal/fake-news)"
- [Daniel Sieradski](https://github.com/selfagency) for identifying fake news sites, and for providing the [BS Detector](https://github.com/selfagency/bs-detector) that Megan used to assemble her fake news database

# Investigating media bias with natural language processing (NLP)

A major problem in contemporary political discourse is that many people consume news exclusively from sources that share their biases. This has contributed to the phenomenon of '[echo chambers](https://en.wikipedia.org/wiki/Echo_chamber_(media))', where different subsets of the population engage separate discursive loops of confirmation bias. The most extreme instance of this is '[fake news](https://en.wikipedia.org/wiki/Fake_news)', which is ...

[Natural language processing](https://en.wikipedia.org/wiki/Natural_language_processing) (NLP) is the domain in data science that models human languages. Common applications of NLP include:
- [speech recognition](https://en.wikipedia.org/wiki/Speech_recognition) (e.g., converting audible speech to text)
- [natural language understanding](https://en.wikipedia.org/wiki/Natural_language_understanding) (e.g., machine translation)
- [natural language generation](https://en.wikipedia.org/wiki/Natural_language_generation) (e.g., chatbots; note: this is the essence of the [Turing Test](https://en.wikipedia.org/wiki/Turing_test))

The purpose of this project is to use NLP to investigate the difference in political discourse by media outlets with various biases, and it has three main parts.

### Part I: Multiclass classification
Article titles and article bodies are grouped into four (4) categories based on their outlet:

| Label         | Outlets                | len(title_df)| len(body_df)|
|:--------------|:----------------------:|:------------:|:-----------:|
| Establishment | New York Times \| Washington Post \| CNN      | 1500         | 150         |
| Rightwing     | Fox News \| National Review \| Breitbart      | 1500         | 150         |
| Leftwing      | Guardian \| Atlantic \| Talking Points Memo   | 1500         | 150         |
| Fake          | 241 online publications* (e.g., InfoWars)     | 1500         | 150         |

\* Fake news online publications identified by [Daniel Sieradski](https://github.com/selfagency)'s [BS Detector](https://github.com/selfagency/bs-detector) Chrome extension, courtesy of [Megan Risdal](https://www.kaggle.com/mrisdal) who pulled 13,000 fake articles in the month prior to the 2016 U.S. presidential election for "[Getting Real about Fake News](https://www.kaggle.com/mrisdal/fake-news)"

Since these are balanced classes, baseline accuracy for classification is 25%.

By using [term frequency–inverse document frequency](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) (TF-IDF) vectorization and logistic regression with article bodies, classification accuracy attained 51.3%, or a 35% reduction in misclassification rate (75% —> 48.7%).

- TF-IDF Vectorizer was selected (and its parameters were tuned) by using Grid Search. (Count Vectorizer had similar bias but more evidence of overfitting.)
- Logistic regression was used so feature coefficients can be assessed (e.g., decision trees preclude such analysis)


### Part II: Binary classification
Article titles and article bodies are grouped into two (2) categories based on their outlet:

| Classification | Labels included                    | len(titles_df) | len(bodies_df) | 
|:---------------|:----------------------------------:|:--------------:|:--------------:|
| Real           | Establishment, Rightwing, Leftwing | 3000           | 300            |
| Fake           | Fake                               | 3000           | 300            |

Since these are balanced classes, baseline accuracy for classification is 50%.

By using [term frequency–inverse document frequency](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) (TF-IDF) vectorization and logistic regression with article bodies, classification accuracy attained 80.0% accuracy, or a 60% reduction in misclassification rate (50% —> 20.0%).

- TF-IDF Vectorizer was selected (and its parameters were tuned) by using Grid Search. (Count Vectorizer had similar bias but more evidence of overfitting.)
- Logistic regression was used so feature coefficients can be assessed (e.g., decision trees preclude such analysis)

### Part III: Word2Vec
[Word2Vec](https://en.wikipedia.org/wiki/Word2vec) is a two-layer neural network designed to learn [word embeddings](https://en.wikipedia.org/wiki/Word_embedding) of a corpus, which refers to internal relationships of the features of an NLP model. Two common forms of word embedding include continuous bag of words (CBOW) and skip-grams. Given some context words, CBOW outputs the word most associated with them; given some word, skip-grams outputs the context most associated with it. Skip-grams architectures are better suited for rare words but take longer to train.

Article bodies are grouped into distinct corpora for each category, and one aggregate corpora for 'real news' across bias spectrum. 

| Classification | Labels included                    | len(bodies_df) |
|:---------------|:----------------------------------:|:--------------:|
| Real           | Establishment, Rightwing, Leftwing | 9000           |
| Fake           | Fake                               | 9000           |
| Establishment  | Establishment                      | 9000           |
| Rightwing      | Rightwing                          | 9000           |
| Leftwing       | Leftwing                           | 9000           |