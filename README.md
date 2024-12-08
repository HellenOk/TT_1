# TT_1
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



