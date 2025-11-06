https://leetcode.com/problems/actors-and-directors-who-cooperated-at-least-three-times/
``` pandas
grouped = (
        actor_director
        .groupby(["actor_id", "director_id"])
        .size()
        .reset_index(name="count")
    )
    
    filtered = grouped[grouped["count"] >= 3]
    
    return filtered.drop(columns="count")
```

``` sql
SELECT actor_id, director_id
FROM ActorDirector
GROUP BY actor_id, director_id
HAVING COUNT(*) >= 3
;
```

https://leetcode.com/problems/fix-names-in-a-table/
```SQL

SELECT
    user_id,
    CONCAT(UPPER(LEFT(name, 1)), LOWER(SUBSTRING(name, 2))) AS name
FROM Users
ORDER BY user_id;
```

```

import pandas as pd

def fix_names(users: pd.DataFrame) -> pd.DataFrame:
    users['name'] = users['name'].str.capitalize()
    return users.sort_values('user_id')
```

https://leetcode.com/problems/second-highest-salary/
```SQL

SELECT (
    SELECT DISTINCT salary
    FROM Employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1
) AS SecondHighestSalary;
```

```pandas

import pandas as pd

def second_highest_salary(employee: pd.DataFrame) -> pd.DataFrame:
    s = employee['salary'].drop_duplicates().sort_values(ascending=False)
    val = s.iloc[1] if len(s) > 1 else None
    return pd.DataFrame({"SecondHighestSalary": [val]})
```

https://leetcode.com/problems/combine-two-tables/
```SQL

SELECT
    p.firstName,
    p.lastName,
    a.city,
    a.state
FROM Person p
LEFT JOIN Address a
ON p.personId = a.personId;
```

```Pandas

import pandas as pd

def combine_two_tables(person: pd.DataFrame, address: pd.DataFrame) -> pd.DataFrame:
    return person.merge(address[['personId','city','state']], on='personId', how='left') \
                 [['firstName', 'lastName', 'city', 'state']]
```
https://leetcode.com/problems/list-the-products-ordered-in-a-period/
```SQL

SELECT p.product_name, SUM(o.unit) AS unit
FROM Products p
JOIN Orders o
  ON p.product_id = o.product_id
WHERE o.order_date BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY p.product_name
HAVING SUM(o.unit) >= 100;
```

```pandas

import pandas as pd

def list_products(products: pd.DataFrame, orders: pd.DataFrame) -> pd.DataFrame:
    feb = orders[(orders['order_date'] >= '2020-02-01') & (orders['order_date'] <= '2020-02-29')]
    total = feb.groupby('product_id', as_index=False)['unit'].sum()
    total = total[total['unit'] >= 100]
    return total.merge(products[['product_id','product_name']], on='product_id')[['product_name','unit']]
```
https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/
```

import pandas as pd

def replace_employee_id(employees: pd.DataFrame, employee_uni: pd.DataFrame) -> pd.DataFrame:
    return employees.merge(employee_uni, on='id', how='left')[['unique_id', 'name']]
```

```

SELECT u.unique_id, e.name
FROM Employees e
LEFT JOIN EmployeeUNI u
ON e.id = u.id;
```

https://leetcode.com/problems/game-play-analysis-iv/
```sql

WITH first_login AS (
    SELECT player_id, MIN(event_date) AS first_date
    FROM Activity
    GROUP BY player_id
),
next_day AS (
    SELECT a.player_id
    FROM Activity a
    JOIN first_login f
      ON a.player_id = f.player_id
     AND a.event_date = DATE_ADD(f.first_date, INTERVAL 1 DAY)
)
SELECT ROUND(COUNT(DISTINCT next_day.player_id) / COUNT(DISTINCT first_login.player_id), 2) AS fraction
FROM first_login
LEFT JOIN next_day
  ON first_login.player_id = next_day.player_id;
```

```pandas

import pandas as pd

def gameplay_analysis(activity: pd.DataFrame) -> pd.DataFrame:
    first = activity.groupby('player_id', as_index=False)['event_date'].min()
    first['next_day'] = first['event_date'] + pd.Timedelta(days=1)

    merged = first.merge(activity[['player_id','event_date']], 
                         left_on=['player_id','next_day'], 
                         right_on=['player_id','event_date'], 
                         how='left')

    fraction = merged['event_date_y'].notna().mean().round(2)
    return pd.DataFrame({'fraction': [fraction]})
```

https://leetcode.com/problems/project-employees-i/
```sql

```

```pandas


```

https://leetcode.com/problems/department-top-three-salaries/
