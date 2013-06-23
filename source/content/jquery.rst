title: jQuery
---

jQuery Basics
-------------

For all the same reasons that it’s desirable to segregate style from structure
within an HTML document, it’s as beneficial (if not more so) to separate the 
behavior from the structure. -- Page 4 jQuery in Action

Movement for seperation of behavior from structure called Unobtrusive JavaScript

.. sourcecode:: javascript

   //Add Text to an element (hide text in jQuery)
   $("#someElement").html("I have added some text to an element"); 

   //Selects all even elements
	$("p:even");

   //Select first row of each table
   $("tr:nth-child(1)");

   //Select div children of body
   $("body > div"); 

   //Select links of PDF files
   $("a[href$=pdf]");

   //Select direct children of body that contain links
   $("body > div:has(a)");

Full List of jQuery selectors
	http://docs.jquery.com/Selectors
