<!-- 4.8.1 -->
## Устранение неполадок запросов REST API

Вы узнали о REST API и изучали их в лабораторных условиях. В предоставленных сценариях все работало правильно и всегда было успешно. Конечно, в реальной жизни так бывает не всегда. В какой-то момент вы обнаружите, что выполняете запросы REST API и не получаете ожидаемого ответа. В этой части вы узнаете, как устранять наиболее распространенные проблемы с REST API.

Обратите внимание, что при применении этих концепций и советов по устранению неполадок к конкретному REST API убедитесь, что у вас есть справочное руководство по API и информация об аутентификации API; это сделает вашу работу намного проще.

<!-- 4.8.2 -->
## Нет ответа и код состояния HTTP от сервера API

Начнем с простого вопроса:

Верно или нет: каждый запрос, отправленный на сервер REST API, будет возвращать ответ с кодом состояния.

Ответ неверный. Хотя можно было ожидать, что вы всегда будете получать код состояния HTTP, такой как 2xx, 4xx и 5xx, во многих случаях сервер API не может быть достигнут или не отвечает. В этих сценариях вы не получите код состояния HTTP, потому что сервер API не может отправить ответ.

Хотя неотвечающий сервер может быть большой проблемой, основную причину отсутствия ответа можно легко определить. Обычно вы можете определить, что пошло не так, по сообщениям об ошибках, полученным в результате запроса.

Давайте рассмотрим несколько советов по устранению неполадок, которые помогут вам отладить, почему сервер API не отправил ответ и код состояния HTTP, а также некоторые потенциальные способы решения проблемы.

### Ошибка на стороне клиента

Первое, что нужно проверить, - это ошибка на стороне клиента; именно здесь у вас будет больше контроля с точки зрения решения проблемы.

### Есть ошибка пользователя?

Некоторые вопросы, которые следует задать, чтобы изолировать ошибки от пользователя, включают:

* URI правильный?
* Работал ли этот запрос раньше?

При первом использовании REST API часто бывает ошибочно вводить URI. Обратитесь к справочному руководству по API для этого конкретного API, чтобы убедиться, что URI запроса правильный.

### Пример неверного URI

Чтобы проверить недопустимое условие URI, запустите такой сценарий, как этот, который просто делает запрос к URI, в котором отсутствует схема. Вы можете создать файл Python или запустить его непосредственно в интерпретаторе Python.

```python
import requests
uri = "sandboxdnac.cisco.com/dna/intent/api/v1/network-device"
resp = requests.get(uri, verify = False)
```
Трассировка будет выглядеть так:

```
....requests.exceptions.MissingSchema: Invalid URL 'sandboxdnac.cisco.com/dna/intent/api/v1/network-device': No schema supplied. Perhaps you meant http://sandboxdnac.cisco.com/dna/intent/api/v1/network-device?....
```

Пример неправильного доменного имени

Чтобы проверить условие неправильного имени домена, запустите сценарий, подобный этому, который просто делает запрос к URI с неправильным именем домена.

```python
import requests
url = "https://sandboxdnac123.cisco.com/dna/intent/api/v1/network-device"
resp = requests.get(url, verify = False)
```

Трассировка будет выглядеть так:

```python
....requests.exceptions.ConnectionError: HTTPSConnectionPool(host='sandboxdnac123.cisco.com', port=443): Max retries exceeded with url: /dna/intent/api/v1/network-device (Caused by NewConnectionError('<urllib3.connection.VerifiedHTTPSConnection object at 0x109541080>: Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known'))....
```

### Есть проблема с подключением?

Если URI правильный, он все еще может быть недоступен. Задайте себе следующие вопросы:

* Есть ли проблемы с прокси, брандмауэром или VPN?
* Есть ли ошибка SSL?

### Пример недействительного сертификата

Эта проблема может возникнуть только в том случае, если URI REST API использует безопасное соединение (HTTPS).

Когда схема URI - HTTPS, соединение будет выполнять квитирование SSL между клиентом и сервером, чтобы аутентифицировать друг друга. Это рукопожатие должно быть успешным до того, как запрос REST API будет отправлен на сервер API.

При использовании библиотеки  `requests` в Python для выполнения запроса REST API, если подтверждение SSL не удается, трассировка будет содержать `request.exceptions.SSLError`.

Например, если квитирование SSL не удается из-за сбоя проверки сертификата, трассировка будет выглядеть следующим образом:

```
requests.exceptions.SSLError: HTTPSConnectionPool(host='xxxx', port=443): Max retries exceeded with url: /dna/intent/api/v1/network-device (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: self signed certificate (_ssl.c:1108)')))
```

В этой ситуации необходимо исправить недействительный сертификат. Но если вы работаете в лабораторной среде, где сертификаты еще не действительны, вы можете отключить настройку проверки сертификата.

Чтобы отключить его для библиотеки запросов в Python, добавьте в запрос параметр `verify`.

```
resp = requests.get(url, verify = False)
```

**Решение**: Ошибки клиента обычно легко исправить, особенно когда сообщения об ошибках в трассировке указывают, что может быть не так и возможное решение. Тщательно проанализируйте журналы на предмет первопричины.

### Ошибка на стороне сервера

После того, как вы убедились, что ошибок на стороне клиента нет, следующее, что нужно проверить, это ошибки на стороне сервера.

### Сервер API работает?

Наиболее очевидное место для начала - убедиться, что сам сервер API работает правильно. Вы должны задать себе несколько вопросов:

* Питание выключено?
* Есть проблемы с кабелем?
* Изменилось ли доменное имя?
* Сеть не работает?

Чтобы проверить, доступен ли IP-адрес, запустите сценарий, подобный этому, который просто делает запрос к URL-адресу и ожидает ответа.

> ** Предупреждение **: ожидайте долгой задержки при запуске этого скрипта.

```python
import requests
url = "https://209.165.209.225/dna/intent/api/v1/network-device"
resp = requests.get(url, verify = False)
```

> Примечание: IP-адрес в этом сценарии вымышленный, но сценарий дает тот же результат, что и сервер, который недоступен.

Если сервер API не работает, вы получите долгое молчание, за которым последует трассировка, которая выглядит следующим образом:

```python
....requests.exceptions.ConnectionError: HTTPSConnectionPool(host='209.165.209.225', port=443): Max retries exceeded with url: /dna/intent/api/v1/network-device (Caused by NewConnectionError('<urllib3.connection.VerifiedHTTPSConnection object at 0x10502fe20>: Failed to establish a new connection: [Errno 60] Operation timed out'))....
```

**Есть ли проблема со связью между сервером API и клиентом?**

Если сервер работает правильно, вам нужно определить, есть ли причина, по которой клиент не получает ответ. Спроси себя:

* Доступны ли IP-адрес и доменное имя из местоположения клиента в сети?
* Сервер API отправляет ответ, но клиент его не получает?

Чтобы проверить это условие, используйте инструмент сетевого захвата, чтобы увидеть, не теряется ли ответ от сервера API при обмене данными между сервером API и клиентом. Если у вас есть доступ, просмотрите журналы сервера API, чтобы определить, был ли получен запрос, обработан ли он и был ли отправлен ответ.

**Решение**: Проблемы на стороне сервера не могут быть решены на стороне клиента API. Обратитесь к администратору сервера API для решения этой проблемы.

<!-- 4.8.3 -->
## Интерпретация кодов состояния

Поскольку REST API основаны на http, они также используют коды состояния http для уведомления клиентов API о результатах запроса.

Если не указано иное, код состояния является частью стандарта HTTP/1.1 (RFC 7231), что означает, что первая цифра кода состояния определяет класс ответа. (Последние две цифры не имеют роли класса или категоризации, но помогают различать результаты.) Всего существует 5 из этих категорий:

* **1xx: информационный**: Запрос получен, обработка продолжается.
* **2xx: успешный**: Действие было успешно получено, понято и принято.
* **3xx: перенаправление**: Для выполнения запроса необходимо предпринять дальнейшие действия.
* **4xx: ошибка клиента**: Запрос содержит неверный синтаксис или не может быть выполнен.
* **5xx: ошибка сервера**: Серверу не удалось выполнить явно действительный запрос.

Обычно мы видим 2xx, 4xx и 5xx с сервера REST API. Обычно вы можете найти основную причину ошибки, когда понимаете значение кодов ответа. Иногда сервер API также предоставляет дополнительную информацию в теле ответа.

Для всех запросов, имеющих код состояния возврата, выполните следующие действия для устранения ошибок:

**Шаг 1**: Проверьте код возврата. Это может помочь вывести код возврата в ваш скрипт на этапе разработки.

**Шаг 2**: Проверить тело ответа. Вывести тело ответа во время разработки; в большинстве случаев вы можете найти, что пошло не так, в ответном сообщении, отправленном вместе с кодом состояния.

**Шаг 3**: Если вы не можете решить проблему, используя два вышеуказанных шага, используйте ссылку на код состояния, чтобы понять определение кода состояния.

<!-- 4.8.4 -->
## Коды состояния 2xx и 4xx

Давайте рассмотрим эти коды подробнее:

### 2xx - успешный

Когда клиент получает код ответа 2xx, это означает, что запрос клиента был успешно получен, понят и принят. Однако вы всегда должны проверять, что ответ указывает на успешное выполнение правильного действия и что сценарий выполняет то, что, по вашему мнению, должно.

### 4xx - ошибка на стороне клиента

Ответ 4xx означает, что ошибка на стороне клиента. Некоторые серверы могут включать объект, содержащий объяснение ошибки. Если нет, то вот несколько общих рекомендаций по устранению распространенных ошибок 4xx:

* 400- Плохой запрос

Запрос не может быть понят сервером из-за неправильного синтаксиса. Проверьте синтаксис вашего API.

Одной из причин ошибки 400 Bad Request является сам ресурс. Дважды проверьте конечную точку и ресурс, который вы вызываете, не ошиблись ли вы в написании одного из ресурсов или забыли "s", чтобы сделать его множественным числом, например `/device`  против `/devices`  или `/interface`  против `/interfaces`? Является ли URI правильным и полным?

Другой причиной ошибки `400 Bad Request` может быть проблема синтаксиса в объекте JSON, который представляет ваш запрос POST.

Давайте посмотрим на воображаемый пример сервиса:

```python
import requests
url = "http://myservice/api/v1/resources/house/rooms/id/"
resp = requests.get(url,auth=("person2","super"),verify = False)
print (resp.status_code)
print (resp.text)
```

Вы можете догадаться, что не так, просто прочитав код?

В этом примере возвращается код состояния 400. Сторона сервера также сообщает вам «Поле идентификатора не указано», поскольку `id` является обязательным для этого запроса API. Вам нужно будет посмотреть это в документации, чтобы убедиться в этом.

```python
import requests
url = "http://myservice/api/v1/resources/house/rooms/id/1001001027331"
resp = requests.get(url,auth=("person2","super"),verify = False)
print (resp.status_code)
print (resp.text)
```

* 401 - Несанкционированный

Это сообщение об ошибке означает, что серверу не удалось проверить подлинность запроса.

Проверьте свои учетные данные, включая имя пользователя, пароль, ключ API, токен и т. д. Если с этими элементами нет проблем, вы можете снова проверить URI запроса, потому что сервер может отклонить доступ в случае неправильного URI запроса.

```
import requests
url = "http://myservice/api/v1/resources/house/rooms/all"
resp = requests.get(url,verify = False)
print (resp.status_code)
print (resp.text)
```

Этот пример возвращает код состояния 401. Можете ли вы угадать, что не так, просто прочитав код?

Запрос не предоставляет учетные данные. Аутентификация `auth=("person1","great")` следует добавить в код.

```python
import requests
url = "http://myservice/api/v1/resources/house/rooms/all"
resp = requests.get(url,auth=("person1","great"),verify = False)
print (resp.status_code)
print (resp.text)
```

* 403- Запрещено

В этом случае сервер распознает учетные данные аутентификации, но клиент не авторизован для выполнения запроса. Некоторые API, такие как Cisco DNA Center, имеют контроль доступа на основе ролей и требуют роли суперадминистратора для выполнения определенных API. Опять же, справочное руководство по API может предоставить дополнительную информацию.

```python
import requests
url = "http://myservice/api/v1/resources/house/rooms/id/1"
resp = requests.get(url,auth=("person1","great"),verify = False)
print (resp.status_code)
print (resp.text)
```

Почему этот воображаемый фрагмент кода не работает? Мы просто использовали имя пользователя `person1` и пароль `great`  и это сработало в последнем примере.

Код состояния 403 не является проблемой аутентификации; сервер верит в личность пользователя, просто у пользователя недостаточно прав для использования этого конкретного API. Если мы используем `person2/super`  в качестве имени пользователя/пароля для аутентификации он будет работать, потому что person2 имеет право выполнять этот API. Можете представить, как заставить его работать?

Аутентификацию следует изменить, чтобы использовать `person2/super`  вместо того `person1/great`.

```python
import requests
url = "http://myservice/api/v1/resources/house/rooms/id/1"
resp = requests.get(url,auth=("person2","super"),verify = False)
print (resp.status_code)
print (resp.text)
```

* 404- Не обнаружена

Сервер не нашел ничего, соответствующего URI запроса; проверьте URI запроса, чтобы убедиться, что он правильный. Если код раньше работал, вы можете проверить последнее справочное руководство по API, поскольку синтаксис API может со временем меняться.

Рассмотрим API Cisco DNA Center для получения всех интерфейсов. В названии написано «все интерфейсы», поэтому вы пытаетесь использовать `api/v1/interfaces`, но вы получите ошибку 404, потому что запрос API на самом деле `api/v1/interface`.

```
import requests
url = "http://myservice/api/v1/resources/house/room/all"
resp = requests.get(url,auth=("person1","great"),verify = False)
print (resp.status_code)
print (resp.text)
```

Почему этот скрипт вернул код состояния 404 и можете ли вы догадаться, как это исправить?

Фактический URI `/api/v1/resources/house/rooms/all`. Здесь нет ресурса `room`, поэтому серверу не удалось найти ресурс, соответствующий URI запроса.

```python
import requests
url = "http://myservice/api/v1/resources/house/rooms/all"
resp = requests.get(url,auth=("person1","great"),verify = False)
print (resp.status_code)
print (resp.text)
```

* 405- Метод не разрешен

В этом случае запрос был распознан сервером, но метод, указанный в запросе, был отклонен сервером. Вы можете проверить справочное руководство по API, чтобы узнать, какие методы ожидает сервер. Ответ сервера может также включать `Allow header` содержащий список допустимых методов для запрошенного ресурса.

Например, если вы по ошибке используете метод POST для API, ожидающего метода GET, вы получите ошибку 405.

```
import requests
url = "http://myservice/api/v1/resources/house/rooms/id/1"
resp = requests.post(url,auth=("person2","super"),verify = False)
print (resp.status_code)
print (resp.text)
```

* 406- Недопустимо

Эта ошибка указывает на то, что целевой ресурс не имеет текущего представления, приемлемого для клиента. У сервера есть данные, но он не может представить их с помощью каких-либо опций, перечисленных в клиентском заголовке `Accept-`.

Например, клиент запрашивает изображения SVG: `Accept: image/svg+xml`

```python
import requests
headers = {'Accept': 'image/svg+xml'}
url = "http://myservice/api/v1/resources/house/rooms/id/1"
resp = requests.get(url,headers=headers,auth=("person2","super"),verify = False)
print (resp.status_code)
print (resp.text)
```

В этом случае сервер не поддерживает запрошенный ресурс, `image/svg+xml`, поэтому он отвечает ошибкой 406.

* 407- Требуется проверка подлинности прокси

Этот код похож на 401 (Неавторизованный), но указывает, что клиент должен сначала аутентифицировать себя с помощью прокси. В этом сценарии между клиентом и сервером есть прокси-сервер, и код ответа 407 указывает, что клиенту необходимо сначала пройти аутентификацию на прокси-сервере.

* 409 - Запрос не может быть выполнен из-за конфликта с текущим состоянием целевого ресурса.

Например, конфликт редактирования, когда ресурс редактируется несколькими пользователями, приведет к ошибке 409. Повторная попытка запроса позже может быть успешной, если конфликт разрешен сервером.

* 415- Неподдерживаемый тип носителя

В этом случае клиент отправил тело запроса в формате, который сервер не поддерживает. Например, если клиент отправляет XML на сервер, который принимает только JSON, сервер вернет ошибку 415.

```python
import requests
headers = {"content-type":"application/xml"}
url = "http://myservice/api/v1/resources/house/rooms/id/1"
resp = requests.get(url,headers=headers,auth=("person2","super"),verify = False)
print (resp.status_code)
print (resp.text)
```

Можете ли вы догадаться по сообщению об ошибке, что может исправить код?

Пропуск заголовка или добавление заголовка `{"content-type":"application/json"}` будет работать.

```
import requests
headers = {"content-type":"application/json"}

url = "http://myservice/api/v1/resources/house/rooms/id/1"
resp = requests.get(url,headers=headers,auth=("person2","super"),verify = False)
print (resp.status_code)
print (resp.text)
```

Это самые распространенные коды ответов 4xx. Если вы встретите другие коды ответа 4xx, вы можете обратиться к RFC 2616, 6.1, строка состояния или RFC 7231, раздел 6, Коды состояния ответа, чтобы получить дополнительную информацию о том, что они означают.

<!-- 4.8.5 -->
## Коды состояния 5xx

### Ошибка на стороне сервера 5xx

Ответ 5xx указывает на ошибку на стороне сервера.

* 500 - Внутренняя ошибка сервера

Эта ошибка означает, что сервер обнаружил непредвиденное условие, которое помешало ему выполнить запрос.

* 501 - Не реализована

Эта ошибка означает, что сервер не поддерживает функции, необходимые для выполнения этого запроса. Например, сервер ответит кодом 501, если он не распознает метод запроса и, следовательно, не может поддерживать его для какого-либо ресурса.

* 502 - Плохой шлюз

Эта ошибка означает, что сервер, выступая в качестве шлюза или прокси, получил недопустимый ответ от входящего сервера, к которому он обращался при попытке выполнить запрос.

* 503 - Сервис недоступен

Этот код указывает на то, что сервер в настоящее время не может обработать запрос из-за временной перегрузки или планового обслуживания, которые, вероятно, будут разрешены после задержки.

* 504 - Тайм-аут шлюза

Эта ошибка означает, что сервер, выступая в качестве шлюза или прокси, не получил своевременного ответа от вышестоящего сервера, к которому он должен был получить доступ, чтобы выполнить запрос.

Если вы получаете ошибку 500 или 501, проверьте справочное руководство по API, чтобы убедиться, что запрос действителен. При возникновении других ошибок 5xx обратитесь к администратору сервера API для решения проблемы.

Прежде чем приступить к устранению неполадок API, крайне важно иметь справочное руководство по API и ссылки на код состояния. Если вы не можете получить код ответа, вы, вероятно, сможете определить причину из журнала ошибок в своем сценарии. Если вы можете получить код ответа, вы сможете определить основную причину ошибки, в том числе на стороне клиента или на стороне сервера, понимая код ответа.