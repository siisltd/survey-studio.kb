# Вопросы и ответы

## Можно ли запускать анкету с определённого вопроса, чтобы не заполнять её с начала во время программирования и тестирования? {: #Q0}

Добавьте в первый вопрос анкеты в скрипт перед показом первой строкой:

```js
return question(777);
```

В скобках необходимо указать номер вопроса, с которого должна начинаться анкета.

## Есть ли возможность на странице опроса выводить сразу несколько вопросов? {: #Q1}

Нет, пока возможно только отображение каждого вопроса на отдельном экране.

## Как заполнять анкету только с помощью клавиатуры? {: #Q2}

- В открытых вопросах курсор по умолчанию установлен в поле ввода, поэтому можно сразу вводить значение.
- В вопросах с выбором нужно вводить числовые коды необходимых ответов, с небольшой паузой между ними.
- В табличном вопросе с множественным выбором нужно нажимать клавишу *Tab*, чтобы перемещаться по ответам вперёд или *Shift+Tab* – назад. Выбрать ответ можно клавишей пробела.
- В табличном вопросе с единственным выбором клавишей *Tab* можно перемещаться по строкам вниз или *Shift+Tab* – вверх. Текущий ответ можно так же выбрать пробелом. Стрелками влево, вправо или вверх, вниз можно выбрать любой ответ в текущей строке.

Для перехода к следующему вопросу нужно нажать *Enter*. Если тип проекта *Ввод анкет*, то на экране *Ожидание*, а также после окончания анкеты, можно тоже нажимать *Enter*, чтобы идти дальше.

## После завершения интервью появляется сообщение об очистке ответов. Почему оно появляется и что с этим делать? {: #Q3}

Перед сохранением интервью в базу данных запускается процедура проверки ответов на соответствие заданной в анкете логике (условиям показа вопросов, переходам и так далее). Если обнаруживается ошибка – появляется сообщение с предупреждением и тремя вариантами решения проблемы для конкретного интервью.

Если во второй строке сообщения написано *«Обработка была остановлена на вопросе QXX из-за отсутствия ответа или ошибки»* – обычно это означает, что в указанном вопросе или рядом с ним не хватает ответа, но, согласно логике анкеты, он должен быть. Нужно внимательно проверить анкету и исправить ошибку логики. В некоторых скриптах необходимо учитывать повторный запуск анкеты перед сохранением интервью. Например, не делать чего-то, если функция [isPostProcessing()](help/2001.md#ispostprocessing) возвращает *true*.

Если вторая строка сообщения начинается словами *«Обработка завершилась корректно...»* – чаще всего это означает, что интервьюер во время заполнения анкеты возвращался к предыдущим вопросам и менял в них ответы. Это привело к переходу к другим вопросам анкеты. В результате часть ответов оказалась лишней. Поэтому в данном случае это не ошибка, а уведомление о лишних ответах. Но это сообщение может появляться и из-за ошибок в логике анкеты – её лучше проверить.

В любом случае, рекомендуем нажимать кнопку *Сохранить без очистки*, чтобы интервью сохранилось с максимально возможным количеством ответов, и проверять массив.

Если понимаете что делаете, то проверку интервью можно отключить в свойствах анкеты флагом *Запретить проверку с очисткой лишних ответов перед сохранением интервью*. В веб-опросах это лучше делать всегда, чтобы не пугать респондентов.

## При выгрузке массива появляется ошибка. Что делать? {: #Q4}

Если в строке с ошибкой справа нажать на кнопку с изображением конверта, то в открывшемся журнале выгрузки обычно можно найти сообщение о причине ошибки. О способах устранения ошибок можно почитать в разделе [Выгрузка результатов](help/3004.md#_4).

## В массиве отсутствуют интервью, превысившие квоту. Куда они пропали и как восстановить? {: #Q5}

По умолчанию интервью, превысившие квоту, нигде не сохраняются и их нельзя восстановить. Однако это поведение можно изменить, поставив [в свойствах проекта](help/3001.md#flags) флаг *Сохранять интервью при срабатывании квоты*. То же самое можно сделать [этой функцией](help/2001.md#enablesavewhenquotareached) в скрипте *Подготовка*. Не забудьте, что все выгружаемые интервью оплачиваются.

## Как удалить из массива лишнее интервью? {: #Q6}

Полностью удалить интервью из базы данных сервера невозможно. Но можно удалить ответ на вопрос, который используется в условии счётчика полных интервью. Таким образом, при запросе массива лишнее интервью выгружаться не будет.

Чтобы удалить ответ, перейдите в раздел [Редактирование ответов](help/3006.md) из левого меню проекта, укажите ID, для которого требуется изменить интервью, и нажмите *Запросить*. Из появившейся таблицы с ответами скопируйте ID ответа на вопрос, который нужно удалить, вставьте его в поле *ID из таблицы* и нажмите *Удалить*.

После изменения ответов нужно обязательно зайти в раздел *Счётчики и Квоты*, выбрать все счётчики и нажать кнопку *Пересчитать по базе*.

Краткое описание формы редактирования ответов находится в самой форме.

## Нашему подрядчику нужно дать полный доступ к одному проекту. Что нужно сделать, чтобы в личном кабинете он видел только свой проект? {: #Q7}

1. В разделе [Пользователи]({{ variables.ss_url }}/client/users) откройте раздел [Группы]({{ variables.ss_url }}/client/usergroups).
2. Нажмите *Добавить* и впишите название на своё усмотрение, например *Подрядчик*.
3. Перейдите в раздел [Права доступа]({{ variables.ss_url }}/client/accessrights), выберите право *Вход в личный кабинет* и пункт *Разрешить* напротив созданной группы.
4. Перейдите в раздел [Активные]({{ variables.ss_url }}/client/users), нажмите *Добавить* и укажите данные нового пользователя.
5. Перед сохранением, нажмите кнопку *Входит в группы*, снимите все флаги и выберите только созданную вами группу (*Подрядчик*).
6. Откройте свойства проекта, к которому необходимо дать доступ, и в блоке *Права доступа* во всех полях укажите название созданной группы (*Подрядчик*).

После этого пользователи созданной группы при входе в личный кабинет будут видеть только тот проект, в свойствах которого указано название их группы. Подробнее о правах доступа можно почитать [в соответствующем разделе](help/4000.md) Базы знаний. Читайте также статью [Делегирование прав подрядчику](articles/1011.md).

## Наши операторы обычно работают на нескольких проектах параллельно. Могут ли они где-нибудь брать рабочие ссылки самостоятельно? {: #Q8}

По адресу [do.survey-studio.com](https://do.survey-studio.com/) находится личный кабинет оператора, который по умолчанию доступен всем пользователям, входящим в группу *Операторы*.

По умолчанию в нём отображаются проекты, находящиеся в состоянии *Сбор данных*. Оператор может начать опрос, нажав в строке проекта на зелёную кнопку, либо заполнить тестовую анкету, нажав на белую.

В настройках [прав доступа]({{ variables.ss_url }}/client/accessrights) можно отключить операторам доступ в их кабинет. Для этого обычно достаточно удалить для соответствующей группы разрешение в праве *Доступ в личный кабинет оператора*.

Вместо запрета доступа можно разграничить права. Например, одной группе пользователей можно дать доступ к одним проектам, другой группе – к другим. Для этого можно создать новую группу пользователей, которой разрешён только доступ в личный кабинет оператора. В [свойствах проекта](help/3001.md#rights) в поле *Сбор данных* блока *Права доступа* нужно указать название созданной группы. Теперь все пользователи, входящие в эту группу, в личном кабинете оператора будут видеть только этот проект. Прописав эту группу в других проектах, можно так же разрешить ей сбор данных в эти проекты.

Подробнее о пользователях и правах доступа можно почитать [в соответствующем разделе](help/4000.md) Базы знаний.

## Во время работы у разных операторов периодически появляются сообщения *Подсистема проведения опроса не инициализирована* или *Отсутствует текущий вопрос*. Почему это происходит? {: #Q9}

Обычно такие сообщения появляются, когда оператор открывает одну анкету, затем, не закрывая её, открывает вторую, заполняет её, а потом пытается заполнить первую. Либо респондент открывает анкету, оставляет её открытой более, чем на 3 часа, а потом пытается продолжить заполнение.

Чтобы избежать таких проблем и не путаться в открытых вкладках, необходимо придерживаться простого правила: после завершения интервью вкладку или весь браузер нужно закрыть. Если интервью оборвалось (например, респондент отказался от участия в середине анкеты), то перед закрытием вкладки нужно нажать кнопку *Завершить* внизу слева, чтобы завершить анкету.

## Есть ли возможность сделать ссылку на опрос короткой и красивой? {: #Q11}

Для этого укажите в поле *Короткая ссылка* свойств проекта желаемое имя, например *the_best_survey*. После сохранения на странице проекта под рабочей ссылкой появится её короткий вариант.

Если в короткой ссылке необходимо использовать параметры, то их нужно указывать после вопросительного знака:

`sst.gl/the_best_survey?id=777&src=vk`

## Как к вопросу анкеты добавить аудио- или видеоматериал? {: #Q12}

Для этого необходимо выложить куда-нибудь файл, чтобы он был доступен по прямой ссылке, например на хостинг вашего сайта (Яндекс.Диск, Dropbox и другие подобные сервисы не подходят). В текст вопроса нужно добавить тег *audio* или *video* со ссылкой на ваш файл. Для этого нужно переключить редактор вопроса в режим исходного кода, нажав кнопку с изображением </> в правой части панели. Примеры:

```html
<audio src="http://example.com/audio.mp3" controls="controls"></audio>
<video src="http://example.com/video.mp4" controls="controls" style="margin-left: auto; margin-right: auto; display: block"></video>
```

Посмотреть примеры вопросов с аудио- и видеоматериалами можно [здесь](https://do.survey-studio.com/survey/start?qnid=2420).

## Можно ли изменить тип вопроса с единственного выбора на множественный (или наоборот) после начала сбора данных в проект? {: #Q13}

Да, ошибки при выгрузке не будет. Если меняется тип с единственного на множественный выбор, то ранее полученные ответы просто будут находиться в соответствующих столбцах массива. Если наоборот – в столбце останется только 1 ответ (с минимальным кодом), при этом из базы ответов ничего не удаляется (лишние ответы просто не будут добавлены в массив), поэтому тип вопроса можно изменить снова, и при очередной выгрузке ответы вернутся на место. Всё то же самое происходит и с табличными вопросами с выбором.

## Есть ли у вас пример анкеты, чтобы посмотреть как может проходить опрос? {: #Q14}

Да, вот ссылка: [https://do.survey-studio.com/survey?pkey=193afd1da7caec4ab92125b70468370e](https://do.survey-studio.com/survey?pkey=193afd1da7caec4ab92125b70468370e)

## Где можно взять ссылку на анкету или проект, которую просят прислать в тех. поддержке? {: #Q18}

Для быстрого поиска анкеты или проекта, с которыми возникли сложности, нужна любая ссылка, относящаяся к ним.

Ссылка на анкету находится в строке адреса вашего браузера, когда открыт редактор вопросов анкеты или когда она запущена в режиме тестирования. Ссылка на проект - так же в строке адреса, когда открыт проект или анкета запущена по рабочей ссылке.

Поиск по неточному описанию может ни к чему не привести и замедлить решение проблемы.

## Почему происходит превышение квоты? {: #Q19}

Допустим, проект уже в самом разгаре, по какой-то квоте сделано 10 интервью из 11 необходимых. 2 интервьюера (или респондента, если это онлайн-опрос) примерно в одно и то же время начинают новые интервью. Они проходят все квотные вопросы, и так получается, что респонденты попадают в эту же квоту. Система не знает, какие из этих 2-х интервью будут полными, может и ни одного – интервью продолжаются, ведь квота открыта. И так выходит, что оба респондента отвечают на все вопросы анкеты – сохраняются 12 интервью из 11 необходимых.

При опросе на планшетах дело осложняется тем, что интернет может быть выключен – приложение не может синхронизировать счётчики.

Другими словами, перебор получается при большом количестве одновременно работающих интервьюеров (респондентов) и высокой достижимости, а если опрос на планшете – ещё и из-за отсутствия доступа к серверу. Если бы на проекте работал один интервьюер или была бы низкая достижимость, то превышения квоты бы не было или оно было бы минимальным.

В некоторых проектах решить эту проблему можно, [поделив квоты](./help/3003.md#_3) между пользователями.

## При работе с внешней системой дозвона анкеты иногда открываются завершёнными. В чём причина? {: #Q20}

Одна из причин – операторы по ошибке ставят неверный статус, и контакт снова попадет в работу. Например, респондент отказался от участия, а оператор поставил перезвон.
Другая возможная причина – ошибка в логике анкеты. Например, в ней выбран ответ, который завершает интервью, но предполагается, что этот ответ автоматически удалится при повторном открытии анкеты для этого же контакта, а этого не происходит.
Начинать поиск проблемы лучше с просмотра сохранённых в интервью ответов. Это можно сделать через раздел [Просмотр интервью](help/3009.md) или [через DEX](help/9000.md#dex_3), если используется он.

## Как подключить систему дозвона? При открытии рабочей ссылки появляется сообщение *Отсутствуют необходимые параметры от внешней системы дозвона*. {: #Q21}

Какого-то специального подключения не требуется. Достаточно определённым образом открывать рабочую ссылку.

Если вы используете [DEX](https://siisltd.ru/dex), необходимо в меню *Режим обработки* свойств проекта DEX выбрать *Проект Survey-Studio (версия 2)*, а в поле *ПО обработки* указать рабочую ссылку. Все данные контакта будут передаваться автоматически.

Если хотите использовать другую систему дозвона, то она должна уметь открывать рабочую ссылку, когда респондент ответил на звонок. К ссылке необходимо добавлять, как минимум, номер телефона респондента, подставляя его к параметру *extPhone*. Например:

*https://do.survey-studio.com/survey?pkey=ef87c2da**&extPhone=79991112233***

Если система дозвона позволяет подставлять уникальные числовые идентификаторы контактов, то их можно передавать через параметр *extid*. Например:

*https://do.survey-studio.com/survey?pkey=ef87c2da&extPhone=79991112233**&extid=284751***

Это необходимо для корректной работы перезвонов, так как система загружает в анкету сохранённые ответы по ID. Если идентификатор не указан, то им является номер телефона. И если, например, в середине интервью респондент попросит перезвонить через час на другой телефон, то анкета откроется с самого начала, потому что изменится идентификатор (номер телефона). При указании в ссылке идентификатора контакта анкета будет открываться на том месте, на котором была закрыта, не зависимо от номера телефона.

Если на проекте будет работать несколько контакт-центров, необходимо учитывать, что идентификаторы контактов могут случайно совпадать – в результате могут открываться анкеты с чужими ответами. При использовании DEX эту проблему можно решить, указав в поле *ID контакт-центра* свойств проекта уникальные для каждого контакт-центра числа.

Через параметр *extData* можно передать в формате [JSON](https://ru.wikipedia.org/wiki/JSON), закодированном через urlencode, какие-либо данные контакта. Например, передать `{"Имя": "Иван", "Город": "Москва"}` можно так:

*https://do.survey-studio.com/survey?pkey=ef87c2da&extPhone=79991112233&extid=284751**&extData=%7B%22%D0%98%D0%BC%D1%8F%22%3A%20%22%D0%98%D0%B2%D0%B0%D0%BD%22%2C%20%22%D0%93%D0%BE%D1%80%D0%BE%D0%B4%22%3A%20%22%D0%9C%D0%BE%D1%81%D0%BA%D0%B2%D0%B0%22%7D***

Если система дозвона не умеет открывать ссылку при соединении с респондентом, можно поменять тип проекта с *Внешней системы дозвона* на *Ввод анкет*. При этом значения из параметров *extid*, *extPhone* и *extData* перестанут приниматься, а при перезвонах анкета будет открываться с начала. Если в массиве нужны телефоны, по которым звонили, то в анкету нужно добавить вопрос для вписывания номеров вручную.

## Как сформулировать обращение в поддержку, чтобы сократить время решения вопроса? {: #Q22}

Необходимо подробно описать проблему и задать вопрос, на который нужен ответ. Наиболее полное описание позволит нам предоставить ответ в кратчайшие сроки без уточнения дополнительной информации.

Вот вопросы, ответив на которые получится хороший запрос в поддержку:

1. Какой идентификатор в системе?
    - Клиент, если сложность возникла с **SURVEY**STUDIO,
    - ссылка на панель администрирования, если с DEX,
    - а также логин пользователя, если вход осуществлялся не под администратором (не под учётной записью, созданной при регистрации).
2. В чём суть проблемы?
3. Какие действия нужно совершить, чтобы увидеть её?
    - Какую [ссылку](#Q18) или какой раздел сайта нужно открыть?
    - Что именно нужно нажать или выбрать?
4. Как ведёт себя система при выполнении этих действий? Здесь может быть полезен скриншот, на котором хорошо виден весь экран с проблемой (можно прочитать любой текст, увидеть адрес страницы, если что-то не так с сайтом).
5. Как, по вашим ожиданиям, должна вести себя система при выполнении этих действий?
6. Есть ли дополнительная информация, которая поможет решить проблему? Например, это могут быть:
    - какие-либо важные комментарии,
    - ссылка на статью из базы знаний, если делали что-то по ней и это не работает,
    - [журналы](./help/9000.md#_1) рабочего места DEX, если проблема с ним.

Если к письму прикрепляется файл, то его размер не должен превышать 7 Мб. Файл большего размера необходимо выложить в какое-либо облако и указать ссылку для скачивания.

Каждый новый запрос *необходимо* отправлять новым письмом, а не ответом на последнее, иначе может получиться путаница или большая задержка с ответом. Новое письмо создаёт в нашей системе новую заявку, а при ответе она дополняется. И если первый вопрос был, например, про телефонию, а потом тут же про программирование анкеты, то, во-первых, вопрос попадёт не к тому специалисту (и он может быть выходной), а во-вторых, при решении разных задач в одной заявке, особенно разными специалистами, можно легко запутаться и что-нибудь пропустить. Поэтому лучше придерживаться простого правила: *один вопрос - одна заявка*.

Создавать и просматривать свои заявки можно в [личном кабинете Zendesk](https://zendesk.siisltd.ru/hc/ru/signin?return_to=https%3A%2F%2Fzendesk.siisltd.ru%2Fhc%2Fru).

## Чем ротация отличается от рандомизации (в контексте социологических опросов)? {: #Q23}

Ротация - последовательный сдвиг на один шаг вопросов или ответов для каждого следующего интервью. Например, если в первом интервью варианты ответа были 1,2,3,4, то во втором будут 2,3,4,1, в третьем - 3,4,1,2 и так далее.

Рандомизация - перемешивание вопросов или ответов случайным образом.

В **SURVEY**STUDIO можно включить как ротацию, так и рандомизацию. Использование ротации было оправдано в бумажных анкетах, так как обеспечить рандомизацию в полевых условиях человек просто не может.
