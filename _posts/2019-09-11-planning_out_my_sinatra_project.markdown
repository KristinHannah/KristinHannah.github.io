---
layout: post
title:      "Planning Out My Sinatra Project "
date:       2019-09-11 23:36:44 +0000
permalink:  planning_out_my_sinatra_project
---


Starting the Sinatra project, I was feeling a little bit overwhelmed with everything we’d just learned. From Rack to Sinatra to ActiveRecord to sessions, then throw in SQLite3 and CSS formatting and my head was swimming with where to start and how to put it all together. For the last project I had dived right in head first, and more than half way through the first week was facing issues getting things to work correctly. After watching a video resource, it seemed that maybe my approach was not the best. So this time around I decided to put aside some time to plan out what I needed to do.  

Luckily, I knew what I wanted to make. I was going to make application to track horror movies users had watched. This idea is inspired in part by a horror movie club my friends bring together during the month of October each year. A few of them do a horror movie challenge- trying to watch as many as they can, but aiming for at least 1 per day in October. Plus, with a theme like horror movies I could make the layout of the website spooky and fun. 

To plan out my project, I headed over to WeWork and found an open conference room with a whiteboard. I knew my project would follow the MVC model. MVC stands for "Model-View-Controller". Within the project directory, their is an app director. Inside the app directory is a directory for the models, views, and controllers. 

This is how I mapped out each:

**Models**
1. Users:
attributes: Username, Password
user has_many movies 
2. Movies: 
attributes: Title, Director, Date Watched, Location Watched, Rating (1-10), Review. 
movie belongs_to user

**Views**
1. Users-        -New (need form for user sign up)
                          -Login (need form to sign user in)
										 
2. Movies-     -New (need form for user to input movie)
                          -Show (need to show the movie selected by the user)
									      	-Edit (need form for user to edit their previously entered movie)
										      -Index (need to show all of the user's movies that they've put in) 
													
3. General-   -404 page 
                         -Layout page 
										     -Landing page 
										 
**Controllers**	
I need to give credit here. I referenced this lesson and specifically this chart while beginning to plan out my controllers, and found it was very helpful:
https://learn.co/tracks/online-software-engineering-structured/sinatra/activerecord/sinatra-restful-routes
										
1.applicationcontroller:
helper methods 

*Sinatra tip:*
to show your own error page when a user navigates to a route that doesn't exist:
not_found do 
 erb :'/error'
end

2. moviescontroller routes:

Get    '/movies' to show all movies 
           '/movies/new' to show form for a new movie 
			     '/movies/:id' to see a specific movie's post
           'movies/:id/edit' to show form to edit a specific movie post 
					 
Post '/movies' to send the new movie form to 

Patch '/movies/:id' to send the edited movie form 

Delete '/movies/:id' to delete a movie 

3. userscontroller routes:

Get    '/signup' to show signup form 

Post  '/users' to send the signup form to 

4. sessionscontroller routes:

Get    '/login'  to show the login form 
           '/logout' to log a user out 
					 
Post  '/sessions' to send the login info to

Taking the time to sit down and map out all the things I would need to build made me realize I had a lot of work in front of me, but it also made it seem more manageable. Now I knew the basics of what I needed. 
