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

--Write and execute a SQL statement to create a view showing the columns listed in the following table, with new column names as shown in the second column.
--Column name in CHICAGO_PUBLIC_SCHOOLS     Column name in view
--NAME_OF_SCHOOL                            School_Name
--Safety_Icon 	                           Safety_Rating
--Family_Involvement_Icon 	                 Family_Rating
--Environment_Icon 	                        Environment_Rating
--Instruction_Icon 	                        Instruction_Rating
--Leaders_Icon 	                           Leaders_Rating
--Teachers_Icon                             Teachers_Rating
SELECT T1.NAME_OF_SCHOOL AS School_Name,
T1.Safety_Icon AS Safety_Rating,
T1.Family_Involvement_Icon AS Family_Rating,
T1.Environment_Icon AS Environment_Rating,
T1.Instruction_Icon AS Instruction_Rating,
T1.Leaders_Icon AS Leaders_Rating,
T1.Teachers_Icon AS Teachers_Rating
FROM CHICAGO_PUBLIC_SCHOOLS AS T1;

Result:
![imagem](https://github.com/romulopires08/SQL_queries/assets/105392322/0cff4de4-54a0-4960-9b73-50da9dbfd85d)

--Write the structure of a query to create or replace a stored procedure called UPDATE_LEADERS_SCORE that takes a in_School_ID parameter as an integer and a 
--in_Leader_Score parameter as an integer. Inside your stored procedure, write a SQL statement to update the Leaders_Score field in the CHICAGO_PUBLIC_SCHOOLS 
--table for the school identified by in_School_ID to the value in the in_Leader_Score parameter.

--#SET TERMINATOR @
CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (IN in_School_ID INTEGER, in_Leader_Score INTEGER) 
LANGUAGE SQL 
MODIFIES SQL DATA 
BEGIN 
    DECLARE leader_icon VARCHAR(20);
    DECLARE rollback_flag BOOLEAN DEFAULT FALSE;
    
    DECLARE CONTINUE HANDLER FOR SQLEXCEPTION SET rollback_flag = TRUE;

    IF in_Leader_Score BETWEEN 0 AND 20 THEN
        SET leader_icon = 'Very weak';
    ELSEIF in_Leader_Score BETWEEN 20 AND 40 THEN
        SET leader_icon = 'Weak';
    ELSEIF in_Leader_Score BETWEEN 40 AND 60 THEN
        SET leader_icon = 'Average';
    ELSEIF in_Leader_Score BETWEEN 60 AND 80 THEN
        SET leader_icon = 'Strong';
    ELSEIF in_Leader_Score BETWEEN 80 AND 100 THEN
        SET leader_icon = 'Very strong';
    ELSE
        SET rollback_flag = TRUE;
    END IF;

    IF rollback_flag THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Invalid score provided';
    ELSE
        UPDATE CHICAGO_PUBLIC_SCHOOLS
        SET leaders_score = in_Leader_Score,
            leaders_icon = leader_icon
        WHERE school_id = in_School_ID;
    END IF;

END@

Before calls the procedure
![imagem](https://github.com/romulopires08/SQL_queries/assets/105392322/7daaed3a-8737-497b-af32-df0e29dd7542)

After CALL UPDATE_LEADERS_SCORE(610038, 51);
![imagem](https://github.com/romulopires08/SQL_queries/assets/105392322/841dcbe8-6861-4397-a138-8077c143a927)

Tryng a wrong input like CALL UPDATE_LEADERS_SCORE(610038, 120);
The message Error: Invalid score provided, will be return
