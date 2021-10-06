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

.. function:: query_Search(...)

Запрос на поиск товаров по ключевому :doc:`слову`

.. param::$query: String

Описание

.. param:: $page: Int

        Описание

    .. param:: $prg: Prg!

        Описание

    .. paramparam:: $facet: String

        Описание

    .. paramparam:: $sort: Sort = best_match!

        Описание

    .. paramparam:: $catId: String

        Описание

    .. paramparam:: $max_price: String

        Описание

    .. paramparam:: $min_price: String

        Описание

    .. paramparam:: $spelling: Boolean = true,

        Описание

    .. paramparam:: $affinityOverride: AffinityOverride

        Описание

    .. paramparam:: $storeSlotBooked: String

        Описание

    .. paramparam:: $ps: Int

        Описание

    .. paramparam:: $ptss: String

        Описание

    .. paramparam:: $recall_set: String

        Описание

    .. paramparam:: $fitmentFieldParams: JSON = {}

        Описание

    .. paramparam:: $fitmentSearchParams: JSON = {}

        Описание

    .. paramparam:: $fetchMarquee: Boolean!

        Описание

    .. paramparam:: $trsp: String

        Описание

    .. paramparam:: $fetchSkyline: Boolean!

        Описание

    .. paramparam:: $fetchSbaTop: Boolean!

        Описание

Variables
.. code-block:: json
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
