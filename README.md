# Connect-a-Web-App-with-Aurora
Let's build a mini version of this pattern, by setting up a web app from scratch and connecting it with a relational database (Amazon Aurora) to store things that users input.

<img width="1464" height="1051" alt="image" src="https://github.com/user-attachments/assets/5fc88135-c6cb-4171-a5a2-fe827d370ca9" />

Step-by-Step Guidance

Welcome to the Step-by-Step Guidance version of this project. Let's do this!

STEP 1:- Login with your IAM user
STEP 2:- Launch an EC2 instance for your web app

<img width="650" height="497" alt="image" src="https://github.com/user-attachments/assets/c1c66a16-7059-428c-bed8-6af6bb6d2893" />

Choose the following settings in the Launch an instance page.
Enter nextwork-ec2-instance-web-server for the instance's name.
Under Application and OS Images (Amazon Machine Image), choose Amazon Linux.
Choose the Amazon Linux 2023 AMI.
Keep the defaults for the other choices.

<img width="1432" height="1196" alt="image" src="https://github.com/user-attachments/assets/b206ff54-6bf6-4225-a800-6f80afdf474d" />

Under Instance type, choose t2.micro.
Under Key pair (login), choose a Create new key pair.
For your Key pair name, enter NextWorkAuroraApp.
Leave your Key pair type as RSA.
Leave your Private key file format as .pem since we're using SSH later on to access our EC2 instance.
Select Create key pair.
A new file will automatically download to your computer - don't be alarmed!
This is your new key pair. We'll need it for later in the project to access our EC2 instance.
Back in our EC2 creation, under Network settings, set these values and keep the other values as their defaults:
For Allow SSH traffic from, choose your IP address if it's correct (you can check your IP by clicking here). Otherwise select Anywhere.
Check the boxes for Allow HTTP traffic from the internet.

<img width="1178" height="864" alt="image" src="https://github.com/user-attachments/assets/ffc78bbd-85d6-4e88-87f5-9d370e10f52a" />

When you're ready, choose Launch instance.
Navigate back to your list of EC2 instances, and then select the checkbox next to your new instance.
In the Details tab, note the following important details:
In Instance summary, note the Public IPv4 DNS: YOUR_EC2_ADDRESS.
Note the value for Key pair assigned at launch.

Wait until Instance state for your instance is Running before continuing.

STEP 3:- Create an Aurora MySQL Database

Let's create your Aurora database
Head to RDS console - search for rds in search bar at the top of the screen.
Notice that even if you search for Aurora, the same result shows up!
<img width="1373" height="446" alt="image" src="https://github.com/user-attachments/assets/908c8c9a-bef8-4a84-9b02-549c94cdf2eb" />

In the left navigation bar, select Databases.
In the Database section, select Create Database.
On the Create database page, choose Standard Create.
In the Configuration section, make the following changes:
For Engine type, choose Aurora (MySQL Compatible).
For Engine Version, choose Aurora MySQL 3.08.2 (compatible with MySQL 8.0.39) - default for major version 8.0
For Templates, choose Dev/Test

<img width="1363" height="1443" alt="image" src="https://github.com/user-attachments/assets/9e4bf1d9-db17-4078-bf8f-266e7bc315b7" />

In the Settings section, set these values:
DB cluster identifier: nextwork-db-cluster
Master username: admiN
For Credentials management select Self managed.
Master password: Type a password (eg. n3xtw0rk)
Confirm password: Retype the password
Make sure you save your database login details somewhere safe! You'll need them later on.

<img width="1790" height="1053" alt="image" src="https://github.com/user-attachments/assets/69da2b8a-a85f-4e91-8a65-30aead0bf530" />

Leave the Cluster storage configuration settings as default.
In the Instance configuration section, set these values:
Burstable classes (includes t classes)
db.t3.medium

<img width="1116" height="529" alt="image" src="https://github.com/user-attachments/assets/e20dd4a0-d3c7-4fe3-823c-294c427abaf5" />

In the Availability and durability section, use the default values. We'll jump into what this means later!
In the Connectivity section, the first thing it asks us is whether we need to connect to an EC2 instance...
For Compute resource, choose Connect to an EC2 compute resource.
Now select the drop down for EC2 instance, and choose nextwork-ec2-instance-web-server.
Scroll down and open the Additional configuration section.
Enter sample for Initial database name.
Keep the default settings for the other options.
Select Create database.
Close any pop-ups that appear.
Your new DB cluster will show in the Databases list with the status Creating.

<img width="1578" height="550" alt="image" src="https://github.com/user-attachments/assets/e88c9674-322f-4464-9db7-ad89527045a5" />

Wait for the Status of your new DB cluster to show as Available.
Select the DB identifier of your top database to take a look at the details.
Notice that there are two Endpoints in our Database. Cool! This is our cluster in action.

This is what we created in this step:

<img width="1047" height="807" alt="image" src="https://github.com/user-attachments/assets/e619f860-b571-4f3c-a184-d1c8069ec98f" />

STEP 4:- Create your web app

On a Windows/Linux:
Press Windows + R to open the Run dialog.
Type cmd or powershell and press Enter.
<img width="677" height="229" alt="image" src="https://github.com/user-attachments/assets/f834c6ae-ce6e-4645-a605-f140e5aa0770" />

Connect to your EC2 instance

You need to access your .pem file in order to login successfully to your EC2 instance - remember, the .pem file is like your keys to your EC2 instance!
Find your .pem file on your local computer (it's probably in your downloads folder!) and put it in a new folder on your Desktop labelled nextwork
Nice! Now we need to navigate to that folder from your terminal, so we can use it.
Run the command ls in your terminal - this shows you all the folders that you can see from your current terminal position.
If you can see the Desktop folder when you run ls then you're in the right place
If you can't see the Desktop folder, then use the following commands to navigate up and down your folders until you can see Desktop...
To go back one folder run the command cd ../.
To go into a folder, run the command cd followed by the folder you'd like to enter (eg. cd nextwork or cd Desktop).

Once you can see your Desktop folder, navigate into that folder by running cd Desktop.
Then navigate into your nextwork folder by running cd nextwork.
Run ls to make sure your .pem file is there!

<img width="472" height="138" alt="image" src="https://github.com/user-attachments/assets/1d7b82ca-2f89-4102-a1bb-3a9cd2b8c376" />

Great! We have found our "keys" to get into our EC2 instance. Let's get inside!
Back in your terminal run the following command:

ssh -i NextWorkAuroraApp.pem ec2-user@YOUR_EC2_ADDRESS

<img width="867" height="343" alt="image" src="https://github.com/user-attachments/assets/63577b06-095f-4c12-90ac-ec8c392158b1" />

Now remember that our EC2 instance is just another computer. First thing we need to do is make sure all the software on it is up to date!
Run the following command:

sudo dnf update -y

After the updates complete, we're going to install a whole bunch of stuff needed to run your web app. In particular...
an Apache web server - the most widely used web server in the world. A web server is a software that gets your content (e.g. web pages) to users via the internet.
PHP - a programming language used for writing beautiful app pages.
php-mysqli - a PHP library (i.e. a collection of pre-written code to help you save time) for establishing a MySQL connection to your database.
MariaDB - a relational database management system. Your Aurora database knows how to manage its data (e.g. how to add a new row or retrieve data), but your web server doesn't know how to send instructions to and understand the responses from your Aurora database. Your web server would need MySQL-compatible client libraries i.e. software designed for servers to communicate with a MySQL database. Installing MariaDB in your EC2 instance is one of the many ways to install these client libraries so it can interact with Aurora.

sudo dnf install -y httpd php php-mysqli mariadb105

All systems go! Let's start the most basic version of our web app with the following command:

sudo systemctl start httpd


Test that your web app is running by entering your IPv4 DNS address into your browser.
Make sure you change https to http - no 's'!

  http://YOUR_EC2_ADDRESS
  
 <img width="820" height="276" alt="image" src="https://github.com/user-attachments/assets/9fe66560-adbb-4f10-b312-e2ed9f41f1d4" />

 STEP 5:- Make your Web App Cool

We've got a working database, EC2 instance, and now a basic web app running. HUGE!
But we still haven't actually connected our web app with our database. Wouldn't it be cool if we could update and see the changes of our database from our web app?
Let's build it!

Connect your EC2 instance to your Database

While still connected to your EC2 instance, navigate to the www folder (this is where we store all our files for our web app!).

 cd /var/www
 
Create a new sub-folder named inc.

mkdir inc

Whoops! Did you get another permission denied error?
Let's find out what's going on here. Who actually has permissions in this folder? 
Run the following command to find out:

ls -ld

Does the details of your folder look like this?

<img width="630" height="194" alt="image" src="https://github.com/user-attachments/assets/98c33a03-de67-4a8a-9861-c44f42104825" />

This means that the root user is the owner of this folder, and only the owner can edit this folder. When you access an EC2 instance using a key pair, you're by default logging in as an admin user (called ec2-user) which is not the root user!

Let's change the owner of this folder so you (ec2-user) can edit it.
Let's navigate back to where we were at the start:

cd ../../

Run the following command:
sudo chown ec2-user:ec2-user /var/www

Now let's see the details of that folder again...
ls -ld var/www

Let's try it again.
Navigate to your www folder

cd /var/www

Create a new sub-folder named inc.
mkdir inc
Woohoo! No errors! Navigate into your new inc folder
cd inc
Create a new file in the inc directory named dbinfo.inc
>dbinfo.inc
Cool! Now we have a blank file. We can edit our new file by calling nano
nano dbinfo.inc
<img width="720" height="524" alt="image" src="https://github.com/user-attachments/assets/358a7b6e-ea9b-4b5f-9ce5-78f6d660c615" />

dbinfo.inc is a settings file that stores the connection details our EC2 instance will need to connect to our Aurora database. To complete this file, we need our Aurora database endpoint.
Navigate to your Aurora database details in AWS and note the Endpoint of your Writer instance:
YOUR_WRITER_ENDPOINT
Copy and paste the following code to connect our EC2 instance to our Aurora database to the dbinfo.inc file.
If you've set up your user with a password other than n3xtw0rk, make sure to update DB_PASSWORD too.

<?php

define('DB_SERVER', 'YOUR_WRITER_ENDPOINT');
define('DB_USERNAME', 'admin');
define('DB_PASSWORD', 'n3xtw0rk');
define('DB_DATABASE', 'sample');
?>

<img width="1466" height="805" alt="image" src="https://github.com/user-attachments/assets/0c0e42a1-5542-45f9-82ef-0332fb70305e" />

Save and close the dbinfo.inc file by using Ctrl+S to save and then Ctrl+X to exit.

Upgrade your Web App
Navigate to your html folder:

cd /var/www/html

Create a new file in the html directory named SamplePage.php, and then edit the file by calling nano

>SamplePage.php
nano SamplePage.php

Copy and paste the following script into your SamplePage.php file:

<?php include "../inc/dbinfo.inc"; ?>
<html>
<body>
<h1>Sample page</h1>
<?php

  /* Connect to MySQL and select the database. */
  $connection = mysqli_connect(DB_SERVER, DB_USERNAME, DB_PASSWORD);

  if (mysqli_connect_errno()) echo "Failed to connect to MySQL: " . mysqli_connect_error();

  $database = mysqli_select_db($connection, DB_DATABASE);

  /* Ensure that the EMPLOYEES table exists. */
  VerifyEmployeesTable($connection, DB_DATABASE);

  /* If input fields are populated, add a row to the EMPLOYEES table. */
  $employee_name = htmlentities($_POST['NAME']);
  $employee_address = htmlentities($_POST['ADDRESS']);

  if (strlen($employee_name) || strlen($employee_address)) {
    AddEmployee($connection, $employee_name, $employee_address);
  }
?>

<!-- Input form -->
<form action="<?PHP echo $_SERVER['SCRIPT_NAME'] ?>" method="POST">
  <table border="0">
    <tr>
      <td>NAME</td>
      <td>ADDRESS</td>
    </tr>
    <tr>
      <td>
        <input type="text" name="NAME" maxlength="45" size="30" />
      </td>
      <td>
        <input type="text" name="ADDRESS" maxlength="90" size="60" />
      </td>
      <td>
        <input type="submit" value="Add Data" />
      </td>
    </tr>
  </table>
</form>

<!-- Display table data. -->
<table border="1" cellpadding="2" cellspacing="2">
  <tr>
    <td>ID</td>
    <td>NAME</td>
    <td>ADDRESS</td>
  </tr>

<?php

$result = mysqli_query($connection, "SELECT * FROM EMPLOYEES");

while($query_data = mysqli_fetch_row($result)) {
  echo "<tr>";
  echo "<td>",$query_data[0], "</td>",
       "<td>",$query_data[1], "</td>",
       "<td>",$query_data[2], "</td>";
  echo "</tr>";
}
?>

</table>

<!-- Clean up. -->
<?php

  mysqli_free_result($result);
  mysqli_close($connection);

?>

</body>
</html>


<?php

/* Add an employee to the table. */
function AddEmployee($connection, $name, $address) {
   $n = mysqli_real_escape_string($connection, $name);
   $a = mysqli_real_escape_string($connection, $address);

   $query = "INSERT INTO EMPLOYEES (NAME, ADDRESS) VALUES ('$n', '$a');";

   if(!mysqli_query($connection, $query)) echo("<p>Error adding employee data.</p>");
}

/* Check whether the table exists and, if not, create it. */
function VerifyEmployeesTable($connection, $dbName) {
  if(!TableExists("EMPLOYEES", $connection, $dbName))
  {
     $query = "CREATE TABLE EMPLOYEES (
         ID int(11) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
         NAME VARCHAR(45),
         ADDRESS VARCHAR(90)
       )";

     if(!mysqli_query($connection, $query)) echo("<p>Error creating table.</p>");
  }
}

/* Check for the existence of a table. */
function TableExists($tableName, $connection, $dbName) {
  $t = mysqli_real_escape_string($connection, $tableName);
  $d = mysqli_real_escape_string($connection, $dbName);

  $checktable = mysqli_query($connection,
      "SELECT TABLE_NAME FROM information_schema.TABLES WHERE TABLE_NAME = '$t' AND TABLE_SCHEMA = '$d'");

  if(mysqli_num_rows($checktable) > 0) return true;

  return false;
}
?>    

Save and close the SamplePage.php file by using Ctrl+S to save and then Ctrl+X to exit.

Verify that your web server successfully connects to your DB cluster by opening a web browser and browsing to
http://YOUR_EC2_ADDRESS/SamplePage.php

<img width="1166" height="424" alt="image" src="https://github.com/user-attachments/assets/d5737713-66c3-402b-96c3-85bfe87feddb" />

STEP 6:- Check that it worked!

In your browser, where your new web app is running, add some new data.

<img width="1089" height="403" alt="image" src="https://github.com/user-attachments/assets/908041e7-99c7-478a-8a81-14fe7a795508" />

Notice how your results show on the page? If you're wondering how that happens, go back and have a look at your SamplePage.php file. All the magic is there!

<img width="1101" height="425" alt="image" src="https://github.com/user-attachments/assets/a1b46a94-25fa-4fb2-9961-031ac20d12ee" />

But is this actually updating our Aurora database? 🤔 Let's check! 

Connect to your Database using MySQL CLI

To access your database, you're going to use MySQL.
Download the MySQL repository into your EC2 instance:

sudo yum install https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm -y

Install MySQL:
sudo yum install mysql-community-client -y

Connect to your Aurora MySQL Database:
mysql -h YOUR_WRITER_ENDPOINT -P 3306 -u admin -p

Make sure you replace:
YOUR_ENDPOINT with the Endpoint from your Aurora Writer instance
YOUR_PORT with the Port from your Aurora Writer instance; 3306
YOUR_AURORA_USERNAME with your Aurora username; admin
Enter in your Aurora password when prompted; n3xtw0rk

<img width="1251" height="369" alt="image" src="https://github.com/user-attachments/assets/24b78314-b8cc-493c-9a3c-58448750e1e9" />

Run SHOW DATABASES;
This shows you the databases that are in your MYSQL server.
Run USE sample;
sample is the name you gave your first database schema.
Run SHOW TABLES;
This shows you the tables in your sample schema.
If you're wondering where Employees came from, check your SamplePage.php file. It's all there! (Hint: this is also where you can change it if you're feeling creative)

<img width="976" height="350" alt="image" src="https://github.com/user-attachments/assets/b4f8778d-0adb-4fe9-aa0f-41b444547472" />

Run DESCRIBE EMPLOYEES;
This tells you how the Employees table is structured.

<img width="1053" height="413" alt="image" src="https://github.com/user-attachments/assets/e24d6c36-a9bc-447a-827a-15cb08b1e830" />

STEP 7:- Delete Your Resources

GUIDE LINK - https://learn.nextwork.org/projects/aws-databases-webapp?track=high


