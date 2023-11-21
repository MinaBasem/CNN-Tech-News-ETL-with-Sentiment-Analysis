
<div align="center">
  <H1 style="font-size: 100;">CNN Business - Tech News ETL with Sentiment Analysis</H1>
  <img src="https://github.com/MinaBasem/CNN-Tech-News-ETL-with-Sentiment-Analysis/assets/42482261/ae9376ef-6246-485a-a29f-3358de960c8c" alt="Description of the image">
</div>

## Overview
An ETL script written in Python that extracts, transforms, and loads tech-themed headlines and their corresponding articles from [CNN Business - Tech](https://edition.cnn.com/business/tech), summarizes the articles, and applies sentiment analysis to them, while logging everything into a dataframe

## Final Result

A pandas dataframe is the final output, containing the following columns:
- **Company:** Lists the companies mentioned in the corresponding article
- **Headline:** Raw headline string
- **Article:** Raw article string
- **Article Sentiment:** Sentiment score of unmodified article
- **Article Sentiment Description:** Sentiment grading of unmodified article
- **Summary Sentiment:** Sentiment score of summaried article
- **Summary Sentiment Description:** Sentiment grading of summaried article 

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

## Process

## Extract Phase

### Extracting company names list

The `companies.csv` file provided includes a list of companies that could be potentially mentioned in the headlines

![Screen Shot 2023-11-21 at 4 51 54 PM](https://github.com/MinaBasem/CNN-Tech-News-ETL-with-Sentiment-Analysis/assets/42482261/cac80429-0c81-44b6-bc0a-cb9f0d1f0e82)

```
companies_from_csv = pd.read_csv('companies.csv')
companies_list = companies_from_csv['company'].tolist()
print(len(companies_list))
print(companies_list)
```
Company names are then stored into a list `companies_list`.

### BeautifulSoup

Data gets extracted from the CNN Tech page using BeautifulSoup
Specific sections of the HTML code obtained are filtered and a search for headlines and links is performed
Obtained headlines and links are then appended to 2 separate lists then filtered to remove unwanted characters or string
Headlines get matched with company names list to find which headlines reference a name from the list, and which get discarded
Company and Headline are tabbed into a dataframe

### Selenium

Selenium was mainly used to delay the extraction done by BeautifulSoup of the article page contents since the search results are not instantly loaded by default. 
[CNN News Search](https://edition.cnn.com/search?q=) is where each article was searched for as a GET query, sorted by reference.
After a delay of 10 seconds post search, BeautifulSoup was used to scrape the article and be placed back into the dataframe

## Transform Phase

### OpenAI API

The function nteracts with the OpenAI API to generate language-based responses. It takes a prompt as input (in this case a summarization prompt), sends a request to the OpenAI API, and returns the generated text as the output to be stored in a summaries list.

```
client = OpenAI(
  api_key = 'XXXXXXXXXXXXXXXXXXX',
)

def get_response(prompt): 
    completions = client.completions.create(
        model = "text-davinci-003",
        prompt = prompt,
        stream = False,
        max_tokens = 1024,
        n = 1,
        stop = None,
        temperature = 0.5,
    )
    message = completions.choices[0].text
    return message
```
Summaries are then ran through the function and stored into a list
The model `text-davinci-003` was used for this purpose.
Note: The API call limit is 3 requests per minute.

### Natural Language Toolkit (NLTK Library)

This block performs sentiment analysis on a list of articles using the VADER sentiment analysis tool from NLTK
For each article in the `articles_list`, analyze its sentiment using the SentimentIntensityAnalyzer and store the compound sentiment score in `article_sentiment_list`.
Sentiments are then categorized based on their compound score:
- If the score is greater than or equal to 0.05, label it as 'Good News' and sentiment as 'Positive'.
- If the score is less than or equal to -0.05, label it as 'Bad News' and sentiment as 'Negative'.
- Otherwise, label it as 'Neutral' and sentiment as 'Neutral'.

## Load Phase

The obtained dataframe is then appended into a csv file: `loaded_data.csv`

```
df.to_csv('loaded_data.csv', mode='a', header=False, index=False)
```

## The log file

Creates a log of runs along with their time and date, stores into `project_log.txt`.

```
timestamp_format = '%Y-%h-%d-%H:%M:%S'          # Year-Monthname-Day-Hour-Minute-Second 
now = datetime.now()                            # get current timestamp 
timestamp = now.strftime(timestamp_format) 
with open("project_log.txt", "a") as f: 
    f.write(timestamp + ': ' + "Data appended " + '\n') 
```

## Possible future additions

- Possibility of adding multiple news sources
- Possibility of eliminating the use of Selenium and merely extracting the headlines and their corresponding links into a dictionary in the extract phase
- Airflow or Cron modification to allow for automated running


