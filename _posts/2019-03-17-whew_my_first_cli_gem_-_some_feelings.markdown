---
layout: post
title:      "Whew! My First CLI Gem - Some Feelings"
date:       2019-03-18 02:29:23 +0000
permalink:  whew_my_first_cli_gem_-_some_feelings
---

So, first things first, this first project - building a Ruby CLI gem - started off very intimidating. However, I will skip to the happy ending and say that now that it's successfully completed, this was a really great experience. Absolutely frustrating at times, but only in the best way!

My main takeaway from this project is that it really is true that the only way to get through building something like this is to take it one step at a time, and be flexible. 

Yes, you should have a general understanding of where you'd like to go with your code, but you do need to just start at the beginning and then be methodical. Just get something coded, get it working, and keep moving forward! There's always later to refine things and expand upon things. 

The flexibility aspect, for me, started at the very beginnning with selecting a website to scrape. It was immediately clear that my initial idea/site, while doable, was going to be very labor intensive in terms of scraping. It would have involved many objects (not a bad thing), but also additional links to scrape for each object to be able to capture all the infomation I'd want/need to create my objects' attributes. I had to abandon that quickly in order to keep this project in scope. So yeah, pick a good website. 

After a much more scrapeable site was picked, I encountered probably the most **suprisingly** difficult part of this project: Setting up a local environment on my mac (I decided to take this plunge early), and then actually setting up my project's environment, requiring gems, using bundler, etc. This actually took me a few days. And I definitely did not think this was going to be the thing that had me questioning my sanity. I thought it would be the easiest part. That did not happen.

But since coding is a lot of overcoming obstacles and fixing broken things, here are two issues I felt happiest about moving past, when everything was said and done:

1. My commitment to being able to use `require_all` in order to require all of the files in my lib folder, as opposed to using `require`, or better yet, `require_relative` for each one. 

The latter worked. I wasn't satisfied. I wanted the flexibility that the former had. No more work needing to be done if the files in my lib folder later changed. (Although, ironically, they actually never changed from my initial set-up). So after a lot of googling, and screaming "I don't understand why this isn't working!", I was finally able to install the 'require_all' gem, include it in my Gemfile, and set up my project environment. Again, should have easy, but this was a hurdle. 

2. Getting nokogiri successfully installed and working. This is one where I had to use my resources. (Unfortunately, after an entire day and a half  of thinking I could go it alone. Don't be afraid to ask for help!). 

End result in my case: Also include `spec.add_dependency "nokogiri"` to your gemspec file. 

All's well that ends well though!

Whew!


