---
title: Weird error in Blogdown/Hugo
summary: Eerie behaviour by Hugo
author: Harshvardhan
date: '2021-07-05'
slug: weird-error-in-blogdown-hugo
categories:
  - package
  - blog
tags: []
---

Yesterday, I was working on my blog post about my first useR. Somehow, no matter how much I tried, it kept showing me 404 error when I opened that new blog. The new blog didn't show up in `Writings` page either. I tried running `blogdown::check_site()` multiple times but it always told me it was working perfectly.

The really weird part is that it worked perfectly in the `localsite`.

I searched for help online — on Hugo forum, Stackoverflow and multiple other sites. I landed on some interesting tutorials that suggested me to change my `baseURL` to something other than `/`, against the  Hugo advised best practice. I tried that as well, didn't break anything but didn't mend my error as well.

So, I copy pasted another blog into the new blog and checked if that still shows 404 error. Surprisingly, it didn't. I concluded the culprit was whatever I was writing in the blog — which wasn't more than a paragraph. I started changing the `YAML` content one by one, pushing every change to Github, Netlify and refreshing fervently to check if the error still existed.

> There I found it! The error was in the date. As soon as I changed the date to yesterday, it worked perfectly. 

But why this error? I remembered distinctly I had published blogs on the same day as today. Probably because at the time of publishing it was July 5, 2021 in my local time (India), whereas in UTC or USA it was still July 4, 2021. Hugo doesn't allow publishing with future dates. 

Weird, right?
