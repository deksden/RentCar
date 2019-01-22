# Domain-specific models

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
* comments: примечания

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
* linkedId: -> Party: ссылка на связанного контрагента
* linkType: PartyLinkType*: вид связи
* description: текстовое описание связи

## Bank

Банк

* bankId: id
* partyId: -> Party: контрагент со сведениями о банке
* codes: BankCodes: коды банка (БИК, к/счёт и тп)

## BankAccount

* bankAccountId: id
* bankId: -> Bank: ссылка на банк в базе контрагентов
* partyId: -> Party: ссылка на контрагента, к которому относится банковский счёт
* accountCurrency: валюта счетёта (RUR, USD, EUR, etc)
* accountType: AccountType*: описасние счёта (расчетный, депозитный, транзитный, корр счёт и тп)
* accountNo: номер счёта
 
## LegalCode

* legalCodeId: id
* partyId: -> Party: ссылка на контрагента, к которому относится этот код
* legalCodeType: тип кода - ИНН, ОГРН, Лицензия определенного вида (перечисление LegalCodeTypes)
* legalCodeValue: значение кода

## Contact (* not yet)

Контактная ифнормация

* contactId: id
* contactType: ContactType*: тип контактной информации (email, tel, messenger, etc)
* caption: этикетка для контактной информации
* contact: контактная информация (значение поля)
* comments: примечания

## PartyContact (*)

Таблица для связки контрагентов и контактной информации

* partyContactId: id
* partyId: -> Party
* contactId: -> Contact

## Company

Компании
 
* companyId: id
* partyId: -> Party: ссылка на сведения о контрагенте для этой прокатной компании
* companyType: тип компании
* bankAccountId: -> BankAccount: основной банковский счёт, который использует компания по-умолчанию в документах

## Group

Группы (разного назначения)

* groupId: id
* groupType: тип группы
* caption: название группы

## PartyGroup

Связка Групп и Контрагентов

* partyGroupId: id
* groupId: -> Group: 
* partyId: -> Party

## Customer

Клиенты сервиса

* customerId: id
* partyId: -> Party: ссылка на сведения о контрагенте

TODO: доработать схему активации профиля клиента

## Product

Продукты - товары или услуги, предоставляемые компанией

* productId: id
* companyId -> Company
* productType: ProductType* : тип продукта - товар или услуга
* caption: название продукта
* description: описание

## Car

Автомобили, зарегистрированные в системе

* carId: id
* carBaseCarId -> CarBase_Car: ссылка на сведения о модели автомобиля в общей базе моделей
* ownerId -> Company
* rentService: -> Service: ссылка на тип услуги, описывающей арендную стоимость автомобиля
* carYear: год выпуска данного автомобиля 
* color: цвет автомобиля

## CarFeature

Опции данного автомобиля (отличия от базовой комплектации, зарегистрированной для CarBaseCar)

* carFeatureId: id
* carId -> Car: машина, к кторой относится опция
* featureType: FeatureType*: тип опции
* featureValue: значение опции
* featureURL: дополнительное описание опции

## Currency (*)

Валюта

* currencyId: id
* name: название валюты
* shortName: сокращенное наименование валюты
* symbol: однобуквенный символ валюты

## CurrencyRate (*)

Курсы валют

* currencyRateId: id
* currencyId: -> Currency: валюта, для которой определяем курс (например, EUR)
* rateCurrencyId: -> Currency: валюта, в которой выражен курс (например, RUR)
* date: дата курса
* rate: значение курса
* source: источник данных о курсе

## Price <--

Цена услуги

* priceId: id
* productId: -> Product: продукт, к которому относится эта цена
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
* invoiceId: заказ с перечнем товаров/услуг
* billId: счёт за услуги

Бронирование может быть связано с пакетом услуг.

## Invoice

Счет (инвойс) на товары / услуги

* invoiceId: id
* partyId: -> Party: кто выставил счёт
* customerId: -> Party: кому выставили счёт
* totalAmount: сумма счёта
* paymentTerms: график оплаты
* description: примечания

Примечания: при развитии системы можно добавлять "purchase order no" (номер заказа клиента), "credit terms" (условия по кредиту) и тп. 

## InvoiceDetails

Детали инвойса, составляющие перечень товаров и услуг в счёте

* billDetailId: id
* billId: -> Bill: счёт, ко ромку относитсяп запись
* priceId: -> Price: цена, по которой выставляется счёт

____


# CarBase: база данных моделей автомобилей

(большой справочник актуальных моделей автомобилей, их моделей, модификаций, цен, характеристик, фотографий) 

## BaseCar

Модель машины

* baseCarId: id
* makerName: 
* modelName:
* modification: 
* complectation:
* modelYear:

----

## Domain Models/Services

### Car

* id: identifier, UUID
* caption: model name
* carType: -> CarType.id
* manufacturingYear: 
* colorName:
* color: (color code) 
* carRegPlate:
* carRegDoc: 
* carVIN:
* engineNo:
* carOwner: 
* carOwnerDoc:

### CarOwner

* id
* carId: -> Car.id
* ownerId: -> BizParty.id

### CarType

* id
* brand: (enum): Toyota, Lexus, etc
* model: 
* generation: CarGeneration.id:
* bodyType: -> CarBodyType.id: unknown, sedan, hatch, coupe, ...
* engineType: gasoline, diesel, hybrid, electro, lpg
* engineOptions: none, (turbine type) 
* engineVolume: in liters, for ex "1.6"
* enginePower: in hp, for ex "249 hp"
* transmission: AT, MT
* transmissionOptions: none, AT-hydro, AT-robo, AT-variator
* gear: rear, front, 4wd
* wheelPosition: left, right

### CarGeneration

* id
* model
* generationCaption:
* generationNo:
* startYear:
* endYear:

### BizParty

* id
* partyType: organization, person
* orgId: -> Org.id
* personId: -> Person.id

