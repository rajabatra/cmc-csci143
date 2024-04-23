# Final Project

You will create your own database-backed webpage from scratch using the Instagram architecture.
The default project will be a Twitter clone webpage, but you are free to create any type of webpage.

**Learning Objectives:**
1. Design your own database schema
1. Integrate knowledge from all parts of the course together
1. Be able to easily create future websites (e.g. to start your own business)
1. Have a production-ready project you can show off to future employers
1. *If you took CS40:* see how much you've grown as a programmer

**Due date:** During your final exam timeslot (May 6-10)

<img src=crud.jpg width=400px />

## Background

[CS40](https://github.com/mikeizbicki/cmc-csci040) is CMC's intro programming course.
The [final project](https://github.com/mikeizbicki/cmc-csci040/tree/2022fall/project_05) was to create a Twitter clone.
The website used the following technologies:
1. Python
1. Flask
1. HTML/CSS
1. Jinja2
1. SQLite3

In CS40, we covered everything the first four of those technologies in detail, but we did not cover much SQL.
Now that we have covered SQL extensively, you are in a position to write a much more capable Twitter clone webpage.

The purpose of this assignment is to integrate your SQL knowledge (and docker knowledge) from this course with this previous material.

## Required Tasks

The project is worth 32 points total.
There are 8 required tasks, each worth 4 points.

**Task 1:** The project structure should follow the structure of the [flask-on-docker homework](https://github.com/mikeizbicki/cmc-csci143/tree/2024spring/topic_03_instagram_tech_stack#homework).  In particular:
1. There should be a development `docker-compose.yml` file with a web service and postgres service.
1. There should be a production `docker-compose.yml` file with a web service, postgres service, and nginx service.
1. You should be able to start your web page by running `docker-compose up` with either of these yml files.
1. You must have appropriate volumes defined so that bringing the containers down does not delete the database.

> **Hint:**
> I recommend starting this project by copying your `flask-on-docker` homework directory rather than starting completely from scratch.

**Task 2:** You must design a database.
1. The schema file should
    1. be located in `services/postgres/schema.sql`.
    1. be loaded automatically into the database on startup
    1. contain at least 3 tables, each of which has:
        1. a primary key
        1. at least 2 columns with appropriate types and constraints
        1. appropriate indexes

        > **Hint:**
        > The easiest database to build would be based on the `pg_normalized` database, with the `tweets`, `users`, and `urls` tables.

        > **Important:**
        > All indexes that you will need for making your routes fast must be included in the `schema.sql` file.

1. You should have a script that loads test data into the database.  After running the script:
    1. Every table should have at least 1,000,000 rows
    1. At least one table must have 10,000,000 rows

    > **HINT:**
    > Write your script so that it takes a parameter of the number of rows to add to the tables.
    > As you're developing, you should use a small number of rows (say 100) so that all of your SQL queries will always run fast.
    > Then once you have a basic prototype working, rerun the script with the full amount of data.

**Task 3-8:** The remaining 6 tasks each correspond to an individual route on your webpage.

> **Note:**
> The requirements listed below are for the default option of making a Twitter clone.
> If you would like to make a different type of website, let me know, and we can work out an alternative set of requirements.

> **Note:**
> The requirements for tasks 3-7 are based off of the routes for the CS40 Twitter clone assignment.
> If you took CS40, I encourage you to try to reuse your code from that assignment.
> You will have to make some modifications to it in order for it to work with your new database schema and the much larger test datasets.
> 
> The following sequence of lecture recordings from CS40 provides working code for several of these routes.
> You might find them useful to review:
> 1. Flask intro: <https://www.youtube.com/watch?v=YdE-Rmwld5I>
> 1. Using python+SQL: <https://www.youtube.com/watch?v=7SYIBTIaoU0>
> 1. (\*) Creating a login system: <https://www.youtube.com/watch?v=_LX6lEBSaLE>
> 1. SQL injection: <https://www.youtube.com/watch?v=wUtCclQnXDc>
> The full course playlist is also available at <https://www.youtube.com/playlist?list=PLSNWQVdrBwobVmLdv8zKEO1TVJHZBjPxU>

*Route `/`*
1. a link to this page should always be visible in your menu, whether the user is logged in or not logged in
1. this route displays all the messages in the system
1. the messages should be ordered chronologically with the most recent message at the top
1. each message should include the user account that created it, the time of creation, and the message contents
1. you should display only the most recent 20 messages, and there should be links at the bottom that will take you to previous messages

    > **Hint:**
    > The main challenge of this route will be writing a good SQL query and corresponding index to achieve the requirements above.


*Route `/login`*
1. a link to this page should only be visible in your menu if the user is not logged in
1. this route will present a form for the user to enter their username/password;
   the password box must not display the user's password as they are typing it in

    > **HINT:**
    > Don't use `type=text` on your `input` tag.
    > Figure out the right type for a password box.

1. you must display appropriate error messages if the user enters an incorrect username/password
1. after a user successfully logs in, you must automatically redirect them to the homepage

*Route `/logout`*
1. a link to this page should only be visible if the user is logged in
1. this route will delete the cookies that the log in form creates, logging the user out

    > **HINT:**
    > We didn't cover how to delete cookies in class,
    > but you can find an explanation in [this stackoverflow post](https://stackoverflow.com/questions/14386304/flask-how-to-remove-cookies).

*Route `/create_account`*
1. a link to this page should only be visible in your menu if the user is not logged in
1. if the user attempts to create an account that already exists, they should get an appropriate error message
1. the user should be prompted to enter their password twice; if the passwords do not match, they should get an appropriate error message

*Route `/create_message`*
1. a link to this page should only be visible in your menu if the user is logged in
1. the user must be able to enter whatever message body they want,
   but you will also need to store the user id of the user that created a message and the time the message was created
1. you will only get credit for this route if the message correctly shows up on the home route after creation

*Route `/search`* **(new)**
1. a link to this page should always be visible
1. the user should be given an input field to enter a search query
1. after the search query has been entered, a FTS should be performed over the messages, and the results returned in a format similar to the home (`/`) route
1. if many messages match search pattern, then the resulting messages must have next/previous buttons to traverse the pages

## Grading

You will demo your webpage to me during your final exam.

In the final exam:

1. You will bring a printout of your SQL schema.
1. You will have your web server already running at the start of the exam.
1. You will tell me what port it is running on, and I will connect to the webserver using my computer.
1. I will traverse your webpages, verifying all of the functionality.
1. In order to implement the routes as specified, you will have to implement:
    1. a query without a JOIN
    1. a query with a JOIN
    1. a full text search query

    I will select three of these queries for you, and then the final exam will proceed as it did for the graduating students.
