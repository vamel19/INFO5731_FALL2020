# **The eighth in-class-exercise (20 points in total, 10/29/2020)**

The data for this exercise is from the dataset you created from assignment three. Please perform answer the following questions based on your data:

## (1) (10 points) Write a python program to extract the sentiment related terms from the corpus. You may use python package such as polyglot or external lexicon resources in the question. Rank the sentiment related terms by frequency.


```python
import re 
import tweepy 
from tweepy import OAuthHandler 
from textblob import TextBlob 

class TwitterClient(object): 
	''' 
	Generic Twitter Class for sentiment analysis. 
	'''
	def __init__(self): 
		''' 
		Class constructor or initialization method. 
		'''
		# keys and tokens from the Twitter Dev Console 
		consumer_key = 'N6cJr99C5Q744nx7HIbVvjYyP'
		consumer_secret = 'sxSjUPkj3K0aIJ0mEUeFiKwa7KVSKTZqW5rRKgnVpOxstePozS'
		access_token = '2747565082-n5VtKoOXk0PXEi705OOABrAp8yWguawU4yJNYbq'
		access_token_secret = 'J6XbBOfzkiB5XR1A3cHtOXc0xbcruLfulatDXYrmiGMQD'

		# attempt authentication 
		try: 
			# create OAuthHandler object 
			self.auth = OAuthHandler(consumer_key, consumer_secret) 
			# set access token and secret 
			self.auth.set_access_token(access_token, access_token_secret) 
			# create tweepy API object to fetch tweets 
			self.api = tweepy.API(self.auth) 
		except: 
			print("Error: Authentication Failed") 

	def clean_tweet(self, tweet): 
		''' 
		Utility function to clean tweet text by removing links, special characters 
		using simple regex statements. 
		'''
		return ' '.join(re.sub("(@[A-Za-z0-9]+)|([^0-9A-Za-z \t])|(\w+:\/\/\S+)", " ", tweet).split()) 

	def get_tweet_sentiment(self, tweet): 
		''' 
		Utility function to classify sentiment of passed tweet 
		using textblob's sentiment method 
		'''
		# create TextBlob object of passed tweet text 
		analysis = TextBlob(self.clean_tweet(tweet)) 
		# set sentiment 
		if analysis.sentiment.polarity > 0: 
			return 'positive'
		elif analysis.sentiment.polarity == 0: 
			return 'neutral'
		else: 
			return 'negative'

	def get_tweets(self, query, count = 10): 
		''' 
		Main function to fetch tweets and parse them. 
		'''
		# empty list to store parsed tweets 
		tweets = [] 

		try: 
			# call twitter api to fetch tweets 
			fetched_tweets = self.api.search(q = query, count = count) 

			# parsing tweets one by one 
			for tweet in fetched_tweets: 
				# empty dictionary to store required params of a tweet 
				parsed_tweet = {} 

				# saving text of tweet 
				parsed_tweet['text'] = tweet.text 
				# saving sentiment of tweet 
				parsed_tweet['sentiment'] = self.get_tweet_sentiment(tweet.text) 

				# appending parsed tweet to tweets list 
				if tweet.retweet_count > 0: 
					# if tweet has retweets, ensure that it is appended only once 
					if parsed_tweet not in tweets: 
						tweets.append(parsed_tweet) 
				else: 
					tweets.append(parsed_tweet) 

			# return parsed tweets 
			return tweets 

		except tweepy.TweepError as e: 
			# print error (if any) 
			print("Error : " + str(e)) 

def main(): 
	# TwitterClient Class 
	api = TwitterClient() 
	# function to get tweets 
	tweets = api.get_tweets(query = 'Halloween', count = 200) 

	# positive tweets from tweets 
	ptweets = [tweet for tweet in tweets if tweet['sentiment'] == 'positive'] 
	# percentage of positive tweets 
	print("Positive tweets percentage: {} %".format(100*len(ptweets)/len(tweets))) 
	# negative tweets from tweets 
	ntweets = [tweet for tweet in tweets if tweet['sentiment'] == 'negative'] 
	# percentage of negative tweets 
	print("Negative tweets percentage: {} %".format(100*len(ntweets)/len(tweets))) 
	# percentage of neutral tweets 
	print("Neutral tweets percentage: {} %".format(100*(len(tweets) - len(ntweets) - len(ptweets))/len(tweets))) 

	# printing first 5 positive tweets 
	print("\n\nPositive tweets:") 
	for tweet in ptweets[:10]: 
		print(tweet['text']) 

	# printing first 5 negative tweets 
	print("\n\nNegative tweets:") 
	for tweet in ntweets[:10]: 
		print(tweet['text']) 

if __name__ == "__main__": 
	# calling main function 
	main() 
```

    Positive tweets percentage: 34.09090909090909 %
    Negative tweets percentage: 7.954545454545454 %
    Neutral tweets percentage: 57.95454545454545 %
    
    
    Positive tweets:
    @shadowlord_king The female would flinch lightly from the hand that reached to her, but shortly come to relaxation… https://t.co/wirOZawx78
    RT @UndeniableLOVE7: Y’all getting real creative for Halloween. I want no parts 😭😭 https://t.co/wBsw9bp7Zt
    RT @ElveroW: 🦇 🎃 🦇 🎃 🦇 🎃 🦇 🎃 The eagle 🦅 has already spoken Trump and you will be doomed ... so get the hell 🔥ready to get your rubbish out…
    RT @najaeminedit: Happy Halloween 🧡🍬👻🎃
    #재민 #JAEMIN #NCT https://t.co/O6SxowFZ2y
    RT @my_rachelle: Happy Halloween! 
    This #NFT #halloween Drakons are now available for grabs in https://t.co/hRBaH18F3I
    
    Don’t miss it or yo…
    RT @rilakkuma_gyr: 🎃Happy Halloween🎃
    
    リラックマが　あらわれた！
    リラックマも　じゅもんを　となえている！
    
    ▷モグモグ アンド ゴロ〜ン？
    
    #ハロウィーン
    #ＲＰＧ仮装
    #リラックマクエスト
    #コメント欄でおかし募集中 https://…
    RT @suedepyjamas: It's important we don't lose sight of the real meaning of Halloween. https://t.co/HjIXN32TIa
    RT @dixie_ichi: Happy Halloween!!! 👻🧟‍♂️ #obeyme https://t.co/i34k683XEx
    Most recent #Halloween2020 #artwork in 2020. #HappyHalloween 🧟‍♂️🎃🧙‍♀️👻
    
    Created by @Microsoft Paint
    By @c_recca… https://t.co/0Of9ytUqNY
    So what's everybody doin for Halloween this year? Post your best costume pic. We will announce the winner Halloween… https://t.co/UtRzF9R45H
    
    
    Negative tweets:
    RT @chopsticklick: for halloween i think ima be drunk
    RT @keeevin_1d: Solo Liam puede hacer promocion a su show de Halloween y 2 días antes sacar una canción de navidad jajaj
    
    STREAM NAUGHTY LI…
    RT @mmpadellan: So, with Halloween only 2 days away, I'm thinking it's NOT SAFE to send our kids out trick-or-treating.
    
    Do you guys agree…
    RT @ATEEZofficial: [🎃] ATEEZ(에이티즈) - ‘THE BLACK CAT NERO’ Halloween Performance Video Teaser
    ⠀
    https://t.co/esqk86c4cS
    ⠀
    #THE_BLACK_CAT_NER…
    @TeacupInTheBay @KQED In time for Halloween scary movies
    ANYWAYS DURING THE BREAK I NOTICED A NEW KING GNU SONG... im gonna project this so hard onto my oc for the Hallowee… https://t.co/Cg7btR8UC5
    gmbjnmhgksghskdjghskgjdhskjghs SOMEONE FOR MY WORKS HALLOWEEN CALL DRESSED AS OUR PREMIER IM CRYING AT THE NORTHFACE


## (2) (10 points) Compare the performance of the following tools in sentiment identification: TextBlob (https://textblob.readthedocs.io/en/dev/), VADER (https://github.com/cjhutto/vaderSentiment), TFIDF-based Support Vector Machine (SVM) (Split your data into training and testing data). Take your own annotation as the standard answers. 

Reference code: https://towardsdatascience.com/fine-grained-sentiment-analysis-in-python-part-1-2697bb111ed4


```python
!pip install -U textblob
!python -m textblob.download_corpora
```

    Requirement already up-to-date: textblob in /opt/anaconda3/lib/python3.8/site-packages (0.15.3)
    Requirement already satisfied, skipping upgrade: nltk>=3.1 in /opt/anaconda3/lib/python3.8/site-packages (from textblob) (3.5)
    Requirement already satisfied, skipping upgrade: regex in /opt/anaconda3/lib/python3.8/site-packages (from nltk>=3.1->textblob) (2020.6.8)
    Requirement already satisfied, skipping upgrade: joblib in /opt/anaconda3/lib/python3.8/site-packages (from nltk>=3.1->textblob) (0.16.0)
    Requirement already satisfied, skipping upgrade: tqdm in /opt/anaconda3/lib/python3.8/site-packages (from nltk>=3.1->textblob) (4.47.0)
    Requirement already satisfied, skipping upgrade: click in /opt/anaconda3/lib/python3.8/site-packages (from nltk>=3.1->textblob) (7.1.2)
    [nltk_data] Downloading package brown to
    [nltk_data]     /Users/melissavang/nltk_data...
    [nltk_data]   Package brown is already up-to-date!
    [nltk_data] Downloading package punkt to
    [nltk_data]     /Users/melissavang/nltk_data...
    [nltk_data]   Package punkt is already up-to-date!
    [nltk_data] Downloading package wordnet to
    [nltk_data]     /Users/melissavang/nltk_data...
    [nltk_data]   Package wordnet is already up-to-date!
    [nltk_data] Downloading package averaged_perceptron_tagger to
    [nltk_data]     /Users/melissavang/nltk_data...
    [nltk_data]   Package averaged_perceptron_tagger is already up-to-
    [nltk_data]       date!
    [nltk_data] Downloading package conll2000 to
    [nltk_data]     /Users/melissavang/nltk_data...
    [nltk_data]   Package conll2000 is already up-to-date!
    [nltk_data] Downloading package movie_reviews to
    [nltk_data]     /Users/melissavang/nltk_data...
    [nltk_data]   Package movie_reviews is already up-to-date!
    Finished.



```python
from textblob import TextBlob

text = '''
"Think we didn't land on the Moon? What about the Earth being flat?
 
We’re diving into conspiracies & hoaxes for a special #NASAHalloween 🎃 episode of #AskNASA. What secret plans do you think we’ve been cooking up over the years? Drop them below.
 
Video reply = extra points 😎",,1
"For many of us, the most important intersection of geography and human culture in our lives is where we go to work. Or used to.",,1
"When #covid19 began to peak in #India, the country faced shortages of testing kits, lack of PPE, and hotspots in populated areas. The 
@actioncovidteam
 provided catalytic funding that helped the country gear up to fight the virus.",,2
"Walt Disney World will lengthen its theme park hours this Halloween weekend, with more extended hours coming in November.",,2
"""Finding strategies – such as utilizing your college’s therapy center, your friends, or even a crisis text line – can make your loan anxieties a little more bearable""",,1
"Waxing gibbous moon symbol ICYMI... using our 
@SOFIATelescope
, we found water on the Moon's sunlit surface for the first time. Scientists think the water could be stored inside glass beadlike structures within the soil that can be smaller than the tip of a pencil. A recap: https://go.nasa.gov/2HFTZbw",,2
Sunbeams illuminate the forest floor in this image that Your Shot photographer Sylvia Michel captured of her dog bounding playfully through the fallen leaves,,2
"One of our 
@asist_org
 poster presentation:
#PhDstudent Daniel A. Houli and Marie L. Radford: An Exploratory Study Using Mindfulness Meditation Apps to Buffer Workplace Technostress and Information Overload.

@MarieLRadford
 #RUSCIresearch  #ASIST2020",,2
"Three of the largest fires in state history have burned this year—covering nearly 700,000 acres",,0
See more from our November special issue on how the pandemic is changing our lives,,1
"Free downloadable pattern for the Adorela quilt (very Orla Kiely!) from 
@cuttopieces
 http://ow.ly/Kdk750BXf1g",,1
Another new beautiful bike trail to add to next year's travel wishlist,,2
"Seeing constellations so clearly grows more difficult each year, which makes dark sky destinations all the more important—both to see and to protect.",,0
Peru is opening up to more tourists from next month with international flights scheduled to arrive in Lima from 25 cities,,1
November is usually the kickoff for Antarctic cruising. But the pandemic may put those plans on ice.,,1
The Adorable Little Birds Called Black-Throated Bushtit,,2
"It’s #SunDay — with a special guest, the Moon! Our satellite’s view of the Sun was briefly interrupted by a lunar transit on Oct. 16. At peak, the Moon covered about 44% of the Sun. https://go.nasa.gov/3mdoe8C",,1
The Czech capital was once a magnet for scientists who studied the stars—and clues to its cosmic side are scattered throughout the city.,,1
Our first block of sessions is starting soon! They will be from 11:00am-12:30pm EDT. Which sessions are you most excited about and why? #ASIST20,,2
Want the Canon EOS Rebel T8i / 850D for 4K? Here's our verdict on the new camera's video https://buff.ly/35th9tR,,1
Happy French Bulldog Dances Whenever He's Excited,,2
"""While visitors can expect the same bespoke holiday shops, delicious food, and winter activities as they have always enjoyed, the Winter Village will also be taking into account new rules regarding public safety in the wake of the pandemic.""",,1
"""After this, please remember the respect all the essential workers deserve, because they’re the ones who got you through the tough times""",,1
"By the end of 2020, about 100 million additional people are projected to find themselves in extreme poverty, living on less than $1.90 a day",,0
Photographer Captured Bees Sleeping In Flower And It’s As Cute As It Sounds,,2
Craziest tripod ever? 3 Legged Thing launches wild (and pricey) new sticks named after pro skateboarders! https://buff.ly/3mfZTio,,1
A  landscape that featured in key scenes from the Lord of the Rings movies has opened to the public to explore,,1
Science—rather than the supernatural—can explain some of the gruesome postmortem phenomena that fueled belief in vampirism,,1
Are we nearing the limit for how accurate storm track predictions can get?,,1
Legend has it that up to 10 people may have been murdered at The Myrtles Plantation. #WorldsMostUnexplained investigates one of America's most haunted houses Sunday night at 11pm|10c.,,0
"Whatever you’re celebrating—your wedding, a milestone birthday, or just the chance to get away from it all—now is a great time to start planning that big trip, even if your departure date is a ways off.",,2
Roots dangle from the ceiling of a lava tube on the island of Hawaii in this eerie scene by #YourShot photographer Andrew Hara.,,1
"Asteroids are the storytellers of our solar system’s youth. This week, 
@OSIRISREx
 collected an asteroid sample & will bring its stories to Earth.

Why is it that asteroid Bennu holds the history of our origins? Let’s go back to the beginning: https://tmblr.co/Zz_UqjZ8UAg84i00",,1
Precious bunny jumps over bars,,2
" As we are preparing for the full engine firing test for the #Artemis I mission, we’ve been asking you — yes, you! — to tell us what you would pack for the Moon. 
 
Camera with flash For inspiration, here's what our own imagery experts would put in their #NASAMoonKit: https://go.nasa.gov/2IVHGrW",,2
"This month is breast cancer awareness month and today we support the #wearitpink day by offering a 15% discount across our site*

5% of all sales this weekend will be donated to Breast Cancer research!

Use the code COPPAFEEL at checkout Two hearts
http://littlemisssewnsew.co.uk
#COPPAFEEL",,2
Some people have made the bold move of leaving the US,,1
Just playin' in the rain. What a glorious feeling. I'm happy again. Musical note,,2
"Our orbiting laboratory circles the Earth every 91 minutes and 12 seconds. Can your workout beat one 
@Space_Station
 orbit?  Woman biking 

Tag your #SweatinWithTheStation photos to commemorate 20 years of continuous human presence in space: https://go.nasa.gov/3jt5e3U 
#SpaceStation20th",,1
"Tourism is down, but nesting success may be up",,2
Getting a tattoo could be one of the best and memorable experiences,,2
"We are readying for #LaunchAmerica! The lead 
@NASAFltDirector
 for 
@NASA
’s SpaceX Crew-1 mission schools us on planning for this flight and future ones to the 
@Space_Station
 on this week's ""Houston We Have a Podcast.""
https://go.nasa.gov/3dTZkYo",,1
Cows are really just grass puppies ,,2
"Whether or not you believe in ghosts, these seven spooky destinations prove that haunting lore is often rooted in very real and traumatizing histories.",,1
5 key differences between the Nikon Z6 II and Nikon Z7 II. Which new Nikon is right for you? https://buff.ly/3jptbJu,,1
Why do they celebrate Bonfire Night in the UK?,,1
"Hawaii had more than 65,000 travelers arrive in the islands in the first week of its pre-travel coronavirus testing program, a state effort to get the tourism-based economy moving again amid the pandemic.",,1
10 of the best scary movie and TV destinations in the US https://fal.cn/3b8HL,,1
"Scientists and collaborators have set thousands of traps around northwestern Washington, in hopes of catching live insects that can be tracked back to their hives",,1
20+ Hilarious Photos Showing That Cats Are Always Full of Surprises,,2
"Just one knicker making workshop this year at 
@SewInBrighton
 on Saturday 21st November, 1.30-6.30pm. Ideal for Improver and above sewing level. Get the lace ready! http://ow.ly/7lQM50BXekg",,1
"As historic Chinatowns struggle during the pandemic, it’s worth looking at why residents—and visitors—flocked to them in the first place.",,1
"To ensure a child’s successful social development, they need to play and interact with their peers—just as their ancestors have done over millions of years",,1
A stunning river in Vermont getting hit hard by fall,,2
"RT 
@TheSRAOrg
: The latest edition of the SRA Journal, Social Research Practice, Spring 2020 is out now. You can view the full open-access issue on the SRA website now.
http://bit.ly/2Vk9jiv
#socialresearch #researchjournal",,1
Guests can also enjoy a fridge stocked with local treats for breakfast,,2
"Sale of fabrics and dressmaking patterns at 
@sewloco
 including these cool Jamie Jeans by Named - see what's on offer here http://ow.ly/BMA550BXdYv",,1
"The gardens of Alcatraz offer vibrant blooms, cool breezes, and—improbably—a sense of escape",,2
Spectacular Footage Of A Real-Life Creature Known As A Sea Angel Recording In The White Sea,,1
"Bro we had a blast with this week’s podcast. Bekah (
@bekah899
) and I talked about so much with Jaleisa (
@Michelle_J36
), be sure to go check it out Call me hand",,2
Mexico's stunning beaches are the best part of any trip to the country Sun with rays,,2
Across the United States' parched West this year there’s been an obvious—and destructive—signal that national parks are struggling to cope with a shifting climate: wildfire.,,0
Cute Dog Thinks She Is The Mother Of These Baby Goats,,2
"13 years + 42 assembly flights = 1 fully constructed 
@Space_Station
 that 241 space travelers from 19 countries have called home.

Learn more about the teamwork it takes to construct an orbiting laboratory and maintain it today: https://go.nasa.gov/3okVzjz
 #SpaceStation20th",,1
Sea Bunnies: Japan Is Going Crazy About These Furry Sea Slugs,,1
"Join me and 
@OSIRISREx
 Principal Investigator Dante Lauretta at 5 p.m. ET as we take media questions about the sample the mission collected from asteroid Bennu on Tuesday: https://nasa.gov/live",,1
"Apps, facial recognition, and smart products can make air transit and border crossings more convenient. But they can come with some risks.",,1
These babies are too funny!,,2
We know classrooms look different this year. Headphones can help students and teachers stay connected. Shout out to all the incredible teachers going above and beyond this year!,,2
"Happy spooky season, everyone! Share your costumes and cosplays with us on the Media Library Discord for a chance to win some really neat prizes. Join the fun @ http://discord.com/invite/NswwyJ2.",,2
Looking for the latest information on #studentloans and #FAFSA amid #COVID19? Make sure to keep up-to-date with news from the US Department of Education. https://bit.ly/2ThJw8D,,1
"The new MRI machine at 
@dellchildrens
 allows doctors to see finer details and perform more complicated brain surgeries than in the past. The machine will help bring more specialty #healthcare to families here in #CentralTexas. Clapping hands signClapping hands signClapping hands sign Read more.",,1
High-tech filters and low-tech masks: How technology and personal responsibility might make flying safer than you think.,,2
"Only 13 percent of Americans say they’d be willing to fly now or before the end of the year, a new National Geographic survey shows.",,1
"A coalition of cruise industry employees and their supporters rallied Wednesday at Port Canaveral, seeking an end to the government's seven-month halt to cruising because of the coronavirus pandemic.",,1
"A dangerous yeast, which can colonize a person’s skin without generating symptoms, is rising due to medical centers being overrun",,0
"If you thought that the 2020 holidays were going to be all ""bah humbug"" in New York City, think again.",,1
"It's officially International #SnowLeopardDay.

You may clap now.",,1
Humboldt County is one of the most uniquely beautiful places in the world.,,2
"""Documenting students’ lives 'virtually' makes it difficult to capture our memories—so instead, we must document the year honestly in the best way we can""",,1
"Airlines are keen to have you on board, but should you go? 
@tomhalltravel
 weighs in",,1
"RT 
@ahrcpress
: Just one week left to apply for a place on our Engaging with Government course, taking place 23-25 February 2021.

Deadline 30 October. Find out more: http://orlo.uk/qixjY",,1
Dog Siblings Recognize Each Other And Their Hug Is Warming Our Hearts,,2
"Win a bundle of Prym Love goodies from 
@trixielixiesays
 - follow them on Facebook and tag 3 friends and for extra entries share the giveaway to your stories. End 31st October 2020- good luck! http://ow.ly/C0ss50BXcxi",,1
"Lush forests cover more than half of Slovenia, an Adriatic enclave of surprising biodiversity.",,1
"This bobsled rollercoaster looks like a blast!

""Feel the Rhythm! Feel the Rhyme! Get on up, it's bobsled time!"" (if you know, you know)",,2
"The colors, Duke, the colors! 

Our space telescopes help us see a whole 
@NASAUniverse
 of color, as seen here in the Small Magellanic Cloud. Explore more images on #NationalColorDay in our image library: http://images.nasa.gov",,1
"Each day, Americans are forced to make the choice between paying rent & having running water, or buying food & keeping the lights on. It's more common than you’d think—50 million households struggle to afford these expenses every single month.",,0
These workout tips Ledgerand videos Personal computerare a great way to mix up your usual routine whenever you’re stuck indoors ,,1
"From spectacular views to rich culture, discover these unique experiences found nowhere else. 
@visitrwanda_now
 #sponsored",,1
Voteology helps college students register to vote where it matters most ,,1
"We've all heard stories about haunted hotels, but what about haunted travel?",,1
"At 
@NASAKennedy
, Launch Pad 39B's newest visitor is the mobile launcher for #Artemis I. This visit to the pad is a dress rehearsal for the launch of 
@NASA_SLS
 and 
@NASA_Orion
 next year. Go behind the scenes: https://go.nasa.gov/2HnCCw0",,1
Multiple incidents of mass bird deaths suggests that Avitrol is often used to exterminate rather than repel birds,,0
"If you love flowers and embroidery, Trish Burr's Book of Embroidered Flowers published by 
@SearchPress
 is for you - a perfect gift! We loved it - read our review here http://ow.ly/j2EN50BWi82",,1
"In case you missed it: last night, 3 space travelers including Chris Cassidy (
@Astro_SEAL
) departed the 
@Space_Station
 after 196 days in space, safely landing back on Earth. Cassidy completed a total of 4 spacewalks on this mission, his 3rd spaceflight: https://go.nasa.gov/3obMZUt",,1
"Branch Manager - Waco-McLennan County Library - Waco, TX https://bit.ly/2TeBCwE #libraryjobs #libraryjob #LISjobs #libjobs #jobs",,1
Who said you can’t make main package after graduation?,,1
Romantic Photos Of Pastel Parrotlet Birds,,2
'''

blob = TextBlob(text)
blob.tags           
blob.noun_phrases  

for sentence in blob.sentences:
    print(sentence.sentiment.polarity)
```

    0.0
    -0.025
    0.35714285714285715
    -0.4
    0.0
    0.27999999999999997
    0.0
    0.0
    0.0
    0.5
    0.0925
    0.0
    0.0
    0.32142857142857145
    0.17954545454545456
    0.0625
    0.25297619047619047
    0.0
    0.0
    0.125
    0.0
    0.4375
    0.0
    0.3215909090909091
    -0.036111111111111094
    0.13522727272727272
    -0.14999999999999997
    0.43333333333333335
    -0.5
    0.0
    0.0
    0.3125
    0.0
    0.75
    0.3333333333333333
    0.0
    0.8
    0.0
    0.0
    0.35388888888888886
    0.0
    0.23
    0.0
    0.21103896103896103
    0.375
    0.26079545454545455
    0.45
    0.25
    0.18333333333333335
    0.25416666666666665
    0.35
    0.26666666666666666
    0.54
    -0.04999999999999999
    0.2380952380952381
    0.0
    0.0
    0.0
    0.6333333333333333
    0.5
    0.3
    0.5
    0.5
    -0.02840909090909091
    0.5
    0.5
    0.2372727272727273
    0.0
    -0.1471590909090909
    0.0
    0.0
    0.49000000000000005
    0.2
    0.0
    0.335
    0.875
    0.2833333333333333
    0.0
    0.0
    0.0
    0.0
    0.0
    -0.024999999999999994
    0.09999999999999998
    0.042857142857142864
    0.31666666666666665
    0.5
    0.0
    0.0
    0.275
    0.3
    0.041666666666666664



```python
import pandas as pd
# Read train data
df = pd.read_csv('annotated_dataset.csv', sep='\t', header=None, names=['truth', 'text'])
df['truth'] = df['truth'].str.replace('__label__', '')
df['truth'] = df['truth'].astype(int).astype('category')
df.head()
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-35-841a96b4b3b6> in <module>
          3 df = pd.read_csv('annotated_dataset.csv', sep='\t', header=None, names=['truth', 'text'])
          4 df['truth'] = df['truth'].str.replace('__label__', '')
    ----> 5 df['truth'] = df['truth'].astype(int).astype('category')
          6 df.head()


    /opt/anaconda3/lib/python3.8/site-packages/pandas/core/generic.py in astype(self, dtype, copy, errors)
       5696         else:
       5697             # else, only a single dtype is given
    -> 5698             new_data = self._data.astype(dtype=dtype, copy=copy, errors=errors)
       5699             return self._constructor(new_data).__finalize__(self)
       5700 


    /opt/anaconda3/lib/python3.8/site-packages/pandas/core/internals/managers.py in astype(self, dtype, copy, errors)
        580 
        581     def astype(self, dtype, copy: bool = False, errors: str = "raise"):
    --> 582         return self.apply("astype", dtype=dtype, copy=copy, errors=errors)
        583 
        584     def convert(self, **kwargs):


    /opt/anaconda3/lib/python3.8/site-packages/pandas/core/internals/managers.py in apply(self, f, filter, **kwargs)
        440                 applied = b.apply(f, **kwargs)
        441             else:
    --> 442                 applied = getattr(b, f)(**kwargs)
        443             result_blocks = _extend_blocks(applied, result_blocks)
        444 


    /opt/anaconda3/lib/python3.8/site-packages/pandas/core/internals/blocks.py in astype(self, dtype, copy, errors)
        623             vals1d = values.ravel()
        624             try:
    --> 625                 values = astype_nansafe(vals1d, dtype, copy=True)
        626             except (ValueError, TypeError):
        627                 # e.g. astype_nansafe can fail on object-dtype of strings


    /opt/anaconda3/lib/python3.8/site-packages/pandas/core/dtypes/cast.py in astype_nansafe(arr, dtype, copy, skipna)
        872         # work around NumPy brokenness, #1987
        873         if np.issubdtype(dtype.type, np.integer):
    --> 874             return lib.astype_intsafe(arr.ravel(), dtype).reshape(arr.shape)
        875 
        876         # if we have a datetime/timedelta array of objects


    pandas/_libs/lib.pyx in pandas._libs.lib.astype_intsafe()


    ValueError: invalid literal for int() with base 10: 'document_id, clean_text ,sentiment'



```python
!pip install pandas
```

    Requirement already satisfied: pandas in /opt/anaconda3/lib/python3.8/site-packages (1.0.5)
    Requirement already satisfied: python-dateutil>=2.6.1 in /opt/anaconda3/lib/python3.8/site-packages (from pandas) (2.8.1)
    Requirement already satisfied: pytz>=2017.2 in /opt/anaconda3/lib/python3.8/site-packages (from pandas) (2020.1)
    Requirement already satisfied: numpy>=1.13.3 in /opt/anaconda3/lib/python3.8/site-packages (from pandas) (1.18.5)
    Requirement already satisfied: six>=1.5 in /opt/anaconda3/lib/python3.8/site-packages (from python-dateutil>=2.6.1->pandas) (1.15.0)



```python
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
    #note: depending on how you installed (e.g., using source code download versus pip install), you may need to import like this:
    #from vaderSentiment import SentimentIntensityAnalyzer

# --- examples -------
sentences = ["Think we didn't land on the Moon? What about the Earth being flat? We’re diving into conspiracies & hoaxes for a special #NASAHalloween 🎃 episode of #AskNASA. What secret plans do you think we’ve been cooking up over the years? Drop them below. Video reply = extra points 😎", "For many of us, the most important intersection of geography and human culture in our lives is where we go to work. Or used to.", 
             "When #covid19 began to peak in #India, the country faced shortages of testing kits, lack of PPE, and hotspots in populated areas. The @actioncovidteam provided catalytic funding that helped the country gear up to fight the virus.", 
             "Walt Disney World will lengthen its theme park hours this Halloween weekend, with more extended hours coming in November.",
             "Finding strategies – such as utilizing your college’s therapy center, your friends, or even a crisis text line – can make your loan anxieties a little more bearable",
             "Waxing gibbous moon symbol ICYMI... using our @SOFIATelescope, we found water on the Moon's sunlit surface for the first time. Scientists think the water could be stored inside glass beadlike structures within the soil that can be smaller than the tip of a pencil. A recap: https://go.nasa.gov/2HFTZbw",
             "Sunbeams illuminate the forest floor in this image that Your Shot photographer Sylvia Michel captured of her dog bounding playfully through the fallen leaves",
             "One of our @asist_org poster presentation: #PhDstudent Daniel A. Houli and Marie L. Radford: An Exploratory Study Using Mindfulness Meditation Apps to Buffer Workplace Technostress and Information Overload. @MarieLRadford #RUSCIresearch  #ASIST2020", 
             "Three of the largest fires in state history have burned this year—covering nearly 700,000 acres", 
             "See more from our November special issue on how the pandemic is changing our lives",
             "Free downloadable pattern for the Adorela quilt (very Orla Kiely!) from @cuttopieces http://ow.ly/Kdk750BXf1g",
             "Another new beautiful bike trail to add to next year's travel wishlist",
             "Seeing constellations so clearly grows more difficult each year, which makes dark sky destinations all the more important—both to see and to protect.",
             "Peru is opening up to more tourists from next month with international flights scheduled to arrive in Lima from 25 cities",
             "November is usually the kickoff for Antarctic cruising. But the pandemic may put those plans on ice.",
             "The Adorable Little Birds Called Black-Throated Bushtit",
             "It’s #SunDay — with a special guest, the Moon! Our satellite’s view of the Sun was briefly interrupted by a lunar transit on Oct. 16. At peak, the Moon covered about 44% of the Sun. https://go.nasa.gov/3mdoe8C",
             "The Czech capital was once a magnet for scientists who studied the stars—and clues to its cosmic side are scattered throughout the city.",
             "Our first block of sessions is starting soon! They will be from 11:00am-12:30pm EDT. Which sessions are you most excited about and why? #ASIST20",
             "Want the Canon EOS Rebel T8i / 850D for 4K? Here's our verdict on the new camera's video https://buff.ly/35th9tR",
             "Happy French Bulldog Dances Whenever He's Excited",
             "While visitors can expect the same bespoke holiday shops, delicious food, and winter activities as they have always enjoyed, the Winter Village will also be taking into account new rules regarding public safety in the wake of the pandemic.",
             "After this, please remember the respect all the essential workers deserve, because they’re the ones who got you through the tough times",
             "By the end of 2020, about 100 million additional people are projected to find themselves in extreme poverty, living on less than $1.90 a day",
             "Photographer Captured Bees Sleeping In Flower And It’s As Cute As It Sounds",
             "Craziest tripod ever? 3 Legged Thing launches wild (and pricey) new sticks named after pro skateboarders! https://buff.ly/3mfZTio",
             "A  landscape that featured in key scenes from the Lord of the Rings movies has opened to the public to explore",
             "Science—rather than the supernatural—can explain some of the gruesome postmortem phenomena that fueled belief in vampirism",
             "Are we nearing the limit for how accurate storm track predictions can get?",
             "Legend has it that up to 10 people may have been murdered at The Myrtles Plantation. #WorldsMostUnexplained investigates one of America's most haunted houses Sunday night at 11pm|10c.",
             "Whatever you’re celebrating—your wedding, a milestone birthday, or just the chance to get away from it all—now is a great time to start planning that big trip, even if your departure date is a ways off.",
             "Roots dangle from the ceiling of a lava tube on the island of Hawaii in this eerie scene by #YourShot photographer Andrew Hara.",
            "Asteroids are the storytellers of our solar system’s youth. This week, @OSIRISREx collected an asteroid sample & will bring its stories to Earth. Why is it that asteroid Bennu holds the history of our origins? Let’s go back to the beginning: https://tmblr.co/Zz_UqjZ8UAg84i00",
            "Precious bunny jumps over bars",
            "As we are preparing for the full engine firing test for the #Artemis I mission, we’ve been asking you — yes, you! — to tell us what you would pack for the Moon. Camera with flash For inspiration, here's what our own imagery experts would put in their #NASAMoonKit: https://go.nasa.gov/2IVHGrW",
            "This month is breast cancer awareness month and today we support the #wearitpink day by offering a 15% discount across our site*",
            "5% of all sales this weekend will be donated to Breast Cancer research! Use the code COPPAFEEL at checkout Two hearts http://littlemisssewnsew.co.uk #COPPAFEEL"]

analyzer = SentimentIntensityAnalyzer()
for sentence in sentences:
    vs = analyzer.polarity_scores(sentence)
    print("{:-<65} {}".format(sentence, str(vs)))
```

    Think we didn't land on the Moon? What about the Earth being flat? We’re diving into conspiracies & hoaxes for a special #NASAHalloween 🎃 episode of #AskNASA. What secret plans do you think we’ve been cooking up over the years? Drop them below. Video reply = extra points 😎 {'neg': 0.036, 'neu': 0.833, 'pos': 0.131, 'compound': 0.6641}
    For many of us, the most important intersection of geography and human culture in our lives is where we go to work. Or used to. {'neg': 0.0, 'neu': 0.92, 'pos': 0.08, 'compound': 0.2716}
    When #covid19 began to peak in #India, the country faced shortages of testing kits, lack of PPE, and hotspots in populated areas. The @actioncovidteam provided catalytic funding that helped the country gear up to fight the virus. {'neg': 0.16, 'neu': 0.84, 'pos': 0.0, 'compound': -0.6705}
    Walt Disney World will lengthen its theme park hours this Halloween weekend, with more extended hours coming in November. {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    Finding strategies – such as utilizing your college’s therapy center, your friends, or even a crisis text line – can make your loan anxieties a little more bearable {'neg': 0.169, 'neu': 0.74, 'pos': 0.092, 'compound': -0.3818}
    Waxing gibbous moon symbol ICYMI... using our @SOFIATelescope, we found water on the Moon's sunlit surface for the first time. Scientists think the water could be stored inside glass beadlike structures within the soil that can be smaller than the tip of a pencil. A recap: https://go.nasa.gov/2HFTZbw {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    Sunbeams illuminate the forest floor in this image that Your Shot photographer Sylvia Michel captured of her dog bounding playfully through the fallen leaves {'neg': 0.092, 'neu': 0.812, 'pos': 0.096, 'compound': 0.0258}
    One of our @asist_org poster presentation: #PhDstudent Daniel A. Houli and Marie L. Radford: An Exploratory Study Using Mindfulness Meditation Apps to Buffer Workplace Technostress and Information Overload. @MarieLRadford #RUSCIresearch  #ASIST2020 {'neg': 0.077, 'neu': 0.923, 'pos': 0.0, 'compound': -0.3612}
    Three of the largest fires in state history have burned this year—covering nearly 700,000 acres {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    See more from our November special issue on how the pandemic is changing our lives {'neg': 0.0, 'neu': 0.838, 'pos': 0.162, 'compound': 0.4019}
    Free downloadable pattern for the Adorela quilt (very Orla Kiely!) from @cuttopieces http://ow.ly/Kdk750BXf1g {'neg': 0.0, 'neu': 0.77, 'pos': 0.23, 'compound': 0.5562}
    Another new beautiful bike trail to add to next year's travel wishlist {'neg': 0.0, 'neu': 0.738, 'pos': 0.262, 'compound': 0.5994}
    Seeing constellations so clearly grows more difficult each year, which makes dark sky destinations all the more important—both to see and to protect. {'neg': 0.097, 'neu': 0.692, 'pos': 0.211, 'compound': 0.5103}
    Peru is opening up to more tourists from next month with international flights scheduled to arrive in Lima from 25 cities {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    November is usually the kickoff for Antarctic cruising. But the pandemic may put those plans on ice. {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    The Adorable Little Birds Called Black-Throated Bushtit---------- {'neg': 0.0, 'neu': 0.652, 'pos': 0.348, 'compound': 0.4939}
    It’s #SunDay — with a special guest, the Moon! Our satellite’s view of the Sun was briefly interrupted by a lunar transit on Oct. 16. At peak, the Moon covered about 44% of the Sun. https://go.nasa.gov/3mdoe8C {'neg': 0.056, 'neu': 0.868, 'pos': 0.076, 'compound': 0.2003}
    The Czech capital was once a magnet for scientists who studied the stars—and clues to its cosmic side are scattered throughout the city. {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    Our first block of sessions is starting soon! They will be from 11:00am-12:30pm EDT. Which sessions are you most excited about and why? #ASIST20 {'neg': 0.114, 'neu': 0.789, 'pos': 0.097, 'compound': -0.1278}
    Want the Canon EOS Rebel T8i / 850D for 4K? Here's our verdict on the new camera's video https://buff.ly/35th9tR {'neg': 0.078, 'neu': 0.78, 'pos': 0.141, 'compound': 0.0772}
    Happy French Bulldog Dances Whenever He's Excited---------------- {'neg': 0.0, 'neu': 0.45, 'pos': 0.55, 'compound': 0.7269}
    While visitors can expect the same bespoke holiday shops, delicious food, and winter activities as they have always enjoyed, the Winter Village will also be taking into account new rules regarding public safety in the wake of the pandemic. {'neg': 0.0, 'neu': 0.737, 'pos': 0.263, 'compound': 0.91}
    After this, please remember the respect all the essential workers deserve, because they’re the ones who got you through the tough times {'neg': 0.058, 'neu': 0.734, 'pos': 0.208, 'compound': 0.5994}
    By the end of 2020, about 100 million additional people are projected to find themselves in extreme poverty, living on less than $1.90 a day {'neg': 0.13, 'neu': 0.87, 'pos': 0.0, 'compound': -0.5563}
    Photographer Captured Bees Sleeping In Flower And It’s As Cute As It Sounds {'neg': 0.0, 'neu': 0.8, 'pos': 0.2, 'compound': 0.4588}
    Craziest tripod ever? 3 Legged Thing launches wild (and pricey) new sticks named after pro skateboarders! https://buff.ly/3mfZTio {'neg': 0.085, 'neu': 0.915, 'pos': 0.0, 'compound': -0.126}
    A  landscape that featured in key scenes from the Lord of the Rings movies has opened to the public to explore {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    Science—rather than the supernatural—can explain some of the gruesome postmortem phenomena that fueled belief in vampirism {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    Are we nearing the limit for how accurate storm track predictions can get? {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    Legend has it that up to 10 people may have been murdered at The Myrtles Plantation. #WorldsMostUnexplained investigates one of America's most haunted houses Sunday night at 11pm|10c. {'neg': 0.231, 'neu': 0.769, 'pos': 0.0, 'compound': -0.8313}
    Whatever you’re celebrating—your wedding, a milestone birthday, or just the chance to get away from it all—now is a great time to start planning that big trip, even if your departure date is a ways off. {'neg': 0.0, 'neu': 0.848, 'pos': 0.152, 'compound': 0.7269}
    Roots dangle from the ceiling of a lava tube on the island of Hawaii in this eerie scene by #YourShot photographer Andrew Hara. {'neg': 0.116, 'neu': 0.884, 'pos': 0.0, 'compound': -0.4357}
    Asteroids are the storytellers of our solar system’s youth. This week, @OSIRISREx collected an asteroid sample & will bring its stories to Earth. Why is it that asteroid Bennu holds the history of our origins? Let’s go back to the beginning: https://tmblr.co/Zz_UqjZ8UAg84i00 {'neg': 0.0, 'neu': 1.0, 'pos': 0.0, 'compound': 0.0}
    Precious bunny jumps over bars----------------------------------- {'neg': 0.0, 'neu': 0.519, 'pos': 0.481, 'compound': 0.5719}
    As we are preparing for the full engine firing test for the #Artemis I mission, we’ve been asking you — yes, you! — to tell us what you would pack for the Moon. Camera with flash For inspiration, here's what our own imagery experts would put in their #NASAMoonKit: https://go.nasa.gov/2IVHGrW {'neg': 0.043, 'neu': 0.842, 'pos': 0.115, 'compound': 0.6114}
    This month is breast cancer awareness month and today we support the #wearitpink day by offering a 15% discount across our site* {'neg': 0.162, 'neu': 0.738, 'pos': 0.1, 'compound': -0.4019}
    5% of all sales this weekend will be donated to Breast Cancer research! Use the code COPPAFEEL at checkout Two hearts http://littlemisssewnsew.co.uk #COPPAFEEL {'neg': 0.156, 'neu': 0.7, 'pos': 0.143, 'compound': -0.1007}



```python
pip install vaderSentiment
```

    Collecting vaderSentiment
      Downloading vaderSentiment-3.3.2-py2.py3-none-any.whl (125 kB)
    [K     |████████████████████████████████| 125 kB 2.0 MB/s eta 0:00:01
    [?25hRequirement already satisfied: requests in /opt/anaconda3/lib/python3.8/site-packages (from vaderSentiment) (2.24.0)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/anaconda3/lib/python3.8/site-packages (from requests->vaderSentiment) (2020.6.20)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/anaconda3/lib/python3.8/site-packages (from requests->vaderSentiment) (1.25.9)
    Requirement already satisfied: chardet<4,>=3.0.2 in /opt/anaconda3/lib/python3.8/site-packages (from requests->vaderSentiment) (3.0.4)
    Requirement already satisfied: idna<3,>=2.5 in /opt/anaconda3/lib/python3.8/site-packages (from requests->vaderSentiment) (2.10)
    Installing collected packages: vaderSentiment
    Successfully installed vaderSentiment-3.3.2
    Note: you may need to restart the kernel to use updated packages.



```python
pip install --upgrade vaderSentiment
```

    Requirement already up-to-date: vaderSentiment in /opt/anaconda3/lib/python3.8/site-packages (3.3.2)
    Requirement already satisfied, skipping upgrade: requests in /opt/anaconda3/lib/python3.8/site-packages (from vaderSentiment) (2.24.0)
    Requirement already satisfied, skipping upgrade: certifi>=2017.4.17 in /opt/anaconda3/lib/python3.8/site-packages (from requests->vaderSentiment) (2020.6.20)
    Requirement already satisfied, skipping upgrade: idna<3,>=2.5 in /opt/anaconda3/lib/python3.8/site-packages (from requests->vaderSentiment) (2.10)
    Requirement already satisfied, skipping upgrade: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/anaconda3/lib/python3.8/site-packages (from requests->vaderSentiment) (1.25.9)
    Requirement already satisfied, skipping upgrade: chardet<4,>=3.0.2 in /opt/anaconda3/lib/python3.8/site-packages (from requests->vaderSentiment) (3.0.4)
    Note: you may need to restart the kernel to use updated packages.



```python
# Your analysis here
The performance of the following tool in TextBlob for sentiment identification shows a tuple of the form (polarity, subjectivity) 
where polarity ranges from -1.0 to 1.0 and subjectivity ranges from 0.0 to 1.0.
The performance of the following tool in Vader for sentiment identification shows standardized thresholds for classifying 
sentences as either positive, neutral, or negative.
```
