---
title: Handling PDF Files using Docsplit and Ruby on Rails
description: Handle PDF one Gem at a time!
date: '2019-11-29T16:58:20.657Z'
layout: single
author: Agustinus Theodorus
categories: ['tech']
keywords: []
slug: >-
  /handling-pdf-files-using-docsplit-and-ruby-on-rails
header:
  teaser: https://cdn-images-1.medium.com/max/800/0*7jenzJUEPTWB9rOr.png
comments:
  provider: "disqus"
  disqus:
    shortname: "agustinustheo"
---

A little backstory, Ruby on Rails is a framework used for web development. Many companies use Ruby on Rails Github, Gojek and Airbnb. [Ruby on Rails was created by David Heinemeier in 2003](https://code.tutsplus.com/articles/ruby-on-rails-study-guide-the-history-of-rails--net-29439). Since it’s inception a lot has changed for this framework and developers are finding more complex uses for it. Ruby on Rails is based on the programming language Ruby and making small libraries to help development is a common practice among Rails users. 

![](https://cdn-images-1.medium.com/max/800/0*7jenzJUEPTWB9rOr.png)

If you have developed a Rails application you must have heard of the term Gems. Gems are like libraries that are made to help simplify your Rails developing experience. In this tutorial, we will be looking into one of those Gems. [Docsplit](https://documentcloud.github.io/docsplit/)!

![](https://cdn-images-1.medium.com/max/600/0*LHSDKI7HY8cQMPUG.png)

Docsplit is one of my favorite gems. I used this gem for my first Rails project, I noticed the docs can be improved upon and in this tutorial I wish to introduce to you this wonderful gem whilst trying to make clearer documentation on the PDF handling part of this gem.

### Adding Docsplit to your Rails project

Of course, what would be a tutorial without an installation tutorial? Installing the Docsplit gem is like installing any other Gem. Assuming that you are using a Linux system, install GraphicsMagick and Poppler using sudo.

```
sudo apt-get install graphicsmagick  
sudo apt-get install poppler
```

GraphicsMagick and Poppler are mandatory for this gem to work, other dependencies need to be installed for some features to work but are not mandatory. One of the optional dependency that I recommend installing is pdftk. Install pdftk using sudo.

```
sudo apt-get install pdftk
```

After installing the dependencies add this line into your Gemfile.

```
gem 'docsplit'
```

Then before you run your Rails app run this in your command prompt.

```
bundle install
```

Running your bundle will help you install gems that you added in your Gemfile.

### Using Docsplit to Handle PDF

#### Extracting images from PDF

Docsplit allows extracting each page as images. In Docsplit there is an _extract\_images_ function, you can call the function like so:

```
Docsplit.extract_images([file_path], :format => [:jpg])
```

Docsplits _extract\_images_ function has six parameters. **These six parameters accept file paths, file output, file size, output format, output page, and file density.**

The format parameter is mandatory because it is needed to define what format the saved images would be in. file paths parameter is the first parameter to be passed, the file path parameter is relative to the location of the script. For example:

```
--|  
  |-- script.rb  
  |  
  |-- example.pdf
```

Assuming the Ruby script is in the same directory as the PDF file then this is how you use Docsplit.extract\_images

```
Docsplit.extract_images('example.pdf', :format => [:jpg])
```

The second parameter is the file output parameter if the file output parameter is not passed then the extracted images would be saved in the root of the Rails app. Otherwise, the file would be saved to the directory the file output parameter is pointing to.

```
Docsplit.extract_images('example.pdf', output: Rails.root.join('temp'), :format => [:jpg])
```

The size parameter controls the quality of the file output, it defines the desired file resolution.

```
Docsplit.extract_images('example.pdf', :size  => '1000x', :format => [:jpg])
```

Next, the page parameter determines which page would be extracted as an image. If a page parameter is not passed then the function would extract all the pages. Example of using page parameters are like so:

```
Docsplit.extract_images('example.pdf', :page => 1, :format => [:jpg])
```

Lastly, the density parameter. According to the [Docsplit docs](https://documentcloud.github.io/docsplit/) the density parameter controls the DPI to rasterize the page to an image.

#### Extracting PDF metadata

Docsplit allows for PDF metadata extracting. Metadata that can be extracted are author, date, creator, keywords, producer, subject, title, and length. There is only one single parameter needed for this function, the file path parameter.

For example, let’s try getting a PDFs title. To do that we just need to run:

```
Docsplit.extract_title('example.pdf')
```

This function will return the title of the PDF, the return can be stored in a variable to keep the result. To extract other metadata categories we would only need to change the latter of the function.

Let’s do another example, to extract the author we only need to run:

```
Docsplit.extract_author('example.pdf')
```

From these two examples, extracting other types of metadata should be quite straightforward.

#### Splitting PDF

If you intend on extracting certain pages from a multiple page PDF. The extract\_pages method would come in handy. **Before trying to implement this method you need to make sure you have installed pdftk.**

For example, if you want to extract the first page of a PDF you can do:

```
Docsplit.extract_pages('example.pdf', :pages => 1)
```

Otherwise, if you want to extract the third page of a PDF you can do:

```
Docsplit.extract_pages('example.pdf', :pages => 3)
```

To extract a range of pages. In this case page one to three you could do:

```
Docsplit.extract_pages('example.pdf', :pages => 1..3)
```

### Conclusion

Docsplit is one of those gems that you should consider trying. It is a very useful tool for your Rails project when needed. This tutorial was written based on my personal experience in using Docsplit and much of it was based on the [original Docsplit docs](https://documentcloud.github.io/docsplit/) (do check it out if you are planning to use it). Cheers.