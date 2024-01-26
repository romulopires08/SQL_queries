--List the case number, type of crime and community area for all crimes in community area number 18.

SELECT T2.CASE_NUMBER, T2.PRIMARY_TYPE,T1.COMMUNITY_AREA_NUMBER
FROM CHICAGO_CENSUS_DATA AS T1
INNER JOIN CHICAGO_CRIME_DATA AS T2 ON T1.COMMUNITY_AREA_NUMBER = T2.COMMUNITY_AREA_NUMBER
WHERE T1.COMMUNITY_AREA_NUMBER =18;

Result:
![imagem](https://github.com/romulopires08/SQL_queries/assets/105392322/87fbf430-d729-4333-8e15-b05bffdb8075)

--List all crimes that took place at a school. Include case number, crime type and community name.
SELECT T2.LOCATION_DESCRIPTION, T2.CASE_NUMBER, T2.PRIMARY_TYPE, T1.COMMUNITY_AREA_NAME 
FROM CHICAGO_CENSUS_DATA AS T1
RIGHT JOIN CHICAGO_CRIME_DATA AS T2 ON T1.COMMUNITY_AREA_NUMBER = T2.COMMUNITY_AREA_NUMBER
WHERE T2.LOCATION_DESCRIPTION LIKE '%SCHOOL%';
Result:
![imagem](https://github.com/romulopires08/SQL_queries/assets/105392322/3350ca06-fb80-44c6-83aa-7d0ba77df982)

--For the communities of Oakland, Armour Square, Edgewater and CHICAGO list the associated community_area_numbers and the case_numbers.
SELECT T1.COMMUNITY_AREA_NAME, T2.CASE_NUMBER
FROM CHICAGO_CENSUS_DATA AS T1
LEFT JOIN CHICAGO_CRIME_DATA AS T2 ON T1.COMMUNITY_AREA_NUMBER = T2.COMMUNITY_AREA_NUMBER
WHERE T1.COMMUNITY_AREA_NAME = 'Oakland' OR T1.COMMUNITY_AREA_NAME ='Armour Square' OR T1.COMMUNITY_AREA_NAME ='Edgewater';
Result:
![imagem](https://github.com/romulopires08/SQL_queries/assets/105392322/54700707-523c-4322-a2ca-f5fba1933b39)

--Write and execute a SQL query to list the school names, community names and average attendance for communities with a hardship index of 98.
SELECT T1.NAME_OF_SCHOOL, T1.COMMUNITY_AREA_NAME, T1.AVERAGE_STUDENT_ATTENDANCE, T1.AVERAGE_TEACHER_ATTENDANCE
FROM CHICAGO_PUBLIC_SCHOOLS AS T1
INNER JOIN CHICAGO_CENSUS_DATA AS T2 ON T1.COMMUNITY_AREA_NUMBER=T2.COMMUNITY_AREA_NUMBER
WHERE T2.HARDSHIP_INDEX = 98
Result:
![imagem](https://github.com/romulopires08/SQL_queries/assets/105392322/c850f4c3-2c90-4700-973d-9a79015675aa)



