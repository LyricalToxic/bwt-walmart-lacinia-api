Обзор (Overview)
==================

Цель (Objective)
------------------
1. Описать алгоритм составления http запросов.
2. Обзор GraphQL документа.
3. Составление graphql документа.
4. Предоставить пример и описание нескольких документов.
5. Предоставить файл с результатами интроспекции.

Описание (Description)
------------------------
.. Конечная точка

Взаимодействие с ``walmart back-end api`` происходит через https протокол. \
Основная конечная точка, на которую выполняются запросы::

    https://www.walmart.com/orchestra/home/graphql

Она также может иметь дополнительные сегменты пути (будут приведены для каждого запроса в описании), но фактически они не на что не влияют.

.. Тело

Тело запроса состоит из

- query - graphql документ.
- variables - переменные для query.

.. Заголовоки

Обязательными заголовки:
::

    Accept: application/json
    Content-Type: application/json
    WM_MP: True
    X-APOLLO-OPERATION-NAME: AnyOperationName

GraphQL документ (GraphQL document)
---------------------------------------


.. tab:: Документ GraphQL (GraphQL document)

    .. admonition:: Определение (Definition)
        :class: note

        ``Документ GraphQL (GraphQL document)``. Строка на языке GraphQl, описывающая одну или несколько операций или фрагментов.

.. tab:: Операция (Operation)

    .. admonition:: Определение (Definition)
        :class: note

        ``Операция (Operation)``. Единичный запрос данных, мутация или подписка, которые интерпретирует исполняемый модуль GraphQL.

Общий вид документа graphql::

    %operation type% %Operation name%(%variable definitions%){
        %selection set%
        %field%(%argument%){
            %field%
        }
    }

.. image:: images/graphql_operation.png
    :height: 300

.. tab:: Тип операции (Operation type)

    .. admonition:: Определение (Definition)
        :class: note

        ``Тип операции (Operation type)``. Возможно одно из трех значений: query, mutation, subscription, что указывает на тип выполняемой операции.

.. tab:: Имя операции (Operation name)

    .. admonition:: Определение (Definition)
        :class: note

        ``Имя операции (Operation name)``. Имя операции подобно имени функции в языке программирования.

.. tab:: Определение переменных (Variable definitions)

    .. admonition:: Определение (Definition)
        :class: note

        ``Определение переменных (Variable definitions)``. Запрос GraphQL может иметь динамическую часть, которая меняется при разных обращениях к серверу, в то время как текст запроса остается постоянным. Это переменные запроса.

.. tab:: Выборка (Selection set)

    .. admonition:: Определение (Definition)
        :class: note

        ``Выборка (Selection set)``. Набор полей, запрашиваемых в операции или внутри другого поля. Для поля необходимо указать выборку, если поле возвращает объектный тип данных. Напротив, для скалярных полей типа Int и String не допускается указывать выборку.

.. tab:: Поле (Field)

    .. admonition:: Определение (Definition)
        :class: note

        ``Поле (Field)``. Единица запрашиваемых данных, которая становится полем в ответе JSON.

.. tab:: Аргументы (Arguments)

    .. admonition:: Определение (Definition)
        :class: note

        ``Аргументы (Arguments)``. Набор пар ключ-значение, связанных с конкретным полем. Они передаются на сервер обработчику поля и влияют на получение данных.


Переменные передаются отдельно от текста запроса в формате. GraphQL обычно используется JSON. Пример::

    {
        "some_variables": "Some value"
    }


.. admonition:: Определение (Definition)
    :class: note

    ``Переменные (Variables)``. Словарь значений, сопутствующий операции GraphQL. Содержит динамические параметры операции.

Дополнительные возможности GraphQL (необязательно):

- `Фрагменты (fragments) <https://graphql.org/learn/queries/#fragments>`_
- `Директивы (directives) <https://graphql.org/learn/queries/#directives>`_

Алгоритм построения GraphQL документа (GraphQL document building algorithm)
-----------------------------------------------------------------------------

Для walmart backend graphql schema определены 2 специальных типа:

- Query - объект, для построения запросов на извлечения данных. Документация сфокусирована на этом типе запроса.
- Mutation - объект, для внесение изменений(мутаций) на стороне сервера. Документация не покрывает этот тип запросов.

В файле с результатами интроспекции содержится описание полей для объекта Query, которые можно извлечь,
указав их в секции `%selection set%`. Поле fields содержит порядка 102 типов. Описание для каждого типа содержится в:

- name - содержит название типа.
- description - описание типа
- args - аргументы, которые принимает тип.
- type - тип возвращаемого объекта.
- isDeprecated - была ли прекращена поддержка типа.
- deprecationReason - причина прекращения поддержки типа.

Итого окончательный алгоритм построение GraphQL документа:

1. Указать тип операции


Интроспекция (Introspection)
------------------------------

`GraphQL schema <https://graphql.org/learn/schema/>`_ можно `интроспектировать <https://graphql.org/learn/introspection/>`_.
Результат интроспекции :download:`introspection result <jsons/introspection_result.json5>`.
Все описанные выше типы/объекты содержаться в этом файле.


Заметки:

- название функций запроса (query AnyFunctionName(...){}) и значение заголовка X-APOLLO-OPERATION-NAME должны совпадать.
- AnyFunctionName может быть произвольным для любого запроса и **предположительно должен соответствовать правилам именование функций Clojure**.
- для локации нужен ACID + locDataV3
