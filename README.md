# RentCar

Simple car rent management app (WIP, not ready yet)

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

Возвращается полный список компаний, зарегистрированных в системе

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


## Entities

### Country 

* countryId
* name

## City

* cityId
* countryId -> Country
* name

## Company

* companyId
* name

## Service

* serviceId
* companyId -> Company
* caption
* description
