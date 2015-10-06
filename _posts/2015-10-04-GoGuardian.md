---
layout: post
title: Classifying Websites with GoGuardian
published: true
---


For my [Insight Data Science](http://insightdatascience.com) Project for the Fall Session of 2015 I opted to work with [GoGuardian](https://www.goguardian.com), a rapidly expanding educational startup based in Los Angeles. This startup develops a Chrome extension that is installed onto student chrome books. 

Slides are located [here](https://speakerdeck.com/christopherrivera/insight-project). 
 
This extension monitors which websites that students visit and classifies them into one of 70 different categories. By collecting data on student website usage, it enables teachers and administrators to gain insights into how student web usage might influence academic performance. In addition, the extension allows for teachers to filter out distracting and or potentially harmful websites.  
 
### The problem: To Classify or not to Classify. 
Being able to quickly classify websites is crucial to GoGuardian’s business. To do this, they have employed two complementary approaches; one that uses a lookup table of previously classified websites and another that searches for keywords. While both methods have proved to be effective, the web is [massive](http://www.internetlivestats.com/total-number-of-websites/) and students are immensely curious. To be able to scale, GoGuardian has decided to develop its own house machine learning classifiers for classing the web and this is the task that I agreed to work on in collaboration with GoGuardian.  

### The approach. 
GoGuardian previously had another company classify approximately 2.6 million different host websites into 16 primary and 70 secondary categories. These categories include labels such as News, Timewasting, and Social. GoGuardian was pretty open to several different approaches to tackling this problem and suggested that I might either use a unsupervised learning approach and cluster websites based upon their text or that I could use their previously classified websites as a starting point to train a supervised learner. I opted for the latter. 

To begin, I obtained the list of the 2.6 millions different previously classified websites.  
