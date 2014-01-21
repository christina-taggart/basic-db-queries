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

Release 1:

1) sqlite> select count(votes.id) from votes join congress_members on congress_members.id = votes.politician_id
   ...> where congress_members.id = 524

2) sqlite> select count(votes.id) from votes join congress_members on congress_members.id = votes.politician_id
   ...> where congress_members.name = 'Sen. Olympia Snowe';

3) sqlite> select count(votes.id) from votes join congress_members on congress_members.id = votes.politician_id
   ...> where congress_members.name = 'Rep. Erik Paulsen';

4) select congress_members.id, congress_members.name, congress_members.party, congress_members.location,
   ...> congress_members.grade_1996, congress_members.grade_current, congress_members.years_in_congress, congress_members.dw1_score,
   ...> COUNT(votes.id)  from congress_members join votes on congress_members.id = votes.politician_id group by 1 order by 9 desc ;

5) select congress_members.id, congress_members.name, congress_members.party, congress_members.location,
   ...> congress_members.grade_1996, congress_members.grade_current, congress_members.years_in_congress, congress_members.dw1_score,
   ...> COUNT(votes.id)  from congress_members join votes on congress_members.id = votes.politician_id group by 1 order by 9 asc;


Release 2:

1)

SELECT voters.first_name, voters.last_name, voters.gender, voters.party
FROM voters JOIN votes ON votes.voter_id = voters.id
WHERE politician_id =
(SELECT congress_members.id FROM congress_members JOIN votes on congress_members.id = votes.politician_id
GROUP BY 1
ORDER BY COUNT(votes.id) DESC
LIMIT 1);

2)

SELECT congress_members.name, congress_members.location, congress_members.grade_1996, COUNT(votes.id)
   ...> FROM congress_members JOIN votes ON congress_members.id = votes.politician_id
   ...> WHERE grade_current < 9
   ...> group by congress_members.name
   ...> ;

3)

SELECT congress_members.location
from congress_members join votes on congress_members.id = votes.politician_id
GROUP BY 1
ORDER BY COUNT(votes.id) DESC
LIMIT 10

——

SELECT voters.*
from voters join votes on voters.id = votes.voter_id
join congress_members on congress_members.id = votes.politician_id
WHERE congress_members.location = (SELECT congress_members.location
from congress_members join votes on congress_members.id = votes.politician_id
GROUP BY 1
ORDER BY COUNT(votes.id) DESC
LIMIT 1);

4)

SELECT voters.*, COUNT(votes.voter_id) from voters join votes on voters.id = votes.voter_id
GROUP BY 1
HAVING COUNT(votes.voter_id) > 2
ORDER BY COUNT(votes.voter_id) DESC;

5)

sqlite> SELECT voters.first_name, voters.last_name, congress_members.name, votes.*
   ...> FROM voters JOIN votes ON votes.voter_id = voters.id
   ...> JOIN congress_members ON congress_members.id = votes.politician_id
   ...> GROUP BY voter_id, politician_id
   ...> HAVING count(voter_id) > 1
   ...> ;

```