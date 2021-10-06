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

.. function:: query Search(...)

Запрос на поиск товаров по ключевому :doc:`слову`

:param$query: String

Описание

:param $page: Int

Описание

:param $prg: Prg!

Описание

:param $facet: String

Описание

:param $sort: Sort = best_match!

Описание

:param $catId: String

Описание

:param $max_price: String

Описание

:param $min_price: String

Описание

:param $spelling: Boolean = true,

Описание

:param $affinityOverride: AffinityOverride

Описание

:param $storeSlotBooked: String

Описание

:param $ps: Int

Описание

:param $ptss: String

Описание

:param $recall_set: String

Описание

:param $fitmentFieldParams: JSON = {}

Описание

:param $fitmentSearchParams: JSON = {}

Описание

:param $fetchMarquee: Boolean!

Описание

:param $trsp: String

Описание

:param $fetchSkyline: Boolean!

Описание

:param $fetchSbaTop: Boolean!

Описание

Variables

.. code:: json
    "variables": {
        "id": "",
        "dealsId": "",
        "query": "Weston",
        "page": 1,
        "prg": "desktop",
        "catId": "",
        "facet": "",
        "sort": "best_match",
        "rawFacet": "",
        "seoPath": "",
        "ps": 40,
        "ptss": "",
        "trsp": "",
        "beShelfId": "",
        "recall_set": "",
        "module_search": "",
        "min_price": "",
        "max_price": "",
        "storeSlotBooked": "",
        "additionalQueryParams": null,
        "fitmentFieldParams": null,
        "fitmentSearchParams": {
          "id": "",
          "dealsId": "",
          "query": "Weston",
          "page": 1,
          "prg": "desktop",
          "catId": "",
          "facet": "",
          "sort": "best_match",
          "rawFacet": "",
          "seoPath": "",
          "ps": 40,
          "ptss": "",
          "trsp": "",
          "beShelfId": "",
          "recall_set": "",
          "module_search": "",
          "min_price": "",
          "max_price": "",
          "storeSlotBooked": "",
          "additionalQueryParams": null,
          "cat_id": "",
          "_be_shelf_id": ""
        },
        "fetchMarquee": true,
        "fetchSkyline": true,
        "fetchSbaTop": true
      }
