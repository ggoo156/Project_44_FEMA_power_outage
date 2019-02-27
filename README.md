# Project 4: FEMA Power Outage Hotspot Tool


### Forward
The goal of this project is to geolocate power outages in the United States using social media data.  Within this repository, you will find a documented process for sourcing, cleaning, modeling, and interpreting Twitter data that in a process that accomplishes this stated goal.  The models and methods contained here are intended for use by FEMA for disaster relief within the United States, but we invite and encourage any organization that provides emergency relief to freely make use of this repository.  


Your partners in Data-Driven Disaster Relief;

[Sung Won (Chris) Lee](https://www.linkedin.com/in/sung-won-chris-lee), [Adiwid Devahastin Na Ayudhya](https://www.linkedin.com/in/boom-devahastin), and [Henry Blais](https://www.linkedin.com/in/henry-blais).

In cooperation with [GeneralAssembly](https://generalassemb.ly); January 2019.


### Project Files and Workflow
- [1.0 Scraping Twitter Data](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/1_Data-Collection.ipynb),
- [2.0 Data Cleaning Process](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/2_Data_Cleaning.ipynb),
- [3.0 Exploratory Data Analysis](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/3_EDA.ipynb),
    - [3.1 Steps for Improving Scraping Keywords](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/3b_Keywords.ipynb),
- [4.0 Geographically Visualizing Power Outages](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/4.%20Visualizing%20-%20Bokeh.ipynb),
- [5.0 Natural Language Processing and Modeling](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/4_NLP-Modeling.ipynb),
- [6.0 Presentation Slides](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/Project%20%234%20Slides.pptx)

# Executive Summary

### Introduction
Social Media data is an endless source of insight for Data Scientists, with a myriad of noble (or nefarious) applications.  Our team resolved to tap into this wealth of information, and use it to create a tool to assist disaster relief efforts in the United States (although a savvy user could apply our method to any region in the world!). 

### Table of Contents
In the remainder of this README file, we will outline the basic steps of our process:
1. Data Collection
2. Data Cleaning
3. Exploratory Data Analysis
4. Visualization and Mapping
5. Processing and Modeling
6. Proposed Next Steps

### 1. Data Collection
We resolved to use Twitter data for this version of this project.  In order to collect this data, we used a tried-and-true scraping method, purpose built into a scraping function for our data collection efforts in [1.0 Scraping Twitter Data](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/1_Data-Collection.ipynb).

The basic theory of our scraping function is very simple; in practice we encountered avoidable setbacks that I will advise you on below.  Our scraper is an implementation of the excellent [twitterscraper](https://pypi.org/project/twitterscraper/0.2.7/) package, tuned to return a body of tweets containing specific words or terms, and posted within a specified time frame.  Both the keywords and timeframe are specified as arguments of the twitterscraper.query_tweets function.

Our basic scraper relies on very few prefiltering arguments and keywords by design.  In future, it might be valuable to revise this and test the impact of tuning for more specific tweets, but this will be discussed later in "Proposed Next Steps". 

As I alluded to; our team encountered scraping issues with [this](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/1_Data-Collection.ipynb) scraping function.  Specifically, we very quickly overtaxed the Twitter API and were suspended from pulling more tweets, typically for a period of 2 or more days at a time.  This shouldn't be a big issue for a user who runs this scraper only once, but if you were to repeatedly use it, say, in the process of refining the prefiltering terms for the scraper, you will quickly be banned from acquiring new data - be warned.  

### 2. Data Cleaning
In order to format our data into a useful and uniform set, we used a concise [cleaning process](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/2_Data_Cleaning.ipynb), and added some new data of our own.  

The most noteworthy development in our Data Clean deserves some explanation here.  While working on this project, our team quickly learned that Twitter does NOT keep usable location data in their html structure or json output.  While the Twitter API does include fields to store the city of origin, account owner location, and even latitude and longitude of the user at the time a tweet is posted, these fields are almost universally blank or unhelpful.  This is probably either because Twitter had deprecated location tracking, or many users choose to opt out, or a combination of these and other factors.  Suffice it to say that the location data was so incomplete in our scraped data, we are forced in this section to randomly impute location data as a proof-of-concept.  Consequently, the conclusions and data drawn from this point forward are purely demonstrative and are not informed by actual geographical data.  

The final step of our data cleaning process was to combine the scraped tweets and imputed cities with a dataframe of all USA cities, including their coordinates, counties, et cetera.  This data will help us in mapping and other efforts later on. 

### 3. Exploratory Data Analysis
After cleaning our data, it's important to explore the characteristics of that data and build an understanding of it.  Our [EDA](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/3_EDA.ipynb) file begins the EDA process by;
1. Reformatting the text of all scraped tweets
    - Removal of unusual characters/punctuation
    - Handling misspelled words
    - Removing 'stopwords' (ie; he, the, it, is...) not helpful to text analysis
2. CountVectorizing the text column to facilitate term frequency analysis
3. Analyzing word frequency and use for further insights.

This process helped us to identify new keywords that might be helpful, eliminate some that were not, and pointed out some important improvements we could make in the future, which will be outlined later on.  

### 4. Visualization and Mapping
In this section, we used the geodata we added and imputed in Data Cleaning, to generate geo-heatmaps that would help to identify "hotspots" of flagged power-outage related twitter chatter.  Our [mapping section](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/4.%20Visualizing%20-%20Bokeh.ipynb) uses [Bokeh](https://bokeh.pydata.org/en/latest/), a really terrific visualization tool. 

Bokeh was useful here because we were able to create an interactive map that we believe will be of more use to our users than a static plot might've been.  In future updates, we envision a re-arrangement of our program that would enable very local searches for power-outage hotspots (for instance, within a county).  In this framework, the interactive mapping we've implemented will be useful for exploring the shape and extent of identified power outages.  

### 5. Processing and Modeling
Our Processing and Modeling workflow is laid out and explained in great detail [here](https://git.generalassemb.ly/boom-deva/project-4-FEMA-power-outages/blob/master/4_NLP-Modeling.ipynb), and I highly recommend reading through it on a step-by-step basis within that file; but the overview of this process is very simple.

As we've discussed before, our current working data does not include meaningful geographical information for individual tweets, which prevents us from empirically identifying tweets that occurred during blackouts.  Without a confirmed test set containing this information, our modeling options were extremely limited.  

We opted to implement a sentiment analysis through Natural Language Processing Classification.  NLPC operates by measuring cosine similarity - in this case we used similarity of chosen keywords, associated with power outages, from the tweets themselves.  In order to generate these cosines, we first tokenized the tweets title strings, then vectorized them to create a usable numeric representation of those words that NLP could be applied to (I'm not sure what the cosine of "blackout" is, after all).  

Our NLPC model was able to generate meaningful predictions of power-outage-likelihood for each individual tweet, based on the language and 'sentiment' used in the tweets themselves.  

### 6. Proposed Next Steps
What couldn't we accomplish, given all the time in the world!  Sadly projects can't go on forever, and what is contained in this repo represents only a current working model - a prototype of what we believe could be an immensely helpful tool for disaster relief groups.

First, lets take stock of what our program does currently.  Front-to-end, this program scrapes a large body of tweets, compares their language with a list of terms we generated that relate to power outages, and then classifies the tweets in that body into 2 groups - 'power outage' and 'no power outage' -  based on Natural Language Processing Classification.  This process is sound, but it suffers from a number of shortcomings that can be addressed through a series of proposed updates below.  We offer the current version of this repository as a sort of sample; a proof-of-concept that can be built out into a much more robust tool with the following additions:

#### A. General
All the steps we've taken and outlined above are intended to create a working model of twitter data.  This is a desirable goal of course, but that goal was badly undermined by the absence of location data within the twitter API. 

Furthermore, generating that model is only an intermediary step towards the finished product we hope to build.  In our finished product, we will generate a supervised, rather than unsupervised, learning model that can accurately differentiate tweets originating  from blacked-out cities and neighborhoods.  This will require past tweets that we can confirm originated in blackout zones, which is an attainable goal (more on this below).  

Rather than stopping at this point, the model described above will become the working core that enables us to build on more useful features.

#### B. Restructure of Scraping Function /// Implement Core Model
The revised model proposed in "Next Steps: A" will require a large initial scrape of twitter data, ideally in the hundreds of thousands of posts, if not more.  This body of tweets will benefit from improvements to our scraping methods, which I will outline now.  

The large initial scrape ('Scrape A') will not be limited by keywords in the future, but will instead depend entirely on windows of time which we will procedurally generate.  These windows of time will correspond to the start time and duration of known past power outages throughout the United States; this data is easily available through the [websites](https://apps.coned.com/stormcenter/external/default.html) of all major power providers by law.  

The revised scraper will pull tweets only during periods where there is one or more active power outage in the United States (we would most likely filter these outage periods to exclude outages with low numbers of effected people, another statistic tracked in the link above), so that we get a mixture of tweets from affected areas ('Targets') and from non-affected areas ('Non Targets').  Then, that data will be used to build a core Dataset and core model ('Core') that later steps will rely on.  

#### C. Establishing Location to enable Supervised Learning
There are some simple steps we can take to ensure the data we're working with comes from a known location.  The first, best, and most obvious one comes straight out of the social media playbook: we sit back and let Twitter users identify themselves for us.  

This proposed feature is an upgrade to the Scrape A above, as well as to smaller scrapes that we will propose and discuss below.  In brief; there is no shortage of tweets on out there on the internet, so we can afford to be very choosy about which ones we scrape up.  

What we could do is add on a second and completely separate prefiltering rule for all scraping functions moving forward, so that a City name AND one of our keywords becomes the prerequisites for our scraper.  

This would limit all returned tweets to those that are the most likely to self-identify their location.  For instance, rather than getting huge numbers of tweets reading something like;

"Oh man my power is out!  Lame!"

we would instead probably get a body of tweets reading more to the tune of; 

"Big power outage in Boston tonight, I can't watch the game!", or even,
"Boston lost power today, serves them right, lousy Red Sox"

The first tweet tells us nothing about it's origin, making it useless, but the second tweet self-identifies location, and the third tweet refers us to a remote blackout location that the user knows of.  In either case, we have a lot more to go on that we started with.

From here we can actually do a lot more to confirm the location of a referenced outage.  Specifically, we can use some of Twitter's helpful metadata; the User Time Zone and Time Posted portions of the Twitter API.  If we include these as features in our scraped data (encoded at UNIX timestamps for easier comparison), then we can compare tweets to known prior blackouts, and examine whether blackout keywords spike in those areas during said known blackouts.  This will allow us to engineer MANY useful features, including better predictive terminology, measurements of how accurately users are when they self-identifying their location, and more.  

#### D. Small Scale Scraping and LocalSearch
Everything we've discussed so far contributes to building a more-and-more effective core model, but as you've no doubt noticed, all of what's been outlining relies on historical data.  That's not very helpful to a disaster relief group!  If they want to know where blackouts occured 4 years ago, they can google that just as well as we can, that doesn't help anyone get immediate aid.  

The features we've discussed will allow us to build a very accurate core model, which will be instrumental for the next proposed addition, which we call LocalSearch.  LocalSearch is the best tool to addresses the core goal of this project, which is to identify and geolocate blackouts in as close to real-time as possible.  

LocalSearch is by design a miniaturized version of the function that generated data to fuel our core model; in a word, by generating an identically formatted but new dataset on the fly, we're essentially creating an (X_test, y_test) to coorespond to the preexisting data (X_train,y_train) used by Core.  

LocalSearch would, consequently:
1. Take an argument designating a city or list of cities to filter tweets
2. Continue to use the same keyword filtering as Core
3. Scrape a much smaller dataset - at a rate that complies with Twitter's API rules so this tool remains usable and does not abuse Twitter's trust - for example ~ 1,000 tweets
4. Use a variety of classification models (including NLP) to identify tweets that we believe are originating from power outage affected areas - using the information learned in Core.
5. (Possibly) Bag these models and have them vote, if we find the results are too variable.  Otherwise, go with the best performing model's classification
6. Generate a Bokeh interactive heatmap of the searched city and it's surroundings, and output this map along with summary data from the modeling results.  

I believe that this represents the best possible product to satisfies our goal.

# Conclusion
In summary, social media is an astonishingly rich source of data, but some small quirks (or in this case, justifiable privacy measures) can limit the awesome scope of it's potential.  In this case, the impact of the near-universally empty "User Location" in Twitter Metadata was disproportionately bad, because location was by far the most desirable piece of information we could've received.  

This setback badly limited the degree to which we could implement a finished product, LocalSearch, that we have just outlined above.  However, with a little creativity (and, most likely, a paid subscription to Twitters Premium API), we have demonstrated that it is completely possible to feature engineer something close enough to "User Location" for our purposes.  

Ultimately it is clear that social media can absolutely be used to viably and accurately pinpoint power outages in the United States.  We hope that you will consider our proof-of-concept model along side our proposals for a finished product, and bear in mind that building out the proposed features will be an ongoing and laborious process.  Thanks for reading about our project, it's been a pleasure to share our ideas with you!







