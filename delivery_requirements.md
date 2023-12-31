# Требования к расчёту доставки курьерскими службами
У каждой курьерской службы есть своя ручка.

Запрос на URL службы доставки возвращает следующие параметры:

"name": Название сервиса
 
 "isItPossibleToDeliver": Возможно ли доставить (true/false)
 
"hostDeliveryCost": Стоимость доставки для нас

 "toBeDeliveredTime": Ограничения доставки по времени
 
 "clientDeliveryCost": Стоимость доставки для клиента

- name — всегда одинаковое при обращении к одной и той же службе
доставки. Значения берутся из таблицы с параметрами в требованиях.
- toBeDeliveredTime — диапазон времени, за которое служба выполнит
доставку. Всегда одинаковое при обращении к одной и той же службе
доставки. Значения параметров приведены в таблице в требованиях.
- В самом запросе deliveryTime — время, к которому пользователь
ожидает доставку; в часах.
- isItPossibleToDeliver — true , когда время deliveryTime попадает в
диапазон работы курьерской службы, если не попадает — false .
- hostDeliveryCost — стоимость внутренней доставки рассчитывается в
соответствии с таблицей.

Вычисление зависит от передаваемых переменных productsCount
(количество продуктов) и productsWeight (вес продуктов).

Например, для службы доставки «Доставка Москва»:
ЕСЛИ productsCount меньше или равно 10 шт.

И productsWeight меньше или равно 3 кг

ТО hostDeliveryCost будет равен 25

Во всех остальных случаях hostDeliveryCost будет равен 45 .

- clientDeliveryCost — стоимость доставки клиенту рассчитывается в
соответствии с таблицей.

Вычисление зависит от передаваемых переменных productsCount
(количество продуктов) и productsWeight (вес продуктов).

Стоимость доставки клиенту может быть равна 0 и 99.

Стоимость доставки будет 99, если соблюдается хотя бы одно из
условий:
    - превышено максимальное количество товаров;
превышен максимальный вес.

Например, для службы доставки «Доставка Москва»:

ЕСЛИ productsWeight больше 7 кг (максимальное значение по таблице)

ИЛИ productsCount больше 15 шт. (максимальное значение по таблице)

ТО clientDeliveryCost будет равна 99

Во всех остальных случаях clientDeliveryCost будет равен 0 .

Условие «Сумма заказа меньше 150 рублей» в курьерской ручке не
накладывается. Это условие используется в ручке заказа продуктов.
