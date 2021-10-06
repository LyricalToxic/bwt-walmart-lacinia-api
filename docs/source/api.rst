API
===

.. autosummary::
   :toctree: generated

Search
-----------
Overview
~~~~~~~~~~~

    Запрос на поиск товаров выполняется на endpoint::

        https://www.walmart.com/orchestra/home/graphql

    Результаты поиска разбиваются на пагинации. Для каждого набора фильтров можно получить не более 1000 товаров.

.. include:: _queries/search_query.rst
