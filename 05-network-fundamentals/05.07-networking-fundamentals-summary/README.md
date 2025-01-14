<!-- 5.7.1 -->
## Что я узнал в этом модуле?

### Введение в основы работы с сетью

Сеть состоит из конечных устройств, таких как компьютеры, мобильные устройства и принтеры, которые соединены сетевыми устройствами, такими как коммутаторы и маршрутизаторы. Сеть позволяет устройствам связываться друг с другом и обмениваться данными. Набор протоколов - это набор протоколов, которые работают вместе для предоставления комплексных услуг сетевой связи. Эталонные модели OSI и TCP/IP используют уровни для описания функций и служб, которые могут выполняться на этом уровне. Форма, которую принимает часть данных на любом уровне, называется блоком данных протокола (PDU). На каждом этапе процесса инкапсуляции PDU имеет другое имя, отражающее его новые функции: данные, сегмент, пакет, кадр и биты.

Уровни эталонной модели OSI описаны здесь снизу вверх:

1.	Физический уровень отвечает за передачу и прием необработанных битовых потоков.
2.	Уровень канала передачи данных обеспечивает обмен данными между сетевыми адаптерами в одной сети.
3.	Сетевой уровень предоставляет услуги, позволяющие конечным устройствам обмениваться данными между сетями.
4.	Транспортный уровень обеспечивает возможность надежности и управления потоком.
5.	Сеансовый уровень позволяет хостам устанавливать сеансы между собой.
6.	Уровень представления определяет контекст между объектами уровня приложения.
7.	Прикладной уровень - это уровень OSI, ближайший к конечному пользователю и содержащий множество протоколов, обычно необходимых пользователям.

Конечные устройства реализуют протоколы для всего «стека», всех уровней. Источник сообщения (данные) инкапсулирует данные с соответствующими протоколами, в то время как конечный пункт назначения деинкапсулирует каждый заголовок/трейлер протокола для получения сообщения (данных).

### Уровень сетевого интерфейса

Ethernet - это набор руководящих принципов и правил, которые позволяют различным сетевым компонентам работать вместе. Эти рекомендации определяют кабели и сигнализацию на физическом уровне и уровне канала передачи данных модели OSI. В терминологии Ethernet контейнер, в который помещаются данные для передачи, называется фреймом. Кадр содержит информацию заголовка, информацию о конце и фактические данные, которые передаются. Важные поля кадра MAC-адреса включают преамбулу, SFD, MAC-адрес назначения, исходный MAC-адрес, тип, данные и FCS. Каждая карта NIC имеет уникальный адрес управления доступом к среде (MAC), который идентифицирует физическое устройство, также известный как физический адрес. MAC-адрес определяет расположение определенного конечного устройства или маршрутизатора в локальной сети. Существует три основных типа сетевых коммуникаций: одноадресная, широковещательная и многоадресная.

Коммутатор создает и поддерживает таблицу (называемую таблицей MAC-адресов), которая сопоставляет MAC-адрес назначения с портом, который используется для подключения к узлу. Коммутатор пересылает кадры путем поиска совпадения между MAC-адресом назначения в кадре и записью в таблице MAC-адресов. В зависимости от результата коммутатор решит, фильтровать или заливать раму. Если MAC-адрес назначения находится в таблице MAC-адресов, он отправит его на указанный порт. В противном случае он затопит все порты, кроме входящего.

VLAN группирует устройства в одной или нескольких локальных сетях, которые настроены для связи, как если бы они были подключены к одному проводу, хотя на самом деле они расположены в нескольких разных сегментах локальной сети. Сети VLAN определяют широковещательные домены 2-го уровня. VLAN часто связаны с IP-сетями или подсетями. Магистраль - это двухточечный канал между двумя сетевыми устройствами, по которому передается более одной VLAN. Магистраль VLAN расширяет VLAN по всей сети. Сети VLAN подразделяются на три диапазона: зарезервированные, обычные и расширенные.

### Межсетевой уровень

Взаимосвязанные сети должны иметь способы связи, а межсетевое взаимодействие обеспечивает этот метод связи «между» (меж) сетями. Каждое устройство в сети имеет уникальный IP-адрес. IP-адрес и MAC-адрес используются для доступа и связи между всеми сетевыми устройствами. Без IP-адресов не было бы Интернета. Адрес IPv4 составляет 32 бита, каждый октет (8 бит) представлен как десятичное значение, разделенное точкой. Это представление называется десятичной системой счисления с точками. Существует три типа IPv4-адресов: сетевой адрес, адреса хоста и широковещательный адрес. Маска подсети IPv4 (или длина префикса) используется, чтобы отличать сетевую часть от части хоста IPv4-адреса.

IPv6 разработан, чтобы стать преемником IPv4. IPv6 имеет большее 128-битное адресное пространство, обеспечивая 340 ундециллионов возможных адресов. Предварительная агрегация IPv6, упрощенная перенумерация сети и множественная адресация сайтов IPv6 обеспечивают иерархию адресации IPv6, которая обеспечивает более эффективную маршрутизацию. Адреса IPv6 представлены как серия 16-битных шестнадцатеричных полей (гекстет), разделенных двоеточиями (`:`) в формате: `x:x:x:x:x:x:x:x`. Предпочтительный формат включает все шестнадцатеричные значения. Есть два правила, которые можно использовать для уменьшения представления IPv6-адреса: 1. Пропускать ведущие нули в каждом гекстете и 2. Заменить одну строку, состоящую из нулевых гекстетов, двойным двоеточием (`::`).

Одноадресный адрес IPv6 - это идентификатор для одного интерфейса на одном узле. Глобальный одноадресный адрес (GUA) (или агрегируемый глобальный одноадресный адрес) - это IPv6, аналогичный общедоступному IPv4-адресу. Префикс глобальной маршрутизации - это префикс или сетевая часть адреса, которая назначается провайдером, например интернет-провайдером, клиенту или сайту. Поле ID подсети - это область между префиксом глобальной маршрутизации и идентификатором интерфейса. Идентификатор интерфейса IPv6 эквивалентен хостовой части адреса IPv4. Локальный адрес канала IPv6 (LLA) позволяет устройству обмениваться данными с другими устройствами с поддержкой IPv6 по тому же каналу и только по этому каналу (подсети). Адреса многоадресной рассылки IPv6 аналогичны адресам многоадресной рассылки IPv4. Напомним, что многоадресный адрес используется для отправки одного пакета одному или нескольким адресатам (многоадресная группа).

Маршрутизатор - это сетевое устройство, которое функционирует на интернет-уровне модели TCP/IP или сетевом уровне 3-го уровня модели OSI. Маршрутизация включает пересылку пакетов между разными сетями. Маршрутизаторы используют таблицу маршрутизации для маршрутизации между сетями. Маршрутизатор обычно выполняет две основные функции: определение пути и маршрутизацию или пересылку пакетов. Таблица маршрутизации может содержать следующие типы записей: напрямую подключенные сети, статические маршруты, маршруты по умолчанию и динамические маршруты.

### Сетевые устройства

Ключевым понятием коммутации Ethernet является широковещательный домен. Широковещательный домен - это логическое разделение, в котором все устройства в сети могут связываться друг с другом посредством широковещательной передачи на уровне канала данных. S-ведьмы теперь могут одновременно передавать и получать данные. Переключатели выполняют следующие функции:

* Работают на уровне доступа к сети модели TCP/IP и канальном уровне 2 уровня модели OSI.
* Фильтрация или лавинная рассылка кадров на основе записей в таблице MAC-адресов
* Имеют большое количество высокоскоростных и полнодуплексных портов

Коммутатор работает в одном из следующих режимов переключения: сквозной и с промежуточным накоплением. Коммутаторы LAN имеют высокую плотность портов, большие буферы кадров и быструю внутреннюю коммутацию.

Маршрутизаторы необходимы для доступа к устройствам, которые не находятся в одной локальной сети. Маршрутизаторы используют таблицы маршрутизации для маршрутизации трафика между разными сетями. Маршрутизаторы подключаются к разным сетям (или подсетям) через их интерфейсы и имеют возможность маршрутизировать трафик данных между ними.

Маршрутизаторы выполняют следующие функции:

* Они работают на интернет-уровне модели TCP/IP и сетевом уровне 3-го уровня модели OSI.
* Маршрутизация пакетов между сетями основана на записях в таблице маршрутизации.
* Они поддерживают большое количество сетевых портов, включая различные медиа-порты LAN и WAN, которые могут быть медными или оптоволоконными. Количество интерфейсов на маршрутизаторах обычно намного меньше, чем в коммутаторах, но количество поддерживаемых интерфейсов больше. IP-адреса настроены на интерфейсах.

Маршрутизаторы поддерживают три механизма пересылки пакетов: коммутация процессов, быстрая коммутация и CEF.

Брандмауэр - это аппаратная или программная система, предотвращающая несанкционированный доступ в сеть или из нее. Самый простой тип межсетевого экрана - это межсетевой экран с фильтрацией пакетов без сохранения состояния. Вы создаете статические правила, разрешающие или запрещающие пакеты на основе информации заголовка пакета. Межсетевой экран проверяет пакеты, когда они проходят через межсетевой экран, сравнивает их со статическими правилами и соответственно разрешает или запрещает трафик. Межсетевой экран с фильтрацией пакетов с отслеживанием состояния выполняет ту же проверку заголовков, что и межсетевой экран с фильтрацией пакетов без сохранения состояния, но также отслеживает состояние соединения. Чтобы отслеживать состояние, эти межсетевые экраны поддерживают таблицу состояний. Самый продвинутый тип межсетевого экрана - межсетевой экран прикладного уровня. При использовании этого типа выполняется глубокая проверка пакета вплоть до уровня 7 модели OSI.

Балансировка нагрузки улучшает распределение рабочих нагрузок между несколькими вычислительными ресурсами, такими как серверы, кластер серверов, сетевые каналы и т. Д. Балансировка нагрузки на серверы помогает обеспечить доступность, масштабируемость и безопасность приложений и служб, распределяя работу одного сервера между несколько серверов. На уровне устройства балансировщик нагрузки обеспечивает высокую доступность сети, поддерживая: избыточность устройства, масштабируемость и безопасность. На уровне сетевых услуг балансировщик нагрузки предоставляет расширенные услуги, поддерживая: высокую доступность услуг, масштабируемость и безопасность на уровне услуг.

Сетевые диаграммы отображают визуальное и интуитивно понятное представление сети, как все устройства подключены, в каких зданиях, полах, туалетах они расположены, какой интерфейс подключается к этому конечному устройству и т. Д. Как правило, существует два типа сетевых диаграмм: Уровни 2 схемы физического подключения и схемы логического подключения уровня 3. Уровень 2 или схемы физического подключения - это сетевые диаграммы, представляющие подключение портов между устройствами в сети. По сути, это визуальное представление того, какой сетевой порт на сетевом устройстве подключается к какому сетевому порту на другом сетевом устройстве. Уровень 3 или схемы логической связи - это сетевые схемы, которые отображают IP-соединение между устройствами в сети.

### Сетевые протоколы

Telnet и SSH или Secure SHell используются для подключения к удаленному компьютеру и входа в эту систему с использованием учетных данных. Telnet сегодня менее распространен, поскольку SSH использует шифрование для защиты данных, проходящих через сетевое соединение, а безопасность данных является главным приоритетом. HTTP означает протокол передачи гипертекста, а HTTPS добавляет ключевое слово «Secure» в конец аббревиатуры. Этот протокол распознается в веб-браузерах как протокол, используемый для подключения к веб-сайтам. NETCONF действительно имеет стандартизованное значение порта, 830. RESTCONF не имеет зарезервированного значения порта, поэтому вы можете увидеть различные реализации разных значений.

Протокол динамической конфигурации хоста (DHCP) используется для передачи информации о конфигурации хостам в сети TCP/IP. DHCP выделяет IP-адреса тремя способами: автоматически, динамически и вручную. Операции DHCP включают четыре сообщения между клиентом и сервером: обнаружение сервера, предложение аренды IP, запрос аренды IP и подтверждение аренды IP.

Протокол DNS определяет автоматическую службу, которая сопоставляет имена ресурсов с требуемым числовым сетевым адресом. Он включает в себя формат запросов, ответов и данных. Связь по протоколу DNS использует единый формат, называемый сообщением DNS. DNS-сервер хранит различные типы записей ресурсов, которые используются для разрешения имен. Эти записи содержат имя, адрес и тип записи.

Система SNMP состоит из трех элементов:

* SNMP-менеджер: система управления сетью (NMS)
* Агенты SNMP (управляемый узел)
* База управленческой информации (MIB)

Существует два основных запроса диспетчера SNMP: получение и установка. Запрос на получение используется NMS для запроса данных на устройстве. NMS использует запрос набора для изменения переменных конфигурации в устройстве агента. Ловушки - это незапрошенные сообщения, предупреждающие диспетчер SNMP о состоянии или событии в сети. SNMPv1 и SNMPv2c используют строки сообщества, которые управляют доступом к MIB. Строки сообщества SNMP (включая только чтение и чтение-запись) аутентифицируют доступ к объектам MIB. Думайте о MIB как о «карте» всех компонентов устройства, которыми управляет SNMP.

NTP используется для распределения и синхронизации времени между распределенными серверами времени и клиентами. Авторитетный источник времени - это обычно радиочасы или атомные часы, подключенные к серверу времени. Серверы NTP могут связываться в нескольких режимах, включая: клиент/сервер, симметричный активный/пассивный и широковещательный.

Трансляция сетевых адресов (NAT) помогает решить проблему истощения адресов IPv4. NAT работает путем сопоставления тысяч частных внутренних адресов с диапазоном общедоступных адресов. Путем сопоставления внешних и внутренних IPv4-адресов NAT позволяет организации с неглобально маршрутизируемыми адресами подключаться к Интернету путем преобразования адресов в глобально-маршрутизируемое адресное пространство. NAT включает четыре типа адресов:

* Внутренний местный адрес
* Внутренний глобальный адрес
* За пределами местного адреса
* Внешний глобальный адрес

Типы NAT включают: статический NAT, динамический NAT и преобразование адресов портов (PAT).

### Устранение проблем с подключением приложений

Устранение неполадок сети обычно следует за уровнями OSI. Вы можете начать сверху вниз, начиная с прикладного уровня, и продвигаясь вниз к физическому уровню, вы можете идти снизу вверх. Если вы не можете найти никаких проблем с сетевым подключением ни на одном из уровней модели OSI, возможно, пришло время взглянуть на сервер приложений.

Обычно ifconfig используется следующим образом:

* Настройте IP-адрес и маску подсети для сетевых интерфейсов.
* Запросить статус сетевых интерфейсов.
* Включение/отключение сетевых интерфейсов.
* Измените MAC-адрес на сетевом интерфейсе Ethernet.

ping - это программная утилита, используемая для проверки доступности IP-сети для хостов и устройств, подключенных к определенной сети. Он также доступен практически во всех операционных системах и чрезвычайно полезен для устранения проблем с подключением. Утилита ping использует протокол управляющих сообщений Интернета (ICMP) для отправки пакетов на целевой хост, а затем ожидает эхо-ответов ICMP. На основе этого обмена пакетами ICMP пинг сообщает об ошибках, потере пакетов, времени приема-передачи, времени жизни (TTL) для полученных пакетов и т. д.

traceroute использует пакеты ICMP для определения пути к месту назначения. Поле Time to Live (TTL) в заголовке IP-пакета используется в первую очередь для предотвращения бесконечных петель в сети. Для каждого перехода или маршрутизатора, через который проходит IP-пакет, поле TTL уменьшается на единицу. Когда значение поля TTL достигает 0, пакет отбрасывается. Обычно поле TTL устанавливается на максимальное значение 255 на хосте, который является источником трафика, поскольку хост пытается максимизировать шансы того, что этот пакет попадет в пункт назначения. traceroute меняет эту логику и постепенно увеличивает значение TTL пакета, который он отправляет, с 1 и продолжает добавлять 1 к полю TTL для следующего пакета и так далее. Установка значения TTL 1 для первого пакета означает, что пакет будет отброшен на первом маршрутизаторе. По умолчанию,

nslookup - еще одна служебная программа командной строки, используемая для запроса DNS для получения сопоставления доменного имени и IP-адреса. Этот инструмент полезен для определения того, работает ли DNS-сервер, настроенный на конкретном хосте, должным образом и действительно ли преобразует имена хостов в IP-адреса. Возможно, DNS-сервер вообще не настроен на хосте, поэтому убедитесь, что вы проверили /etc/resolv.conf в UNIX-подобных операционных системах и что у вас есть хотя бы определенный сервер имен.

<!-- 5.7.2 -->
<!-- quiz -->
