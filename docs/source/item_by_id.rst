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

Запрос предназначен для получения информации об item. \

.. HTTP метод и endpoint.

Поиск информации на сайте walmart можно выполнить используя `post` запрос на endpoint:
::

    https://www.walmart.com/orchestra/home/graphql или https://www.walmart.com/orchestra/home/graphql/ip/{item_id}

.. Описание свойств запроса.

.. Описание ответа.

Результатом поиска является информация об item.


.. Особенности


- в случае если itemId не был найден, то будет возвращена :download:`ошибка 404 в теле ответа <jsons/item_by_id_404_response.json5>`.
- если item out of stock, тогда информации о продавце может и не быть.

Body
~~~~~~~~~~~

Query
"""""""""""

.. function:: product(...)

    .. admonition:: Schema reference
            :class: info

            Description: null

    :parameter String $itemId!: \

        .. admonition:: Schema reference
                :class: info

                Description: Item ID, returned in Product/usItemId field. Required for post-transaction scenario. \

        Уникальный идентификатор item-a. \

    :parameter String $offerId: \

        .. admonition:: Schema reference
                :class: info

                Description: Offer id of the Product. \

        Уникальный идентификатор оффера. \

    :parameter PostalAddress $postalAddress: \

        .. admonition:: Schema reference
                :class: info

                Description: null

        Местоположение пользователя. Не влияет на фактическую локацию. Фактическая локация хранится в куках. \

        Состоит из полей::

                {
                    "postalCode": str,
                    "zipLocated": bool,
                    "stateOrProvinceCode": str,
                    "stateOrProvinceName": str,
                    "countryCode": str,
                    "addressType": str,
                    "isPoBox": bool,
                }

    :parameter [StoreFrontId] $storeFrontIds: \

        .. admonition:: Schema reference
                :class: info

                Description: null

        Уникальный идентификатор магазина. \

        Состоит из полей::

                {
                    "usStoreId": int,
                    "preferred": bool,
                    "distance": float,
                    "inStore": bool,
                    "lastPickupStore": bool,
                }


    :parameter Boolean $selected: \

        .. admonition:: Schema reference
                :class: info

                Description: True if the current variant is selected"

        Неизвестно \

    :parameter P13NRequest $p13N: \

        .. admonition:: Schema reference
                :class: info

                Description: null

        Обязательный параметр, содержащий метаинформацию о запросе. Нужен для modules. \

    :parameter String $variantFieldId: \

        .. admonition:: Schema reference
                :class: info

                Description: null

        Неизвестно \

    :parameter Int $semStoreId: \

        .. admonition:: Schema reference
                :class: info

                Description: Optional, required only for the SEM (Search Engine Marketing) use case. The storeId for the advertised SEM store. Required for store comparison and for temporarily setting the users store to the SEM store if required.

        Неизвестно. \

    :parameter String $catalogSellerId: \

        .. admonition:: Schema reference
                :class: info

                Description: catalog Seller Id  of primary seller of product

        Уникальный идентификатор страницы продавца. \

    :parameter String $fulfillmentIntent: \

        .. admonition:: Schema reference
                :class: info

                Description: null

        Неизвестно. \

    :parameter [ComponentOfferDetail] $componentOffers: \

        .. admonition:: Schema reference
                :class: info

                Description: null

        Неизвестно. \

Пример запроса:
    .. code-block::

        query ItemById($catalogSellerId:String $semStoreId: Int $postalAddress:PostalAddress $itemId:String! $selected:Boolean $variantFieldId:String $storeFrontIds:[StoreFrontId] $p13N:P13NRequest  $fulfillmentIntent:String  ){product( catalogSellerId:$catalogSellerId itemId:$itemId postalAddress:$postalAddress storeFrontIds:$storeFrontIds selected:$selected semStoreId:$semStoreId p13N:$p13N variantFieldId:$variantFieldId fulfillmentIntent:$fulfillmentIntent ){...FullProductFragment}}fragment FullProductFragment on Product{blitzItem giftingEligibility shipAsIs subscriptionEligible showFulfillmentLink additionalOfferCount shippingRestriction availabilityStatus averageRating suppressReviews brand badges{...BadgesFragment}rhPath partTerminologyId aaiaBrandId manufacturerProductId productTypeId tireSize tireLoadIndex tireSpeedRating viscosity model buyNowEligible earlyAccessEvent showBuyWithWplus preOrder{...PreorderFragment}canonicalUrl catalogSellerId sellerReviewCount sellerAverageRating category{...ProductCategoryFragment}classType classId fulfillmentTitle shortDescription fulfillmentType fulfillmentBadge checkStoreAvailabilityATC fulfillmentLabel{checkStoreAvailability wPlusFulfillmentText message shippingText fulfillmentText locationText fulfillmentMethod addressEligibility fulfillmentType postalCode}hasSellerBadge itemType id imageInfo{...ProductImageInfoFragment}location{postalCode stateOrProvinceCode city storeIds}manufacturerName name numberOfReviews orderMinLimit orderLimit offerId offerType priceInfo{priceDisplayCodes{...PriceDisplayCodesFragment}currentPrice{...ProductPriceFragment}wasPrice{...ProductPriceFragment}unitPrice{...ProductPriceFragment}savings{priceString}subscriptionPrice{price priceString intervalFrequency duration percentageRate subscriptionString}priceRange{minPrice maxPrice priceString currencyUnit unitOfMeasure denominations{price priceString selected}}}returnPolicy{returnable freeReturns returnWindow{value unitType}}fsaEligibleInd sellerId sellerName sellerDisplayName secondaryOfferPrice{currentPrice{priceType priceString price}}semStoreData{pickupStoreId deliveryStoreId isSemLocationDifferent}shippingOption{...ShippingOptionFragment}type pickupOption{slaTier accessTypes availabilityStatus storeName storeId}salesUnit usItemId variantCriteria{id categoryTypeAllValues name type variantList{availabilityStatus id images name products swatchImageUrl selected}}variants{...MinimalProductFragment}groupMetaData{groupType groupSubType numberOfComponents groupComponents{quantity offerId componentType productDisplayName}}upc wfsEnabled sellerType ironbankCategory snapEligible showAddOnServices addOnServices{serviceType serviceTitle serviceSubTitle groups{groupType groupTitle assetUrl shortDescription services{displayName offerId selectedDisplayName currentPrice{price priceString}}}}productLocation{displayValue}}fragment BadgesFragment on UnifiedBadge{flags{__typename...on BaseBadge{id text key query type}...on PreviouslyPurchasedBadge{id text key lastBoughtOn numBought criteria{name value}}}labels{__typename...on BaseBadge{id text key}...on PreviouslyPurchasedBadge{id text key lastBoughtOn numBought}}tags{__typename...on BaseBadge{id text key}}}fragment ShippingOptionFragment on ShippingOption{accessTypes availabilityStatus slaTier deliveryDate maxDeliveryDate shipMethod shipPrice{...ProductPriceFragment}}fragment ProductCategoryFragment on ProductCategory{categoryPathId path{name url}}fragment PreorderFragment on PreOrder{streetDate streetDateDisplayable streetDateType isPreOrder preOrderMessage preOrderStreetDateMessage}fragment MinimalProductFragment on Variant{availabilityStatus imageInfo{...ProductImageInfoFragment}priceInfo{priceDisplayCodes{...PriceDisplayCodesFragment}currentPrice{...ProductPriceFragment}wasPrice{...ProductPriceFragment}unitPrice{...ProductPriceFragment}}productUrl usItemId id:productId fulfillmentBadge}fragment ProductImageInfoFragment on ProductImageInfo{allImages{id url zoomable}thumbnailUrl}fragment PriceDisplayCodesFragment on PriceDisplayCodes{clearance eligibleForAssociateDiscount finalCostByWeight hidePriceForSOI priceDisplayCondition pricePerUnitUom reducedPrice rollback strikethrough submapType unitOfMeasure unitPriceDisplayCondition}fragment ProductPriceFragment on ProductPrice{price priceString variantPriceString priceType currencyUnit}

Пример переменных:
    .. code-block::

        {"itemId": "493824815","semStoreId": null, "selected": true, "filters": []}


Response
~~~~~~~~~~~

Ответ может содержать порядка 140 полей. Список всех полей можно найти в :download:`link <jsons/introspection_result.json5>`
::

    {
        "data": {
            "product": {...},
        }
    }

Некоторые поля из ответа:

- product.allOffers - список всех предложений
    .. admonition:: Schema reference
            :class: info

            Description: All product offers

    - allOffers.offerId - уникальный идентификатор оффера
        .. admonition:: Schema reference
            :class: info

            Description: offerId of the product

    - allOffers.offerType - тип оффера
        .. admonition:: Schema reference
            :class: info

            Description: offer Type

    - allOffers.availabilityStatus - статус доступности
        .. admonition:: Schema reference
            :class: info

            Description: Availability status of item

    - allOffers.fulfillmentType - тип выполнения заказа
        .. admonition:: Schema reference
            :class: info

            Description: fulfillment types - STORE or FC or MARKETPLACE

    - allOffers.fulfillmentBadge - время выполнения заказа
        .. admonition:: Schema reference
            :class: info

            Description: fulfillment badge

    - allOffers.fulfillmentTitle - название выполнения заказа
        .. admonition:: Schema reference
            :class: info

            Description: fulfillment types based on pickup and shipping

    - allOffers.sellerId - уникальный идентификатор основного продавца товара
        .. admonition:: Schema reference
            :class: info

            Description: Primary seller of the product

    - allOffers.catalogSellerId - уникальный идентификатор страницы основного продавца товара
        .. admonition:: Schema reference
            :class: info

            Description: catalog Seller Id  of primary seller of product

    - allOffers.sellerDisplayName - имя основного продавца, которое будет отображаться
        .. admonition:: Schema reference
            :class: info

            Description: seller display name of primary seller of product

    - allOffers.sellerType - тип основного продавца
        .. admonition:: Schema reference
            :class: info

            Description: seller type of primary seller of product for eg. INTERNAL

    - allOffers.wfsEnabled - выполняется ли заказ волмартом
        .. admonition:: Schema reference
            :class: info

            Description: WFS flag. Fulfilled by Walmart

    - allOffers.hasSellerBadge - профессиональный ли продавец
        .. admonition:: Schema reference
            :class: info

            Description: is Pro Seller

    - allOffers.priceInfo - информация о цене
        .. admonition:: Schema reference
            :class: info

            Description: All price information related tothe product. e.g. current price, was price, price format

    - allOffers.returnPolicy - политика возврата
        .. admonition:: Schema reference
            :class: info

            Description: Return policy

    - allOffers.shippingOption - информация о доставки товара
        .. admonition:: Schema reference
            :class: info

            Description: shipping details of the item

    - allOffers.pickupOption - информация о получении товара
        .. admonition:: Schema reference
            :class: info

            Description: pickup details of the item

    - allOffers.preOrder - информация о предзаказе
        .. admonition:: Schema reference
            :class: info

            Description: preOrder details of the item

- product.sellerId - уникальный идентификатор продавца
    .. admonition:: Schema reference
            :class: info

            Description: Primary seller of the product

- product.additionalOfferCount - количество офферов
    .. admonition:: Schema reference
            :class: info

            Description: Additional offer count

- product.availabilityStatus - статус доступности товара
    .. admonition:: Schema reference
            :class: info

            Description: Availability status of item

- product.averageRating - средний рейтинг от 0 до 5
    .. admonition:: Schema reference
            :class: info

            Description: rating of the product of 5

- product.brand - бренд
    .. admonition:: Schema reference
            :class: info

            Description: Brand of the product.

- product.canonicalUrl - ссылка на товар
    .. admonition:: Schema reference
            :class: info

            Description: canonical Url of the product for eg. /ip/Beef-Choice-Angus-New-York-Strip-Steak-0-82-1-57-lb/39944456

- product.catalogSellerId - уникальный идентификатор страницы продавца
    .. admonition:: Schema reference
            :class: info

            Description: catalog Seller Id  of primary seller of product

- product.category - категории и подкатегории продукта
    .. admonition:: Schema reference
            :class: info

            Description: Categories that the product falls under. There are mutiple category levels

- product.classType - тип класса продукта
    .. admonition:: Schema reference
            :class: info

            Description: Class type of the product.

- product.shortDescription - краткое описание товара. Содержит html теги
    .. admonition:: Schema reference
            :class: info

            Description: Short description of the product.

- product.detailedDescription - полное описание товара. Содержит html теги
    .. admonition:: Schema reference
            :class: info

            Description: Detailed description of the product.

- product.fulfillmentLabel - описание доставки
    .. admonition:: Schema reference
            :class: info

            Description: fulfillment label

- product.id - уникальный идентификатор страницы продукта
    .. admonition:: Schema reference
            :class: info

            Description: Unique product id

- product.imageInfo - информация об изображениях
    .. admonition:: Schema reference
            :class: info

            Description: All images for the product

- product.location - информация о локации
    .. admonition:: Schema reference
            :class: info

            Description: fulfillment location details

- product.name - названия товара
    .. admonition:: Schema reference
            :class: info

            Description: Name of the product.

- product.numberOfReviews - количество оценок
    .. admonition:: Schema reference
            :class: info

            Description: number of reviews of the product of 5

- product.offerId - уникальный идентификатор оффера
    .. admonition:: Schema reference
            :class: info

            Description: primary offer id of product

- product.offerType - тип офферов
    .. admonition:: Schema reference
            :class: info

            Description: offer type of primary offer of product for eg. ONLINE_ONLY, ONLINE_AND_STORE

- product.priceInfo - информация о цене
    .. admonition:: Schema reference
            :class: info

            Description: All price information related tothe product. e.g. current price, was price, price format

- product.sellerName - имя продавца
    .. admonition:: Schema reference
            :class: info

            Description: seller name of primary seller of product

- product.shippingOption - информация о типах доставки
    .. admonition:: Schema reference
            :class: info

            Description: shipping details of the item

- product.salesUnit - минимальное количество продаваемых единиц за раз
    .. admonition:: Schema reference
            :class: info

            Description: sales unit For e.g EACH

- product.usItemId - уникальный идентификатор товара
    .. admonition:: Schema reference
            :class: info

            Description: A unique reference id to identify the product. e.g. 646105256"

- product.variants - информация о продавцах
    .. admonition:: Schema reference
            :class: info

            Description: variants of the item

- product.upc - upc товара
    .. admonition:: Schema reference
            :class: info

            Description: UPC

- product.sellerType - тип продавца
    .. admonition:: Schema reference
            :class: info

            Description: seller type of primary seller of product for eg. INTERNAL


.. admonition:: Response example
    :class: note

    Полный пример ответа для товара с id "139340877": :download:`link <jsons/item_by_id_response.json5>`

UI-Response table comparison
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. _title: https://monosnap.com/file/BjKYYlkxVlEjWbo4BTvmnBmfiJJv5Z
.. |title| replace:: Title

.. _avg_rating: https://monosnap.com/file/IPsiMU8iVqDEi6W039Si7qm0lquqkY
.. |avg_rating| replace:: Average rating

.. _number_reviews: https://monosnap.com/file/lwJtWjP45T6rLOPBlS14hgFaF0ezwj
.. |number_reviews| replace:: Number of reviews

.. _brand: https://monosnap.com/file/pksi3Wf6k7Bx6XY2naaaN0VOmKIJxc
.. |brand| replace:: Brand

.. _categories: https://monosnap.com/file/g4m0k9gvsUvnU31OYkbrkmoOvGo1Ky
.. |categories| replace:: Categories

.. _price: https://monosnap.com/file/wdnCPPjk5CEmHX1IMNAzvABDGZCKIO
.. |price| replace:: Price

.. _variants: https://monosnap.com/file/mB6aumUbXjPOxx9PdY90mJFm7iRNfU
.. |variants| replace:: Variants

.. _fulfillment: https://monosnap.com/file/rZZogORcDnvZuaVfIGwkL90RrXAlKD
.. |fulfillment| replace:: Fulfillment

.. _seller: https://monosnap.com/file/hTY61775be68dnXNWgKuDQcDBlTwAe
.. |seller| replace:: Seller

.. _images: https://monosnap.com/file/pOWiAmGtU39eKEzTEO4dlWgHLiSdrN
.. |images| replace:: Images


+--------------------------------------------------------------------------------+
|                     Product                                                    |
+-------------------+---------------------------+--------------------------------+
| Title             | Description               | JSON-Path                      |
+===================+===========================+================================+
| |title|_          | Product title             | data.product.name              |
+-------------------+---------------------------+--------------------------------+
| |avg_rating|_     | Average product rating    | data.product.averageRating     |
+-------------------+---------------------------+--------------------------------+
| |number_reviews|_ | Product number of reviews | data.product.numberOfReviews   |
+-------------------+---------------------------+--------------------------------+
| |brand|_          | Product brand             | data.product.brand             |
+-------------------+---------------------------+--------------------------------+
| |categories|_     | Product categories        | data.product.category          |
+-------------------+---------------------------+--------------------------------+
| |price|_          | Product price info        | data.product.priceInfo         |
+-------------------+---------------------------+--------------------------------+
| |variants|_       | Product variants          | data.product.variants          |
+-------------------+---------------------------+--------------------------------+
| |fulfillment|_    | Product fulfillment info  | data.product.fulfillmentLabel  |
+-------------------+---------------------------+--------------------------------+
| |seller|_         | Main product seller       | data.product.sellerDisplayName |
+-------------------+---------------------------+--------------------------------+
| |images|_         | Product images            | data.product.imageInfo         |
+-------------------+---------------------------+--------------------------------+

.. _o_price: https://monosnap.com/file/jDuyeIPgiFA41KRKLwnh7F6TE4SpPc
.. |o_price| replace:: Offer price

.. _o_seller: https://monosnap.com/file/FIzbDJNrUvQM29p6DEyebNMD8ZbOXp
.. |o_seller| replace:: Offer seller

.. _is_pro: https://monosnap.com/file/l46Lh0sWt9gs8OVHv2mqGijKyyNeyl
.. |is_pro| replace:: Offer pro seller

.. _shipping: https://monosnap.com/file/mY3O3bM8GpRWfsWIq5sLlvbMw5xuSV
.. |shipping| replace:: Offer shipping info

.. _returning: https://monosnap.com/file/Ds1dDZpIrPilRHdGWWzEfWfdcpU0E8
.. |returning| replace:: Offer returning policy


+--------------+------------------------+---------------------------------------------+
|               ProductOffer                                                          |
+--------------+------------------------+---------------------------------------------+
| Title        | Description            | JSON-Path                                   |
+==============+========================+=============================================+
| |o_price|_   | Offer price info       | data.product.allOffers[i].priceInfo         |
+--------------+------------------------+---------------------------------------------+
| |o_seller|_  | Offer seller           | data.product.allOffers[i].sellerDisplayName |
+--------------+------------------------+---------------------------------------------+
| |is_pro|_    | Is whether seller pro  | data.product.allOffers[i].hasSellerBadge    |
+--------------+------------------------+---------------------------------------------+
| |shipping|_  | Offer shipping info    | data.product.allOffers[i].shippingOption    |
+--------------+------------------------+---------------------------------------------+
| |returning|_ | Offer returning policy | data.product.allOffers[i].returnPolicy      |
+--------------+------------------------+---------------------------------------------+
