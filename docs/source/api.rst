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
.. js:method:: query Search(...)

    Запрос на поиск товаров по ключевому :doc:`слову`

    .. js:attribute::$query: String

        Описание

    .. js:attribute:: $page: Int

        Описание

    .. js:attribute:: $prg: Prg!

        Описание

    .. js:attribute:: $facet: String

        Описание

    .. js:attribute:: $sort: Sort = best_match!

        Описание

    .. js:attribute:: $catId: String

        Описание

    .. js:attribute:: $max_price: String

        Описание

    .. js:attribute:: $min_price: String

        Описание

    .. js:attribute:: $spelling: Boolean = true,

        Описание

    .. js:attribute:: $affinityOverride: AffinityOverride

        Описание

    .. js:attribute:: $storeSlotBooked: String

        Описание

    .. js:attribute:: $ps: Int

        Описание

    .. js:attribute:: $ptss: String

        Описание

    .. js:attribute:: $recall_set: String

        Описание

    .. js:attribute:: $fitmentFieldParams: JSON = {}

        Описание

    .. js:attribute:: $fitmentSearchParams: JSON = {}

        Описание

    .. js:attribute:: $fetchMarquee: Boolean!

        Описание

    .. js:attribute:: $trsp: String

        Описание

    .. js:attribute:: $fetchSkyline: Boolean!

        Описание

    .. js:attribute:: $fetchSbaTop: Boolean!

        Описание



.. js:attribute:: $query: String
    описание

.. include:: _queries/search_query.rst
