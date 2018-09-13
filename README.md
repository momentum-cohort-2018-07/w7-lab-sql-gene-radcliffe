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
- Find the client for whom Mrs. Lupe Schowalter (the developer) has worked the greatest number of hours.
- List all client names with their project names (multiple rows for one client is fine).  Make sure that clients still show up even if they have no projects.
- Find all developers who have written no comments.

Unless otherwise specified, return all columns in the requested table (e.g. developers).

## Challenges

Try these queries if you have time:

- Find all developers with at least five comments.
- Find the developer who worked the fewest hours in January of 2015.
- Find all time entries which were created by developers who were not assigned to that time entry's project.
- Find all developers with no time put towards at least one of their assigned projects.
- Find all pairs of developers who are in two or more different groups together.

### Resources

- [DB Browser for SQLite](https://sqlitebrowser.org/)
- [Markdown](https://guides.github.com/features/mastering-markdown/)