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

select first_name, last_name, gender, voters.party from voters
join votes on voters.id = voter_id
where politician_id = (select id from congress_members where name = 'Rep. Dan Benishek');

select name, count(votes.id) from congress_members
join votes on congress_members.id = politician_id
group by politician_id
order by count(votes.id) desc
limit 1;

select count(votes.id), name, location, grade_1996 from congress_members
join votes on congress_members.id = politician_id
where grade_current < 9
group by politician_id;

select congress_members.location, count(votes.id) from votes
join congress_members on congress_members.id = politician_id
group by congress_members.location
order by count(votes.id) desc limit 10;

select first_name, last_name from voters
join votes on voter_id=voters.id
where politician_id in (select id from congress_members where location = 'CA');

select first_name, last_name from voters
join votes on voter_id=voters.id
where politician_id in (select id from congress_members where location = 'CA')
group by voter_id
having count(voter_id) > 2;

select voters.id from voters
  join votes on voter_id=voters.id
  where politician_id in (select id from congress_members where location = 'CA')
  group by voter_id
  having count(voter_id) > 2;

select politician_id from votes
where voter_id in (1312, 1599, 2478, 2738, 3028, 3823, 4235, 4554, 4581)
group by voter_id
having count(politician_id)>2;

select voter_id, politician_id from votes where politician_id in (83, 423, 271, 284, 227, 471, 436, 103, 423, 268, 258, 212, 296, 192, 293, 155, 392, 411, 508, 196, 187, 155, 231, 155, 13, 192, 323, 502, 444, 510, 500, 449, 250, 169, 432, 334, 480, 411, 250, 264, 261, 297, 192, 502)
and voter_id in (1312, 1599, 2478, 2738, 3028, 3823, 4235, 4554, 4581)
order by voter_id, politician_id;

select first_name, last_name, name from voters, congress_members
where voters.id = 4581 and congress_members.id = 192;