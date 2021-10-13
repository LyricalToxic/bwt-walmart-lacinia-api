ItemById
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

Запрос предназначен для поиска информации о продукте с помощью item id. \

.. HTTP метод и endpoint.

Поиск информации о продукте на сайте walmart можно выполнить используя `post` запрос на endpoint:
::

    https://www.walmart.com/orchestra/home/graphql или https://www.walmart.com/orchestra/home/graphql/ip/{item_id}

.. Описание свойств запроса.

Для запроса являются обязательными заголовки:
::

    Accept: application/json
    Content-Type: application/json
    WM_MP: True
    X-APOLLO-OPERATION-NAME: ItemById

Помимо ключевого слова, в запросе можно указать различные фильтры и сортировку, что в свою очередь будет влиять на количество результатов.
Также локация влияет на количество результатов.

.. Описание ответа.

Результатом поиска является информация о продуктах, рекламе и метаданных.
Продукты разбиваются на пагинации по N штук на K страниц.
Для каждого набора фильтров можно получить не более 1000 товаров, то есть при 40 товарах на странице будет 25 пагинаций.

.. Особенности

- размер пагинации всегда 40
- при размере пагинации 40 может приходить меньше 40 товаров
- на пагинациях есть повторяющиеся товары
- при запросе на страницы следующие за 25, в ответе могут содержаться продукты. Они будут дубликатами
- сортировка по цене работает некорректно. Результаты сортируются в пределах одной пагинации.

Body
~~~~~~~~~~~

Query
"""""""""""

.. function:: query ItemById(...)

    :parameter String $itemId!: \

        Уникальный идентификатор товара на walmart. \

        Число. \

    :parameter Boolean $selected: \

        Неизвестно \

    :parameter String $variantFieldId: \

        Неизвестно \

    :parameter PostalAddress $postalAddress: \

        Местоположение пользователя. Не влияет на фактическую локацию. Фактическая локация хранится в куках. \

        Состоит из полей::

                {
                    "postalCode": "",
                    "zipLocated": bool,
                    "stateOrProvinceCode": "",
                    "stateOrProvinceName": "",
                    "countryCode": "",
                    "addressType": "",
                    "isPoBox": bool,
                }

    :parameter  $storeFrontIds: \

        Уникальный идентификатор магазина. \

    :parameter Int $page: \

        Страница пагинации ревью. \

    :parameter String $sort: \

        Тип сортировки ревью. \

        Может принимать значения::

            ["relevancy", "helpful", "submission-desc", "submission-asc", "rating-desc", "rating-asc"]

    :parameter Int $limit: \

        Количество ревью. Не более 20. \

    :parameter  $filters: \

        Фильтры для ревью по звездам. \

    :parameter String $channel!: \

        Неизвестно. Влияет на modules и layouts. Известное значение "WWW". \

    :parameter String $pageType!: \

        Тип страницы. Для страницы продуктов `ItemPageGlobal`. \

    :parameter String $tenant!: \

        Параметр для contentLayout. Значение для страницы продуктов `WM_GLASS`. \

    :parameter String $version!: \

        Версия contentLayout. Значение не влияет на результат. \

    :parameter P13NRequest $p13N: \

        Обязательный параметр, содержащий метаинформацию о запросе. Нужен для modules. \

    :parameter JSON $p13nCls: \

        Интерфейс p13N. \

    :parameter [String] $layout: \

        Список типов layout. \

    .. admonition:: Response example
        :class: note

        Layout - набор параметров, определяющих местоположение объектов на странице. По сути является метаинформацией для front-end.

    :parameter JSON $tempo: \

        Неизвестно. \

        Необязательный параметр.

    :parameter Int $semStoreId: \

        Уникальный идентификатор магазина. *Неизвестно на что влияет* \

    :parameter String $catalogSellerId: \

        Уникальный идентификатор каталога продавца. \

        Можно переопределить. Тогда в ответе будет информация о продавце, который был указан. \

    :parameter Boolean $fetchBuyBoxAd!: \

        Нужно ли возвращать информацию о buyBoxAd. \

    :parameter Boolean $fetchMarquee!: \

       Нужно ли возвращать информацию о marquee. \

    :parameter Boolean $fetchSkyline!: \

        Нужно ли возвращать информацию о skyline. \

    :parameter Boolean $fetchSpCarousel!: \

        Нужно ли возвращать информацию о spCarousel. \

    :parameter String $fulfillmentIntent: \

        Неизвестно. \

Пример запроса:
    .. code-block::

        query ItemById( $itemId:String! $selected:Boolean $variantFieldId:String $postalAddress:PostalAddress $storeFrontIds:[StoreFrontId]$page:Int $sort:String $limit:Int $filters:[String]$channel:String! $pageType:String! $tenant:String! $version:String! $p13N:P13NRequest $p13nCls:JSON $layout:[String]$tempo:JSON $semStoreId:Int $catalogSellerId:String $fetchBuyBoxAd:Boolean! $fetchMarquee:Boolean! $fetchSkyline:Boolean! $fetchSpCarousel:Boolean! $fulfillmentIntent:String ){contentLayout( channel:$channel pageType:$pageType tenant:$tenant version:$version ){modules(p13n:$p13nCls tempo:$tempo){configs{...on EnricherModuleConfigsV1{zoneV1}...on TempoWM_GLASSWWWItemCarouselConfigsV1{products{...ContentLayoutProduct}subTitle tileOptions{addToCart averageRatings displayAveragePriceCondition displayPricePerUnit displayStandardPrice displayWasPrice fulfillmentBadging mediaRatings productFlags productLabels productPrice productTitle}title type spBeaconInfo{adUuid moduleInfo pageViewUUID placement max}viewAllLink{linkText title uid}}...on TempoWM_GLASSWWWItemFitmentModuleConfigs{fitment{partTypeID partTypeIDs result{status notes position formId quantityTitle resultSubTitle suggestions{id position loadIndex speedRating searchQueryParam labels{...FitmentLabel}fitmentSuggestionParams{id value}cat_id}extendedAttributes{...FitmentFieldFragment}labels{...FitmentLabel}}labels{...FitmentLabel}savedVehicle{...FitmentVehicleFragment}}}...on TempoWM_GLASSWWWItemRelatedShelvesConfigs{seoItemRelmData(id:$itemId){relm{id url name}}}...on TempoWM_GLASSWWWCapitalOneBannerConfigsV1{bannerBackgroundColor primaryImage{alt src}bannerCta{ctaLink{linkText title clickThrough{value}uid}textColor}bannerText{text isBold isUnderlined underlinedColor textColor}}...on TempoWM_GLASSWWWProductWarrantyPlaceholderConfigs{expandedOnPageLoad}...on TempoWM_GLASSWWWGeneralWarningsPlaceholderConfigs{expandedOnPageLoad}...on TempoWM_GLASSWWWProductIndicationsPlaceholderConfigs{expandedOnPageLoad}...on TempoWM_GLASSWWWProductDescriptionPlaceholderConfigs{expandedOnPageLoad}...on TempoWM_GLASSWWWProductDirectionsPlaceholderConfigs{expandedOnPageLoad}...on TempoWM_GLASSWWWProductSpecificationsPlaceholderConfigs{expandedOnPageLoad}...on TempoWM_GLASSWWWNutritionValuePlaceholderConfigs{expandedOnPageLoad}...on TempoWM_GLASSWWWReviewsPlaceholderConfigs{expandedOnPageLoad}...on TempoWM_GLASSWWWProductDescriptionPlaceholderConfigs{expandedOnPageLoad}...BuyBoxAdConfigsFragment @include(if:$fetchBuyBoxAd)...MarqueeDisplayAdConfigsFragment @include(if:$fetchMarquee)...SkylineDisplayAdConfigsFragment @include(if:$fetchSkyline)...SponsoredProductCarouselConfigsFragment @include(if:$fetchSpCarousel)}moduleId matchedTrigger{pageType pageId zone inheritable}name type version status publishedDate}layouts(layout:$layout){id layout}pageMetadata{location{postalCode stateOrProvinceCode city storeId}pageContext}}product( catalogSellerId:$catalogSellerId itemId:$itemId postalAddress:$postalAddress storeFrontIds:$storeFrontIds selected:$selected semStoreId:$semStoreId p13N:$p13N variantFieldId:$variantFieldId fulfillmentIntent:$fulfillmentIntent ){...FullProductFragment}idml(itemId:$itemId html:true){...IDMLFragment}reviews( itemId:$itemId page:$page limit:$limit sort:$sort filters:$filters ){...FullReviewsFragment}}fragment FullProductFragment on Product{showFulfillmentLink additionalOfferCount shippingRestriction availabilityStatus averageRating brand badges{...BadgesFragment}rhPath partTerminologyId aaiaBrandId manufacturerProductId productTypeId tireSize tireLoadIndex tireSpeedRating viscosity model buyNowEligible preOrder{...PreorderFragment}canonicalUrl catalogSellerId sellerReviewCount sellerAverageRating category{...ProductCategoryFragment}classType classId fulfillmentTitle shortDescription fulfillmentType fulfillmentBadge fulfillmentLabel{wPlusFulfillmentText message shippingText fulfillmentText locationText fulfillmentMethod addressEligibility fulfillmentType postalCode}hasSellerBadge itemType id imageInfo{...ProductImageInfoFragment}location{postalCode stateOrProvinceCode city storeIds}manufacturerName name numberOfReviews orderMinLimit orderLimit offerId priceInfo{priceDisplayCodes{...PriceDisplayCodesFragment}currentPrice{...ProductPriceFragment}wasPrice{...ProductPriceFragment}unitPrice{...ProductPriceFragment}subscriptionPrice{price priceString intervalFrequency duration percentageRate subscriptionString}priceRange{minPrice maxPrice priceString currencyUnit unitOfMeasure denominations{price priceString selected}}}returnPolicy{returnable freeReturns returnWindow{value unitType}}fsaEligibleInd sellerId sellerName sellerDisplayName secondaryOfferPrice{currentPrice{priceType priceString price}}semStoreData{pickupStoreId deliveryStoreId isSemLocationDifferent}shippingOption{...ShippingOptionFragment}type pickupOption{slaTier accessTypes availabilityStatus storeName storeId}salesUnit usItemId variantCriteria{id categoryTypeAllValues name type variantList{availabilityStatus id images name products swatchImageUrl selected}}variants{...MinimalProductFragment}groupMetaData{groupType groupSubType numberOfComponents groupComponents{quantity offerId componentType}}upc wfsEnabled sellerType ironbankCategory snapEligible promoData{id description terms type templateData{priceString imageUrl}}showAddOnServices addOnServices{serviceType serviceTitle serviceSubTitle groups{groupType groupTitle assetUrl shortDescription services{displayName offerId selectedDisplayName currentPrice{price priceString}}}}productLocation{displayValue}}fragment BadgesFragment on UnifiedBadge{flags{__typename...on BaseBadge{id text key query}...on PreviouslyPurchasedBadge{id text key lastBoughtOn numBought criteria{name value}}}labels{__typename...on BaseBadge{id text key}...on PreviouslyPurchasedBadge{id text key lastBoughtOn numBought}}tags{__typename...on BaseBadge{id text key}}}fragment ShippingOptionFragment on ShippingOption{accessTypes availabilityStatus slaTier deliveryDate maxDeliveryDate shipMethod shipPrice{...ProductPriceFragment}}fragment ProductCategoryFragment on ProductCategory{categoryPathId path{name url}}fragment PreorderFragment on PreOrder{streetDate streetDateDisplayable streetDateType isPreOrder preOrderMessage preOrderStreetDateMessage}fragment MinimalProductFragment on Variant{availabilityStatus imageInfo{...ProductImageInfoFragment}priceInfo{priceDisplayCodes{...PriceDisplayCodesFragment}currentPrice{...ProductPriceFragment}wasPrice{...ProductPriceFragment}unitPrice{...ProductPriceFragment}}productUrl usItemId id:productId fulfillmentBadge}fragment ProductImageInfoFragment on ProductImageInfo{allImages{id url zoomable}thumbnailUrl}fragment PriceDisplayCodesFragment on PriceDisplayCodes{clearance eligibleForAssociateDiscount finalCostByWeight hidePriceForSOI priceDisplayCondition pricePerUnitUom reducedPrice rollback strikethrough submapType unitOfMeasure unitPriceDisplayCondition}fragment ProductPriceFragment on ProductPrice{price priceString variantPriceString priceType currencyUnit}fragment NutrientFragment on Nutrient{name amount dvp childNutrients{name amount dvp}}fragment NutritionAttributeFragment on NutritionAttribute{name mainNutrient{...NutrientFragment}childNutrients{...NutrientFragment childNutrients{...NutrientFragment}}}fragment IdmlAttributeFragment on IdmlAttribute{name value attribute}fragment ServingAttributeFragment on ServingAttribute{name values{...IdmlAttributeFragment values{...IdmlAttributeFragment}}}fragment IDMLFragment on Idml{chokingHazards{...LegalContentFragment}directions{name value}indications{name value}ingredients{activeIngredientName{name value}activeIngredients{name value}inactiveIngredients{name value}ingredients{name value}}longDescription shortDescription interactiveProductVideo specifications{name value}warnings{name value}warranty{information length}esrbRating mpaaRating nutritionFacts{calorieInfo{...NutritionAttributeFragment}keyNutrients{name values{...NutritionAttributeFragment}}vitaminMinerals{...NutritionAttributeFragment}servingInfo{...ServingAttributeFragment}additionalDisclaimer{...IdmlAttributeFragment values{...IdmlAttributeFragment values{...IdmlAttributeFragment}}}staticContent{...IdmlAttributeFragment values{...IdmlAttributeFragment values{...IdmlAttributeFragment}}}}}fragment FullReviewsFragment on ProductReviews{averageOverallRating customerReviews{...CustomerReviewsFragment}ratingValueFiveCount ratingValueFourCount ratingValueOneCount ratingValueThreeCount ratingValueTwoCount roundedAverageOverallRating topNegativeReview{rating reviewSubmissionTime negativeFeedback positiveFeedback reviewText reviewTitle}topPositiveReview{rating reviewSubmissionTime negativeFeedback positiveFeedback reviewText reviewTitle}totalReviewCount}fragment LegalContentFragment on LegalContent{ageRestriction headline headline image mature message}fragment CustomerReviewsFragment on CustomerReview{rating reviewSubmissionTime reviewText reviewTitle userNickname}fragment ContentLayoutProduct on Product{name badges{...BadgesFragment}canonicalUrl classType availabilityStatus showAtc averageRating fulfillmentBadge fulfillmentSpeed fulfillmentTitle fulfillmentType imageInfo{thumbnailUrl}numberOfReviews offerId orderMinLimit orderLimit p13nDataV1{predictedQuantity flags{PREVIOUSLY_PURCHASED{text}CUSTOMERS_PICK{text}}}previouslyPurchased{label}preOrder{...PreorderFragment}priceInfo{currentPrice{...ProductPriceFragment}listPrice{...ProductPriceFragment}subscriptionPrice{priceString}priceDisplayCodes{clearance eligibleForAssociateDiscount finalCostByWeight hidePriceForSOI priceDisplayCondition pricePerUnitUom reducedPrice rollback strikethrough submapType unitOfMeasure unitPriceDisplayCondition}priceRange{minPrice maxPrice priceString}unitPrice{...ProductPriceFragment}wasPrice{...ProductPriceFragment}}rhPath salesUnit sellerId sellerName hasSellerBadge seller{name sellerId}shippingOption{slaTier shipMethod}showOptions snapEligible sponsoredProduct{spQs clickBeacon spTags}usItemId variantCount variantCriteria{name id variantList{name swatchImageUrl selectedProduct{usItemId canonicalUrl}}}}fragment FitmentLabel on FitmentLabels{links{...FitmentLabelEntity}messages{...FitmentLabelEntity}ctas{...FitmentLabelEntity}images{...FitmentLabelEntity}}fragment FitmentVehicleFragment on FitmentVehicle{vehicleYear{...FitmentVehicleFieldFragment}vehicleMake{...FitmentVehicleFieldFragment}vehicleModel{...FitmentVehicleFieldFragment}additionalAttributes{...FitmentVehicleFieldFragment}}fragment FitmentVehicleFieldFragment on FitmentVehicleField{id value label}fragment FitmentFieldFragment on FitmentField{id value displayName data{value label}extended dependsOn}fragment FitmentLabelEntity on FitmentLabelEntity{id label}fragment BuyBoxAdConfigsFragment on TempoWM_GLASSWWWBuyBoxAdConfigs{_rawConfigs moduleLocation lazy ad{...SponsoredProductsAdFragment}}fragment MarqueeDisplayAdConfigsFragment on TempoWM_GLASSWWWMarqueeDisplayAdConfigs{_rawConfigs ad{...DisplayAdFragment}}fragment DisplayAdFragment on Ad{...AdFragment adContent{type data{__typename...AdDataDisplayAdFragment}}}fragment AdFragment on Ad{status moduleType platform pageId pageType storeId stateCode zipCode pageContext moduleConfigs adsContext adRequestComposite}fragment AdDataDisplayAdFragment on AdData{...on DisplayAd{json status}}fragment SkylineDisplayAdConfigsFragment on TempoWM_GLASSWWWSkylineDisplayAdConfigs{_rawConfigs ad{...SkylineDisplayAdFragment}}fragment SkylineDisplayAdFragment on Ad{...SkylineAdFragment adContent{type data{__typename...SkylineAdDataDisplayAdFragment}}}fragment SkylineAdFragment on Ad{status moduleType platform pageId pageType storeId stateCode zipCode pageContext moduleConfigs adsContext adRequestComposite}fragment SkylineAdDataDisplayAdFragment on AdData{...on DisplayAd{json status}}fragment SponsoredProductCarouselConfigsFragment on TempoWM_GLASSWWWSponsoredProductCarouselConfigs{_rawConfigs moduleType ad{...SponsoredProductsAdFragment}}fragment SponsoredProductsAdFragment on Ad{...AdFragment adContent{type data{__typename...AdDataSponsoredProductsFragment}}}fragment AdDataSponsoredProductsFragment on AdData{...on SponsoredProducts{adUuid adExpInfo moduleInfo products{...ProductFragment}}}fragment ProductFragment on Product{usItemId offerId badges{flags{key text}labels{key text}tags{key text}}priceInfo{priceDisplayCodes{rollback reducedPrice eligibleForAssociateDiscount clearance strikethrough submapType priceDisplayCondition unitOfMeasure pricePerUnitUom}currentPrice{price priceString}wasPrice{price priceString}priceRange{minPrice maxPrice priceString}unitPrice{price priceString}}showOptions sponsoredProduct{spQs clickBeacon spTags}canonicalUrl numberOfReviews averageRating availabilityStatus imageInfo{thumbnailUrl allImages{id url}}name fulfillmentBadge classType type p13nData{predictedQuantity flags{PREVIOUSLY_PURCHASED{text}CUSTOMERS_PICK{text}}labels{PREVIOUSLY_PURCHASED{text}CUSTOMERS_PICK{text}}}}

Variables
""""""""""""
Variables
    - **semStoreId** (str) - неизвестно. По дефолту null. Относится к `product`.
    - **selected** (bool) - неизвестно. По дефолту true. Относится к `product`.
    - **channel** (str) - Тип contentLayout. По дефолту WWW. Относится к `contentLayout`.
    - **pageType** (str) - Тип страницы contentLayout. По дефолту ItemPageGlobal. Относится к `contentLayout`.
    - **tenant** (str) - Тип макета страницы contentLayout. По дефолту WM_GLASS. Относится к `contentLayout`.
    - **version** (str) - Версия contentLayout. Относится к `contentLayout`.
    - **layout** (str) - Тип устройства contentLayout. Относится к `contentLayout`.
    - **tempo** (str) - Метаинформация для contentLayout. Относится к `contentLayout`::

            {
                "targeting": "",
                "params": [
                    {"key": "expoVars", "value": "expoVariationValue"},
                    {"key": "expoVars", "value": "expoVariationValue2"}
                ]
            }

    - **page** (int) - Порядковый номер пагинации ревью. Относится к `reviews`.
    - **limit** (int) - Количество ревью на пагинации. Относится к `reviews`.
    - **sort** (str) - Сортировка ревью. Относится к `reviews`.
    - **filters** ([str]) - Фильтры ревью. Относится к `reviews`.
    - **p13N** (object) - Метаинформация о запросе.  Минимальный набор параметров::

            "p13N": {
                "reqId": "",
                "pageId": "",
                "modules": [],
                "userClientInfo": {
                    "deviceType": ""
                },
                "userReqInfo": {}
            }

    - **p13nCls** (object) - Интерфейс для p13N.
    - **fetchBuyBoxAd** (bool) - Нужно ли возвращать информацию о BuyBoxAd. Относится к `contentLayout`.
    - **fetchMarquee** (bool) - Нужно ли возвращать информацию о Marquee. Относится к `contentLayout`.
    - **fetchSkyline** (bool) - Нужно ли возвращать информацию о Skyline. Относится к `contentLayout`.
    - **fetchSpCarousel** (bool) - Нужно ли возвращать информацию о SpCarousel. Относится к `contentLayout`.

Пример переменных:
    .. code-block::

        {"itemId":"231791272","semStoreId":null,"catalogSellerId":"101007063","selected":true,"channel":"WWW","pageType":"ItemPageGlobal","tenant":"WM_GLASS","version":"v1","layout":["itemDesktop"],"tempo":{"targeting":"%7B%22userState%22%3A%22loggedIn%22%7D","params":[{"key":"expoVars","value":"expoVariationValue"},{"key":"expoVars","value":"expoVariationValue2"}]},"page":1,"limit":10,"sort":"relevancy","filters":[],"p13N":{"reqId":"ngWu9kvQeGSh0KgoRhCjyfnFF7lozS3Gd9MT","pageId":"231791272","modules":[{"moduleType":"PersonalizedLabels","moduleId":"234-sdfsfvns-sdfdskvl"}],"userClientInfo":{"ipAddress":"IP=0:0:0:0:0:0:0:1-0:0:0:0:0:0:0:1","isZipLocated":true,"callType":"CLIENT","deviceType":"desktop"},"userReqInfo":{"refererContext":{"source":"itempage"},"pageUrl":"/ip/Knee-Brace-Compression-Support-Sleeve-Adjustable-Strap-Pad-Pain-Relief-Meniscus-Tear-Arthritis-ACL-MCL-Quick-Recovery-Running-Basketball-Crossfit/231791272?athcpid=231791272&athpgid=sellerpage_522636284&athcgid=null&athznid=mtsi&athieid=v0&athstid=CS020&athguid=8fak-MXtapHYRdDAdGz_8UvvG-KFMsDcCKRo&athancid=null&athena=true&selectedSellerId=101007063"}},"p13nCls":{"pageId":"231791272","userClientInfo":{"ipAddress":"IP=0:0:0:0:0:0:0:1-0:0:0:0:0:0:0:1","isZipLocated":true,"callType":"CLIENT","deviceType":"desktop"},"userReqInfo":{"refererContext":{"source":"itempage"}}},"fetchBuyBoxAd":true,"fetchMarquee":true,"fetchSkyline":true,"fetchSpCarousel":true}

Response
~~~~~~~~~~~
Стандартный ответ на верхнем уровне состоит из нескольких частей:
::

    {
        "data": {
            "contentLayout": {...},
            "product": {...},
            "idml": {...},
            "reviews": {...}
        }
    }

- data.contentLayout - Содержит результат поиска и некоторые метаданные.
    - modules - содержит информацию о различных конфигурациях таких как: marquee, ad, fitment etc.
    - layouts - набор параметров, определяющих местоположение объектов на странице.
    - pageMetadata - метаинформация о странице::

        "pageMetadata": {
            "location": {}, - информация и местоположения пользователя
            "pageContext": {} - информация о контексте страницы
        }

- data.product - содержит информацию о целевом продукте.
- data.idml - содержит контент секции `About this item <https://monosnap.com/file/ciymqSkuys0bKshEu39z0fP9OukZxL>`_.
- data.reviews - содержит информацию об отзывах.

.. admonition:: Response example
    :class: note

    Полный пример ответа для товара с id "14532503": :download:`link <data/item_by_id_response.json5>`
