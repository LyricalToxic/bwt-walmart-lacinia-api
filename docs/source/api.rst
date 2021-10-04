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

Query input
~~~~~~~~~~~
.. js:function:: query Search(
    $query: String
    $page: Int
    $prg: Prg!
    $facet: String
    $sort: Sort = best_match
    $catId: String
    $max_price: String
    $min_price: String
    $spelling: Boolean = true
    $affinityOverride: AffinityOverride
    $storeSlotBooked: String
    $ps: Int
    $ptss: String
    $recall_set: String
    $fitmentFieldParams: JSON = {}
    $fitmentSearchParams: JSON = {}
    $fetchMarquee: Boolean!
    $trsp: String
    $fetchSkyline: Boolean!
    $fetchSbaTop: Boolean!
)


.. js:attribute:: $query: String
    описание


------------
