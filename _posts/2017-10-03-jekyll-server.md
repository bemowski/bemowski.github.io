---
layout: post
title: Installing, and running jekyll locally
tags: 
  - ubuntu
  - linux
  - jekyll
---

Running a jekyll server locally makes testing the markdown, style changes, etc
much easier.  Here's some quick instructions - and issues to run locally.

## To start, install:

```sudo apt-get install jekyll```


## Then jekyll build

### Error - jekyll-sitemap

Once I did this - i tried ```jekyll build``` - however that failed:

```bemo@yoga:/home/bemo/gitroot/jekyll-now>jekyll build
Configuration file: /home/bemo/gitroot/jekyll-now/_config.yml
/usr/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- jekyll-sitemap (LoadError)
	from /usr/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require'
	from /usr/lib/ruby/vendor_ruby/jekyll/plugin_manager.rb:30:in `block in require_gems'
	from /usr/lib/ruby/vendor_ruby/jekyll/plugin_manager.rb:27:in `each'
	from /usr/lib/ruby/vendor_ruby/jekyll/plugin_manager.rb:27:in `require_gems'
	from /usr/lib/ruby/vendor_ruby/jekyll/plugin_manager.rb:19:in `conscientious_require'
	from /usr/lib/ruby/vendor_ruby/jekyll/site.rb:97:in `setup'
	from /usr/lib/ruby/vendor_ruby/jekyll/site.rb:49:in `initialize'
	from /usr/lib/ruby/vendor_ruby/jekyll/commands/build.rb:30:in `new'
	from /usr/lib/ruby/vendor_ruby/jekyll/commands/build.rb:30:in `process'
	from /usr/lib/ruby/vendor_ruby/jekyll/commands/build.rb:18:in `block (2 levels) in init_with_program'
	from /usr/lib/ruby/vendor_ruby/mercenary/command.rb:220:in `block in execute'
	from /usr/lib/ruby/vendor_ruby/mercenary/command.rb:220:in `each'
	from /usr/lib/ruby/vendor_ruby/mercenary/command.rb:220:in `execute'
	from /usr/lib/ruby/vendor_ruby/mercenary/program.rb:42:in `go'
	from /usr/lib/ruby/vendor_ruby/mercenary.rb:19:in `program'
	from /usr/bin/jekyll:15:in `<main>'
```


That is fixed by a ruby install: 

```bemo@yoga:/home/bemo/gitroot/jekyll-now>sudo gem install jekyll-sitemap```

All good - so now jekyll build works: 

```bemo@yoga:/home/bemo/gitroot/jekyll-now>jekyll build
Configuration file: /home/bemo/gitroot/jekyll-now/_config.yml
            Source: /home/bemo/gitroot/jekyll-now
       Destination: /home/bemo/gitroot/jekyll-now/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
                    done in 0.279 seconds.
 Auto-regeneration: disabled. Use --watch to enable.```
 
 Which builds a static version of the site to ```_site``` - but that still isn't
 really what I want.  I want it to serve dynamically.  
 
 ## serve
 
 ```bemo@yoga:/home/bemo/gitroot/jekyll-now>jekyll serve
Configuration file: /home/bemo/gitroot/jekyll-now/_config.yml
            Source: /home/bemo/gitroot/jekyll-now
       Destination: /home/bemo/gitroot/jekyll-now/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
                    done in 0.224 seconds.
 Auto-regeneration: enabled for '/home/bemo/gitroot/jekyll-now'
Configuration file: /home/bemo/gitroot/jekyll-now/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
      Regenerating: 1 file(s) changed at 2017-10-01 08:32:01 ...done in 0.192039214 seconds.
      Regenerating: 1 file(s) changed at 2017-10-02 12:56:43 ...done in 0.126847491 seconds.
      Regenerating: 1 file(s) changed at 2017-10-03 11:18:35 ...done in 0.124718642 seconds.
      Regenerating: 1 file(s) changed at 2017-10-03 11:19:39 ...done in 0.124422203 seconds.
```

This is updating each time I save a new file.  (this one included)


 

