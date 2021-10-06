Search
-----------
Overview
~~~~~~~~~~~

Запрос на поиск товаров выполняется на endpoint:

:class:`https://www.walmart.com/orchestra/home/graphql`

Результаты поиска разбиваются на пагинации. Для каждого набора фильтров можно получить не более 1000 товаров.


Body
~~~~~~~~~~~

Query

.. py:function:: query Search(...)

Запрос на поиск товаров по ключевому :doc:`слову`

:param String $query: Описание

:param  Int $page: Описание
