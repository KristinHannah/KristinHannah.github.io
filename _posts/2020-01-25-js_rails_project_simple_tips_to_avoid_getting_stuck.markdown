---
layout: post
title:      "JS Rails Project: Simple Tips to Avoid Getting Stuck "
date:       2020-01-25 21:50:54 +0000
permalink:  js_rails_project_simple_tips_to_avoid_getting_stuck
---


Starting out the JavaScript and Rails Project I was definitely intimidated. Since starting the JavaScript sections, things just haven’t clicked for me as well as they had for Rails and Ruby. I started my project by following along with a past tutorial recording. It was helpful for setting up my project. Once I reached the end of the recording, I was on my own, and again faced with that feeling of ‘so what do I do now?’ 

	I knew what I wanted my project to have, so I started working on it out, piece by piece. I made progress and felt pretty good. But there were a few things that I got stuck on. It seems almost inevitable when working a project that something doesn’t work as expected. When things stop working, it can be easy to start getting frustrated. Here’s some of my tips to avoid frustration, and a place where I got stuck and how I worked through it:

## Tip number 1: Always Test What You Just Did. 
This was a take away from the tutorial I watched and probably my favorite one. Pretty much the entire time I was working on this project I had my rails server running and was looking at the index file on my google chrome browser. Sometimes when I moved something, or added something, or deleted something it had unexpected consequences. All of a sudden none of the information that was being rendered before was coming up, or something else would seem to go wrong. I was glad that I was checking things so frequently, because it was infinitely easier to determine where the issue lied. I could comment out what I just wrote, and check that things worked like they had before. 

## Tip number 2: Debugger.
When you add ‘debugger’ into your JavaScript, the chrome browser will stop the process at that point. From there, you can go into the console in the developer tools in the browser and play around. You can understand what ‘this’ is, or what other variables are at that point. This was such a helpful tool during this project sometimes things are not what you think they are. 

## Tip number 3: Don’t Forget About Pry.
When the backend is involved, don’t forget you can throw a ‘binding.pry’ into your code to see what’s going on. This came in handy for me, which transitions nicely into one of the places I got stuck…


## My Issue: 
For my project I created a goal tracking app. You can add a goal you’re working on, and then you can also add actions you’ve taken toward that goal. So I had a “goal” class and an “action” class. A goal ‘has_many’ actions. I set up my goal form first and was able to get a fetch POST request pretty easily. So I modeled the form for actions off of what I had done for goals. 

```
createAction(nameValue, dateValue, goalValue) {
        const action = {
            name: nameValue,
            date: dateValue,
            goal_id: goalValue
        }
return fetch(this.baseUrl, {
            method: 'POST',
            headers: {
                'content-type': 'application/json',
            },
            body: JSON.stringify({ action }),
        }).then(res => res.json())
    }

```

In my console on chrome I see an error: 
500 (internal server error).

Yikes. Why? 

I clicked into the network tab and got a description of the error. It was a NoMethodError, saying  undefined method ‘permit’ for “create”:string. 

I was confused. I could see the action object I made, and there was no “create” in there.

I knew enough that the reference to permit should bring me to my actionsController. The parameters I was permitting matched what I thought I was sending in the action object, “name, date, goal_id”. 

Putting a binding.pry in the create method allowed me to see the parameters that were really being sent. They were not what I thought: 

```
{"action"=>"create", "controller"=>"api/v1/actions"}
```

Now I understood and noted for next time not to name a class “action”. (Who knew? Apparently not me.)

I changed the name of the object in my createAction() method to “newAction”, the variable passed into the fetch request body to “newAction”, and in my action_params I changed it to “params.require(:newAction).permit(:name, :date, :goal_id)”. This fixed the problem.  

