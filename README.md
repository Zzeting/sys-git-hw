# Домашнее задание к занятию "`Базы данных, их типы`" - `Мамонтов Александр`


### Задание 1. СУБД

# Кейс

Крупная строительная компания, которая также занимается проектированием и девелопментом, решила создать правильную архитектуру для работы с данными. Ниже представлены задачи, которые необходимо решить для каждой предметной области.

Какие типы СУБД, на ваш взгляд, лучше всего подойдут для решения этих задач и почему?

1.1. Бюджетирование проектов с дальнейшим формированием финансовых аналитических отчётов и прогнозирования рисков. СУБД должна гарантировать целостность и чёткую структуру данных. 

Для подобной задачи необходима СУБД, которая сможет обеспечить целостность и четкую структуру. Тем самым, обиспечить формирование финансовых и аналитических отчетов на малых объемах данных смогут `PosgreSQL`, `MySQL`

1.1.* Хеширование стало занимать длительно время, какое API можно использовать для ускорения работы?

`pg_hint_plan`, данная опция ускоряет hash joins в `PostgreSQL`, либо же имеет смысл для определенных данных интегрировать `Memcached`

1.2. Под каждый девелоперский проект создаётся отдельный лендинг, и все данные по лидам стекаются в CRM к маркетологам и менеджерам по продажам. Какой тип СУБД лучше использовать для лендингов и для CRM? СУБД должны быть гибкими и быстрыми.

Для подобных задач подойдет использование как реляционных БД так и нереляционных. Так как нет необходимости хранить много структурированных данных, можно использовать такие СУБД как `InfluxDB`, `Kdb+`, `Prometheus`

1.2.* Можно ли эту задачу закрыть одной СУБД? И если да, то какой именно СУБД и какой реализацией?

Можно данную задачу закрыть полностью `PostgreSQL`, использовав расширение `TimescaleDB`

1.3. Отдел контроля качества решил создать базу по корпоративным нормам и правилам, обучающему материалу и так далее, сформированную согласно структуре компании. СУБД должна иметь простую и понятную структуру.

В этом случае подойдет графовая СУБД `Neo4J`. Использование данной СУБД позволит граммотно распередлить обучающий материал

1.4. Департамент логистики нуждается в решении задач по быстрому формированию маршрутов доставки материалов по объектам и распределению курьеров по маршрутам с доставкой документов. СУБД должна уметь быстро работать со связями.

`PostgreSQL` сможет быстро работать со связями, также к ней есть возможность подключения `PostGIS` для работы с построением маршрутов

1.4.* Можно ли к этой СУБД подключить отдел закупок или для них лучше сформировать свою СУБД в связке с СУБД логистов?
1.5.* Можно ли все перечисленные выше задачи решить, используя одну СУБД? Если да, то какую именно?

Отвечая сразу на два вопроса, можно все перечисленные задачи решить используя одну СУБД, к примеру `PostgreSQL`, `MySQL`. Данные СУБД позволяют дополнительно устанавливать расширения, для увелечения своего функционала. Однако даже при использовании одной СУБД следует разделять сами БД на кластеры по логике и требуемым задачам


### Задание 2

# Транзакции

2.1. Пользователь пополняет баланс счёта телефона, распишите пошагово, какие действия должны произойти для того, чтобы транзакция завершилась успешно. Ориентируйтесь на шесть действий.

- пользователь в личном кабинете оператора связи вводит сумму пополнения баланса телефона, после чего указывает данные банковской карты
- на основе данных, считанных с карты, формируется запрос, а будующей транзацкии присваивается номер
- запрос поступает в процессинговый центр, который принадлежит банку-эммитенту. Заявка обрабатывается и поступает непсредственно эмитенту
- далее происходит сверка полученных данных с базой, подтверждается транзацкионность, и доступ к счету разрешается или запрещается.
- банк получает подтверждение на сделку, и при помощи платежной системы налаживает канал для перевода средств. 
- транзакция завершена


### Задание 3

# SQL vs NoSQL

3.1. Напишите пять преимуществ SQL-систем по отношению к NoSQL.:
SQL и NoSQL две популярные модели данных, которые используют для решения различного рода
задач, однако отвечая на поставленный вопрос можно выделить следующие преимущества SQL:
- структурированность
- лучшая поддержка транзакционности, что обеспечивает ACID свойства для отказоустойчивости
и надёжности базы данных
- можно легко установить ограничения на доступ к данным для разных пользователей, а также
применять различные аутентификационные механизмы для обеспечения безопасности данных
- в связи с жесткой структурой построения данных, также обеспечивается целостность данных.
- большая поддержка сообщества


### Задание 4

# Кластеры

Необходимо производить большое количество вычислений при работе с огромным количеством данных, под эту задачу выделено 1000 машин.
На основе какого критерия будете выбирать тип СУБД и какая

Анализируя постановку задачи, можно выделить такие критерии выбора СУДБ:
- Нужно обрабатывать большое количество данных
- Сложная логика производимых вычеслений
- Необходимо частое обновление
- Нужно обеспечить минимальное время сбора отчета.

Для решения подобных задач подойдет `GreenPlum`, так как это мощная СУБД, а для работы с хранящимся в ней данными используется SQL. Поскольку нам необходимо выполнять сложные вычесления, будет удобно использовать SQL, для создания сложных запросов к БД.
