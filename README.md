# Восстановление золота из руды  
### Общее описание
Подготовьте прототип модели машинного обучения для «Цифры». Компания разрабатывает решения для эффективной работы промышленных предприятий.

Модель должна предсказать коэффициент восстановления золота из золотосодержащей руды. Используйте данные с параметрами добычи и очистки.

Модель поможет оптимизировать производство, чтобы не запускать предприятие с убыточными характеристиками.

Вам нужно:
 - Подготовить данные;
 - Провести исследовательский анализ данных;
 - Построить и обучить модель.

Чтобы выполнить проект, обращайтесь к библиотекам pandas, matplotlib и sklearn. Вам поможет их документация.

### Технологический процесс
Технологический процесс представляет из себя постепенное преобразование смеси золотоносной руды в финальный концентрат. Руда -> Смесь золотоносной руды -> Черновой концентрат -> Первый этап очистки -> Второй этап очистки -> Финальный концентрат
Описание стадий процесса:
 * Первичная обрботка;
 * Когда добытая руда проходит первичную обработку, получается дроблёная смесь. Её отправляют на флотацию (обогащение) и двухэтапную очистку.
    Флотация;
 * Во флотационную установку подаётся смесь золотосодержащей руды. После обогащения получается черновой концентрат и «отвальные хвосты», то есть остатки продукта с низкой концентрацией ценных металлов. На стабильность этого процесса влияет непостоянное и неоптимальное физико-химическое состояние флотационной пульпы (смеси твёрдых частиц и жидкости).
 * Очистка.
 * Черновой концентрат проходит две очистки. На выходе получается финальный концентрат и новые отвальные хвосты. Если быть точным, то отвальные хвосты получается при каждом переходе с этапа на этап.

### Описание данных
**Технологический процесс**

Rougher feed — исходное сырье
Rougher additions (или reagent additions) — флотационные реагенты: Xanthate, Sulphate, Depressant
 - Xanthate — ксантогенат (промотер, или активатор флотации);
 - Sulphate — сульфат (на данном производстве сульфид натрия);
 - Depressant — депрессант (силикат натрия).
Rougher process (англ. «грубый процесс») — флотация
Rougher tails — отвальные хвосты
Float banks — флотационная установка
Cleaner process — очистка
Rougher Au — черновой концентрат золота
Final Au — финальный концентрат золота

**Параметры этапов**
air amount — объём воздуха
fluid levels — уровень жидкости
feed size — размер гранул сырья
feed rate — скорость подачи

**Наименование признаков**  
Наименование признаков должно быть такое: [этап].[тип_параметра].[название_параметра] Пример: rougher.input.feed_ag  

**Возможные значения для блока [этап]:**
rougher — флотация
primary_cleaner — первичная очистка
secondary_cleaner — вторичная очистка
final — финальные характеристики

**Возможные значения для блока [тип_параметра]:**

input — параметры сырья
output — параметры продукта
state — параметры, характеризующие текущее состояние этапа
calculation — расчётные характеристики  

**Метрика качества**
Для решения задачи введём новую метрику качества — sMAPE (англ. Symmetric Mean Absolute Percentage Error, «симметричное среднее абсолютное процентное отклонение»).
Она похожа на MAE, но выражается не в абсолютных величинах, а в относительных. Почему симметричная? Она одинаково учитывает масштаб и целевого признака, и предсказания.

Нужно спрогнозировать сразу две величины:
 * эффективность обогащения чернового концентрата rougher.output.recovery;
 * эффективность обогащения финального концентрата final.output.recovery.

**Итоговая метрика складывается из двух величин:**
sMAPE = 25% * sMAPE(rougher) + 75% * sMAPE(final)

### Ход выполнения

    Подготовить данные
    1.1. Открыть файлы и изучите их. Путь к файлам:

     /datasets/gold_recovery_train_new.csv
     /datasets/gold_recovery_test_new.csv
     /datasets/gold_recovery_full_new.csv

    1.2. Проверить, что эффективность обогащения рассчитана правильно:
    1.2.1 Вычислиь её на обучающей выборке для признака rougher.output.recovery.
    1.2.2 Найти MAE между вашими расчётами и значением признака.
    1.2.3 Опишите выводы.
    1.3. Проанализировать признаки, недоступные в тестовой выборке. Что это за параметры? К какому типу относятся?
    1.4. Провести предобработку данных.

    Проанализируйте данные
    2.1. Посмотреть, как меняется концентрация металлов (Au, Ag, Pb) на различных этапах очистки. Описать выводы.
    2.2. Сравнить распределения размеров гранул сырья на обучающей и тестовой выборках. Если распределения сильно отличаются друг от друга, оценка модели будет неправильной.
    2.3. Исследовать суммарную концентрацию всех веществ на разных стадиях: в сырье, в черновом и финальном концентратах.

    Построить модель
    3.1. Написать функцию для вычисления итоговой sMAPE.
    3.2. Обучить разные модели и оцените их качество кросс-валидацией. Выбрать лучшую модель и проверить её на тестовой выборке.
    3.3 Описать выводы.
