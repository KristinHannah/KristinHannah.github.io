---
layout: post
title:      "What I Learned from Restarting my CLI Project Halfway through Project Week"
date:       2019-07-14 23:36:45 +0000
permalink:  what_i_learned_from_restarting_my_cli_project_halfway_through_project_week
---


Starting out on Project week I was beyond excited to put together my own gem. I attended the study group for class that week that went over set up, and thought I pretty much understood how to set it up and what to do. So I started coding, working on mimicking things I’d done in past labs dealing with classes and/or scraping. By the end of the first week I had written lots of code but … it wasn’t working. 

At this point, I started looking at resources on learn and that our cohort lead had been kind enough to supply. I decided to re-start the project following along with a video that went through set up in detail. 

From this, I learned two things- always use the resources available, and that the set up is really important. After successfully setting up my project, the methods I had written out in the first draft worked and I was able to re-use methods from my scraper class and class that created objects from the information scraped. Now, I’m taking a look at that first version I abandoned to see where the set up went wrong. I'm going to walk you through my process:

**bin/run:**

Original: 
has the shebang indicating the file should be interpreted as ruby
		requires the version file 
		Instantiates an instance of the CLI class and calls on the “call” method
		require_relative the environment folder 

Final:
		The only different is that it doesn’t require the version file.

When I try to run the program by typing “bin/run” in terminal I get an error:

```
/usr/local/rvm/rubies/ruby-2.6.1/lib/ruby/site_ruby/2.6.0/rubygems/core_ext/kernel_require.rb:54:in `require': cannot load such file -- ../lib/cli/version.rb (LoadError)
```

I changed “require” to “require_relative” in the original. 

My new error message indicates an unitialized constant “Horoscopes::CLI”. 

**Environment.rb**:
	The environment folder is where things really start to get different. In my final version I require all other ruby gems, and have “require_relative” for all files in the environment folder. In the original, I have require relative for the version file, and the cli file. 

Okay, so now I’m moving my “require ‘nokogiri’” and others into the environment folder of my original project, and using “require_relative” to require my scraper class, and zodiac sign class. I’m removing all those requirements from each of the individual cli.rb, scraper.rb and zodiacsign.rb files. 

Still getting the same error message.

**gemspec**:

Missing from the Original, is the following:

```
spec.add_development_dependency "pry"
  
 spec.add_dependency "nokogiri"
```

The development dependency is useful for when the gem is being modifying, and the other dependency is needed for when the program is running. I’ve added in these dependencies to the Original.

Still no luck with the error message. 

I decided to run “bundle install” after making these edits. 
I got these error messages:

```
[!] There was an error parsing `Gemfile`:
[!] There was an error while loading `cli.gemspec`: uninitialized constant CLI
Did you mean?  CGI. Bundler cannot continue.
```

Looks like I’m heading over to my Gemfile. 

**Gemfile**:

In the Original, I have a lot more in my Gemfile. Here’s what is extra in the Gemfile:

```
require "open-uri"
require "nokogiri”
group: development do 
Gem ‘pry’
end’
```

Now I know that the dependencies are all in the gemspec file and required in the environment, so I’m going to remove them from the gemfile. 

I’m still getting the same error message above. In like 8 of the Original gemspec file I have:
  ```
	spec.version  = CLI::VERSION
```

I look in the version folder. My module is named “Horoscopes”, not “CLI”. I’m changing “CLI” to “Horoscopes”, because the VERSION is indicated in that module.

“bundle install” will now run successfully.


But I’m still getting the same uninitialized constant error message. 

I’m going back to the beginning and looking at bin/run. Earlier I had said the only difference was that the version file wasn’t required in the Original. That’s not technically true. The environment file appears first. I’m putting the environment file, where everything (all the files and required gems) is stored, ahead of where I’m instantiating an instance of my cli class. Now, my cli class was working. 

Spending the time to go through all of this makes me realize how important the set up of a program is, and understanding the importance of what information is going in each file. Going through all this, it makes sense that the environment file has to come first sequentially. Without that by bin/run file doesn’t know about all of the classes and methods I’m trying to access. Writing out the scraper class, and the cli class is a lot more fun. But after this project I will never try to brush past intricacies of the gemspec file or the environment file again. 
