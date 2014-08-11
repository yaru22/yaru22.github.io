---
layout: post
title: "Blogging with Jekyll"

tags: [jekyll]

meta-description: "My first blog post showing how I setup my blog using Jekyll and Amazon S3."
meta-robots: ""
---

I've been meaning to setup [Jekyll static blog][jekyll] for a long time and finally got it working. Since Amazon started supporting static websites on Amazon S3 at the root domain, I decided to take advantage of its massively scaleable infrastructure.

Setting it up was super quick and easy:

```bash
~ $ gem install jekyll
~ $ jekyll new myBlog
~ $ cd myBlog
~/myBlog $ jekyll build
# Your static blog is ready at `_site` directory
```

Locally, I use a super simple http-server built on NodeJs, [serve][serve].

```bash
~/myBlog $ cd _site
~/myBlog/_site $ serve
# Your static blog is served at http://localhost:3000
```

In order to serve your static blog off of Amazon S3, I use [jekyll-s3][jekyll-s3]. Setting it up using jekyll-s3 took less than 5 minutes.

It's now time for me to design my blog =)

Check out the [Jekyll docs][jekyll] for more information on how to get the most out of Jekyll. Happy blogging!

[jekyll]:    http://jekyllrb.com
[jekyll-s3]: https://github.com/laurilehmijoki/jekyll-s3
[serve]:     https://github.com/visionmedia/serve
