[EXIT](./readme.md)
# 3.4 Ветвление в Git - Работа с ветками

Теперь, когда вы познакомились с основами ветвления и слияния, возникает вопрос: что можно или нужно делать с этим? В этом разделе мы разберём некоторые основные рабочие процессы, ставшие возможными благодаря облегчённой процедуре ветвления, которые вы возможно захотите применить в собственном цикле разработки.

***Долгоживущие ветки***
Так как в Git применяется простое трёхстороннее слияние, ничто не мешает многократно объединять ветки в течение длительного времени. Это значит, что у вас может быть несколько постоянно открытых веток, которые вы используете для разных этапов вашего цикла разработки; вы можете регулярно сливать изменения из одной ветки в другую.

Многие разработчики, использующие Git, придерживаются именно такого подхода, оставляя полностью стабильный код только в ветке `master` — возможно, только тот код, который был или будет выпущен. При этом существует и параллельная ветка с именем `develop` или `next`, предназначенная для работы и тестирования стабильности; она не обязательно должна быть всегда стабильной, но при достижении стабильного состояния ее содержимое можно слить в ветку `master`. Она используется для слияния завершённых задач из тематических веток (временных веток наподобие `iss53)`, чтобы гарантировать, что эти задачи проходят тестирование и не вносят ошибок.

По сути, мы говорим про указатели, перемещающиеся по линии создаваемых вами коммитов. Стабильные ветки находятся в нижнем конце истории коммитов, а самые свежие наработки — ближе к её верхней части
<![Долгоживущие ветки](./img/lr-branches-1.png)>
_Рисунок 24. Линейное представление повышения стабильности веток_
В общем случае это можно представить в виде накопителей, в которых наборы коммитов перемещаются на более стабильный уровень только после полного тестирования.

<![lr-branches-2](./img/lr-branches-2.png)>
_Рисунок 25. Представление диаграммы стабильности веток в виде многоуровневого накопителя_

Число уровней стабильности можно увеличить. В крупных проектах зачастую появляется ветка `proposed` или `pu` (предлагаемые обновления), объединяющая ветки с содержимым, которое ещё не готово к включению в ветки next или master. Идея состоит в том, что каждая ветка представляет собой определённый уровень стабильности; как только он повышается, содержимое сливается в ветку уровнем выше. Разумеется, можно вообще обойтись без долгоживущих веток, но зачастую они имеют смысл, особенно при работе над большими и сложными проектами.

***Тематические ветки***
А вот такая вещь, как тематические ветки, полезна вне зависимости от величины проекта. Тематической веткой называется временная ветка, создаваемая и используемая для работы над конкретной функциональной возможностью или решения сопутствующих задач. Скорее всего, при работе с другими СКВ вы никогда ничего подобного не делали, так как там создание и слияние веток — затратные операции. Но в Git это обычное дело — много раз в день создавать ветки, работать с ними, сливать их и удалять.

Пример тематических веток вы видели в предыдущем разделе, когда мы создавали ветки `iss53` и `hotfix`. В каждой из них было создано несколько коммитов, после чего, сразу же после слияния с основной веткой, они были удалены. Такая техника позволяет быстро и полностью осуществлять переключения контекста — так как работа разделена по уровням и все изменения в конкретной ветке относятся к определённой теме, что позволяет легко увидеть что именно было сделано во время процедуры просмотра кода или аналогичной. Ветки с внесёнными в них изменениями можно хранить минуты, дни или даже месяцы, а слияние выполнить только когда это действительно потребуется, вне зависимости от порядка их создания.

Предположим, мы работаем в ветке `master`, ответвляемся для решения попутной проблемы (`iss91`), некоторое время занимаемся ею, затем создаём ветку, чтобы попробовать решить эту задачу другим способом (`iss91v2`), возвращаемся в ветку `master` и выполняем там некие действия, затем создаём новую ветку для изменений, в результате которых не уверены (ветка `dumbidea`). Результирующая история коммитов будет выглядеть примерно так:

<![topic-branches-1](./img/topic-branches-1.png)>
_Рисунок 26. Набор тематических веток_

Предположим, вам больше нравится второй вариант решения задачи (`iss91v2`), а ветку `dumbidea` вы показали коллегам, и оказалось, что там содержится гениальная идея. Фактически вы можете удалить ветку `iss91` (потеряв коммиты C5 и C6) и слить две другие ветки. После этого история будет выглядеть так:

<![topic-branches-2](./img/topic-branches-2.png)>
_Рисунок 27. История после слияния веток `dumbidea` и `iss91v2`_

Важно помнить, что во время всех этих манипуляций ветки полностью локальны. Ветвления и слияния выполняются только в вашем Git репозитории — связь с сервером не требуется.
