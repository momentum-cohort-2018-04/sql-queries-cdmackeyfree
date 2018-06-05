- Find all time entries.
SQL:
    SELECT *
    FROM time_entries;
Results:
    500 rows returned 

- Find the developer who joined most recently.
SQL:
    SELECT name, MAX(updated_at)
    FROM developers;
Results:
    Mr. Leopold Carter, 2015-07-14 16:15:19.600858

- Find the number of projects for each client.
SQL: 
    SELECT *, COUNT(projects.id)
    FROM clients
    LEFT JOIN projects
    ON clients.id = projects.client_id
    GROUP BY projects.client_id;
Results:
    10 rows returned

- Find all time entries, and show each one's client name next to it.
SQL:
    SELECT clients.name
    FROM time_entries
    LEFT JOIN projects
    ON projects.id= time_entries.project_id
    LEFT JOIN clients
    ON clients.id = projects.client_id;
Results:
    500 rows returned in 17ms 

- Find all developers in the "Ohio sheep" group.
SQL:
    SELECT developers.name, group_id
    FROM developers
    LEFT JOIN group_assignments
    ON developers.id = group_assignments.developer_id
    WHERE group_assignments.group_id = 3;
Results:
    3 rows returned in 7ms
    <!-- tried to get this one to work with the actual text "Ohio Sheep" and got hung up in syntax errors, so scaled it back to using the group id, assuming that the person querying would know. My best try was:
    SELECT developers.name, group_id
    FROM developers
    LEFT JOIN group_assignments
    ON developers.id = group_assignments.developer_id
    LEFT JOIN groups
    ON group_assignments.group_id = groups.id
    WHERE groups.name = "Ohio Sheep";-->

- Find the total number of hours worked for each client.
SQL: 
    SELECT time_entries.duration
    FROM time_entries
    LEFT JOIN projects
    ON projects.id = time_entries.project_id
    GROUP BY projects.client_id;
Result:
    9 rows returned in 38ms

- Find the client for whom Mrs. Lupe Schowalter (the developer) has worked the greatest number of hours.
SQL:
    SELECT time_entries.duration, clients.name
    FROM developers 
    JOIN time_entries ON developers.id = time_entries.developer_id 
    JOIN projects ON time_entries.project_id = projects.id
    JOIN clients ON projects.client_id = clients.id
    WHERE developers.name = "Mrs. Lupe Schowalter"GROUP BY clients.name
    ORDER BY time_entries.duration DESC
    LIMIT 1;
Result:
    duration: 6 name: West Group

- List all client names with their project names (multiple rows for one client is fine).  Make sure that clients still show up even if they have no projects.
SQL:
    SELECT clients.name, projects.name
    FROM clients
    LEFT JOIN projects 
    ON clients.id = projects.client_id
    GROUP BY projects.client_id;
Result:
    10 rows returned in 21ms

- Find all developers who have written no comments.
SQL:
    SELECT *
    FROM developers
    LEFT JOIN comments 
    ON comments.developer_id = developers.id
    WHERE comments.id IS NULL;
Result:
    13 rows returned