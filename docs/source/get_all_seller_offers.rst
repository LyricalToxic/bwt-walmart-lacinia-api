GetAllSellerOffers
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

Запрос предназначен для поиска информации об оферах продукта на основании его item id. \

.. HTTP метод и endpoint.

Поиск информации об оферах продукта на сайте walmart можно выполнить используя `post` запрос на endpoint:
::

    https://www.walmart.com/orchestra/home/graphql

.. Описание свойств запроса.

Для запроса являются обязательными заголовки:
::

    Accept: application/json
    Content-Type: application/json
    WM_MP: True
    X-APOLLO-OPERATION-NAME: GetAllSellerOffers

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
.. function:: query GetAllSellerOffers(...)

    :parameter String $itemId!: \

        Уникальный идентификатор товара на walmart. \

        Число. \

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

Пример запроса:
    .. code-block::

        query GetAllSellerOffers( $itemId:String! $postalAddress:PostalAddress $storeFrontIds:[StoreFrontId]){product( itemId:$itemId postalAddress:$postalAddress storeFrontIds:$storeFrontIds ){  allOffers{offerId offerType availabilityStatus fulfillmentType fulfillmentBadge sellerId catalogSellerId sellerName sellerDisplayName sellerType wfsEnabled hasSellerBadge priceInfo{priceDisplayCodes{eligibleForAssociateDiscount}currentPrice{price priceString priceType}wasPrice{price}priceRange{minPrice maxPrice priceString}unitPrice{price priceString}}returnPolicy{returnable freeReturns returnWindow{value unitType}}shippingOption{shipPrice{price priceString}}}}}

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
