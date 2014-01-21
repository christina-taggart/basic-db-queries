### Our Code

```
SELECT count(*)
FROM congress_members JOIN votes ON congress_members.id = politician_id
WHERE politician_id=524;

SELECT count(*)
FROM votes
WHERE politician_id=(SELECT id
FROM congress_members
WHERE name='Sen. Olympia Snowe');

SELECT count(*)
FROM congress_members JOIN votes ON congress_members.id = politician_id
WHERE congress_members.name='Rep. Erik Paulsen';

SELECT politician_id, name, party, location, count(*)
FROM congress_members JOIN votes ON congress_members.id = politician_id
GROUP BY politician_id
ORDER BY count(*) DESC;

SELECT politician_id, name, party, location, count(*)
FROM congress_members JOIN votes ON congress_members.id = politician_id
GROUP BY politician_id
ORDER BY count(*);


SELECT congress_members.name, congress_members.id, count(*)
FROM congress_members JOIN votes ON congress_members.id = politician_id
GROUP BY politician_id
ORDER BY count(*) DESC
LIMIT 1;

SELECT first_name, last_name, party
FROM voters JOIN votes on voters.id = voter_id
WHERE politician_id=224;

SELECT congress_members.name, congress_members.location, grade_1996, count(*)
FROM congress_members JOIN votes ON congress_members.id = politician_id
WHERE grade_current < 9;

SELECT location, COUNT(1)
FROM congress_members JOIN votes ON congress_members.id = politician_id
GROUP BY location
ORDER BY COUNT(1) DESC
LIMIT 10;

SELECT first_name, last_name
FROM voters JOIN votes ON voter_id=voters.id
JOIN congress_members ON politician_id=congress_members.id
WHERE congress_members.location IN (SELECT location
FROM congress_members JOIN votes ON congress_members.id = politician_id
GROUP BY location
ORDER BY COUNT(1) DESC
LIMIT 10);

SELECT first_name, last_name
FROM voters JOIN votes ON voter_id=voters.id
JOIN congress_members ON politician_id=congress_members.id
WHERE 2<(
SELECT COUNT(*)
FROM voters JOIN votes ON voter_id=voters.id
JOIN congress_members ON politician_id=congress_members.id
GROUP BY last_name, first_name
);

SELECT first_name, last_name, COUNT(*)
FROM voters JOIN votes ON voter_id=voters.id
JOIN congress_members ON politician_id=congress_members.id
GROUP BY first_name, last_name
HAVING COUNT() > 2
ORDER BY last_name DESC;

SELECT first_name, last_name, congress_members.name, COUNT(*)
FROM voters JOIN votes ON voter_id=voters.id
JOIN congress_members ON politician_id=congress_members.id
GROUP BY first_name, last_name
HAVING COUNT() > 2
ORDER BY last_name DESC;
```

### Table Schema
```
CREATE TABLE congress_members (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name VARCHAR(64) NOT NULL,
  party VARCHAR(64) NOT NULL,
  location VARCHAR(64) NOT NULL,
  grade_1996 REAL,
  grade_current REAL,
  years_in_congress INTEGER,
  dw1_score REAL
, created_at DATETIME, updated_at DATETIME);
CREATE TABLE voters (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    first_name VARCHAR(64) NOT NULL,
    last_name  VARCHAR(64) NOT NULL,
    gender VARCHAR(64) NOT NULL,
    party VARCHAR(64) NOT NULL,
    party_duration INTEGER,
    age INTEGER,
    married INTEGER,
    children_count INTEGER,
    homeowner INTEGER,
    employed INTEGER,
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL
  );
CREATE TABLE votes (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    voter_id INTEGER,
    politician_id INTEGER,
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY(voter_id) REFERENCES voters(id),
    FOREIGN KEY(politician_id) REFERENCES congress_members(id)
  );
```