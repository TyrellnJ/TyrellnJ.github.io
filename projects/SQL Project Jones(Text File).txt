--1)What museum has the highest count of cubist paintings? 
select * from artist
select * from museum

SELECT museum.name,
COUNT(*) AS cubist_count
FROM work
JOIN museum
ON work.museum_id = museum.museum_id
JOIN artist
ON work.artist_id = artist.artist_id
WHERE artist.style = 'Cubist'
GROUP BY museum.name, work.museum_id

--What other styles of art does this museum typically display?
SELECT DISTINCT museum.name, artist.style
FROM work
JOIN museum
ON work.museum_id = museum.museum_id
JOIN artist
ON work.artist_id = artist.artist_id
WHERE museum.name LIKE 'The Metropolitan%'

--2) Which artists have their work displayed in museums in many different countries?
SELECT
COUNT(DISTINCT museum.country) AS country_co, artist.full_name
FROM work
JOIN museum
ON work.museum_id = museum.museum_id
JOIN artist
ON work.artist_id = artist.artist_id
GROUP BY artist.full_name
HAVING COUNT(DISTINCT museum.country) > 1

/* 
3)	Create a table that shows the most frequently painted subject for each style of painting and 
how many paintings there were for the most frequently painted subject in that style
Exclude cases where there is no information
*/

select * from subject

DROP TABLE IF EXISTS f_subject;
CREATE TABLE f_subject AS
SELECT work.style, subject.subject,
COUNT(*) AS subject_co
FROM work
JOIN subject
ON work.work_id = subject.work_id
WHERE style IS NOT NULL
GROUP BY work.style, subject.subject

SELECT * FROM f_subject
SELECT DISTINCT style FROM f_subject


WITH rankings AS (
SELECT style, subject, subject_co,
ROW_NUMBER() OVER (PARTITION BY style
ORDER BY subject_co DESC) AS ranking 
FROM f_subject)

SELECT style, subject, subject_co
FROM rankings
WHERE ranking = 1
ORDER BY subject_co DESC

SELECT * FROM f_subject
WHERE style LIKE 'Rococo%'
ORDER BY subject_co DESC
























