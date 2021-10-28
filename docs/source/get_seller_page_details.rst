GetSellerPageDetails
---------------------
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

Запрос предназначен для получения информации о продавце. \

.. HTTP метод и endpoint.

Поиск информации на сайте walmart можно выполнить используя `post` запрос на endpoint:
::

    https://www.walmart.com/orchestra/home/graphql

.. Описание свойств запроса.

Для запроса являются обязательными заголовки:
::

    Accept: application/json
    Content-Type: application/json
    WM_MP: True
    X-APOLLO-OPERATION-NAME: AnyFunctionName

Обязательным параметром является catalogSellerId.

.. Описание ответа.

Результатом поиска является информация о продавце \

.. Особенности

- за раз можно получить информацию о нескольких продавцах используя `alias <https://graphql.org/learn/queries/#aliases>`_

Body
~~~~~~~~~~~

Query
"""""""""""

.. function:: seller(...)

    .. admonition:: Schema reference
            :class: info

            Description: null

    :parameter String $catalogSellerId!:
        .. admonition:: Schema reference
                :class: info

                Description: catalog Seller Id  of primary seller of product

        Уникальный идентификатор страницы продавца. \

Пример запроса:
    .. code-block::

        query GetSellerPageDetails( $catalogSellerId:String! ){seller(catalogSellerId:$catalogSellerId){...SellerFragment}}fragment SellerFragment on Seller{sellerTaxPolicy catalogSellerId sellerId sellerName sellerDisplayName sellerPhone sellerEmail sellerType sellerDisplayName sellerLogoURL hasSellerBadge address1 address2 city state postalCode country countryCode}


Пример переменных:
    .. code-block::

       {"catalogSellerId": "4650"}

Response
~~~~~~~~~~~
Ответ содержит объект Seller:
::

    {
        "data": {
            "seller": {Seller}
        }
    }

- data.seller:Seller
    - seller.sellerId:String - уникальный хэш продавца
    - seller.catalogSellerId:String - уникальный идентификатор страницы продавца
    - seller.sellerName:String - имя продавца в системе walmart
    - seller.sellerDisplayName:String - отображаемое имя продавца
    - seller.proSeller:Boolean - deprecated
    - seller.hasSellerBadge:Boolean - является ли продавец профессиональным
    - seller.sellerType:String - тип продавца
    - seller.sellerEmail:String - почта продавца
    - seller.sellerPhone:String - телефон продавца
    - seller.returnPolicyText:String - политика возращения товаров
    - seller.sellerLogoURL:String - ссылка на лого продавца
    - seller.sellerAboutUs:String - описание продавца
    - seller.sellerTaxPolicy:String - налоговая политика продавца
    - seller.address1:String - адрес
    - seller.address2:String - адрес
    - seller.city:String - город
    - seller.state:String - штат
    - seller.postalCode:String - зип код
    - seller.country:String - страна
    - seller.sellerReviews:SellerReview - ревью


.. admonition:: Response example
    :class: note

    Полный пример ответа для продавца "4650": :download:`link <data/seller_response.json5>`

UI-Response table comparison
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. _about: https://monosnap.com/file/ZFDseFYGv55yVbltJxOX8T5MMeIZRE
.. |about| replace:: About seller

.. _tax: https://monosnap.com/file/gFpVcvIXlCtgHTZvfAs86VCrhhYYwD
.. |tax| replace:: Tax policy

.. _name: https://monosnap.com/file/TvTmaxsPED9ncYQ6qyvMB2PYd7VqNC
.. |name| replace:: Seller name

.. _phone: https://monosnap.com/file/8romvcLL0bYWFDvM70fw2SVVR1dpf4
.. |phone| replace:: Seller phone

.. _logo: https://monosnap.com/file/f0fn2xDt8DP3PVhXxLXyUBOLj0TXgo
.. |logo| replace:: Seller logo

.. _address: https://monosnap.com/file/unmFpLbDFLeftb3ypMPC1K3omCbEl7
.. |address| replace:: Seller address


+------------+-------------------------------+-------------------------------+
| Title      | Description                   | JSON-Path                     |
+============+===============================+===============================+
| |about|_   | Seller description            | data.seller.sellerAboutUs     |
+------------+-------------------------------+-------------------------------+
| |tax|_     | Seller tax policy description | data.seller.sellerTaxPolicy   |
+------------+-------------------------------+-------------------------------+
| |name|_    | Seller display name           | data.seller.sellerDisplayName |
+------------+-------------------------------+-------------------------------+
| |phone|_   | Seller clear phone            | data.seller.sellerPhone       |
+------------+-------------------------------+-------------------------------+
| |logo|_    | Seller logo                   | data.seller.sellerLogoURL     |
+------------+-------------------------------+-------------------------------+
| |address|_ | Seller full address           | data.seller.address1          |
|            |                               | data.seller.address2          |
|            |                               | data.seller.city              |
|            |                               | data.seller.state             |
|            |                               | data.seller.postalCode        |
|            |                               | data.seller.country           |
|            |                               | data.seller.countryCode       |
+------------+-------------------------------+-------------------------------+

