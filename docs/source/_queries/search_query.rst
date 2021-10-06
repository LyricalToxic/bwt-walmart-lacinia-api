Search
-----------
Overview
~~~~~~~~~~~

    Запрос на поиск товаров выполняется на endpoint:

        :link:`https://www.walmart.com/orchestra/home/graphql`

    Результаты поиска разбиваются на пагинации. Для каждого набора фильтров можно получить не более 1000 товаров.


Body
~~~~~~~~~~~

.. title::query
.. js:function:: query Search(...)

    Запрос на поиск товаров по ключевому :doc:`слову`

    .. js:param::$query: String

        Описание

    .. js:param:: $page: Int

        Описание

    .. js:param:: $prg: Prg!

        Описание

    .. js:param:: $facet: String

        Описание

    .. js:param:: $sort: Sort = best_match!

        Описание

    .. js:param:: $catId: String

        Описание

    .. js:param:: $max_price: String

        Описание

    .. js:param:: $min_price: String

        Описание

    .. js:param:: $spelling: Boolean = true,

        Описание

    .. js:param:: $affinityOverride: AffinityOverride

        Описание

    .. js:param:: $storeSlotBooked: String

        Описание

    .. js:param:: $ps: Int

        Описание

    .. js:param:: $ptss: String

        Описание

    .. js:param:: $recall_set: String

        Описание

    .. js:param:: $fitmentFieldParams: JSON = {}

        Описание

    .. js:param:: $fitmentSearchParams: JSON = {}

        Описание

    .. js:param:: $fetchMarquee: Boolean!

        Описание

    .. js:param:: $trsp: String

        Описание

    .. js:param:: $fetchSkyline: Boolean!

        Описание

    .. js:param:: $fetchSbaTop: Boolean!

        Описание

Body
~~~~~~~~~~~

.. title:: variables
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
