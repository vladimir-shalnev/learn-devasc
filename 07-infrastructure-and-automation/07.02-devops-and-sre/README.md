<!-- 7.2.1 -->
## Введение в DevOps и SRE

Чтобы полная автоматизация была действительно эффективной, необходимы изменения в организационной культуре, в том числе устранение исторического разрыва между разработкой (Dev) и операциями (Ops).

В этом разделе обсуждается история подхода DevOps и основные принципы успешных организаций DevOps.

<!-- 7.2.2 -->
## DevOps Divide

Исторически создание приложений было задачей разработчиков программного обеспечения (Dev), а обеспечение того, чтобы приложения работали для пользователей и бизнеса, было специализированной областью ИТ-операций (Ops).

| **Характерная черта**         | **Dev**                                                                                                  | **Операции**                                                                                                                                                                         |
| ----------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Заботится о:                  | Сделанные на заказ приложения и как они работают                                                         | Приложения и то, как они работают, а также инфраструктура (исторически - оборудование), ОС, сеть, стандартные услуги, доступность, безопасность, масштабирование и обслуживание      |
| Бизнес рассматривает как:     | Центр прибыли: требует ресурсов                                                                          | Центр затрат: предоставляет ресурсы и учитывает их                                                                                                                                   |
| Участвует в дежурной ротации: | Иногда (и его беспокоят только тогда, когда проблема передается разработчикам)                           | Регулярно (острие копья)                                                                                                                                                             |
| Производительность измерена:  | Абстрактно (включая плохие показатели, например, количество строк кода в день)                           | Конкретно (соблюдение SLA, проблемы решены)                                                                                                                                          |
| Требуются навыки:             | Более глубокие, чем общие: языки, API, архитектура, инструменты, процессы, «интерфейс», «бэкэнд» и т. д. | Более широкий, чем глубокий: конфигурация, администрирование, ОС, специфические для производителя интерфейсы командной строки/API, оболочка, автоматизация, БД, безопасность и т. д. |
| Требуется ловкость:           | Много! Двигайтесь быстро, вводите новшества, ломайте вещи, исправляйте позже.                            | Не так много! Инвестиции должны быть полностью оправданы, ожидания - оправданы. Это нормально. Безопаснее сказать «нет», чем «да».                                                   |

В традиционной корпоративной ИТ-экосистеме, предшествующей виртуализации, разделение Dev и Ops казалось разумным. Поскольку инфраструктура основывалась на инвестициях в капитальное оборудование (вручную устанавливаемое, настраиваемое и управляемое оборудование), требовались привратники. Привратники Ops работали в иной временной шкале, чем пользователи и разработчики, и имели другие стимулы и проблемы.

### Унаследованные узкие места

Традиционно выделение ресурсов для проекта или масштабирование системы будет определяться планом, а не спросом. Запрос, покупка, установка, подготовка и настройка серверов или емкости сети и услуг для проекта могут занять месяцы. При ограниченных физических ресурсах совместное использование ресурсов было обычным делом.

Отсутствие простых способов создания и разрушения изолированных сред и подключений означало, что организации были склонны создавать системы с длительным сроком службы, которые становились единственными точками отказа. Многофункциональные сети было сложно защитить и требовало постоянного тщательного управления.

В просторечии такие давно работающие и сложные системы назывались «домашними животными», потому что люди давали им имена и заботились об этой единственной системе. Альтернативой является «крупный рогатый скот», который представляет собой урезанные, эфемерные рабочие нагрузки и виртуальную инфраструктуру, которые создаются и разрушаются с помощью автоматизации. Этот метод обеспечивает доступность новой системы (или «коровы») для выполнения работы.

### Слияние Dev и Ops

Сложные технологические организации уходили от этих исторических крайностей на протяжении поколений. Этот процесс ускорился с повсеместным внедрением виртуализации серверов, облачных вычислений и гибкой разработки программного обеспечения. В начале 2000-х появилось движение за то, чтобы рассматривать Dev и Ops как единое целое:

* Сделайте кодировщиков ответственными за развертывание и обслуживание
* Рассматривайте виртуализированную инфраструктуру как код

<!-- 7.2.3 -->
## Развитие DevOps

DevOps развивалась и продолжает развиваться во многих местах параллельно. Некоторые ключевые события сформировали дисциплину, которую мы знаем сегодня.

### Определяющие моменты 1. Инжиниринг надежности площадки (SRE)

К 2003 году крупнейшие и самые передовые интернет-компании мира в значительной степени перешли на виртуализацию. Они имели дело с крупными центрами обработки данных и приложениями, работающими в массовом масштабе. Были неудачи, которые привели к тому, что Dev vs. Ops указали пальцем, быстро растущим и постоянно недостаточным количеством сотрудников в организации, а также к стрессу на работе.

Google была одной из первых компаний, которые поняли и внедрили новый вид гибридных должностных инструкций Dev + Ops. Это был инженер по надежности сайта (SRE). Роль SRE предназначена для объединения дисциплин и навыков разработчиков и операторов, создавая новую специальность и учебник лучших практик для выполнения операций с помощью программных методов.

Подход SRE был принят многими другими компаниями. Этот подход основан на:

* Общая ответственность
* Принятие риска
* Признание отказа как нормальное
* Обязательство использовать автоматизацию для сокращения или устранения «тяжелого труда»
* Измерение всего
* Квалифицированный успех с точки зрения достижения количественных целей уровня обслуживания

### Определяющие моменты 2: Дебуа и «гибкая инфраструктура»

На Agile 2008 бельгийский разработчик Патрик Дебуа выступил с презентацией под названием Agile Infrastructure and Operations. В его презентации обсуждалось, как применять методы разработчика к Ops, сохраняя при этом, что Dev и Ops должны оставаться отдельными. Тем не менее, в следующем году Дебуа основал серию мероприятий DevOpsDays.

Презентация Дебуа сыграла важную роль в продвижении дискуссий об автоматизации виртуальной (и физической) инфраструктуры, использовании контроля версий (например, Git) для хранения кода развертывания инфраструктуры (процедурного или декларативного) и применении гибких методов для разработки и обслуживания решений на уровне инфраструктуры. .

### Определяющие моменты 3: Олспоу и Хаммонд

К концу 2000-х годов популярность DevOps росла. Одной из первых убедительных иллюстраций его ценности была презентация Джона Олспоу и Пола Хаммонда на VelocityConf в 2009 году. В этой презентации авторы обрисовывают простой набор лучших практик DevOps, основанных на идее о том, что и DevOps, и Ops совместно обеспечивают бизнес. Эти передовые методы включают:

* Автоматизированная инфраструктура
* Общий контроль версий
* Пошаговые сборки и развертывания

В презентации также описываются методы автоматизации, командной работы, разделения ответственности, прозрачности, доверия, взаимной ответственности и коммуникаций, которые с тех пор стали обычным явлением среди практиков DevOps/SRE. Эти методы включают CI/CD, автоматическое тестирование, метрики, SLO/SLA и безупречное принятие риска и неизбежного отказа.

<!-- 7.2.4 -->
## Основные принципы DevOps

DevOps/SRE имеют множество основных принципов и лучших практик:

* Акцент на автоматизацию
* Идея о том, что «неудача - это нормально»
* Переосмысление «доступности» с точки зрения того, что может терпеть бизнес.

Подобно тому, как гибкую разработку можно рассматривать как метод определения и контроля ожиданий руководства в отношении программных проектов, DevOps можно рассматривать как способ структурирования здоровой рабочей культуры для технологических частей бизнеса. Это также можно рассматривать как способ снижения затрат. Сделав отказ нормальным явлением и автоматизируя устранение последствий, можно перейти от дорогостоящих и напряженных часов работы по вызову к запланированному графику рабочего дня.

### Автоматизация, отказ от тяжелого труда и сохранение талантов

Автоматизация обеспечивает скорость и повторяемость, устраняя при этом повторяющийся ручной труд. Это позволяет техническим специалистам тратить время на решение новых проблем, повышая ценность затраченного времени для бизнеса.

Часто ожидается, что специалисты-практики DevOps/SRE будут посвящать значительную часть рабочего времени (50% или более в некоторых сценариях) предоставлению новых операционных возможностей и проектированию надежного масштабирования, включая разработку инструментов автоматизации. Это сокращает время, затрачиваемое на вызовы и вмешательство вручную, чтобы понять и исправить проблемы.

Приобретение и удержание технических талантов требует от организаций сотрудничества со своими новаторами, чтобы свести к минимуму скуку и стресс от малоценного труда и работы по вызову, а также риск столкнуться с неизбежными технологическими сбоями в режиме «пожарной тренировки».

Это особенно важно для предприятий с изначально низкой рентабельностью, которые получают прибыль за счет быстрого роста и крупных масштабов и нуждаются в строгом контроле расходов, особенно в отношении квалифицированного персонала. Однако для всех видов организаций важно, чтобы работа в области информационных технологий воспринималась как центр прибыли, а не как центр затрат.

### Отказ - это нормально

Предположение, что сбои произойдут, действительно влияет на методологию проектирования программного обеспечения. DevOps должен создавать продукты и платформы с большей отказоустойчивостью, большей устойчивостью к задержкам, где это возможно, лучшим самоконтролем, ведением журнала, сообщениями об ошибках конечным пользователям и самовосстановлением.

Когда случаются сбои и необходимо вмешательство DevOps, результирующие действия следует рассматривать не просто как ремонтные работы, а как исследования для выявления и ранжирования процедурных кандидатов для новых этапов автоматизации.

### SLO, SLI и бюджеты ошибок

Критически важными для культуры DevOps/SRE являются две взаимосвязанные идеи: 1. DevOps должен приносить измеримую, согласованную ценность для бизнеса, И 2. Статистическая реальность, чтобы сделать это идеально, невозможно. Эти идеи кодифицированы в цели уровня обслуживания (SLO), которая определяется в терминах реальных показателей, называемых индикаторами уровня обслуживания (SLI).

SLI спроектированы так, чтобы соответствовать практической реальности предоставления услуги клиентам: они могут представлять собой единый порог или обеспечивать более сложный брекетинг для дальнейшей классификации резко отклоняющихся результатов. Например, SLI может указывать, что 99% запросов будут обслуживаться в течение 50 миллисекунд, а также может потребовать сбора информации, например о том, завершен ли вообще один запрос> 50 мсек или не удалось ли выполнить конкретный запрос для вашего крупнейшего клиента.

Методология SLO/SLI позволяет более дешево и быстро обеспечивать ценность бизнеса, устраняя обязательство стремиться к совершенству в пользу создания «достаточно хорошего». Это также может повлиять на темп, объем и другие аспекты разработки, чтобы обеспечить и улучшить адекватность.

Один из способов моделирования результатов SLO/SLI требует установления так называемого «бюджета ошибок» для услуги за определенный период времени (день, неделя, месяц, квартал и т. Д.), А затем вычитания неудач для достижения SLO из этого значения. . Если бюджет ошибок превышен, можно принять разумные решения, например, замедлить темпы выпуска выпусков до тех пор, пока не будут определены источники ошибок, а также внесены и протестированы конкретные исправления.

На рисунке изображена горизонтальная полоса, три четверти которой голубые, а одна четверть - золотая. Примерно на две трети ниже синего участка находится стрелка с надписью sl a. Стрелка slo находится в начале золотого сечения, а бюджет ошибок слов - под золотым сечением.

### SLA против SLO и бюджета ошибок

![](./assets/7.2.4.png)
<!-- 7. /courses/devnet/1035e850-b0bc-11ea-a04f-618a6d0a372e/10404890-b0bc-11ea-a04f-618a6d0a372e/assets/ddc90312-c0c5-11ea-a947-f5323e7496e3.svg -->

Для данной услуги имеет смысл взять на себя обязательство выполнять поставку в пределах возможностей, но затем перевыполнять ее. SLA (внешние соглашения) лучше всего устанавливать там, где их будет легко выполнить. SLO (целевые показатели фактической производительности) можно установить выше. Бюджет ошибок - это разница между SLO и 100% доступностью.

<!-- 7.2.5 -->
## DevOps и SRE Summary

Вы видели, как DevOps/SRE развивается вместе с такими технологиями, как виртуализация и контейнеризация, обеспечивая единый подход и унифицированный набор инструментов для поддержки скоординированного проектирования приложений и инфраструктуры.

Далее вы узнаете о некоторых механизмах автоматизации инфраструктуры.
