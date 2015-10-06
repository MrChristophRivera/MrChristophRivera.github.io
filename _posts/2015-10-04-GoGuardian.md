---
layout: post
title: Classifying Websites with GoGuardian
published: true
---


For my [Insight Data Science](http://insightdatascience.com) Project for the Fall Session of 2015 I opted to work with [GoGuardian](https://www.goguardian.com), a rapidly expanding educational startup based in Los Angeles. This startup develops a Chrome extension that is installed in student chrome books. 

Slides are located [here](https://speakerdeck.com/christopherrivera/insight-project). 
 
This extension monitors the websites that students visit and classifies them into one of 70 different categories. By collecting data on student website usage, it enables teachers and administrators to gain insights into how student web usage might influence their academic performance, In addition, the extension allows for teachers to filter out distracting and or potentially harmful websites.  
 
### The problem. To Classify or not to Classify. 
Being able quickly to classify websites is crucial to GoGuardian’s business. To do this, they have been using two complementary approaches, one that uses a lookup table of previously classified websites and another that searches for keywords. While both methods have proved to be effective, the web is [massive](http://www.internetlivestats.com/total-number-of-websites/) and students are immensely curious. To be able to scale, GoGuardian has decided to develop its own house machine learning classifiers for classing the web and this is the task that I agreed to work on in collaboration with GoGuardian.  

### The approach. 
GoGuardian previously had 
