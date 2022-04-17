create database store;
use store;

create table countries(
	code int primary key,
    name varchar(20) not null,
    continent_name varchar(20) not null
);

create table users(
	id int primary key,
	full_name varchar(20),
    email varchar(20) unique,
    gender varchar(1) check (gender='m' or gender='f'),
    date_of_birth varchar(15),
    created_at datetime default CURRENT_TIMESTAMP,
    country_code int,
    foreign key(country_code) references countries(code)
);

create table orders(
	id int primary key,
	user_id int,
    status varchar(6) check (status='start' or status='finish'),
    created_at datetime default CURRENT_TIMESTAMP,
    foreign key(user_id) references users(id)
);

create table products(
	id int primary key,
    name varchar(10) not null,
    price int default 0,
    status varchar(10) check (status='valid' or status='expired'),
    created_at datetime default CURRENT_TIMESTAMP
);

create table order_products(
	order_id int ,
    product_id int,
    quantity int default 0,
    primary key (order_id, product_id),
    foreign key(order_id) references orders(id),
    foreign key(product_id) references products(id)
);

insert into countries values (669, 'Saudi Arabia', 'Asia');

insert into users values (1, 'Maha AlGhamdi', 'Maha@mail.com', 'f', '1995-8-12', CURRENT_TIMESTAMP, 669);

insert into orders values (1,1, 'start', CURRENT_TIMESTAMP);

insert into products values (1, 'product', 897, 'valid', CURRENT_TIMESTAMP);

insert into order_products values (1,1,1);

update countries set name='KSA' where code=669;

/* An error comes when we want to delete a row in a products table because containing a foreign key in another table in our case here is the order_products table. 
To solve this, the row in the order_products table must be deleted and then deleted from here..*/
delete from order_products where product_id=1;
delete from products where id=1;
