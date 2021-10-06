Query input
~~~~~~~~~~~
.. method:: query.Search(
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

    Запрос на поиск товаров по ключевому :doc:`слову`
