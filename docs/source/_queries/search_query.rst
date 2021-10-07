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

    :parameter String $query: \

        Описание \

    :parameter Int $page: \

        Описание \

    :parameter Prg $prg!: \

        Описание \

    :parameter String $facet: \

        Описание \

    :parameter Sort $sort: \

        Default = best_match \

        Описание \

    :parameter String $catId: \

        Описание \

    :parameter String $max_price: \

        Описание \

    :parameter String $min_price: \

        Описание \

    :parameter Boolean $spelling: \

        Default = true \

        Описание \

    :parameter AffinityOverride $affinityOverride: \

        Описание \

    :parameter String $storeSlotBooked: \

        Описание \

    :parameter Int $ps: \

        Описание \

    :parameter String $ptss: \

        Описание \

    :parameter String $recall_set: \

        Описание \

    :parameter JSON $fitmentFieldParams: \

        Default = {} \

        Описание \

    :parameter JSON $fitmentSearchParams: \

        Default = {} \

        Описание \

    :parameter Boolean $fetchMarquee!: \

        Описание \

    :parameter String $trsp: \

        Описание \

    :parameter Boolean $fetchSkyline!: \

        Описание \

    :parameter Boolean $fetchSbaTop!: \

        Описание \

    :parameter JSON $additionalQueryParams: \

        Default = {} \

        Описание \


Process finished with exit code 0




Запрос на поиск товаров по ключевому :doc:`слову`


