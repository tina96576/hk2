# homework2

create database order_biandang_system;

use order_biandang_system;


create table department(
    dp_id int primary key,
    dp_name varchar(20) not null,
    person_in_charge varchar(20) not null,
    dp_floor varchar(20) not null,
    dp_tel varchar(20) not null);


insert into department values 
(1,'personnel','a','13','923011'),
(2,'sales','b','15','850103'),
(3,'research','c','15','104567'),
(4,'accounting','d','20','526491'),
(5,'productive','e','25','625874');


create table order_biandang(
    order_id int primary key,
    dp_id int not null);


alter table order_biandang
    add constraint fk_dp_order
    foreign key (dp_id)
    references department(dp_id)
    on update cascade
    on delete cascade;


insert into order_biandang values (1,1),(2,1),(3,2),(4,3),(5,3);


create table order_detail(
    odl_id int primary key,
    order_id int not null,
    bd_id int not null,
    quantity varchar(20));


alter table order_detail
    add constraint fk_orderbd_orderdtl
    foreign key (order_id)
    references order_biandang(order_id)
    on update cascade
    on delete cascade;


insert into order_detail values (1,1,1,13),(2,2,2,2),(3,3,3,5),(4,4,3,2),(5,5,4,7);


create table biandang(
    bd_id int primary key,
    bdsp_id int not null,
    bd_name varchar(20) not null,
    bd_price int not null);


insert into biandang values
(1,1,'chicken',60),
(2,1,'pork',60),
(3,2,'fish',70),
(4,2,'beef',80),
(5,2,'pork2',65);


create table biandang_store(
    bdsp_id int primary key,
    bdsp_name varchar(20) not null,
    bdsp_tel varchar(20) not null,
    bdsp_address varchar(20) not null);


insert into biandang_store values 
(1,'good_food','551234','abcde'),
(2,'apple','7896456','fghi'),
(3,'very_good','321564','jklmn'),
(4,'eat','220145','opqrs'),
(5,'banana','635879','tuvwx');


alter table biandang
    add constraint fk_biandang_biandang_store
    foreign key (bdsp_id)
    references biandang_store (bdsp_id)
    on update cascade
    on delete cascade;




SELECT  order_id,
(select dp_name from department where dp_id in (select dp_id from order_biandang where o.order_id=order_id)) as department_name,
(select bdsp_name from biandang_store where bg.bdsp_id=bdsp_id) as biandang_store_name, 
bd_name,bd_price,quantity,(bd_price*quantity) as total
FROM order_detail as o join biandang as bg on o.bd_id=bg.bd_id
ORDER BY order_id
