# CNN Tech News ETL with Sentiment Analysis

## Summary
An ETL script written in Python that extracts, transforms, and loads tech-themed headlines and their corresponding articles from [CNN Business - Tech](https://edition.cnn.com/business/tech), summarizes the articles, and applies sentiment analysis to them, while logging everything into a dataframe

## Final Result

A pandas dataframe is the final output, containing the following columns:
- **Company:** Lists the companies mentioned in the corresponding article
- **Headline:** Raw headline string
- **Article:** Raw article string
- **Article Sentiment:** Sentiment of article without modification
- **Article Sentiment Description:** 

![Screen Shot 2023-11-20 at 4 57 57 PM](https://github.com/MinaBasem/CNN-Tech-News-ETL-with-Sentiment-Analysis/assets/42482261/673af86e-51cd-45b0-b224-bef0e27689bc)

## Libraries
```
BeautifulSoup
requests
Pandas
NumPy
datetime
Selenium
time
openAI
```
Can also be found in requirements.txt
