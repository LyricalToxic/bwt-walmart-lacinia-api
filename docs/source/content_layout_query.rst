ContentLayout
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

Запрос предназначен для поиска информации об офферах продукта на основании его item id. \

.. HTTP метод и endpoint.

Поиск информации об офферах продукта на сайте walmart можно выполнить используя `post` запрос на endpoint:
::

    https://www.walmart.com/orchestra/home/graphql

.. Описание свойств запроса.

Для запроса являются обязательными заголовки:
::

    Accept: application/json
    Content-Type: application/json
    WM_MP: True
    X-APOLLO-OPERATION-NAME: FunctionName

.. Описание ответа.

Результатом поиска является информация об офферах продуктах.
Офферы не разбиваются на пагинацию, а приходят все сразу.

.. Особенности

- нет информации о локации. Локацию можно добавить, указав location в query
- детальная информация о продавцах отсутствует

Body
~~~~~~~~~~~

Query
"""""""""""
.. function:: contentLayout(...):

    :parameter JSON $fitmentFieldParams: \

        Default = {} \

        Параметры автомобиля при поиске товаров для автомобиля. \

    :parameter JSON $fitmentSearchParams: \

        Default = {} \

        Параметры поиска. Дублирует основные параметры поиска. Необязательное. \

    :parameter Boolean $fetchMarquee!: \

        Будет ли приходить marquee конфигурации в contentLayout.  \

        Предположительно вид рекламы.\


    :parameter Boolean $fetchSkyline!: \

        Будет ли приходить skyline конфигурации в contentLayout. \

        Предположительно вид рекламы.

    :parameter Boolean $fetchSbaTop!: \

        Будет ли приходить sbatop конфигурации в contentLayout. \

        Предположительно вид рекламы.
        Находятся в ответе между продуктами и имеют __typename=SponsoredBrands. `Пример <https://monosnap.com/file/1GbI0G0TS9mGdNvyjsvoUh6CPlu4CK>`_. \

Пример запроса:
    .. code-block::

        query GetAllSellerOffers( $itemId:String! $postalAddress:PostalAddress $storeFrontIds:[StoreFrontId]){product( itemId:$itemId postalAddress:$postalAddress storeFrontIds:$storeFrontIds ){  allOffers{offerId offerType availabilityStatus fulfillmentType fulfillmentBadge sellerId catalogSellerId sellerName sellerDisplayName sellerType wfsEnabled hasSellerBadge priceInfo{priceDisplayCodes{eligibleForAssociateDiscount}currentPrice{price priceString priceType}wasPrice{price}priceRange{minPrice maxPrice priceString}unitPrice{price priceString}}returnPolicy{returnable freeReturns returnWindow{value unitType}}shippingOption{shipPrice{price priceString}}}location{postalCode city}}}

Variables
""""""""""""
Variables
    - **itemId** (str) - Уникальный идентификатор товара. По дефолту null. Относится к `product`.

Пример переменных:
    .. code-block::

        {"itemId":"231791272"}

Response
~~~~~~~~~~~
Стандартный ответ на верхнем уровне:
::

    {
        "data": {
            "product": {
                "allOffers": {...}
            }
        }
    }


.. admonition:: Response example
    :class: note

    Полный пример ответа для товара с id "14532503": :download:`link <data/offers_response.json5>`

