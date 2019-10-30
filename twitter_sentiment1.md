## Using Sentiment Analysis to Understand the Effect of News on the Stock Market

**Project description:** One of the homework assignments for my Information Exposition class required an exploratory data analysis of data collected by the student not previoulst published on the internet. This resulted in me using the Alphavantage API and BeautifulSoup4 to retrieve financial data and article text.

### 1. Using Alphavantage API retrieve stock data for each company 

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

```python
# get weekly stock price data for each company
msft = requests.get("https://www.alphavantage.co/query?function=TIME_SERIES_WEEKLY&symbol=MSFT&apikey=M5BAAOQ3PWDT935S&datatype=csv").content
msft_df = pd.read_csv(io.StringIO(msft.decode('utf-8')))
# https://stackoverflow.com/questions/39213597/convert-text-data-from-requests-object-to-dataframe-with-pandas
msft_df.head()
```
<img src="images/msft_df.png?raw=true"/>

### 2. Creating DataFrame of Stock Prices 
```python
df_list = [aapl_df, amzn_df, fb_df, googl_df, msft_df]

row = 0
for i in df_list:
    temp_df = i.copy()
    temp_df = temp_df.drop(['timestamp', 'high', 'low', 'volume'],axis=1)
    info = list(temp_df.loc[1])
    change_pct = (info[1] - info[0]) / info[1]
    info.append(change_pct)
    stock_df.loc[names[row]] = info
    row += 1
    info = []
```
<img src="images/stock_df.png?raw=true"/>
### 3. Support the selection of appropriate statistical tools and techniques
```python
rcParams['figure.figsize'] = 11.7,8.27
y = list(stock_df['Change Percentage'])
x = list(stock_df.index)
sns.set(style='whitegrid')
plt.xlabel('Stock')
plt.ylabel('Percent Change')
plt.title('Stock Price Change Percentage from 10/7/19 to 10/11/19')
ax = sns.barplot(x=x, y=y,palette="GnBu_d")
```

<img src="images/stock_graph.png?raw=true"/>


### 4. Create Web Scraper to Extract Article Data

I created a crawler like object that extracted article text from the CNBC website.
Attatched below is the code to get article data for Apple using BeautifulSoup4.
```python
aapl_links = ['https://www.cnbc.com/2019/10/09/apple-augmented-reality-glasses-to-launch-in-2020-kuo.html'
              ,'https://www.cnbc.com/2019/10/07/apple-macos-catalina-released-for-macs-whats-new-and-how-to-get-it.html',
            'https://www.cnbc.com/2019/10/11/apple-aapl-hits-all-time-high.html']
aapl_list = []
for i in aapl_links:
    temp_list = []
    page = requests.get(i).text
    soup = BeautifulSoup(page, 'html.parser')
    a = soup.find_all(class_='group')
    for strong_tag in a:
        temp_list.append(strong_tag.text.encode('ascii', 'ignore').decode("utf-8").strip().replace("\"",''))
        aapl_str = ' '.join(temp_list)
    aapl_list.append(aapl_str)
       
```
 
 ### 5. Use VADER to Generate Sentiment Scores for Article Text
 ``` python
 aapl_avg = 0 
for i in range(len(aapl_list)):
    analyzer = SentimentIntensityAnalyzer()
    vs = analyzer.polarity_scores(aapl_list[i])
    aapl_avg += vs['compound']
    scores = list(vs.values())
    scores_df.loc[article_titles[i]] = scores
aapl_avg = aapl_avg / 3

j = 3
amzn_avg = 0 
for i in range(len(amzn_list)):
    analyzer = SentimentIntensityAnalyzer()
    vs = analyzer.polarity_scores(amzn_list[i])
    amzn_avg += vs['compound']
    scores = list(vs.values())
    scores_df.loc[article_titles[j]] = scores
    j += 1
amzn_avg = amzn_avg / 3

j = 6 
fb_avg = 0
for i in range(len(fb_list)):
    analyzer = SentimentIntensityAnalyzer()
    vs = analyzer.polarity_scores(fb_list[i])
    fb_avg += vs['compound']
    scores = list(vs.values())
    scores_df.loc[article_titles[j]] = scores
    j+=1
fb_avg = fb_avg / 3

j = 9 
googl_avg = 0
for i in range(len(googl_list)):
    analyzer = SentimentIntensityAnalyzer()
    vs = analyzer.polarity_scores(googl_list[i])
    googl_avg += vs['compound']
    scores = list(vs.values())
    scores_df.loc[article_titles[j]] = scores
    j+=1
googl_avg = googl_avg / 3

j = 12
msft_avg = 0
for i in range(len(msft_list)):
    analyzer = SentimentIntensityAnalyzer()
    vs = analyzer.polarity_scores(msft_list[i])
    msft_avg += vs['compound']
    scores = list(vs.values())
    scores_df.loc[article_titles[j]] = scores
    j+=1
msft_avg = msft_avg / 3
 
 ```
 
 <img src="images/sentiment_df.png?raw=true"/>
 
 ### 6. Visualize results
 ```python
 for i,type in enumerate(names):
        y = percentages[i]
        x = averages[i]
        plt.scatter(x, y,color='blue')
        plt.text(x, y, type, fontsize=12)
plt.xlabel('Average Compound Article Sentiment Scores')
plt.ylabel('Stock Price Change Percentage')
plt.title('Average Compound Article Sentiment Score vs Stock Price Change Percentage for 10/7/19 - 10/11/19')
 
 ```
<img src="images/sentiment_graph.png?raw=true"/>


### Analysis Explained More In Depth in Medium Article
<a href="https://medium.com/@haydenpoore/using-sentiment-analysis-to-understand-the-effect-of-news-on-the-stock-market-c346dc8c5a90"> Link </a?>

