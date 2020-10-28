<a href="https://colab.research.google.com/github/unt-iialab/INFO5731_Spring2020/blob/master/Assignments/INFO5731_Assignment_Two.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

# **INFO5731 Assignment Two**

In this assignment, you will try to gather text data from open data source via web scraping or API. After that you need to clean the text data and syntactic analysis of the data.

# **Question 1**

(40 points). Write a python program to collect text data from **either of the following sources** and save the data into a **csv file**:

(1) Collect all the customer reviews of the product [2019 Dell labtop](https://www.amazon.com/Dell-Inspiron-5000-5570-Laptop/dp/B07N49F51N/ref=sr_1_11?crid=1IJ7UWF2F4GHH&keywords=dell%2Bxps%2B15&qid=1580173569&sprefix=dell%2Caps%2C181&sr=8-11&th=1) on amazon.

(2) Collect the top 100 User Reviews of the film [Joker](https://www.imdb.com/title/tt7286456/reviews?ref_=tt_urv) from IMDB.

(3) Collect the abstracts of the top 100 research papers by using the query [natural language processing](https://citeseerx.ist.psu.edu/search?q=natural+language+processing&submit.x=0&submit.y=0&sort=rlv&t=doc) from CiteSeerX.

(4) Collect the top 100 tweets by using hashtag ["#CovidVaccine"](https://twitter.com/hashtag/CovidVaccine) from Twitter. 



```python
import pandas as pd
import tweepy
from tweepy import OAuthHandler
import re
import csv
import pandas as pd

consumer_key = 'MIvyODCGp4C2TTPpg3WMJxjZi'
consumer_secret = 'DtxMvnPu3PmitR87in5YQBW3XBx4KBTS1OvPBqDIukY5PaBveh'
access_token = '2747565082-WzdVCnvXz9ct0T6qZxFCtBDOAIoKEYhwUzGGSmt'
access_token_secret = 'di4x9eZCHviOtS1uoIApMNvN63xUXPtY6dZyq3NfMZj1K'


authorizer = OAuthHandler(consumer_key, consumer_secret)
authorizer.set_access_token(access_token, access_token_secret)
api = tweepy.API(authorizer ,timeout=15)
 
all_tweets = []
search_query = '#CovidVaccine'
csvFile = open('tweets.csv', 'a')

for tweet in tweepy.Cursor(api.search,q=search_query+" -filter:retweets",lang='en',result_type='recent').items(100):
  csvWriter = csv.writer(csvFile)
  print(all_tweets)
  print(tweet.text)
  print(tweet.created_at, tweet.text)
  csvWriter.writerow([tweet.created_at, tweet.text.encode('utf-8')])
  
  d = pd.read_csv("tweets.csv",sep='\t',names=['Cleaned'])
print(d)
```

    []
    I’m helping.. 10days since my first vaccine, No side effects...feeling good.  #BePartOfResearch #CovidVaccine #NHS… https://t.co/jYlDRppuXy
    2020-10-27 06:16:59 I’m helping.. 10days since my first vaccine, No side effects...feeling good.  #BePartOfResearch #CovidVaccine #NHS… https://t.co/jYlDRppuXy
    []
    #Covid vaccine (unapproved) is out in China, being sold at yuan 400 (US$60) and people are getting it… https://t.co/N2ennlLjnI
    2020-10-27 06:02:16 #Covid vaccine (unapproved) is out in China, being sold at yuan 400 (US$60) and people are getting it… https://t.co/N2ennlLjnI
    []
    The Trump administration this week will announce a plan to cover the out-of-pocket costs of #COVID19 vaccines for m… https://t.co/9HRf6tOd9l
    2020-10-27 06:01:00 The Trump administration this week will announce a plan to cover the out-of-pocket costs of #COVID19 vaccines for m… https://t.co/9HRf6tOd9l
    []
    Egypt reaches deal with Russia over #Covidvaccine, amid Russian soft power push as concerns remain over the drugs c… https://t.co/3EGdOcFEb8
    2020-10-27 05:30:11 Egypt reaches deal with Russia over #Covidvaccine, amid Russian soft power push as concerns remain over the drugs c… https://t.co/3EGdOcFEb8
    []
    But we are supposed to trust these folks to give us a #COVID19 
    #COVIDVaccine ? They knowingly had asbestos in baby… https://t.co/tpOwHxvRnc
    2020-10-27 04:41:45 But we are supposed to trust these folks to give us a #COVID19 
    #COVIDVaccine ? They knowingly had asbestos in baby… https://t.co/tpOwHxvRnc
    []
    #CoronaVirusUpdates :
    
    #Australian state of #Victoria  records zero new #coronavirus cases in 48 hours
    
    #India to s… https://t.co/cjXs5IfUKv
    2020-10-27 04:35:19 #CoronaVirusUpdates :
    
    #Australian state of #Victoria  records zero new #coronavirus cases in 48 hours
    
    #India to s… https://t.co/cjXs5IfUKv
    []
    Cadila, one of India's two Covid vaccine makers, is going in for a huge capacity boost. #COVIDVaccine… https://t.co/z0DKq1dl5I
    2020-10-27 04:21:46 Cadila, one of India's two Covid vaccine makers, is going in for a huge capacity boost. #COVIDVaccine… https://t.co/z0DKq1dl5I
    []
    #Oxford #vaccine produces #immuneresponse among #old and #youngadults 
    
    https://t.co/RtrB6m624H
    
    @mpostdigital… https://t.co/xSC7eTKfa4
    2020-10-27 04:15:00 #Oxford #vaccine produces #immuneresponse among #old and #youngadults 
    
    https://t.co/RtrB6m624H
    
    @mpostdigital… https://t.co/xSC7eTKfa4
    []
    Even Paint ads are now claiming to protect you against virus, bacteria and pollution. Itna toh vaccine bhi nahi kar… https://t.co/oxQcKHD23a
    2020-10-27 04:01:11 Even Paint ads are now claiming to protect you against virus, bacteria and pollution. Itna toh vaccine bhi nahi kar… https://t.co/oxQcKHD23a
    []
    ETHealthworld | Do those who have recovered from Covid-19 need the vaccine? #CovidVaccine #AntiBody #ImmuneSystem… https://t.co/sGVkk5iCwo
    2020-10-27 04:00:18 ETHealthworld | Do those who have recovered from Covid-19 need the vaccine? #CovidVaccine #AntiBody #ImmuneSystem… https://t.co/sGVkk5iCwo
    []
    Very sad to see one of the vaccines, Eli Lilly, drop out after it not seeming to have a reaction in the body.… https://t.co/sYfzYBp4KK
    2020-10-27 03:57:21 Very sad to see one of the vaccines, Eli Lilly, drop out after it not seeming to have a reaction in the body.… https://t.co/sYfzYBp4KK
    []
    Growth Your Immune System....https://t.co/JxwLiCYinO
    #immunityboosters 
    #Immunology 
    #ImmuneSystem 
    #immunotherapy… https://t.co/q7RI7lRKzk
    2020-10-27 03:53:11 Growth Your Immune System....https://t.co/JxwLiCYinO
    #immunityboosters 
    #Immunology 
    #ImmuneSystem 
    #immunotherapy… https://t.co/q7RI7lRKzk
    []
    Low key hoping every Lib takes their #FluShot and #COVIDVaccine when it is available! 
    
    😂🤣
    
    We don’t need them repr… https://t.co/iQm4LCVQAm
    2020-10-27 03:12:04 Low key hoping every Lib takes their #FluShot and #COVIDVaccine when it is available! 
    
    😂🤣
    
    We don’t need them repr… https://t.co/iQm4LCVQAm
    []
    Episode 11 is here! @Shawn_Cortese #constitutionalmoment #COVIDVaccine #FossilFuels Check out our podcast, The Trut… https://t.co/GI7NGKTCBA
    2020-10-27 03:03:06 Episode 11 is here! @Shawn_Cortese #constitutionalmoment #COVIDVaccine #FossilFuels Check out our podcast, The Trut… https://t.co/GI7NGKTCBA
    []
    @nistula Now such studies seem to be favourite pastime of scientists...Every other day, they come up with weird fin… https://t.co/flt4hDcirB
    2020-10-27 02:56:03 @nistula Now such studies seem to be favourite pastime of scientists...Every other day, they come up with weird fin… https://t.co/flt4hDcirB
    []
    There is a very significant political and diplomatic incentive for China to be the first in the race to the develop… https://t.co/XXTsTHbuq5
    2020-10-27 02:22:36 There is a very significant political and diplomatic incentive for China to be the first in the race to the develop… https://t.co/XXTsTHbuq5
    []
    Distribution of corona virus vaccine is becoming election manifesto is utterly shame on the govt..
    #CovidVaccine… https://t.co/M8c2sbuIbU
    2020-10-27 02:14:19 Distribution of corona virus vaccine is becoming election manifesto is utterly shame on the govt..
    #CovidVaccine… https://t.co/M8c2sbuIbU
    []
    “This is a day of hope for the citizens of Israel,” Defense Minister Benny Gantz said. “Just two months ago, I rece… https://t.co/VntmQLVSrp
    2020-10-27 02:11:43 “This is a day of hope for the citizens of Israel,” Defense Minister Benny Gantz said. “Just two months ago, I rece… https://t.co/VntmQLVSrp
    []
    AHMED: TRUE vaccine safety will not be known until we have tens of millions vaccinated and followed for prolonged p… https://t.co/Vr3MQX44mJ
    2020-10-27 02:01:00 AHMED: TRUE vaccine safety will not be known until we have tens of millions vaccinated and followed for prolonged p… https://t.co/Vr3MQX44mJ
    []
    AHMED: news on #COVIDVaccine VERY positive - Oxford ⁦@AstraZeneca⁩ candidate induces ROBUST immune response in over… https://t.co/LgW7JaUjRU
    2020-10-27 01:55:06 AHMED: news on #COVIDVaccine VERY positive - Oxford ⁦@AstraZeneca⁩ candidate induces ROBUST immune response in over… https://t.co/LgW7JaUjRU
    []
    However, God’s Elect now called in the KINGDOM OF GOD have NOTHING TO FEAR! “[THE NAME] of the LORD is [A STRONG TO… https://t.co/eNdB8E7g0C
    2020-10-27 01:40:57 However, God’s Elect now called in the KINGDOM OF GOD have NOTHING TO FEAR! “[THE NAME] of the LORD is [A STRONG TO… https://t.co/eNdB8E7g0C
    []
    Important to understand the tech behind the vaccines but have a feeling letting some folks know the delivery vector… https://t.co/YUBzvwwDen
    2020-10-27 00:48:30 Important to understand the tech behind the vaccines but have a feeling letting some folks know the delivery vector… https://t.co/YUBzvwwDen
    []
    My colleague @natetabak w/some important #COVIDVaccine logistics developments . . .  Canada needs logistics help wi… https://t.co/8ptS2EzcfP
    2020-10-27 00:08:12 My colleague @natetabak w/some important #COVIDVaccine logistics developments . . .  Canada needs logistics help wi… https://t.co/8ptS2EzcfP
    []
    You know what Santa is an anagram of...🔥😈🔥  #COVID__19 #COVIDVaccine #vaccines #BillGatesBioTerrorist https://t.co/s3OpEFQ6mJ
    2020-10-27 00:07:19 You know what Santa is an anagram of...🔥😈🔥  #COVID__19 #COVIDVaccine #vaccines #BillGatesBioTerrorist https://t.co/s3OpEFQ6mJ
    []
    THE @WHO backed #CovidVaccine by #AstraZenecaOXFORD .. apparently works well in Seniors too.(Phase3) results. 
    and… https://t.co/L39jZC44Wr
    2020-10-26 23:56:33 THE @WHO backed #CovidVaccine by #AstraZenecaOXFORD .. apparently works well in Seniors too.(Phase3) results. 
    and… https://t.co/L39jZC44Wr
    []
    Not satisfied w/ their attempts to discredit #HydroxyChloroquine, &amp; even #COVIDVaccine studies, the Left is now att… https://t.co/Z5uZFK8etX
    2020-10-26 23:40:04 Not satisfied w/ their attempts to discredit #HydroxyChloroquine, &amp; even #COVIDVaccine studies, the Left is now att… https://t.co/Z5uZFK8etX
    []
    PLEASE BE TRUE!!! #COVIDVaccine 
    
    https://t.co/GEPClcsGWc
    2020-10-26 23:32:45 PLEASE BE TRUE!!! #COVIDVaccine 
    
    https://t.co/GEPClcsGWc
    []
    What's the worse that could happen 🤔 😛 #COVID19 #COVIDVaccine https://t.co/2xVvcjr4J2
    2020-10-26 22:25:28 What's the worse that could happen 🤔 😛 #COVID19 #COVIDVaccine https://t.co/2xVvcjr4J2
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry; I did! 
    
    #BePartofResearch… https://t.co/GXL9OAUPm2
    2020-10-26 22:20:23 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry; I did! 
    
    #BePartofResearch… https://t.co/GXL9OAUPm2
    []
    $nvax for sake of transparency, i made hard decision+ lowered exposure in final trading min due to delays:
    
    - p2 da… https://t.co/k2oYXtQlrP
    2020-10-26 22:10:28 $nvax for sake of transparency, i made hard decision+ lowered exposure in final trading min due to delays:
    
    - p2 da… https://t.co/k2oYXtQlrP
    []
    By me via #OPENnews #Covid_19 #COVID #COVID19france #COVIDVaccine #covideurope #europe #covidspain #coviditaly… https://t.co/3QD3dTeLL5
    2020-10-26 21:38:49 By me via #OPENnews #Covid_19 #COVID #COVID19france #COVIDVaccine #covideurope #europe #covidspain #coviditaly… https://t.co/3QD3dTeLL5
    []
    @NadineDorries So why are healthcare workers being conned into being guinea pigs to trial a covid vaccine. No thank… https://t.co/CV8C7oPO3c
    2020-10-26 21:04:47 @NadineDorries So why are healthcare workers being conned into being guinea pigs to trial a covid vaccine. No thank… https://t.co/CV8C7oPO3c
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/IkOMfIN38P
    2020-10-26 20:59:04 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/IkOMfIN38P
    []
    WATCH: Americans share concerns over COVID vaccine
    https://t.co/BpK5GYyXHZ @CBSThisMorning @CBSNews @k_stephensonMD… https://t.co/TzuVvf4Fta
    2020-10-26 20:44:51 WATCH: Americans share concerns over COVID vaccine
    https://t.co/BpK5GYyXHZ @CBSThisMorning @CBSNews @k_stephensonMD… https://t.co/TzuVvf4Fta
    []
    Blue-and-white #COVIDVaccine  coming up!
    
    If all works fine, Israel's vaccine #Brilife should gain approval by July… https://t.co/nL5l7ECA0e
    2020-10-26 20:21:42 Blue-and-white #COVIDVaccine  coming up!
    
    If all works fine, Israel's vaccine #Brilife should gain approval by July… https://t.co/nL5l7ECA0e
    []
    Overcoming the Top Challenges of #COVID19 Vaccine Distribution https://t.co/png2PlsmhZ #CovidVaccine #KFF #SDOH
    2020-10-26 20:20:59 Overcoming the Top Challenges of #COVID19 Vaccine Distribution https://t.co/png2PlsmhZ #CovidVaccine #KFF #SDOH
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/bxCPOwYnBS
    2020-10-26 20:03:51 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/bxCPOwYnBS
    []
    When there is a #COVIDVaccine, some vulnerable patients may not be able to get it. T-Cell therapy could provide pro… https://t.co/Xdb0od60Z2
    2020-10-26 19:55:28 When there is a #COVIDVaccine, some vulnerable patients may not be able to get it. T-Cell therapy could provide pro… https://t.co/Xdb0od60Z2
    []
    And there might never be a #COVIDVaccine just there wasn’t for the Spanish flu or lots of other viruses - we need t… https://t.co/60zO5ysCU5
    2020-10-26 19:45:01 And there might never be a #COVIDVaccine just there wasn’t for the Spanish flu or lots of other viruses - we need t… https://t.co/60zO5ysCU5
    []
    None of the #COVIDVaccine trials are doing well.
    NIH Pauses Eli Lilly COVID-19 Antibody Trial Due to ‘Safety Concer… https://t.co/TmSgtEJPsd
    2020-10-26 19:42:18 None of the #COVIDVaccine trials are doing well.
    NIH Pauses Eli Lilly COVID-19 Antibody Trial Due to ‘Safety Concer… https://t.co/TmSgtEJPsd
    []
    Edward Jones-Lopez, an infectious disease expert at @KeckMedUSC, answers frequently asked questions on a potential… https://t.co/cECsHw46sJ
    2020-10-26 19:37:06 Edward Jones-Lopez, an infectious disease expert at @KeckMedUSC, answers frequently asked questions on a potential… https://t.co/cECsHw46sJ
    []
    @NewsBytesApp Its not needed to be dependent on #government 
    More than 50% #India can pay for #COVIDVaccine… https://t.co/KsG8izGlrI
    2020-10-26 19:34:35 @NewsBytesApp Its not needed to be dependent on #government 
    More than 50% #India can pay for #COVIDVaccine… https://t.co/KsG8izGlrI
    []
    There is something so messed up about trying to sway #SantaClaus to spread your propoganda. Why don't dress up… https://t.co/ir6pQbQkiy
    2020-10-26 19:25:28 There is something so messed up about trying to sway #SantaClaus to spread your propoganda. Why don't dress up… https://t.co/ir6pQbQkiy
    []
    Getting a #COVIDVaccine implies more than a chemical formula. In this tweet, @anticorruption’s chair @DeliaFerreira… https://t.co/21vdpCOEqc
    2020-10-26 19:24:00 Getting a #COVIDVaccine implies more than a chemical formula. In this tweet, @anticorruption’s chair @DeliaFerreira… https://t.co/21vdpCOEqc
    []
    I am not emotionally prepared for #SantaGate
    
    #COVIDVaccine https://t.co/4vg8ipqH4P
    2020-10-26 19:21:12 I am not emotionally prepared for #SantaGate
    
    #COVIDVaccine https://t.co/4vg8ipqH4P
    []
    @PattyHajdu what's going on here!? Please resign &amp; move to China. #UNshill #cbc #canada #cndnpoli #covid #covid19… https://t.co/2rPZiJ4L1B
    2020-10-26 19:20:12 @PattyHajdu what's going on here!? Please resign &amp; move to China. #UNshill #cbc #canada #cndnpoli #covid #covid19… https://t.co/2rPZiJ4L1B
    []
    What if France will be the first to Invent #COVIDVaccine 
    
    Would you still #Boycott_French_Products 
    
    #WeSupportFrance
    2020-10-26 19:07:49 What if France will be the first to Invent #COVIDVaccine 
    
    Would you still #Boycott_French_Products 
    
    #WeSupportFrance
    []
    #Winteriscoming! Recent stats have shown a dramatic increase in #COVID cases in 75% of the US. Watch more on HHS Se… https://t.co/fI0nUZDu2K
    2020-10-26 19:00:19 #Winteriscoming! Recent stats have shown a dramatic increase in #COVID cases in 75% of the US. Watch more on HHS Se… https://t.co/fI0nUZDu2K
    []
    Vaccines could be quite near to approval. 
    The young should be asked to not get themselves vaccinated unless they a… https://t.co/tiaqVMJvek
    2020-10-26 18:19:38 Vaccines could be quite near to approval. 
    The young should be asked to not get themselves vaccinated unless they a… https://t.co/tiaqVMJvek
    []
    This is good news, if true!!!
    
    This means that the AstraZeneca #COVIDVaccine has higher safety (less adverse effect… https://t.co/GODkgqOSGB
    2020-10-26 17:58:28 This is good news, if true!!!
    
    This means that the AstraZeneca #COVIDVaccine has higher safety (less adverse effect… https://t.co/GODkgqOSGB
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/vyoypoMXSi
    2020-10-26 17:54:46 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/vyoypoMXSi
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/nklgTseZNh
    2020-10-26 17:45:22 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/nklgTseZNh
    []
    Experts say there will be a clear answer on whether a safe and effective #COVIDVaccine has been developed by early… https://t.co/zCkxu76uYy
    2020-10-26 17:43:48 Experts say there will be a clear answer on whether a safe and effective #COVIDVaccine has been developed by early… https://t.co/zCkxu76uYy
    []
    Empty roads, vacant pandals..it was a strange Durga Pujo this year! With a second wave of the outbreak just around… https://t.co/3WT2BBaWfG
    2020-10-26 17:30:19 Empty roads, vacant pandals..it was a strange Durga Pujo this year! With a second wave of the outbreak just around… https://t.co/3WT2BBaWfG
    []
    A very useful article for all health professions CPD on #COVIDVaccine progress in @PJOnline_News thankyou… https://t.co/neXrRtHgom
    2020-10-26 17:25:31 A very useful article for all health professions CPD on #COVIDVaccine progress in @PJOnline_News thankyou… https://t.co/neXrRtHgom
    []
    WHO chief contraindicates vaccine nationalism
    #UrbanUpdate #NewsAlert #COVID__19 #COVIDVaccine #WHO #COVID19 
    
    Read… https://t.co/WKNuRODEIU
    2020-10-26 17:24:45 WHO chief contraindicates vaccine nationalism
    #UrbanUpdate #NewsAlert #COVID__19 #COVIDVaccine #WHO #COVID19 
    
    Read… https://t.co/WKNuRODEIU
    []
    I’ve signed up to the NHS Covid-19 Vaccine Research Registry because I want to contribute to keeping the vulnerable… https://t.co/aBRYCA6jRJ
    2020-10-26 17:20:27 I’ve signed up to the NHS Covid-19 Vaccine Research Registry because I want to contribute to keeping the vulnerable… https://t.co/aBRYCA6jRJ
    []
    Reading @beckershr 4 takeaways from the FDA vaccine advisory panel meeting https://t.co/9hUSdDDoUO 
    via @BeckersHR… https://t.co/ux1UUm3zBa
    2020-10-26 17:17:44 Reading @beckershr 4 takeaways from the FDA vaccine advisory panel meeting https://t.co/9hUSdDDoUO 
    via @BeckersHR… https://t.co/ux1UUm3zBa
    []
    FDA COVID-19 Vaccine Process Is 'Thoughtful And Deliberate,' Says Former FDA Head https://t.co/tU4nKfWIrE 
    via… https://t.co/loL0I2qoWH
    2020-10-26 17:16:16 FDA COVID-19 Vaccine Process Is 'Thoughtful And Deliberate,' Says Former FDA Head https://t.co/tU4nKfWIrE 
    via… https://t.co/loL0I2qoWH
    []
    Vaccine storage issues could leave 3B people without access https://t.co/paVL9YV9oh via @StarTribune 
    --
    #vaxnews… https://t.co/NVp4GX1SPH
    2020-10-26 17:15:35 Vaccine storage issues could leave 3B people without access https://t.co/paVL9YV9oh via @StarTribune 
    --
    #vaxnews… https://t.co/NVp4GX1SPH
    []
    In a recent Q&amp;A with @EPM_Magazine, Rafael Teixeira, President of World Courier and ICS, speaks about the challenge… https://t.co/jp4iQScTUF
    2020-10-26 17:03:05 In a recent Q&amp;A with @EPM_Magazine, Rafael Teixeira, President of World Courier and ICS, speaks about the challenge… https://t.co/jp4iQScTUF
    []
    As COVID-19 vaccines roll out, we are encouraged the state to consider caseworkers and direct care staff as priorit… https://t.co/ytGZJpwZwC
    2020-10-26 17:01:06 As COVID-19 vaccines roll out, we are encouraged the state to consider caseworkers and direct care staff as priorit… https://t.co/ytGZJpwZwC
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/144IFQ2J33
    2020-10-26 16:58:05 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/144IFQ2J33
    []
    Facepalm.  That's all I can do. #COVIDVaccine https://t.co/ODMsNSam52
    2020-10-26 16:56:39 Facepalm.  That's all I can do. #COVIDVaccine https://t.co/ODMsNSam52
    []
    “the timeline shows there is zero chance a vaccine plays a meaningful role in the next three months.” —… https://t.co/fp8S56Wks7
    2020-10-26 16:53:09 “the timeline shows there is zero chance a vaccine plays a meaningful role in the next three months.” —… https://t.co/fp8S56Wks7
    []
    Israel Institute for Biological Research to begin clinical trials for its new #COVID__19 vaccine
    #clinicaltrials… https://t.co/dSmmxXdtKj
    2020-10-26 16:46:29 Israel Institute for Biological Research to begin clinical trials for its new #COVID__19 vaccine
    #clinicaltrials… https://t.co/dSmmxXdtKj
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry!
    I did!
    #BePartofResearch… https://t.co/HluG0spy2e
    2020-10-26 16:42:25 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry!
    I did!
    #BePartofResearch… https://t.co/HluG0spy2e
    []
    I am part of the NHS Covid-19 Vaccine Research Registry, please think about joining. #BePartofResearch #COVIDVaccine
    2020-10-26 16:34:57 I am part of the NHS Covid-19 Vaccine Research Registry, please think about joining. #BePartofResearch #COVIDVaccine
    []
    #Covidvaccine hopes rise, #Oxford shot prompts immune response in old, young.
    
    https://t.co/Nc6qGIrIDf
    2020-10-26 16:30:21 #Covidvaccine hopes rise, #Oxford shot prompts immune response in old, young.
    
    https://t.co/Nc6qGIrIDf
    []
    Gilead's Veklury gets full approval; AstraZeneca's Tagrisso heads toward second indication in NSCLC; another use fo… https://t.co/QHaN53G0R1
    2020-10-26 16:13:03 Gilead's Veklury gets full approval; AstraZeneca's Tagrisso heads toward second indication in NSCLC; another use fo… https://t.co/QHaN53G0R1
    []
    @trom771 @midnightlament6 The last laugh will be on tRump. #DumpTrump2020 Get the straight jacket ready for his fin… https://t.co/mDZHUZDjxd
    2020-10-26 15:52:03 @trom771 @midnightlament6 The last laugh will be on tRump. #DumpTrump2020 Get the straight jacket ready for his fin… https://t.co/mDZHUZDjxd
    []
    Great thread. These 4 segments take about 4 minutes and will get you up to speed on the latest on #covid,… https://t.co/qUmgLt9RJu
    2020-10-26 15:34:04 Great thread. These 4 segments take about 4 minutes and will get you up to speed on the latest on #covid,… https://t.co/qUmgLt9RJu
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/W3qZXAWbnC
    2020-10-26 15:18:55 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/W3qZXAWbnC
    []
    Sign up to be contacted about coronavirus vaccine research
    The WORLD needs your help to find a vaccine!… https://t.co/5v6VZTMBUG
    2020-10-26 15:16:47 Sign up to be contacted about coronavirus vaccine research
    The WORLD needs your help to find a vaccine!… https://t.co/5v6VZTMBUG
    []
    A report has claimed that British national pharmaceutical AstraZeneca’s vaccine for combating the novel coronavirus… https://t.co/15IModsxaU
    2020-10-26 15:16:20 A report has claimed that British national pharmaceutical AstraZeneca’s vaccine for combating the novel coronavirus… https://t.co/15IModsxaU
    []
    #complianceMonday Follow this link to How @CDCgov Is Making #COVIDー19 Recommendations: https://t.co/pnVDxfz36W… https://t.co/PZxo2TpfVC
    2020-10-26 15:10:00 #complianceMonday Follow this link to How @CDCgov Is Making #COVIDー19 Recommendations: https://t.co/pnVDxfz36W… https://t.co/PZxo2TpfVC
    []
    Oxford #CovidVaccine Draws Immune Response Among Young, Old: AstraZeneca https://t.co/jv5kMy34ag Stupid and arrogan… https://t.co/Ep1xnjt45L
    2020-10-26 15:08:58 Oxford #CovidVaccine Draws Immune Response Among Young, Old: AstraZeneca https://t.co/jv5kMy34ag Stupid and arrogan… https://t.co/Ep1xnjt45L
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/yIQ25hBgRm
    2020-10-26 15:03:12 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/yIQ25hBgRm
    []
    I’m volunteering to be part of the Bradford Research Covid-19 vaccination trials. Register to join in.… https://t.co/2jsYcqp6vM
    2020-10-26 15:01:40 I’m volunteering to be part of the Bradford Research Covid-19 vaccination trials. Register to join in.… https://t.co/2jsYcqp6vM
    []
    @TexasHHSC presented their COVID-19 distribution plan last week based on the @CDC's vaccine guidelines. Critical po… https://t.co/JBN6wDm8KS
    2020-10-26 15:01:08 @TexasHHSC presented their COVID-19 distribution plan last week based on the @CDC's vaccine guidelines. Critical po… https://t.co/JBN6wDm8KS
    []
    Good news keeps coming out on the various vaccines/I know people keep saying they don’t want to be first/remember/I… https://t.co/W6oLLzKBv8
    2020-10-26 14:57:59 Good news keeps coming out on the various vaccines/I know people keep saying they don’t want to be first/remember/I… https://t.co/W6oLLzKBv8
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch … https://t.co/9vBpgDeinE
    2020-10-26 14:53:10 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch … https://t.co/9vBpgDeinE
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/HcWNlcjnis
    2020-10-26 14:51:26 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/HcWNlcjnis
    []
    The SARS outbreak was in 2003. 17yrs later there’s no vaccine or cure. Yet we’re expected to believe that a vaccine… https://t.co/lEZRVSOS2B
    2020-10-26 14:47:59 The SARS outbreak was in 2003. 17yrs later there’s no vaccine or cure. Yet we’re expected to believe that a vaccine… https://t.co/lEZRVSOS2B
    []
    Coronavirus vaccine by Oxford-AstraZeneca produces immune response among adults - Finally some good news! This soun… https://t.co/UPZ63XZzPa
    2020-10-26 14:46:50 Coronavirus vaccine by Oxford-AstraZeneca produces immune response among adults - Finally some good news! This soun… https://t.co/UPZ63XZzPa
    []
    Livestream Tuesday 10/27 8:45A Central US
    The great @DrPaulOffit returns to JAMA's Q&amp;A series to provide an update… https://t.co/rlaAVhNifQ
    2020-10-26 14:43:15 Livestream Tuesday 10/27 8:45A Central US
    The great @DrPaulOffit returns to JAMA's Q&amp;A series to provide an update… https://t.co/rlaAVhNifQ
    []
    Oxford #CovidVaccine Draws Immune Response Among Young, Old: AstraZeneca https://t.co/LYDs630DAH https://t.co/wLJWs6y4XQ
    2020-10-26 14:38:10 Oxford #CovidVaccine Draws Immune Response Among Young, Old: AstraZeneca https://t.co/LYDs630DAH https://t.co/wLJWs6y4XQ
    []
    Be part of the solution #COVID19 #COVIDVaccine  https://t.co/a96jckDNeb
    2020-10-26 14:31:35 Be part of the solution #COVID19 #COVIDVaccine  https://t.co/a96jckDNeb
    []
    ETHealthworld | UK hospital told to prepare for Oxford Covid vaccine in November: Report #CovidVaccine #Oxford… https://t.co/xjITgFqT6h
    2020-10-26 14:30:25 ETHealthworld | UK hospital told to prepare for Oxford Covid vaccine in November: Report #CovidVaccine #Oxford… https://t.co/xjITgFqT6h
    []
    #biotwitter Today $SPPI approval delay due to inspection of foreign facilities located in South Korea which has a 1… https://t.co/iIvkA4lPV6
    2020-10-26 14:26:28 #biotwitter Today $SPPI approval delay due to inspection of foreign facilities located in South Korea which has a 1… https://t.co/iIvkA4lPV6
    []
    If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/CxHahi7QUW
    2020-10-26 14:23:51 If you have a spare 5 minutes, sign up to the NHS Covid-19 Vaccine Research Registry, I did! #BePartofResearch… https://t.co/CxHahi7QUW
    []
    Hopeful results for #COVIDVaccine trials among #elderly https://t.co/C2NCzWjcxf
    2020-10-26 13:55:46 Hopeful results for #COVIDVaccine trials among #elderly https://t.co/C2NCzWjcxf
    []
    I’m looking to speak to somebody that works in the health profession, or is in the ‘vulnerable’ category with Covid… https://t.co/WQdOjFxV8X
    2020-10-26 13:50:28 I’m looking to speak to somebody that works in the health profession, or is in the ‘vulnerable’ category with Covid… https://t.co/WQdOjFxV8X
    []
    #BharatBiotech set for phase-3 trials of COVAXIN
    
    #India's first indigenously developed #COVIDvaccine, #COVAXIN wil… https://t.co/77PlNUpcmo
    2020-10-26 13:33:05 #BharatBiotech set for phase-3 trials of COVAXIN
    
    #India's first indigenously developed #COVIDvaccine, #COVAXIN wil… https://t.co/77PlNUpcmo
    []
    .@WHO chief @DrTedros said the only way to recover from the #CoronavirusPandemic was together and by making sure po… https://t.co/W87at3WbsH
    2020-10-26 13:24:54 .@WHO chief @DrTedros said the only way to recover from the #CoronavirusPandemic was together and by making sure po… https://t.co/W87at3WbsH
    []
    AstraZeneca: Our coronavirus vaccine triggers adult immune response
    
    #covid #vaccine #CovidVaccine #azd1222… https://t.co/tcms9UYGoY
    2020-10-26 13:12:41 AstraZeneca: Our coronavirus vaccine triggers adult immune response
    
    #covid #vaccine #CovidVaccine #azd1222… https://t.co/tcms9UYGoY
    []
    Coronavirus: Boots announces 12-minute COVID tests
    https://t.co/Bqm8DGeS9I ⁦@BootsUK⁩ Great news&amp; reassuring £120 i… https://t.co/PVmr6EzaYm
    2020-10-26 13:06:28 Coronavirus: Boots announces 12-minute COVID tests
    https://t.co/Bqm8DGeS9I ⁦@BootsUK⁩ Great news&amp; reassuring £120 i… https://t.co/PVmr6EzaYm
    []
    Potentially exciting new regarding the @Oxford #COVIDVaccine 
    - Similar immune response in older and younger adults… https://t.co/ahF9fDmBMc
    2020-10-26 13:05:04 Potentially exciting new regarding the @Oxford #COVIDVaccine 
    - Similar immune response in older and younger adults… https://t.co/ahF9fDmBMc
    []
    Light at the end of the COVID tunnel?...
    https://t.co/er2mQ6Mu0f
    #COVID19 #COVIDVaccine #Vaccine #AstraZeneca
    2020-10-26 12:56:46 Light at the end of the COVID tunnel?...
    https://t.co/er2mQ6Mu0f
    #COVID19 #COVIDVaccine #Vaccine #AstraZeneca
    []
    @KeithCo30475934 Several news sources reported that a vaccine for COVID will likely require two doses. The research… https://t.co/VwPMlXAXFv
    2020-10-26 12:49:52 @KeithCo30475934 Several news sources reported that a vaccine for COVID will likely require two doses. The research… https://t.co/VwPMlXAXFv
                                                  Cleaned
    0   2020-10-27 06:16:59,"b'I\xe2\x80\x99m helping....
    1   2020-10-27 06:02:16,"b'#Covid vaccine (unappro...
    2   2020-10-27 06:01:00,b'The Trump administration...
    3   2020-10-27 05:30:11,"b'Egypt reaches deal with...
    4   2020-10-27 04:41:45,b'But we are supposed to t...
    ..                                                ...
    90  2020-10-26 14:23:51,"b'If you have a spare 5 m...
    91  2020-10-26 13:55:46,b'Hopeful results for #COV...
    92  2020-10-26 13:50:28,"b'I\xe2\x80\x99m looking ...
    93  2020-10-26 13:33:05,"b""#BharatBiotech set for...
    94  2020-10-26 13:24:54,b'.@WHO chief @DrTedros sa...
    
    [95 rows x 1 columns]



```python
!pip install pandas
```

    Requirement already satisfied: pandas in /opt/anaconda3/lib/python3.8/site-packages (1.0.5)
    Requirement already satisfied: numpy>=1.13.3 in /opt/anaconda3/lib/python3.8/site-packages (from pandas) (1.18.5)
    Requirement already satisfied: python-dateutil>=2.6.1 in /opt/anaconda3/lib/python3.8/site-packages (from pandas) (2.8.1)
    Requirement already satisfied: pytz>=2017.2 in /opt/anaconda3/lib/python3.8/site-packages (from pandas) (2020.1)
    Requirement already satisfied: six>=1.5 in /opt/anaconda3/lib/python3.8/site-packages (from python-dateutil>=2.6.1->pandas) (1.15.0)



```python
pip install tweepy
```

    Collecting tweepy
      Downloading tweepy-3.9.0-py2.py3-none-any.whl (30 kB)
    Requirement already satisfied: six>=1.10.0 in /opt/anaconda3/lib/python3.8/site-packages (from tweepy) (1.15.0)
    Collecting requests-oauthlib>=0.7.0
      Downloading requests_oauthlib-1.3.0-py2.py3-none-any.whl (23 kB)
    Requirement already satisfied: requests[socks]>=2.11.1 in /opt/anaconda3/lib/python3.8/site-packages (from tweepy) (2.24.0)
    Collecting oauthlib>=3.0.0
      Downloading oauthlib-3.1.0-py2.py3-none-any.whl (147 kB)
    [K     |████████████████████████████████| 147 kB 3.8 MB/s eta 0:00:01
    [?25hRequirement already satisfied: chardet<4,>=3.0.2 in /opt/anaconda3/lib/python3.8/site-packages (from requests[socks]>=2.11.1->tweepy) (3.0.4)
    Requirement already satisfied: idna<3,>=2.5 in /opt/anaconda3/lib/python3.8/site-packages (from requests[socks]>=2.11.1->tweepy) (2.10)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/anaconda3/lib/python3.8/site-packages (from requests[socks]>=2.11.1->tweepy) (1.25.9)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/anaconda3/lib/python3.8/site-packages (from requests[socks]>=2.11.1->tweepy) (2020.6.20)
    Requirement already satisfied: PySocks!=1.5.7,>=1.5.6; extra == "socks" in /opt/anaconda3/lib/python3.8/site-packages (from requests[socks]>=2.11.1->tweepy) (1.7.1)
    Installing collected packages: oauthlib, requests-oauthlib, tweepy
    Successfully installed oauthlib-3.1.0 requests-oauthlib-1.3.0 tweepy-3.9.0
    Note: you may need to restart the kernel to use updated packages.


# **Question 2**

(30 points). Write a python program to **clean the text data** you collected above and save the data in a new column in the csv file. The data cleaning steps include:

(1) Remove noise, such as special characters and punctuations.

(2) Remove numbers.

(3) Remove stopwords by using the [stopwords list](https://gist.github.com/sebleier/554280).

(4) Lowercase all texts

(5) Stemming. 

(6) Lemmatization.


```python
# remove special characters
d['Cleaned'] = d['Cleaned'].str.replace("[^a-zA-Z#]", " ")
d['Cleaned'].head(n=100)

#remove punctuations
import re
import string
d['Cleaned']=d['Cleaned'].str.replace('[{}]'.format(string.punctuation), '')
d['Cleaned'].head(n=100)

from nltk.corpus import stopwords
#remove stop words
import nltk
nltk.download('stopwords')
stop=stopwords.words('english')

d['Cleaned']=d['Cleaned'].apply(lambda x:" ".join(x for x in x.split() if x not in stop))
d['Cleaned'].head(n=100)

#lower casing
d['Cleaned']=d['Cleaned'].str.lower()
d['Cleaned'].head(n=100)

#stemming
from nltk.stem import PorterStemmer
st=PorterStemmer()

d['Cleaned']=d['Cleaned'].apply(lambda x: " ".join([st.stem(word) for word in x.split()]))
d['Cleaned'].head(n=100)

#lemmatization
from textblob import Word

import nltk
nltk.download('wordnet')
d['Cleaned']=d['Cleaned'].apply(lambda x: " ".join([Word(word).lemmatize() for word in x.split()]))
d['Cleaned'].head(n=100)

d.to_csv("clean_text.csv",index= False)

colname = ['Date','Uncleaned']
df1 = pd.read_csv("tweets.csv",names= colname)
df1

df2 = pd.read_csv("clean_text.csv")
df3 = df1.join(df2)

df3.to_csv("twitter_final.csv")
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-8-100677e931bb> in <module>
          1 # remove special characters
    ----> 2 d['Cleaned'] = d['Cleaned'].str.replace("[^a-zA-Z#]", " ")
          3 d['Cleaned'].head(n=100)
          4 
          5 #remove punctuations


    NameError: name 'd' is not defined



```python
pip install textblob
```

    Requirement already satisfied: textblob in /opt/anaconda3/lib/python3.8/site-packages (0.15.3)
    Requirement already satisfied: nltk>=3.1 in /opt/anaconda3/lib/python3.8/site-packages (from textblob) (3.5)
    Requirement already satisfied: click in /opt/anaconda3/lib/python3.8/site-packages (from nltk>=3.1->textblob) (7.1.2)
    Requirement already satisfied: tqdm in /opt/anaconda3/lib/python3.8/site-packages (from nltk>=3.1->textblob) (4.47.0)
    Requirement already satisfied: regex in /opt/anaconda3/lib/python3.8/site-packages (from nltk>=3.1->textblob) (2020.6.8)
    Requirement already satisfied: joblib in /opt/anaconda3/lib/python3.8/site-packages (from nltk>=3.1->textblob) (0.16.0)
    Note: you may need to restart the kernel to use updated packages.



```python
pip install --user -U nltk
```

    Requirement already up-to-date: nltk in /opt/anaconda3/lib/python3.8/site-packages (3.5)
    Requirement already satisfied, skipping upgrade: regex in /opt/anaconda3/lib/python3.8/site-packages (from nltk) (2020.6.8)
    Requirement already satisfied, skipping upgrade: tqdm in /opt/anaconda3/lib/python3.8/site-packages (from nltk) (4.47.0)
    Requirement already satisfied, skipping upgrade: click in /opt/anaconda3/lib/python3.8/site-packages (from nltk) (7.1.2)
    Requirement already satisfied, skipping upgrade: joblib in /opt/anaconda3/lib/python3.8/site-packages (from nltk) (0.16.0)
    Note: you may need to restart the kernel to use updated packages.



```python
#saving the data as column in csv file
df2 = pd.read_csv("clean_text.csv")
df3 = df1.join(df2)

df3.to_csv("twitter_final.csv")
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-6-73dd43f3ef1d> in <module>
          1 #saving the clean data as column in csv file
    ----> 2 df2 = pd.read_csv("clean_text.csv")
          3 df3 = df1.join(df2)
          4 
          5 df3.to_csv("twitter_final.csv")


    NameError: name 'pd' is not defined



```python
!pip install Corenlp
```


```python
nltk.download('universal_tagset')


# print(nltk.pos_tag(w,tagset= 'universal'))
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-7-a2a472bb460c> in <module>
    ----> 1 nltk.download('universal_tagset')
          2 
          3 
          4 # print(nltk.pos_tag(w,tagset= 'universal'))


    NameError: name 'nltk' is not defined



```python
texts = df3['Cleaned'].tolist()
```


```python
map(word_tokenize, texts)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-2-2adcfc07be34> in <module>
    ----> 1 map(word_tokenize, texts)
    

    NameError: name 'word_tokenize' is not defined


# **Question 3**

(30 points). Write a python program to conduct **syntax and structure analysis** of the clean text you just saved above. The syntax and structure analysis includes: 

(1) Parts of Speech (POS) Tagging: Tag Parts of Speech of each word in the text, and calculate the total number of N(oun), V(erb), Adj(ective), Adv(erb), respectively.

(2) Constituency Parsing and Dependency Parsing: print out the constituency parsing trees and dependency parsing trees of all the sentences. Using one sentence as an example to explain your understanding about the constituency parsing tree and dependency parsing tree.

(3) Named Entity Recognition: Extract all the entities such as person names, organizations, locations, product names, and date from the clean texts, calculate the count of each entity.


```python
#POS Tagging and counting
from nltk import pos_tag_sents
nltk.download('averaged_perceptron_tagger')
WtoPOS = pos_tag_sents( df3['Cleaned'].apply(word_tokenize).tolist() )
```

    [nltk_data] Downloading package averaged_perceptron_tagger to
    [nltk_data]     /Users/melissavang/nltk_data...
    [nltk_data]   Package averaged_perceptron_tagger is already up-to-
    [nltk_data]       date!



    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-74-54177352b8a3> in <module>
          2 from nltk import pos_tag_sents
          3 nltk.download('averaged_perceptron_tagger')
    ----> 4 WtoPOS = pos_tag_sents( df3['Cleaned'].apply(word_tokenize).tolist() )
    

    NameError: name 'word_tokenize' is not defined



```python
df3['POS_Tag'] = WtoPOS
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-10-92189fc8e47f> in <module>
    ----> 1 df3['POS_Tag'] = WtoPOS
    

    NameError: name 'WtoPOS' is not defined



```python
!pip install spacy
```

    Collecting spacy
      Downloading spacy-2.3.2-cp38-cp38-macosx_10_9_x86_64.whl (10.1 MB)
    [K     |████████████████████████████████| 10.1 MB 10.9 MB/s eta 0:00:01
    [?25hRequirement already satisfied: numpy>=1.15.0 in /opt/anaconda3/lib/python3.8/site-packages (from spacy) (1.18.5)
    Requirement already satisfied: tqdm<5.0.0,>=4.38.0 in /opt/anaconda3/lib/python3.8/site-packages (from spacy) (4.47.0)
    Collecting thinc==7.4.1
      Downloading thinc-7.4.1-cp38-cp38-macosx_10_9_x86_64.whl (2.1 MB)
    [K     |████████████████████████████████| 2.1 MB 9.9 MB/s eta 0:00:01
    [?25hCollecting preshed<3.1.0,>=3.0.2
      Downloading preshed-3.0.2-cp38-cp38-macosx_10_9_x86_64.whl (112 kB)
    [K     |████████████████████████████████| 112 kB 6.8 MB/s eta 0:00:01
    [?25hCollecting blis<0.5.0,>=0.4.0
      Downloading blis-0.4.1-cp38-cp38-macosx_10_9_x86_64.whl (3.7 MB)
    [K     |████████████████████████████████| 3.7 MB 7.1 MB/s eta 0:00:01
    [?25hCollecting murmurhash<1.1.0,>=0.28.0
      Downloading murmurhash-1.0.2-cp38-cp38-macosx_10_9_x86_64.whl (19 kB)
    Requirement already satisfied: setuptools in /opt/anaconda3/lib/python3.8/site-packages (from spacy) (49.2.0.post20200714)
    Collecting srsly<1.1.0,>=1.0.2
      Downloading srsly-1.0.2-cp38-cp38-macosx_10_9_x86_64.whl (183 kB)
    [K     |████████████████████████████████| 183 kB 3.5 MB/s eta 0:00:01
    [?25hCollecting catalogue<1.1.0,>=0.0.7
      Downloading catalogue-1.0.0-py2.py3-none-any.whl (7.7 kB)
    Collecting cymem<2.1.0,>=2.0.2
      Downloading cymem-2.0.3-cp38-cp38-macosx_10_9_x86_64.whl (31 kB)
    Collecting plac<1.2.0,>=0.9.6
      Downloading plac-1.1.3-py2.py3-none-any.whl (20 kB)
    Collecting wasabi<1.1.0,>=0.4.0
      Downloading wasabi-0.8.0-py3-none-any.whl (23 kB)
    Requirement already satisfied: requests<3.0.0,>=2.13.0 in /opt/anaconda3/lib/python3.8/site-packages (from spacy) (2.24.0)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/anaconda3/lib/python3.8/site-packages (from requests<3.0.0,>=2.13.0->spacy) (2020.6.20)
    Requirement already satisfied: chardet<4,>=3.0.2 in /opt/anaconda3/lib/python3.8/site-packages (from requests<3.0.0,>=2.13.0->spacy) (3.0.4)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/anaconda3/lib/python3.8/site-packages (from requests<3.0.0,>=2.13.0->spacy) (1.25.9)
    Requirement already satisfied: idna<3,>=2.5 in /opt/anaconda3/lib/python3.8/site-packages (from requests<3.0.0,>=2.13.0->spacy) (2.10)
    Installing collected packages: wasabi, blis, murmurhash, srsly, cymem, preshed, catalogue, plac, thinc, spacy
    Successfully installed blis-0.4.1 catalogue-1.0.0 cymem-2.0.3 murmurhash-1.0.2 plac-1.1.3 preshed-3.0.2 spacy-2.3.2 srsly-1.0.2 thinc-7.4.1 wasabi-0.8.0



```python
df3['Cleaned'][0]
```




    'b xe x x help day sinc first vaccin side effect feel good bepartofresearch covidvaccin nh xe x xa http co jyldrppuxi'




```python
import spacy
from spacy import displacy

nlp = spacy.load("en_core_web_sm")
for i in range(0,100):
  text = nlp(df3['Cleaned'][i])
  sentence_spans = list(text.sents)
  displacy.render(sentence_spans, style='dep', jupyter=True, options={'distance': 90})
```


    ---------------------------------------------------------------------------

    OSError                                   Traceback (most recent call last)

    <ipython-input-62-0afe242178b5> in <module>
          2 from spacy import displacy
          3 
    ----> 4 nlp = spacy.load("en_core_web_sm")
          5 for i in range(0,100):
          6   text = nlp(df3['Cleaned'][i])


    /opt/anaconda3/lib/python3.8/site-packages/spacy/__init__.py in load(name, **overrides)
         28     if depr_path not in (True, False, None):
         29         warnings.warn(Warnings.W001.format(path=depr_path), DeprecationWarning)
    ---> 30     return util.load_model(name, **overrides)
         31 
         32 


    /opt/anaconda3/lib/python3.8/site-packages/spacy/util.py in load_model(name, **overrides)
        173     elif hasattr(name, "exists"):  # Path or Path-like to model data
        174         return load_model_from_path(name, **overrides)
    --> 175     raise IOError(Errors.E050.format(name=name))
        176 
        177 


    OSError: [E050] Can't find model 'en_core_web_sm'. It doesn't seem to be a shortcut link, a Python package or a valid path to a data directory.


**Write your explanations of the constituency parsing tree and dependency parsing tree here (Question 3-2):** 


```python

```




    '\nWrite your explanations of the constituency parsing tree and dependency parsing tree here\n\n\n\n'


