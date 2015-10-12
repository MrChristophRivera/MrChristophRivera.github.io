---
layout: post
title: Classifying Websites with GoGuardian
published: true
---






For my [Insight Data Science](http://insightdatascience.com) Project for the Fall Session of 2015 I opted to work with [GoGuardian](https://www.goguardian.com), a rapidly expanding educational startup based in Los Angeles. This startup develops a Chrome extension that is installed onto student chrome books. 

Slides are located [here](https://speakerdeck.com/christopherrivera/insight-project). 
 
This extension monitors which websites that students visit and classifies them into one of 70 different categories. By collecting data on student website usage, it enables teachers and administrators to gain insights into how student web usage might influence academic performance. In addition, the extension allows for teachers to filter out distracting and or potentially harmful websites.  
 
## The problem: To Classify or not to Classify. 
Being able to quickly classify websites is crucial to GoGuardian’s business. To do this, they have employed two complementary approaches; one that uses a lookup table of previously classified websites and another that searches for keywords. While both methods have proved to be effective, the web is [massive](http://www.internetlivestats.com/total-number-of-websites/) and students are immensely curious. To be able to scale, GoGuardian has decided to develop its own house machine learning classifiers for classing the web and this is the task that I agreed to work on in collaboration with GoGuardian.  

### The approach. 
GoGuardian previously had another company classify approximately 2.6 million different host websites into 16 primary and 70 secondary categories. These categories include labels such as News, Timewasting, and Social. GoGuardian was pretty open to several different approaches for tackling this problem and suggested that I might use either a unsupervised learning approach by clustering websites based on their text or that I could use their previously classified websites as a starting point to train a supervised learner. I opted for the latter. 

#### Url Selection and Processing. 
To begin, I obtained the list of 2.6 million previously classified websites. These websites had been classified into 15 primary categories, and 69 secondary categories. The majority of the websites belonged to a single primary category; to simplify the problem, I subset the list to include urls that belonged to only one category. 
 
From the remaining ~2.3 million urls, I selected a subset of the urls. I wrote python scripts using the excellent [Requests](http://docs.python-requests.org/en/latest/) and multiprocessing modules to access the urls and download the resulting HTML documents as a text file onto an AWS 16 node cluster. I downloaded approximately 50,000 websites. 
 
Here is the distribution of the ![The initial count.]({{site.baseurl}}/images/DocumentCount.pdf). 
 
I employed [Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/) (it is indeed beautiful) to parse the HTML documents and extract text belonging to the paragraph, title, link, image, header and metadata tags. I concatenated the text from the different tags into one string and removed all punctuation and numbers. Using my functions, I also counted the number of each of these tags with in the document. 
 
Using [SQL Alchemy](http://www.sqlalchemy.org), I inserted the scraped text along with the tag and word counts into a mysql database for storage and easy access. 
 
#### Training the Machine Learning algorithms. 

I employed pandas to access and manipulate the downloaded text strings. Plotting the distribution of the word length indicated that many of downloaded documents had fewer than 5 words after parsing. To increase the information content, I selected for documents that had greater than or equal to 5 words and, to control for unbalanced classes, I also sub-selected for a smaller subset of documents. 
 
Following sub-selection, I employed the sci-kit learn functions to split the remaining documents into a 75% training and 25% test set; the test set were never touched by the algorithm until testing. I used the sklearn CountVectorizer function to tokenize the document strings, remove stop words and count the word frequencies. I used to the TfidfTransformer function to normalize the word counts across the different categories. 

The number of unique words in the combined text was quite large, totaling >300,000. To reduce the feature complexity (which helped increase computational performance  

The documents had a combined word vocabulary totaling ~ 300,000 word. To reduce the feature complexity (for increasing computational performance, and accuracy), I performed Chi Squared tests on the word matrix to select the k-best most statistically significant words. I then used the transformed data set to train a series of models, Stochastic Gradient Descent Support Vector Classifier, Naïve Bayes and random forest. For each class of model I varied either the regularization parameter or the number of trees (in the case of the random forest). I used the sklearn pipeline objects to facilitate rapid training and cross validation of the models using a series of hyper-parameters. For this I used 10 fold cross validation. 

The models performed similarly, with the random forest classifier having a 62.5 % , which was 7.4 x more accurate than expected compared to random chance of 7.01% (I estimated this by shuffling labels 1000 times).
