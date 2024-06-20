Sentiment Analysis on RBC App Reviews

Overview
This project analyzes user reviews for the RBC mobile app on Google Play Store to determine sentiment (positive, negative, neutral).

1) Installation

  a) Clone the repository:
  git clone https://github.com/yourusername/rbc-sentiment-analysis.git
  cd rbc-sentiment-analysis
  b) Install dependencies:
  pip install google_play_scraper pandas

2) Usage

  a) Collect Reviews:
  from google_play_scraper import reviews_all
  
  appid = 'com.rbc.mobile.android'
  rbc_reviews = reviews_all(appid, lang='en', country='ca')
  
  b) Download Lexicon:
  import urllib.request
  
  with urllib.request.urlopen("https://storage.googleapis.com/wd13/lexicon.txt") as url:
      lexicon_file = url.read().decode()
  lexicon = {line.split('\t')[0]: float(line.split('\t')[1]) for line in lexicon_file.split('\n')}
  
  c) Calculate Sentiment Scores:
  import re
  
  def sentiment_score(doc):
      tokens = re.findall('[A-Za-z0-9]+', doc.lower())
      return sum(lexicon.get(token, 0) for token in tokens)
      
  d) Assign Sentiment and Year:
  for review in rbc_reviews:
      review_text = review['content']
      review['sentiment_score'] = sentiment_score(review_text) if review_text else 0
      review['sentiment_flag'] = 'pos' if review['sentiment_score'] > 0 else 'neg' if review['sentiment_score'] < 0 else 'neu'
      review['year'] = review['at'].year
  
  import pandas as pd
  df = pd.DataFrame.from_records(rbc_reviews)

