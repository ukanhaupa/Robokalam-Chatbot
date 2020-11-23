# Robokalam-Chatbot

In [1]:
import nltk
import numpy
import random
import string
In [2]:
chatbots_file = open('chatbot.txt','r',errors = 'ignore')
content = chatbots_file.read()
content = content.lower()
nltk.download('punkt')
nltk.download('wordnet')
sentence_tokens = nltk.sent_tokenize(content)
word_tokens = nltk.word_tokenize(content)
[nltk_data] Downloading package punkt to
[nltk_data]     C:\Users\Gaurav\AppData\Roaming\nltk_data...
[nltk_data]   Package punkt is already up-to-date!
[nltk_data] Downloading package wordnet to
[nltk_data]     C:\Users\Gaurav\AppData\Roaming\nltk_data...
[nltk_data]   Package wordnet is already up-to-date!
In [3]:
lemmer = nltk.stem.WordNetLemmatizer()
In [4]:
def lem_tokens(tokens):
    return[lemmer.lemmatize(token) for token in tokens]
In [5]:
remove_punct_dict = dict((ord(punct),None) for punct in string.punctuation)
In [6]:
print(string.punctuation)
!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
In [7]:
def lem_normalize(text):
    return lem_tokens(nltk.word_tokenize(text.lower().translate(remove_punct_dict)))
In [8]:
GREETING_INPUTS = ("hello","hi","what's up","sup","hey")
GREETING_RESPONSES = ("hi","hey","*nods*","hi there","hello","I am glad! You are talking to me")

def greeting(sentence):
    for word in sentence.split(" "):
        if word.lower() in GREETING_INPUTS:
            return random.choice(GREETING_RESPONSES)
In [9]:
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
In [10]:
def response(user_response):
    robo_response = ''
    sentence_tokens.append(user_response)
    TfidfVec = TfidfVectorizer(tokenizer = lem_normalize, stop_words = 'english')
    tfidf = TfidfVec.fit_transform(sentence_tokens)
    
    values = cosine_similarity(tfidf[-1],tfidf)
    idx = values.argsort()[0][-2]
    flat = values.flatten()
    flat.sort()
    
    req_tfidf = flat[-2]
    if(req_tfidf==0):
        robo_response = robo_response + "I am sorry! I don't understand you"
    else:
        robo_response = robo_response + sentence_tokens[idx]
    return robo_response
In [11]:
flag = True
print("Chiku: My name is chiku. I will answer your queries about chatbots. If you want to exit, type bye!")
while(flag==True):
    user_response = input()
    user_response = user_response.lower()
    if(user_response!='bye'):
        if(user_response=='thanks' or user_response=='thank you'):
            flag = False
            print("Chiku: You are welcome")
        else:
            if(greeting(user_response)!=None):
                print("Chiku: "+greeting(user_response))
            else:
                print("Chiku: ")
                print(response(user_response))
                sentence_tokens.remove(user_response)
    else:
        flag = False
        print("Chiku: Bye, take care! ")
Chiku: My name is chiku. I will answer your queries about chatbots. If you want to exit, type bye!
hi
Chiku: hi there
what is a turing machine?
Chiku: 
d:\chrome downloads\lib\site-packages\sklearn\feature_extraction\text.py:300: UserWarning: Your stop_words may be inconsistent with your preprocessing. Tokenizing the stop words generated tokens ['ha', 'le', 'u', 'wa'] not in stop_words.
  'stop_words.' % sorted(inconsistent))
background
in 1950, alan turing's famous article "computing machinery and intelligence" was published, which proposed what is now called the turing test as a criterion of intelligence.
who are the classic historic early chatbots?
Chiku: 
development
the classic historic early chatbots are eliza (1966) and parry (1972).more recent notable programs include a.l.i.c.e., jabberwacky and d.u.d.e (agence nationale de la recherche and cnrs 2006).
ok thank you
Chiku: 
I am sorry! I don't understand you
thank you
Chiku: You are welcome
