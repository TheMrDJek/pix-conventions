# Соглашение и рекомендации о разработке роботов на PIX

### В этой статье
- [Соглашение о структуре проекта](#соглашение-о-структуре-проекта)
- [Соглашение об именах](#соглашение-об-именах)
- [Соглашение о БД (в работе)](#соглашение-о-базе-данных)
- [Соглашение об очередях (в работе)](#соглашение-об-очередях)
- [Соглашение об UI/Web (в работе)](#соглашение-об-uiweb)
- [Соглашение о логирование (в работе)](#соглашение-о-логирование)
- [Соглашение об исключениях (в работе)](#соглашение-об-исключениях)
- [Рекомендации по наименованию](#Рекомендации-по-наименованию)
- [Рекомендации по написанию скриптов](#рекомендации-по-написанию-скриптов)

Соглашения о написании роботов на PIX предназначены для реализации следующих целей.

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

<details>
    <summary><b>Подключение стороних библиотек</b></summary>
Подключать стороние библиотеки или кастомные активности нужно напрямую в проект,
а не в папку с студией

![AddDll](/common/addDll.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте шаблон на основе State Machine</b></summary>
Про шаблон состояний и примеры

![AddDll](/common/addDll.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Соглашение об именах
<details>
    <summary><b>Используйте PascalCasing</b></summary>
Используйте регистр pascal (”PascalCasing”) и префиксы in, out, io при именовании параметров скрипта

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте camelCasing</b></summary>
Используйте регистр camel (”camelCasing”) при именовании переменных в скриптах<p>

![camelCasing](/common/camelCasing.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не игнорирейте конвенции нейминга языка или компании</b></summary>
Мы должны знать общие правила нейминга в компании и своего языка программирования.
Наш код должен быть достаточно близок к индустрии, из которой будут приходить люди, чтобы его поддерживать и развивать.
В то же время в компании может быть своя прослойка правил,
чтобы поддержка большой кодовой базы была проще и инженеры могли легко переключаться между проектами.<p>

- [Правила и соглашения об именовании C#](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/coding-style/identifier-names)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Наименование должно отражать значение</b></summary>
Переменная — это какие-то данные, какое-то значение, но не стоит её так и называть.
Конкретизируйте. Что за значение мы хотим в ней хранить? Так и назовём переменную.<p>

![domainName](/common/domainName.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не используйте несколько вариантов для одного термина</b></summary>
Можно встретить варианты нейминга одних и тех же терминов,
поэтому лучше обговаривать найминг или вести глоссарий<p>

Примеры:<br>
> Поставщик: provider, supplier, vendor, contractor -> suplier<br>
> Заказчик: customer, client, consumer -> customer<br>
> Цена: price, rate, cost, pricing, worth -> price<br>
> Объем: volume, amount, size, bulk, quantity -> volume<br>
> Склад: warehouse, storage, store, storehouse -> storehouse

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не используйте кириллицу</b></summary>
В языках программирования принятно писать используя ASCII Characters,
используя кириллицу появятся недопонимание со стороны других разработчиков и есть риск появления ошибок кодировки<p>

![ruCharts](/common/ruCharts.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не используйте венгерских обозначений</b></summary>
Венгерская нотация повторяет тип, который уже присутствует в объявлении.
Это бессмысленно, поскольку PIX Studio идентифицируют тип.<p>

![FixHungarianNotation](/common/FixHungarianNotation.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Cоглашение о базе данных

<details>
    <summary><b>Используйте Bulk объекты</b></summary>
Дл<p>

![FixHungarianNotation](/common/FixHungarianNotation.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Создавайте таблицы кодом (Временно)</b></summary>
Дл<p>

![FixHungarianNotation](/common/FixHungarianNotation.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Выборку данных реализуйте в SQL запросах</b></summary>
Дл<p>

![FixHungarianNotation](/common/FixHungarianNotation.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Рекомендации по наименованию

<details>
    <summary><b>Избегайте избыточных названий</b></summary>
В название добавляется тип переменной или избыточный префикс названия объекта.
В итоге мы читаем код как «добавлено дата время» или «корзина — айди корзины».<p>

![Context](/common/Context.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте названий без контекста</b></summary>
Только благодаря значениям  мы поняли, что за source имеется в виду, в каком смысле употреблены state, status.
Без значений всё усложняется — придётся отвлекаться и уточнять, о чём идёт речь.<p>

![NotContext](/common/NotContext.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте абстрактных названий</b></summary>
По названию свойств сложно догадаться, о чём идёт речь.
Почему бы не назвать свойства именем метрики, которую они несут?
Не нужно придумывать лишние обозначения, которые не применяются в реальной жизни.<p>

![Abstract](/common/Abstract.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте отрицательных переменных</b></summary>
Отрицательные переменные сложные в понимании, а если в коде есть не отрицательные переменные, то легко запутаться<p>

![NotVariables](/common/NotVariables.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Рекомендации по написанию скриптов
<details>
    <summary><b>Соблюдайте SRP (SOLID)</b></summary>

SRP – принцип единой ответственности. Этот принцип означает, что каждый скрипт в вашем коде должен выполнять одну операцию.<p>

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте минимальное кол-во аргументов</b></summary>

- Меньше - лучше
- Если скрипт принимает слишком много аргументов, возможно он нарушает SRP
- Большое кол-во аргументов - проблемы с тестированием
- Большое кол-во аргументов возможно стоит завернуть в объект(например словарь)
<p>

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте LINQ</b></summary>

Используйте LINQ вместо циклов и встроенных активностей при работе с коллекциями<p>

Плюсы
- Компактный, умещается в одну активность
- Упрощает понимание запроса/алгоритма
- Гибкий, расширяемость и деревья выражений позволяют выполнить любой запрос 
- Наличие методы для паралельной обработки

Минусы
- Сложная отладка больших запросов
- Производительность (for > foreach > LINQ)

Примеры:

![UseLINQ](/common/UseLINQ.png)
![UseLINQ2](/common/UseLINQ2.png)

Исключения:
- При работе с внешними сервисами (файлы, почта, БД,UI)
- Большая бизнес логика

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте условные операторы</b></summary>

Использование условных операторов вместо встроенных if активностей, позволяют писать компактные и легко читаемый код

Операторы с условным ?. значением NULL и ?[]

![operator](/common/operator.png)

Тернарный условный оператор (?: оператор)

![operator2](/common/operator2.png)

Операторы объединения со значением NULL (?? И?? = операторы)

![operator3](/common/operator3.png)

Исключения:
- Большая бизнес-логика
![operatornotuse](/common/NotUseOperator.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте Boxing и Unboxing</b></summary>

По сравнению с простыми операциями присваивания операции упаковки и распаковки являются весьма затратными процессами с точки зрения вычислений. При выполнении упаковки типа значения необходимо создать и разместить новый объект. Объем вычислений при выполнении операции распаковки, хотя и в меньшей степени, но тоже весьма значителен.

[Источник (Microsoft)](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/types/boxing-and-unboxing#performance)

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
Если комментарии необходимы, стоит задуматься об чистоте кода<p>

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