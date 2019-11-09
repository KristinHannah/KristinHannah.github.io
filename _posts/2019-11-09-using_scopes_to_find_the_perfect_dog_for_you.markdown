---
layout: post
title:      "Using Scopes to Find the Perfect Dog for You"
date:       2019-11-09 12:27:14 -0500
permalink:  using_scopes_to_find_the_perfect_dog_for_you
---


Before starting my Rails Project, I hadn’t used scope methods before. Lucky for me, they were extremely useful for what I was planning. I decided for my project I would make a website that allowed user’s to put in some information about themselves and be matched with dog breeds that would work for their lifestyle. 

I saved the preferences for the users on a separate preferences table/model, that has columns for user_id and dog_breed_id. On the preferences table I also saved the dogs version of the preferences. For instance if a user answered “true” to the question “Are you or anyone in your household allergic to dogs?”, their preferences would be saved on the preferences table as “hypoallergenic: true”. All dogs that are hypoallergenic also have “hypoallergenic: true” on the preferences table. 

**What is a scope?** A scope allows you to specific a query on a table in your database. You use the scope like you would a class method. So for my above referenced project, the Preferences model would look something like this:

```
class Preferences < ApplicationRecord 

     scope :dog, -> { where.not(dog_breed_id: nil) }

end 

```

Since my preferences table has information for both users and dog breeds, I wanted to make sure I was only getting dog breeds so users wouldn’t be matched with other users. Now, when I call “Preferences.dog”, I’ll get an ActiveRecord::Relation containing only the rows from the table that contain a dog_breed_id. 


**Scopes can accept arguments.** You can feed an argument into a scope. For finding matches, this feature is very important. Users can say what size dog they prefer. Scopes will allow us to get only rows for the Preferences table based on the users response. 

```
class Preferences < ApplicationRecord 

	scope :dog, -> { where.not(dog_breed_id: nil) }
scope :by_size, -> (answer) { where(:size => answer)}

end 

```

Now if I call “Preferences.by_size(“small”)”, I’ll get only rows that have the “small” for their size. 
But didn’t I only want dog breed ids? Good news! Scopes are also chainable. So now I can call “Preferences.dog.by_size(“small”)”, and now I’m only getting rows that have small dog breeds!

