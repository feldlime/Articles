RFM-анализ одной кнопкой или как мы облегчили клиентам жизнь
====================

<a href="">
<img align="left" alt="image" width=400 src="https://habrastorage.org/webt/rn/wo/mv/rnwomvuqtlukaj3mrs2ghlna8zq.png">
</a>

С тех пор как в компании Mindbox впервые произнесли *Machine Learning*, общей целью стала *Большая Зеленая Кнопка*. Это такая кнопка во весь экран, при нажатии на которую всё работает само и приносит прибыль.

В аналитическом проекте «RFM» цель менее амбициозная — *Маленькая зеленая кнопка*. Нажимаешь, и база автоматически делится на сегменты, по которым запускается отправка писем (например).

<br> <br clear="left"/>

Чтобы добиться цели, мы написали автоматический RFM-сегментатор и разработали специальный отчет, чтобы наглядно представлять результаты.

Рассказываем, как это все случилось и почему теперь можно ~~обойтись без аналитиков~~ уделять больше времени менее тривиальным задачам .

<cut/>

# Что такое RFM-анализ
Результат email-рассылки зависит от охвата аудитории и качества самой рассылки. Бесконечно увеличивать охват нельзя, а значит, нужно увеличивать качество. Для этого рассылку нужно персонализировать, так как все люди разные и каждому нужно что-то свое.

<img alt="image"  src="https://habrastorage.org/webt/2j/9k/ak/2j9kakz6ohehj_7vs0lortndwsw.jpeg">
<br> 

Потребителей обычно много, сделать индивидуальное письмо под каждого сложно. Чтобы справиться с проблемой, маркетологи делят потребителей на группы — сегменты.

Делить можно по-разному. Один из вариантов — [RFM-анализ](https://ru.wikipedia.org/wiki/RFM-%D0%B0%D0%BD%D0%B0%D0%BB%D0%B8%D0%B7).

То есть RFM-анализ  — это способ сегментации. Сегментами называются непересекающиеся группы потребителей. RFM-анализ предлагает для каждого покупателя выделить три признака:

* R (Recency) — насколько давно клиент сделал последний заказ.
* F (Frequency) — сколько всего заказов сделал клиент.
* M (Monetary) — сколько денег клиент потратил.

Многие маркетинговые компании делают и используют RFM-анализ. Мы в том числе. [В статье про RFM-сегментацию](https://mindbox.ru/blog/marketing/rfm-raspredelenie/) рассказали, какой отчет умеем делать, и как он может помочь маркетологам.

# Существующие подходы к RFM-анализу

Существующие подходы к RFM-анализу у всех примерно схожи.

<img alt="image" width=400 align="left" src="https://habrastorage.org/webt/4l/op/yi/4lopyik0yzn8jufa1eypt0h_wsc.png">

Клиентов делят на группы по каждому признаку. Обычно таких групп не больше пяти. Пересечения групп называют сегментами.

Например, при делении на четыре группы по каждому из трех признаков образуется 64 (4x4x4) сегмента потребителей, а на пять — уже 125 сегментов.

Основная сложность — определить границы групп, потому что нет определенного правила, как это делать.

<br> <br clear="left"/>

Рассмотрим наиболее популярные подходы на примере одной базы клиентов:

<img alt="image" src="https://habrastorage.org/webt/c5/zq/5d/c5zq5dulgeig4spnojqq7ub3r-o.png">
<br>

Здесь мы используем только два измерения (R и M) из трех для удобства восприятия.

В нашем примере:

* Сумма покупок лежит в диапазоне от 0 до 15 тысяч рублей.
* Давность покупки лежит в диапазоне от 1 часа до 240 дней.

### Подход 1. Разделение на равные части по диапазонам значений
При этом подходе разделение делается исходя из из значений признаков. В нашем случае выделяем три группы по тратам: до 5 тысяч рублей, от 5 до 10 тысяч и от 10 тысяч. И три группы по давности срока покупки: до 80 дней, от 80 до 160 дней, от 160 дней.

Получаем девять сегментов:

<img  alt="image" src="https://habrastorage.org/webt/di/x-/mx/dix-mxjzjvqvtvyihz6ynghs3x4.png">
<br> 

Плюсы метода:

* Легко автоматизировать.
* Можно выявить «самых-самых»: покупающих больше всех, чаще всех и не покупавших дольше всех.

Минусы метода:

* Распределение по группам неравномерное: в примере 86% потребителей в одном сегменте, 13% — во втором, 1% распределился по оставшимся семи сегментам.
* Количество групп по каждому признаку одинаковое.
* Много сегментов (помним, что даже при разделении на 3 части по каждому признаку, сегментов будет 27).

### Подход 2. Разделение на равные части по количеству потребителей
При таком подходе разделение по каждому признаку выполняется так, чтобы в группы попадало одинаковое количество потребителей.
Вот так распределяются покупатели из нашего примера (по-прежнему делим на три части по каждому признаку):

<img alt="image" src="https://habrastorage.org/webt/uk/ql/yv/ukqlyvy8thmp451bep5cq-qywks.png">
<br> 

Плюсы метода:

* Легко автоматизировать.
* Обычно, нет сильного дисбаланса между группами.

Минусы метода:

* Плохо выделяются «особые» клиенты.
В примере в одном сегменте оказались потребители, купившие на 1 тыс. рублей и на 15 тысяч. При этом те, кто покупал на действительно крупные суммы, в отдельную группу не выделились (в отличие от предыдущего метода).
* Количество групп по каждому признаку одинаковое.
* Много сегментов.

### Подход 3. Ручной

Аналитик изучает базу данных и подбирает правильное разделение. 

Плюсы метода:

* Хорошее разделение на сегменты.

Минусы метода:

* Нужен специалист.
* Нужно много времени.

# RFM-отчет одной кнопкой с помощью Machine Learning

Мы решили избавиться от недостатков старых подходов. Для этого пришлось прибегнуть к алгоритмам [Machine Learning](https://ru.wikipedia.org/wiki/Машинное_обучение).

Используя методы кластеризации, мы автоматически определяем, сколько же на самом деле сегментов потребителей в базе и что это за сегменты. А с помощью решающего дерева приводим эти сегменты к удобному для восприятия виду. Как это работает, расскажем в отдельной статье про устройство сегментатора.

Для примера выше мы получили вот такой результат:

<img alt="image" src="https://habrastorage.org/webt/q9/pm/br/q9pmbrvurcgyazb6aben74jsr1y.png" />
<br>

Чтобы все это было удобным и понятным для маркетологов, мы разработали отчет, в котором удобно и понятно (как нам кажется) описаны результаты сегментации.

Чтобы получить его, достаточно нажать одну кнопку — и система все сделает сама.
Отчет помещается на одну страницу и состоит из трех таблиц.

### Часть 1. Оценка состояния базы

Первая таблица — сводная. В ней собрана информация по всем сегментам базы, полученная на основе RFM-анализа. Ключевые показатели: активность потребителей в сегменте и их ценность.

Активность определяется давностью последней покупки, а ценность — потраченной суммой.

Каждый сегмент относится к одной из категорий. В каждой категории может быть несколько сегментов или вообще не быть ни одного. В ячейках указано общее количество потребителей из всех сегментов категории. 

<img  alt="image" src="https://habrastorage.org/webt/fn/s-/4i/fns-4igr3soh36lfax2d_tdivek.png">
*Показатели «Активность» и «Ценность» образуют девять категорий сегментов. Еще одна категория: «Никогда не покупали»*

P.S. Здесь выражения "Отток" и "Риск оттока" используются как сокращения для "Давно не покупавшие клиенты" и "Клиенты, покупавшие среднее количество времени назад" и не означают *отток* в прямом смысле этого слова. Аналогично, "Активные" — обозначение для "Клиенты, недавно сделавшие покупку".

В примере выше 80% клиентов не имеют покупок, почти треть высокоценных — в оттоке, еще треть — в группе риска.

Оценка состояния базы помогает выбрать категорию, с которой важно работать в первую очередь.

Чтобы показать, как пользоваться отчетом, возьмем клиентов с высокой ценностью, то есть клиентов, потративших больше всего денег.

### Часть 2. Изучение сегментов
Во второй таблице отчета выводятся: размер сегментов, оборот, то есть сумма, потраченная всеми потребителями в сегменте, и средний чек.

Все сегменты потребителей представляются списком. Например, вот список сегментов покупателей, имеющих покупки:

<img alt="image" src="https://habrastorage.org/webt/7o/bu/i6/7obui6vxcws367sikyxdk53scrg.png">
<br> 

Чтобы вывести в отчет только потребителей с высокой ценностью, используем фильтр.

<img alt="image" src="https://habrastorage.org/webt/hq/dq/wd/hqdqwdleefjj6vffswgyooiplaa.png">
<br> 

В результате применения фильтра получаем семь сегментов потребителей с высокой ценностью.

<img alt="image" src="https://habrastorage.org/webt/5s/7m/wy/5s7mwyqff8b_1u9lh8nkwlprpdq.png">
<br> 

На основе этой информации можно сделать разные выводы.

Например, сегмент №2 имеет значительно больший оборот, чем другие, при умеренном среднем чеке. Это говорит о большом числе покупок потребителей в этом сегменте и их высокой лояльности. Не опасаясь оттока клиентов, им можно рассылать письма и рассказывать, например, о новинках.

Теперь обратим внимание на средний чек: сегмент №7 с самым большим средним чеком находится в оттоке, а сегмент №9 со вторым по величине средним чеком — в группе риска. Потребители из данных сегментов готовы покупать на крупные суммы, но не покупали уже давно. Возможно, имеет смысл побудить их к действиям с помощью промокода или информационного письма.

Изучение сегментов нужно, чтобы понять, с какими сегментами стоит усиленно поработать.

### Часть 3. Детальная информация по сегментам
В последней таблице показаны границы сегментов по каждому признаку (R, F, M) и средние значения по ним.

<img alt="image" src="https://habrastorage.org/webt/vp/fv/nu/vpfvnuaah3ivgyfpqbycrdkolh8.png">

*Из этой таблицы видно, что потребители из сегмента №2 действительно имеют больше покупок, чем другие, — в среднем 12*

Нам нужно выбрать, с каким сегментом мы хотим работать первым. Допустим, нас заинтересовали сегменты с самыми большими средними чеками: №7 и №9. Рассмотрим их подробнее.

В сегменте №7 клиенты не делали покупки почти год — вернуть их будет нелегко. Но, возможно, попробовать стоит, поскольку в среднем потребители из данного сегмента покупали 2,1 раза — это значит, что первая покупка их не разочаровала. Вполне вероятно, что хорошая скидка поможет им снова активно заинтересоваться брендом.

С сегментом №9 проще — средняя давность покупки у клиентов из него составляет всего три месяца, а среднее количество покупок — 2,8. Скорее всего, эти клиенты достаточно лояльны и не требуют по отношению к себе никаких действий. Но можно отправить письмо с рекламой или небольшой скидкой, чтобы напомнить о бренде.

Когда сегменты для дальнейших действий выбраны, можно запускать нужные маркетинговые кампании.

# До настоящей *Зеленой Кнопки* осталось совсем немного

Мы создали автоматический RFM-сегментатор и остались довольны — нужно 20 секунд времени человека, чтобы получить распределение базы клиентов по сегментам.

Мы собираемся автоматизировать настройку маркетинговых кампаний для сегментов, чтобы человеку и на это не нужно было тратить время.

Конечно, жалко будет, что никому больше не понадобится наш отчет, но технический прогресс не щадит никого.