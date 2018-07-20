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

## Company

Прокатные компании
 
* companyId: id
* companyType
* partyId: -> Party: ссылка на сведения о контрагенте для этой прокатной компании

## Customer

Клиенты сервиса

* customerId: id
* partyId: -> Party: ссылка на сведения о контрагенте

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

## Price

Цена услуги

* priceId: id
* serviceId: -> Service: сервис, к которому относится эта цена
* startDateType: тип стартовой даты - единоразово или резулярно
* startDateValue: значение даты
* endDateType
* endDateValue
* price
* description: описание
* activeDiscounts: какие скидки действуют от этой цены
* inactiveDiscounts: какие скидки НЕ действуют для этой цены

Примечания:

Идея в модели цены - возможность делать расписание для цен, типа, последнее воскресенье месяца - скидки! 
Удобно для сезонных цен - замой цена низкая, летом вырастает. 
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

