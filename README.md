# RentCar

Simple car rent management app (WIP, not ready yet)

## Models (System Entities)

### Country 

Страна, где работает сервис

* countryId: id
* name: название страны

## City

Город, где работает сервис

* cityId: id
* countryId -> Country
* name: название города

## Party

Контрагенты (юридические и физические лица)

* partyId: id
* legalType: тип контрагента ООО, ИП, Физ лицо
* fullName: полное наименование организации для использования в доукментах
* shortName: сокращенное наименование организации
* akaName: управленческое наименование, товарный знак и тп 
* legalCodes: [ LegalCode ] : ИНН, ОГРН, лицензии и другие коды
* bankAccounts: [ BankAccount ] : банковские счета 
* addresses: [ PartyAddress ] : массив адресов контрагента (юридический, главный офис, филиалы и тп)
* management
* linkedParties: [ LinkedParty ]

## Address 

Таблица хранения адресов

* addressId
* countryId: > Country
* countryRegion: регион страны (область, штат и тп)
* cityId: -> City
* district:
* street:
* building:
* office
* comments:

## PartyAddress

Таблица для хранения различных адресов, связанных с организацией

* partyAddressId: id
* partyId: -> Party
* addressId: -> Address
* addressType: AddressType* : тип адреса: юридический, главный офис, подразделение
* description: описание этого адреса (например, "Офис в Центральном районе")

## LinkedParty

Связанные организации

* linkedPartyId: id
* partyId: -> Party: контрагент
* linkedPartyId: -> Party: ссылка на связанного контрагента
* linkType: PartyLinkType*: вид связи
* description: текстовое описание связи

## BankAccount

* bankAccountId: id
* partyId: -> Party: ссылка на контрагента, к которому относится банковский счёт
* bankId: -> Party: ссылка на банк в базе контрагентов
* bankCode: БИК код, SWIFT и тп
* accountCurrency: валюта счетёта (RUR, USD, EUR, etc)
* accountCaption: описасние счёта (расчетный, для банков - корр счёт  и тп)
* accountNo: номер счёта
 
## LegalCode

* legalCodeId: id
* partyId: -> Party: ссылка на контрагента, к которому относится этот код
* legalCodeType: тип кода - ИНН, ОГРН, Лицензия определенного вида (перечисление LegalCodeTypes)
* legalCodeValue: значение кода

## Contact

Контактная ифнормация

* contactId: id
* contactType: ContactType*: тип контактной информации (email, tel, messenger, etc)
* caption: этикетка для контактной информации
* contact: контактная информация (значение поля)
* comments: примечания

## PartyContact

Таблица для связки контрагентов и контактной информации

* partyContactId: id
* partyId: -> Party
* contactId: -> Contact

## Company

Прокатные компании
 
* companyId: id
* companyType
* partyId: -> Party: ссылка на сведения о контрагенте для этой прокатной компании

## Customer

Клиенты сервиса

* customerId: id
* partyId: -> Party: ссылка на сведения о контрагенте

TODO: доработать схему активации профиля клиента

## Service

Дополнительные услуги, предоставляемые компанией

* serviceId: id
* companyId -> Company
* serviceType: ServiceType* : тип дополнительной услуги
* caption: название услуги
* description: описание услуги


## Car

Автомобили, зарегистрированные в системе

* carId: id
* carBaseCarId -> CarBase_Car: ссылка на сведения о модели автомобиля в общей базе моделей
* ownerId -> Company
* rentService: -> Service: ссылка на тип услуги, описывающей арендную стоимость автомобиля

## CarFeatures

Опции данного автомобиля (отличия от базовой комплектации, зарегистрированной для CarBaseCar)

* carFeatureId: id
* carId -> Car: машина, к кторой относится опция
* featureTypeId -> FeatureType: тип опции
* featureValue: значение опции
* featureURL: дополнительное описание опции

## Currency

Валюта

* currencyId: id
* name: название валюты
* shortName: сокращенное наименование валюты
* symbol: однобуквенный символ валюты

## CurrencyRate

Курсы валют

* currencyRateId: id
* currencyId: -> Currency: валюта, для которой определяем курс (например, EUR)
* rateCurrencyId: -> Currency: валюта, в которой выражен курс (например, RUR)
* date: дата курса
* rate: значение курса
* source: источник данных о курсе

## Price

Цена услуги

* priceId: id
* serviceId: -> Service: сервис, к которому относится эта цена
* startDateType: тип стартовой даты - единоразово или регулярно
* startDateValue: значение стартовой даты
* endDateType: тип финишной даты
* endDateValue: знаение финишной даты
* price
* currencyId
* description: описание
* activeDiscounts: какие скидки действуют от этой цены
* inactiveDiscounts: какие скидки НЕ действуют для этой цены

Примечания:

Идея в модели цены - возможность делать расписание для цен, типа, последнее воскресенье месяца - скидки! 
Удобно для сезонных цен - замой цена низкая, летом вырастает. 

## Booking

Резервирование в системе

* bookingId

## CarBooking

Сведения о бронировании автомобилей

* carBookingId: id
* carId: -> Car: выбранный для бронирования автомобиль
* priceId: -> Price: цена, по которой бронировался автомобиль
* customerId: -> Customer: клиент, забронировавший автомобиль
* date: дата, когда было сделано бронирование
* pickDate: дата, с которой нужно бронировать авто
* pickTime: время с которого забирают авто
* pickLocation: -> Location: место, где забирают авто
* pickAddress: текстовое описание места 
* dropDate:
* dropTime:
* dropLocation:
* dropAddress:
* invoiceId: заказ с перечнем услуг
* billId: счёт за услуги

Бронирование может быть связано с пакетом услуг.

 


____


# CarBase: база данных моделей автомобилей

(большой справочник актуальных моделей автомобилей, их моделей, модификаций, цен, характеристик, фотографий) 

----

## Endpoints (russian doc)

> GET /countries

Возвращается список стран, где работает эта система бронирования

> GET /countries/:countryId/cities

Возвращается список городов в указанной стране, где работает система

> GET /cities

Возвращается полный список городов, где работает система

> GET /cities/:cityId

Возвращается полная информация по городу, в котором работает система

>GET /countries/:countryId/cities/:cityId/locations
> GET /cities/:cityId/locations

Возвращается список стандартных локаций, где можно забрать/оставить автомобиль
возможность доставки в нестандартную локацию - доп услуга прокатной компании, должна быть в перечне услуг для данного города.

> GET /countries/:countryId/cities/:cityId/companies
> GET /cities/:cityId/companies

Возвращается список прокатных компаний, которые работают в данном городе

> GET /companies

Возвращается полный список прокатных компаний, зарегистрированных в системе

> GET /countries/:countryId/cities/:cityId/companies/:companyId
> GET /cities/:cityId/companies/:companyId
> GET /companies/:companyId

Возвращается информация о компании, зарегистрированной в системе

> GET /companies/:companyId/cities

Возвращается сипсок городов, где работает компания

> GET /companies/:companyId/services

Доп услуги, оказываемые компанией: доставка до адреса (список районов-исключений), кресла, доп оборудование и тп;

> GET /me

Профиль пользователя в системе

> GET /me/bookings 

Бронирования пользователя в системе

> GET /bookings

Получить доступные текущему авторизованному пользователю бронирования в системе

> GET /companies/:companyId/cars

Получить список имеющихся машин от этой компании в системе

> GET /cars

Получить список доступных машин 

> POST /bookings

Создать новое бронирование для текущего пользователя


----

# User Stories

## Регистрация пользователя

Пользователь создает аккаунт на сервисе через форму регистрации, вносит необходимые данные в поля форм, прикрепляет фото документов и отправляет анкету на проверку.

Система проверяет анкету, и отправляет пользователю уведомление о готовности его аккаунта к работе.

## Поиск бронирования

Пользователь заходит на сайт системы и вводит данные в форму поиска: где нужен автомобиль, с какой даты и по какую.

Система выдает список автомобилей, доступных на выбранную дату.

Пользователь использует фильтры для настройки списка: выбирает класс автомобиля, тип трансмиссии и другие опции.

После выбора автомобиля система предлагает пользователю забронировать его, укомплектовав дополнительными опциями: детское кресло, доп водитель, и тп.

Пользовтаель выбирает место передачи, 
