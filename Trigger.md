Triggers are powerful tools in database systems that enable you to execute custom logic automatically in response to certain events, such as inserts, updates, or deletes on a table.

Here's why this trigger might be important:
1 - Audit Trail
2 - Data Integrity
3 - Troubleshooting
4 - Security
5 - Historical Analysis

Here some exemple. First create a log table:

	CREATE TABLE CHICAGO_PUBLIC_SCHOOLS_LOG (
    log_id INT GENERATED ALWAYS AS IDENTITY,
    school_id INTEGER,
    updated_column VARCHAR(50),
    old_value INTEGER,
    new_value INTEGER,
    updated_at TIMESTAMP,
    PRIMARY KEY (log_id)
	);

Then the trigger:

	--#SET TERMINATOR @
	CREATE OR REPLACE TRIGGER update_chicago_public_schools_trigger
	AFTER UPDATE ON CHICAGO_PUBLIC_SCHOOLS
	REFERENCING OLD AS old_row NEW AS new_row
	FOR EACH ROW
	BEGIN
  	  INSERT INTO CHICAGO_PUBLIC_SCHOOLS_LOG (school_id, updated_column, old_value, new_value, updated_at)
      VALUES (old_row.school_id, 'leaders_score', old_row.leaders_score, new_row.leaders_score, CURRENT_TIMESTAMP);
	END@

 Check the log data:
 
	SELECT * 
 	FROM CHICAGO_PUBLIC_SCHOOLS_LOG,

![imagem](https://github.com/romulopires08/SQL_queries/assets/105392322/72ec12b5-950e-4f15-b9e3-9affb76058c3)

