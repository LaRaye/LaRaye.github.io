---
layout: post
title:      "BeHold! My Sinatra Art Collection Web App "
date:       2019-05-20 00:17:31 +0000
permalink:  behold_my_sinatra_art_collection_web_app
---

For my sinatra web appplication project, I built an app called BeHold! that allows users to catalog the various works of art that they might have in their personal collections. Mostly, I chose this topic because I felt like it organically had a lot of object relationships that I could explore, i.e., a user has many works of art in their collection and  those works belong to the user. 

To be honest, when I was first brainstorming how I would build my app, I was thinking very ambitously. These works of art not only belong to the user, but to the artists who've created them, or perhaps also to the categories of styles that they fall into, the time periods, etc. However, it became clear very quickly that building that many models and relationships was going to be too much  for the time constraints of the project, as well as pretty repetitive in terms of code. I ultimately had only three models: a User class, a Painting class and a Sculpture class.

In retrospect, the only additional features that I would look forward to learning to include on a project like this would be the visuals for the art. What's an art app without the visuals?  But for now, this project was a great experience, and felt that much closer to being on the way to developing more sophisticated, complex applications. 

As was intended, this project really hammered home the importance of RESTful routes. My main takeaway would definitely be to reiterate how key it is to understand the intricacies of your controller actions in connection to your views. I relied a lot on the labs in this section, as they did a great job of teaching you what you needed to build this app from scratch. 

One of the more simple (deceptively so) hurdles I came across in my code was actually while building my Edit actions. I wanted for users to be able to submit an edit form that only changed the object params for those of  which the user actually provided an input for, and leave all of the others the same if there was no input submitted. Not all that complicated, but I did pause for a moment thinking about how to accomplish this goal. (I also regretted maybe just a little bit including so many attributes for my paintings and scultptures.) To do so, my edit action ended up like this:

```
patch '/paintings/:id' do
    @painting = Painting.find_by_id(params[:id])

    params.each do |k, v|
      if k == 'name' && v != ""
        @painting.name = v
      elsif k == 'artist' && v != ""
        @painting.artist = v
      elsif k == 'date' && v != ""
        @painting.date = v
      elsif k == 'description' && v != ""
        @painting.description = v
      elsif k == 'style' && v != ""
        @painting.style = v
      end
      @painting.save
    end
    redirect to "/paintings/#{@painting.id}"
  end
```

My gut says there's probably a much more elegant way to refactor this, but this did the job it was intended to do. I just iterated through my params hash, checking my key/value pairs, and setting my painting's attributes to those values if the user's input was not an empty string. Thus, if my user only filled in the date field when editing their painting's profile, only the date would be updated.





