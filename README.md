jQuery Kladr
================================================================================

Плагин для автодополнения полей адреса при вводе.<br>
В качестве источника данных используется сервис [kladr-api.ru] [1].

Архитектура
--------------------------------------------------------------------------------

* **$.kladr** - ( *статичный объект* ) предоставляет методы и свойства для работы 
с сервисом
* **$( '' ).kladr** - ( *плагин jQuery* ) плагин для автодополнения адреса

Подключение

`````javascript
$( 'input' ).kladr({
    option1: 'value1',
    option2: 'value2'
});
`````

Чтение опции, свойства

`````javascript
$( 'input' ).kladr('option1');
`````

Изменение опции, свойства

`````javascript
$( 'input' ).kladr('option1', 123);
`````

Параметры
--------------------------------------------------------------------------------

* **token** - токен для доступа к сервису, **обязательный параметр**
* **key** – ключ для доступа к сервису, **обязательный параметр**
* **type** – тип подставляемых объектов, **обязательный параметр**
* **parentType** – тип родительского объекта, по-умолчанию *null*
* **parentId** - идентификатор родительского объекта, по-умолчанию *null*
* **limit** - количество отображаемых в выпадающем списке объектов, по-умолчанию *10*
* **withParents** - получить объект вместе с родителями, по-умолчанию *false*
* **showSpinner** - отображать ajax-крутилку, по-умолчанию *true*
* **verify** - проверять введённые данные, по-умолчанию *false*
* **current** - текущий, выбранный объект КЛАДР, только для чтения

Методы
--------------------------------------------------------------------------------

* **source** *= function( query ) { return objects; }* - функция для получения 
списка объектов отображаемых при автодополнении. По-умолчанию запрашивает данные 
у сервиса [kladr-api.ru] [1]. Может быть переопределена для получения объектов из
другого источника.
* **labelFormat** *= function( obj, query) { return label; }* - функция для 
форматирования значений в списке. В качестве параметров принимает *obj* – объект 
КЛАДР, *query* – текущее значение поля ввода.
* **valueFormat** *= function( obj, query) { return label; }* – функция для 
форматирования подставляемых в поле ввода значений. В качестве параметров 
принимает *obj* – объект КЛАДР, *query* – текущее значение поля ввода.

События
--------------------------------------------------------------------------------

* **open** *= function() {}* - открыт список объектов. Доступно как событие *kladr_open*
поля ввода.
* **close** *= function() {}* - закрыт список объектов. Доступно как событие *kladr_close*
поля ввода.
* **send** *= function() {}* - отправлен запрос к сервису. Доступно как событие *kladr_send*
поля ввода.
* **received** *= function() {}* - получен ответ от сервиса. Доступно как событие *kladr_received*
поля ввода.
* **select** *= function( obj ) {}* - выбран объект в списке. В параметре передаётся 
текущий, выбранный объект КЛАДР. Доступно как событие *kladr_select*
поля ввода.
* **check** *= function( obj ) {}* - проверен введённый пользователем объект. 
Вызывается только если параметр *verify* = *true*.
В параметре передаётся текущий объект КЛАДР. Доступно как событие *kladr_check*
поля ввода.

Свойства объекта $.kladr
--------------------------------------------------------------------------------

* **type** - перечисление используемых типов объектов. Список значений: *region*, 
*district*, *city*, *street*, *building*.
* **api** *= function( query, callback ) {}* - функция непосредственно выполняющая
запрос к сервису. В качестве параметров принимает объект запроса и функцию, которой 
будет передан ответ сервиса.
* **check** *= function( query, callback ) {}* - функция для проверки существования 
объекта. В качестве параметров принимает объект запроса и функцию, которой 
будет передан объект (если он существует) либо *false*

Структура папок, файлов
--------------------------------------------------------------------------------

* **jquery.kladr.js** - Плагин
* **jquery.kladr.min.js** - Минимифицированный код плагина
* **jquery.kladr.css** - Стили
* **jquery.kladr.min.css** - Минимифицированные стили
* **jquery.kladr.images** - Изображения
* **kladr** - Состовляющие плагина (ядро и плагин для автодополнения) по отдельности
* **examples** - Примеры использования плагина

Примеры
--------------------------------------------------------------------------------

Автодополнение городами России

`````javascript
$( 'input' ).kladr({
    token: 'token',
    key: 'key',
    type: $.kladr.type.city
});
`````

Изменении подписи при выборе района

`````javascript
$( 'input' ).kladr({
    token: 'token',
    key: 'key',
    type: $.kladr.type.district,
    select: function( obj ){
        $( 'label' ).text( obj.type );
    }
});
`````

Проверка названия города, введённого пользователем самостоятельно

`````javascript
$( 'input' ).kladr({
    token: 'token',
    key: 'key',
    type: $.kladr.type.city,
    check:  function( obj ){
        if(obj){
            $( 'input' ).css('color', 'black');
        } else {
            $( 'input' ).css('color', 'red');
        }
    }
});
`````


[1]: http://kladr-api.ru/        "КЛАДР API"