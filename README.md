# Working-with-Olympics-Data-Set-in-MySQL-from-Scratch
You will get to know the common mistakes and then how to correct them as you progress.

# Let`s learn how to load a csv file in MySQL
Step 1: Create or Select a Database
Open MySQL Workbench.
Create a new database or select an existing one.
![image](https://github.com/user-attachments/assets/47d9cc61-bf55-4c26-bcc4-b577eeadb870)

Step 2: Use the Table Data Import Wizard
Right-click on the database in the Navigator panel.
Select Table Data Import Wizard from the context menu.

![image](https://github.com/user-attachments/assets/a7f29054-4395-4973-889c-c327ca463504)


Step 3: Select the CSV File
In the wizard, browse to and select the CSV file you want to import.
Click Next.

# Let`s learn how we can play with this dataset using some questions:

To replace blank values with NA
```diff
UPDATE games
SET `Cost, Billion USD` = NULL
WHERE `Cost, Billion USD` = '' OR `Cost, Billion USD` = '**';

```
# Commad to remove safe mode
```diff
SET SQL_SAFE_UPDATES=0;
```

# 1. Which countries hosted the most Olympic Games?
Thoughts that could come to your mind:
You want to display country Coloumn ( so SELECT COUNTRY )
The data mentions USA 5 times in different random rows ( therefore GROUP BY COUNTRY)
I want to count how many times a country hosted olympics hence ( COUNT )

USE name_of_your_database;

SELECT Country, COUNT(*) as num

FROM Games

group by country;

Output: Oh no! This is not in order!


![image](https://github.com/user-attachments/assets/acd7a129-ba02-4d97-9dfb-0a0b9016be33)

SO arrange it in order

![image](https://github.com/user-attachments/assets/977fd044-6985-4a75-91b9-9cb7913a5eda)

COUNT(*): This function counts the number of records (rows) for each country, essentially telling us how many times each country has hosted the Olympics.

GROUP BY Country: We use this clause to group the data by country so that we can perform an aggregation (counting) for each country separately.

ORDER BY num_olympics DESC: This sorts the result in descending order of the number of Olympic Games hosted. Countries with the most Olympic Games appear first.

# 2 What is the average number of athletes per Olympic Games, grouped by season (Winter/Summer)?
## Caution
```diff
- COUNT(*): Can be used to count all rows in a table or group, including those with NULL values.
You cannot use * with other aggregate functions like SUM(), AVG(), MIN(), etc.,
because they require a specific column to perform calculations on.
```
![image](https://github.com/user-attachments/assets/de73d4f2-9996-400e-a88e-c802eeb9db75)

# 3 Display unique countries present in the dataset?
![image](https://github.com/user-attachments/assets/37447983-7fe7-4640-ad94-f9fff9972ea6)

# 4 What is the total cost of hosting the games by country (in billions USD)?
## Caution
```diff
- Coloumn name in csv file had a space in it Cost, Billion USD which will give syntax errors later to avoid that we will change the column name for that we use ALTER 
```
```dif
ALTER TABLE Games

CHANGE COLUMN `Cost, Billion USD` Cost DECIMAL(10, 7);

SHOW COLUMNS FROM Games;

```
```diff
select country,sum(cost) as total_cost
from games
group by country;
```
# 5 What is the maximum and minimum number of athletes in any game?

This will just give you the maximum number but won`t give you the Country name

```
SELECT MAX(Athletes) FROM Games
```

![image](https://github.com/user-attachments/assets/3dc37714-e484-43fe-8e16-5159ac89e605)



To display both the game with the maximum number of athletes and the game with the minimum number of athletes in the same output, you can use a UNION query

```
SELECT 'Max' AS Type, Games, Athletes
FROM Games
WHERE Athletes = (SELECT MAX(Athletes) FROM Games)
UNION 
SELECT 'Min' AS Type, Games, Athletes
FROM Games
WHERE Athletes = (SELECT MIN(Athletes) FROM Games);
```
![image](https://github.com/user-attachments/assets/a4e7658e-c3ec-4b27-b62b-aad787f953ac)


```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
