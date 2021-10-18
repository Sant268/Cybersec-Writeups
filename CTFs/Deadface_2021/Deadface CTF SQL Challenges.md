---
tags: [CTF]
title: Deadface CTF SQL Challenges
created: '2021-10-18T13:17:51.361Z''
---

# Deadface CTF SQL Challenges

1. Body Count
-> to find the number of customers in the leaked dB
solution: I just counted out the number of inserted values in the .sql file via text editor 
flag{10000}
---
2. Keys
-> to find any foreign key from loans table
solution: Again, browsed the file via Notepad++, to find 3 valid foreign keys
flag{fk_loans_employee_id} (tho any of the other two would be acceptable solutions)
---
3. City Lights
-> to find out how many unique cities the employees live in
solution: 
```sql
SELECT COUNT(*) FROM (SELECT DISTINCT city FROM customers) as ans; 
```
flag{460}

4. El Paso
->  find out the dollar value of all outstanding loan balances issued by employees who live in El Paso
solution (1):
```sql
select X.* from loans X where X.employee_id in (select Y.employee_id from employees Y where Y.city = 'El Paso')

then sum(X.balance) it
```
solution 2: Use inner join between employee_id of loans and  employees table
but I used soln. 1 in the CTF
flag{$877,401.00}

5. All A-Loan 
-> Which city in California has the most money in outstanding Small Business loans?
soln:
Check (...and re-check-) loan_type from another table first (which I didn't and was trying with loan_type==2)
then innero join emp_id's, select sum and order by sum
(didn't submit in time)
```sql
select
    sum(A.balance), B.city
from
    loans A
inner join employees B
    on A.employee_id = B.employee_id
where
    B.state = 'CA' AND A.loan_type_id = 3
GROUP BY B.city
ORDER BY sum(A.balance) DESC;
```
flag{Oakland_$90,600.00}
