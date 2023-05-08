Download Link: https://assignmentchef.com/product/solved-web322-assignment-4
<br>
Build upon the code created in Assignment 3 by incorporating the Handlebars view engine to render our JSON data visually in the browser using .hbs views and layouts.  Additionally, update our data-service module to allow for people to be updated using a web form.




NOTE: If you are unable to start this assignment because Assignment 3 was incomplete – email your professor for a clean version of the Assignment 3 files to start from (effectively removing any custom CSS or text added to your solution).

Specification:

As mentioned above, this assignment will build upon your code from Assignment 3.  To begin, make a copy of your assignment 3 folder and open it in Visual Studio Code.  Note:  this will copy your .git folder as well (including the “heroku” remote for assignment 3).  If you wish to start fresh with a new git repository, you will need to delete the copied .git folder and execute “git init” again in.




<h1>Part 1: Getting Express Handlebars &amp; Updating your views</h1>




<h2><u>Step 1:</u> Install &amp; configure express-handlebars</h2>

<ul>

 <li>Use npm to install the “express-handlebars” module</li>

 <li>Wire up your server.js file to use the new “express-handlebars” module, ie:

  <ul>

   <li>“require” it as exphbs</li>

   <li>add the app.engine() code using exphbs({ … }) and the “extname” property as “.hbs” and the “defaultLayout” property as “main” (See the Week 6 Notes) o call app.set() to specify the ‘view engine’ (See the Week 6 Notes)</li>

  </ul></li>

 <li>Inside the “views” folder, create a “layouts” folder</li>

</ul>

<h2><u>Step 2:</u> Create the “default layout” &amp; refactor home.html to use .hbs</h2>




<ul>

 <li>In the “layouts” directory, create a “main.hbs” file (this is our “default layout”)</li>

 <li>Copy all the content of the “home.html” file and paste it into “main.hbs”</li>

</ul>

o Quick Note: if your site.css link looks like this href=”css/site.css”, it must be <u>modified</u> to use a leading “/”, ie href=”/css/site.css”

<ul>

 <li>Next, in your main.hbs file, remove all content <u>INSIDE</u> (not including) the single &lt;div class=”container”&gt;…&lt;/div&gt; element and replace it with {{{body}}}</li>

 <li>Once this is done, rename home.html to home.hbs</li>

 <li>Inside home.hbs, remove all content <u>EXCEPT</u> what is INSIDE the single &lt;div class=”container”&gt;…&lt;/div&gt; element (this should leave a single &lt;div class=”row”&gt;…&lt;/div&gt; element containing two “columns”, ie elements with class “col-md- …” and their contents)</li>

 <li>In your server.js file, change the GET route for “/” to “render” the “home” view, instead of sending home.html</li>

 <li>Test your server – you shouldn’t see any changes. This means that your default layout (“main.hbs”), “home.hbs” and server.js files are working correctly with the express-handlebars module.</li>

</ul>

<h2><u>Step 3:</u> Update the remaining “about”, “addPeople” and “addPicture” files to use .hbs</h2>




<ul>

 <li>Follow the same procedure that was used for “home.html”, for each of the above 3 files, ie:

  <ul>

   <li>Rename the .html file to .hbs o Delete all content <u>EXCEPT</u> what is INSIDE the single &lt;div class=”container”&gt;…&lt;/div&gt; element</li>

   <li>Modify the corresponding GET route (ie: “/about”, “/pictures/add” or “/people/add”) to “res.render” the appropriate .hbs file, instead of using res.sendFile</li>

  </ul></li>

 <li>Test your server – you shouldn’t see any changes, except for the fact that your menu items are no longer highlighted when we change routes (only “Home” remains highlighted, since it is the only menu item within our main.hbs “default layout” with the class “active”</li>

</ul>

<h2><u>Step 4:</u> Fixing the Navigation Bar to Show the correct “active” item</h2>




<ul>

 <li>To fix the issue we created by placing our navigation bar in our “default” layout, we need to make some small updates, including adding the following middleware function above your routes in server.js:</li>

</ul>




app.use(function(req,res,next){     let route = req.baseUrl + req.path;     app.locals.activeRoute = (route == “/”) ? “/” : route.replace(//$/, “”);     next();

});




This will add the property “activeRoute” to “app.locals” whenever the route changes, ie: if our route is “/people/add”, the app.locals.activeRoute value will be “/people/add”.

<ul>

 <li>Next, we must use the following handlebars custom “helper” (See the Week 6 notes for adding custom “helpers”)”</li>

</ul>




navLink: function(url, options){     return ‘&lt;li’


((url == app.locals.activeRoute) ? ‘ class=”active” ‘ : ”)


‘&gt;&lt;a href=”‘ + url + ‘”&gt;’ + options.fn(this) + ‘&lt;/a&gt;&lt;/li&gt;’;

}

<ul>

 <li>This basically allows us to replace all of our existing navbar links, ie: &lt;li&gt;&lt;a href=”/about”&gt;About&lt;/a&gt;&lt;/li&gt; with code that looks like this {{#navLink “/about”}}About{{/navLink}}. The benefit here is that the helper will automatically render the correct &lt;li&gt; element add the class “active” if app.locals.activeRoute matches the provided url, ie “/about”</li>

 <li>Now that our helpers are in place, update all the navbar links in main.hbs to use the new helper, for example:</li>

</ul>

o &lt;li&gt;&lt;a href=”/about”&gt;About&lt;/a&gt;&lt;/li&gt; will become {{#navLink “/about”}}About{{/navLink}}

<ul>

 <li>Test the server again – you should see that the correct menu items are highlighted as you navigate between views</li>

</ul>

<h1>Part 2: Rendering the Pictures in the “/pictures” route</h1>




Next, we’ll work with pictures.   It’ll be easier if 1 or more pictures have been added via the application, so do this now.




<h2><u>Step 1:</u> Add / configure “pictures.hbs” view and server.js</h2>

<ul>

 <li>First, add a file “pictures.hbs in the “views” directory</li>

 <li>Inside your newly created pictures.hbs file, add the following code to render 1 (one) of your (already-existing) uploaded pictures, ie (picture “1518186273491.jpg” – your picture will have a different timestamp):</li>

</ul>

&lt;div class=”row”&gt;

&lt;div class=”col-md-12″&gt;

&lt;h2&gt;Pictures&lt;/h2&gt;

&lt;hr /&gt;

&lt;/div&gt;




&lt;div class=”col-md-4″&gt;

&lt;img src=”/pictures/uploaded/1518186273491.jpg” class=”img-responsive img-thumbnail” /&gt;     &lt;/div&gt;

&lt;/div&gt;

Note the classes “img-responsive” and “img-thumbnail”.  These are simply bootstrap classes that correctly scale and decorate the picture with a border. See <u>https://getbootstrap.com/docs/3.3/css/#images</u> / <u>https://getbootstrap.com/docs/3.3/css/#images-shapes</u> for more information

<ul>

 <li>Next, modify your GET route for /pictures. Instead of executing res.json and sending the “pictures” array, you should use res.render(“pictures”, Object Containing “pictures” Array Here); so that you can send the object containing the array of pictures as data for your “pictures” view</li>

 <li>Once this is complete, modify your pictures.hbs file using the handlebars #each helper to iterate over the</li>

</ul>

“pictures” array, such that every picture is shown in its own &lt;div class=”col-md-4″&gt;…&lt;/div&gt; element

(effectively replacing our single “static” picture).  This will have the effect of giving us a nice, responsive grid of multiple “col-md-4” columns, each containing its own picture.

o NOTE: you can use {{this}} within the loop to get the current value of the item in the array of strings

(this will be the filename of the current picture, ie: “1518186273491.jpg”)

<ul>

 <li>If there are no pictures (ie the “pictures” array is empty), show the following element instead:</li>

</ul>

&lt;div class=”col-md-12 text-center”&gt;

&lt;strong&gt;No Pictures Available&lt;/strong&gt; &lt;/div&gt;

<ul>

 <li>NOTE: since we are hosting our app on heroku, you will notice that once the app “sleeps” and starts up again, any uploaded pictures are gone. This is expected behavior.  However, if you wish to develop an application that will persist its images between restarts of the app, you can look at something like <u>Cloudinary</u> (this service provides a mechanism to upload pictures to Cloudinary from your server.js code and store them online using their service)</li>

</ul>

<h1>Part 3: Updating the People Route &amp; Adding a View</h1>




Rather than simply outputting a list of people using res.json, it would be much better to actually render the data in a table that allows us to access individual people and filter the list using our existing req.params code.




<h2><u>Step 1:</u> Creating a simple “People” list &amp; updating server.js</h2>




<ul>

 <li>First, add a file “people.hbs” ” in the “views” directory</li>

 <li>Inside the newly created “people.hbs” view, add the html:</li>

</ul>

&lt;div class=”row”&gt;

&lt;div class=”col-md-12″&gt;

&lt;h2&gt;People&lt;/h2&gt;

&lt;hr /&gt;




&lt;p&gt;TODO: render a list of all People first and last names here&lt;/p&gt;




&lt;/div&gt;

&lt;/div&gt;







<ul>

 <li>Replace the &lt;p&gt; element (containing the TODO message) with code to iterate over each person and simply render their first and last names (you may assume that there will be an “people” array (see below).</li>

 <li>Once this is done, update your GET “/people” route according to the following specification

  <ul>

   <li>Every time you would have used res.json(data), modify it to instead use res.render(“people”, {people:</li>

  </ul></li>

</ul>

data})

<ul>

 <li>Every time you would have used res.json({message: “no results”}) – ie: when the promise has an error (ie in .catch()), modify instead to use res.render({message: “no results”});</li>

</ul>

<ul>

 <li>Test the Server – you should see the following page for the “/people” route:</li>

</ul>










<h2><u>Step 2:</u> Building the Table &amp; Displaying the error “message”</h2>

<ul>

 <li>Update the people.hbs file to render all of the data in a table, using the bootstrap classes: “table-responsive”</li>

</ul>

(for the &lt;div&gt; containing the table) and “table” (for the table itself) – Refer to the  completed sample here <u>https://gentle-earth-90224.herokuapp.com/people</u>

<ul>

 <li>The table must consist of 5 columns with the headings: Person ID, Full Name, Phone Number, Address, and Car Vin</li>

 <li>Additionally, the Name in the Full Name column must link to /person/id where id is the person’s id for that row o The “Phone” column must be a “tel” link to the user’s phone number for that row</li>

 <li>The “Car Vin” link must link to /cars?vin=vin where vin is the vin number for the person for that row</li>

</ul>

<ul>

 <li>Beneath &lt;div class=”col-md-12″&gt;…&lt;/div&gt; element, add the following code that will conditionally display the “message” only if there are no people (HINT: #unless people)</li>

</ul>




&lt;div class=”col-md-12 text-center”&gt;

&lt;strong&gt;{{message}}&lt;/strong&gt; &lt;/div&gt;

This will allow us to correctly show the error message from the .catch() in our route




<h2><u>Step 3:</u> Adding new query route for /stores</h2>

Update server.js to add a new query route for stores

<ul>

 <li>/stores?retailer=value

  <ul>

   <li>return a JSON string consisting of all stores where value could is retailer attribute of the stores object – this can be accomplished by calling the getStoresByRetailer(value) function of your data-service (defined below)</li>

  </ul></li>

 <li>Add getStoresByRetailer(data) Function

  <ul>

   <li>In data-service.js this function will provide an array of “stores” objects whose retailer property matches the data parameter using the resolve method of the returned promise.</li>

   <li>If for some reason, the length of the array is 0 (no results returned), this function must invoke the reject method and pass a meaningful message, ie: “no results returned”.</li>

  </ul></li>

</ul>










<h1>Part 4: Updating the Stores Route &amp; Adding a View</h1>

Now that we have the “People” data rendering correctly in the browser, we can use the same pattern to render the “Stores” data in a table:




<h2><u>Step 1:</u> Creating a simple “Stores” list &amp; updating server.js</h2>




<ul>

 <li>First, add a file “stores.hbs” in the “views” directory</li>

 <li>Inside the newly created ” stores.hbs” view, add the html:</li>

</ul>

&lt;div class=”row”&gt;

&lt;div class=”col-md-12″&gt;

&lt;h2&gt;Stores&lt;/h2&gt;

&lt;hr /&gt;




&lt;p&gt;TODO: render a list of all stores id’s and names here&lt;/p&gt;




&lt;/div&gt; &lt;/div&gt;

<ul>

 <li>Replace the &lt;p&gt; element (containing the TODO message) with code to iterate over each store and simply render their id, name, phone and address values (you may assume that there will be a “stores” array (see below).</li>

 <li>Once this is done, update your GET “/stores” route according to the following specification</li>

</ul>

o Instead of using res.json(data), modify it to instead use res.render(“stores”, {stores: data});

<ul>

 <li>Test the Server – you should see the following page for the “/stores” route:</li>

</ul>










<h2><u>Step 2:</u> Building the Table</h2>

<ul>

 <li>Update the stores.hbs file to render all of the data in a table, using the bootstrap classes: “table-responsive” (for the &lt;div&gt; containing the table) and “table” (for the table itself).</li>

 <li>The table must consist of 4 columns with the headings: Store id, Store Name, Store Phone, and Address</li>

 <li>The “Store Name” link must link to /cars?make=make where make is the make of the retailer for that row</li>

 <li>The “Phone” column must be a “tel” link to the user’s phone number for that row</li>

 <li>Refer to the example online at <u>https://gentle-earth-90224.herokuapp.com/stores</u></li>

</ul>




<h1>Part 5: Updating the Cars Route &amp; Adding a View</h1>

Now that we have the “People” and “Stores” data rendering correctly in the browser, we can use the same pattern to render the “Cars” data in a table:




<h2><u>Step 1:</u> Creating a simple “Cars” list &amp; updating server.js</h2>




<ul>

 <li>First, add a file “cars.hbs” in the “views” directory</li>

 <li>Inside the newly created ” cars.hbs” view, add the html:</li>

</ul>

&lt;div class=”row”&gt;

&lt;div class=”col-md-12″&gt;

&lt;h2&gt;Cars&lt;/h2&gt;

&lt;hr /&gt;




&lt;p&gt;TODO: render a list of all cars id’s and names here&lt;/p&gt;




&lt;/div&gt; &lt;/div&gt;

<ul>

 <li>Replace the &lt;p&gt; element (containing the TODO message) with code to iterate over each car and simply render their id, make, model, year and vin values (you may assume that there will be a “cars” array (see below).</li>

 <li>Once this is done, update your GET “/cars” route according to the following specification</li>

</ul>

o Instead of using res.json(data), modify it to instead use res.render(“cars”, {cars: data});

<ul>

 <li>Test the Server – you should see the following page for the “/cars” route:</li>

</ul>










<h2><u>Step 2:</u> Building the Table</h2>

<ul>

 <li>Update the cars.hbs file to render all of the data in a table, using the bootstrap classes: “table-responsive” (for the &lt;div&gt; containing the table) and “table” (for the table itself).</li>

 <li>The table must consist of 5 columns with the headings: Car id, Car Make, Car Model, Car Year and Vin</li>

 <li>The “Car make” link must link to /stores?retailer=retailer where retailer is the retailer of the make for that row</li>

 <li>The “Car Year” ” link must link to /cars?year=year where year is the year of the car for that row</li>

 <li>The “Car Vin” link must link to /people?vin=vin where vin is the vin of the car for that row</li>

 <li>Refer to the example online at <u>https://gentle-earth-90224.herokuapp.com/cars</u></li>

</ul>




<h1>Part 6: Updating Existing People</h1>

The last piece of the assignment is to create a view for a single person.  Currently, when you click on an person’s name in the “/people” route, you will be redirected to a page that shows all of the information for that person as a JSON-formatted string (ie: accessing http://localhost:8080/person/21, should display a JSON formatted string representing the corresponding person – person 21).

Now that we are familiar with the express-handlebars module, we should add a view to render this data in a form and allow the user to save changes.




<h2><u>Step 1:</u> Creating new .hbs file / route to Update Person</h2>




<ul>

 <li>First, add a file “person.hbs” in the “views” directory</li>

 <li>Inside the newly created “person.hbs” view, add the html (NOTE: Some of the following html code may wrap across lines to fit on the .pdf – be sure to check that the formatting is correct after pasting the code):</li>

</ul>

&lt;div class=”row”&gt;

&lt;div class=”col-md-12″&gt;

&lt;h2&gt;{{person.first_name}} {{ person.last_name}} – Person: {{ person.id}}&lt;/h2&gt;

&lt;hr /&gt;

&lt;form method=”post” action=”/person/update”&gt;

&lt;fieldset&gt;

&lt;legend&gt;Personal Information&lt;/legend&gt;

&lt;div class=”row”&gt;

&lt;div class=”col-md-6″&gt;

&lt;div class=”form-group”&gt;

&lt;label for=”first_name”&gt;First Name:&lt;/label&gt;

&lt;input class=”form-control” id=”first_name” name=”first_name” type=”text” value=”{{person.first_name}}” /&gt;

&lt;/div&gt;

&lt;/div&gt;

&lt;div class=”col-md-6″&gt;

&lt;div class=”form-group”&gt;

&lt;label for=”last_name”&gt;Last Name:&lt;/label&gt;

&lt;input class=”form-control” id=”last_name” name=”last_name” type=”text” value=”{{person.last_name}}” /&gt;

&lt;/div&gt;

&lt;/div&gt;                 &lt;/div&gt;

&lt;/fieldset&gt;

&lt;hr /&gt;

&lt;input type=”submit” class=”btn btn-primary pull-right” value=”Update Person” /&gt;&lt;br /&gt;&lt;br /&gt;&lt;br /&gt;         &lt;/form&gt;

&lt;/div&gt;

&lt;/div&gt;




<ul>

 <li>Once this is done, update your GET “/person/:id” route according to the following specification</li>

</ul>

o Use res.render(“person”, { person: data }); inside the .then() callback (instead of res.json) and use res.render(“person”,{message:”no results”}); inside the .catch() callback

<ul>

 <li>Test the server (/person/1) – this will get you started on creating / populating the form with user data:</li>

</ul>







<ul>

 <li>Continue this pattern to develop the full form to match the <u>completed sample here</u> – you may use the code in the sample to help guide your solution</li>

</ul>

o Id: type: “hidden”, name: “id” o Phone: type: “text”, name: “phone” o Address (Street): type: “text”, name: “address” o Address (City): type: “text”, name: “city” o Person’s Vin Number: type: “text”, name: “vin”

<ul>

 <li>No validation (client or server-side) is required on any of the form elements at this time</li>

 <li>Once the form is complete, we must add the POST route: /person/update in our server.js file:</li>

</ul>




app.post(“/person/update”, (req, res) =&gt; {     console.log(req.body);

res.redirect(“/people”);

});

This will show you all the data from your form in the console, once the user clicks “Update Person”. However, in order to take that data and update our “people” array in memory, we must add some new functionality to the data-service.js module:

<h2><u>Step 2:</u> Updating the data-service.js module</h2>




<ul>

 <li>Add the new method: updatePerson(personData) that returns a promise. This method will:</li>

</ul>

o Search through the “people” array for a person with an id that matches the JavaScript object (parameter personData). o When the matching person is found, overwrite it with the new person passed into the function (parameter personData) o Once this has completed successfully, invoke the resolve() method without any data.

<ul>

 <li>Now that we have a new updatePerson() method, we can invoke this function from our newly created app.post(“/person/update”, (req, res) =&gt; { … });   Simply invoke the updatePerson() method with the req.body as the parameter.  Once the promise is resolved use the then() callback to execute the res.redirect(“/people”); code.</li>

 <li>Test your server in the browser by updating Person 21 (Regina Dunphy). Once you have clicked “Update Person” and are redirected back to the People list, Person 21 should show your changes!</li>

</ul>




<h1>Part 7: Pushing to Heroku</h1>

Once you are satisfied with your application, deploy it to Heroku:

<ul>

 <li>Ensure that you have checked in your latest code using git (from within Visual Studio Code)</li>

 <li>Open the integrated terminal in Visual Studio Code</li>

 <li>Log in to your Heroku account using the command heroku login</li>

 <li>Create a new app on Heroku using the command heroku create</li>

 <li>Push your code to Heroku using the command git push heroku master</li>

 <li>IMPORTANT NOTE: Since we are using an “unverified” free account on Heroku, we are limited to only 5 apps, so if you have been experimenting on Heroku and have created 5 apps already, you must delete one (or verify your account with a credit card). Once you have received a grade for Assignment 1, it is safe to delete this app (login to the Heroku website, click on your app and then click the Delete app… button under “Settings”).</li>

</ul>




<h2><u>Testing</u>: Sample Solution</h2>




To see a completed version of this app running, visit: <u>https://gentle-earth-90224.herokuapp.com/</u>




Please note: This solution is visible to ALL students and professors at Seneca College.  It is your responsibility as a student of the college not to post inappropriate content / images to the shared solution.  It is meant purely as an exemplar and any misuse will not be tolerated.





