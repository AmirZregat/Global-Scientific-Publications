# Global Scientific Publications
This repository is a data mining project that scrape and analyzed data from the [SCImago Journal & Country Ranking dataset](http://www.scimagojr.com/countryrank.php).

The objective of this project is to analyze and visualize the change of number and trend of the publications among countries and fields for the past two decades (1996 - 2016).

## About the data source
The SCImago Journal & Country Rank is a publicly available portal that includes the journals and country scientific indicators developed from the information contained in the Scopus® database (Elsevier B.V.). 

SCImago is a research group from the Consejo Superior de Investigaciones Científicas (CSIC), University of Granada, Extremadura, Carlos III (Madrid) and Alcalá de Henares, dedicated to information analysis, representation and retrieval by means of visualisation techniques.

## Python scripts
The project use `BeautifulSoup`, `pandas`,`requests`,`numpy`,`matplotlib` and `math` packages. Please refer to the [jupyter notebook](/DataMining_visualization.ipynb)

The raw data can be obtained from the SJR website with interative dropdown menu selections. Considering the length of time and number of publicaiton areas and countries to explore and download, interacting with the website does not seem so fun, though~ So, simple steps of web scrpaing can be done by `BeautifulSoup` and `requests`. A quick example of the chunk of codes that scrapes the raw data:

```
deposit = pd.DataFrame()
for year in yr:
    for area in ar:
        url = 'http://www.scimagojr.com/countryrank.php?'+year+'&'+area
        with requests.Session() as session:
            buffer = []
            session.headers['content-type'] = 'application/json'
            response = session.get(url)
            soup = BeautifulSoup(response.content, "lxml")
            for mytable in soup.find_all('tbody'):
                for trs in mytable.find_all('tr'):
                    tds = trs.find_all('td')
                    row = [elem.text.strip() for elem in tds]
                    buffer.append(row)
            buffer = pd.DataFrame(buffer)
            buffer.drop(buffer.columns[1], axis=1, inplace = True)
            buffer.insert(0,'Year', [year[-4:]]*len(buffer))
            buffer.insert(0,'Area', [area[-4:]]*len(buffer))
        deposit = deposit.append(buffer)
```
The rest of the scripts were wrangling with scraped data. If interested, please check out the jupyter notebook file in this repository.
## The story
### What are the hot topics in research publications? 
Here are the contrast between the two decades (1996-2006 vs. 2007-2016).

<img src="images/WorldPubs_areaDecade1.png" width="400"><img src="images/WorldPubs_areaDecade2.png" width="400">

There are some changes in the ranking of total number of publications by areas between the two decades, of course. Not surprising, with the continued prosperity of information technology and whole lot of internet-based buisness and applications, publications related to computer science was catching up pretty fast. However, some of the traditionally "publication heavy" areas such as medicine, engineering and physics remained on top of the ranking

### How about the trend?
Again, not suprising that the number of publications in each field has been steadily increasing, even veterinery, which was at the bottom of the "hot topic" list.

<img src="images/PubTrend.png" width="500">

### Another field of competition for big countries?
Scientific publications are not full reprensentation of the level of science and technology research, but to large extent, they suggest the investment and dedication of a country to fundamental and applied research. 
The United States has been the absolute leading force in the number of publications of every field. If we just compare the total number of publications for the past two decades, the US published as many research articles as the sum of China, the United Kingdom and Germany. However, this is slowly changing. China picked up the pace and is rapid catching up with the US in many areas *See below: number of the publications by country in 1996(left) and 2016(right))*

<img src="images/TotPub_bubble1996.png" width="400"><img src="images/TotPub_bubble2016.png" width="400">

### Shape of research
![](images/SR_US.gif)
<img src="images/SR_US.gif" width="400">
