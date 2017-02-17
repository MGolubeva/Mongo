<style>
span.key{
   color:red;
}
span.text{
   color:blue;
}
span.num{
   color:green;
}
span.bool{
   color:purple;
}
tr td{
 text-align: left;
 color: black;
}
.not_order{
  list-style:none;
}
ul li pre code{
  background: none;
}
</style>

## About
&emsp;&emsp;This lesson opens a new page in the study of MongoDB. Here we are going to show you the primary operations commonly called CRUD (**c**reate, **r**ead, **u**pdate and **d**elete). We will answer next questions during this lesson:
<ul>
	<li>How to create new databases, collections and documents?</li>
	<li>How to insert, update and delete documents and its data?</li>
	<li>How to investigate MongoDB data types and its particularities</li>
</ul>
&emsp;&emsp;Finally, you will learn the basic search and filter operators for data and documents.<br/>
This lesson consists of more simplified operations with data and a lot of examples. More advanced usage and commonly used best practices we are going to demonstrate in the next lesson. Also starting this lesson, students will be offered exercises to consolidate the material. You will find the list of tasks at the end of this lesson.<br/>
&emsp;&emsp;In the current lesson, we will work with the dataset containing data about 12 movies (its title, genres, duration, IMDB rating, director, and leading actors, etc.) collected from http://www.imdb.com. IMDB is a well-known online database of information related to films, television programs and video games, including cast, production crew, fictional characters, biographies, plot summaries, trivia and reviews in JSON-like format. The file with data may be downloaded from here <a href="https://study.moderndeveloper.com/wp-content/uploads/2016/09/lesson2_dataset.js" target="_blank">dataset file</a>. Below we provide one record from this file, i.e. one JavaScript object that corresponds to the movie “The Shawshank Redemption”. It consists of many fields of various data types - string, integer, decimal, array, boolean, etc.<br/>
<pre><code>
{ 
      <span class="key">title</span>: <span class="text">“The Shawshank Redemption”</span>,
      <span class="key">genres</span>: [<span class="text">“crime”</span>, <span class="text">“drama”</span>],
      <span class="key">year</span>: <span class="num">1994</span>,
      <span class="key">duration</span>: <span class="num">142</span>,
      <span class="key">country</span>: <span class="text">“USA”</span>,
      <span class="key">rating</span>: <span class="num">9.3</span>,
      <span class="key">oscar_nominated</span>: <span class="bool">true</span>,
      <span class="key">oscar_awards</span>: <span class="num">7</span>,
      <span class="key">director</span>: {
              <span class="key">name</span>: <span class="text">“Frank Darabont”</span>,
              <span class="key">born</span>: <span class="text">“1959-01-28”</span>,
              <span class="key">country</span>: <span class="text">“France”</span>,
              <span class="key">gender</span>: <span class="text">"male"</span>
      },
      <span class="key">actors</span>: [
              {
                    <span class="key">name</span>: <span class="text">“Tim Robbins”</span>,
                    <span class="key">born</span>: <span class="text">“1958-10-16”</span>,
                    <span class="key">country</span>: <span class="text">“USA”</span>,
                    <span class="key">gender</span>: <span class="text">"male"</span>
              },
              {
                    <span class="key">name</span>: <span class="text">“Morgan Freeman”</span>,
                    <span class="key">born</span>: <span class="text">“1937-06-01”</span>,
                    <span class="key">country</span>: <span class="text">“USA”</span>,
                    <span class="key">gender</span>: <span class="text">"male"</span>
              },
              {
                    <span class="key">name</span>: <span class="text">“Bob Gunton”</span>,
                    <span class="key">born</span>: <span class="text">“1945-11-15”</span>,
                    <span class="key">country</span>: <span class="text">“USA”</span>,
                    <span class="key">gender</span>: <span class="text">"male"</span>
              }
      ]
}
</code></pre>
&emsp;&emsp;We have constructed this dataset so that it contains many data types supported by MongoDB. Moreover, if you look attentively at the records of the just downloaded file, you may see that some movies possess by various fields (for example, `next_parts`, `previous_parts`, etc.) or have the same fields (with the same name) but different types (for instance, `genres` and `country`). It was emphasized in the previous lesson that documents of one and the same MongoDB’s collection may have different fields and also different amount of fields. And here we are going to show how we should work with this type of documents.<br/>
&emsp;&emsp;First of all, let’s run the `mongod` process like we did it before: just open Terminal or Command Prompt and type **`mongod`** there. Remember that `mongod` (“Mongo Daemon”) is basically the host process for the database. When you start `mongod` you are saying “start the MongoDB process and run it in the background”. It has several default parameters, such as storing data in `/data/db` (we defined this directory at the MongoDB installation, but it may be changed) and running on port 27017.<br/>
&emsp;&emsp;Then we need connect to MongoDB server. It would be more convenient for us to use MongoDB API for Node.js than mongo shell. So, open another Terminal or Command Prompt window and run “test.js” JavaScript file created in the previous lesson:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **`node test.js`** <br/>
&emsp;&emsp;If you see the message  “Connected correctly to server.”, then everything works fine and we can continue. Otherwise, please return to the previous lesson and read more detailed instruction.<br/>

## How to create a new MongoDB database?
&emsp;&emsp;In order to create a brand new database you need to set its name in the URL for connection to MongoDB server like it was done before for connection to “test” database. If the database exists, MongoClient tries getting access to it. Otherwise, it creates a new one.<br/>
&emsp;&emsp;Let’s create a new JavaScript file “movies.js”, copy the content of  “test.js” to it and make the following changes<br/>
**`EX: 1`**<br/>
<pre lang="javascript" line="1">
 var MongoClient = require('mongodb').MongoClient;
 var assert = require('assert');

 var url = 'mongodb://localhost:27017/imdb';
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     console.log("Connected correctly to `imdb` MongoDB database");
     db.close();
 });
</pre>
&emsp;&emsp;Thus, when you run **`node movies.js`** in the Terminal, a new database “movies” will be created.<br/>
&emsp;&emsp;Here and further we will also provide examples of how the same results may be obtained with the help of mongo shell. Particularly, to create a new database in mongo shell we need to type **`use <database_name>`**. In the current case **`<database_name> = imdb`** (before open a new Terminal and run mongo shell there - **`mongo`**).<br/>
&emsp;&emsp;So now we should have at least two databases - “test” (we wrote this name in “test.js” JavaScript file) and “imdb”. Let’s check whether they already exist. Create a new file “show_dbs.js”, copy and paste the following code to it<br/>
**`EX: 2`**<br/>
<pre lang="javascript" line="1">
 var assert = require('assert'),
     mongodb = require('mongodb');
 var Db = mongodb.Db,
     Server = mongodb.Server;

 var db = new Db('imdb', new Server('localhost', 27017));
 db.open(function(err, db) {
     db.admin().listDatabases(function(err, dbs) {
         assert.equal(null, err);
         assert.ok(dbs.databases.length > 0);
         console.log(dbs);
         db.close();
     });
 });
</pre>
&emsp;&emsp;Here `Db` instance is used for creation or getting access to a database; Server defines the available port for connection. It is an alternative way of database creation or getting access to it. We use `admin()` method to list all existing databases, that command gives access to the Admin database (allows the user to access the admin functionality of MongoDB). `listDatabases` returns an object containing the list of all databases along with basic statistics about them.<br/>
&emsp;&emsp;To display list of existing databases in mongo shell use **`show dbs`** command.<br/>
After running the above code (with the help of **`node show_dbs.js`** command) you will see the following message<br/>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/1.jpg"/></center>
&emsp;&emsp;So what happened? Where are “test” and “imdb” databases? I’m sure, that I’ve created them.<br/>
&emsp;&emsp;It is not a mistake. MongoDB storages only not empty databases, i.e. those that have at least one collection with at least one document containing at least one field.<br/>
&emsp;&emsp;Note about assert: We have already mentioned about "assert" module in the previous lesson, but we want to draw your attention to it once more, since we will be using it extensively.<br/>
&emsp;&emsp;Two main methods of this module that we will use today are assert.ok() and assert.equal(). The former is to check whether the argument passed to it is truthy, if it is execution goes on, if not the AssertionError is raised. The latter checks for equality of two arguments passed to it and applies the same rule: if arguments are indeed equal execution goes on, otherwise AssertionErrer is thrown.<br/>
<div class="exercises-div">
<h4>Exercise 1</h4>
<p>Create a new database “my_db” with the help of mongo shell.</p>
</div>
## How to create new MongoDB collections and documents?
&emsp;&emsp;Now let’s create a new collection “movies” in “imdb” database and insert one document with only one field `title`. To do it just paste the following changes in the “movies.js” file<br/>
**`EX: 3`**<br/>
<pre lang="javascript" line="1">
 var MongoClient = require('mongodb').MongoClient;
 var assert = require('assert');

 var url = 'mongodb://localhost:27017/imdb';
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     console.log("Connected correctly to `imdb` MongoDB database");
     var collection = db.collection("movies");
     console.log("Collection `movies` was created");
     collection.insert({title: "The Shawshank Redemption"});
     console.log("The first document was created");
     db.close();
 });
</pre>
&emsp;&emsp;The `collection(name)` method is used to create a new collection `name` or to get access to an existing collection `name`. After creation of an empty collection we have added a new document with `{title: "The Shawshank Redemption"}` record (only with movie title for now) to it using method `insert`.<br/>
&emsp;&emsp;In the same way you may add documents to a collection in mongo shell:<br/>
<ul>
	<li>Select database <code>use imdb</code></li>
	<li>Create a new collection <code>db.createCollection("movies")</code></li>
 	<li>Call <code>insert()</code> method <code>db.movies.insert({title: "The Shawshank Redemption"})</code></li>
	<li>The used database gets name db automatically.</li>
</ul>
&emsp;&emsp;Let’s check now whether the “imdb” was added to the list of existing MongoDB databases. To do this run “show_dbs.js” again<br/>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/2.jpg"/></center>
<div class="exercises-div">
<h4>Exercise 2</h4>
<p>In the database “my_db” created in the previous exercise, create a new collection “mountains” that will contain data about mountain peaks and add the following two documents to it:</p>
<pre><code>
{ 
      <span class="key">"name"</span>: <span class="text">"Mount Everest"</span>,
      <span class="key">"height"</span>: <span class="num">8848</span>
}
</code></pre>
and
<pre><code>
{ 
      <span class="key">"name"</span>: <span class="text">"Mount Kilimanjaro"</span>,
      <span class="key">"height"</span>: <span class="num">5895</span>
}
</code></pre>
</div>
## Basic CRUD operations
### Read documents (find queries)
&emsp;&emsp;Great, we have created a new collection and insert a new document into it. But how to check that it was really inserted? Or more general, we may transfer data from some file or another database to a MongoDB collection or made some transformation with data. How can we see how it looks now? MongoDB API provides method `find` for this purpose. It returns query results in a cursor, which is an iterable object that yields documents.<br/>
&emsp;&emsp;The query condition for an equality match on a field has the following form:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **`{ <field1>: <value1>, <field2>: <value2>, ... }`** <br/>
&emsp;&emsp;More detailed query features will be considered further.<br/>
&emsp;&emsp;Let’s display all documents of “movies” collection. Note, here and further we will modify the “movies.js” file and will show basically only the changed lines<br/>
**`EX: 4`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.find().each(function(err, doc) {
         if (doc !== null) {
             console.dir(doc);
         }
     });
     db.close();
 });
</pre>
The output should look like this:<br/>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/3.jpg"/></center>
&emsp;&emsp;Note, if you have run the “movies.js” file with the code for creation a new document (the last code from “How to create new MongoDB collections and documents” section) few times, there were also created few new documents with the same title field, but with different _id fields. What is _id? We didn’t add such a field with such a complicated structure to the insert method.<br/>
### What is _id and ObjectID?
&emsp;&emsp;MongoDB uses _id for uniquely identity column in the document, and it behaves as a Primary Key.  MongoDB use ObjectID as the default value for _id. BSON ObjectID is generated by MongoDB drivers. If MongoDB Client does not add the _id field, then MongoDB automatically create a _id field for the document to identify column and avoid duplicates uniquely. So even if you want to build many documents with the same fields, MongoDB allows doing this, but in this case, it will contain many documents with all the same fields except for _id. You should be careful in this way.<br/>
&emsp;&emsp;An `ObjectID` is a 12-byte BSON type having the following structure:
<pre>
ObjectID = [
	4 bytes representing the seconds since the unix epoch,
	3 bytes are the machine hash identifier,
	2 bytes consists of process id,
	3 bytes are a random counter value
]
</pre>
&emsp;&emsp;In the last code, we have used `console.dir()` to show an expanded structure of `ObjectID`. In general, it is represented in hashed form with 24 characters. Replace<br/>
<pre lang="javascript" line="10">
             console.dir(doc);
</pre>
to<br/>
<pre lang="javascript" line="10">
             console.log(doc);
</pre>
to see the commonly used output<br/>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/4.jpg"/></center>
To display collection’s documents in mongo shell, write **`db.movies.find()`**.<br/>
We will display additional options of documents reading below.<br/>

### Insert documents
&emsp;&emsp;Besides `insert` method, you can use the `insertOne` (ensure insertion of only one document) method and the `insertMany` (allows insertion in bulk) method to add documents to a collection in MongoDB. If you attempt to add documents to a collection that does not exist, MongoDB will create the collection for you.<br/>
&emsp;&emsp;So, let’s add few new documents to the “movies” collection<br/>
**`EX: 5`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     // Insert a single document
    collection.insertOne({title: "Now You See Me", rating: 7.8}, function(err, result) {
        assert.equal(err, null);
        console.log("Inserted a document into the `movies` collection");
    });
    // Insert a few documents
    collection.insertMany([
            {title: "Lucky Number Slevin", rating: 7.8},
            {title: "Forrest Gump", rating: 8.8}
        ], function(err, result) {
            assert.equal(err, null);
            console.log("Inserted two documents into the `movies` collection");
    });
    collection.find().each(function(err, doc) {
        if (doc !== null) console.log(doc);
    });
    db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/5.jpg"/></center>
&emsp;&emsp;Pay attention that `insert`, `insertOne` and `insertMany` methods may have an optional callback function. In the previous case, they were used only for displaying messages about insertion one or many documents to the collection.<br/>

### Update documents        
&emsp;&emsp;To update the records we use `update`(criteria, updated_object) method. To update a document with updated_object, we should determine which document we are going to update with the help of native MongoDB filtering queries criteria. They represent an object with one or more defined fields. You can use the `updateOne` (updates only one document) and `updateMany` (changes a set of documents) methods to update documents of a collection.<br/>
&emsp;&emsp;Let’s take a look at an example below where we change the title of existing document and add a new field rating because new documents created in the previous step have already contained this new field:<br/>
**`EX: 6`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.update(
         {title: "The Shawshank Redemption"}, 
         {title: "The Shawshank Redemption (1994)", rating: 8.3}
     );
     collection.findOne({rating: 8.3}, function(err, doc) {
         if (doc !== null) console.log(doc);
     });
     db.close();
 });
</pre>
&emsp;&emsp;So, we have found all documents with the `title == "The Shawshank Redemption"` (sure, we have only one such document) and change it to `"The Shawshank Redemption (1994)"`. Also, we’ve added a new field rating with the value 8.3. Then, to display changes we have used method `findOne`, which works as well as `find`, but returns only one (the first if there are few available variants) also with specific filter `{rating: 8.3}`<br/>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/6.jpg"/></center>
&emsp;&emsp;The updating of a document in mongo shell is analogous<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **`db.movies.update({title: "The Shawshank Redemption"}, {title: "The Shawshank Redemption (1994)", rating: 8.3})`** <br/>
&emsp;&emsp;Oh, we set incorrect value of movies rating, it should be equal to 9.3. Let’s fix it<br/>
**`EX: 7`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.update({rating: 8.3}, {rating: 9.3});
     collection.findOne(function(err, doc) {
         if (doc !== null) console.log(doc);
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/7.jpg"/></center>
&emsp;&emsp;Wait, where did the field `title` disappear? I expected to change only rating.<br/>
&emsp;&emsp;It has happened because the `update` method replaces the old document by the new one. To change a field value, MongoDB provides update operators (the main operators are gathered in the table below), such as `$set` to modify values. Some update operators, will create the field if the field does not exist.<br/>
<table>
<thead>
<tr>
<th style="text-align: center"><b>Name</b></th>
<th style="text-align: center"><b>Description</b></th>
</tr>
</thead>
<tbody>
<tr bgcolor="#B3E5FC">
<td colspan="2"><strong>Fields operators</strong></td>
</tr>
<tr>
<td><code>$inc</code></td>
<td>Increments the value of the field by the specified amount</td>
</tr>
<tr>
<td><code>$mul</code></td>
<td>Multiplies the value of the field by the specified amount</td>
</tr>
<tr>
<td><code>$rename</code></td>
<td>Renames a field</td>
</tr>
<tr>
<td><code>$set</code></td>
<td>Sets the value of a field in a document</td>
</tr>
<tr>
<td><code>$unset</code></td>
<td>Removes the specified field from a document</td>
</tr>
<tr>
<td><code>$min</code></td>
<td>Only updates the field if the specified value is less than the existing field value</td>
</tr>
<tr>
<td><code>$max</code></td>
<td>Only updates the field if the specified value is greater than the existing field value</td>
</tr>
<tr>
<td><code>$currentDate</code></td>
<td>Sets the value of a field to current date</td>
</tr>

<tr bgcolor="#FFCC80">
<td colspan="2"><strong>Array operators</strong></td>
</tr>
<tr>
<td><code>$</code></td>
<td>Acts as a placeholder to update the first element that matches the query condition in an update</td>
</tr>
<tr>
<td><code>$addToSet</code></td>
<td>Adds elements to an array only if they do not already exist in the set</td>
</tr>
<tr>
<td><code>$pop</code></td>
<td>Removes the first or last item of an array</td>
</tr>
<tr>
<td><code>$pullAll</code></td>
<td>Removes all matching values from an array</td>
</tr>
<tr>
<td><code>$pull</code></td>
<td>Removes all array elements that match a specified query</td>
</tr>
<tr>
<td><code>$push</code></td>
<td>Adds an item to an array</td>
</tr>

<tr bgcolor="#A5D6A7">
<td colspan="2"><strong>Modifiers</strong></td>
</tr>
<tr>
<td><code>$each</code></td>
<td>Modifies the $push and $addToSet operators to append multiple items for array updates</td>
</tr>
<tr>
<td><code>$slice</code></td>
<td>Modifies the $push operator to limit the size of updated arrays</td>
</tr>
<tr>
<td><code>$sort</code></td>
<td>Modifies the $push operator to reorder documents stored in an array</td>
</tr>
<tr>
<td><code>$position</code></td>
<td>Modifies the $push operator to specify the position in the array to add elements</td>
</tr>
</tbody>
</table>
&emsp;&emsp;The update operators have the following form:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **`{ operator: { <field1>: <value1>, <field2>: <value2>, ... } }`** <br/>
&emsp;&emsp;and may be applied to many various fields at once.<br/>
&emsp;&emsp;Let’s return to the result obtained in the EX. 6 and show how we had to change `rating`’s value<br/>
**`EX: 8`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     // Return to EX. 6
     collection.update({rating: 9.3}, {title: "The Shawshank Redemption (1994)", rating: 8.3});
     collection.findOne(function(err, doc) {
         if (doc !== null) console.log(doc);
     });
     // Change `rating`'s value
     collection.update({title: "The Shawshank Redemption (1994)"}, {$set: {rating: 9.3} });
     collection.findOne(function(err, doc) {
         if (doc !== null) console.log(doc);
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/8.jpg"/></center>
&emsp;&emsp;The `$set` operator performs addition of the fields to an existing document without affecting the fields that are already in this document. It is behaviour not specific to this function, but also to many others<br/>
**`EX: 9`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     // Return to EX. 6
     collection.update(
         {title: "The Shawshank Redemption (1994)"}, 
         {$set: {
             title: "The Shawshank Redemption",
             year: 1994,
             genres: ["crime", "drama"],
             duration: 142,
             country: "USA",
             oscar_nominated: true,
             oscar_awards: 7,
             director: {
                 name: "Frank Darabont",
                 born: "1959-01-28",
                 country: "France",
                 gender: "male"
             }
         } 
     });
     collection.findOne(function(err, doc) {
         if (doc !== null) console.log(doc);
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/9.jpg"/></center>
&emsp;&emsp;Thus we have added a lot of new fields, and we are going to demonstrate the usage of updating operators on these fields. Let’s do the following:<br/>
<ul>
	<li>reduce rating by 20% with the help of the <code>$mul</code> operator;</li>
	<li>increase duration of 10 minutes using the <code>$inc</code> operator;</li>
	<li>reduce year by two years using the <code>$inc</code> operator;</li>
	<li>change <code>oscar_awards</code> to 5 if this values is less than the current <code>oscar_awards</code> value (which equals to 7) with the help of the <code>$min</code> operator;</li>
	<li>add two new fields <code>last_modified</code> and <code>timestamp</code> which will contain the date and time of the last changing of the document in Date and <a href="https://en.wikipedia.org/wiki/Timestamp" target="_blank">timestamp</a> formats respectively using the <code>$currentDate</code> operator;</li>
	<li>and finally add a new country “Canada” to the country field; we also need to change the type of this field using <code>$set</code> operator.</li>
</ul>
**`EX: 10`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.update(
         {title: "The Shawshank Redemption"}, 
         {   // reduce `rating` by 20%
             $mul: {rating: 0.8},
             // increase `duration` by 10 minutes and reduce `year` by 2 years
             $inc: {
                 duration: 10, 
                 year: -2
             },
             // change `oscar_awards` to 5 
             $min: {oscar_awards: 5},
             // add the current date
             $currentDate: {
                 last_modified: true,
                 timestamp: {$type: "timestamp"}
             },
             // expand countries list
             $set: {country: ["USA", "Canada"]},
         }
     );
     collection.findOne(function(err, doc) {
         if (doc !== null) console.log(doc);
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/10.jpg"/></center>
&emsp;&emsp;Note, in general, we can apply one operator to one field within one update query. Otherwise, you should divide one query into two or more queries. For example, if we had wanted to rename also the year field above, the update method would not work and changed nothing. You may check it.<br/>

### BSON specific data types
&emsp;&emsp;Also, you might detect that we have used the `$type` operator. It selects and returns the documents where the value of the field is an instance of the specified BSON type. Querying by data type is useful when dealing with highly unstructured data where data types are not predictable. BSON supports the following data types as values in documents<br/>
<ul>
	<li><strong>String</strong> - is the most commonly used datatype to store the data; it must be UTF-8 valid</li>
	<li><strong>Integer</strong> - is used to store a numerical value; can be 32 bit or 64 bit depending upon the server</li>
	<li><strong>Boolean</strong> - is used to store a boolean (true/false) values</li>
	<li><strong>Double</strong> - is used to store floating point values</li>
	<li><strong>Min/Max keys</strong> - is used to compare a value against the lowest and highest BSON elements</li>
	<li><strong>Arrays</strong> - is used to store arrays or list or multiple values into one key</li>
	<li><strong>Timestamp</strong> - can be handy for recording when a document has been modified or added</li>
	<li><strong>Object</strong> - is used for embedded documents</li>
	<li><strong>Null</strong> - is used to store a Null value</li>
	<li><strong>Symbol</strong> - is used identically to a string however, it’s generally reserved for languages that use a specific symbol type</li>
	<li><strong>Date</strong> - is used to store the current date or time in UNIX time format. You can specify your own date time by creating object of Date and passing day, month, year into it</li>
	<li><strong>ObjectID</strong> - is used to store the document’s ID</li>
	<li><strong>Binary data</strong> - is used to store binary data</li>
	<li><strong>Code</strong> - is used to store javascript code into document</li>
	<li><strong>Regular expression</strong> - is used to store regular expression</li>
</ul>
&emsp;&emsp;Each data type has a corresponding number and string alias that can be used with the `$type` operator to query documents by BSON type. In our lessons, we will meet all these datatypes as well.<br/>
&emsp;&emsp;In the next example, we are going to do the following:<br/>
<ul>
	<li>rename <code>year</code> field to <code>released</code> with the help of <code>$rename</code>;</li>

	<li>remove <code>last_modified</code> field using the <code>$unset</code> operator;</li>

	<li>add new elements to genres and country using both <code>$addToSet</code> and <code>$push</code> operators</li>

	<li>add new field <code>numbers</code> with the few integers that we need in the future.</li>

</ul>
**`EX: 11`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.update(
         {title: "The Shawshank Redemption"},
         {    // rename `year` field to `released` with the help of $rename;
              $rename: {year: "released"},
              // remove defined fields
              $unset: {
                  last_modified: "", 
                  unknown: ""
              },
              // add new elements to genres and country
              $addToSet: {genres: "drama"},
              $push: {country: "USA"},
              // add a new array with numbers
              $set: {numbers: [0, 1, 2, 4, -1, 5, 0, 2, 7]}
         }
     );
     collection.findOne(function(err, doc) {
         if (doc !== null) console.log(doc);
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/11.jpg"/></center>
&emsp;&emsp;As you can see, the `$unset` operator only misses some fields if they don’t exist in a document (in the example above it was `unknown`). Let’s emphasize that `$addToSet` check whether the adding value exists in an array and add it only if it is not included in the array opposite to `$push` operator that adds a new element in any case.<br/>
&emsp;&emsp;Below example demonstrates usage of `$pop`, `$pull` and `$pullAll` operators (all necessary comments are given in the code)<br/>
**`EX: 12`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.update(
         {title: "The Shawshank Redemption"}, 
         {   // remove the last element in `country`
           $pop: {country: 1}, // set -1 instead of 1 to remove the first element
            // remove from `genres` all instances of a value or values that match a specified condition,
             // i.e. "drama" in the current case
           $pull: {genres: "drama"},
           // remove only the elements in `numbers` that match the specified value exactly, including order
           $pullAll: {numbers: [0, 2, 7]}
        }
     );
     collection.findOne(function(err, doc) {
         if (doc !== null) {  // Display only changed fields
             console.log("country:", doc.country);
             console.log("genres:", doc.genres);
             console.log("numbers:", doc.numbers);
         }
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/12.jpg"/></center>
&emsp;&emsp;Let’s underline that in the `$pull` operator we may use more complex conditions with query selectors (see below).<br/>
&emsp;&emsp;Besides, in the EX 11, we added only one element to arrays. We cannot extend an existing array by the passing another array to the `$addToSet` or `$push` operators like<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **`$addToSet: {genres: ["drama", "comedy"]}`** <br/>
&emsp;&emsp;In case genres will be changed as follows:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **`genres: ["crime", "drama", ["drama", "comedy"]]`** <br/>
&emsp;&emsp;You may check it yourself.<br/>
&emsp;&emsp;To add two elements we may run `update` method twice and so on. But it is not the most convenient way. Modifiers help here, especially `$each`<br/>
**`EX: 13`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.update(
         {title: "The Shawshank Redemption"}, 
         {$push: {    // add an array of values to `genres` and sort them
             genres: {
                 $each: ["drama", "crime", "fantasy", "action"],
              $sort: 1 // sorting in ascending order; to sort in descending order set -1 
             },   // leave only one first element in `country` field
             country: {
                 $each: [],
                 $slice: 1 // you may set also negative values
             },   // push [10, 20, 30] starting from the third element
             numbers: {
                 $each: [10, 20, 30],
                 $position: 2
             }}
         }
     );
     collection.findOne(function(err, doc) {
         if (doc !== null) {  // Display only changed fields
             console.log("country:", doc.country);
             console.log("genres:", doc.genres);
             console.log("numbers:", doc.numbers);
         }
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/13.jpg"/></center>
&emsp;&emsp;Pay attention that you may combine few modifiers in one query for one field (and even use all of them) and how it should be done. `$sort`, `$slice` operators, work only with `$each` operator, and if you need to sort an array or select a few elements from there, you should set `$each` with empty array [] as we did it for the country field.<br/>
&emsp;&emsp;We have been updating many fields but every time has avoided the director field which has the object (or document) structure. To update a field within an embedded document, use the dot notation. When using the dot notation, enclose the whole dotted field name in quotes.  For example:<br/>
**`EX: 14`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.update(
         {title: "The Shawshank Redemption"},
         {$set: {
             // let’s change `director`’s name and add its age    
             "director.name": "Darabont",
             "director.age": 57
             }
         }
     );
     collection.findOne(function(err, doc) {
         if (doc !== null) console.log("director;\n", doc.director);
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/14.jpg"/></center>
&emsp;&emsp;The _id field may also be used in query selectors of the update method.<br/>
Of course, new values do not correspond to reality, and we have selected them to illustrate the work of updating operators.<br/>

### Deleting documents and collections
&emsp;&emsp;In the previous section we saw how to delete fields of a document with the help of `$unset` operator. You can use `remove`, `deleteOne` and `deleteMany` (first method will delete single, specified number or all documents matching query, second will ensure that only one is deleted, an the last deletes all documents matching the condition accordingly) methods to remove documents from a collection. The method takes a conditions document that determines documents to remove.<br/>
&emsp;&emsp;Now the “movies” collection consists of 4 documents. Let’s remove the document for “The Shawshank Redemption” movie<br/>
**`EX: 15`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.remove(
         {title: "The Shawshank Redemption"},
         function(err, res) {
             console.log('"The Shawshank Redemption" was deleted')
         }
     );
     collection.find().each(function(err, doc) {
         if (doc !== null) console.log(doc.title);
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/15.jpg"/></center>
&emsp;&emsp;The following operation removes all documents that match the specified condition - all documents with `rating = 7.8` (“Now You See Me” and “Lucky Number Slevin” have such rating)<br/>
**`EX: 16`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.deleteMany({rating: 7.8});
     collection.find().each(function(err, doc) {
         if (doc !== null) console.log(doc);
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/16.jpg"/></center>
&emsp;&emsp;Note, the callback function is not required. If you had used the `deleteOne` method, then only the one (the first) document would be removed.<br/>
&emsp;&emsp;To remove all documents use,  `remove` or `deleteMany` methods with empty conditional query {}. The `remove` or `deleteMany` methods only eliminate the documents from the collection. The collection itself, as well as any indexes for the collection, remain. To remove all documents from a collection, it may be more efficient to drop the entire collection, including the indexes, and then recreate the collection and rebuild the indexes. Use the `drop` method to drop a collection, including any indexes. Let’s make sure<br/>
**`EX: 17`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     // Display all collections
     db.listCollections().toArray(function(err, collection) {
         console.log(collection);
     });
     // Remove all documents in "movies" collection
     var collection = db.collection("movies");
     collection.deleteMany({}, function(err, doc) { 
         console.log("All documents were deleted"); 
     });
     collection.find().each(function(err, doc) {
         if (doc !== null) console.log(doc);
     });
     // The "movies" collection is empty now. Does it exist?
     db.listCollections().toArray(function(err, collection) {
         console.log(collection);
     });
     // Yes, it does. Let's drop the "movies" collection
     db.collection("movies").drop(function(err, res) { 
         console.log('The "movies" collection is deleted') 
     });
     // Check whether it still exists
     db.listCollections().toArray(function(err, collection) {
         console.log(collection);
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/17.jpg"/></center>
&emsp;&emsp;Pay attention, the `listCollections` method returns the list of all collections in a database.<br/>
&emsp;&emsp;In MongoDB, write operations are atomic on the level of a single document. If a single remove operation removes multiple documents from a collection, the operation can interleave with other write operations on that collection.<br/>

<div class="exercises-div">
<h4>Exercise 3</h4>
<p>Update documents in the “mountains” collection with following new fields:</p>
<ul>
	<li>for {<span class="key">"name"</span>: <span class="text">"Mount Everest"</span>, <span class="key">"height"</span>: <span class="num">8848</span>} add these fields</br>
<pre><code>
{
      <span class="key">"conquest_year"</span>: <span class="num">1953</span>, 
      <span class="key">"country"</span>: [<span class="text">"Nepal"</span>, <span class="text">"China"</span>],
      <span class="key">"mountains"</span>: <span class="text">"Himalayas"</span>,
      <span class="key">"continent"</span>: <span class="text">"Asia"</span>,
      <span class="key">"lat/lon"</span>: [<span class="num">27.988056</span>, <span class="num">86.925278</span>]
}
</code></pre>
	</li>

	<li>for {<span class="key">"name"</span>: <span class="text">"Mount Kilimanjaro"</span>, <span class="key">"height"</span>: <span class="num">5895</span>} add these fields</br>
<pre><code>
{
      <span class="key">"conquest_year"</span>: <span class="num">1889</span>, 
      <span class="key">"country"</span>: [<span class="text">"Tanzania"</span>],
      <span class="key">"mountains"</span>: <span class="text">"East African Plateau"</span>,
      <span class="key">"continent"</span>: <span class="text">"Africa"</span>,
      <span class="key">"lat/lon"</span>: [<span class="num">-3.075833</span>, <span class="num">37.353333</span>]
}
</code></pre>
	</li>

</ul>
</div>

<div class="exercises-div">
<h4>Exercise 4</h4>
<p>Download file “<a href="https://study.moderndeveloper.com/wp-content/uploads/2016/09/mountains_dataset.js" target="_blank">mountains_dataset.js</a>” and insert elements of <code>mountains</code> array to the “mountains” collection. Display message with documents amount in this collection.</p>
</div>

<div class="exercises-div">
<h4>Exercise 5</h4>
<p>The downloaded dataset contains few mistakes. You should correct them:</p>
<ul>
	<li>Aconcagua is in South America not in North America;</li>
	<li>the height of Aconcagua is 20% higher than the given value;</li>
	<li>the year of the the conquest of Aneto is 1817; change this value using $inc operator;</li>
	<li>Ras Dashen is on the territory of Ethiopia only;</li>
	<li>Mont Blanc contains only latitude; please add longitude 6.865 also.</li>
</ul>
</div>

<div class="exercises-div">
<h4>Exercise 6</h4>
<p>Delete document for Vinson Massif.</p>
</div>


## Finding of documents using query selectors
&emsp;&emsp;The “imdb” database is empty now. To continue our work we have to insert the data about collected movies from IMDB you saved earlier. So let’s copy `movies` array from the JavaScript file downloaded at the beginning of this lesson and past it to the end of “movies.js” file. We are going to insert all 12 objects of `movies` array to the movies collection<br/>
**`EX: 18`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.insert(movies, function(err, res) {
         console.log("The data were inserted")
     });
     collection.find().each(function(err, doc) {
         if (doc !== null) console.log(" " + doc.title)
     });
     db.close();
 });
 // You need to open the "lesson2_dataset.js" file, then copy all data and replace "var movies = [...]"
 var movies = [...]
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/18.jpg"/></center>
&emsp;&emsp;Use the dot notation like in `update` method above to specify a condition on a field within an embedded document. It is the general rule of MongoDB’s queries. Dot notation requires quotes around the whole dotted field name. The following operation specifies an equality condition on the `name` field in the `director` embedded document. It returns all documents where `director.name == "Frank Darabont"` (to avoid huge output for the whole documents we will display where it is possible only specific fields of this document which allow determining the correct filtering result, but you may print the entire document if any)<br/>
**`EX: 19`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
    collection.find({"director.name": "Frank Darabont"}).each(function(err, doc) {
         if (doc !== null) console.log(doc.title + ": " + doc.director.name)
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/19.jpg"/></center>
&emsp;&emsp;Note, the same way works also for filtering by arrays of embedded documents (the `actors` field in the collection).<br/>
&emsp;&emsp;But what if I want to filter by more than one field in the collection’s documents? Implicitly, a logical AND conjunction connects the clauses of a compound query so that the query selects the documents in the collection that match all the conditions. So, you need only to write all conditions by comma like in the example below where we display all movie’s titles without any oscar (`oscar_nominated == false`) and where Jesse Eisenberg was acted (`actors.name = "Jesse Eisenberg"`)<br/>
**`EX: 20`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.find({oscar_nominated: false, "actors.name": "Jesse Eisenberg"})
              .each(function(err, doc) {
         if (doc !== null) console.log(doc.title)
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/20.jpg"/></center>
&emsp;&emsp;Fine, but we are still too limited in document selection. For example, how to find all document with the `rating` over 8 or movies where at least one woman was acted (we mean here only records in the provided dataset), or all dramas, i.e. find where `genres` contain “drama” record? MongoDB provides operators to specify query conditions on<br/>

<table>
<thead>
<tr>
<th style="text-align: center"><b>Name</b></th>
<th style="text-align: center"><b>Description</b></th>
</tr>
</thead>
<tbody>
<tr bgcolor="#B3E5FC">
<td colspan="2"><strong>Comparison selectors</strong></td>
</tr>
<tr>
<td><code>$eq</code></td>
<td>Matches values that are equal to a specified value</td>
</tr>
<tr>
<td><code>$gt</code></td>
<td>Matches values that are greater than a specified value</td>
</tr>
<tr>
<td><code>$gte</code></td>
<td>Matches values that are greater than or equal to a specified value</td>
</tr>
<tr>
<td><code>$lt</code></td>
<td>Matches values that are less than a specified value</td>
</tr>
<tr>
<td><code>$lte</code></td>
<td>Matches values that are less than or equal to a specified value</td>
</tr>
<tr>
<td><code>$ne</code></td>
<td>Matches all values that are not equal to a specified value</td>
</tr>
<tr>
<td><code>$in</code></td>
<td>Matches any of the values specified in an array</td>
</tr>
<tr>
<td><code>$nin</code></td>
<td>Matches none of the values specified in an array</td>
</tr>

<tr bgcolor="#CE93D8">
<td colspan="2"><strong>Logical selectors</strong></td>
</tr>
<tr>
<td><code>$or</code></td>
<td>Returns all documents that match the conditions of either clause</td>
</tr>
<tr>
<td><code>$and</code></td>
<td>Returns all documents that match the conditions of both clauses</td>
</tr>
<tr>
<td><code>$not</code></td>
<td>Inverts the effect of a query expression and returns documents that do not match the query expression</td>
</tr>
<tr>
<td><code>$nor</code></td>
<td>Returns all documents that fail to match both clauses</td>
</tr>

<tr bgcolor="#A5D6A7">
<td colspan="2"><strong>Element selectors</strong></td>
</tr>
<tr>
<td><code>$exists</code></td>
<td>Matches documents that have the specified field</td>
</tr>
<tr>
<td><code>$type</code></td>
<td>Selects documents if a field is of the specified type</td>
</tr>

<tr bgcolor="#FFF59D">
<td colspan="2"><strong>Evaluation selectors</strong></td>
</tr>
<tr>
<td><code>$mod</code></td>
<td>Performs a modulo operation on the value of a field and selects documents with a specified result</td>
</tr>
<tr>
<td><code>$regex</code></td>
<td>Selects documents where values match a specified regular expression</td>
</tr>
<td><code>$text</code></td>
<td>Performs text search</td>
</tr>
<td><code>$where</code></td>
<td>Matches documents that satisfy a JavaScript expression</td>
</tr>

<tr bgcolor="#FFCDD2">
<td colspan="2"><strong>Array selectors</strong></td>
</tr>
<tr>
<td><code>$all</code></td>
<td>Matches arrays that contain all elements specified in the query</td>
</tr>
<tr>
<td><code>$elemMatch</code></td>
<td>Selects documents if element in the array field matches all the specified $elemMatch conditions</td>
</tr>
<td><code>$size</code></td>
<td>Selects documents if the array field is a specified size</td>
</tr>

<tr bgcolor="#FFCC80">
<td colspan="2"><strong>Geospatial selectors</strong></td>
</tr>
<tr>
<td><code>$geoWithin</code></td>
<td>Selects geometries within a bounding GeoJSON geometry</td>
</tr>
<tr>
<td><code>$geoIntersects</code></td>
<td>Selects geometries that intersect with a GeoJSON geometry</td>
</tr>
<td><code>$near</code></td>
<td>Returns geospatial objects in proximity to a point. Requires a geospatial index</td>
</tr>
<td><code>$nearSphere</code></td>
<td>Returns geospatial objects in proximity to a point on a sphere. Requires a geospatial index</td>
</tr>

<tr bgcolor="#D1C4E9">
<td colspan="2"><strong>Comments selectors</strong></td>
</tr>
<tr>
<td><code>$comment</code></td>
<td>Adds a comment to a query predicate</td>
</tr>

<tr bgcolor="#C5CAE9">
<td colspan="2"><strong>Projection selectors</strong></td>
</tr>
<tr>
<td><code>$</code></td>
<td>Projects the first element in an array that matches the query condition</td>
</tr>
<tr>
<td><code>$elemMatch</code></td>
<td>Projects the first element in an array that matches the specified $elemMatch condition</td>
</tr>
<td><code>$meta</code></td>
<td>Projects the document’s score assigned during $text operation</td>
</tr>
<td><code>$slice</code></td>
<td>Limits the number of elements projected from an array. Supports skip and limit slices</td>
</tr>
</tbody>
</table>
&emsp;&emsp;The following couple of examples demonstrates how to construct complex queries in MongoDB.<br/>
**`EX: 21`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.find({
         rating: {$gte: 6.9}, 
         $or: [
               { 
                next_parts: {$exists: true}, 
                actors: {$ne: {$size: 2}}, 
                genres: {$nin: ["drama", "triller"]},
               }, 
               {
                genres: {$in: ["drama", "crime"]}, 
                year: {$lt: 1995},
                "actors.0.name": {$regex: "Tom"},
               } 
              ]
     }).each(function(err, doc) {
         if (doc !== null) console.log(doc.title)
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/21.jpg"/></center>
&emsp;&emsp;Thus, in this example we are building enough complex finding query and are searching all movies with `rating` not less than 6.9 (“The Shawshank Redemption”, “Now You See Me”, “Lucky Number Slevin”, “Forrest Gump”, “Now You See Me 2”, “The Green Mile”, “Taxi” and “Shichinin no samurai” satisfy this condition). Then we are narrowing the set of movies by selecting of movies corresponding one of the following conditions (the first OR the second ones):<br/>
<ol class="not_order">
  <li><span>1.1</span> The selected movie should have continuations, i.e. the <code>next_parts</code> field (only “Now You See Me” and “Taxi” lie here)</li>
  <li><span>1.2</span> It should not have 2 <code>actors</code> documents (1, 3 or 4 is allowable, but only not 2) (the “Taxi” movies remains only); <code>$size</code> counts all documents in <code>actors</code> field</li>
  <li><span>1.3</span> And finally this movie should not be a drama or a thriller (“Taxi” satisfies this condition)</li>
  <li><span>2.1</span> The selected movie should be a drama or a crime (“The Shawshank Redemption”, “Now You See Me”, “Lucky Number Slevin”, “Forrest Gump”, “Now You See Me 2”, “The Green Mile” and “Taxi”)</li>
  <li><span>2.2</span> It was released before 1995 (only “The Shawshank Redemption” and  “Forrest Gump” remain)</li>
  <li><span>2.3</span> The first actor has a name Tom (“Forrest Gump”  remains because of Tom Hanks is written as its first actor).</li>
</ol>
As a result, we have only two movies satisfying absolutely all conditions: “Taxi” and “Forrest Gump”.<br/>
&emsp;&emsp;`$or` may have more than two sets of conditions.<br/>
&emsp;&emsp;To select a specific element in an array (like we did for the first element in the actors array) use the following rule **`<array_name>.<element_index>.<embeded_document_filed>`**.<br/>
&emsp;&emsp;Instead of `$regex` operator you can also use regular expression objects (i.e. `/pattern/`) to specify regular expressions (for example, the condition **`"actors.0.name": {$regex: "Tom"}`** may look **`"actors.0.name": /Tom/`**). Remember, you cannot use `$regex` operator expressions inside an `$in`. The full list of regular expressions syntax you may find here <a href="https://study.moderndeveloper.com/wp-content/uploads/2016/09/RE-table.docx" target="_blank">Regular expressions table</a>. Please look at this table and learn provided examples. Particularly, if we write **`"actors.0.name": /T.m/`** (or the same **`"actors.0.name": {$regex: "T.m"}`**), i.e. any character may be between letters T and m, then both Tim Robbins and Tom Hanks will fit the pattern and the movie “The Shawshank Redemption” will also satisfy the second set of conditins in `$or` operator.<br/>
&emsp;&emsp;Just one example where we want to display all movies where are given information about at least one woman (in `director` or `actors` fields), where the age of director is over 60 years (and, of course, he/she is alive) and which was released in an even year.<br/>
**`EX: 22`**<br/>
<pre lang="javascript" line="5">
 MongoClient.connect(url, function(err, db) {
     assert.equal(null, err);
     var collection = db.collection("movies");
     collection.find({
         $or: [
             {"director.gender": "female"}, 
             {"actors.gender": "female"}, 
         ],
         $and: [
             {"director.died": {$exists: false}}, 
    {$where: "Math.abs(new Date(Date.now() - new Date(this.director.born).getTime()).getUTCFullYear() - 1970) > 60"}
         ],
         year: {$mod: [2, 0]}
     }).sort({year: -1})
     .each(function(err, doc) {
         if (doc !== null) console.log(doc.title + ", " + doc.year)
     });
     db.close();
 });
</pre>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/09/22.jpg"/></center>
&emsp;&emsp;To specify an order for the result set, we have appended the sort method to the query. Pass to sort method a document which contains the field(s) to sort by and the corresponding sort type, e.g. 1 for ascending and -1 for descending (as we did for the year field).
&emsp;&emsp;Another cursor methods will be considered in the next lesson.<br/>
&emsp;&emsp;Above examples do not contain all query operators. Most of them are very straightforward and similar to those we have already used. Some of them we will investigate more detail in the next lessons.<br/>
<div class="exercises-div">
<h4>Exercise 7</h4>
<p>Display all mountains lying in Himalayas.</p>
</div>

<div class="exercises-div">
<h4>Exercise 8</h4>
<p>Display all mountains conquered between 1910 and 1960 with the height over 6000 m.</p>
</div>

<div class="exercises-div">
<h4>Exercise 9</h4>
<p>Show all mountains lying on the territory of China (it may belong also to some other country) or  those that have the word “Mount” in the name. Order output results by the height in descending order.</p>
</div>

<div class="exercises-div">
<h4>Exercise 10</h4>
<p>Display all mountains lying only in China or on territory of at least two countries but not in Asia.</p>
</div>

## Conclusions
In this lesson we’ve shown you:<br/>
<ul>
	<li>how to create a new database;</li>

	<li>how to create, remove collection and fill them with data;</li>

	<li>how to insert, update, read and remove documents in collections;</li>

	<li>and finally how to select documents satisfying to some conditions and display them.</li>

</ul>


&emsp;&emsp;We’ve considered the essential update and query operators, and now we can manipulate the data. But we still don’t know how, for example, how to group documents by some column and aggregate some field, for example, group all movies by its released year and calculate the average rating or count in how many movies was acted some actor, etc. Also, it would be perfect to have the possibility to do full-text search when collections have a lot of text fields. The next lessons will be devoted to these questions.<br/>
<div class="questions-div">
<h4>Questions</h4>
<ol>
	<li>How does MongoDB store the data? What is a collection and a document?</li>

	<li>How to connect to MongoDB server using API for Node.js?</li>

	<li>How to create a new MongoDB database using both API for Node.js and mongo shell?</li>

	<li>How to display all existing databases? Why may this list contain not all databases?</li>

	<li>How can a new collection be created?</li>

	<li>How to add a new document to the collection? Which methods are used for creation of new documents?</li>

	<li>How to display all documents? And how to display some specific records?</li>

	<li>What is ObjectID?</li>

	<li>Which methods are used for changing whole documents?</li>

	<li>How to change a specific field of a document?</li>

	<li>List update operators. What does each of them do? What may they be used for?</li>

	<li>Which data types of MongoDB do you know?</li>

	<li>Why we cannot apply few update operators within one update query?</li>

	<li>How to delete one or many documents?</li>

	<li>How to delete the whole collection? What is the difference between an empty and a removed collection?</li>

	<li>How to find documents by embedded fields?</li>

	<li>How to filter documents by a specific element (both value and index) of an array field?</li>

	<li>List query selectors? How do they work?</li>

	<li>How to filter documents by text field? What is the regular expression? How can patterns of regular expressions be constructed?</li>

	<li>Is there a possibility to order found documents?</li>

</ol>
</div>
