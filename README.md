# ExampleSQIQuery
create table customer(
  id serial primary key,
  name varchar(255),
  phone varchar(30),
  email varchar(255)
);
select *from customer;
create table product(
  id serial primary key,
  name varchar(255),
  description text,
  price integer
              );
select *from product;

create table product_photo(
  id serial primary key,
  url varchar(255),
  product_id integer references product(id)
  );
select *from product_photo;

create table  cart(
  customer_id integer references customer(id),
  id serial primary key
);
select *from cart;
create  table  cart_product(
  cart_id integer references cart(id),
  product_id integer references cart(id));
select *from cart_product;

insert into customer(name, phone, email) values('Василий', '02', 'vas@mail.ru');
insert into customer(name, phone, email) values('Piter Pen', '03', 'sem@gmail.com');
select *from customer;

insert into product(name, description, price) values('Iphon', 'крутой телефон', '100000');

insert into product(name, description, price) values('Samsung', 'отличный выбор', '50000');
insert into product_photo(url, product_id) values('iphone_photo', 1);
select *from product_photo;
select url from product_photo;
-- соединение 2-х таблиц с помощью left join
select pp.*, p.name from product_photo pp left join product p on p.id=pp.product_id;
-- добавление фотографии на не существующий продукт
-- для этого удаляем Foreign key следуюим образом
alter table product_photo drop constraint product_photo_product_id_fkey;
-- просмотр товаров
select *from product;
-- создадим фото на не существующий продукт
insert into product_photo (url, product_id) values ('unknown_photo', 100);
select *from product_photo;
-- как отработает запрос с left join
select pp.*, p.name from product_photo pp left join product p on p.id=pp.product_id;
-- просмотр работы right join
select pp.*, p.name from product_photo pp right join product p on p.id=pp.product_id;
--просмотр работы inner join
select pp.*, p.name from product_photo pp inner join product p on p.id=pp.product_id;
-- удалим фото, ссылающуюся на не существующий продукт
select *from product_photo;
delete from product_photo where id=3;
select *from product_photo;
--обновить url в фотографии
update product_photo set url='iphone_image_2' where id=1;
-- создание заказа для василия
insert into cart(customer_id) values(1);
select *from cart;
select *from product;
--добавление в корзину
insert into cart_product (cart_id, product_id) values (1, 1), (1, 2);
-- просмотр
select *from cart_product;
-- достать имена клиентов с общей суммо их заказов
select*from customer;
--достать первую колонку
select c.name from customer c;
--приджонить к табличке клиента табличку карты
select c.name from customer c left join cart on cart.customer_id=c.id;
--добавление в выходное поле id карты идентификатор карты
select c.name, cart.id as card_id from customer c left join cart on cart.customer_id=c.id;

select c.name, cart.id as card_id, cp.product_id from customer c left join cart on cart.customer_id=c.id left join
  cart_product cp on cp.cart_id=cart_id;
