Download Link: https://assignmentchef.com/product/solved-swen-304-database-system-engineering-project-2
<br>
Project 2<strong>                                                                                         </strong>

<strong> </strong>School of Engineering and Computer Science

<strong> </strong>This project will build your understanding of database transaction processing and locking using JDBC.

The project is worth 10% of your final grade, and is marked out of 100.

<h1>Submission</h1>

You should submit the following two files:

<ul>

 <li>java and</li>

 <li>java</li>

</ul>

electronically using the link to the submission system on the course home page. You do not need to hand in paper copies of your program.




<h1>Background</h1>

To do the project you will use

<ul>

 <li>PostgreSQL Database Management System to build a database,</li>

 <li>Java programming language to write a transaction program, and</li>

 <li>Java Database Connectivity (JDBC) library to exchange data between your database and your transaction program.</li>

</ul>




To familiarise yourself with JDBC use the lecture and review notes on JDBC published on the course web page as an introductory resource for learning how to use JDBC in your programs.

You can find out more about JDBC from PostgreSQL and Java Tutorial

(http://ecs.victoria.ac.nz/technical/java/tutorial/index.html). You will find both manuals in the Technical web pages that are referenced from the SWEN 304 website. Do not print out all of the tutorials.

<h1>Short Description of the Problem</h1>

In this project, you will implement a simple library system. The system keeps track of books, authors, library customers, and their relationships.

Using your system, a library clerk should be able to retrieve information about books, authors, and customers. Also, the clerk should be able to update the appropriate records when a customer borrows or returns a book. Your program should send polite and understandable messages in a plain language to users.

Your program should be written in such a way that allow multiple instances of same program to access and update a common database without introducing any inconsistencies. Therefore, you should make a real transaction program.

<h1>Setting Up the PostgreSQL Database</h1>

Your transaction program will need to use Java libraries, PostgreSQL, and the PostgreSQL JDBC driver. To use all of these, you should insert the following command in your .cshrc file:

<h2>need comp302tools   or  need postgresql</h2>




It should be inserted between the lines

need SYSfirst … need SYSlast

The comp302tools package sets up the CLASSPATH to include “postgresql94.jar” (which contains a JDBC driver manager), and also does a “need java2 ” and a “need postgresql”. So if you already have any of these other “need” lines you can delete them (although it won’t harm to keep them).




Create your database, named <strong>userid</strong><strong>_jdbc </strong>by typing

<h2>&gt; createdb userid_jdbc</h2>

Note, this is the only database that should be set up to allow access from a Java program. Please use your ECS username as your userid.




To set up the password for your new database &lt;userid&gt;_jdbc, open an existing database (say with the name &lt;userid&gt;) by typing:

<strong>&gt; psql </strong>userid

and then proceed with

<h2>userid =&gt;ALTER USER ECS username WITH PASSWORD</h2>

<strong>’</strong>newpassword<strong>’;</strong>

This will set your password for the database &lt;userid&gt;_jdbc. Note, this password should NOT be the same as your system password. It should protect your database from possible misuse, but need not to be as secure as your system password. Type

userid=&gt;q

to close your old database.




To load your database with schema and data, you should use the file

Project2_19.data

which contains CREATE TABLE and INSERT commands and data to populate your database. You will find the file on the Assignments&amp;Projects course web page. Store it in your private directory. The command

<strong>&gt; psql –d </strong>db_name<strong> -f </strong>~/Project2_19.data

executes SQL CREATE TABLE and INSERT commands in the file /Project2_19.data and populates your database. Note, db_name will be <strong>userid</strong><strong>_jdbc.</strong>




For this project, you are not asked to use your database in an interactive mode, but if you need to (to try out a query, for example), do it in the same way that you did in Project 1.







<h2>Eclipse</h2>

<strong> </strong>

If you are using Eclipse you need to:




<ul>

 <li>Select the project you are working on.</li>

 <li>Select “Project” -&gt; “Properties” from the Menu</li>

 <li>Select “Java Build Path” from the window that appears</li>

 <li>Select “Add External Jars”</li>

 <li>Navigate to “/usr/pkg/lib/java/postgresql94.jar”</li>

</ul>




<h1>The Structure of the Database Transaction Application</h1>

The database transaction application has the following components:




<ul>

 <li>A library database (in the file data)</li>

 <li>A Java program named java that contains the code for the GUI and the main method.</li>

 <li>A Java file named java that should implement methods that provide functionality to the options offered by the GUI in LibraryUI.java.</li>

</ul>




A dump of the schema and data for the database, and the two java files are in the Assignment web page. You will need to complete the LibraryModel.java file, but you should not need to modify LibraryUI.java at all. <strong>The Library Database </strong>

Table 1 contains descriptions of the library database tables, and Table 2 contains description of the referential integrity constraints.




In the book table, the attribute <em>NumOfCop</em> is the number of copies of a book that the library owns, and the attribute <em>NumLeft</em> is the number of copies of a book that are currently available in the library. The difference between <em>NumOfCop</em> and <em>NumLeft</em> indicates how many copies of a book are currently out on loan. A book can be simultaneously loaned up to <em>NumOfCop</em> customers.




<table width="627">

 <tbody>

  <tr>

   <td width="104"><strong>Attribute</strong></td>

   <td width="87"><strong>Data Type</strong></td>

   <td width="70"><strong> Length</strong></td>

   <td width="46"><strong>Null</strong></td>

   <td width="42"><strong>Def</strong></td>

   <td width="278"><strong>Condition</strong></td>

  </tr>

  <tr>

   <td colspan="6" width="627"><strong>Table name: <em>Customer</em>, Primary Key: <em>CustomerId</em></strong></td>

  </tr>

  <tr>

   <td width="104"><em>CustomerID</em></td>

   <td width="87">Integer</td>

   <td width="70">4</td>

   <td width="46">N</td>

   <td width="42">0</td>

   <td width="278">≥ 0</td>

  </tr>

  <tr>

   <td width="104"><em>L_Name </em></td>

   <td width="87">Char</td>

   <td width="70">15</td>

   <td width="46">N</td>

   <td width="42"></td>

   <td width="278"></td>

  </tr>

  <tr>

   <td width="104"><em>F_Name </em></td>

   <td width="87">Char</td>

   <td width="70">15</td>

   <td width="46">Y</td>

   <td width="42"></td>

   <td width="278"></td>

  </tr>

  <tr>

   <td width="104"><em>City </em></td>

   <td width="87">Char</td>

   <td width="70">15</td>

   <td width="46">Y</td>

   <td width="42"></td>

   <td width="278">(Wellington, Upper Hutt, Lower Hutt)</td>

  </tr>

  <tr>

   <td colspan="6" width="627"><strong>Table name: <em>Cust_Book</em>, Primary Key: <em>CustomerId</em> + <em>ISBN</em></strong></td>

  </tr>

  <tr>

   <td width="104"><em>CustomerId </em></td>

   <td width="87">Integer</td>

   <td width="70">4</td>

   <td width="46">N</td>

   <td width="42">0</td>

   <td width="278">≥ 0</td>

  </tr>

  <tr>

   <td width="104"><em>DueDate </em></td>

   <td width="87">Date</td>

   <td width="70"></td>

   <td width="46">Y</td>

   <td width="42"></td>

   <td width="278"></td>

  </tr>

  <tr>

   <td width="104"><em>ISBN </em></td>

   <td width="87">Integer</td>

   <td width="70">4</td>

   <td width="46">N</td>

   <td width="42">0</td>

   <td width="278">≥ 0</td>

  </tr>

  <tr>

   <td colspan="6" width="627"><strong>Table name: <em>Book</em>, Primary Key: <em>ISBN</em></strong></td>

  </tr>

  <tr>

   <td width="104"><em>ISBN </em></td>

   <td width="87">Integer</td>

   <td width="70">4</td>

   <td width="46">N</td>

   <td width="42"> 0</td>

   <td width="278">≥ 0</td>

  </tr>

  <tr>

   <td width="104"><em>Title </em></td>

   <td width="87">Char</td>

   <td width="70">60</td>

   <td width="46">N</td>

   <td width="42"></td>

   <td width="278"></td>

  </tr>

  <tr>

   <td width="104"><em>Edition_No </em></td>

   <td width="87">SmallInt</td>

   <td width="70">2</td>

   <td width="46">Y</td>

   <td width="42">1</td>

   <td width="278">&gt; 0</td>

  </tr>

  <tr>

   <td width="104"><em>NumOfCop</em></td>

   <td width="87">SmallInt</td>

   <td width="70">2</td>

   <td width="46">N</td>

   <td width="42">1</td>

   <td width="278"></td>

  </tr>

  <tr>

   <td width="104"><em>NumLeft</em></td>

   <td width="87">SmallInt</td>

   <td width="70">2</td>

   <td width="46">N</td>

   <td width="42">1</td>

   <td width="278"></td>

  </tr>

  <tr>

   <td colspan="6" width="627"><strong>Table name: <em>Author</em>, Primary Key: <em>AuthorId</em></strong></td>

  </tr>

  <tr>

   <td width="104"><em>AuthorId</em></td>

   <td width="87">Integer</td>

   <td width="70">4</td>

   <td width="46">N</td>

   <td width="42">0</td>

   <td width="278">≥ 0</td>

  </tr>

  <tr>

   <td width="104"><em>Name</em></td>

   <td width="87">Char</td>

   <td width="70">15</td>

   <td width="46">Y</td>

   <td width="42"></td>

   <td width="278"></td>

  </tr>

  <tr>

   <td width="104"><em>Surname </em></td>

   <td width="87">Char</td>

   <td width="70">15</td>

   <td width="46">N</td>

   <td width="42"></td>

   <td width="278"></td>

  </tr>

  <tr>

   <td colspan="6" width="627"><strong>Table name: <em>Book_Author</em>, Primary Key: (<em>ISBN</em> + <em>AuthorId</em>)</strong></td>

  </tr>

  <tr>

   <td width="104"><em>ISBN</em></td>

   <td width="87">Integer</td>

   <td width="70">4</td>

   <td width="46">N</td>

   <td width="42">0</td>

   <td width="278">≥ 0</td>

  </tr>

  <tr>

   <td width="104"><em>AuthorId </em></td>

   <td width="87">Integer</td>

   <td width="70">4</td>

   <td width="46">N</td>

   <td width="42">0</td>

   <td width="278">≥ 0</td>

  </tr>

  <tr>

   <td width="104"><em>AuthorSeqNo </em></td>

   <td width="87">Integer</td>

   <td width="70">2</td>

   <td width="46">Y</td>

   <td width="42">1</td>

   <td width="278">&gt; 0</td>

  </tr>

 </tbody>

</table>

<strong>Table 1.</strong>




<table width="628">

 <tbody>

  <tr>

   <td rowspan="2" width="100"></td>

   <td width="104"></td>

   <td colspan="3" width="296"><em>Referencing Relation Schemas</em></td>

   <td width="129"></td>

  </tr>

  <tr>

   <td width="104"><em>Customer</em></td>

   <td width="104"><em>Cust_Book</em></td>

   <td width="96"><em>Book</em></td>

   <td width="96"><em>Author</em></td>

   <td width="129"><em>Book_Author</em></td>

  </tr>

  <tr>

   <td width="100"><em>Customer </em></td>

   <td width="104"><em> </em></td>

   <td width="104">(<em>d</em>, <em>r</em>), (<em>m</em>, <em>r</em>)</td>

   <td width="96"><em> </em></td>

   <td width="96"><em> </em></td>

   <td width="129"></td>

  </tr>

  <tr>

   <td width="100"><em>Cust_Book</em></td>

   <td width="104"><em> </em></td>

   <td width="104"></td>

   <td width="96"></td>

   <td width="96"></td>

   <td width="129"></td>

  </tr>

  <tr>

   <td width="100"><em>Book</em></td>

   <td width="104"></td>

   <td width="104">(<em>d</em>, <em>r</em>), (<em>m</em>, <em>r</em>)</td>

   <td width="96"></td>

   <td width="96"></td>

   <td width="129">(<em>d</em>, <em>d</em>), (<em>m</em>, <em>c</em>)</td>

  </tr>

  <tr>

   <td width="100"><em>Author</em></td>

   <td width="104"></td>

   <td width="104"></td>

   <td width="96"></td>

   <td width="96"></td>

   <td width="129">(<em>d</em>, <em>d</em>), (<em>m</em>, <em>c</em>)</td>

  </tr>

  <tr>

   <td width="100"><em>Book_Author</em></td>

   <td width="104"></td>

   <td width="104"></td>

   <td width="96"></td>

   <td width="96"></td>

   <td width="129"></td>

  </tr>

 </tbody>

</table>

<strong>Table 2.</strong>

<strong> </strong>

The referential integrity constraints are specified in Table 2. The relation schemas in the column headers are referencing the relation schemas in the row headers. If the corresponding table cell is empty, there is no referential integrity constraint defined between the two relation schemas. If the cell is not empty, then there is a referential integrity constraint defined between the two relation schemas, and the cell contains a pair (<em>operation, action</em>). <em>Operation</em> is either <em>d</em> (delete), or <em>m</em> (modify), and <em>action </em>can be <em>r</em> (restrict), <em>c</em> (cascade), <em>n</em> (set to null), or <em>d</em> (set default).




<h2>The LibraryUI.java and LibraryModel.java Files</h2>

You should copy LibraryUI.java and LibraryModel.java into your own directory, compile them both using

&gt;javac LibraryUI.java and run using

&gt; java LibraryUI

You will see that the program runs, setting up a GUI, and asking for a username and database password. However, at present none of the buttons on the GUI do much. They only display some messages in the GUI result area. LibraryUI.java, which contains the code for the GUI, is complete and you do not need to change it. LibraryModel.java contains a skeleton consisting of stubs of all the methods that provide the functionality for the buttons of the GUI. You need to implement all these methods.




The GUI contains an area for displaying results of queries, and a menu (tabs) with the following options:

<ul>

 <li>Book o Book Lookup o Show Catalogue o Show Loaned Books</li>

 <li>Author o Show Author o Show All Authors</li>

 <li>Customer o Show Customer o Show All Customers</li>

 <li>Borrow Book</li>

 <li>Return Book</li>

 <li>File o Exit</li>

</ul>




When the GUI is constructed, it will create an instance of the LibraryModel class, passing it a username and a database password. Each option on the GUI will then call a method of the LibraryModel object, and display the string returned by the method in the result area.




Note, you can change the given GUI, but you should not expect to receive extra marks for it.

<h2>Demo Program</h2>

There is a demo version of the program in the ~comp302/ directory that you can run on your own database. The demo program was initially made by Pavle, but was corrected and decorated by Jerome Dolman. To run the demo version, you need to:

<ul>

 <li>Create your own &lt;userid&gt;_jdbc database,</li>

 <li>Register your new password with PostgreSQL,</li>

 <li>Populate your own &lt;userid&gt;_jdbc database using data file  Type:</li>

</ul>

&gt; demo comp302 project2 from Unix shell prompt, and

<ul>

 <li>Enter your &lt;user_name&gt; and the new password in the Authentication Pane that will appear on the screen. <strong>Note: </strong></li>

 <li>This is just a <strong>demo</strong>, you may want to make a much better java program.</li>

</ul>

<h1>What should you do</h1>

<ol>

 <li>Load your database from the file data.</li>

 <li>Complete the implementation of the LibraryModel class:

  <ol>

   <li>Define the constructor to open a connection to the database</li>

   <li>Define each of the methods to perform an appropriate SQL query (or queries) and return the result of the query as a string to be displayed.</li>

  </ol></li>

</ol>




When implementing methods of the LibraryModel class, pay attention to commands for handling transactions correctly (for starting and ending a transaction, and locking), and to JDBC commands (for acquiring a driver object, establishing a connection to the database, controlling the transaction environment, and executing SQL statements using Statement and PreparedStatement objects). For the sake of performance, allow read only queries to read all database items, regardless whether they are locked or not. Most of your methods will need to handle exceptions raised by the JDBC commands. The most important goal of your transaction program is to leave the database in a consistent state after executing each of the requested database operations.




An important part of the database application you are going to implement is the method for the

“BorrowBook” transaction. After receiving <em>ISBN, CustomerId</em>, and <em>DueDate</em> values from the GUI that method needs to perform the following actions (assuming that none of the actions fails):




<ol>

 <li>Check whether the customer exists (and lock him/her as if the delete option were available).</li>

 <li>Lock the book (if it exists, and if a copy is available).</li>

 <li>Insert an appropriate tuple in the <em>Cust_Book</em></li>

 <li>Update the <em>Book</em> table, and</li>

 <li>Commit the transaction (if actions were all successful, otherwise rollback)</li>

</ol>




This transaction is intended to work in a multi-user transaction-processing environment where there are multiple copies of your program running at the same time. To allow you (and the marker) to test whether your program works correctly, insert an interaction command between steps 3 and 4 above. This interaction command should put up a dialog box and ask the user to click YES/OK to continue. The effect of this is to stall the processing of the program while you run another copy of the program and invoke a transaction that should interfere with the transaction that is part way through. This way, you can simulate contention for the same database items, and check whether your program:

 Avoids lost update, dirty and unrepeatable read for database update transactions and  Allows reading all database items for read only transactions.

<strong> </strong>

Apply the steps, similar to steps 1 to 5 above also when implementing the Return Book function. Of course, this time you need to delete an appropriate tuple from the <em>Cust_Book</em> table.

<strong>How Will the Project be Marked?</strong>

The project contains two parts:




The first part will be worth 85 marks and consists of the following tasks:




<table width="602">

 <tbody>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the database</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[1 mark]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Book Lookup Function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[5 marks]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">(show book authors sorted according to AuthSeqNo)</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74"></td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Show Catalogue function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[5 marks]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Show Loaned Books function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[5 marks]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Borrow Book function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[35 marks]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Exit function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[4 marks]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Show Author function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[5 marks]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Show All Authors function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[5 marks]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Show Customer function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[5 marks]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Show All Customers function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="74">[5 marks]</td>

  </tr>

  <tr>

   <td width="24"></td>

   <td width="360">Implement the Return Book function</td>

   <td width="48"></td>

   <td width="48"></td>

   <td width="48"></td>

   <td rowspan="2" width="74">[10 marks]</td>

  </tr>

  <tr>

   <td colspan="5" width="528">The second part will be worth 15 marks and consists of the following tasks:</td>

  </tr>

  <tr>

   <td colspan="5" width="528"> Implement Delete customer function,</td>

   <td width="74">[6 marks]</td>

  </tr>

  <tr>

   <td colspan="5" width="528"> Implement Delete author function, and</td>

   <td width="74">[3 marks]</td>

  </tr>

  <tr>

   <td colspan="5" width="528"> Implement Delete book function.</td>

   <td width="74">[6 marks]</td>

  </tr>

 </tbody>

</table>




The <strong>marking</strong> will be performed by markers after your electronic submission.




Also note we expect your code to be tidy and properly indented. Also, we expect to see user friendly and properly structured messages from your program. Please provide a regular exit from your program, particularly in the case of an error, e.g., database connection error. If you do not meet these expectations, there may be some penalties.




<h1>Using PostgreSQL on the workstations</h1>

<strong> </strong>

We have a command line interface to PostgreSQL server, so you need to run it from a Unix prompt in a shell window. To enable the various applications required, first type either

<h2>&gt; need comp302tools</h2>

You may wish to add “need comp302tools” command to your .cshrc file so that it is run automatically. Add this command after the command need SYSfirst, which has to be the first need command in your .cshrc file.




There are several commands you can type at the unix prompt:

<h2>&gt; createdb 〈userid_jdbc〉</h2>

Creates an empty database. The database is stored in the same PostgreSQL default school cluster used by all the students in the class. <strong>Note</strong>, for this project, your database has to have the name userid_jdbc, where userid is your UNIX userid. To ensure security, you must define a password, as instructed in this handout. You only need to do this once (unless you get rid of your database to start again).

<h2>&gt; psql  [ –d 〈db name〉]</h2>

Starts an interactive SQL session with PostgreSQL to create, update, and query tables  in the database. The db name is optional (unless you have multiple databases)

<h2>&gt; dropdb 〈db name〉</h2>

Gets rid of a database. (In order to start again, you will need to create a database again)

<strong>&gt; pg_dump  -i </strong>〈db name〉&gt;〈file name〉

Dumps your database into a file in a form consisting of a set of SQL commands that would reconstruct the database if you loaded that file.

&gt; <strong>psql –d</strong> &lt;database_name&gt; <strong>-f</strong> &lt;file_name&gt;

Copies the file &lt;file_name&gt; into your database &lt;database_name&gt;.

Inside an interactive SQL session, you can type SQL commands. You can type the command on multiple lines (note how the prompt changes on a continuation line). End commands with a

‘;’

There are also many single line PostgreSQL commands starting with ‘’ . No ‘;’ is required.

The most useful are

<strong>?   </strong> to list the commands,

<strong>i </strong> 〈file name〉 loads the commands from a file (e.g., a file of your table definitions or the file of data we provide).

<strong>dt </strong> to list your tables.

<strong>d</strong>〈table name〉to describe a table.

<strong>q   </strong> to quit the interpreter

<strong>copy</strong> &lt;table_name&gt; <strong>to</strong> &lt;file_name&gt;

Copy your table_name data into the file file_name.

<strong>copy</strong> &lt;table_name&gt; <strong>from</strong> &lt;file_name&gt;

Copy data from the file file_name into your table table_name.




Note also that the PostgreSQL interpreter has some line editing facilities, including up and down arrow to repeat previous commands.




For longer commands, it is safer (and faster) to type your commands in an editor, then paste them into the interpreter!





