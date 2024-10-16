# Задание на знание SQL (1)

## Описание

Есть 3 таблицы:
- **Students**: содержит ID и имя студентов.
- **Friends**: содержит связи между студентами.
- **Packages**: содержит информацию о зарплате.

## Задача

Написать SQL запрос, который выведет имена студентов, друзья которых получили бОльшую зарплату, чем они. Имена должны быть отсортированы в порядке убывания зарплаты, предлагаемой их друзьям.

## РЕШЕНИЕ

1. Создадим игрушечную БД в Python:

   ```python.
    import sqlite3
    import numpy as np
    import pandas as pd

    id = np.arange(1, 11)
    names = np.array(['Ivan', 'Olesya', 'Alexander', 'Anastasiya', 'Oleg', 'Evgeniy', 'Anna', 'Diana', 'Dmiriy', 'Venera'])
    friends = np.random.choice(id, size=10, replace=False)
    salary = np.random.randint(100000, high=150000, size=10)

    Students = pd.DataFrame(columns=['ID', 'Name'])
    Friends = pd.DataFrame(columns=['ID', 'Friend_ID'])
    Packages = pd.DataFrame(columns=['ID', 'Salary'])

    Students['ID'] = id
    Students['Name'] = names
    Friends['ID'] = id
    Friends['Friend_ID'] = friends
    Packages['ID'] = id
    Packages['Salary'] = salary

    con = sqlite3.connect('C:/Users/79504/Desktop/Projects/Test/db', timeout=10) 
    cur = con.cursor()
    Students.to_sql(con=con, name='Students', index=False, if_exists = 'replace')
    Friends.to_sql(con=con, name='Friends', index=False, if_exists = 'replace')
    Packages.to_sql(con=con, name='Packages', index=False, if_exists = 'replace')
    data_test = cur.execute('select * from Students') #тестовый запрос
    con.commit() 
    cur.fetchall()  
2. Откроем её в DBeaver и напишем соответствующий запрос:

```sql
SELECT 
    s.ID as ID,
    s.Name as Name,
    p.Salary as Salary,
    f.Friend_ID as Friend,
    p2.Salary as Friend_salary
FROM Students s 
JOIN Packages p ON s.ID = p.ID
JOIN Friends f ON s.ID = f.ID
JOIN Packages p2 ON f.Friend_ID = p2.ID
ORDER BY p2.Salary DESC;