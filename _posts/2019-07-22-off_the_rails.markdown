---
layout: post
title:      "Off The Rails!"
date:       2019-07-22 00:09:29 -0400
permalink:  off_the_rails
---


So this rails project definitely felt a little unweildy, but all in all was another good project experience. My number one take away for this project is definitely get your routes right and make sure your CRUD functionality is solid. Aside from really mapping out your associations in the beginning, this aspect for me was the most time-consuming. I think these features seem deceptively simple until you start trying to expand the user functionality in your app, and then it's easy to break these backbone features. 

I think the most interesting thing I tackled in this project was ensuring that my child object forms had the ability to create my parent objects at the point at which the user was filling out and submitting the child form. I did so by creating custom setters/getters. For example:

```
def candidate_name=(candidate_name)
    if !candidate_name.nil? && candidate_name != ""
      first_name = candidate_name.split(" ")[0]
      last_name = candidate_name.split(" ")[1]

      if last_name == nil
        last_name = " "
      end

      self.candidate = Candidate.find_or_create_by(first_name: first_name, last_name: last_name)
    end
  end

  def candidate_name
    self.candidate ? self.candidate.full_name : nil
  end

  def contributor_name=(contributor_name)
    self.contributor = Contributor.find_or_create_by(name: contributor_name)
  end

  def contributor_name
    self.contributor ? self.contributor.name : nil
  end
```

And utilizing my strong params in my controller and using those custom setters/getters in my form in my view as opposed to using the object ids:



```
def contribution_params
    params.require(:contribution).permit(:amount, :date, :contributor_name, :candidate_name, :contributor_id, :candidate_id)
  end
```

```
<%= form_for @contribution do |f| %>
  <%= render partial: 'layouts/errors', locals: {object: @contribution} %>

  <%= f.label :contributor_name, "Contributor Name" %>
  <%= f.text_field :contributor_name, placeholder: "ex. Jane Smith" %><br><br>

  <%= f.label :candidate_name, "Candidate Name" %>
  <%= f.text_field :candidate_name, placeholder: "ex. Jane Smith" %><br><br>

  <%= f.label :amount, "Amount $" %>
  <%= f.text_field :amount, placeholder: "1000" %><br><br>

  <%= f.label :date %>
  <%= f.text_field :date, placeholder: "mm/dd/yyyy" %><br><br>

  <%= f.submit 'Contribute' %>
<% end %>
```



