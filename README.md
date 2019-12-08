# Analyzing the characteristics and global distribution of Data and AI startups

![startups](images/startups.jpeg)

I love startups and how there is no bound to what one could do to make this world a better place. And, since I work in Data and AI domain, I was more interested in exploring the Data and AI startups on AngelList. Gathering data from AngelList was not trivial as it would not let you export more than a 100 records at a go. So, I applied multiple filters, like Type=”Startup”, Market=”Big Data Analytics”, “Data Science”, “Big Data”, “Machine Learning, Predictive Modeling”, “Computer Vision”, “Natural language Processing”, “Neural Networks”, “Deep Learning”, “Reinforcement Learning” and then excluded the duplicates later as results of these filters were having an overlap in few operations. I got less than 100 records 26 times, hence created 26 .csv files. Finally, I merged these .csv files in one .csv using the following command and added header to the dataframe later.

sed 1d companies*.csv > startup.csv

This is a peek into the initial dataset.

df=pd.read_csv("startup.csv",names=["Name","Profile URL","Signal","Joined","Location","Market","Website","Employees","Stage","Total Raised"])


![peek_into_data](images/peek_into_data.png)

I worked on this dataset to split 'Joined' to 'Joining_Year' and 'Joining_Month' and dropped 'Joined' , appended '20' in front of year, removed duplicate rows using the following commands.

df.drop_duplicates(keep=False,inplace=True)
df[['Joining_Month','Joining_Year']]=df.Joined.str.split("'",expand=True)
df.drop(['Joined'], axis=1, inplace=True)
df['Joining_Year'] = ('20' + df['Joining_Year'])
df.head()

![peek_into_data](images/peek_into_data_2.png)

Using Counter from python, I found out top 10 startup markets using following code.

from collections import Counter
c=Counter(df.Market)
histo=c.most_common(10)
freq={}
for item,count in histo:
    freq[item]=count
explode = (0.05,0.05,0.05,0.05,0.05,0.05,0.05,0.05,0.05,0.05)
plt.pie([float(v) for v in freq.values()], labels=[(k) for k in freq],
           autopct='%.2f',startangle=90, pctdistance=0.85, explode = explode,radius=3,textprops={'fontsize': 14})
centre_circle = plt.Circle((0,0),0.90,fc='white')
fig = plt.gcf()
fig.gca().add_artist(centre_circle)
plt.tight_layout()
plt.show()


![top_10_startup_markets](images/top_10_startup_markets.png)
Top 10 Startup Markets in Data/AI. We see SaaS is the leader followed by 'Big Data Analytics', 'Predictive Analytics' and 'IoT'.
