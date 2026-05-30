# Lab 8

## Question 1
The partition key is user_id. 

## Question 2
The clustering columns are played_at and song_id. 

## Question 3
We created two different tables because Cassandra doesn't support joins. So, each table is specifically designed for one kind of
query pattern. The plays_by_user table is for lookups by user and the plays_by_song is for lookups by song. 

## Question 4
If you try to query plays_by_user by song_id only, Cassandra will error. This is becuase the song_id isn't the partition key, the 
user_id is. Without the partition key, Cassandra would have to, inefficiently, scan every partition across the entire table to find matching rows. 

## Question 5
Data duplication is common when using Cassandra because you make tables based off of the queries you want to make, not the data. 
Because of this, it often occurs that the same data is stored in multiple tables just so that each query can be answered without 
needing to use joins. 

## Question 6
First query: 
 user_id | played_at                       | song_id | artist        | device | title
---------+---------------------------------+---------+---------------+--------+-----------------
      u1 | 2026-05-01 10:10:00.000000+0000 |      s3 | Billie Eilish | laptop |         bad guy
      u1 | 2026-05-01 10:05:00.000000+0000 |      s2 |    The Weeknd | iphone | Blinding Lights
      u1 | 2026-05-01 10:00:00.000000+0000 |      s1 |  Taylor Swift | iphone |       Anti-Hero

Second query: 
 song_id | played_at                       | user_id | artist       | device  | title
---------+---------------------------------+---------+--------------+---------+-----------
      s1 | 2026-05-01 12:00:00.000000+0000 |      u3 | Taylor Swift |  laptop | Anti-Hero
      s1 | 2026-05-01 11:00:00.000000+0000 |      u2 | Taylor Swift | android | Anti-Hero
      s1 | 2026-05-01 10:00:00.000000+0000 |      u1 | Taylor Swift |  iphone | Anti-Hero