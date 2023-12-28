# Customer-Segmentation-and-Perk-Allocation-Analysis
## THIS PROJECT BACKGROUND:

In this Project, I work with the Market Department of Travel-Tide an online travel industry to segment customer data according to business needs and deliver data-driven recommendations based on my findings.

Following the startup playbook, TravelTide has maintained a hyper-focus on building an unfair advantage along a limited number of dimensions - in this case, building the biggest travel inventory and making it easily searchable. Because of this narrow focus, certain aspects of the TravelTide customer experience are underdeveloped, resulting in poor customer retention.

The Head of the marketing dept is well known in the Marketing community as an expert in customer retention strategies, Using specific rewards programs, an advanced feature proven to generate repeat business if executed well.

The team's mission is to design and execute a fantastic personalized rewards program that keeps customers returning to the TravelTide platform. It is difficult to personalize rewards for customers without first understanding them, so for the project to be successful, The Team needs the data team for customer insights.

As a Data Analyst on this Project, Based on the context, I used SQL to extract datasets from the Database, imported them into Tableau, Performed Data Modelling, Explored the data at different levels of aggregation, and formed a plan for further analysis. I Made calculations related to the business context and then segmented customer behavior using statistical and visual techniques using RFM techniques.


## CHAPTER 1: THE PROJECT GOAL

The goal of this Project is to work with the Marketing department to achieve customer retention by using the following strategy (specifically rewards programs), an advanced feature proven to generate repeat business if executed well.

My mission as a Data analyst on this project is to check if the data supports the Marketing department’s hypothesis about the existence of customers that would be especially interested in the perks they are proposing, Then for each customer segment, I am to assign a likely favorite perk.
Listed Below are the Perks.

1. Free hotel meal
2. Free checked bag
3. No cancellation fees
4. Exclusive discounts
5. 1-night free hotel with a flight

My Analysis must show what kind of travel behavior indicates affinity to each perk. For example, what kind of customer could be especially interested in a free checked bag?
Which fields in the database contain information about these behaviors? How should the data be set up (e.g., filtered, aggregated) to avoid a logically flawed segmentation analysis?


## CHAPTER 2: EXPLORE THE TABLE & DATASETS

TravelTide stores it's data in a relational database, which I  access by Connecting to the TravelTide database.
This database comprises four tables: Users, Sessions, Flights, and Hotels.
In the following sections, I explored and provided snapshots of each table that queried using the SQL Select statement, and also provided the Query.


### USERS TABLE

This table displays demographic information for users, encompassing 11 columns and approximately 50,000 user records.I employed Data Definition Language (SELECT) to present a subset of columns and records from this table. See Query below

    Select u.user_id,u.birthdate,u.gender,
    u.married,u.has_children,u.home_country,
    u.home_city,u.home_airport 
    From users As u;
 
![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/7dca6ae2-b74b-4483-926f-8acdefbdc3d8)

### SESSION TABLE:
This table presents data on individual browsing sessions, specifically including sessions with a minimum of 2 clicks. It comprises 13 columns, and it has recorded over 50,000 records as detailed below. See Query Below

    SELECT sessions.session_id ,sessions.user_id,sessions.trip_id,sessions.flight_booked,sessions.hotel_booked,sessions.cancellation  
    FROM public.sessions;

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/97a5b723-e804-4482-98b4-bb94d51a4015)



### FLIGHT TABLE: 
This table captures the information about purchased flights, This table contains 13 columns and more than 50,000 records were captured as Below.See Query Below

    SQL Query All Column from Flights Table
    SELECT * FROM public.flights;

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/ea9dccd0-9cfe-4729-9ee2-1639026f66c6)


### HOTELS TABLE 
    This table records details of hotel stays that were purchased. It consists of 6 columns and has documented over 50,000 records, as described below.
    SELECT * FROM public.hotels;


## CHAPTER 3: EXPLORATORY ANALYSIS/ DATA AGGREGATION
In this Chapter, I performed some Exploratory Analysis and aggregation on the Dataset to see the performance of the Users.
1. What are the Top Airlines with the most customers and Total count of uncancelled Trips?
2. How many customers Booked both flights and a hotel, How Many customers booked only either of the two?
3. How many users Canceled or Didn’t cancel their flight Booking?
4. How many Users Brought in Checked Bags, or How many Checked Bags were brought by each Customer and Customer with No Check Bag?
5. How many Users are consistent with the hotel, How many rooms and what’s the total amount spent By Each of the Customers?


### Q1: What are the Top Airlines with the most customers and Total count of uncancelled Trips? See the Query Below

    select flights.trip_airline,count(Distinct flights.trip_id) As Total_count__of_trips,
    count(Distinct users.user_id) As No_of_customer
    From public.users 
    Join public.sessions 
    On sessions.user_id = users.user_id
    Join public.flights
    On flights.trip_id = sessions.trip_id
    where sessions.cancellation=false
    Group by flights.trip_airline
    order by count(Distinct users.user_id) desc;

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/66a86f7a-7aca-4f7b-85dd-5e651121a4e9)


### Q2. How many customers Booked both flights and a hotel, How Many customers booked only either of the two? See query below

    Select count(Distinct users.user_id) As Total_customer,sessions.flight_booked,
    sessions.hotel_booked  
    From public.users 
    Join public.sessions 
    On sessions.user_id = users.user_id
    Join public.flights
    On flights.trip_id = sessions.trip_id
    where sessions.cancellation =false
    Group by sessions.flight_booked,sessions.hotel_booked 
    Order by count(Distinct users.user_id)Desc ;

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/76db1745-ab0d-4979-8d62-f3ccecba2138)


### Q3. How many users Canceled or Didn’t cancel their flight Booking? See Query Below

    Select sessions.cancellation,count(Distinct users.user_id) As no_of_customer,
    count(Distinct flights.trip_id) As no_trip
    From public.users 
    Join public.sessions 
    On sessions.user_id = users.user_id
    Join public.flights
    On flights.trip_id = sessions.trip_id
    Group by sessions.cancellation
    order by sessions.cancellation ASC;

 ![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/2c4a1a07-c909-49ed-b977-6b14d605aede)


### Q4: How many users Canceled or Didn’t cancel their flight Booking?

The table contains the total count of unique users. The query results reveal that 1,901,038 customers chose not to cancel their trips, while 89,344 decided to cancel. To better understand the reasons for cancellations, further investigation is required.
For this project's objective of enhancing customer retention, the focus shifts to understanding users' baggage preferences. Specifically, we aim to determine how many users opted for checked bags and how many checked bags each customer brought. The table below illustrates that 584,473 customers opted not to bring any checked bags, while 565,580 had one checked bag, and 89,669 brought two checked bags. See query Below

    Select count(Distinct users.user_id) As no_of_customer, flights.checked_bags
    From public.users 
    Join public.sessions 
    On sessions.user_id = users.user_id
    Join public.flights
    On flights.trip_id = sessions.trip_id
    where sessions.cancellation =false
    Group by flights.checked_bags
    order by flights.checked_bags ASC;


### Q5: How many Users are consistent with the hotel, How many rooms, and what’s the total amount spent By Each of the Customers

This query identifies customers who consistently book both flights and hotels, displaying the number of trips, rooms booked, and the number of nights spent by each of these customers. It also calculates the total amount spent by each customer. The goal is to recognize and reward customers who demonstrate loyalty to the airline, See the Query Below

    Select *
    From (select users.user_id,count(hotels.trip_id) As No_trip,count(hotels.nights) As Total_night_spent,
    count(hotels.rooms) As No_rooms_booked,sum(hotels.hotel_per_room_usd) As Total_amt_paid 
    From public.users 
    Join public.sessions 
    On sessions.user_id = users.user_id
    Join public.flights
    On flights.trip_id = sessions.trip_id
    Join public.hotels
    On hotels.trip_id = flights.trip_id
    Group by users.user_id) As sub
    where  sub.Total_amt_paid > (select avg(hotels.hotel_per_room_usd) 
    From hotels) and sub.No_rooms_booked > 3
    order by sub.Total_amt_paid desc;

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/59e8be6c-3c54-4a5a-a847-2c1a1cad82ad)


### Q6: Identify the highest-performing customers based on the total number of trips, the total number of seats booked, the total amount charged, and the total discount applied by each customer.

Offering rewards based on the number of seats paid for can encourage customer loyalty. so here I segmented the customers based on their No of trips and seats and the Total amount of base fare Booked, filtering them based on customers that didn’t cancel their flights. See the Query Below

    select users.user_id,count(flights.trip_id) As No_trip,max(flights.seats)As No_seat,
    sum(flights.base_fare_usd) As Amount_charged  
    From public.users 
    Join public.sessions 
    On sessions.user_id = users.user_id
    Join public.flights
    On flights.trip_id = sessions.trip_id
    where sessions.cancellation = False 
    Group by users.user_id 
    order by max(flights.seats) desc
    limit 250;

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/a707caa5-1eb9-49fe-b226-57cbf71d48a1)


## CHAPTER 4: RFM ANALYSIS

In this chapter, I made calculations related to the business context and then segmented customer behavior data with statistics using (RFM ANALYSIS- Recency, Frequency, Monetary ) and visual techniques using Tableau

Based on the Business context, customers are to be segmented based on their behavior using the data provided and assigned the Perk that best suits each customer, To achieve this properly I adopted the RFM ANALYSIS 
PERKS:

1. Free hotel meal 
2. Free checked bag
3. No cancellation fees 
4. Exclusive discounts 
5. 1-night free hotel with a flight

RFM analysis is a marketing technique used for customer segmentation based on their past behavior. It is a data-driven approach that helps businesses understand and categorize their customers into different groups to tailor marketing strategies more effectively. RFM stands for Recency, Frequency, and Monetary.

* Recency (R): Recency measures how recently a customer has engaged with your product or service. It answers the question: "When was the last time the customer made a purchase or interacted with us?" Customers who have engaged more recently are often considered more valuable.
  
* Frequency (F): Frequency measures how often a customer engages with your product or service. It answers the question: "How frequently does the customer make purchases or interact with us?" Customers who engage more often may be more loyal.
  
* Monetary (M): Monetary measures how much money a customer has spent on your product or service. It answers the question: "How much revenue or value has the customer generated?" Customers who have spent more tend to be more profitable. But Before we dive deeper let’s explore the Customer’s behaviour Habit.

### TraveTide Customer Flight Boarding Habits

The below diagram shows the TravelTide customers' Boarding Habits, ranging from the last time a customer boarded, the Total Number of trips per customer, the number of check bags, the base fare Amount per customer, and the Total seats booked per customer. This was calculated based Using DAX

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/074cfbe5-d590-43a8-84ff-f1570a3253ff)


### Calculating Recency, Frequency, and Monetary

To calculate Recency, I use the "DATEDIFF" function to find the time difference between today's date and the last date Each customer boarded a flight. Recency is often used in customer analytics to measure how recently a customer has engaged with a product or service

* The Frequency was obtained by calculating the Total Trip count for each customer using a Measure called (Distinct count).

* The Monetary was calculated by summing up the Base fare Amount for the Total trip for Each customer.

      The columns used for the RFM include (Flight.Depature_Time, Flight.trip_id, Flight.Base_Fare_Amount, Hotel.trip_id, Hotel.check_in_time, Hotel. Amount_per_room, Check bags, Nights       spent, Session.Cancellation. Afterward.

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/2d3ece23-df45-42a0-987f-af087ae0b1f8)


### Calculation of RFM Scores: 

RFM Scores mean Assigning numerical scores to each customer based on recency, frequency, and monetary value. For this Project, I assigned scores ranging from 1 to 5, with 5 indicating the highest recency, frequency, or monetary value.
See the Analysis Below, Showing the segmentation of users based on their Recency, Frequency, and Monetary Scores, Filtering Each Segment by the number of check bags. 

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/097f5554-5c9b-4035-8855-4ae4964e6c83)

### Targeted Marketing
The Customers were Segmented into five different groups, this was achieved by Concatenating the RFM Scores, Below is the Segment.

1. High Recency, High Frequency, High Monetary Value: "High Performing customer "
2. Average Recency, Average Frequency, Average Monetary Value: "Average performing customer"
3. Low Recency, Low Frequency, Low Monetary Value: "Low Performing Customers"
4. Very low Recency, Very Low Frequency, Very High Monetary Value: "Almost Lost customer”
5. Extremely low Recency, Extremely Low Frequency, Extremely High Monetary Value: 'Lost customer'

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/5147718f-7e72-45ac-a9fb-d5f037009258)


### Conclusion 
 Having carried out the Segmentation, the Final step is to tailor marketing strategies and campaigns for each customer segment based on the RFM Aggregated score. For this project, I offered the listed Perks as follows.
 1. High-performing customers To Exclusive Discounts
 2. Average performing customers To 1-night free hotel with a flight
 3. Low-performing customers To  Free hotel meal
 4. Almost Lost customer  To 'Free checked bag
 5. Lost customer To cancellation fees' 

![image](https://github.com/Bumzeal/Customer-Segmentation-and-Perk-Allocation-Analysis/assets/78567274/ce4f59c2-b227-4535-8f3e-c9950934e4b7)






















































