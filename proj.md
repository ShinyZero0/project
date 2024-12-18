# Системы контроля версий

https://habr.com/ru/amp/post/552872/

Система контроля версий является прежде всего инструментом, а инструмент призван решать некоторый класс задач.
Итак, система контроля версий – это система, записывающая изменения в файл или набор файлов в течение времени и позволяющая вернуться позже к определенной версии.
Мы хотим гибко управлять некоторым набором файлов, откатываться до определенных версий в случае необходимости.
Можно отменить те или иные изменения файла, откатить его удаление, посмотреть кто что-то поменял.
Как правило системы контроля версий применяются для хранения исходного кода, но это необязательно.
Они могут применяться для хранения файлов совершенно любого типа.

Как хранить различные версии файлов?
Люди пришли к такому инструменту как системы контроля версий не сразу, да и они сами бывают очень разные.
Предложенную задачу можно решить с применением старого доброго copy-paste, локальных, централизованных или распределенных систем контроля версий.

## Copy-paste

Известный метод при применении к данной задаче может выглядеть следующим образом: будем называть файлы по шаблону filename_{version}, возможно с добавлением времени создания или изменения.

Данный способ является очень простым, но он подвержен различным ошибкам: можно случайно изменить не тот файл, можно скопировать не из той директории (ведь именно так переносятся файлы в этой модели).

## Локальная система контроля версий

Следующим шагом в развитии систем контроля версий было создание локальных систем контроля версий.
Они представляли из себя простейшую базу данных, которая хранит записи обо всех изменениях в файлах.

Одним из примеров таких систем является система контроля версий RCS, которая была разработана в 1985 году (последний патч был написан в 2015 году) и хранит изменений в файлах (патчи), осуществляя контроль версий.
Набор этих изменений позволяет восстановить любое состояние файла.
RCS поставляется с Linux'ом.

Локальная система контроля версий хорошо решает поставленную перед ней задачу, однако ее проблемой является основное свойство — локальность.
Она совершенно не преднезначена для коллективного использования.

## Централизованная система контроля версий

Централизованная система контроля версий предназначена для решения основной проблемы локальной системы контроля версий.

Для организации такой системы контроля версий используется единственный сервер, который содержит все версии файлов.
Клиенты, обращаясь к этому серверу, получают из этого централизованного хранилища.
Применение централизованных систем контроля версий на протяжении многих лет являлась стандартом.
К ним относятся CVS, Subversion, Perforce.

Такими системами легко управлять из-за наличия единственного сервера.
Но при этом наличие централизованного сервера приводит к возникновению единой точки отказа в виде этого самого сервера.
В случае отключения этого сервера разработчики не смогут выкачивать файлы.
Самым худшим сценарием является физическое уничтожение сервера (или вылет жесткого диска), он приводит к потерю кодовой базы.

Несмотря на то, что мода на SVN прошла, иногда наблюдается обратный ход — переход от Git'а к SVN'у.
Дело в том, что SVN позволяет осуществлять селективный чекаут, который подразумевает выкачку лишь некоторых файлов с сервера.
Такой подход приобретает популярность при использовании монорепозиториях, о которых можно будет поговорить позже.

## Распределенная система контроля версий

Для устранения единой точки отказа используются распределенные системы контроля версий.
Они подразумевают, что клиент выкачает себе весь репозиторий целиком заместо выкачки конкретных интересующих клиента файлов.
Если умрет любая копия репозитория, то это не приведет к потере кодовой базы, поскольку она может быть восстановлена с компьютера любого разработчика.
Каждая копия является полным бэкапом данных.

Все копии являются равноправными и могут синхронизироваться между собой.
Подобный подход очень напоминает (да и является) репликацией вида master-master.

К данному виду систем контроля версий относятся Mercurial, Bazaar, Darcs и Git.
Последняя система контроля версий и будет рассмотрена нами далее более детально.

## История Git

В 2005 году компания, разрабатывающая систему контроля версий BitKeeper, порвала отношения с сообществом разработчиков ядра Linux.
После этого сообщество приняло решение о разработке своей собственной системы контроля версий.
Основными ценностями новой системы стали: полная децентрализация, скорость, простая архитектура, хорошая поддержка нелинейной разработки.
