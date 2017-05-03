---
layout: post
title: Deploying lavpathak.com
---

After I decided to build a personal website, I thought about going the route of plan static html and sign up with a hosted blogging service for my blog.
But what is fun in that, right?
Instead I went with generated static website that would give me cleaner solution in terms of keeping content as a text instead of having to maintain it in a database.
Also in future I can plug additional features (eg. Comments section for blog posts) easily and grow the website.

I had heard about Jekyll while back so decided to give it a shot. It was a fun and painless experience.

Jekyll has following system prerequisites
* Linux, Unix or MacOS
* Ruby version 2.0 or above
* [Ruby Gems](https://rubygems.org/pages/download)
* [GCC](https://gcc.gnu.org/install/) and [Make](https://www.gnu.org/software/make/) available in your command line interface


To get started install Jekyll.
```
$ gem install jekyll bundler
```

Now create new website.
```
$ jekyll new mywebsite
$ cd mywebsite
$ jekyll serve
```

After that when you go to ```localhost:4000```, browser will show default website already setup!

You can now change the layout and style as you wish. Instead for creating your layout and styling from scratch, if you want to start from a pre-defined
theme then visit <a href="http://jekyllthemes.org/" target="_blank">http://jekyllthemes.org/</a>.
That should get you started. Fork any of those layouts from github and customize it according to your needs.

To preview your changes before deployment try
```
$ jekyll serve
```

To deploy this app I decided to use <a href="http://travis-ci.org/" target="_blank">Travis CI</a> and amazon S3 buckets.

First create your S3 bucket using AWS console. It should match your domain name (eg. lavpathak.com in my case). After creating your bucket go to bucket policy
under Permissions as shown in the image below.
![img]({{ site.url }}/assets/img/deploying_my_website_img_1.png)

Add following lines in bucket policy. It allows get access on all objects inside the given bucket and make them public.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "MakeItPublic",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::yourbucketname.com/*"
        }
    ]
}
```

Travis-ci needs .travis.yml file in your repository. More info on how to get started on travis-ci can be found at
[https://docs.travis-ci.com/user/getting-started/](https://docs.travis-ci.com/user/getting-started/).

My .travis.yml script looks like following
```
language: ruby
rvm:
  - 2.1
install: gem install jekyll -v 2.4.0 && gem install s3_website
script: jekyll build
after_success: s3_website push
```

As you notice, it sets up my ruby environment first and then installs jekyll and another gem called s3_website.
After that it executes ```jekyll build``` and then pushes the artifact to S3 bucket. s3_website needs another file called s3_website.yml. Which looks like following
```
s3_id: <%= ENV['S3_ACCESS_KEY_ID'] %>
s3_secret: <%= ENV['S3_SECRET_KEY'] %>
s3_bucket: yourbucketname
```

S3_ACCESS_KEY_ID and S3_SECRET_KEY are set as environment variables in my travis-ci build settings. You should **never** checkin your credentials in version control repository.

Thats all it took to automate deployment of my website. After that setup every time I push changes to my repo, it deploys the latest version including new posts to s3 bucket.
There are further improvements that I'd like to make by automatic creation of S3 bucket and setting bucket policy, if it doesn't exists!

Hope this was helpful to you all. Please reach out me if you have any questions. I will add comments section in near future.

-- Lav
