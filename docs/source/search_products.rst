Поиск (Search)
----------------

Обзор (Overview)
~~~~~~~~~~~~~~~~~~

..
    Overview для каждого запроса должно содержать:

        1. Предназначение.
        2. HTTP метод и endpoint.
        3. Описание свойств запроса.
        4. Описание ответа.
        5. Особенности.

.. Предназначение

Запрос предназначен для поиска товаров по ключевому слову. \

.. admonition:: Schema reference
    :class: info

    Description: Search Query - fetches search results from SIS(search interface service), layout & modules from Content Layout Service. \

.. HTTP метод и endpoint.

Поиск на сайте walmart выполняется с помощью `post` запроса на endpoint:
::

    https://www.walmart.com/orchestra/home/graphql/search

.. Описание свойств запроса.

Помимо ключевого слова, в запросе можно указать различные фильтры и сортировку, что в свою очередь будет влиять на количество результатов.
Также локация влияет на количество результатов.

.. Описание ответа.

Результатом поиска является информация о продуктах, рекламе и метаданных.
Продукты разбиваются на пагинации по N штук на K страниц.
Для каждого набора фильтров можно получить не более 1000 товаров, то есть при 40 товарах на странице будет 25 пагинаций.

.. Особенности

Примечание:

- размер пагинации всегда 40, в независимости от указанного размера
- при размере пагинации 40 может приходить меньше 40 товаров
- на пагинациях есть повторяющиеся товары
- при запросе на страницы следующие за 25, в ответе могут содержаться продукты. Они будут дубликатами
- сортировка по цене работает некорректно. Результаты сортируются в пределах одной пагинации

Тело (Body)
~~~~~~~~~~~~~

Запрос (Query)
""""""""""""""""

.. function:: search(...)

    :parameter AffinityOverride $affinityOverride: \

        .. admonition:: Schema reference
            :class: info

            Description: Affinity Override. \

        Влияет на результат.

        Перечисление может принимать значения::

            ["default", "default_fc", "store_only", "store_led"]

    :parameter Boolean $displayGuidedNav: \

        .. admonition:: Schema reference
            :class: info

            Description: Display Guided Navigation Section in Response. \

    :parameter String $storeSlotBooked: \

        Относится к seoTopicData.

    :parameter String $stores: \

        .. admonition:: Schema reference
            :class: info

            Description: The Customer Store. If no value is passed, it will attempt to default to the value from location service. \

        Уникальный идентификатор магазина относительно которого будет выполняться поиск.

    :parameter Prg $prg: \

        .. admonition:: Schema reference
            :class: info

            Description: Helps to identify device type. \
            Possible values: desktop, mWeb, ios, android. \

        Тип устройства. На результат поиска не влияет. Нужен для contentLayout. \

    :parameter String $zipcode: \

        .. admonition:: Schema reference
            :class: info

            Description: Customer ZIP Code. If no value is passed, it will attempt to default to the value from location service. \
            Eg: 94087. \

        Зип код пользователя. Предположительно нужен для релевантной рекламы. \

    :parameter String $stateOrProvinceCode: \

        .. admonition:: Schema reference
            :class: info

            Description: State or Province Code. If no value is passed, it will attempt to default to the value from location service. \
            Eg: CA. \

        Код штата или провинция пользователя. Предположительно нужен для релевантной рекламы. \

    :parameter Boolean $guided_nav: \

        .. admonition:: Schema reference
            :class: info

            Description: Guided Nav param to indicate guided navigation is set to true. \

    :parameter Int $pos: \

        .. admonition:: Schema reference
            :class: info

            Description: Guided Nav param to indicate the position of the guided nav pill. \
            Eg: 1. \

    :parameter String $s_type: \

        .. admonition:: Schema reference
            :class: info

            Description: Guided Nav param to indicate the type of the guided nav pill. \
            Eg: ref. \

    :parameter String $src_query: \

        .. admonition:: Schema reference
            :class: info

            Description: Guided Nav param to indicate the source / parent query. \
            Eg: tv. \

    :parameter String $query: \

        .. admonition:: Schema reference
            :class: info

            Description: Search query. \
            Eg: tv. \

        Ключевое слово для поиска. \

        Может быть пустым. В таком случае результатом поиска будет 15000 результатов соответствующим сортировки. \

    :parameter String $cat_Id: \

        .. admonition:: Schema reference
            :class: info

            Description: Category Id.\
            Eg: 4044. \

        Уникальный идентификатор категорий. \

        Если поле указано, то в результате выдачи попадут товары только относящиеся к конечной категории

        Форма записи::

            главная категория_под категория_...._конечная категория

            Например: 1229749_1086045_9412206_8443517_3254837

        Уникальные идентификаторы содержаться в ответе. \

    :parameter String $_be_shelf_id: \

        .. admonition:: Schema reference
            :class: info

            Description: Manual shelf id. \
            Eg: 7778. \

        Относиться к seoBrowseMetaData. \

    :parameter String $facet: \

        .. admonition:: Schema reference
            :class: info

            Description: selected facets. For manual shelf FE sends the shelf id as separate query param and as part of facet as well. Example: https://www.walmart.com/browse/all-apple-ipad/0/0/?_refineresult=true&_be_shelf_id=7780&search_sort=100&facet=shelf_id:7780 . \

        Фильтры поиска. \

        Форма записи::

            тип фильтра:значение||тип фильтра:значение ...

            Например: fulfillment_method:Delivery||brand:Cra-Z-Art

        Значение фильтров находятся в ответе.

    :parameter Int $page: \

        .. admonition:: Schema reference
            :class: info

            Description: This determines the page selected by customer. \

        Порядковый номер страницы пагинации. \

        При положительных значениях возвращает результат поиска для указанной страницы, если она существует.
        В противном случае результат возвращен не будет и количество результатов будет равно 0. \

        При 40 товаров на пагинации максимальное значение страницы 25. \

    :parameter Int $ps: \

        .. admonition:: Schema reference
            :class: info

            Description: The number of items per page. \

        Количество товаров на пагинации. \

        Фактически не влияет на размер пагинации. Всегда будет приходить не более 40 товаров на страницу. \

        Но при разных значениях ps будут приходить разные товары.

    :parameter String $max_price: \

        .. admonition:: Schema reference
            :class: info

            Description: Max price entered by customer. \

        Максимальная цена продукта. \

        Скорее всего цена, на стороне сервера, парсится из строки в числовое значение.
        Если распарсить не удалось, то при выдаче поисковый движок будет считать что цена равна 0. \

        Максимальная цена не может быть:

        - меньше минимальной

        - дробной

        При достаточно большом значении цены(значение больше чем наибольшая цена из результатов) и отсутствии значение " ", количество результатов будет отличаться.
        В основном при отсутствии значения количество результатов будет больше. \

        .. admonition:: Attention
            :class: attention

            Этот параметр не гарантирует, что в поисковой выдаче не будет товара с ценой выше чем указано. \

    :parameter String $min_price: \

        .. admonition:: Schema reference
            :class: info

            Description: Min price entered by customer. \

        Минимальная цена продукта. \

        Скорее всего цена, на стороне сервера, парсится из строки в числовое значение.
        Если распарсить не удалось, то при выдаче поисковый движок будет считать что цена равна 0. \

        Минимальная цена не может быть:

        - больше максимальной

        - дробной

        При значении цены "0" и отсутствии значение " ", количество результатов будет отличаться.
        В основном при отсутствии значения количество результатов будет больше. \

        .. admonition:: Attention
            :class: attention

            Этот параметр не гарантирует, что в поисковой выдаче не будет товара с ценой ниже чем указано. \

    :parameter Sort $sort: \

        .. admonition:: Schema reference
            :class: info

            Description: Chosen sort option. \

        *Default: best_match* \

        Тип сортировки результата. \

        Перечисление может принимать значения::

            ["best_seller", "price_low", "price_high", "best_match"]

        .. admonition:: Caution
            :class: caution

            При сортировке best_match в результатах возвращаются спонсорские продукты. \

    :parameter Boolean $soft_sort: \

    :parameter Boolean $spelling: \

        .. admonition:: Schema reference
            :class: info

            Description: Indicates whether to apply spell correction. \

        *Default: true* \

        Нужно ли исправлять `query`. \

        Значение запроса `query` может быть исправлено на более релевантное.

    :parameter String $xpa: \

        .. admonition:: Schema reference
            :class: info

            Description: This is for expo to enable A/B test on back end. Desktop/mweb sends as query param. \
            Eg:werw1. \

    :parameter Boolean $grid: \

        .. admonition:: Schema reference
            :class: info

            Description: Grid/List view. \

    :parameter String $typehead: \

    :parameter String $strategy: \

    :parameter String $recall_set: \

        .. admonition:: Schema reference
            :class: info

            Description: Stack recall. Indicates the recall set to use. \

    :parameter Boolean $preciseSearch: \

    :parameter String $pap: \

        .. admonition:: Schema reference
            :class: info

            Description: This is a piggy back param. Whenever Preso sends them FE has to url-encode and send it back to preso in the next pagination call. \

    :parameter String $ptss: \

    :parameter String $c_btc_id: \

    :parameter String $c_bstc: \

    :parameter String $sod: \

    :parameter String $channel: \

        .. admonition:: Schema reference
            :class: info

            Description: Tempo channel query params. \

        Известное значение: "WWW". \

    :parameter String $pageType: \

        .. admonition:: Schema reference
            :class: info

            Description: Tempo pageType query params. \

        Известные значения: "SearchPage". \

    :parameter String $tenant: \

        .. admonition:: Schema reference
            :class: info

            Description: Tempo pageType query params. \

        Известные значения: "WM_GLASS". \

    :parameter Boolean $previewMode: \

        .. admonition:: Schema reference
            :class: info

            Description: Whether directed spend is enabled for a cat_id. \
            Eg: browse_shelf. \

    :parameter String $module_search: \

        .. admonition:: Schema reference
            :class: info

            Description: Access tempo-preview environment. \

    :parameter String $trsp: \

        .. admonition:: Schema reference
            :class: info

            Description: Way to override polaris switch properties through FE. \

    :parameter String $dealsId: \

        .. admonition:: Schema reference
            :class: info

            Description: id for dealsPages like gift-finder, savings etc. \

    :parameter JSON $additionalQueryParams: \

        .. admonition:: Schema reference
            :class: info

            Description: In the case of view all, pagination or facet, client will pass all params as the key-value pairs in this query param. \

        Default = {} \

Переменные (Variables)
""""""""""""""""""""""""

.. collapse:: Пример запроса

    .. code-block::

        query Search( $query:String $page:Int $prg:Prg! $facet:String $sort:Sort = best_match $catId:String $max_price:String $min_price:String $spelling:Boolean = true $affinityOverride:AffinityOverride $storeSlotBooked:String $ps:Int $ptss:String $recall_set:String $trsp:String  $additionalQueryParams:JSON ={}){search( query:$query page:$page prg:$prg facet:$facet sort:$sort cat_id:$catId max_price:$max_price min_price:$min_price spelling:$spelling affinityOverride:$affinityOverride storeSlotBooked:$storeSlotBooked ps:$ps ptss:$ptss recall_set:$recall_set trsp:$trsp additionalQueryParams:$additionalQueryParams ){query searchResult{...SearchResultFragment}}}fragment SearchResultFragment on SearchInterface{title aggregatedCount...BreadCrumbFragment...DebugFragment...ItemStacksFragment...PageMetaDataFragment...PaginationFragment...SpellingFragment...RequestContextFragment...ErrorResponse modules{facetsV1{...FacetFragment}guidedNavigation{...GuidedNavFragment}guidedNavigationV2{...PillsModuleFragment}pills{...PillsModuleFragment}spellCheck{title subTitle urlLinkText url}}}fragment BreadCrumbFragment on SearchInterface{breadCrumb{id name url}}fragment DebugFragment on SearchInterface{debug{sisUrl}}fragment ItemStacksFragment on SearchInterface{itemStacks{displayMessage meta{adsBeacon{adUuid moduleInfo max_ads}query stackId stackType title layoutEnum totalItemCount totalItemCountDisplay viewAllParams{query cat_id sort facet affinityOverride recall_set min_price max_price}}itemsV2{...ItemFragment...InGridMarqueeAdFragment}}}fragment ItemFragment on Product{__typename id usItemId fitmentLabel name type shortDescription imageInfo{...ProductImageInfoFragment}canonicalUrl externalInfo{url}category{path{name url}}badges{flags{key text}tags{...on BaseBadge{key text type}}}classType averageRating numberOfReviews esrb mediaRating salesUnitType sellerId sellerName hasSellerBadge availabilityStatusV2{display value}productLocation{displayValue aisle{zone aisle}}badge{type dynamicDisplayName}fulfillmentSpeed offerId preOrder{...PreorderFragment}priceInfo{...ProductPriceInfoFragment}variantCriteria{...VariantCriteriaFragment}fulfillmentBadge fulfillmentTitle fulfillmentType brand manufacturerName showAtc sponsoredProduct{spQs clickBeacon spTags}showOptions}fragment ProductImageInfoFragment on ProductImageInfo{thumbnailUrl}fragment ProductPriceInfoFragment on ProductPriceInfo{priceRange{minPrice maxPrice}currentPrice{...ProductPriceFragment}wasPrice{...ProductPriceFragment}unitPrice{...ProductPriceFragment}listPrice{...ProductPriceFragment}shipPrice{...ProductPriceFragment}subscriptionPrice{priceString subscriptionString}priceDisplayCodes{priceDisplayCondition finalCostByWeight}}fragment PreorderFragment on PreOrder{isPreOrder preOrderMessage preOrderStreetDateMessage}fragment ProductPriceFragment on ProductPrice{price priceString}fragment VariantCriteriaFragment on VariantCriterion{name type id isVariantTypeSwatch variantList{id images name rank swatchImageUrl availabilityStatus products selectedProduct{canonicalUrl usItemId}}}fragment InGridMarqueeAdFragment on MarqueePlaceholder{__typename type moduleLocation lazy}fragment PageMetaDataFragment on SearchInterface{pageMetadata{title canonical description location{addressId}}}fragment PaginationFragment on SearchInterface{paginationV2{maxPage pageProperties}}fragment SpellingFragment on SearchInterface{spelling{correctedTerm}}fragment RequestContextFragment on SearchInterface{requestContext{isFitmentFilterQueryApplied searchMatchType categories{id name}}}fragment ErrorResponse on SearchInterface{errorResponse{correlationId source errors{errorType statusCode statusMsg source}}}fragment GuidedNavFragment on GuidedNavigationSearchInterface{title url}fragment PillsModuleFragment on PillsSearchInterface{title url image:imageV1{src alt}baseSeoURL}fragment FacetFragment on Facet{name type layout min max selectedMin selectedMax unboundedMax stepSize values{id name description type itemCount isSelected baseSeoURL}}

.. collapse:: Пример переменных

    .. code-block:: json
        :linenos:

        {
            "query": "coffee starbucks",
            "page": 3,
            "prg": "desktop",
            "catId": "",
            "facet": "",
            "sort": "price_low",
            "ps": 40,
            "ptss": "",
            "trsp": "",
            "beShelfId": "",
            "recall_set": "",
            "module_search": "",
            "min_price": "",
            "max_price": "",
            "storeSlotBooked": ""
        }

Ответ (Response)
~~~~~~~~~~~~~~~~~~

Стандартный ответ на верхнем уровне состоит из нескольких частей:
::

    {
        "data": {
            "search": {
                "query": "{$query}",
                "searchResult": {SearchInterface},
                "contentLayout": {ContentLayout}
            },
        }
    }

- data.search.query:String - Содержит финальный вариант ключевого слова.
- data.search.searchResult:SearchInterface - Содержит результат поиска типа SearchInterface.

.. collapse:: Структура SearchInterface

        .. code-block:: json
                :linenos:

                {
                    "searchResult": {
                        "query": "",
                        "searchInterfaceKey": "{SearchInterfaceKey}",
                        "_prefetch_": "{JSON}",
                        "itemStacks": "[Stack]",
                        "title": "",
                        "aggregatedCount": 0,
                        "modules": "{SearchInterfaceModule}",
                        "errorResponse": "{ErrorResponse}",
                        "requestContext": "{RequestContext}",
                        "pageMetadata": "{PageMetadata}",
                        "spelling": "{Spelling}",
                        "paginationV2": "{PaginationV2}",
                        "gridViewToggle": "{GridViewToggle}",
                        "debug": "{Debug}",
                        "breadCrumbs": "[breadCrumb]",
                    }
                }

\
    - query: String \
    - searchInterfaceKey: SearchInterfaceKey \
    - _prefetch_: JSON \
    - itemStacks: [Stack] \
        .. admonition:: Schema reference
            :class: info

            Description: Stacks of Items/Products. \
    - title: String
        .. admonition:: Schema reference
            :class: info

            Description: Computed title containing query & result count information. \
    - aggregatedCount: Int - количество результатов. \
    - modules: SearchInterfaceModule \
    - errorResponse: ErrorResponse
        .. admonition:: Schema reference
            :class: info

            Description: Error Information provided by SIS (search interface service). \

    - requestContext: RequestContext
        .. admonition:: Schema reference
            :class: info

            Description: Request Context provided by SIS (search interface service). \

    - pageMetadata: PageMetaData
        .. admonition:: Schema reference
            :class: info

            Description: Page Metadata. \

    - spelling: Spelling
        .. admonition:: Schema reference
            :class: info

            Description: Corrected Spelling and suggestions. \

    - paginationV2: PaginationV2
        .. admonition:: Schema reference
            :class: info

            Description: Pagination information. \

    - gridViewToggle: GridViewToggle
        .. admonition:: Schema reference
            :class: info

            Description: Grid View/List View links. \

    - debug: Debug
        .. admonition:: Schema reference
            :class: info

            Description: Debug information provided by SIS (search interface service). \

    - breadCrumb: [BreadCrumb]
        .. admonition:: Schema reference
            :class: info

            Description: Breadcrumb Navigation Links. \

.. admonition:: Response example
    :class: note

    Полный пример ответа для ключевого слова :download:`"coffee starbucks" <jsons/search_response.json5>`

Таблица сопоставления ответа и визуального местоположения данных (UI-Response table comparison)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


.. _product_id: https://monosnap.com/file/xOYVsDHxKuk24OF6rbzt5tEaH5Bopz
.. |product_id| replace:: Product id

.. _item_id: https://monosnap.com/file/lc9mTjlrMn8V9VK0iyzUDdUxsq6fhH
.. |item_id| replace:: Item id

.. _title: https://monosnap.com/file/a1ooAD8UDZOyd5ZhDPk5UeQt7XPZfR
.. |title| replace:: Title

.. _image_url: https://monosnap.com/file/z6jF9RnLuIeiM30ymucuTSMkFVZce5
.. |image_url| replace:: Image url

.. _canonical_url: https://monosnap.com/file/gg9W6jGzoh0bEf297lJFhC0XHCCX4A
.. |canonical_url| replace:: Canonical url

.. _tags: https://monosnap.com/file/DGBXAVSV08loNF5s4F6QsvieC859aR
.. |tags| replace:: Item tags

.. _rating: https://monosnap.com/file/x7FgX58Id8fFJioLCM8LyMbMJjh0uA
.. |rating| replace:: Rating

.. _price: https://monosnap.com/file/bcI6FqW4UbCzjVJLAUzIPXEpnm7D8L
.. |price| replace:: Price

.. _stock: https://monosnap.com/file/SFHnK3QqLxixLr4MxHRxh40VKltJyD
.. |stock| replace:: Stock status

.. _sponsored: https://monosnap.com/file/cdJDnxhCEzxsMxfQxI3BVuQN8rHVhU
.. |sponsored| replace:: Sponsored product

.. _variants: https://monosnap.com/file/PZCVoM3VRZ4RYYmnRRo16pZ66BtHwP
.. |variants| replace:: Item variants

+-------------------+---------------------------+----------------------------------------------------------------+
| Title             | Description               | JSON-Path                                                      |
+===================+===========================+================================================================+
| |product_id|_     | Unique page identifier    | data.search.searchResult.itemStacks[0].itemsV2[i].id           |
+-------------------+---------------------------+----------------------------------------------------------------+
| |item_id|_        | Unique item identifier    | data.search.searchResult.itemStacks[0].itemsV2[i].usItemId     |
+-------------------+---------------------------+----------------------------------------------------------------+
| |title|_          | Title of item             | data.search.searchResult.itemStacks[0].itemsV2[i].name         |
+-------------------+---------------------------+----------------------------------------------------------------+
| |image_url|_      | Image of item             | data.search.searchResult.itemStacks[0].itemsV2[i].imageInfo.   |
|                   |                           | thumbnailUrl                                                   |
+-------------------+---------------------------+----------------------------------------------------------------+
| |canonical_url|_  | Url of item page          | data.search.searchResult.itemStacks[0].itemsV2[i].canonicalUrl |
+-------------------+---------------------------+----------------------------------------------------------------+
| |tags|_           | Item tags of features     | data.search.searchResult.itemStacks[0].itemsV2[i].badges.flags |
+-------------------+---------------------------+----------------------------------------------------------------+
| |rating|_         | Average rating & review   | data.search.searchResult.itemStacks[0].itemsV2[i]              |
|                   | numbers                   | .averageRating and .numberOfReviews                            |
+-------------------+---------------------------+----------------------------------------------------------------+
| |price|_          | Item price                | data.search.searchResult.itemStacks[0].itemsV2[i].priceInfo    |
+-------------------+---------------------------+----------------------------------------------------------------+
| |stock|_          | Stock status of item      | data.search.searchResult.itemStacks[0].itemsV2[i].             |
|                   |                           | availabilityStatusV2                                           |
+-------------------+---------------------------+----------------------------------------------------------------+
| |sponsored|_      | Whether the product is    | data.search.searchResult.itemStacks[0].itemsV2[i]              |
|                   | sponsored                 | .sponsoredProduct                                              |
+-------------------+---------------------------+----------------------------------------------------------------+
| |variants|_       | Variants criteria &       | data.search.searchResult.itemStacks[0].itemsV2[i]              |
|                   | variants items            | .variantCriteria                                               |
+-------------------+---------------------------+----------------------------------------------------------------+
