---
layout: post
title: "Create Custom Rss Feed for Octopress"
date: 2012-07-24 22:27
comments: true
external-url: 
categories:  [programming, life]

---

Octopress offers an atom.xml feed only. But many other blog providers' default feed is different from this, such as /feed on wordpress. So how to move your existing blog subscribers smoothly from your old blog to Octopress?

Rss feed address can be change by modifying the `subscribe_rss` value in _config.yml. But this only change the url for the feed link on your page. It won't generate a feed there. Then how do you generate a feed there?

## If you have ssh access to your blog server:

just ssh to the server and do   `$ ln -s atom.xml $your_rss_file`, then you are done.

## If you don't have ssh access:

If you host your Octopress on heroku or github, then you have no ssh access to the server. 

### For heroku user
You can redirect the request. Details can be found in the redirect part of this [article](http://approache.com/blog/migrating-from-blogger-to-octopress/), and code can be found [here](https://github.com/dnagir/approache-redirects/blob/master/app.rb).
 
### For github user

#### Simple method:
You can simply copy the atom.xml in your source(not public) directory to any location in the source directory or its sub-directory.

But this method has a little disadvantage, that due to your have two(or more) same feed source files now, Octopress will generate the rss feed more than one time. But that would not be a problem.

#### Better method:
To eliminate this disadvantage, I added a rake task to copy the generated atom.xml to other feed files in the Rakefile as follow:

``` ruby
# Feed files other than atom.xml that needed to be compatible with previous blog
feed_files = ["feed"]

desc "copy atom.xml to feed_files"
task :copyfeeds do
  feed_files.each do |filename|
    cp("#{public_dir}/atom.xml","#{public_dir}/#{filename}")
  end
end


```

And invoke the task before push to github by modify the deploy task in Rakefile:

```ruby
desc "Default deploy task"
task :deploy do
  Rake::Task[:copydot].invoke(source_dir, public_dir)
  Rake::Task[:copyfeeds].invoke
  Rake::Task["#{deploy_default}"].execute
end

```

#### A little problem and how to work around:
Methods above serve well if the feed you want is somehing like rss.xml. But there is still a little problem if your old feed url is like /feed.  

Since in github pages, the content-type of a file is determinate by the file extension. The response content-type of the request to /feed is application/octet-stream rather than text/xml as a normal rss feed. When user clicked on the /feed clink, it eventual downloads the feed file other than displaying the feed content or bringing up the Rss subscribe interface. And even if you add the address to google reader manually , although it can read the content first time you import this feed, you can't get further updates.

What is worse is that github has no support for .htaccess
 or other ways to redirect /feed to atom.xml.  
 
After searching a long time and no solution was found, I almost gave up. But suddenly I came up with an idea: how about look /feed as a directory rather than a file? Then the /feed request eventually redirect to /feed/index.html, and the content-type can be specified in the html.  

I tried that. And it DID WORK!
 
So the solution is :  

* copy your source/atom.xml to source/feed/index.xml if you use the simple method  

or

* set the feed_files = ["feed/index.html"] if you use the better method