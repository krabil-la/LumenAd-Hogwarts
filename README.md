# LumenAd-Hogwarts
A custom app for distributing points via slash commands in Slack

# Introduction 
This repo will show you how to set up your very own House Cup in Slack.  You too can give points for bravery, detract points for not liking someone and even have your own house cup ceremony!

# What you'll need
* An AWS (Amazon Web Services) Account 
* A Slack account with enough priviledges to create custom Slash commands 
* Some familiarity with Python 
* Courage 

# The Database
First You'll need to login to your AWS account or create an account first.  If you don't have an account it should be noted that you have to provide a credit card to create an account.  Even though most of this will fall within the Free Tier usage you should be aware that you can accrue charges if something is misconfigured.  I take no responsibility for unexpected AWS bills.  After the Free Tier is over the charges are still pretty small.  LumenAd has over 60 employees registered and the app gets tons of use, but the monthly bill for the service and database is about $2.

After you're logged in you're ready to create a DynamoDB table.  I chose DynamoDB to make development of new features easier and because I wanted to learn the technology for another project.  If you're more familiar with a different database you can easily switch it out here. I recommend staying within the AWS environment for this step as Lambda functions connect very easily to AWS databases. 

The database configuraton options here are pretty varied, but I recommend using the slack username as your Primary Key accepting the default settings.  If you want to get fancy you can add a Sort Key and Secondary Indexes to improve query efficiency and speed.  However I only recommend doing this if you both know what you're doing and plan on having tons of users.  If you have < 1000 users I wouldn't worry about it. 

Once you have your table created you can begin adding items.  You can get very creative with the attributes of your users, but for this tutorial I recommend 5: 
* `name` - String - the username provided by slack.  You can include the @ if you want, but I've chosen to parse that out. 
* `full_name` - String - The full name of the user.  This one isn't completely necessary, but it helps make the responses readable. 
* `house` - String - the house the user belongs to:  Gryffindor, Slytherin, Ravenclaw or Hufflepuff
* `points` - Float/Decimal - the number of points the user has. Everyone will start with 0
* `enabled` - Boolean -  a special flag so you can disable users without having to shut the whole app down. 

You may be wondering where to set these attributes in DynamoDB.  The answer is nowhere.  DynamoDB is a NoSQL database meaning it's unstructured.  For the uninitiated that means you can have highly varied data all in the same table.  In our example you could have a row with 3 attributes and a row with 30 attributes so long as they both have valid Primary Keys.  The easiest way to set this up is to store the data of your members with the attributes you want filled out then upload the data to DynamoDB.   
The one part of this whole process I haven't come with a good solution to is putting everyone in houses.  When I did this I had to go around to everyone in the company and ask what house they'd like to be in and keep track in a .csv file.  This was kind of a pain, but there's not really any other way to do it.  You could have people fill out their own houses, but I had a hard time just getting everyone to come up with a house, let alone write it down.  Either way you'll need a default house for everyone who can't decide. I chose Hufflepuff as the default because of a quote from the books: <em>"Good Hufflepuff, she took the rest and taught them all she knew"</em>
TODO: provide sample code for batch upload in Python

# The Code
This part is pretty straightforward.  Navigate to AWS Lambda and select `Create Function`.  The one catch here is to make sure you use the same region as your database to make connecting easier.  You can use the code I've provided with this repository and 
