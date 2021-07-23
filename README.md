# Data Wrangling
This project focuses on the data wrangling process. It follows the steps of gathering data, assessing data, cleaning data and then repeating as necessary.
We will analyze the [WeRateDogs](https://twitter.com/dog_rates?s=20twitter) twitter page which is a popular page that gives dog rankings usually out of 10 along with a short caption about them and then a couple images. We have a list given to us called "twitter-archive-enhanced.csv" which contains some basic information about tweets from this page starting from their first ever tweet up until August 1st 2017.

We will use Tweepy, which is a python library used to access the twitter API.
You will need a Twitter Developer account so that you can get your unique API Keys and Tokens. Once you have these you can fill your keys and tokens in the commented out space seen in the below code:

    import tweepy
    from tweepy import OAuthHandler
    import json
    from timeit import default_timer as timer

    # Query Twitter API for each tweet in the Twitter archive and save JSON in a text file
    # These are hidden to comply with Twitter's API terms and conditions
    consumer_key = 'HIDDEN'
    consumer_secret = 'HIDDEN'
    access_token = 'HIDDEN'
    access_secret = 'HIDDEN'

    auth = OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_secret)

    api = tweepy.API(auth, wait_on_rate_limit=True)

Now the game plan is, from the twitter-archive-enhanced.csv file, we will use tweepy to pull all the tweets mentioned. Next, as each tweet is pulled, write its returned JSON as a new line in a text file. This can be done using the below code:

    #run a twitter API search for the tweets by its tweet ID
    #create a list and append the tweets into it
    tweets = []
    #create a dictionary for failed tweet ID lookups
    fail = {}

    # Save each tweet's returned JSON as a new line in a .txt file
    with open('tweet_json.txt', 'w') as outfile:
        for id in tweet_ids:
            try:
                tweet = api.get_status(id,
                                       tweet_mode= 'extended',
                                       wait_on_rate_limit_notify=True)
                tweets.append(tweet)
                json.dump(tweet._json, outfile)
                outfile.write('\n')
                print('we are on id:', len(tweets))
            except tweepy.TweepError as e:
                print('fail')
                fail[id] = e
                pass
**Note that this will take a long time to run as twitter places a rate limit on the number of tweets that can be downloaded per minute. This should take around 30-45 minutes to complete.**

Next we will programmatically download the "image-predictictions.tsv" file which is tab-separated-file format, from a given URL. We do this by running the code below:

    # programmatically download the image-predictictions.tsv from udacity server using the given link
    import requests
    url = 'https://d17h27t6h515a5.cloudfront.net/topher/2017/August/599fd2ad_image-predictions/image-predictions.tsv'
    r = requests.get(url, allow_redirects=True)

Then we programmatically name this file:

    # get filename
    if url.find('/'):
        filename = url.rsplit('/', 1)[1]
        print (filename)

Now we load it into the IDE workspace:

    # load file and read as tsv
    df_prediction = pd.read_csv(filename, delimiter='\t' )

#### This completes the gathering task.
##### The assessing, and cleaning can be followed throughout the notebook
##### The cleaned data frame ready for analysis is saved as CSV file called **"twitter_archive_master.csv"**

##### A data wrangling report is provided as a PDF called **"wrangle_report.pdf"**
##### After the analysis was done a report called **"wrangle_act.pdf"** is used to show some insights and visualizations generated
