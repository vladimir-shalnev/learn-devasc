<!-- 3.6.1 -->
## Форматы данных

API-интерфейсы Rest, о которых вы узнаете в следующем модуле, позволяют обмениваться информацией с удаленными службами и оборудованием. То же самое и с интерфейсами, построенными на этих API, включая специализированные инструменты интерфейса командной строки и комплекты средств разработки программного обеспечения (SDK) для популярных языков программирования.

При управлении этими API-интерфейсами с помощью программного обеспечения полезно иметь возможность получать и передавать информацию в формах, соответствующих стандартам и пригодных для машинного и человеческого чтения. Это позволяет вам:

* С легкостью используйте готовые программные компоненты и/или встроенные языковые инструменты для преобразования сообщений в формы, которые вам легко манипулировать и извлекать данные, например структуры данных, родные для языка (языков) программирования, который вы используете. Вы также можете преобразовать их в другие стандартные форматы, которые могут вам понадобиться для различных целей.
* Легко писать код для составления сообщений, которые могут потреблять удаленные объекты.
* Читайте и интерпретируйте полученные сообщения самостоятельно, чтобы убедиться, что ваше программное обеспечение обрабатывает их правильно, и вручную составляйте тестовые сообщения для отправки удаленным объектам.
* Более легко обнаруживать «искаженные» сообщения, вызванные ошибками передачи или другими ошибками, мешающими обмену данными.

Сегодня тремя наиболее популярными стандартными форматами для обмена информацией с удаленными API являются XML, JSON и YAML. Стандарт YAML был создан как надмножество JSON, поэтому любой законный документ JSON можно проанализировать и преобразовать в эквивалентный YAML и (с некоторыми ограничениями и исключениями) наоборот. XML, более старый стандарт, не так прост для синтаксического анализа, и в некоторых случаях он только частично (или не может быть преобразован вообще) в другие форматы. Поскольку XML старше, инструменты для работы с ним достаточно зрелые.

Анализ XML, JSON или YAML является частым требованием для взаимодействия с интерфейсами прикладного программирования (API). Позже в этом курсе вы узнаете больше о REpresentational State Transfer (REST) API. На данный момент вам достаточно понять, что часто встречающийся шаблон в реализациях REST API выглядит следующим образом:

1.	Аутентификация, обычно путем POST комбинации пользователя/пароля и получения истекающего токена для использования при аутентификации последующих запросов.
2.	Выполните запрос GET к заданной конечной точке (при необходимости аутентифицируя), чтобы получить состояние ресурса, запросив XML, JSON или YAML в качестве формата вывода.
3.	Измените возвращенный XML, JSON или YAML.
4.	Выполните POST (или PUT) для той же конечной точки (опять же, аутентифицируя по мере необходимости), чтобы изменить состояние ресурса, снова запрашивая XML, JSON или YAML в качестве формата вывода и интерпретируя его по мере необходимости, чтобы определить, была ли операция успешной .

<!-- -->
## 3.6.2 XML

Расширяемый язык разметки (XML) является производным от структурированного обобщенного языка разметки (SGML), а также является родителем языка разметки гипертекста (HTML). XML - это общая методология упаковки текстовых данных в симметричные теги для обозначения семантики. Имена файлов XML обычно заканчиваются на «.xml».

Пример XML-документа

Например, простой XML-документ может выглядеть так:

```
<?xml version="1.0" encoding="UTF-8"?>
<!-- Instance list -->
<vms>
  <vm>
    <vmid>0101af9811012</vmid>
    <type>t1.nano</type>
  </vm>
  <vm>
    <vmid>0102bg8908023</vmid>
    <type>t1.micro</type>
  </vm>
</vms>
```

В этом примере имитируется информация, которую вы можете получить от API управления облачными вычислениями, в котором перечислены экземпляры виртуальных машин.

### Текст XML-документа

На данный момент игнорируйте первую строку документа, которая является специальной частью, известной как пролог (подробнее об этом ниже), и вторую строку, содержащую комментарий. Остальная часть документа называется телом.

Обратите внимание, как отдельные элементы данных в теле (читаемые строки символов) окружены симметричными парами тегов, а открывающий тег окружен `<` а также `>` символы и закрывающий тег, который аналогичен, но с `/` (косая черта) перед именем закрывающего тега.

Также обратите внимание, что некоторые пары тегов окружают несколько экземпляров данных с тегами (например, `<vm>` и соответствующие `<vm>` теги). Основная часть документа в целом всегда окружена самой внешней парой тегов (например, `<vms>...</vms>` пара тегов) или пара корневых тегов.

Структура тела документа подобна дереву с ветвями, отходящими от корня, содержащими возможные дальнейшие ветви, и, наконец, листовыми узлами, содержащими фактические данные. При перемещении вверх по дереву каждая пара тегов в XML-документе имеет родительскую пару тегов и так далее, пока вы не дойдете до пары корневых тегов.

### Пользовательские имена тегов

Имена тегов XML определяются пользователем. Если вы составляете XML для своего собственного приложения, лучше всего выбирать имена тегов, которые четко выражают значение элементов данных, их отношения и иерархию. Имена тегов могут повторяться по мере необходимости для включения нескольких элементов данных (или групп элементов с тегами) одного типа. Например, вы можете создать больше `<vm>` пары тегов для включения дополнительных `<vmid>` а также `<type>` группировки.

При использовании XML из API имена тегов и их значения обычно документируются поставщиком API и могут быть репрезентативными для использования, определенного в схеме общедоступного пространства имен.

### Кодировка специальных символов

Данные передаются в XML как читаемый текст. Как и в большинстве языков программирования, кодирование специальных символов в полях данных XML представляет определенные проблемы.

Например, поле данных не может содержать текст, включающий `<` или `>` символы, используемые XML для разграничения тегов. При написании собственного XML (без схемы) для кодирования таких символов обычно используются кодировки сущностей HTML. В этом случае символы можно заменить их эквивалентами. `<` а также `>` кодировки сущностей. Вы можете использовать аналогичную стратегию для представления широкого спектра специальных символов, лигатурных символов и других объектов.

Обратите внимание, что если вы используете XML в соответствии с требованиями схемы или определенным словарем и иерархией имен тегов (это часто имеет место при взаимодействии с API, такими как NETCONF), вам не разрешается использовать сущности HTML. В редких случаях, когда требуются специальные символы, вы можете использовать их числовые представления, которые для знаков «меньше» и «больше» `<` а также `>` соответственно.

Чтобы избежать индивидуального поиска и преобразования специальных символов, можно включать целые необработанные строки символов в файлы XML, окружая их так называемыми блоками CDATA. Вот пример:

```
<hungarian_sentence><![CDATA[Minden személynek joga van a neveléshez.]]></hungarian_sentence>
```
### XML-пролог

Пролог XML - это первая строка в файле XML. Он имеет специальный формат, заключенный в скобки `<?` а также `?>`. Он содержит имя тега xml и атрибуты, указывающие версию и кодировку символов. Обычно версия - «1.0», а кодировка символов в большинстве случаев - «UTF-8»; в противном случае - «UTF-16». Включение пролога и кодировки может быть важным для обеспечения надежной интерпретации ваших XML-документов синтаксическими анализаторами, редакторами и другим программным обеспечением.

### Комментарии в XML

XML-файлы могут включать комментарии, используя то же соглашение о комментариях, что и в HTML-документах. Например:

```xml
<!-- This is an XML comment. It can go anywhere -->
```

### Атрибуты XML

XML позволяет вставлять атрибуты в теги для передачи дополнительной информации. В следующем примере номер версии XML и кодировка символов находятся внутри `xml` тег. Тем не менее `vmid` а также `type` элементы также могут быть включены как атрибуты в `xml` тег:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- Instance list -->
<vms>
  <vm vmid="0101af9811012" type="t1.nano" />
  <vm vmid="0102bg8908023" type="t1.micro"/>
</vms>
```

Здесь нужно отметить несколько вещей:

* Значения атрибутов всегда должны быть заключены в одинарные или двойные кавычки.
* У элемента может быть несколько атрибутов, но только один атрибут для каждого конкретного имени.
* Если элемент не имеет содержимого, вы можете использовать сокращенную запись, в которой вы помещаете косую черту внутри открытого тега, а не включаете закрывающий тег.

### Пространства имен XML

Некоторые сообщения и документы XML должны включать ссылку на определенные пространства имен, чтобы указывать конкретные тэги и способы их использования в различных контекстах. Пространства имен определяются IETF и другими органами власти в Интернете, организациями и другими объектами, а их схемы обычно размещаются в Интернете как общедоступные документы. Они идентифицируются унифицированными именами ресурсов (URN), используемыми для обеспечения доступности постоянных документов, при этом искателю не нужно беспокоиться об их местонахождении.

В приведенном ниже примере кода показано использование пространства имен, определенного как значение атрибута xmlns, для подтверждения того, что содержимое вызова удаленной процедуры XML должно интерпретироваться в соответствии с устаревшим стандартом NETCONF 1.0. В этом примере кода показана инструкция удаленного вызова процедуры NETCONF в XML. Атрибуты в открывающем теге rpc обозначают идентификатор сообщения и пространство имен XML, которые должны использоваться для интерпретации значения содержащихся тегов. В этом случае вы просите удаленный объект убить определенный сеанс. XML-схема NETCONF задокументирована IETF.

```xml
<rpc message-id="101" xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
  <kill-session>
    <session-id>4</session-id>
  </kill-session>
</rpc>
```

### Интерпретация XML

В приведенном выше примере то, что представлено, предназначено как список или одномерный массив (называемый «экземплярами») объектов (каждый из которых идентифицируется как «экземпляр» тегами в скобках). Каждый объект экземпляра содержит две пары ключ-значение, обозначающие уникальный идентификатор экземпляра и тип сервера виртуальной машины. Семантически эквивалентную структуру данных Python можно объявить следующим образом:

```
vms [
  {
    {"vmid": "0101af9811012"},
    {"type": "t1.nano"}
  },
  {
    {"vmid": "0102bg8908023"},
    {"type": "t1.micro"}
  }  
]
```
Проблема в том, что XML не имеет возможности намеренно указать, что определенное расположение тегов и данных следует интерпретировать как список. Таким образом, нам необходимо интерпретировать намерение автора XML при переводе. Сопоставления между древовидными структурами XML и более эффективными представлениями, возможными на различных компьютерных языках, требуют понимания семантики данных. В меньшей степени это относится к более современным форматам данных, таким как JSON и YAML, которые структурированы так, чтобы хорошо отображать общие простые и составные типы данных в популярных языках программирования.

В этом случае `<vm>` теги, заключающие в скобки данные каждого экземпляра (идентификатор и тип), сворачиваются в пользу использования простых скобок (`{}`) для группировки. Это оставляет вам список объектов Python (которые вы можете назвать «vm objects», но которые явно не названы в этом объявлении), каждый из которых содержит два подобъекта, каждый из которых содержит пару ключ/значение.

<!-- 3.6.3 -->
## JSON

JSON или нотация объектов JavaScript - это формат данных, полученный из того, как литералы сложных объектов записываются в JavaScript (что, в свою очередь, похоже на то, как литералы объектов записываются в Python). Имена файлов JSON обычно заканчиваются на «.json».

Вот пример файла JSON, содержащего несколько пар ключ/значение. Обратите внимание, что два значения - это текстовые строки, одно - логическое значение, а два - массивы:

```json
{
  "edit-config":
  {
    "default-operation": "merge",
    "test-operation": "set",
    "some-integers": [2,3,5,7,9],
    "a-boolean": true,
    "more-numbers": [2.25E+2,-1.0735],
  }
}
```

### Основные типы данных JSON

Основные типы данных JSON включают числа (записанные как положительные и отрицательные целые числа, как числа с плавающей запятой с десятичным числом или в экспоненциальной нотации), строки, логические значения («истина» и «ложь») или значения NULL (значение оставлено пустым).

### Объекты JSON

Как и в JavaScript, отдельные объекты в JSON содержат пары ключ/значение, которые могут быть заключены в фигурные скобки по отдельности:

```json
{"keyname": "value"}
```

В этом примере показан объект со строковым значением (например, слово «value»). Число или логическое значение не должны заключаться в кавычки.

### Карты и списки JSON

Объекты также могут содержать несколько пар ключ/значение, разделенных запятыми, что создает структуры, эквивалентные сложным объектам JavaScript или словарям Python. В этом случае для каждой отдельной пары ключ/значение не требуется отдельный набор скобок, но для всего объекта требуется. В приведенном выше примере ключ «edit-config» идентифицирует объект, содержащий пять пар ключ/значение.

Составные объекты JSON могут быть глубоко вложенными со сложной структурой.

JSON также может выражать упорядоченные массивы (или «списки») данных или объектов JavaScript. В приведенном выше примере ключи «some-integers» и «more-numbers» идентифицируют такие массивы.

### В JSON нет комментариев 

В отличие от XML и YAML, JSON не поддерживает никаких стандартных методов для включения неанализируемых комментариев в код.

### Незначительные пробелы

Пробелы в JSON не имеют значения, и файлы могут быть с отступом с использованием табуляции или пробелов по желанию или вообще не иметь (что является плохой идеей). Это делает формат эффективным (пробелы можно удалить без ущерба для смысла) и надежным для использования в командных строках и в других сценариях, где символы пробелов могут быть легко потеряны при операциях вырезания и вставки.

<!-- 3.6.4 -->
## YAML

YAML, аббревиатура от «YAML Ain't Markup Language», представляет собой надмножество JSON, разработанное для еще более удобного чтения человеком. Он становится все более распространенным форматом для файлов конфигурации, особенно для написания декларативных шаблонов автоматизации для таких инструментов, как Ansible.

Как надмножество JSON, парсеры YAML обычно могут анализировать документы JSON (но не наоборот). Из-за этого YAML лучше, чем JSON, в некоторых задачах, включая возможность встраивать JSON напрямую (включая кавычки) в файлы YAML. JSON также может быть встроен в файлы JSON, но кавычки должны быть экранированы обратной косой чертой. `\"` или закодированы как символы HTML `&quote;`.

Вот версия файла JSON из подраздела JSON, выраженная в YAML. Используйте это как пример, чтобы понять, как работает YAML:

```yaml
---
edit-config:
  a-boolean: true
  default-operation: merge
  more-numbers:
  - 225.0
  - -1.0735
  some-integers:
  - 2
  - 3
  - 5
  - 7
  - 9
  test-operation: set
...
```

### Структура файла YAML

Как показано в примере, файлы YAML обычно открываются тремя дефисами (`---` на отдельной строке) и заканчиваются тремя точками (`...` также отдельная строка). YAML также поддерживает понятие нескольких «документов» в одном физическом файле, в данном случае разделяя каждый документ тремя дефисами на отдельной строке.

### Типы данных YAML

Основные типы данных YAML включают числа (записанные как положительные и отрицательные целые числа, как числа с плавающей запятой с десятичным числом или в экспоненциальной нотации), строки, логические значения (истина и ложь) или нули (значение оставлено пустым).

Строковые значения в YAML часто не заключаются в кавычки. Кавычки требуются, только если строки содержат символы, которые имеют значение в YAML. Например, `{` фигурная скобка, за которой следует пробел, обозначает начало карты. Также необходимо учитывать обратную косую черту и другие специальные символы или строки. Если вы заключите текст в двойные кавычки, вы можете экранировать специальные символы в строке, используя выражения с обратной косой чертой, например `\n` для новой строки.

YAML также предлагает удобные способы кодирования многострочных строковых литералов (подробнее ниже).

### Основные объекты

В YAML базовые (и сложные) типы данных приравниваются к ключам. Ключи обычно не заключаются в кавычки, хотя они могут быть заключены в кавычки, если они содержат двоеточия (`:`) или некоторые другие специальные символы. Ключи также не обязательно должны начинаться с буквы, хотя обе эти функции противоречат требованиям большинства языков программирования, поэтому по возможности лучше держаться от них подальше.

Двоеточие (`:`) разделяет ключ и значение:

```yaml
my_integer: 2
my_float: 2.1
my_exponent: 2e+5
'my_complex:key': "мое строковое значение в кавычках \n"
0.2 : "Вы можете поверить, что это ключ?"
my_boolean: true
my_null: null # может интерпретироваться как пустая строка, иначе
```

### Отступы YAML и файловая структура

YAML не использует скобки и не содержит пары тегов, а вместо этого указывает свою иерархию с помощью отступов. Элементы с отступом под меткой являются «членами» этого помеченного элемента.

Размер отступа зависит от вас. Там, где требуется отступ, можно использовать всего один пробел, хотя рекомендуется использовать два пробела на каждый уровень отступа. Важно быть абсолютно последовательным и использовать пробелы, а не табуляции.

### Карты и списки

YAML легко представляет более сложные типы данных, такие как карты, содержащие несколько пар ключ/значение (эквивалентных словарям в Python) и упорядоченные списки.

Карты обычно выражаются в нескольких строках, начиная с ключа метки и двоеточия, за которыми следуют элементы с отступом в последующих строках:

```yaml
mymap:
  myfirstkey: 5
  mysecondkey: The quick brown fox
```

Списки (массивы) представлены аналогичным образом, но с элементами с необязательным отступом, которым предшествуют одиночное тире и пробел:

```yaml
mylist:
  - 1
  - 2
  - 3
```

Карты и списки также могут быть представлены в так называемом «синтаксисе потока», который очень похож на JavaScript или Python:

```yaml
mymap: { myfirstkey: 5, mysecondkey: The quick brown fox}
mylist: [1, 2, 3]
```

### Длинные строки

Вы можете представлять длинные строки в YAML, используя синтаксис «сворачивания», где предполагается, что разрывы строк заменяются пробелами при анализе/использовании файла, или в синтаксисе без сворачивания. Длинные строки не могут содержать экранированные специальные символы, но могут (теоретически) содержать двоеточия, хотя некоторые программы не соблюдают это правило.

```yaml
mylongstring: >
  This is my long string
  which will end up with no linebreaks in it
myotherlongstring: |
  This is my other long string
  which will end up with linebreaks as in the original

Обратите внимание на разницу в двух приведенных выше примерах. Индикатор больше (>) дает нам синтаксис сворачивания, а вертикальная черта (|) - нет.

### Комментарии

Комментарии в YAML могут быть вставлены где угодно, кроме длинного строкового литерала, и им предшествуют знак решетки и пробел:

```yaml
# это комментарий
```

### Дополнительные возможности YAML

YAML имеет гораздо больше функций, которые чаще всего встречаются при его использовании в контексте определенных языков, таких как Python, или при преобразовании в JSON или другие форматы. Например, YAML 1.2 поддерживает схемы и теги, которые можно использовать для устранения неоднозначности интерпретации значений. Например, чтобы заставить число интерпретироваться как строка, вы можете использовать строку !!str, которая является частью схемы YAML «Failsafe»:

```yaml
mynumericstring: !!str 0.1415
```
<!-- 3.6.5 -->
## Парсинг и сериализация

Парсинг означает анализ сообщения, разбиение его на составные части и понимание их целей в контексте. Когда сообщения передаются между компьютерами, они перемещаются в виде потока символов, который фактически является строкой. Это необходимо проанализировать в семантически эквивалентную структуру данных, содержащую данные распознанных типов (например, целые числа, числа с плавающей запятой, строки и т. д.). Прежде чем приложение сможет интерпретировать данные и действовать с ними.

Сериализация примерно противоположна парсингу. Например, для передачи информации через интерфейс REST вас могут попросить взять локально сохраненные данные (например, данные, хранящиеся в словарях Python) и вывести их как эквивалентные JSON, YAML или XML в строковой форме для представления удаленному устройству. ресурс.

### Пример

Например, представьте, что вы хотите отправить первоначальный запрос какой-либо удаленной конечной точке REST, чтобы узнать о состоянии запущенных служб. Для этого обычно требуется пройти аутентификацию в REST API, указав свое имя пользователя (адрес электронной почты), а также ключ разрешения, полученный в ходе предыдущей транзакции. Вы могли сохранить имя пользователя и ключ в словаре Python, например:

```python
auth = {
  "user": {
    "username": "myemail@mydomain.com",
    "key": "90823ff08409408aebcf4320384"
  }
}
```
Но REST API требует, чтобы эти значения были представлены в виде XML в строковой форме, добавленной к вашему запросу как значение пары ключ/значение, называемой "auth":

```
https://myservice.com/status/services?auth=<XML строка, содержащая имя пользователя и ключ>
```
Сам XML может принять этот формат, при этом значения ключей Python будут преобразованы в пары одноименных тегов, содержащие значения данных:

```xml
<user>
  <username>myemail@mydomain.com</username>
  <key>90823ff08409408aebcf4320384</key>
</user>
```

Обычно вы используете функцию сериализации (из библиотеки Python) для вывода структуры данных аутентификации в виде строки в формате XML, добавляя ее в свой запрос:

```
import dicttoxml    // serialization library
import requests     // http request library
auth = {            // Python dict, containing authentication info
  "user": {
    "username": "myemail@mydomain.com",
    "key": "90823ff08409408aebcf4320384"
  }
}
get_services_query = "https://myservice.com/status/services"
xmlstring = dicttoxml(auth)       // convert dict to XML in string form
myresponse = requests.get(get_services_query,auth=xmlstring)  // query service
```

На этом этапе служба может ответить, установив для переменной `myresponse` строку, подобную приведенной ниже, содержащую имена и статусы служб в формате XML:

```xml
<services>
  <service>
    <name>Service A</name>
    <status>Running</status>
  </service>
  <service>
    <name>Service B</name>
    <status>Idle</status>
  </service>
</services>
```

Затем вам нужно будет проанализировать XML, чтобы извлечь информацию в форму, к которой Python мог бы удобно получить доступ.

```python
import untangle     // xml parser library
myreponse_python = untangle.parse(myresponse)
print myreponse_python.services.service[1].name.cdata,myreponse_python.services.
service[1].status.cdata
```

В этом случае библиотека `untangle` будет преобразовывать XML в словарь, корневой элемент (`services`) которого содержит список (`service []`) пар элементов объекта ключ/значение, обозначающих имя и статус каждой службы. Затем вы можете получить доступ к значению `cdata` элементов, чтобы получить текстовое содержимое каждого листового узла XML. Приведенный выше код напечатает:

```
Service B  Idle
```

Популярные языки программирования, такие как Python, обычно включают в себя простые в использовании функции синтаксического анализа, которые могут принимать данные, возвращаемые функцией ввода-вывода, и создавать семантически эквивалентную внутреннюю структуру данных, содержащую допустимые типизированные данные. На исходящей стороне они содержат сериализаторы, которые делают противоположное, превращая внутренние структуры данных в семантически эквивалентные сообщения, отформатированные в виде символьных строк.

<!-- 3.6.6 -->
## Лабораторная работа - Анализ различных типов данных с помощью Python

В этой лабораторной работе вы будете использовать Python для анализа каждого формата данных по очереди: XML, JSON и YAML. Вы пройдете по примерам кода и исследуете, как работает каждый парсер.

Вы выполните следующие задачи:

* Часть 1. Запуск виртуальной машины DEVASC
* Часть 2: Анализ XML в Python
* Часть 3: Разбор JSON в Python
* Часть 4: Разбор YAML в Python