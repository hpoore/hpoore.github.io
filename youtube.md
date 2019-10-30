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


### 4. Find Best Time of Day to Post a Video by Looking at Hour Posted and Total Views for Each Video

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
<img src="images/newplot(5).png?raw=true">

```python
for i in range(len(df)):
    hour_dict_views[df.iloc[i]['publish_time'][11:13]] += df.iloc[i]['views']

views_hour = list(hour_dict_views.keys())
views_count = list(hour_dict_views.values())
hour_views_df = pd.DataFrame()
hour_views_df['Hour'] = views_hour
hour_views_df['Number of Views'] = views_count

```
<img src="images/newplot(6).png?raw=true">






### 4. Calculate User Engagement Statistics

```python
avg_df = pd.DataFrame()
avg_df['Average Views'] = views
avg_df['Average Likes'] = avg_likes
avg_df['Average Dislikes'] = avg_dislikes
avg_df['Average Comments'] = avg_comments

avg_views = round(df['views'].mean(),0)
avg_likes = round(df['likes'].mean(),0)
avg_dislikes = round(df['dislikes'].mean(),0)
avg_comments = round(df['comment_count'].mean(),0)

avg_list = [avg_views,avg_likes,avg_dislikes,avg_comments]

avg_dict = {}
avg_dict['Average Views'] = avg_views
avg_dict['Average Likes'] = avg_likes
avg_dict['Average Dislikes'] = avg_dislikes
avg_dict['Average Comments'] = avg_comments

avg_df = pd.DataFrame()
avg_df = avg_df.from_dict(avg_dict, orient='index')


```
<img src="images/user_eng.png?raw=true">
