# Aircargo_SQL
identifying the regular customers to provide offers, analyze the busiest route which helps to increase the number of aircraft required and prepare an analysis to determine the ticket sales details. This will ensure that the company improves its operability and becomes more customer-centric and a favorable choice for air travel. 

Create Database Aircargo;
Show Databases;
Use Aircargo;
Create table if not exists customer(
customer_id int not null auto_increment primary key;
first_name VARCHAR(20) not null,
last_name VARCHAR(20) not null,
Date_of_Birth date not null,
gender char(1) not null
);
Describe customer;
load data local infile 'C:\Program Files\MYSQL\MYSQL Workbence 8.0\data\datasets\customer.csv'
Into table customer 
fields terminated by ',' enclosed by '"' lines terminated by'\n' ignore 1 rows;
SELECT * FROM customer;
Create table if not exists roures( 
route_id int not null unique primary key,  
flight_null constraint chk_1 check (flight_num is not null),
origin_airport char(3) not null,
destination_airport char(3) not null,
aircraft_id VARCHAR(10) not null,
distance_miles int not null constraint check_2 check (distance_miles > 0)
);
describe routes; 
load data local infile 'C:\Program Files\MYSQL\MY SQL Workbench 8.0\data\datasets\routes.csv' 
into table routes 
fields terminated by ',' enclosed by  '"' lines terminated by '/n' ignore 1 rows;
select * from routes;
create table if not exists pof (
pof_id int auto_increment primary key,
customer_id int not null,
aircraft_id varchar(10) not null,
route_id int not null,
depart char(3) not null,
arrival char(3) not null,
seat_num char(4) not null,
class_id varchar(15) not null,
travel_date date not null,
flight_num int not null,
constraint fk_pof foreign key (customer_id) references customer (customer_id) 
);
describe pof;
load data local infile 'C:\Program Files\MYSQL\MY SQL Workbench 8.0\data\datasets\passengers_on_flights.csv'
into table routes 
fields terminated by ',' enclosed by '"' lines terminated by '\n' ignore 1 rows;
select * from pof;
create table if not exists ticket_details(
tkt_id int auto_increment primary key,
P_date date int not null,
customer_id int not null,
aircraft_id varchar(10) not null, 
class_id varchar(15) not null,
no_of_tkts int not null,
a_code char(3) not null,
price_per_tkt decimal(5,2) not null,
brand varchar(30) not null,
constraint fk_tkt_dts foreign key (customer_id) references customer(customer_id) 
);
describe ticket_details;
load data local infile 'C:\Program Files\MYSQL\MYSQL Workbench 8.0\data\datasets\ticket_details.csv'
into table ticket_details
fields terminated by ',' enclosed by '"' lines terminated by '\n' ignore 1 rows;
select * from ticket details; 
fields terminated by ',' enclosed by '"' lines terminated '\n' ignore 1 rows;
select * from ticket details;
select * from customer where customer_id in (select distinct customer_id from pof where route_id between 1 and 25) order by customer_id;
select * from customer where customer_id in (select distinct customer_id from pof where route_id between 1 and 25) order by customer_id;
select count(distinct customer_id) as num_passengers, sum(no_of_tkts * price_per_tkt) as total_revenue from ticket_details where class_id = 'Business' 
select concat(first_name, "",last_name) as full_name from customer);
select first_name, last_name from customer where customer_id in (select distinct b.customer_id from customer a, ticket_details b); 
select first_name, last_name from customer where customer_id in (select distinct customer_id from ticket_details where brand = 'Emirates');
select class_id, count(distinct customer_id) as num_passengers from pof group by class_id having class_id ='Economy Plus';
select * from customer a  
inner join (select distinct customer_id from pof where class_id = 'Economy Plus') b 
on a.customer_id = b.customer_id;
inner join (select distinct customer_id from pof where class_id = 'Economy Plus') b 
on a.customer_id = b.customer_id;
select if((select sum(no_of_tkts * price_per_tkt) as total_revenue from ticket_details > 10000, 'Crossed 10K', 'Not Crossed 10K') as revenue_check;
create user if not exists 'raghuram'@'123.0.01' identified by 'password123';
grant all privileges on aircargo to raghuram@123.0.01;
create user if not exists 'raghuram'@'123.0.01' identified by 'password123';
grant all privileges on aircargo to raghuram@123.0.01;
select class_id, max(price_per_tkt) from ticket_details group by class_id;
select distinct class_id, max(price_per_tkt) over (partition by class_id) as max_price from ticket_details order by max_price;
select class_id, max(price_per_tkt) from ticket_details group by class_id;
select distinct class_id, max(price_per_tkt) over (partition by class_id) as max_price from ticket_details order by max_price;
explain select * from pof where route_id = 4; 
select distinct class_id, max(price_per_tkt) over (partition by class_id) as max_price from ticket_details order by max_price;
explain select * from pof where route_id = 4;
create index idx_rid on pof (route_id);
explain select * from pof where route_id = 4;
explain select * from pof where route_id = 4; 
create index idx_rid on pof (route_id);
explain select * from pof where route_id =4; 
select customer_id, aircraft_id, sum(price_per_tkt * no_of_tkts) as total_price from ticket_details group by customer_id, aircraft-id order by customer_id, aircraft_id;
explain select * from pof where route_id = 4;
select customer_id, aircraft_id, sum(price_per_tkt) as total_price from ticket_details group by customer_id, aircraft_id with rollup order by customer_id, aircraft_id;
create view buss_class_customers as 
select a. *, b.brand from customer a 
inner join select distinct customer_id, brand from ticket_details where class_id = 'Business' order by customer_id) b 
on a.customer_id = on b.customer_id;
select * from buss_class_customers; 
select * from customer where customer_id in (select distinct customer_id from pof where route_id in (1,5));
delimiter // 
create procedure check_route (in rid varchar(255))
declare TableNotFound condition for 1146 ; 
declare exist handler for TableNotFound 
select 'Please check if table customer/route id are created - one/both are missing' Message; 
set @query = concat('select * from customer where customer_id in (select distinct customer_id from pof where route_id in (',rid,'));'); 
prepare sql_query from @query;
execute sql_query;
end//
delimiter ;
call check_route("1,5");
delimiter //
create procedure check_dist()
begin
select * from routes where distance_miles > 2000;
end // 
delimiter ;
call check_dist;
select flight_num, distance_miles, case
                                           when distance_miles between 0 and 2000 then "STD" 
                                           when distance_miles between 2001 and 6500 then "IDT'
										   else "LDT"
				                       end distance_category from routes;
prepare sql_query from @query;
execute sql_query ;
end //
delimiter ;
call cehck_route ("1,5") 
select flight_num, distance_miles, case 
                                          when distance_miles between 0 and 2000 then "STD" 
                                          when distance_miles between 2001 and 6500 then "IDT" 
                                          else "LDT"
                                          end distance_category from routes;
delimiter // 
create function group_dist(dist int)
returns varchar(10)
deterministic 
begin
declare dist_cat char(3);
if dist between 0 and 2000 then 
set dist_cat = 'IDT';
elseif dist between 2001 and 6500 then 
    set dist_cat = 'IDT';
    elseif dist > 6500 then
    set dist_cat = 'LDT';
     end if;
     return (dist_cat); 
end //
create procedure group_dist proc()
begin
select flight_num, distance_miles, group_dist(distance_miles) as distance_category from routes;
end //
delimiter ;
call group_dist_proc();
create procedure group_dist_proc()
begin
select flight_num, distance_miles, group_dist(distance_miles) as distance_category from routes;
end //
delimiter ;
call gruop_dist_proc();
select p_date, customer_id, class_id, case 
									   when class_id in ('Business', 'Economy Plus') then "Yes" 
                                          else "No" 
                                          end as complimentary_Service from ticket_details;
delimiter // 
create function check_comp_serv(clsvarchar(15))
return char(3)
deterministic 
begin
declare comp_ser char(3);
if cls in ('Business', 'Economy Plus') then
  set comp_ser = "Yes" ;
else
  set comp_ser = "No";
  end if;
  return (comp_ser);
  end // 
  create procedure check_comp_serv_proc()
  begin
  select p_date, customer_id, class_id, check_comp_serv(class_id) as complimentary_services from ticket_details;
  end //
  delimiter ;
  call check_comp_serv_proc();
  delimiter ;
  call check_comp_serv_proc();
  select * from customer where last_name = 'Scott' limit 1;
  delimiter ;
  call check_comp_serv_proc();
  select * from customer where last_name = 'Scott' limit 1;
  delimiter //
  create procedure cust_1name_scott()
  begin 
  declare c_id int;
  declare f_name varchar(20);
  declare l_name varchar(20);
  declare dob date;
  declare gen char(1);
  declare cust_rec cursor 
  for
  select * from customer where last_name ='Scott';
  create table if not exists cursor_table(
                               c_id int ,
                               f_name varchar(20),
                               l_name varchar(20),
                               dob date,
                               gen char(1) 
                               ); 
     open cust_rec;
     fetch cust_rec into c_id, f_name,l_name, dob, gen;
     insert into cursor_table(c_id,f_name, l_name, dob, gen) values c_id, f_name, l_name, dob, gen);
     close cust_rec;
     select * from cursor_table ;
     end //
     delimiter ;
declare c_id int;
declare f_name varchar(20);
declare l_name varchar(20);
declare dob date; 
declare gen char(1);
declare cust_rec cursor 
for 
select * from customer where last_name ='Scott'
create table if not exists cursor_table(
                                   c_id int,
                                   f_name varchar(20),
                                   l_name varchar(20),
                                   dob date,
                                   gen char(1)
                                   );
 open cust_rec;
 fetch cust_rec into c_id, f_name, l_name, dob, gen ;
 insert into cursor_table(c_id, f_name, l_name, dob, gen) values (c_id, f_name, l_name, dob, gen);
 close cust_rec;
 select * from cursor_table;
 end//
 delimiter ;
 cell cust_lname_scott();
                                   
                                   
                                   
                               
  
  
  
    
    




                














