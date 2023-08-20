# Соглашение о написании роботов на PIX

### В этой статье
- [Соглашение о структуре репозитория (в работе)](#соглашение-о-структуре-репозитория)
- [Соглашение о структуре проекта](#соглашение-о-структуре-проекта)
- [Соглашение о шаблонах (в работе)](#соглашение-о-шаблонах)
- [Соглашение об именах](#соглашение-об-именах)
- [Соглашение о скриптах](#соглашение-о-скриптах)
- [Соглашение о переменных (в работе)](#соглашение-о-переменных)
- [Соглашение о базе данных (в работе)](#соглашение-о-базе-данных)
- [Соглашение о очередях (в работе)](#соглашение-о-очередях)
- [Соглашение о папках и файлах (в работе)](#соглашение-о-папках-и-файлах)
- [Соглашение о UI/Web (в работе)](#соглашение-о-uiweb)
- [Соглашение о логирование (в работе)](#соглашение-о-логирование)
- [Соглашение о исключениях (в работе)](#соглашение-о-исключениях)
- [Соглашение о Мастере (в работе)](#соглашение-о-мастере)
- [Соглашение о Ассетах (в работе)](#соглашение-о-ассетах)

Соглашения о написании роботов на PIX предназначены для реализации следующих целей.

## Соглашение о структуре репозитория
(В работе)

**[⬆ К началу статьи](#в-этой-статье)**

## Соглашение о структуре проекта
<details>
    <summary><b>Разделение проекта на слои</b></summary>
Разбиение этого проекта на несколько слоев на основе обязанностей позволяет повысить удобство поддержки проекта

```console
├───Common (слой шаблонов и прочих файлов)
├───Helpers (слой независимых функций)
├───Infrastructure (слой сервисов)
│   ├───DataBase (сервис бд)
│   ├───Mail (сервис почты)
│   └───PIX Master (сервис мастера)
├───States (слой состояний)
│   ├───Init
│   │   └───Transactions
│   ├───GetSetTransaction
│   │   └───Transactions
│   ├───Process
│   │   └───Transactions
│   └───EndProcess
└───Tests (слой тестов)
    ├───IntegrationTests
    └───UnitTests
```

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Соглашение о шаблонах
(В работе)

**[⬆ К началу статьи](#в-этой-статье)**


## Соглашение об именах
<details>
    <summary><b>Используйте PascalCasing</b></summary>
Используйте регистр pascal (”PascalCasing”) и префиксы in, out, io при именовании параметров скрипта

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте camelCasing</b></summary>
Используйте регистр camel (”camelCasing”) при именовании переменных в скриптах

![camelCasing](common\camelCasing.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не игнорирейте конвенции нейминга языка или компании</b></summary>
Мы должны знать общие правила нейминга в компании и своего языка программирования.
Наш код должен быть достаточно близок к индустрии, из которой будут приходить люди, чтобы его поддерживать и развивать.
В то же время в компании может быть своя прослойка правил,
чтобы поддержка большой кодовой базы была проще и инженеры могли легко переключаться между проектами.

- [Правила и соглашения об именовании C#](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/coding-style/identifier-names)


**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Наименование должно отражать значение</b></summary>
Переменная — это какие-то данные, какое-то значение, но не стоит её так и называть.
Конкретизируйте. Что за значение мы хотим в ней хранить? Так и назовём переменную.

![domainName](common\domainName.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте использование несколько вариантов для одного термина</b></summary>
Можно встретить варианты нейминга одних и тех же терминов,
поэтому лучше обговаривать найминг или вести глоссарий

Поставщик: provider, supplier, vendor, contractor
Заказчик: customer, client, consumer
Цена: price, rate, cost, pricing, worth
Объем: volume, amount, size, bulk, quantity
Склад: warehouse, storage, store, storehouse

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте кириллицу</b></summary>
В языках программирования принятно писать используя ASCII Characters,
используя кириллицу появятся недопонимание со стороны других разработчиков и есть риск появления ошибок кодировки

![ruCharts](common\ruCharts.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте венгерских обозначений</b></summary>
Венгерская нотация повторяет тип, который уже присутствует в объявлении.
Это бессмысленно, поскольку PIX Studio идентифицируют тип.

![FixHungarianNotation](common\FixHungarianNotation.png)



**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте избыточных названий</b></summary>
В название добавляется тип переменной или избыточный префикс названия объекта.
В итоге мы читаем код как «добавлено дата время» или «корзина — айди корзины».

![Context](common\Context.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте названий без контекста</b></summary>
Только благодаря значениям  мы поняли, что за source имеется в виду, в каком смысле употреблены state, status.
Без значений всё усложняется — придётся отвлекаться и уточнять, о чём идёт речь.

![NotContext](common\NotContext.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте абстрактных названий</b></summary>
По названию свойств сложно догадаться, о чём идёт речь. 
Почему бы не назвать свойства именем метрики, которую они несут? 
Не нужно придумывать лишние обозначения, которые не применяются в реальной жизни.

![Abstract](common\Abstract.png)


**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте отрицательных переменных</b></summary>
Отрицательные переменные сложные в понимании, а если в коде есть не отрицательные переменные, то легко запутаться

![NotVariables](common\NotVariables.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Соглашение о скриптах
<details>
    <summary><b>Соблюдайте SRP (SOLID)</b></summary>

SRP – принцип единой ответственности. Этот принцип означает, что каждый скрипт в вашем коде должен выполнять одну операцию.

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте минимальное кол-во аргументов</b></summary>

- Меньше - лучше
- Если скрипт принимает слишком много аргументов, возможно он нарушает SRP
- Большое кол-во аргументов - проблемы с тестированием
- Большое кол-во аргументов возможно стоит завернуть в объект(например словарь)


**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте LINQ</b></summary>

Используйте LINQ вместо циклов и встроенных активностей при работе с коллекциями

Исключения:
- При работе с внешними сервисами (файлы, почта, БД,UI)
- Большая бизнес логика


**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте новые фичи языка и PIX</b></summary>
Добавить

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте DRY</b></summary>
Добавить

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте комментариев</b></summary>
Комментарии не нужны, пишите самодокументируемый код.

Если комментарии необходимы, стоит задуматься об чистоте кода

Исключения:
- TODO
- Поведения, противоречащее логике

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте флагов в аргументах</b></summary>

Лучше разделить на два например вместо CreateFile → CreateFile, CreateTempFile

**[⬆ К началу статьи](#в-этой-статье)**

</details>

</details>

<details>
    <summary><b>Избегайте глубоких вложенний</b></summary>
Добавить

**[⬆ К началу статьи](#в-этой-статье)**

</details>

</details>

<details>
    <summary><b>Избегайте отрицательных условий</b></summary>
Добавить

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте громоздких if-ов</b></summary>
Добавить

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Временные решения</b></summary>

- Если скрипт часто вызывается, замените его на контейнер (пока не решится вопрос логами)
- Создавайте таблицу кодом, а не активностью. (пока не появится возможность указывать null)

**[⬆ К началу статьи](#в-этой-статье)**

</details>
