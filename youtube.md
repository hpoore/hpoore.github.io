## How Does YouTube's Trending Videos Algorithm Work?

**Project description:** An exploratory data analysis on Youtube's Trending Videos algorithm. Data set was found on kaggle. I looked at various factors that could affect the chances of a video becoming trending such as sentiment of title, time posted and user engagement. 

### 1. Read in Data

```python
df = pd.DataFrame()
df = pd.read_csv('USvideos.csv')
df.head()
```
<img src="images/youtube_df1.png?raw=true"/>

### 2. Calculate Sentiment of Video Titles

```python
title_list = df['title']
score_list = []
for title in title_list:
    temp_score_dict = analyzer.polarity_scores(title)
    score_list.append(temp_score_dict['compound'])
```
<img src="images/title_df.png?raw=true">

### 3. Support the selection of appropriate statistical tools and techniques

<img src="images/dummy_thumbnail.jpg?raw=true"/>

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

