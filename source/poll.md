Release 1:
SELECT COUNT(1)
FROM votes
JOIN congress_members
ON congress_members.id = votes.politician_id
where congress_members.name LIKE 'Sen. Olympia Snowe';

SELECT COUNT(1)
FROM votes
JOIN congress_members
ON congress_members.id = votes.politician_id
WHERE congress_members.name LIKE 'Rep. Erik Paulsen';

SELECT congress_members.id, congress_members.name, congress_members.party, congress_members.location, congress_members.grade_1996, congress_members.grade_current, congress_members.years_in_congress, congress_members.dw1_score
FROM congress_members
JOIN votes
ON congress_members.id = votes.politician_id
GROUP BY congress_members.name
ORDER BY COUNT(votes.politician_id) DESC;

SELECT congress_members.id, congress_members.name, congress_members.party, congress_members.location, congress_members.grade_1996, congress_members.grade_current, congress_members.years_in_congress, congress_members.dw1_score
FROM congress_members
JOIN votes
ON congress_members.id = votes.politician_id
GROUP BY congress_members.name
ORDER BY COUNT(votes.politician_id) ASC;

Release 2:

SELECT congress_members.name, COUNT(1), voters.first_name, voters.last_name, voters.gender, voters.party
FROM congress_members
JOIN voters;

ELECT congress_members.name, COUNT(1), voters.first_name, voters.last_name, voters.gender, voters.party
FROM congress_members
JOIN votes
ON congress_members.id = votes.politician_id
JOIN voters
ON voters.id = votes.voter_id
GROUP BY voters.id
ORDER BY COUNT(1);
SELECT congress_members.name, COUNT(1), voters.first_name, voters.last_name, voters.gender, voters.party
FROM congress_members
JOIN votes
ON congress_members.id = votes.politician_id
JOIN voters
ON voters.id = votes.voter_id
GROUP BY voters.id
ORDER BY COUNT(1);

SELECT congress_members.name, count(votes.politician_id)
FROM congress_members JOIN votes ON congress_members.id = votes.politician_id
GROUP BY votes.politician_id
ORDER BY votes.politician_id DESC
;
SELECT congress_members.name, count(votes.politician_id)
FROM congress_members JOIN votes ON congress_members.id = votes.politician_id
GROUP BY votes.politician_id
ORDER BY count(votes.politician_id) DESC
;
LIMIT 10;
SELECT voters.first_name, voters.last_name
FROM voters
JOIN votes ON voters.id = votes.voter_id
JOIN congress_members ON votes.politician_id = congress_members.id
GROUP BY congress_members.location
ORDER BY SUM(votes.politician_id) DESC;
SELECT voters.*
FROM votes
JOIN congress_members ON congress_members.id = votes.politician_id
JOIN voters ON voters.id = votes.voter_id
WHERE location = 'CA';
SELECT voters.*, COUNT(1)
FROM votes
JOIN voters ON voters.id = votes.voter_id
GROUP BY voters.id
HAVING COUNT(1) > 2;
SELECT voters.*, congress_members.name
FROM voters
JOIN votes ON voters.id = votes.voter_id
JOIN congress_members ON votes.politician_id = congress_members.id
;
SELECT first_name, last_name, congress_members.name, COUNT(1)
FROM votes
JOIN voters ON voters.id = votes.voter_id
JOIN congress_members ON congress_members.id = votes.politician_id
GROUP BY voter_id, politician_id HAVING count(1) > 1;

 SELECT first_name, last_name, congress_members.name, COUNT(1)
   ...> FROM votes
   ...> JOIN voters ON voters.id = votes.voter_id
   ...> JOIN congress_members ON congress_members.id = votes.politician_id
   ...> GROUP BY voter_id, politician_id HAVING count(1) > 1;