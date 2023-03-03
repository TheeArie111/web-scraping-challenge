# Module 12 Challenge
## Web Scraping

# Background
You’re now ready to take on the full web-scraping and data analysis project for the mission to Mars. You’ve learned to identify HTML elements on a page, identify their id and class attributes, and use this knowledge to extract information via both automated browsing with Splinter and HTML parsing with Beautiful Soup. You’ve also learned to scrape various types of information. These include HTML tables and recurring elements, like multiple news articles on a webpage.

As you work on this Challenge, remember that you’re strengthening the same core skills that you’ve been developing until now: collecting data, organizing and storing data, analyzing data, and then visually communicating your insights.

## Deliverables:
* Scrape titles and preview text from Mars news articles
* Scrape and analyze Mars weather data from a table

# Deliverable 1: Scrape News Titles and Preview Text
## Create a Beautiful Soup object and use it to extract text elements
```python
# Create a Beautiful Soup object
# load browser and parse

html = browser.html
soup = bs(html, 'html.parser')
type(soup)

bs4.BeautifulSoup
```
![](Screenshot%202023-03-03%20132703.jpg)
## Extract titles and preview text of the news articles
```python
# Extract all the text elements
# content_title
# article_teaser_body

titles = soup.find_all('div', class_ = 'content_title')
previews = soup.find_all('div', class_ = 'article_teaser_body')
```

## Prepare to store each title-preview pair in a Python dctionary
* Step 1: 
```python
# Create Empty lists to store text elements
news_titles = []
news_previews = []

# Create empty list for dictionaries
list_of_dict = []
```
* Step 2: 
```python
# Loop through the title text elements

for title in titles:
    print('-------------')
    print(title.text)
    news_titles.append(title.text)
```

```python
# Loop through preview elements

for preview in previews:
    print('-------------')
    print(preview.text)
    news_previews.append(preview.text)
```

## Store dictionaries in a list using for loop
```python
# for loop 

number_of_stories = len(news_titles)
for i in range(number_of_stories):
        list_of_dict.append({'title': news_titles[i], 'preview': news_previews[i]})
```
```python
# print to confirm success
list_of_dict

[{'title': "NASA's Perseverance Rover Will Carry First Spacesuit Materials to Mars",
  'preview': 'In a Q&A, spacesuit designer Amy Ross explains how five samples, including a piece of helmet visor, will be tested aboard the rover, which is targeting a July 30 launch. '},
 {'title': 'Naming a NASA Mars Rover Can Change Your Life',
  'preview': 'Want to name the robotic scientist NASA is sending to Mars in 2020? The student who named Curiosity — the rover currently exploring Mars — will tell you this is an opportunity worth taking.'},
 {'title': 'NASA Establishes Board to Initially Review Mars Sample Return Plans',
  'preview': 'The board will assist with analysis of current plans and goals for one of the most difficult missions humanity has ever undertaken.'},
 {'title': "How NASA's Mars Helicopter Will Reach the Red Planet's Surface",
  'preview': 'The small craft will seek to prove that powered, controlled flight is possible on another planet. But just getting it onto the surface of Mars will take a whole lot of ingenuity.'},
 {'title': "NASA's Curiosity Keeps Rolling As Team Operates Rover From Home",
  'preview': 'The team has learned to meet new challenges as they work remotely on the Mars mission.'},
 {'title': "Screening Soon: 'The Pathfinders' Trains Lens on Mars",
  'preview': 'With the Mars 2020 mission ramping up, the documentary — the first of four about past JPL missions to the Red Planet to be shown at Caltech — tells a gripping backstory.'},
 {'title': "NASA's Perseverance Mars Rover Gets Its Wheels and Air Brakes",
  'preview': 'After the rover was shipped from JPL to Kennedy Space Center, the team is getting closer to finalizing the spacecraft for launch later this summer.'},
 {'title': "NASA's Treasure Map for Water Ice on Mars",
  'preview': 'A new study identifies frozen water just below the Martian surface, where astronauts could easily dig it up.'},
 {'title': "The Launch Is Approaching for NASA's Next Mars Rover, Perseverance",
  'preview': "The Red Planet's surface has been visited by eight NASA spacecraft. The ninth will be the first that includes a roundtrip ticket in its flight plan. "},
 {'title': "NASA's Ingenuity Mars Helicopter Recharges Its Batteries in Flight",
  'preview': 'Headed to the Red Planet with the Perseverance rover, the pioneering helicopter is powered up for the first time in interplanetary space as part of a systems check.'},
 {'title': 'Two Rovers to Roll on Mars Again: Curiosity and Mars 2020',
  'preview': 'They look like twins. But under the hood, the rover currently exploring the Red Planet and the one launching there this summer have distinct science tools and roles to play.'},
 {'title': "Curiosity Mars Rover's Summer Road Trip Has Begun",
  'preview': 'After more than a year in the "clay-bearing unit," Curiosity is making a mile-long journey around some deep sand so that it can explore higher up Mount Sharp.'},
 {'title': "NASA's Mars Helicopter Attached to Mars 2020 Rover ",
  'preview': 'The helicopter will be first aircraft to perform flight tests on another planet.'},
 {'title': 'Mars 2020 Stands on Its Own Six Wheels',
  'preview': "In time-lapse video, taken at JPL, captures the first time NASA's Mars 2020 rover carries its full weight on its legs and wheels."},
 {'title': "NASA's InSight Flexes Its Arm While Its 'Mole' Hits Pause",
  'preview': "Now that the lander's robotic arm has helped the mole get underground, it will resume science activities that have been on hold."}]
  ```

  # Deliverable 2: Scrape and Analyze Mars Weather Data
  ## Create Beautiful Soup object and scrape HTML table, assemble scraped data into Pandas Data Frame
  ```python
html = browser.html
soup = soup(html, 'html.parser')
mars_table = soup.find('table', class_ = 'table')
mars_table
```

```python
# Create a Pandas DataFrame by using the list of rows and a list of the column names
mars_weather_df = pd.DataFrame(columns=mars_weather)
for row in mars_table.find_all('tr',class_='data-row'):
        data = row.find_all('td')
        row_data=[td.text.strip() for td in data]
        lenghth=len(mars_weather_df)
        mars_weather_df.loc[lenghth]=row_data

mars_weather_df.head()

	id	terrestrial_date	sol	ls	month	min_temp	pressure
0	2	2012-08-16	10	155	6	-75.0	739.0
1	13	2012-08-17	11	156	6	-76.0	740.0
2	24	2012-08-18	12	156	6	-76.0	741.0
3	35	2012-08-19	13	157	6	-74.0	732.0
4	46	2012-08-20	14	157	6	-74.0	740.0
```

  ## Convert data types into appropriate types
```python
# Change data types for data analysis
mars_weather_df['id']=mars_weather_df['id'].astype(int)
mars_weather_df['terrestrial_date']=pd.to_datetime(mars_weather_df['terrestrial_date'])
mars_weather_df['sol']=mars_weather_df['sol'].astype(int)
mars_weather_df['ls']=mars_weather_df['ls'].astype(int)
mars_weather_df['month']=mars_weather_df['month'].astype(int)
mars_weather_df['min_temp']=mars_weather_df['min_temp'].astype(float)
mars_weather_df['pressure']=mars_weather_df['pressure'].astype(float)

mars_weather_df.dtypes

id                           int32
terrestrial_date    datetime64[ns]
sol                          int32
ls                           int32
month                        int32
min_temp                   float64
pressure                   float64

```


  ## Analyze dataset using Panadas functions
  * How many months exist on Mars?
  ```python
  months=mars_weather_df['month'].nunique()
print(f' There are {months} months on Mars.')

There are 12 months on Mars.
```
  * How many Martian days are in the dataset?
```python
days=mars_weather_df['sol'].nunique()
print(f' There are {days} Martian days worth of data.')

There are 1867 Martian days worth of data.
```

  * What are the coldest and warmest months on Mars recorded by Curiosity?:
    - Find the avg minimum daily temps
    - Plot the results as a bar chart
  ```python
  	month	min_temp
0	1	-77.160920
1	2	-79.932584
2	3	-83.307292
3	4	-82.747423
4	5	-79.308725
5	6	-75.299320
6	7	-72.281690
7	8	-68.382979
8	9	-69.171642
9	10	-71.982143
10	11	-71.985507
11	12	-74.451807
```
![](Screenshot%202023-03-03%20160145.jpg)

```python
The coldest month was the 3rd and the hottest month was the 8th.
```

  * Which months have the lowest and highest atmospheric pressure?
    - Find the avg daily atmospheric pressures
    - Plot the results as a bar chart
```python
	month	Average Pressure
0	1	862.488506
1	2	889.455056
2	3	877.322917
3	4	806.329897
4	5	748.557047
5	6	745.054422
6	7	795.105634
7	8	873.829787
8	9	913.305970
9	10	887.312500
10	11	857.014493
11	12	842.156627
```
![](Screenshot%202023-03-03%20160627.jpg)
* About how many Earth days exist in a Martian year?
    - Consider how many days elapse on Earth in the time that Mars circles the Sun once
    - Visually estimate the result by plotting the daily minimum temperature
    
![](Screenshot%202023-03-03%20160840.jpg)

