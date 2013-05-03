---
layout: post
title: "SEO for Octopress Websites"
date: 2013-05-03 20:26
comments: true
categories: ["octopress", "theme", "seo"]
keywords: "seo for octopress, seo optimized theme for octopress"
description: "Search Engine Optimizations for Octopress Blogs and Websites"
---

I recently switched my blog from Blogger to Octopress (hosted on
[Github](https://github.com/)) and it's fantastic!!!  Here are the search engine
optimizations I performed on the blog. Most of them are from
[www.yatishmehta.in][yatishmehta], [www.learnaholic.me][learnaholic] and
[nothoughtcontrol.com][nothoughtcontrol].

I've also incorporated these SEO fixes into the classic octopress theme. It is
available at [octopress-classic-no-blog][octopress_classic_no_blog].

<!-- more -->

## Keywords & Description for individual posts

```
---
layout: post
title: "SEO for Octopress Websites"
date: 2013-05-03 20:26
comments: true
categories: ["octopress", "theme", "seo"]

keywords: "keywords, for, this, post"
description: "Description for this post"
---
```

Octopress, by default, when generating a new post using `rake new_post["Title"]`
doesn't include `keywords` and `description` metadata for individual posts. But,
if they are manually added to a post, the generated html will include them.

You could include both manually for each newly created post or modify the
`Rakefile` in the root of the Octopress installation to include these whenever a
new post is created. Find the `new_post` task and modify it as follows:

```ruby
# usage rake new_post[my-new-post] or rake new_post['my new post'] or rake new_post (defaults to "new-post")
desc "Begin a new post in #{source_dir}/#{posts_dir}"
task :new_post, :title do |t, args|
  if args.title
    title = args.title
  else
    title = get_stdin("Enter a title for your post: ")
  end
  raise "### You haven't set anything up yet. First run `rake install` to set up an Octopress theme." unless File.directory?(source_dir)
  mkdir_p "#{source_dir}/#{posts_dir}"
  filename = "#{source_dir}/#{posts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title.to_url}.#{new_post_ext}"
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/&/,'&amp;')}\""
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M')}"
    post.puts "comments: true"
    post.puts "categories: "
    post.puts "keywords: "
    post.puts "description: "
    post.puts "---"
  end
end
```

Similar modifications can be performed for `new_page` task as well.

## Keywords & Description for the main website

Octopress utilizes the `keywords` and `description` of the latest blog post for the
main website. In most cases, this doesn't bode well for the website in terms of
SEO. It is possible to provide a separate set of keywords and description for
the main website. Modify `_config.yml` in the root of the Octopress
installation to include the additional metadata. `description` option would
already be present there.

```
url: http://yoursite.com
title: My Octopress Blog
subtitle: A blogging framework for hackers.
author: Your Name
simple_search: http://google.com/search

description: "Description for the website"
keywords: "keywords, for, the, website"
```

Now modify `source/_includes/head.html`, the file responsible for generating the
html header for the website, to use this metadata when available. The
modifications go after the `author` meta tag.

{% codeblock html source/_includes/head.html %}
{% raw %}

{% capture description %}{% if page.description %}{{ page.description }}{% elsif site.description %}{{ site.description }}{% else %}{{ content | raw_content }}{% endif %}{% endcapture %}
<meta name="description" content="{{ description | strip_html | condense_spaces | truncate:150 }}" />

{% capture keywords %}{% if page.keywords %}{{ page.keywords }}{% elsif site.keywords %}{{ site.keywords }}{% endif %}{% endcapture %}
<meta name="keywords" content="{{ keywords | strip_html | condense_spaces }}" />

{% endraw %}
{% endcodeblock %}

## Remove Redundant /blog Prefix From Octopress URLs

Octopress, by default, generates static websites with a `/blog` prefix attached to
categories, archives and post URLs. This is particularly annoying when the blog
is hosted at `blog.yourdomain.tld` and have funny URLs like
`blog.yourdomain.tld/blog/archives`. I've written about this [before][remove_blog_prefix].

## Single domain for the website

To prevent google from de-valuing content for having two different URLs, one
through `www.yoursite.tld` and another through `yoursite.tld`, it is better to
redirect one to the other with a HTTP 301 status code.

I've redirected `www.xit0.org` to `xit0.org` through a DNS CNAME record. Refer
your domain registar's help pages on setting this up. If you run the
blog/website off Heroku, `rack-rewrite` can be used to achieve this as mentioned
at [www.yatishmehta.in][yatishmehta].

## In Short

These 4 SEO fixes are beneficial to an Octopress website:

* `description` & `keywords` metadata for individual posts/pages
* `description` & `keywords` for the main website
* Remove redundant `/blog` prefix from Octopress URLs
* Single domain for the website

[yatishmehta]: http://www.yatishmehta.in/seo-for-octopress "Yatish Mehta"
[learnaholic]: http://learnaholic.me/2012/10/15/octopress-seo-and-disabling-the-blog-route/ "learnaholic.me"
[nothoughtcontrol]: http://nothoughtcontrol.com/2012/12/16/customizing-octopress-pt-dot-1/ "No Thought Control"
[octopress_classic_no_blog]: https://github.com/pmirshad/octopress-classic-no-blog "Octopress Classic No Blog"
[remove_blog_prefix]: http://xit0.org/2013/04/remove-redundant-slash-blog-prefix-from-octopress-website/ "Remove Redundant /blog Prefix From Octopress Website URLs"















