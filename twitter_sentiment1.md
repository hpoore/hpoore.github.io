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

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
