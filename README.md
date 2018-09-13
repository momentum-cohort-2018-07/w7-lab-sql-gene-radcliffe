# SQL Queries!

## Description

For each of the questions below, add the following information to a Markdown file in your homework repo:

- The original question text
- Your final SQL query (which you must have run and validated on the included database)
- The number of results returned (if more than one)
- The specific result returned (if a single record is returned)

## The queries

- Find all time entries.

~~~~sql
SELECT *
FROM time_entries
~~~~
- Find the developer who joined most recently.
~~~~sql
SELECT name AS developer, email, joined_on AS joined
FROM developers
ORDER BY(joined_on) DESC
LIMIT 5
~~~~
###results:
~~~~sql
5 rows returned in 2ms from: SELECT name AS developer, email, joined_on AS joined
FROM developers
ORDER BY(joined_on) DESC
LIMIT 5
~~~~~
developer | email | joined
----------|-------|------
"Dr. Danielle McLaughlin" | "shakira.carter@kohler.org" | "2015-07-10"
"Orlo Renner" | "aisha@cole.net" | "2015-05-10"
"Miss Cruz Johnston" | "fredrick.barton@schiller.name"|	"2015-05-08"
"Mr. Rogers Greenholt" | "ismael@sanford.info"|	"2015-04-29"
"Joany Hodkiewicz" |	"ardith@kiehnsauer.biz" | "2015-04-02"

- Find the number of projects for each client.
~~~~sql
SELECT clients.id, clients.name, COUNT(projects.client_id) AS number_of_projects
FROM clients
JOIN projects ON clients.id = projects.client_id
WHERE clients.id = projects.client_id
GROUP BY (projects.client_id)

~~~~
id | client| number_of_projects
---|-------|-------------------
"1" | "West Group" | "3"
"2" | "Mohr Inc" | "3"
"3" | "Jast LLC" | "3"
"4" | "Carter, Farrell and Goodwin" | "3"
"6" | "Ortiz, Gislason and Rutherford" | "6"
"7" | "Zieme-Ortiz" | "3"
"8" | "Eichmann, Altenwerth and Morar" | "3"
"9" | "Kuhic-Bartoletti" | "3"
"10" | "Goodwin Group" | "3"

~~~~sql
9 rows returned in 2ms from: SELECT clients.id, clients.name, COUNT(projects.client_id) AS number_of_projects
FROM clients
JOIN projects ON clients.id = projects.client_id
WHERE clients.id = projects.client_id
GROUP BY (projects.client_id)
~~~~

- Find all time entries, and show each one's client name next to it.
~~~~sql
SELECT clients.id, clients.name, time_entries.worked_on AS time_entry
FROM clients
LEFT JOIN projects ON projects.client_id = clients.id
LEFT JOIN time_entries ON time_entries.project_id = projects.id

~~~~
~~~~sql
503 rows returned in 6ms from: SELECT clients.id, clients.name, time_entries.worked_on AS time_entry
FROM clients
LEFT JOIN projects ON projects.client_id = clients.id
LEFT JOIN time_entries ON time_entries.project_id = projects.id
~~~~

- Find all developers in the "Ohio sheep" group.
~~~~sql
SELECT  developers.id, developers.name, developers.email
FROM developers
JOIN group_assignments ON group_assignments.developer_id = developers.id
JOIN groups ON groups.id = group_assignments.group_id
WHERE groups.name ="Ohio sheep";
~~~~
id | deveoper| email
---|-------|-------------------
"1" | "Bruce Wisoky Jr." | "mara@hayeswalsh.org"
"26" | "Eli Wunsch MD" | "joaquin_cain@romaguera.name"
"47" | "Reyes Vandervort IV" | "delores@murphymayer.net"

~~~~sql
3 rows returned in 2ms from: SELECT  developers.id, developers.name, developers.email
FROM developers
JOIN group_assignments ON group_assignments.developer_id = developers.id
JOIN groups ON groups.id = group_assignments.group_id
WHERE groups.name ="Ohio sheep";
~~~~
- Find the total number of hours worked for each client.
~~~~sql
SELECT clients.id, clients.name, time_entries.duration
FROM clients
LEFT JOIN projects ON projects.client_id = clients.id
LEFT JOIN time_entries ON time_entries.project_id = projects.id
GROUP BY(clients.id)
~~~~
id | client| duration
---|-------|-------------------
"1" | "West Group" | "8"
"2" | "Mohr Inc" | "7"
"3" | "Jast LLC" | "7"
"4" | "Carter, Farrell and Goodwin" | "7"
"5" | "Abbott-Stroman" | 
"6" | "Ortiz, Gislason and Rutherford" | "7"
"7" | "Zieme-Ortiz" | "7"
"8" | "Eichmann, Altenwerth and Morar" | "7"
"9" | "Kuhic-Bartoletti" | "8"
"10" | "Goodwin Group" | "7"
"13" | "Momentum Learning" |  null
"15" | "Carolina Code School" | null

~~~~sql
12 rows returned in 5ms from: SELECT clients.id, clients.name, time_entries.duration
FROM clients
LEFT JOIN projects ON projects.client_id = clients.id
LEFT JOIN time_entries ON time_entries.project_id = projects.id
GROUP BY(clients.id)
~~~~

- Find the client for whom Mrs. Lupe Schowalter (the developer) has worked the greatest number of hours.

~~~~sql
SELECT clients.id, clients.name AS client, SUM(time_entries.duration) AS total_hours
FROM clients
LEFT JOIN projects ON projects.client_id = clients.id
LEFT JOIN time_entries ON time_entries.project_id = projects.id
LEFT JOIN developers ON developers.id = time_entries.developer_id
WHERE developers.name = "Mrs. Lupe Schowalter"
GROUP BY (projects.id)
ORDER BY total_hours DESC
~~~~

id | client| total_hours
---|-------|-------------------
"9" | "Kuhic-Bartoletti" | "7"
"1" | "West Group" | "6"
"2" | "Mohr Inc" | "4"
"4" | "Carter, Farrell and Goodwin" | "4"
"9" | "Kuhic-Bartoletti" | "4"
"3" | "Jast LLC" | "3"
"10" | "Goodwin Group" | "3"
"6" | "Ortiz, Gislason and Rutherford" | "1"
"6" | "Ortiz, Gislason and Rutherford" | "0"

~~~~sql
9 rows returned in 6ms from: SELECT clients.id, clients.name AS client, SUM(time_entries.duration) AS total_hours
FROM clients
LEFT JOIN projects ON projects.client_id = clients.id
LEFT JOIN time_entries ON time_entries.project_id = projects.id
LEFT JOIN developers ON developers.id = time_entries.developer_id
WHERE developers.name = "Mrs. Lupe Schowalter"
GROUP BY (projects.id)
ORDER BY total_hours DESC
~~~~

- List all client names with their project names (multiple rows for one client is fine).  Make sure that clients still show up even if they have no projects.

~~~~sql
SELECT clients.id, clients.name AS client, projects.name AS project_name
FROM clients
LEFT JOIN projects ON projects.client_id = clients.id
LEFT JOIN time_entries ON time_entries.project_id = projects.id
LEFT JOIN developers ON developers.id = time_entries.developer_id
GROUP BY (projects.name)
ORDER BY (clients.id) DESC

~~~~
id | client| project_name
---|-------|-------------------
"15" | "Carolina Code School" | 
"10" | "Goodwin Group" | "Incredible Granite Table"
"10" | "Goodwin Group" | "Intelligent Steel Shirt"
"10" | "Goodwin Group" | "Practical Rubber Shoes"
"9" | "Kuhic-Bartoletti" | "Awesome Plastic Computer"
"9" | "Kuhic-Bartoletti" | "Intelligent Plastic Shirt"
"9" | "Kuhic-Bartoletti" | "Intelligent Rubber Gloves"
"8" | "Eichmann, Altenwerth and Morar" | "Gorgeous Concrete Hat"
"8" | "Eichmann, Altenwerth and Morar" | "Gorgeous Cotton Hat"
"8" | "Eichmann, Altenwerth and Morar" | "Intelligent Wooden Hat"
"7" | "Zieme-Ortiz" | "Fantastic Rubber Chair"
"7" | "Zieme-Ortiz" | "Fantastic Wooden Shirt"
"7" | "Zieme-Ortiz" | "Practical Concrete Shoes"
"6" | "Ortiz, Gislason and Rutherford" | "Awesome Rubber Computer"
"6" | "Ortiz, Gislason and Rutherford" | "Gorgeous Granite Car"
"6" | "Ortiz, Gislason and Rutherford" | "Intelligent Cotton Shirt"
"6" | "Ortiz, Gislason and Rutherford" | "Practical Wooden Shirt"
"6" | "Ortiz, Gislason and Rutherford" | "Sleek Granite Car"
"6" | "Ortiz, Gislason and Rutherford" | "Sleek Plastic Shoes"
"4" | "Carter, Farrell and Goodwin" | "Awesome Rubber Pants"
"4" | "Carter, Farrell and Goodwin" | "Gorgeous Wooden Table"
"4" | "Carter, Farrell and Goodwin" | "Rustic Granite Shirt"
"3" | "Jast LLC" | "Awesome Rubber Shirt"
"3" | "Jast LLC" | "Ergonomic Rubber Car"
"3" | "Jast LLC" | "Fantastic Cotton Computer"
"2" | "Mohr Inc" | "Awesome Cotton Hat"
"2" | "Mohr Inc" | "Incredible Concrete Shirt"
"2" | "Mohr Inc" | "Rustic Steel Chair"
"1" | "West Group" | "Ergonomic Plastic Shoes"
"1" | "West Group" | "Gorgeous Steel Hat"
"1" | "West Group" | "Rustic Concrete Table"
- Find all developers who have written no comments.

~~~~sql
SELECT developers.id, developers.name, comments.comment
FROM developers
LEFT JOIN comments ON comments.developer_id = developers.id
WHERE comments.comment is NULL

~~~~
id | client| comment
---|-------|-------------------
"7" | "Ms. Tremayne Kuhn" | null 
"9" | "Jaylin Auer" | null
"12" | "Myles Keebler" | null
"15" | "Mr. Rogers Greenholt" | null
"21" | "Jayne Brakus DDS" | null
"23" | "Orlo Renner" | null
"27" | "Hyman Borer" | null
"30" | "Verda Zieme" | null
"34" | "Dr. Danielle McLaughlin" | null
"37" | "Joanie Krajcik PhD" | null
"40" | "Reese Dooley" | null
"44" | "Amy Dickinson" | null
"49" | "Miss Brendan Zboncak" | null

~~~~sql
13 rows returned in 3ms from: SELECT developers.id, developers.name, comments.comment
FROM developers
LEFT JOIN comments ON comments.developer_id = developers.id
WHERE comments.comment is NULL
~~~~

Unless otherwise specified, return all columns in the requested table (e.g. developers).

## Challenges

Try these queries if you have time:

- Find all developers with at least five comments.
~~~~sql

SELECT developers.id, developers.name, COUNT(comments.comment) AS number_of_comments
FROM developers
LEFT JOIN comments ON comments.developer_id = developers.id
WHERE comments.comment NOT NULL
GROUP BY (developers.id)
HAVING COUNT (developers.id) 
ORDER BY number_of_comments DESC
LIMIT 5

~~~~
id | developer| number_of_comments
---|-------|-----------------------
"45" | "Joelle Hermann" | "5"
"47" | "Reyes Vandervort IV" | "4"
"14" | "Reagan Schneider" | "3"
"17" | "Ms. Angelo Ziemann" | "3"
"35" | "Mrs. Danny Ebert" | "3"

~~~~sql

5 rows returned in 1ms from: SELECT developers.id, developers.name, COUNT(comments.comment) AS number_of_comments
FROM developers
LEFT JOIN comments ON comments.developer_id = developers.id
WHERE comments.comment NOT NULL
GROUP BY (developers.id)
HAVING COUNT (developers.id)
ORDER BY number_of_comments DESC
LIMIT 5
~~~~

- Find the developer who worked the fewest hours in January of 2015.
~~~~sql

SELECT developers.id, developers.name, SUM(time_entries.duration) AS total_hours
FROM developers
LEFT JOIN time_entries ON time_entries.developer_id = developers.id
WHERE time_entries.worked_on BETWEEN '2015-01-01' AND '2015-01-31'
GROUP BY developers.id 
ORDER BY total_hours ASC
LIMIT 3
~~~~
id | developer|total_hours
---|----------|-----------------------
"7" | "Ms. Tremayne Kuhn" | "0"
"10" | "Delphia Abshire IV" | "1"
"12" | "Myles Keebler" | "1"

~~~~sql
3 rows returned in 4ms from: SELECT developers.id, developers.name, SUM(time_entries.duration) AS total_hours
FROM developers
LEFT JOIN time_entries ON time_entries.developer_id = developers.id
WHERE time_entries.worked_on BETWEEN '2015-01-01' AND '2015-01-31'
GROUP BY developers.id
ORDER BY total_hours ASC
LIMIT 3
~~~~
- Find all time entries which were created by developers who were not assigned to that time entry's project.
- Find all developers with no time put towards at least one of their assigned projects.
- Find all pairs of developers who are in two or more different groups together.

### Resources

- [DB Browser for SQLite](https://sqlitebrowser.org/)
- [Markdown](https://guides.github.com/features/mastering-markdown/)