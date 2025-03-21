# Соглашение и рекомендации при разработке роботов на PIX

### В этой статье

- [Соглашение о структуре проекта](#соглашение-о-структуре-проекта)
- (NEW) Соглашения о Git //В работе
- [Соглашение об именовании](#соглашение-об-именовании)
- [Соглашение о БД](#Cоглашение-о-БД)
- [Соглашение об очередях](#соглашение-об-очередях)
- [Соглашение о логировании](#Соглашение-о-логировании)
- (NEW) Соглашения об исключения //В работе
- [Рекомендации по написанию скриптов](#рекомендации-по-написанию-скриптов)
- (NEW) Рекомендации о тестировании //В работе

Стандарт скрипта необходим для поддержания удобочитаемости скрипта, согласованности и совместной работы в команде разработчиков. Скрипт, который соответствует отраслевым методикам и установленным рекомендациям, проще понимать, поддерживать и расширять. Большинство проектов применяют согласованный стиль с помощью соглашений о скрипте

## Соглашение о структуре проекта

<details>
    <summary><b>Разделение проекта на слои</b></summary>
Разбиение этого проекта на несколько слоев на основе обязанностей позволяет повысить удобство поддержки проекта

```console
├───Common (слой шаблонов и прочих файлов)
├───Helpers (слой независимых функций)
├───Infrastructure (слой работы с сервисами/интеграциями)
│   ├───DataBase (работа с бд)
│   ├───Mail (работа с почтой)
│   └───PIX Master API (работа с API мастера)
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
    <summary><b>Используйте шаблон на основе State Machine</b></summary>

[Список шаблонов](https://github.com/TheMrDJek/pix-templates)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте слои по назначению</b></summary>

Робот должен состоять минимум из 4 основных состояний:

<b>INIT</b>
-
Состояние INIT используется для инициализации робота, в этом блоке мы реализуем:
- Сброс ошибок
- Инициализация конфига
- Проверка доступов (доступ к бд, к папке, к api и так далее)
- Запуск и авторизация в приложение если мы работаем с UI
- Парсинг данных, если мы работаем не с очередями

<b>GET/SET TRANSACTION</b>
-
Состояние GET/SET TRANSACTION используется для получения транзакции* роботом, в этом
<br> блоке мы реализуем:
- Получение транзакции
- Обработка сигнала стоп из мастера

транзакция в роботе* - минимальный набор данных для обработки сущностей/домена.

Например:
- Если робот обрабатывает очередь, то транзакцией будет - элемент очереди
- Если робот обрабатывает письма, то транзакцией будет - письмо
- Если робот обрабатывает папку с файлами, то транзакцией будет - файл

<b>PROCESSING TRANSACTION</b>
-
Состояние PROCESSING TRANSACTION используется для обработки транзакции роботом, в этом блоке мы реализуем:
- Обработка транзакции

<b>ВАЖНО</b>

Не размазывайте бизнес логику связанную с транзакцией на другие состояние, 
<br>если нужно отправить письмо, сохранить значения, отправить отчет по транзакции,
<br>то все реализуем в этом состоянии и его ветках.

<b>END PROCESS</b>
-
Состояние END PROCESS используется для завершения робота, в этом блоке мы реализуем:
- Отключение соединений
- Закрытие приложений (если нужно, лучше оставлять по не закрытым что бы при <br>след запусках робота не тратить время на авторизацию и запуск самого приложения)
- Проверка ошибок
- Удаление временных папок

<b>ВАЖНО</b>

Не игнорирейте ошибки, если у вас в каком то состоянии произошла системная ошибка робот должен завершиться с ошибкой

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте ассеты мастера для хранения конфигурации</b></summary>

Конфиг должен быть реализован с учетом использование ассетов только из мастера.<br>
Заполнение конфига из других место увеличивает кол-во зависимостей, плюс может приводить к ошибкам.

Например:
- excel файл не доступен
- потеря соединения с бд

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте зашифрованные ассеты, для хранения конфиденциальных данных</b></summary>

УЗ, подключение и прочие чуствительные данные используемые 
в конфиге робота должны храниться в ассетах с типами:
- Защищенные данные
- Учетные данные для автороризации

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Храните шаблоны и прочие файлы в самом проекте робота</b></summary>

Не используйте сетевые, временные папки для хранения шаблонов. в PIX можно хранить подобные файлы в самом проекте робота

![alt text](/common/AddPatternFile.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте использование общих библиотек с другими роботами</b></summary>

Использование общих Utils(кастомных) библиотек или скриптов с другими роботами приводит к зависимости робот друг от друга или других конфликтах

За частую если требуется внести изменения в библиотеку, то приходится вностить изменения в несколько роботов сразу, что приводит к увеличению трудозатрат и нарушению DRY.

В качестве решения, можно держать набор общих библитек, но после использования в проекте развивать отдельно в рамках одного проекта.

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Соглашение об именовании

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

<details>
    <summary><b>Не используйте избыточных названий</b></summary>
В название добавляется тип переменной или избыточный префикс названия объекта.
В итоге мы читаем код как «добавлено дата время» или «корзина — айди корзины».<p>

![Context](/common/Context.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не используйте названия без контекста</b></summary>
Только благодаря значениям  мы поняли, что за source имеется в виду, в каком смысле употреблены state, status.
Без значений всё усложняется — придётся отвлекаться и уточнять, о чём идёт речь.<p>

![NotContext](/common/NotContext.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не используйте абстрактные названия</b></summary>
По названию свойств сложно догадаться, о чём идёт речь.
Почему бы не назвать свойства именем метрики, которую они несут?
Не нужно придумывать лишние обозначения, которые не применяются в реальной жизни.
<p>

*Например: переменная **checkAttachments** - абстрактное название, если дословно переводить получится "проверка вложений", из названия не понятно что переменная возвращает при наличие или отсутствие вложений. Лучше назвать **isWithAttachments** или **isAttchaments**, и нам сразу будет понятно что при наличии вложений будет возвращать **true***

![Abstract](/common/Abstract.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не используйте отрицательные переменные</b></summary>
Отрицательные переменные сложные в понимании, а если в коде есть не отрицательные переменные, то легко запутаться<p>

![NotVariables](/common/NotVariables.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Cоглашение о БД

<details>
    <summary><b>Используйте Bulk запросы</b></summary>
При записи больших таблиц используйте bulk запросы, вместо циклов<p>

- достаточно одного запроса в БД
- быстреее чем простые запросы

|Кол-во записей|Обычный запрос (Insert)|Bulk-запрос (bulk insert)|
|-:|:-:|:-:|
|100|2 мс|1,9 мс|
|1 000|18 мс|8 мс|
|10 000|203 мс|76 мс|
|100 000|2,13 с|742 мс|
|1 000 000|21,56 с|8,3 с|

Тестовая таблица содержит 6 столбцов (Guid, string x2, int, decimal?, DateTime).<p>
Тест проводился локально по конфигурации:
- CPU INTEL i7-10510U с частотой 2,30 ГГц
- RAM DDR3 16 ГБ
- SSD SAMSUNG 512 ГБ

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Выборку данных реализуйте в SQL запросах, а не в активностях</b></summary>

- уменьшает потребление памяти (не надо тянуть всю таблицу в оперативку)
- увеличивает производительность

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте создание хранимок в БД</b></summary>

- уменьшает кол-во зависимостей (не надо поддерживать хранимки)
- увеличивает гибкость настройки робота
- упрощает поддержку скриптов

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Учитывайте различия между NULL и пустой строкой</b></summary>
При работе с БД, учитывайте различия между null и '', если значения нет то указывайте null.
Но если значение должно быть пустое (например результат распознавания вернул пустую строку) то в БД пишем ''

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Соглашение об очередях

- Элементы очереди обновляйте в последнею очередь

**[⬆ К началу статьи](#в-этой-статье)**

## Соглашение о логировании

- Логируйте прогресс обработки больших данных в % или 0/100
- Логируйте все вложенные ошибки, но избегайте логирование всего трейса исключений в транзакции
- Логируйте исключения добавляя данные об транзакции (не конфиденциальные)
- Избегайте логирование конфиденциальной информации
- Избегайте логирование начало/конца скрипта

**[⬆ К началу статьи](#в-этой-статье)**

## Соглашение об исключениях

<details>
    <summary><b>Избегайте появления 'Exception has been thrown by the target of an invocation.'</b></summary>

Некоторые ошибки в PIX выбрасывают исключение с текстом 'Exception has been thrown by the target of an invocation'. На самом деле, настоящая ошибка указана в InnerException данного исключения.

Пример вызова исключения NullReferenceException

![Exception1](/common/Exception1.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не генерируйте исключения повторно</b></summary>

// В работе

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Обработка общих условий без выдачи исключений</b></summary>

// В работе

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не оставляйте Cath пустым и не используйте Try/Cath/Finaly в бизнес логике</b></summary>

// В работе

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Не создавайте в Cath </b></summary>

// В работе

**[⬆ К началу статьи](#в-этой-статье)**

</details>

## Рекомендации по написанию скриптов

<details>
    <summary><b>Соблюдайте соглашение о коде C#</b></summary>

Стандарт кода необходим для поддержания удобочитаемости кода, согласованности и совместной работы в команде разработчиков. Код, который соответствует отраслевым методикам и установленным рекомендациям, проще понимать, поддерживать и расширять. Большинство проектов применяют согласованный стиль с помощью соглашений о коде. И dotnet/docsdotnet/samples проекты не являются исключением. В этой серии статей вы узнаете о наших соглашениях по программированию и средствах, которые мы используем для их применения. Вы можете принять наши соглашения как есть или изменить их в соответствии с потребностями вашей команды.

[Общие соглашение о коде C#](https://learn.microsoft.com/ru-ru/dotnet/csharp/fundamentals/coding-style/coding-conventions)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Соблюдайте SRP (SOLID)</b></summary>

SRP – принцип единой ответственности. Этот принцип означает, что каждый скрипт в вашем коде должен выполнять одну операцию.<p>

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Соблюдайте принцип DRY</b></summary>

DRY – Don’t repeat yourself (не повторяй себя). Всегда думайте о том, как можно переиспользовать тот или иной фрагмент скрипта. Что можно выделить в универсальный скрипт/функцию. Речь не идет о написании активностей - я имею в виду очень похожею логику, которая встречается в нескольких местах.

*Есть еще принипы KISS, YAGNI:<p>
KISS - Keep it simple, stupid («Сделай это проще, тупица») - "чем проще - тем лучше", но это не означает сделать быстро и кое как<p>
YAGNI - "тебе это не нужно", не прописывай функции, которые могут и не понадобиться в будущем*


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
    <summary><b>Используйте $,@,""" при работе со строками</b></summary>

- [Строковые литералы verbatim](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/strings/#verbatim-string-literals)
- [Необработанные строковые литералы (Скоро появится в PIX)](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/strings/#raw-string-literals)
- [Интерполяция строк](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/strings/#string-interpolation)


**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте LINQ</b></summary>

Используйте LINQ вместо циклов и встроенных активностей при работе с коллекциями<p>

<b>Чек-лист По LINQ</b>
- Понимать разницу между IEnumerable и IQueryable
- Фильтровать (Where()) данные как можно раньше в цепочке вызовов.
- Извлекать (Select()) только нужные поля, а не всю сущность.
- Использовать Any() вместо Count() > 0 для проверки наличия элементов.
- Избегать многократных проходов по коллекции (повторных .Where(), .Select())
- Знать разницу между отложенным и немедленным выполнением.
- Не использовать Where().FirstOrDefault() – просто FirstOrDefault().
- Вызывать Where() перед Select(), а не наоборот.
- Использовать FirstOrDefault(predicate), если проверяется только одно значение.
- Использовать ?? для значений, которые могут быть null.
- Использовать DefaultIfEmpty() при GroupBy().
- Использовать Distinct() для уникальных значений.
- Использовать Union() для объединения без дубликатов.
- Использовать Except() и Intersect() для разницы между коллекциями.

<b>Плюсы</b>
- Компактный, умещается в одну активность
- Упрощает понимание запроса/алгоритма
- Гибкий, расширяемость и деревья выражений позволяют выполнить любой запрос 
- Наличие методы для паралельной обработки

<b>Минусы</b>
- Сложная отладка больших запросов
- Производительность (for > foreach > LINQ)

<b>Примеры:</b>

![UseLINQ](/common/UseLINQ.png)
![UseLINQ2](/common/UseLINQ2.png)

Исключения:
- При работе с внешними сервисами (файлы, почта, БД,UI)
- Большая бизнес логика

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Дополнительные рекомендации по использованию LINQ</b></summary>

<b>Избегайте повторной обаботки коллекции</b><br>
Повторный вызов методов LINQ, таких как Where, приводит к повторному перебору коллекции.<br>
<b>Решение:</b> кэшируйте результаты, если они используются несколько раз:
```csharp
var filtered = myCollection.Where(x => x.Age > 30).ToList(); // Выполняется один раз
var count = filtered.Count;
var average = filtered.Average(x => x.Salary);
```


<b>Избегайте использования Count для проверки на пустоту</b><br>
Count перебирает всю коллекцию, что замедляет выполнение.<br>
<b>Решение:</b> используйте Any() вместо Count():<br>
```csharp
if (myCollection.Any()) // Быстрее, чем myCollection.Count() > 0
```


<b>Параллельное выполнение (PLINQ)</b><br>
Обработка больших коллекций может быть медленной.<br>
<b>Решение:</b> используйте PLINQ для параллельного выполнения:<br>
```csharp
var result = myCollection.AsParallel()
.Where(x => x.IsActive)
.ToList();
```


<b>Проекционная оптимизация (Select)</b><br>
Извлечение всех данных, когда требуется только несколько полей.<br>
<b>Решение:</b> Используйте Select для выборки только необходимых данных:<br>
```csharp
var names = myCollection.Select(x => x.Name).ToList();
```


<b>Фильтрация перед проекцией</b><br>
Проекция больших объёмов данных перед фильтрацией замедляет запрос.<br>
<b>Решение:</b> сначала фильтруйте, затем проецируйте:<br>
```csharp
var result = myCollection.Where(x => x.Age > 30)
.Select(x => x.Name)
.ToList();
```

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте условные операторы</b></summary>

Использование условных операторов вместо встроенных if активностей, позволяют писать компактные и легко читаемый код

Операторы с условным ?. значением NULL и ?[]

![operator](/common/operator.png)

Тернарный условный оператор (?: оператор)

![operator2](/common/operator2.png)

в PIX контекстные значения, это свойства в C#, поэтому нельзя использовать out и ref напрямую

![operator2_2](/common/operator2_2.png)

Операторы объединения со значением NULL (?? И?? = операторы)

![operator3](/common/operator3.png)

Исключения:
- Большая бизнес-логика

![operatornotuse](/common/NotUseOperator.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Используйте класс Path для работы с файлами</b></summary>

Используйте методы из класса Path для работы с файлами:

Например:

- Path.Combine() - объеденяет две строки в путь
- Path.GetExtensions() - Возвращает расширения файла
- PathGetFileName() - Возвращает имя файла и расширение
- Path.GetFullPath() - Возвращает абсолютный путь
- Path.GetTempPath() - Возвращает путь к временной папке <br>
(особено важно использовать вместо созданий временных папок в хард код путях)
- Path.GetTempFileName() - Создает уникальный временный файл


**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте жесткого кодирования</b></summary>

Жесткое кодирование - это практика разработки программного обеспечения, заключающаяся в встраивании данных непосредственно в исходный код программы.

Например:
- Строка подключения к БД
- Учетные записи
- URL
- Пути к папкам
- Id объектов
- Адреса почтовых ящиков
- Параметры конфигурации робота

Все эти данные должны находиться в конфиге или получать из вне (бд и прочие хранилища)

![activity](/common/UseCSharp.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте большого кода в активности 'Выполнить'</b></summary>

Использование большого кода в данной активности приводит к усложнению отладки, т.к. нет возможности отладить код (нет точки остановки, не возможно просматривать контекст) внутри этой активности.

Лучше разбить код на несколько активностей, или полностью отойти от нее.

![activity](/common/UseCSharp.png)

**[⬆ К началу статьи](#в-этой-статье)**

</details>

<details>
    <summary><b>Избегайте Boxing и Unboxing</b></summary>

По сравнению с простыми операциями присваивания операции упаковки и распаковки являются весьма затратными процессами с точки зрения вычислений. При выполнении упаковки типа значения необходимо создать и разместить новый объект. Объем вычислений при выполнении операции распаковки, хотя и в меньшей степени, но тоже весьма значителен.

[Источник (Microsoft)](https://learn.microsoft.com/ru-ru/dotnet/csharp/programming-guide/types/boxing-and-unboxing#performance)

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
Флаг указывает на то, что у метода есть несколько функций. Лучше всего, если у метода есть только одна функция (принцип SRP). Разделите метод на две части, если логический параметр добавляет к методу несколько функций.

- упрощает поддержку
- упрощает модульное тестирование

Например вместо CreateFile → CreateFile, CreateTempFile

**[⬆ К началу статьи](#в-этой-статье)**

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

**[⬆ К началу статьи](#в-этой-статье)**

</details>
