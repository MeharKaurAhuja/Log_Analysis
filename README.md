# Log_Analysis
- Udacity Project3
- Log-Analysis is the third project of Udacity's [Full Stack Web Developer Nanodegree](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004)

### Project Overview
The task is that we need to create a reporting tool which prints the reports (in plain text)
on the basis of data in the database. This reporting tool is a Python program
that uses the psycopg2 module connecting to the database.

### Questions that we need to Answer are:
1. **What are the most popular three articles of all time?** Which articles have been
accessed the most? Present this information as a sorted list with the most popular
article at the top.

Example:
- "Princess Shellfish Marries Prince Handsome" — 1201 views
- "Baltimore Ravens Defeat Rhode Island Shoggoths" — 915 views
- "Political Scandal Ends In Political Scandal" — 553 views

2. **Who are the most popular article authors of all time?** That is, when you sum up
all of the articles each author has written, which authors get the most page views?
Present this as a sorted list with the most popular author at the top.

Example:
- Ursula La Multa — 2304 views
- Rudolf von Treppenwitz — 1985 views
- Markoff Chaney — 1723 views

3. **On which days did more than 1% of requests lead to errors?**  The log table
includes a column status that indicates the HTTP status code that the news site sent
to the user's browser.

Example:
- July 29, 2016 — 2.5% errors

## How to Run?
This project is run in a virtual machine that is created using Vagrant. Hence, there are some steps
to get set up:

#### PreRequisites:
  * [Python3](https://www.python.org/)
  * [Vagrant](https://www.vagrantup.com/)
  * [VirtualBox](https://www.virtualbox.org/)

#### Install all the softwares and files:
1. Install [Vagrant](https://www.vagrantup.com/)
2. Install [VirtualBox](https://www.virtualbox.org/)
3. The vagrant setup files are being downloaded from [Udacity's Github](https://github.com/udacity/fullstack-nanodegree-vm)
These setup files are being configured by the virtual machine and all the tools that are needed to run this project are installed.
Now,
1. Firstly, download the database setup: [data](https://d17h27t6h515a5.cloudfront.net/topher/2016/August/57b5f748_newsdata/newsdata.zip)
2. Then, unzip the data to get the newsdata.sql file.
3. Then add the newsdata.sql file into the vagrant directory.
4. Download this project:
[log analysis] (https://d17h27t6h515a5.cloudfront.net/topher/2016/August/57b5f748_newsdata/newsdata.zip) from here.
5. Atlast, upzip as needed and copy all files into the vagrant directory into a folder called news_analysis.

#### Launching the Virtual Machine:
1. Open the Terminal and open the project folders as stated above.
2. Then, cd into the vagrant directory ```vagrant\```.
3. Now, run ``` $ vagrant up ``` to build the VM for the first time.
4. Once it is built, then run the ```   $ vagrant ssh ``` to connect.
5. Now, cd into the correct project directory: ``` cd /vagrant/news_analysis ```

#### Load the data into the database:
1. Load the data using the following command: ``` psql -d news -f newsdata.sql ```
The database includes three tables:
* The authors table includes information about the authors of articles.
* The articles table includes the articles themselves.
* The log table includes one entry for each time a user has accessed the site.
2. Use ```psql -d news``` to connect to database.
3. Create view err_view using:
```
  create view err_view as select date(time),round(100.0*sum(case log.status when '200 OK'
  then 0 else 1 end)/count(log.status),2) as "Percent Error" from log group by date(time)
  order by "Percent Error" desc;
```

## Run The Project
1. To connect you should have vagrant up.
2. If you have done already, then cd into the correct project directory: ``` cd /vagrant/news_analysis ```
3. At last, run ``` $ python news_analysis.py ```

Generating this information will take several seconds, but will now start loading.

## Expected Output is:
  The result of the database would be:

    a) TOP THREE ARTICLES BY PAGE VIEWS:

    Candidate is jerk, alleges rival -- 338647 views
    Bears love berries, alleges bear -- 253801 views
    Bad things gone, say good people -- 170098 views

    b) TOP THREE AUTHORS BY VIEWS:

    Ursula La Multa -- 507594 views
    Rudolf von Treppenwitz -- 423457 views
    Anonymous Contributor -- 170098 views

    c) DAYS WITH MORE THAN 1% ERRORS:

    2016-07-17 -- 2.26 % errors
