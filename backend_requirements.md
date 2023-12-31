# Требования к бэкенду приложения
## Описание общей логики
### Авторизация и данные для заказа
Пользователь может зарегистрироваться. Если пользователь не зарегистрировался, то форма заполнения: имя, e-mail, телефон, адрес, комментарий — появляется, когда пользователь уже сформировал корзину и хочет оформить заказ. Пользователь не может сделать заказ, если не ввёл обязательные поля. Если пользователь зарегистрировался, то ему не нужно вновь вводить данные, однако он может их изменить. Пользователь может оформить несколько заказов.

Для заказа нужно ввести:
- имя;
- телефон;
- адрес;
- e-mail (необязательно);
- комментарий к заказу (необязательно).

### Ограничение полей
| Элемент               | Требования | Обязательность |
| ------------------|:--------:|:------------------:|
| Имя               | Только русские и английские буквы, пробел, тире. Длина не менее 2 и не более 15 символов. | Yes |
| E-mail            | Только английские буквы, цифры, тире, точка, @. Длина не менее 5 и не более 50 символов. | No |
| Телефон           | Только цифры и знак +. Длина не менее 10 и не более 12   символов. | Yes |
| Адрес             | Только русские буквы, цифры, пробел, тире, точка, запятая. Длина не менее 5 и не более 50 символов. Пробелы до и после адреса удаляются при снятии фокуса. | Yes |
| Комментарий к заказу | Только русские буквы, цифры, пробел, тире, точка, запятая. Длина не более 24 символов. | No |

### Главное меню заказа
На выбор даётся 3 карточки:
- «Под ситуацию» (вечер кино, на дачу и пикник, вкусы Парижа);
- «Приготовь блюдо» (сырники, борщ, карбонара, штрудель);
- «Создать свой набор» (пользователь сам называет и добавляет туда
продукты).

Переходишь в карточку — видишь варианты наборов.

Переходишь в набор — видишь перечень возможных продуктов. Каждый продукт
относится к определённой категории (например, «Напитки»). В перечне
пользователь видит название продукта, его массу, цену. Когда клиент нажимает на
продукт, ему даётся возможность выбрать количество продуктов. При этом
появляется кнопка «Корзина», которая отображает сумму выбранных товаров и
время доставки.

При нажатии на кнопку пользователь может посмотреть свою корзину.

### Корзина
Отображает наименование продукта, его количество, цену для этого продукта с
учётом количества, итог. Если доставка платная, то отображает сумму доставки и
итоговую сумму заказа. Пользователь может удалить корзину, добавить новые
продукты, убрать выбранные продукты.

Создание корзины:
- При создании корзины должна быть возможность указать время доставки
продуктов
- При создании корзины проверять, что все службы доставки могут обработать
заказ в указанное время
- Если время доставки не указано - брать, как текущее время с сервера.

При удалении и просмотре корзины время доставки не учитывается.

### Создание своего набора
Пользователь может создать свой набор и выбрать продукты. Он обязательно
даёт имя набору и выбирает продукты. Пользователь может изменить название
набора, удалить набор, удалить выбранные продукты, добавить новые. Если
данные при создании или изменении набора введены неверно — выводится
сообщение об ошибке.


### Ограничения создания набора
| Наименование      | Требования | Обязательность |
| ------------------|:--------:|:------------------:|
| Имя набора        | Только русские и английские буквы, пробел, тире. Длина не менее 2 и не более 15 символов. | Yes |
| Количество товаров в наборе | Не более 30 | Yes |

### Работа с курьерами
Работа с курьерами предполагает два режима работы:

1. При оформлении заказа: URL POST /api/v1/orders

Логика выбора курьерской службы при оформлении заказа пользователем: служба
должна работать в указанное в заказе время и должна быть самой дешёвой.

Пользователю отображается время доставки в зависимости от выбранной службы.

В заказе доставка становится платной, если соблюдается хотя бы одно условие:
- превышено максимальное количество товаров;
- превышен максимальный вес;
-   сумма заказа меньше 150 рублей.

Логика расчёта стоимости доставки заказа для пользователя:
- Если вес или количество превысили максимальное значение, стоимость
доставки для пользователя становится 99 рублей.
- Если вес или количество в заказе не превышают максимального, берётся
price этих продуктов, и если их сумма меньше 150 рублей, то стоимость
доставки для пользователя также становится 99 рублей.

Стоимость доставки прибавляется в итоговую сумму заказа.

Если ни одно из обозначенных условий не соблюдается, то цена доставки
курьерской службы не включается в итоговую сумму заказа.

2. Узнать возможность доставки продуктов и цену для отдельной курьерской
службы: у каждой курьерской службы свой URL.

Цена доставки курьерской службой рассчитывается по количеству и весу
указанных продуктов.

Детальные требования к расчёту стоимости доставки курьерскими службами
можно посмотреть в delivery_requirements.md

### Ограничения Couriers
| Наименование      | Время работы | Количество товаров в заказе, шт | Вес заказа      | Время доставки | Цена доставки курьерской службы, руб. | Формат данных |
| ------------------|:----------:|:--------------:|:-----------------:|:----------:|:--------------:|:--------------:|
| Доставка Москва | 08-22 | 0-10 | 0-3 кг | 30-35 мин | 25 | JSON |
| Доставка Москва | 08-22 | 11-15 | 3,1-7 кг | 30-35 мин | 45 | JSON |
| Привезём быстро | 07-21 | 0-7 | 0-2,5 кг | 25-30 мин | 23 | XML |
| Привезём быстро | 07-21 | 8-14 | 2,6-6 кг | 25-30 мин | 43 | XML |
| Чух-чух и уже у вас | 06-20 | 0-12 | 0-3,5 кг | 25-30 мин | 29 | SOAP |
| Чух-чух и уже у вас | 06-20 | 13-20 | 3,6-9 кг | 25-30 мин | 53 | SOAP |
| На метле уюта | 08-22 | 0-9 | 0-3 кг | 20-25 мин | 25 | JSON |
| На метле уюта | 08-22 | 10-15 | 3,1-6 кг | 20-25 мин | 40 |JSON |

### Работа со складом
Имеется 4 складских отделения. У каждого склада своя ручка. У каждого свой
ограниченный набор продуктов. Когда пользователь сделал заказ, ручка уточняет,
какой склад сформирует заказ. Логика выбора склада: есть продукты на складе,
должен работать во время заказа и самый дешёвый. Пользователь может заказать
только те продукты и их количество, которые есть в полной мере хотя бы на одном
складе (то есть ситуации, где он набрал корзину, а ему пишут «Не привезём» —
нет).

### Ограничения Warehouse
| Наименование      | Время работы | Формат данных | Цена для одного заказа, руб. |
| ------------------|:----------:|:--------------:|:-----------------:|
| Имеется все | 07-23 | JSON | 9 |
| Чердак | 08-21 | XML | 8 |
| Большой мир | 05-20 | SOAP | 9,5 |
| Шведский дом | 08-23 | JSON | 7 |

## Список URL реализованных в API
### URL для авторизации
- POST /api/v1/users - создать пользователя
### URL для наборов
- POST /api/v1/kits - создать набор
- GET /api/v1/kits - получить список наборов
- DELETE /api/v1/kits - удалить набор
- PUT /api/v1/kits - переименовать набор, изменить список продуктов в наборе
- GET /api/v1/kits/search - получить список продуктов в наборе
### URL для продуктов
- POST /api/v1/products/kits - получить список наборов по продуктам
- POST /api/v1/kits/{id}/products - добавить продукты в набор
- PUT /api/v1/products/:id - изменить цену продукта
- POST /api/v1/orders - посчитать сумму продуктов
- POST /api/v1/warehouse/check - проверить наличие продуктов на складах
### URL для складов
- GET /api/v1/warehouses - получить список складов
- POST /api/v1/warehouses/amount - получить информацию о количестве продуктов на складах
- /api/wsdl - получить информацию о количестве продуктов на складах
- POST /api/v1/orders - получить информацию, какой склад возьмет заказ
### URL для курьерских служб
- GET /api/v1/couriers - получить список курьерских служб
- POST /api/v1/couriers/check - узнать информацию доступна ли курьерская служба для
доставки заказа
- POST /api/v1/orders - узнать информацию, какой курьер возьмет заказ
- POST /moscow-delivery/v1/calculate - возможность доставки и её стоимость курьерской
«Доставка Москва»
- POST /on-a-broomstick/v1/delivery - возможность доставки и её стоимость курьерской
службой «На метле уюта»
- POST /fast-delivery/v3.1.1/calculate-delivery.xml - возможность доставки и её стоимость
курьерской службой «Привезём быстро»
- POST /train-noises/wsdl - возможность доставки и её стоимость курьерской службой «Чух-чух
и уже у вас»
### URL для заказов
- POST /api/v1/orders - посчитать итоговую сумму заказа (вместе с доставкой)
- POST /api/v1/orders - посчитать стоимость доставки с учётом различных курьерских служб
- GET /api/v1/orders - получить список заказов
### URL для корзины
- GET /api/v1/orders/id - получить список продуктов в корзине
- POST /api/v1/orders - создать корзину
- PUT /api/v1/orders/:id - добавить продукты в корзину
- DELETE /api/v1/orders/:id - удалить корзину

## Описание содержимого базы данных
### Таблица "user_model"
| Название поля  | Тип поля | Описание | Пример |
| ------------------|:----------:|:--------------:|:-----------------:|
| firstName | Строка | Имя пользователя | Иван |
| phone | Строка | Телефон пользователя | 88005553535 |
| email | Строка | Электронная почта пользователя | tema@tema.ru |
| address | Строка | Адрес пользователя | Москва, улица Пушкина дом 17 |
| comment | Строка | Комментарий | Не бойтесь собаку |
| authToken | Строка | Токен авторизации | ah22fOJDAdamMO86D |
| id | Число | Уникальный идентификатор | 1 |

### Таблица "order_model"
| Название поля  | Тип поля | Описание | Пример |
| ------------------|:----------:|:--------------:|:-----------------:|
| id | Число | Уникальный идентификатор | 1 |
|userid | Число | Идентификатор пользователя, которому принадлежит заказ |5 |
| productsList |Массив чисел |Список продуктов в заказе | [1,2,3] |
| deliveryPrice |Число | Cтоимость доставки |  |
|deliveryPriceOur | Число | Стоимость доставки для нас |   |
|wareHouse | Строка | Название склада |   |
|courierService | Строка | Название службы доставки |   |
|status | Число | Статус заказа. Может принимать три значения: 0 — заказ формируется, 1 — заказ в доставке, 2 — заказ завершён. |   |
| deliveryTime | Строка | Время, за которое доставят заказ | 20~30 |

### Таблица "kit_model"
| Название поля  | Тип поля | Описание | Пример |
| ------------------|:----------:|:--------------:|:-----------------:|
| name | Строка | Название набора | На пикник |
| productsList | Массив чисел | Список продуктов в наборе | [1,2,3] |
| productsCount | Число | Количество продуктов в наборе |   |
| cardId | Число | Идентификатор карточки, которой принадлежит набор. Внешний ключ к таблице card_model |   |
| userId | Число | Идентификатор пользователя, которому принадлежит набор. Внешний ключ к таблице user_model |   |
| id | Число | Уникальный идентификатор |   |

### Таблица "product_model"
| Название поля  | Тип поля | Описание | Пример |
| ------------------|:----------:|:--------------:|:-----------------:|
| id | Число | Уникальный идентификатор |   |
| name | Строка | Наименование продукта | Сок «Добрый» |
| price | Число | Стоимость |   |
| weight | Число | Вес |   |
| units | Строка | Единица измерения. Допустимые значения: г, кг, мл, л, шт | шт |
| categoryId |Число | Идентификатор категории, в которой находится товар. Внешний ключ к таблице category_model |   |

### Таблица "card_model"
| Название поля  | Тип поля | Описание | Пример |
| ------------------|:----------:|:--------------:|:-----------------:|
| id | Число | Уникальный идентификатор |   |
| name | Строка | Название карточки | Разное |

### Таблица "category_model"
| Название поля  | Тип поля | Описание | Пример |
| ------------------|:----------:|:--------------:|:-----------------:|
| id | Число | Уникальный идентификатор |   |
| name | Строка | Название категории | Сыры |
