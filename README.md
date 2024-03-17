# Case Description

SQL is often just used to fetch raw data from data storage that is later transferred to other software for preprocessing. But this programming language can do more than select data from a table. Sometimes, aggregating or preprocessing data in SQL directly might be easier or more beneficial—precisely the purpose of this project. This Extracting User Journey Data with SQL project centers around creating a customer journey data extract as the starting point for later analysis. Here, “user journey” refers to the steps each user goes through while exploring the product or platform before purchasing. The context is an online subscription-based company offering monthly, quarterly, and annual subscription plans. We’ll use our own 365 platform as the basis for the data. Using only SQL, you must consider all users that purchased a plan and aggregate all visited pages into a big sequence or string. (This user journey will be analyzed as a different project)

# Project requirements

For this project, you’ll need MySQL Workbench 8.0 or later.

# Project content

- 2 Project files
- Part 1: Extracting the Data

# Project files

For this Extracting User Journey Data with SQL project , you’re provided with an SQL file User_Journey_Database.sql to generate the database you’ll use to extract customer journey data. The steps in generating it are straightforward. First, open your local connection in MySQL. Then, open the SQL file from within this connection and run all commands in that file, creating a new schema containing all the relevant tables. This may take a while (1-10 min), so just be patient and don't restart MySQL during that time.

All this data has been stripped of personally identifiable information (PII). But you may find some test user records in the database that must be excluded from the queries.

Now, let’s consider the structure and contents of the database.

The database you’ll work with consists of three tables: front_interactions , student_purchases , and front_visitors.

The front_interactions  table records all visitor activity on the company’s front page, including visiting specific pages, clicks, and other interactions on said pages. The table consists of the following six fields or columns:

- visitor_id  – (int) the ID number of the visitor
- session_id  – (int) the session number during which the interaction took place
- event_source_url  – (string) the URL of the page on which the given event took place
- event_destination_url   – (string) the URL of the page when the event was completed/processed (for interactions during which the user stays on the same page, this is the same as source URL)
- event_date   – (datetime) the exact timestamp of the event/interaction
- event_name   – (string) an internal name of the event
- This table records all events on the front pages—from scrolling to clicking on buttons. The significant aspect regarding this project is that it also records the source and destination URLs for every event. 

These are the same for such interactions as scrolling or clicking on a form field. But when a visitor clicks on a page link, these two URLs would differ since they moved to a different page. This is what you’ll focus on for this project since we are interested in the sequence of pages in the visitor’s journey.

The next table is student_purchases  which contains records of user payments and the type of product they purchased. This includes all payments—even if they are subsequent recurring payments for the same subscription. Its columns contain the following:

- user_id   – (int) the ID of the user, different from the visitor_id
- purchase_id   – (int) the ID of the purchase
- purchase_type   – (int) the type of subscription purchased (0=monthly, 1=quarterly, 2=annual)
- purchase_price   – (decimal) the price the user paid in dollars
- date_purchased   – (datetime) the exact datetime of the purchase

Notice that since the person needs to have purchased a product to be in this table and has an account, they are no longer considered a visitor but a user. It’s also noteworthy that the purchase price in this table can be used as an indicator for test users. If a user has purchased a product at $0, they’re probably just a test one.

The default sorting for this table is based on date_purchased, from old to new.

The final table front_visitors  is the link between front_interactions and student_purchases. There are only two columns in this table:

- visitor_id   – (int) the ID of the visitor—each record has this field filled in
- user_id   – (int) the ID of the user corresponding to this visitor—many NULL values here because many visitors never made an account and so were never assigned a user_id
- From this table, you can determine which user corresponds to which visitor. Just bear in mind that most visitors have never created an account and are not considered users.

These are the general guidelines concerning the database. You can investigate all these specifics in SQL by yourself. (Remember that familiarization with the data you’re working with is an essential and often overlooked first step.)

Finally, to help when coding, you can download an additional file URL_Aliases.xlsx—a reference list with all the URLs you need and suggested aliases for the pages—because you presumably won’t be familiar with all the pages in the dataset.  
