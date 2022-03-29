# SQL
kooli sql ül

-- ul 21.1

SELECT * FROM d110077sd465105.books;

-- ul 21.2

select * from d110077sd465105.books where release_date >= 2010 and type='new';

-- ul 21.3

select title, release_date, price, type from d110077sd465105.books where release_date < 1970 and type='used' and price < 20;

-- ul 21.4

SELECT year(order_date) as aasta, count(*) as 'tellimuste arv' FROM d110077sd465105.orders group by year(order_date);

-- ul 21.5

select year(order_date) as aasta, count(*) as 'tellimuste arv', round(sum(price),2) as 'müükide summa' from orders left join books on orders.book_id = books.id group by year(orders.order_date);

-- ul 21.6

select count(*) as 'tellimuste arv', sum(books.price) as 'müükide summa' from orders left join books on orders.book_id = books.id where year(order_date) = 2017;

-- ul 21.7

select count(*), sum(price), clients.username from clients left join orders on orders.client_id = clients.id left join books on orders.book_id = books.id where year(orders.order_date) = 2017 group by clients.id order by sum(price) desc;

-- ul 21.8

select books, title, books.price, count(*) from orders left join books on books.id = orders.book_id group by book_id order by count(*) desc limit 10;

-- ul 22.1

select round(sum(price * stock_saldo),2) as 'laoväärtus' from books;

-- ul 22.2

select round(avg(price),2), round(max(price),2), round(min(price),2) from books;

-- ul 22.3

select round(max(price),2) as 'koige kallim kasutatud raamat' from books where type='used';

-- ul 22.4

Select case when type='new' then 'uus' when type ='used' then 'kasutatud' when type='ebook' then 'e-raamat' end as Tüüp, round(avg(price),2) as keskmine_hind, count(*) as hulk from books group by type;

-- ül 22.5

select title, price, type from books where type='used' and price > (select avg(price) from books where type='new') order by price desc;

-- ül 22.6

select * from books where price > (select avg(price) from authors left join book_authors on book_authors.author_id = authors.id left join books on books.id = book_authors.book_id left join orders on  orders.book_id = books.id group by authors.id order by count(*) desc limit 1);

-- ul 22.7

select * from books where release_date % 2 = 0;

-- ul 22.8

select count(*), language from books group by language order by count(*) desc;

-- ul 23.1

insert into clients (username, first_name, last_name, gmail, password, address) values ('freppy','raiko','tomaslu','raigo@hotmail.yahoo','1123','E');

-- ul 23.2

update books set language = 'eesti' where id=1;

-- ul 23.3

delete from orders where id=2300;

-- ul 23.4

insert into clients (username, first_name, last_name, email, address) 
values
	('eesl', 'fqaa', 'kasa', 'isane@gmail.com', 'teat'),
	('ecel', 'fasa', 'kata', 'emane@gmail.com', 'teat'),
    ('evel', 'fara', 'kara', 'teane@gmail.com', 'teat'),
    ('etel', 'fava', 'kawa', 'emaane@gmail.com', 'teat');
    
-- ul 23.5

select id from books where title ='Vendetta';
select id from clients where username ='mcage1o';
insert into orders (delivery_address, order_date, status, client_id, book_id)
values ('tomatotown', '2005.05.05', 'ordered', (select id from clients where username ='mcage1o'), (select id from books where title ='Vendetta'));

-- ul 23.6

update books 
set price = price*1.05, pages = pages -5 where books.id > 0;

-- ul 23.7

delete from authors where id in (select authors.id from authors left join book_authors on authors.id = book_authors.author_id where book_authors.id is null);
