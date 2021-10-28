Overview
==========

Цель (Objective)
------------------
Целью данного документа является предоставить описание ``walmart back-end api``, взаимодействие с ним, а также всевозможные линии поведения.


Описание (Description)
------------------------
В этой документации описан интерфейс ``walmart back-end api``.

Тело запроса состоит из

    - query - graphql запрос
    - variables - переменные, которые подставляются в query.

Заметки:

    - название функций запроса (query AnyFunctionName(...){}) и заголовка X-APOLLO-OPERATION-NAME должны совпадать.
    - AnyFunctionName может быть произвольным для любого запроса и **предположительно должен соответствовать правилам именование функций Clojure**.
    - для локации нужен ACID + locDataV3


Introspection
---------------

Результат интроспекции :download:`introspection result <data/introspection_result.json5>`
