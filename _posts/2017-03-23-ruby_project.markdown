---
layout: post
title:  "Ruby Project "
date:   2017-03-23 08:54:18 -0400
---


We finished our Ruby project last week. It was the most fun i have had programming and allowed us to apply all of the stuff we have learned in creative and thoughtful ways. It offered for a degree of freedom and experimentation that is not present when working on labs.  While a super productive week in terms of improving programming skills, I also learned a lot about different challenges one might face in an eventual job.

We had four people on our team. Fortunately, they were all great to work with. But there are certainly problems you encounter working with others that you don’t see when working alone. For example, when there are multiple ways to do certain things I tend to find the one that works and makes the most sense to me and rely on that going forward. However, when working with teams and interacting with others code one does not have that same luxury. Throughout the project I was seeing different ways of doing things, ways that i had not focused on and did not make as much sense to me, but ways that also worked. This was challenging, especially when building off it or having to break it down and debug it. However, I also realized that this is way more representative of what I would be seeing in any programming job, and learning to understand and work with others’ code is ultimately a valuable skill if not a necessity. From that perspective too the project time was valuable.
The other challenge I kept encountering was the balance between ugly code and just getting it to work. This is a constant trade off for me - anyone knows me is aware that nothing I do is especially elegant in any aspect of my life. My work space is always a mess, my thoughts are scattered, I am generally not the most organized person in the world, and if anything thrive off a degree of organized chaos.  In ways this is evident in my code - it often feels especially satisfying when the long and convoluted code does work. But at the same time, I place a high value on intuitive code - I don't mind the extra lines, but what is there has to be logical and make sense to me and to others. 
Anyway, this manifested itself when working through the filter / search page I was struggling a bit to get the filter (for categories) to apply in conjunction with a search (for an event name).  We wanted a user to be able to search by either one or by both, and to be able to choose as many categories as they wanted in the filter. The final code is below, and needless to say it is ugly. 

```
  def index
    @cat_restore = []
    if !params[:name].blank?
      @name = params[:name]
      @events = Event.where("name like ?", "%#{params[:name]}%").where(active: true)
    end
    if params[:categories]

      @cat_restore = params[:categories].map do |category_id|
        category_id.to_i
      end
      @events_cat = Event.joins(:event_categories).where("category_id = ?", params[:categories][0]).or(Event.joins(:event_categories).where("category_id = ?", params[:categories][1])).or(Event.joins(:event_categories).where("category_id = ?", params[:categories][2])).or(Event.joins(:event_categories).where("category_id = ?", params[:categories][3])).or(Event.joins(:event_categories).where("category_id = ?", params[:categories][4])).or(Event.joins(:event_categories).where("category_id = ?", params[:categories][5]))
    end
    if @events_cat && !@events
      @events = @events_cat.uniq
    elsif @events_cat && @events
      @events.map do |event|
        event if @events_cat.include?(event)
      end
    end

    if !@events
      @events = []
      Event.all.each do |event|
        @events << event if event.active
      end
      if params[:name] || params[:categories]
        flash[:alert] = "No Results were found"
      end
    end
  end
```

Yes, not great. But at least it works. I hope to write about a better way to do this in a future lab. Regardless, solving problems like this was one of the joys of working on the project, and the entire week made me very excited to work with a team on a dedicated project in the future. 
