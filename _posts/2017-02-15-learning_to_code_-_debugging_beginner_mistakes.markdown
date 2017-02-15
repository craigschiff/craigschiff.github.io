---
layout: post
title:  "Learning to Code - debugging beginner mistakes"
date:   2017-02-14 20:44:11 -0500
---


At the end of week one of the Flatiron School Web Development program we created a command line application. This was a pretty cool opportunity to test out some of our Ruby progress while integrating API’s in a functional and somewhat open-ended matter. While working through this the one bug that gave us the most trouble was in how we connected the user’s response to our array of possibilities. We wanted to iterate through our array to find the match, while also giving the user specific feedback in the event there is no match. 

While implementing this we created an if / elsif statement for a response if there is no match but when testing the app we realized that we kept getting no results even when typing in something that definitely should have matched. Ultimately we found that we were running the if / elsif on each element of our array rather than on the specific outcome (i.e. whether there are any matches at all). 

Our first instinct was to create a dummy variable that we can flip from nil to true if a match is found and then run the elsif below on if the dummy is nil. 
  
location = get_user_input
dummy = nil
locations.each do |place, coordinates|
		 if location == place.downcase
				@location_data = coordinates
				run
				dummy = "yay"
		 end
 end
 if location == "exit"
		exit
 elsif dummy == nil && location != "exit"
		puts "Please try again."
		call
end



This functionally worked, but in hindsight is needlessly complex. When looking to refactor we found another option that is both more intuitive and slightly easier to implement. We could start the function by running a .include? on the array of options (in this case locations.keys) and setting the if .include? to pull the location entered (only applicable if the .include? evaluates to true and thus the location entered is among our set of possibilities) and elsif to immediately present the alternative options. This option benefits from being more intuitive to the reader and saves a few lines of code too.

location = get_user_input
if locations.keys.include?(location)
	 locations.each do |place, coordinates|
				if location == place.downcase
				@location_data = coordinates
				run
		 end
elsif location == "exit"
		exit
elsif location != "exit"
		puts "Please try again."
		call
end

Anyway, this was not any great detective work or coding mastery to find and utilize .include, but with this post I am moreso tying to highlight the problem, debugging, and then refactoring process as a coding beginner. Going forward I will certainly not make this mistake again. 


