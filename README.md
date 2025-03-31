# TT_1

## Description
This is a technical task for a data analyst, consisting of two parts: working with SQL and two tasks in EXCEL.


<details>
         
  <summary>💡 SQL task </summary>
  
         - Number of new user registrations by day by country groups
         
         - CTR of different types of emails by day
              
         - % of emails clicked within 10 minutes after sending, by types of emails in total for the last 7 days
              
</details>

<details>
  <summary>💡 Excel task_1  </summary>

The "call log" tab shows the work of operators. The operator and the player called are indicated. 
The Call field logs the attempted call. If the cell is empty, there was no call, 0 - no call, 1 - call.
The "Activity" tab shows the activity of players:
 - player, date of activity, type of activity, amount. We consider that all activity was after the events on the call log.
It is required to summarize statistics in the context of each operator, how many calls were made, how many were successful dialers. 
Also, how many unique players made deposits after the call from the operator and the amount of deposits after the call.
Deposits on the activity tab in the Type column, filter by the Deposit value.

</details>



![image](https://github.com/user-attachments/assets/3f15336e-42c8-46e6-bab3-0ea2173a05e9)

Потрібно написати SQL скріпти для того, чтобы:
###  1. Визначити кількість регистрацій нових користувачів  по дням та по групам країн:
         SELECT DATE( u. `date_reg`) AS дата,  u.COUNT(`id`), c. `group` AS группа_стран
         FROM  `users` u 
         JOIN `countries` c
         ON u. `id_country`=c. `id`
         GROUP BY DATE( u. `date_reg`), c. `group`
         ORDER BY DATE( u. `date_reg`), c. `group`;

###  2. Визначити CTR різних типів листів по дням:
        SELECT DATE(es. `date_sent`) AS дата,   es. `id_type` AS тип_писем, 
        100* COUNT(ec. `id`)/ COUNT(DISTINCT es. `id`) AS CTR
        FROM  `emails_sent` es
        LEFT JOIN `emails_clicks` ec
        ON es. `id`= ec.` id_email `
        GROUP BY DATE(es. `date_sent`), es. `id_type`
        ORDER BY DATE(es. `date_sent`), es. `id_type`;

###  3. Визначити % листів, що були клікнуті на протязі 10 хв після відправки, за типам листів суммарно за останні 7 суток:
        SELECT ,   es.`id_type` AS тип_писем,  100*COUNT(ec.`id`)/COUNT(es.`id`) AS %_писем
        FROM  `emails_sent` es
        LEFT JOIN `emails_clicks` ec
        ON es. `id`= ec.` id_email ` AND DATEDIFF(MINUTE, es. `date_sent`,ec. `date_click`)<=10
        WHERE ec. `date_click`>=DATEADD(DAY,-7, GETDATE())
        GROUP BY es.`id_type`
        ORDER BY %_писем DESC;



