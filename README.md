###How to run the application:
1.Enter 'http://benjaminmartin05.github.io/frontend-nanodegree-mobile-portfolio/' into browser.
2.Click on 'Cam's Pizzeria ' to see updates made to the webpage.

###Steps to Optimize pagespeed insights:

1.Ran jpeg optimizer and jpegtran on profilepic and pizzeria.jpg to minimize files.
This allowed the page speed insights score to be 90 desktop and 77 for mobile.

2.Removed render blocking js by using async tag on google analytics script...doesn't change page speed insights score

3.Removed googleapis fonts link from page. Page speed insights increased 77 to 82 mobile and 90 to 92 desktop.

4.Used media="print" tag on print.css link in index to delay print from render blocking until its needed for print..mobile 82 to 87 and desktop 92 to 94 page speed insights.

5.Created an inlined style sheet in index.html with style.css. mobile page speed increase 87 to 95 and desktop 94 to 97.

###Update moving pizzas in the background:

'phase' variable in updatePositions() function was causing a lot of jank in the frame rates.
I took the document.body.scrollTop/1250 section out of the for loop so that it wasn't having
to calculate this each time the loop ran.

In updatePositions(), I then used .transform instead of .left to change the position of the pizza
without disrupting the normal document flow. Doing this put all the pizzas
on the right side of the page during full screen viewing. I fixed the problem
by adding left:0px; after position: fixed; on the .mover class in styles.css.

Finally, I was rendering many pizzas that were having to be painted, but couldn't be seen.
In document.addEventListener('DOMContentLoaded', function()), I originally had the for loop below set to i<200.
Since updatePosition runs about 10x per second (as the user scrolls), that adds up to 2000 calculations per second
not including the repaint of the un-rendered pizzas. I found that i<24 will create enough pizzas to have the
same visualization as before while scrolling with only 240 calculations.

###Update the slider controlling the size of the pizzas

Changed sizeSwitcher function to give a % newWidth instead of returning a number and then having to convert it to a % in another function. Then selected all the randomPizza outside of the for loop and placed in a variable. This keeps them from having to be pulled over and over every time the previous function with the for loop ran. I then loop over the variable of all the random pizzas and change the width to the newWidth.