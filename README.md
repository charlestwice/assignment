# part 1
##sql code to create table and import data

create table restaurants(
   id integer primary key,
   name blob,
   category blob,
   price_tier blob,
   neighborhood blob,
   opening_hours blob,
   average_rating blob,
    good_for_kids blob
   );

.mode csv
.import /Users/charleschang/desktop/restaurants.csv restaurants

.mode csv
.import /Users/charleschang/desktop/id.csv reviews

###1.	Find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example).
select name from restaurants where neighborhood= "columbus circle" and price_tier="cheap";
###2.	Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.
select name from restaurants where category="russian" and average_rating>=3 order by average_rating desc;
###3.	Find all restaurants that are open now (see hint below).
select name, opening_hours from restaurants where opening_hours<=strftime('%H:%M', 'now');
###4.	Leave a review for a restaurant (pick any restaurant as an example).
update reviews set review="everything was awful" where id=1;
###5.	Delete all restaurants that are not good for kids.
delete from restaurants where good_for_kids="0";
###6.	Find the number of restaurants in each NYC neighborhood.
select neighborhood, count(id) from restaurants group by neighborhood;

#part2
####sql code to create table and import data

CREATE TABLE users(
   id INTEGER PRIMARY KEY,
   email blob, 
   password blob, 
   handle blob
   );
sqlite> create table posts(
   id INTEGER PRIMARY KEY,
   messages_sender integer,
   messages blob, 
   messages_receiver integer,
   messages_time blob,
   stories blob,
   stories_time blob
   );
.mode csv
.import /Users/charleschang/desktop/users.csv users

.mode csv
.import /Users/charleschang/desktop/posts.csv posts

###1.	Register a new User.
insert into users(email, password, handle) values ("2000chang@gmail.com","0975529020","charlestwice");
###2.	Create a new Message sent by a particular User to a particular User (pick any two Users for example).
insert into posts(messages_sender,messages, messages_receiver, messages_time) values ("456","HI THERE!","789",”2021-03-17 11:22:36”);
###3.	Create a new Story by a particular User (pick any User for example).
insert into posts(messages_sender,stories,stories_time) values ("123","wow","2021-03-17 11:22:36”);
###4.	Show the 10 most recent visible Messages and Stories, in order of recency.
select messages, stories from posts order by ROUND((JULIANDAY('now') - JULIANDAY(messages_time))* 24) and round((julianday('now')-julianday(stories_time))*24) desc limit 10;
###5.	Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.
select messages_sender,messages, messages_receiver, messages_time,((JULIANDAY('now') - julianday(messages_time)) * 24) as time from posts order by time asc limit 10;
###6.	Make all Stories that are more than 24 hours old invisible.
Delete from posts where round ((JULIANDAY('now') - julianday(messages_time)) * 24)>=24;
###7.	Show all invisible Messages and Stories, in order of recency.
select messages, stories, stories_time  from posts where ROUND((JULIANDAY('now') - JULIANDAY(stories_time)) * 24)>24 order by stories_time desc;
###8.	Show the number of posts by each User.
select messages_sender, count(*) from posts group by messages_sender order by messages_sender asc;
###9.	Show the post text and email address of all posts and the User who made them within the last 24 hours.
select posts.stories, users.email from users inner join posts on users.id=posts.messages_sender where ROUND((JULIANDAY('now') - JULIANDAY(stories_time)) * 24)<24;
###10.	Show the email addresses of all Users who have not posted anything yet.
select users.email from users left join posts on users.id=posts.messages_sender where posts.stories_time IS NULL;




