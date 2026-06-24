### les codes ou la logique que j ai compris **maintenant compris**




### Function.php

Dans => get_all_departments

 
mijery nombre total de ligne amin`ny table iray

```sql
SELECT COUNT (*) FROM dept_emp

```

Concat => Manambatra resultat samy hafa anaty colonne iray 

```sql
    
    CONCAT(e.first_name, ' ', e.last_name) AS manager_name

```



Dans get_jobs_stats()

INNER JOIN dia manambatra anle zavatra mitovy amle table

```sql 
 INNER JOIN
```

Conversion de resultat en int

```sql
  return (int)$line['total'];
```



DATEDIFF = nombre de jours entre deux dates

Si to_date = '9999-01-01', alors utiliser la date actuelle (CURDATE()).
Sinon, utiliser la valeur de to_date

```sql
IF(to_date = '9999-01-01', CURDATE(), to_date)

```

How to use 

```sql
DATEDIFF( fin_date ,from_date)
```






### Fonction, logique et code que j ai pas compris
```sql
ON DUPLICATE KEY UPDATE
```

<> 
```sql

function get_departments_except($dept_no)
{
    $sql = "SELECT dept_no, dept_name
            FROM departments
            WHERE dept_no <> '%s'
            ORDER BY dept_name";ON DUPLICATE KEY UPDATE
    $sql = sprintf($sql, $dept_no);
    return get_all_lines($sql);
}


```
All
```sql
function search_employees($dept_no, $name, $age_min, $age_max)
{

    if ($dept_no !== '') {
        $conditions[] = sprintf("de.dept_no = '%s'", $dept_no);
    }
    if ($name !== '') {
        // %% produit un % littéral dans sprintf → '%nom%' pour le LIKE
        $conditions[] = sprintf("(e.first_name LIKE '%%%s%%' OR e.last_name LIKE '%%%s%%')", $name, $name);
    }
    if ($age_min !== '') {
        $conditions[] = sprintf("TIMESTAMPDIFF(YEAR, e.birth_date, CURDATE()) >= %d", $age_min);
    }
    if ($age_max !== '') {
        $conditions[] = sprintf("TIMESTAMPDIFF(YEAR, e.birth_date, CURDATE()) <= %d", $age_max);
    }

    // S'il n'y a aucun filtre, "1=1" garde une clause WHERE valide.
    $where = empty($conditions) ? '1=1' : implode(' AND ', $conditions);

    $sql = "SELECT DISTINCT
                   e.emp_no,
                   e.first_name,
                   e.last_name,
                   e.gender,
                   TIMESTAMPDIFF(YEAR, e.birth_date, CURDATE()) AS age,
                   d.dept_name
            FROM employees e
            INNER JOIN dept_emp de
                    ON de.emp_no = e.emp_no AND de.to_date = '9999-01-01'
            INNER JOIN departments d
                    ON d.dept_no = de.dept_no
            WHERE $where
            ORDER BY e.last_name, e.first_name
            LIMIT 200";
    return get_all_lines($sql);
}



```