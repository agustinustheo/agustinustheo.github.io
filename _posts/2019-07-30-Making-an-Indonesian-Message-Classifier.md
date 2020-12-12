---
layout: single
author: Agustinus Theodorus
title: Making an Indonesian Message Classifier
description: >-
  Want to make your own SMS Classifier? Here I will guide you step-by-step on
  how I assembled an Indonesian SMS Classifier.
date: '2019-07-30T16:46:56.055Z'
categories: []
keywords: []
slug: /making-an-indonesian-message-classifier
header:
  teaser: /assets/img/0__OOA21LWp__huhJtxN.jpg
comments:
  provider: "disqus"
  disqus:
    shortname: "agustinustheo"
---

Ever got a spam message on your phone? Annoying right?!   
I mean from all of the things that could irritate me spam message is one of them. It especially ticks me off when I am waiting for an important text from a special someone and when my phone _lit_ guess what? Some random numbers texted me offering me a free loan. Wonderful.

![](/assets/img/0__OOA21LWp__huhJtxN.jpg)

As a computer scientist, I thought I could tackle the problem while using the project as a college assignment. So essentially I am killing two birds with one stone.

![](/assets/img/0__Yas__UXmkZ4JvqL__x.png)

[**Which is More Promising: Data Science or Software Engineering? | Data Driven Investor**  
_About a month back, while I was sitting at a café and working on developing a website for a client, I found this woman…_www.datadriveninvestor.com](https://www.datadriveninvestor.com/2019/01/23/which-is-more-promising-data-science-or-software-engineering/ "https://www.datadriveninvestor.com/2019/01/23/which-is-more-promising-data-science-or-software-engineering/")[](https://www.datadriveninvestor.com/2019/01/23/which-is-more-promising-data-science-or-software-engineering/)

To start it all of we are going to use python as our programming language, and we will need to install the required libraries, the libraries used for this build are as follows.

nltk  
sklearn  
numpy  
pandas

Copy these to the requirements.txt file on your project’s root folder and run this command.

For Windows:

pip install -r requirements.txt

For Linux (install pip3 first):

pip3 install -r requirements.txt

If you have followed these steps, you are ready to train the SMS classifier model. The model will classify SMS into three categories normal, spam, and promo messages. To create the models there needs to be a dataset, and like all datasets, it needs to be normalized to remove noise.

### Step 1: Making an Indonesian Text Tokenizer

Before we can train the SMS classifier we will need to make an Indonesian Sentence Tokenizer. To do this we will use NLTK’s **PunktSentenceTokenizer**. Other libraries include:

Libraries imported for this build.

First, a huge corpus filled with Indonesian literature will need to be loaded After we opened them we will need to read and process the texts to remove noise such as symbols, numbers, and emails to replace them with texts easily understood by the classifier.

Function to replace symbols, numbers, and email used in the text.

After processing the texts using the function above, we can train the tokenizer.

Training the PunktSentenceTokenizer.

### Step 2: Let’s process the dataset

To train the classifier we will need a dataset, luckily I have made a dataset of about 200+ spam, promo, and normal SMS each.

Loading dataset into a data frame.

When loading the dataset into a data frame, as always we should remove the noise in the data. To remove noise in an SMS we can use this function.

Function to replace symbols, numbers, and email used in the text.

### Step 3: Collecting the Bag of Words…

To separate classifiers between words, we can use a popular algorithm called Bag of Words. This algorithm will put words into a bag, and get the most used words.

Creating a bag-of-words to then find the features.

After collecting the bag-of-words we need to extract the word features and pair them with their corresponding class.

Attaching features to their corresponding sets.

This algorithm will put words of each class into a bag, making a correspondence between the word and the class involved. Before we can use this bag-of-words as a training set we need to randomize the data so that the classifier would train accurately.

Randomizing the features sets, to increase the accuracy of the classifier when training.

### Step 4: Start Training the Classifier!

First, we must define the classifying algorithms that we will use.

Determining algorithms that are going to be used in the classifier.

Then all we need to do is to train the classifiers!

Training classifiers to generate the model.

### Step 5: Last but not least, start classifying!

Because we have trained the model, we just need to open it and start classifying with it.

Opening the model and classifying text.

There are all kinds of characters used in a standard spam SMS. In this case, there is a unique characteristic in Indonesian spam messages. Spammers like to replace letters with words, for example:

*   1 = i
*   3 = e
*   4 = a
*   5 = s
*   6 = g
*   7 = t
*   8 = b
*   9 = g
*   0 = o

To normalize these types of texts we just need to create a function to decode it.

Function to replace rogue numbers used in the text as a replacement for letters.

In the function above the numbers that don’t have the ‘ribu’, ‘rbu’ or ‘rb’ in the back of the numbers will be replaced with normal letters (‘ribu’, ‘rbu’ or ‘rb’ is the Indonesian shortened slang for thousand).

_Voilà!_ these are the 5 steps to make your own Indonesian SMS Classifier.

If you want to check out my git repo on this subject, click [here](https://github.com/agustinustheo/sms-classifier-model)! I hope you understood this tutorial, have a nice spam-free day!