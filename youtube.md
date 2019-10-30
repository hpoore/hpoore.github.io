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

### 3. Visualize Sentiment Scores
<img src="images/sent_graph.png?raw=true"/>


### 4. Find Best Time of Day to Post a Video

```python
time_list = list(df['publish_time'])
hour_list = []

for i in range(len(time_list)):
    if len(time_list[i]) != 24:
        print('broken')
        break
    else:
        hour_list.append(time_list[i][11:13])
        
hour_dict = {}
hour_set = set(hour_list)
hour_dict = hour_dict.fromkeys(hour_set,0)
hour_dict_views = hour_dict.fromkeys(hour_set,0)

for key in hour_dict:
    for i in range(len(hour_list)):
        if hour_list[i] == key:
            hour_dict[key] += 1

hour_freq = list(hour_dict.values())
hours = list(hour_dict.keys())
hour_freq_df = pd.DataFrame()
hour_freq_df['Hour'] = hours
hour_freq_df['Occurances'] = hour_freq
```
<img src="images/new_plot(5).png?raw=true">

### 4. Provide a basis for further data collection through surveys or experiments

Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. 

