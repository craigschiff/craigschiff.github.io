---
layout: post
title:  "Learning React"
date:   2017-04-10 20:58:59 +0000
---


React is challenging in a non-conceptual way. The lifecycle and organization is specific and inelastic, so it generally takes some trial and error to figure it out. But at the same time there are no especially challenging concepts, and really no data manipulation - if you understand javascript and jquery at all you should be able to pick up React relatively quickly.

It is also easy to see why it is a popular framework. The ability to provide all of the performance benfits while allowing a relatively intuitive and structured style is highly appealing. The JSX does take some time to get used to though, and to help I wanted to list out a few common mistakes I have made and adjustments that have been helpful.

* No "if" statements in the return portion of your render method. If this is necessary the ternary operator can work (statement '?' value if statement is true ':' value if statement is false)
* In the return portion of your render function you can send an array of values and React will accurately parse this into a list and display (or create components) depending on what your array holds. This is counterintuitive but very helpful.
* All non-html in return statements need to be put in brackets ({})
* The console can be unreliable if you are using a debugger. You sometimes use your "this" context and the code can look very different given it has already gone through the compiler. However the React developer tools (download the chrome extension) are super helpful
* No debuggers inside the return statement. Place it in the render function above the return if you need it. 

This is obviously a non-exhaustive list and i will return to it and add as I encounter more things. Hope it's helpful.
