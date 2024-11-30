# `vader-sentiment-cpp`

## Quick Start
**`vader-sentiment-cpp`** is a fast, C++ port of the **Python NLTK Vader Sentiment Analyzer**, optimized for speed. This version retains key functionality while being lighter and quicker, ideal for performance-sensitive applications.

## Approach
This implementation mimics the core sentiment analysis features of the original model using a sentiment lexicon to classify text as positive, negative, or neutral.

### 🔧 Example Usage

#### 1. **Loading the Lexicon**
Load the **`lexicon.txt`** file into an unordered map to store words and their sentiment scores.

```c++ 
std::ifstream lexicon("resources/lexicon.txt");
std::unordered_map<std::string, float> lexicals;
if (lexicon) {
    std::string line;
    while (std::getline(lexicon, line)) {
        uint16_t pos = 0, last = 0;
        pos = line.find("\t", last);
        std::string word = line.substr(last, pos - last);
        last = pos + 1;
        pos = line.find("\t", last);
        float val = std::stof(line.substr(last, pos - last));
        lexicals[word] = val;
    }
    lexicon.close();
} else throw std::runtime_error("[Vader](ERROR) Cannot open lexicon.txt");
```

#### 2. **Storing Review Data**
Define a **`Review`** struct to store review details: ID, rating, and text.

```c++ 
struct Review {
    uint16_t m_rating;
    uint64_t m_id;
    std::string m_corpus;

    Review(uint16_t rating, uint64_t id, std::string corpus) :
        m_id(id), m_rating(rating), m_corpus(corpus) {};
};
```

#### 3. **Storing Review Data**
Load data from the **`reviews.csv`** file into a vector of **`Review`** objects.

```c++
std::vector<Review> revs;
std::ifstream file("resources/reviews.csv");
if (file) {
    std::string line;
    std::getline(file, line); // Skip header
    while (std::getline(file, line)) {
        uint16_t pos = 0, last = 0;
        pos = line.find(',', last);
        uint32_t id = std::stoi(line.substr(last, pos - last));
        last = pos + 1;
        pos = line.find(',', last);
        uint16_t rating = std::stoi(line.substr(last, pos - last));
        last = pos + 1;
        revs.emplace_back(rating, id, line.substr(last, pos - last));
    }
} else {
    throw std::runtime_error("[Vader](ERROR) Cannot open reviews.csv");
}
```

#### 4. **Evaluating Sentiment**
Evaluate the sentiment of the first review using **`vader::eval`**.

```c++
vader::eval(revs[0].m_corpus, lexicals);
```

## Citation
**Hutto, C.J. & Gilbert, E.E. (2014). VADER: A Parsimonious Rule-based Model for Sentiment Analysis of Social Media Text. Eighth International Conference on Weblogs and Social Media (ICWSM-14). Ann Arbor, MI, June 2014.** 


