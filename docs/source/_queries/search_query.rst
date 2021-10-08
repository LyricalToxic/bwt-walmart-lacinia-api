Search
-----------
Overview
~~~~~~~~~~~

..
    Overview для каждого запроса должно содержать:

        1. Предназначение.
        2. HTTP метод и endpoint.
        3. Описание свойств запроса.
        4. Описание ответа.
        5. Особенности.

.. Предназначение

Запрос предназначен для поиска товаров по ключевому слову. \

.. HTTP метод и endpoint.

Поиск по ключевым словам на сайте walmart можно выполнить используя `post` запрос на endpoint:
::

    https://www.walmart.com/orchestra/home/graphql

.. Описание свойств запроса.

Для запроса являются обязательными заголовки:
::

    Accept: application/json
    Content-Type: application/json
    WM_MP: True
    X-APOLLO-OPERATION-NAME: Search

.. Описание ответа.

Результатом поиска будет информация о продуктах, рекламе и метаданных ответа.
Продукты разбиваются на пагинации по N штук на K страниц.
Для каждого набора фильтров можно получить не более 1000 товаров, то есть при 40 товарах на странице будет 25 пагинаций.

.. Особенности

- размер пагинации всегда 40
- при размере пагинации 40 может приходить меньше 40 товаров
- на пагинациях есть повторяющиеся товары


.. _body:

Body
~~~~~~~~~~~

Query
"""""""""""

.. function:: query Search(...)

    :parameter String $query: \

        Ключевое слово для поиска. \

        Может быть пустым. В таком случае результатом поиска будет 15000 результатов соответствующим сортировки. \


    :parameter Int $page: \

        Порядковый номер страницы пагинации. \

        При положительных значениях возвращает результат поиска для указанной страницы, если она существует.
        В противном случае результат возвращен не будет и количество результатов будет равно 0. \

        При 40 товаров на пагинации максимальное значение страницы 25. \

    :parameter Prg $prg!: \

        Тип устройства. На результат поиска не влияет. \

        Перечисление может принимать значения::

                ["desktop", "mWeb", "ios", "android"]

    :parameter String $facet: \

        Фильтры поиска. \

        Форма записи::

            тип фильтра:значение||тип фильтра:значение ...

            Например: fulfillment_method:Delivery||brand:Cra-Z-Art

        Значение фильтров находятся в ответе.

    :parameter Sort $sort: \

        *Default: best_match* \

        Тип сортировки результата. \

        Перечисление может принимать значения::

            ["best_seller", "price_low", "price_high", "best_match"]

        .. admonition:: Caution
            :class: caution

            При сортировке best_match в результатах возвращаются спонсорские продукты.

    :parameter String $catId: \

        Уникальный идентификатор категорий. \

        Если поле указано, то в результате выдачи попадут товары только относящиеся к конечной категории

        Форма записи::

            главная категория_под категория_...._конечная категория

            Например: 1229749_1086045_9412206_8443517_3254837

        Уникальные идентификаторы содержаться в ответе.

    :parameter String $max_price: \

        Максимальная цена продукта. \

        Скорее всего цена, на стороне сервера, парсится из строки в числовое значение.
        Если распарсить не удалось, то при выдаче поисковый движок будет считать что цена равна 0. \

        Максимальная цена не может быть:

        - меньше минимальной

        - дробной

        При достаточно большом значении цены(значение больше чем наибольшая цена из результатов) и отсутствии значение " " количество результатов будет отличаться.
        В основном при отсутствии значения количество результатов будет больше. \

        .. admonition:: Attention
            :class: attention

            Этот параметр не гарантирует, что в поисковой выдаче не будет товара с ценой выше чем указано.

    :parameter String $min_price: \

        Минимальная цена продукта. \

        Скорее всего цена, на стороне сервера, парсится из строки в числовое значение.
        Если распарсить не удалось, то при выдаче поисковый движок будет считать что цена равна 0. \

        Минимальная цена не может быть:

        - больше максимальной

        - дробной

        При значении цены "0" и отсутствии значение " " количество результатов будет отличаться.
        В основном при отсутствии значения количество результатов будет больше. \

        .. admonition:: Attention
            :class: attention

            Этот параметр не гарантирует, что в поисковой выдаче не будет товара с ценой ниже чем указано.


    :parameter Boolean $spelling: \

        *Default: true* \

        Нужно ли исправлять `query`. \

        Значение запроса `query` может быть исправлено на более релевантное при значении true.

    :parameter AffinityOverride $affinityOverride: \

        Неизвестно  \

        Необязательный параметр. Влияет на результат.

        Перечисление может принимать значения::

            ["default", "default_fc", "store_only", "store_led"]

    :parameter String $storeSlotBooked: \

        Неизвестно  \

    :parameter Int $ps: \

        Количество товаров на пагинации. \

        Не на что не влияет. Всегда будет приходить не более 40 товаров на страницу.

    :parameter String $ptss: \

        Неизвестно \

    :parameter String $recall_set: \

        Неизвестно \

    :parameter JSON $fitmentFieldParams: \

        Default = {} \

        Параметры автомобиля при поиске товаров для автомобиля. \

    :parameter JSON $fitmentSearchParams: \

        Default = {} \

        Параметры поиска. Дублирует основные параметры поиска. Необязательное. \

    :parameter Boolean $fetchMarquee!: \

        Будет ли приходить marquee сущности.  \

        Предположительно вид рекламы.
        Находятся в ответе между продуктами и имеют __typename=MarqueePlaceholder. \


    :parameter String $trsp: \

        Неизвестно \

    :parameter Boolean $fetchSkyline!: \

        Будет ли skyline сущности.  \

        Предположительно вид рекламы.

    :parameter Boolean $fetchSbaTop!: \

        Будет ли sbatop сущности.  \

        Предположительно вид рекламы.
        Находятся в ответе между продуктами и имеют __typename=SponsoredBrands. `Пример <https://monosnap.com/file/1GbI0G0TS9mGdNvyjsvoUh6CPlu4CK>`_. \


    :parameter JSON $additionalQueryParams: \

        Default = {} \

        Описание \

Пример запроса:
    .. code-block::

        query Search( $query:String $page:Int $prg:Prg! $facet:String $sort:Sort = best_match $catId:String $max_price:String $min_price:String $spelling:Boolean = true $affinityOverride:AffinityOverride $storeSlotBooked:String $ps:Int $ptss:String $recall_set:String $fitmentFieldParams:JSON ={}$fitmentSearchParams:JSON ={}$fetchMarquee:Boolean! $trsp:String $fetchSkyline:Boolean! $fetchSbaTop:Boolean! $additionalQueryParams:JSON ={}){search( query:$query page:$page prg:$prg facet:$facet sort:$sort cat_id:$catId max_price:$max_price min_price:$min_price spelling:$spelling affinityOverride:$affinityOverride storeSlotBooked:$storeSlotBooked ps:$ps ptss:$ptss recall_set:$recall_set trsp:$trsp additionalQueryParams:$additionalQueryParams ){query searchResult{...SearchResultFragment}}contentLayout( channel:"WWW" pageType:"SearchPage" tenant:"WM_GLASS" searchArgs:{query:$query cat_id:$catId prg:$prg}){modules{...ModuleFragment configs{...SearchNonItemFragment __typename...on TempoWM_GLASSWWWSponsoredProductCarouselConfigs{_rawConfigs}...on _TempoWM_GLASSWWWSearchSortFilterModuleConfigs{facetsV1{...FacetFragment}}...on _TempoWM_GLASSWWWSearchGuidedNavModuleConfigs{guidedNavigation{...GuidedNavFragment}}...on TempoWM_GLASSWWWPillsModuleConfigs{moduleSource pillsV2{...PillsModuleFragment}}...on TempoWM_GLASSWWWSearchFitmentModuleConfigs{fitments( fitmentSearchParams:$fitmentSearchParams fitmentFieldParams:$fitmentFieldParams ){...FitmentFragment sisFitmentResponse{...SearchResultFragment}}}...BrandAmplifierAdConfigs @include(if:$fetchSbaTop)...BannerModuleFragment...MarqueeDisplayAdConfigsFragment @include(if:$fetchMarquee)...SkylineDisplayAdConfigsFragment @include(if:$fetchSkyline)...HorizontalChipModuleConfigsFragment}}...LayoutFragment pageMetadata{location{postalCode stateOrProvinceCode city storeId}pageContext}}}fragment SearchResultFragment on SearchInterface{title aggregatedCount...BreadCrumbFragment...DebugFragment...ItemStacksFragment...PageMetaDataFragment...PaginationFragment...SpellingFragment...RequestContextFragment...ErrorResponse modules{facetsV1{...FacetFragment}guidedNavigation{...GuidedNavFragment}guidedNavigationV2{...PillsModuleFragment}pills{...PillsModuleFragment}spellCheck{title subTitle urlLinkText url}}}fragment ModuleFragment on TempoModule{name version type moduleId schedule{priority}matchedTrigger{zone}}fragment LayoutFragment on ContentLayout{layouts{id layout}}fragment BreadCrumbFragment on SearchInterface{breadCrumb{id name url}}fragment DebugFragment on SearchInterface{debug{sisUrl}}fragment ItemStacksFragment on SearchInterface{itemStacks{displayMessage meta{adsBeacon{adUuid moduleInfo max_ads}query stackId stackType title layoutEnum totalItemCount totalItemCountDisplay viewAllParams{query cat_id sort facet affinityOverride recall_set min_price max_price}}itemsV2{...ItemFragment...InGridMarqueeAdFragment}}}fragment ItemFragment on Product{__typename id usItemId fitmentLabel name type shortDescription imageInfo{...ProductImageInfoFragment}canonicalUrl externalInfo{url}category{path{name url}}badges{flags{key text}tags{...on BaseBadge{key text type}}}classType averageRating numberOfReviews esrb mediaRating salesUnitType sellerId sellerName hasSellerBadge availabilityStatusV2{display value}productLocation{displayValue aisle{zone aisle}}badge{type dynamicDisplayName}fulfillmentSpeed offerId preOrder{...PreorderFragment}priceInfo{...ProductPriceInfoFragment}variantCriteria{...VariantCriteriaFragment}fulfillmentBadge fulfillmentTitle fulfillmentType brand manufacturerName showAtc sponsoredProduct{spQs clickBeacon spTags}showOptions}fragment ProductImageInfoFragment on ProductImageInfo{thumbnailUrl}fragment ProductPriceInfoFragment on ProductPriceInfo{priceRange{minPrice maxPrice}currentPrice{...ProductPriceFragment}wasPrice{...ProductPriceFragment}unitPrice{...ProductPriceFragment}listPrice{...ProductPriceFragment}shipPrice{...ProductPriceFragment}subscriptionPrice{priceString subscriptionString}priceDisplayCodes{priceDisplayCondition finalCostByWeight}}fragment PreorderFragment on PreOrder{isPreOrder preOrderMessage preOrderStreetDateMessage}fragment ProductPriceFragment on ProductPrice{price priceString}fragment VariantCriteriaFragment on VariantCriterion{name type id isVariantTypeSwatch variantList{id images name rank swatchImageUrl availabilityStatus products selectedProduct{canonicalUrl usItemId}}}fragment InGridMarqueeAdFragment on MarqueePlaceholder{__typename type moduleLocation lazy}fragment PageMetaDataFragment on SearchInterface{pageMetadata{title canonical description location{addressId}}}fragment PaginationFragment on SearchInterface{paginationV2{maxPage pageProperties}}fragment SpellingFragment on SearchInterface{spelling{correctedTerm}}fragment RequestContextFragment on SearchInterface{requestContext{isFitmentFilterQueryApplied searchMatchType categories{id name}}}fragment ErrorResponse on SearchInterface{errorResponse{correlationId source errors{errorType statusCode statusMsg source}}}fragment GuidedNavFragment on GuidedNavigationSearchInterface{title url}fragment PillsModuleFragment on PillsSearchInterface{title url image:imageV1{src alt}baseSeoURL}fragment BannerModuleFragment on TempoWM_GLASSWWWSearchBannerConfigs{moduleType viewConfig{title image imageAlt displayName description url urlAlt appStoreLink appStoreLinkAlt playStoreLink playStoreLinkAlt}}fragment FacetFragment on Facet{name type layout min max selectedMin selectedMax unboundedMax stepSize values{id name description type itemCount isSelected baseSeoURL}}fragment FitmentFragment on Fitments{partTypeIDs result{status formId position quantityTitle extendedAttributes{...FitmentFieldFragment}labels{...LabelFragment}resultSubTitle}labels{...LabelFragment}savedVehicle{vehicleYear{...VehicleFieldFragment}vehicleMake{...VehicleFieldFragment}vehicleModel{...VehicleFieldFragment}additionalAttributes{...VehicleFieldFragment}}fitmentFields{...VehicleFieldFragment}fitmentForms{id fields{...FitmentFieldFragment}title labels{...LabelFragment}}}fragment LabelFragment on FitmentLabels{ctas{...FitmentLabelEntityFragment}messages{...FitmentLabelEntityFragment}links{...FitmentLabelEntityFragment}images{...FitmentLabelEntityFragment}}fragment FitmentLabelEntityFragment on FitmentLabelEntity{id label}fragment VehicleFieldFragment on FitmentVehicleField{id label value}fragment FitmentFieldFragment on FitmentField{id displayName value extended data{value label}dependsOn}fragment MarqueeDisplayAdConfigsFragment on TempoWM_GLASSWWWMarqueeDisplayAdConfigs{_rawConfigs ad{...DisplayAdFragment}}fragment DisplayAdFragment on Ad{...AdFragment adContent{type data{__typename...AdDataDisplayAdFragment}}}fragment AdFragment on Ad{status moduleType platform pageId pageType storeId stateCode zipCode pageContext moduleConfigs adsContext adRequestComposite}fragment AdDataDisplayAdFragment on AdData{...on DisplayAd{json status}}fragment SkylineDisplayAdConfigsFragment on TempoWM_GLASSWWWSkylineDisplayAdConfigs{_rawConfigs ad{...SkylineDisplayAdFragment}}fragment SkylineDisplayAdFragment on Ad{...SkylineAdFragment adContent{type data{__typename...SkylineAdDataDisplayAdFragment}}}fragment SkylineAdFragment on Ad{status moduleType platform pageId pageType storeId stateCode zipCode pageContext moduleConfigs adsContext adRequestComposite}fragment SkylineAdDataDisplayAdFragment on AdData{...on DisplayAd{json status}}fragment BrandAmplifierAdConfigs on TempoWM_GLASSWWWBrandAmplifierAdConfigs{_rawConfigs moduleLocation ad{...SponsoredBrandsAdFragment}}fragment SponsoredBrandsAdFragment on Ad{...AdFragment adContent{type data{__typename...AdDataSponsoredBrandsFragment}}}fragment AdDataSponsoredBrandsFragment on AdData{...on SponsoredBrands{adUuid adExpInfo moduleInfo brands{logo{featuredHeadline featuredImage featuredImageName featuredUrl logoClickTrackUrl}products{...ProductFragment}}}}fragment ProductFragment on Product{usItemId offerId badges{flags{key text}labels{key text}tags{key text}}priceInfo{priceDisplayCodes{rollback reducedPrice eligibleForAssociateDiscount clearance strikethrough submapType priceDisplayCondition unitOfMeasure pricePerUnitUom}currentPrice{price priceString}wasPrice{price priceString}priceRange{minPrice maxPrice priceString}unitPrice{price priceString}}showOptions sponsoredProduct{spQs clickBeacon spTags}canonicalUrl numberOfReviews averageRating availabilityStatus imageInfo{thumbnailUrl allImages{id url}}name fulfillmentBadge classType type p13nData{predictedQuantity flags{PREVIOUSLY_PURCHASED{text}CUSTOMERS_PICK{text}}labels{PREVIOUSLY_PURCHASED{text}CUSTOMERS_PICK{text}}}}fragment SearchNonItemFragment on TempoWM_GLASSWWWSearchNonItemConfigs{title subTitle urlLinkText url}fragment HorizontalChipModuleConfigsFragment on TempoWM_GLASSWWWHorizontalChipModuleConfigs{chipModuleSource:moduleSource chipModule{title url{linkText title clickThrough{type value}}}chipModuleWithImages{title url{linkText title clickThrough{type value}}image{alt clickThrough{type value}height src title width}}}

Variables
""""""""""""
Variables
    - **id** (str) - неизвестно.
    - **dealsId** (str) - неизвестно.
    - **query** (str) - поисковый запрос. Соответствует :class:`$query`.
    - **page** (int) - номер страницы. Соответствует :class:`$page`.
    - **spelling** (bool) - исправлять ли поисковый запрос. Соответствует :class:`$spelling`.
    - **prg** (str) - тип устройства. Соответствует :class:`$prg`.
    - **catId** (str) - номер категории. Соответствует :class:`$catId`.
    - **facet** (str) - фильтр поиска. Соответствует :class:`$facet`.
    - **sort** (str) - фильтр сортировки. Соответствует :class:`$sort`.
    - **rawFacet** (str) - неизвестно.
    - **seoPath** (str)- неизвестно.
    - **ps** (int) - количество товаров на странице. Соответствует :class:`$ps`.
    - **ptss** (str) - неизвестно.
    - **trsp** (str) - неизвестно.
    - **beShelfId** (str) - неизвестно.
    - **recall_set** (str) - неизвестно.
    - **module_search** (str) - неизвестно.
    - **min_price** (str) - минимум ценового диапазона. Соответствует :class:`max_price`.
    - **max_price** (str) - максимум ценового диапазона. Соответствует :class:`max_price`.
    - **storeSlotBooked** (str) - неизвестно.
    - **additionalQueryParams** (object) - неизвестно. Соответствует :class:`$additionalQueryParams`.
    - **fitmentFieldParams** (object) - параметры автомобиля. Соответствует :class:`$fitmentFieldParams`.
    - **fitmentSearchParams** (object) - параметры поиска. Соответствует :class:`$fitmentSearchParams`.
    - **fetchMarquee** (bool) - будет ли приходить marquee сущности. Соответствует :class:`$fetchMarquee`.
    - **fetchSkyline** (bool) - будет ли skyline сущности.. Соответствует :class:`$fetchSkyline`.
    - **fetchSbaTop** (bool) - будет ли sbatop сущности. Соответствует :class:`$fetchSbaTop`.

Пример переменных:
    .. code-block::

        {"id":"","dealsId":"","query":"C2G","page":1,"prg":"desktop","catId":"","facet":"","sort":"best_match","rawFacet":"","seoPath":"","ps":40,"ptss":"","trsp":"","beShelfId":"","recall_set":"","module_search":"","min_price":"","max_price":"","storeSlotBooked":"","fitmentFieldParams":null,"fitmentSearchParams":{"id":"","dealsId":"","query":"C2G","page":1,"prg":"desktop","catId":"","facet":"","sort":"best_match","rawFacet":"","seoPath":"","ps":40,"ptss":"","trsp":"","beShelfId":"","recall_set":"","module_search":"","min_price":"","max_price":"","storeSlotBooked":"","cat_id":"","_be_shelf_id":""},"fetchMarquee":true,"fetchSkyline":true,"fetchSbaTop":true}

Response
~~~~~~~~~~~
Стандартный ответ на верхнем уровне состоит из нескольких частей:
::

    {
        "data": {
            "search": {...}
            "contentLayout": {...}
        }
    }

- data.search - Содержит результат поиска и некоторые метаданные.
    - query - Поисковый запрос
    - searchResult - Результат поиска.

::

    {
        "title": "",
        "aggregatedCount": 0,
        "breadCrumb": null,
        "debug": {},
        "itemStacks": [],
        "pageMetadata": {},
        "paginationV2": {},
        "spelling": {},
        "requestContext": {},
        "errorResponse": {},
        "modules": null,
    }

\
    Из важного:
        - aggregatedCount - количество результатов
        - itemStacks - список состоящий из типов. Известные типы продуктов: результат поиска, `похожие продукты <https://monosnap.com/file/4gSV6zy1HznqJXs3JsVlJNRVYNzNKR>`_ .\
            - meta - мета информации результате
            - itemsV2 - список результатов
        - pageMetadata - описательная  информация о странице. Содержит локацию
        - paginationV2 - параметры запроса.

- data.contentLayout - Содержит modules, layouts и pageMetadata.

::

    "contentLayout": {
        "modules": [...],
        "layouts": [...],
        "pageMetadata": {...},
    }

\
    - modules - Содержит информацию о различных конфигурациях таких как: facet, sort, marquee etc.
    - layouts - Содержит информацию о расположении макетов на странице. Зависит от типа устройства.
    - pageMetadata - Содержит информацию о локации и контексте.

Полный пример ответа для ключевого слова "Toyo": :download:`link <data/search_response.json5>`
