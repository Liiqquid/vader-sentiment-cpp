## Vader-Lite (C++)
### Quick Start
***C++*** port of ***Python NLTK Vader Sentiment Analyzer***. The workflow used is a *faster* variation of the original library which reproduce seemingly results. 

### Approach
An example of usage of the function `vader::eval`, using the data provided in the resources folder **resources**,
```c++ 
// Open the lexicon.txt file;
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
}
else throw std::runtime_error(
	"[Vader](ERROR) Cannot open lexicon.txt");
```

Creating a *struct* to store the content of each row of the *reviews.csv* file,

```c++ 
struct Review {
	uint16_t m_rating;
	uint64_t m_id;
	std::string m_corpus;

	Review(
		uint16_t rating,
		uint64_t id,
		std::string corpus
	) :
		m_id(id),
		m_rating(rating),
		m_corpus(corpus) {};
};
```
Then storing *reviews* data into a `std::vector<Review>`,
```c++
// Opening the reviews.csv;
std::vector<Review> revs;
std::ifstream file("resources/reviews.csv");
if (file) {
	std::string line;
	std::getline(file, line);
	while (std::getline(file, line)) {
		uint16_t pos = 0, last = 0;
		pos = line.find(',', last);
		uint32_t id = std::stoi(line.substr(last, pos - last));
		last = pos + 1;
		pos = line.find(',', last);
		uint16_t rating = std::stoi(line.substr(last, pos - last));
		last = pos + 1;
		revs.emplace_back(
			std::move(rating),
			std::move(id),
			std::move(line.substr(last, pos - last))
		);
	}
}
else throw std::runtime_error(
	"[Vader](ERROR) Cannot open reviews.csv");
```

Then in order to evaluate the first review with ***Vader***,
```c++ 
vader::eval(revs[0].m_corpus, lexicals);
```
