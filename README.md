# dbms_assignment
IntercityExpress Database Assignment

Group Members: 
Nazrana Bagwan 23112047
Arya Jawarkar 23112015
Aparimita Ranpise 23112031

This database was created after veryfying all the data requirements and then creating the required tables(normalized). All the work was done on MySQL.
All tables creation and data loading was done by Nazrana and Arya. 
Then the set of queries assigned was created and executed by each member and respective set queries with output are added in the repository.
This repository has:
. Part1 Solution - create table queries and data insertion statements
. Set A,B,C Query Solutions with outputs
. Relational Schema
. Database file

Set C- Nazrana Bagwan 23112047
Set A - Arya Jawarkar 23112015
Set B - Aparimita Ranpise 23112031


------SET A 

Q.1)	Show all trains information travelling between Goa Mumbai and Ajmer-Lonavala where at least half the coaches are due for maintenance on or before 30 November this year.
```sql
select Train_id,Train_name from (routes r natural join tickets t) natural join (select t.*,count(Coach_no) as totCoaches,sum(case when Last_maintained_date <= '2023-11-30' then 1 end) as coachesdue from trains t natural join coaches c  group by Train_id having coachesdue >= totCoaches/2 ) ts where r.Route = 'Ajmer-Lonavala' or r.Route = 'Goa-Mumbai';
```
+----------+------------------+
| Train_id | Train_name       |
+----------+------------------+
| T001     | Shatabdi Express |
| T002     | Rajdhani Express |
+----------+------------------+


Q.2) List all the routes in descending order of seats sold, including route information and distribution of seats sold (Children, Adult, Senior Citizen) in the month of October this year.
```sql
select r.Route,count(Ticket_no) as totSeats,sum(case when p.P_category = 'Children' then 1 else 0 end) as Children,sum(case when p.P_category = 'Adult' then 1 else 0 end) as Adult,sum(case when p.P_category = 'senior citizen' then 1 else 0 end) as SeniorCitizen from routes r join tickets t on r.Route_id = t.route_id join passengers p on p.P_id = t.P_id where monthName(t.Date) = 'October' and year(t.Date) = year(now()) group by r.Route order by totSeats desc;
``` 
+------------------------+----------+----------+-------+---------------+
| Route                  | totSeats | Children | Adult | SeniorCitizen |
+------------------------+----------+----------+-------+---------------+
| Dharwad - Bengaluru    |       17 |        3 |    13 |             1 |
| Ahmedabad - Surat      |        2 |        0 |     2 |             0 |
| Goa-Mumbai             |        1 |        0 |     1 |             0 |
| Ajmer-Lonavala         |        1 |        1 |     0 |             0 |
| Bangalore - Mysuru     |        1 |        0 |     1 |             0 |
| Chennai - Coimbatore   |        1 |        0 |     1 |             0 |
| Kolkata - Dhanbad      |        1 |        1 |     0 |             0 |
| Hyderabad - Vijayawada |        1 |        0 |     1 |             0 |
| Jaipur - Jodhpur       |        1 |        0 |     1 |             0 |
| Lucknow - Varanasi     |        1 |        1 |     0 |             0 |
| Chandigarh - Amritsar  |        1 |        0 |     1 |             0 |
| Indore - Bhopal        |        1 |        0 |     1 |             0 |
| Coimbatore - Kochi     |        1 |        0 |     1 |             0 |
| Kanpur - Allahabad     |        1 |        1 |     0 |             0 |
| Jaipur - Udaipur       |        1 |        0 |     1 |             0 |
| Varanasi - Allahabad   |        1 |        0 |     0 |             1 |
+------------------------+----------+----------+-------+---------------+



Q.3) List all agents’ information with more than 10 confirmed bookings in the month of September this year.
```sql
 select t.* from (select TA_id from tickets where month(Date) = 9 group by TA_id having count(1) > 10) a join travelagents t  on a.TA_id = t.TA_id;
```
+-------+---------------+-------------+
| TA_id | TA_name       | TA_Phone_no |
+-------+---------------+-------------+
| TA001 | Rahul Travels |  9876543210 |
+-------+---------------+-------------+


Q.4) Display the details of the route most travelled by Senior Citizens.
```sql
select r.*,count(P_category) as rc from (tickets t natural join passengers p )natural join routes r where p.P_category = 'senior citizen' group by Route_id order by rc desc limit 1;
```
+----------+-----------------------+---------------+-------------------+----------------+------------+----------------+----+
| Route_id | Route                 | Start_station | End_station       | Distance_in_Km | Time_taken | Operating_days | rc |
+----------+-----------------------+---------------+-------------------+----------------+------------+----------------+----+
| R011     | Chandigarh - Amritsar | Chandigarh    | Amritsar Junction |            240 | 3 hours    | Mon-Sat        | 12 |
+----------+-----------------------+---------------+-------------------+----------------+------------+----------------+----+



Q.5) Display the details of the route where a train was always on time.


This is alternate query for Q.5 because it is not working properly or i am not getting the query the below query is of set C


Q3.List the details of most popular route of InterCity Express Trains.
```sql
select r.* from routes r natural join tickets t group by r.Route_id order by count(Ticket_no) desc limit 1;
```
+----------+---------------------+---------------+-------------+----------------+------------+--------------------------------+
| Route_id | Route               | Start_station | End_station | Distance_in_Km | Time_taken | Operating_days                 |
+----------+---------------------+---------------+-------------+----------------+------------+--------------------------------+
| R004     | Dharwad - Bengaluru | Dharwad       | Bengaluru   |            432 | 7 hours    | Monday, Wednesday and Saturday |
+----------+---------------------+---------------+-------------+----------------+------------+--------------------------------+


------SET B


Set B
1.	Show schedule of all trips including main driver information for 10th October this year.
```sql
SELECT ts.date, ts.start_time, ts.end_time, tss.driver_id, s.staff_name AS driver_name
FROM trainrouteschedule ts
JOIN trainstaffschedule tss ON ts.train_id = tss.train_id AND ts.date = tss.date
JOIN staff s ON tss.driver_id = s.staff_id
WHERE ts.date = '2023-10-10';
```
+------------+------------+----------+-----------+-------------+
| date       | start_time | end_time | driver_id | driver_name |
+------------+------------+----------+-----------+-------------+
| 2023-10-10 | 11:20:00   | 22:30:00 | S043      | Rahul Patel |
| 2023-10-10 | 10:15:00   | 20:15:00 | S043      | Rahul Patel |
+------------+------------+----------+-----------+-------------+

2.	List all coaches with mileage between 4000 and 4999 km covered for September this year; include information on the coach, its last service date and total number of scheduled trips.
```sql
SELECT c.coach_no, c.mileage, m.service_date AS last_maintained_date, COUNT(trs.route_id) AS total_trips
FROM coaches c
JOIN maintainance m ON c.coach_no = m.coach_no
LEFT JOIN trainrouteschedule trs ON c.train_id = trs.train_id AND MONTH(trs.date) = 9
WHERE c.mileage BETWEEN 4000 AND 4999
GROUP BY c.coach_no, c.mileage, m.service_date;
```
+----------+---------+----------------------+-------------+
| coach_no | mileage | last_maintained_date | total_trips |
+----------+---------+----------------------+-------------+
| COACH001 |    4500 | 2023-04-20           |           0 |
| COACH009 |    4700 | 2023-03-13           |           0 |
| COACH011 |    4100 | 2023-05-12           |           0 |
+----------+---------+----------------------+-------------+


3.	List all agents, in descending order of percentage of confirmed booking each trip in the month of October this year. Include agent and route information in your result.
```sql
SELECT ta.TA_id, ta.TA_name, r.route_id, COUNT(t.ticket_no) AS total_bookings,SUM(CASE WHEN t.confirmed = 'yes' THEN 1 ELSE 0 END) AS confirmed_bookings,(SUM(CASE WHEN t.confirmed = 'yes' 	THEN 1 ELSE 0 END) / COUNT(t.ticket_no)) * 100 AS percentage_confirmed
FROM travelagents ta
JOIN tickets t ON ta.TA_id = t.TA_id
JOIN trainrouteschedule trs ON t.train_id = trs.train_id AND t.route_id = trs.route_id AND MONTH(trs.date) = 10
JOIN routes r ON t.route_id = r.route_id
GROUP BY ta.TA_id, ta.TA_name, r.route_id
ORDER BY percentage_confirmed DESC;
```

+-------+------------------+----------+----------------+--------------------+----------------------+
| TA_id | TA_name          | route_id | total_bookings | confirmed_bookings | percentage_confirmed |
+-------+------------------+----------+----------------+--------------------+----------------------+
| TA020 | Alok Travelpoint | R020     |              1 |                  1 |             100.0000 |
+-------+------------------+----------+----------------+--------------------+----------------------+


4.	Display the details of the routes where majority of bookings are not made by agents.
```sql
SELECT r.route_id, r.route, COUNT(t.ticket_no) AS total_bookings,SUM(CASE WHEN t.TA_id IS NULL THEN 1 ELSE 0 END) AS bookings_not_by_agents
FROM routes r
LEFT JOIN tickets t ON r.route_id = t.route_id
GROUP BY r.route_id, r.route
HAVING bookings_not_by_agents > (0.5 * COUNT(t.ticket_no));
```
+----------+-------------------------+----------------+------------------------+
| route_id | route                   | total_bookings | bookings_not_by_agents |
+----------+-------------------------+----------------+------------------------+
| R020     | Gandhinagar - Ahmedabad |              3 |                      2 |
+----------+-------------------------+----------------+------------------------+



5.	Display the details of the agents who have made maximum commission in the Month of September.


```sql
SELECT ta.TA_id, ta.TA_name, SUM(t.price * (p.discount_offered / 100)) AS total_commission
FROM travelagents ta
JOIN tickets t ON ta.TA_id = t.TA_id
JOIN passengers p ON t.p_id = p.p_id
WHERE MONTH(t.date) = 09
GROUP BY ta.TA_id, ta.TA_name
ORDER BY total_commission DESC
LIMIT 1;
```
+-------+------------------+------------------+
| TA_id | TA_name          | total_commission |
+-------+------------------+------------------+
| TA009 | Aryan Travel Hub |               10 |
+-------+------------------+------------------+


----SET C

										  
Q1.List all trains not scheduled on 10th October this year.
```sql
select * from trains where train_id not in(select distinct train_id from trainrouteschedule where date='2023-10-10');
```
+----------+------------------------------------+
| Train_id | Train_name                         |
+----------+------------------------------------+
| T002     | Rajdhani Express                   |
| T003     | Deccan Queen                       |
| T004     | Duronto Express                    |
| T006     | Humsafar Express                   |
| T007     | Vande Bharat Express               |
| T008     | Tejas Express                      |
| T010     | Ahilyanagari Express               |
| T011     | Lokmanya Tilak Terminus Express    |
| T012     | Konkan Kanya Express               |
| T013     | Purvanchal Express                 |
| T014     | Ganga Kaveri Express               |
| T015     | Garib Rath Express                 |
| T016     | Chennai Express                    |
| T017     | Howrah Mail                        |
| T019     | Puri Shatabdi Express              |
| T020     | Swarna Jayanti Express             |
| T021     | Goa Sampark Kranti Express         |
| T022     | Maharashtra Sampark Kranti Express |
| T023     | Ratnagiri Express                  |
| T024     | Sarnath Express                    |
| T025     | Kashi Vishwanath Express           |
| T026     | Kaveri Express                     |
| T027     | Kolkata Mail                       |
| T028     | Ahmedabad Express                  |
| T029     | Mysuru Express                     |
| T030     | Vasco Da Gama Express              |
+----------+------------------------------------+

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Q2.List all fleets from Dharwad to Bengaluru, in ascending order of their monthly seats sold for the month of October this year.
```sql
SELECT distinct
    tr.train_name,
    SUM(CASE WHEN MONTH(t.date) = 10 THEN 1 ELSE 0 END) AS seats_sold_in_october
FROM
    coaches c
JOIN
    tickets t ON c.coach_no = t.coach_no
JOIN
    trainrouteschedule trs ON t.train_id = trs.train_id AND t.route_id = trs.route_id
JOIN
    routes r ON t.route_id = r.route_id
JOIN
    trains tr ON t.train_id = tr.train_id
WHERE
    r.start_station = 'Dharwad' AND r.end_station = 'Bengaluru'
GROUP BY
    t.train_id
ORDER BY
    seats_sold_in_october ASC;
```
+------------------+-----------------------+
| train_name       | seats_sold_in_october |
+------------------+-----------------------+
| Duronto Express  |                     2 |
| Tejas Express    |                     8 |
| Gatimaan Express |                    18 |
+------------------+-----------------------+

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Q3.List the details of most popular route of InterCity Express Trains.
```sql
SELECT t.Route_id,r.Route, COUNT(Ticket_no) AS TicketCount
FROM tickets t,routes r
where t.Route_id=r.Route_id
GROUP BY Route_id
ORDER BY TicketCount DESC
LIMIT 1;
```
+----------+---------------------+-------------+
| Route_id | Route               | TicketCount |
+----------+---------------------+-------------+
| R004     | Dharwad - Bengaluru |          18 |
+----------+---------------------+-------------+


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Q4.Display the details of the passengers who are frequent travellers with InterCity Express Trains. [Frequent traveller can be defined as the one who has travelled at least three times, irrespective of the route]
```sql
SELECT p.p_id,p.p_name as NAME,p.p_age as AGE,p.p_phone_no as PHONE_NO,COUNT(Ticket_no) AS TotalTrips
FROM tickets t,passengers p
where t.p_id=p.p_id
GROUP BY t.P_id
HAVING TotalTrips > 3;
```
+------+--------------+------+------------+------------+
| p_id | NAME         | AGE  | PHONE_NO   | TotalTrips |
+------+--------------+------+------------+------------+
| P004 | Anjali Gupta |   68 | 6543210987 |         13 |
| P107 | Raj Tiwari   |   28 | 8765432109 |          5 |
| P150 | Smita Tiwari |   25 | 4321098765 |          4 |
+------+--------------+------+------------+------------+

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Q5.Display the details of trains which arrived late at their destination, more than once in this year; Include the driver and co-driver information in the output. 
```sql
SELECT
    tas.train_id,
    t.train_name,
    trs.route_id,
    tas.date,
    ts.driver_id,
    MAX(s.staff_name) AS driver_name,
    ts.co_driver_id,
    MAX(st.staff_name) AS co_driver_name
FROM
    trainstationschedule tas
JOIN
    trains t ON tas.train_id = t.train_id
JOIN
    trainrouteschedule trs ON tas.train_id = trs.train_id AND tas.date = trs.date
JOIN
    trainstaffschedule ts ON tas.train_id = ts.train_id AND tas.date = ts.date
JOIN
    staff s ON ts.driver_id = s.staff_id
LEFT JOIN
    staff st ON ts.co_driver_id = st.staff_id
WHERE
    YEAR(tas.date) = 2023 -- Replace with the desired year
    AND (tas.act_arrival_time > tas.exp_arrival_time OR tas.act_departure_time > tas.exp_departure_time)
GROUP BY
    tas.train_id,
    trs.route_id,
    tas.date,
    ts.driver_id,
    ts.co_driver_id
HAVING
    COUNT(*) > 1;
```
+----------+------------------+----------+------------+-----------+--------------+--------------+----------------+
| train_id | train_name       | route_id | date       | driver_id | driver_name  | co_driver_id | co_driver_name |
+----------+------------------+----------+------------+-----------+--------------+--------------+----------------+
| T001     | Shatabdi Express | R001     | 2023-01-01 | S001      | Rahul Sharma | S003         | Amit Singh     |
+----------+------------------+----------+------------+-----------+--------------+--------------+----------------+
