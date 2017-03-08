# University of Washington, CSE 154
## Homework Assignment 6: Book Review
This assignment is about making a web service and using Ajax to retrieve data from it. You will write a PHP service that will generate a variety of data about different books depending on the parameters it is sent. You will also write Javascript code to make requests to this service and inject the responses into a page. Turn in these files:

- bestreads.php, the PHP service that will supply the book data
- bestreads.js, the Javascript that will request the information from bestreads.php and inject it into bestreads.html

bestreads.html and bestreads.css are provided for you. You may not modify them. When the page loads it should display the images and titles of each book that you have data for and the singlebook div should be hidden. If the user clicks a book cover or title all of the books and titles should be removed from the page and the singlebook div should be shown. You should send a request to the server for data for this book and display its cover image, title, author, rating, description and reviews. The files will be located in relative paths such as `books/harrypotter/info.txt` and `books/wizardofoz/cover.jpg`. Your code should assume these are the relative paths to use.

## Web Service Details:
Your bestreads.php service will provide different data based upon a couple query parameters named mode and title that are passed from your Javascript to the page in its URL. The value of mode should be description, info, reviews or books depending on which information you want. The value of title should be a string representing the single book to display. The browser will request your page with a URL such as the following: https://webster.cs.washington.edu/your_uwnetid/hw6/bestreads.php?mode=description&title=harrypotter Your PHP code can store these parameters into variables using code such as the following:
```php
$book = $_GET["title"];
```
All of your PHP code should use these parameters’ values, and you should never hard-code particular book names. Your code may assume that the browser has properly supplied these parameters and has given them valid values. You may assume that the book exists and has a corresponding folder of valid input data and images. You do not have to handle the case of a missing or empty mode or title parameter, a value that contains bad characters, a value of a book that is not found, etc. Based upon the mode variable's value, your service must output that data. All book data is in a books directory. Each book is stored in a directory named the same as your query parameter in that books directory. For example, the book harrypotter stores its files in a folder named `books/harrypotter/` . You may assume that the files exist and are valid at all times. The behavior of each mode is described below:

- `mode=info`: The title parameter must also be passed with this mode. Your service should output the contents of `info.txt`, a file with three lines of information about the book: its title, author, and number of stars, as JSON. For example:
```js
{"title":"Harry Potter and the Prisoner of Azkaban", "author":"by J.K. Rowling, Mary GrandPre(Illustrator)", "stars":"4.5"}
```
- `mode=description`: The title parameter must also be passed with this mode. Your service should output the entire contents of `description.txt`, a file with information about the book, as plain text.
- `mode=reviews`: The title parameter must also be passed with this mode. Output HTML for a partial page containing all of the reviews for the book. Output the reviewer name and number of stars in an h3 and the review text in a paragraph. The reviews are stored in files called `review1.txt`, `review2.txt`, etc. Each file contains one review for each book which is exactly three lines: The reviewer's name, the number of stars they gave the book and their review. If a book has 10 or more reviews, the names will be e.g. `review01.txt`, .... So don't hard-code file names like "`review1.txt`"; look for all files that begin with "review" and end with ".txt". Example output:
```html
<h3>Wil Wheaton<span>4</span></h3>
<p>I'm beginning to wonder if there will ever be a Defense Against The Dark
Arts teacher who is just a teacher.</p>
<h3>Zoe<span>5</span></h3>
<p>Yup yup yup I love this book</p>
<h3>Kiki<span>5</span></h3>
<p>Literally one of the best books I've ever read. I was chained to it for two
days. </p>
```
- `mode=books`: Outputs XML containing the titles and folder names for each of the books that you have data for. The overall XML tag should be called books. Inside it each book’s data should be in a book tag. Each book tag should contain a title tag with the book’s title inside it and a folder tag with the book’s folder name inside it. If a new folder of book data is added to your books folder your program should automatically adjust to include it the next time your service is called. Example output:
```xml
<books>
<book>
<title>Harry Potter and the Prisoner of Azkaban</title>
<folder>harrypotter</folder>
</book>
...
</books>
```

## Javascript Details:
Your bestreads.js will use Ajax to request data from your PHP service and insert it into `bestreads.html`. Here is the functionality your page should have:

- When the page loads it should request all of the books (`mode=books`) from the web service. It should display each of these books by adding the image of the book’s cover and the book’s title (in a paragraph) to a `div` and adding that `div` to the `allbooks` `div` already on the page. The `singlebook` `div` should be hidden. 
- If the home button on the upper right is clicked it should do the same thing that the page does when it loads. Even if the button is pressed multiple times very quickly, only one listing for each book should appear on the page. 
- When a user clicks on a book cover or title of a book the `allbooks` `div` should be emptied out and the `singlebook` `div` should be shown. You should then request the info, description and reviews for that book from the server. The description can be directly inserted into the paragraph with the id `description` and the reviews HTML can be directly inserted into the `div` with the id `reviews`. The title, author and stars gotten from the info request should be inserted into the elements with ids matching their names.

Development Strategy and Hints: PHP code is difficult to debug if you have errors. Write the program incrementally, adding small pieces of code to a working page, and not advancing until you have tested the new code. The following functions may be helpful:

- `count` - returns the number of elements in an array
- `explode` - breaks apart a string into an array of smaller strings based on a delimiter
- `file` - reads the lines of a file and returns them as an array
- `glob` - given a file path or wildcard such as `"foo/bar/*.jpg"`, returns an array of matching file names
- `list` - unpacks an array into a set of variables; useful for dealing with fixed-size arrays of data
- `trim` - removes whitespace at the start/end of a string (gets rid of stray spaces in output)

## Implementation and Grading:
For full credit, your JS and PHP code should follow the rules listed in the Style Guide on the class web site. Your page's output must successfully pass the W3C HTML validator. (Not the PHP service but your `bestreads.html` after you have inserted data you have gotten back from the service in it. To validate your page, view the page, then choose View Source in your browser and copy/paste it into the validator.) Your JavaScript code should pass our JSLint tool with no errors. Your `.js` file must run in JavaScript strict mode by putting `"use strict";` at the top. Your PHP code should not cause errors or warnings. We will also grade the style of your PHP and JS code; like a CSE 14x assignment. Do not use the global keyword. Use indentation/spacing, and avoid long lines over 100 characters. Avoid redundant code, and use parameters and return values properly. Make extra effort to minimize redundant code. Capture common operations as functions to keep code size and complexity from growing. For full credit your code must have at least one function in each language to remove redundancy and/or capture structure. Make sure to test your code on all available books from the ZIP file. You may want to think about other edge cases, such as what your page should do if the book has only a single review, etc. You should not have code that depends on particular books or uses if/else statements to see which book to display. Use the "module pattern" shown in class to wrap your JS code in an anonymous function. No global variables or symbols, nor "module-global" vars, are allowed in JS on this assignment; values should be declared at the most local scope possible. If a constant value is used, you may declare it as a module-global "constant" variable IN_UPPER_CASE. Fetch data using Ajax. Be careful to avoid redundancy in your Ajax code; if you do similar Ajax operations in many places, make a helper function(s). Process XML data using the XML DOM. Process JSON data using `JSON.parse`. Another grading focus is PHP and JS commenting. We expect comments here similar to CSE 14x. Put a descriptive header (name, course, TA, description) at the top of your code and comment each non-trivial section of PHP and JS code explaining the purpose of that code. Format your JS and PHP code similarly to the examples from class. Properly use whitespace and indentation. Part of your grade will also come from successfully uploading your files to the Webster web server in a subdirectory named hw6, so that it is possible to navigate to your page by entering the following URL into the browser: https://webster.cs.washington.edu/students/your_uwnetid/hw6/bestreads.html

Please do not place a solution to this assignment online on a publicly accessible web site. Copyright © Allison Obourn, licensed under Creative Commons Attribution 2.5 License. All rights reserved.

