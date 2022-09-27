# predicts-temperature-of-steel
Чтобы оптимизировать производственные расходы, требуется уменьшить потребление электроэнергии на этапе обработки стали.  Построение модели, которая предскажет температуру стали

# ОПИСАНИЕ ЭТАПА ОБРАБОТКИ

Сталь обрабатывают в металлическом ковше вместимостью около 100 тонн. Чтобы ковш выдерживал высокие температуры, изнутри его облицовывают огнеупорным кирпичом. Расплавленную сталь заливают в ковш и подогревают до нужной температуры графитированными электродами. Они установлены в крышке ковша.

Из сплава выводится сера (десульфурация), добавлением примесей корректируется химический состав и отбираются пробы. Сталь легируют — изменяют её состав — подавая куски сплава из бункера для сыпучих материалов или проволоку через специальный трайб-аппарат (англ. tribe, «масса»).

Перед тем как первый раз ввести легирующие добавки, измеряют температуру стали и производят её химический анализ. Потом температуру на несколько минут повышают, добавляют легирующие материалы и продувают сплав инертным газом. Затем его перемешивают и снова проводят измерения. Такой цикл повторяется до достижения целевого химического состава и оптимальной температуры плавки.

Тогда расплавленная сталь отправляется на доводку металла или поступает в машину непрерывной разливки. Оттуда готовый продукт выходит в виде заготовок-слябов (англ. slab, «плита»).

# Цель исследования:

- построение модели для предсказания температуру стали.
# Данные состоят из файлов, полученных из разных источников:

- data_arc.csv — данные об электродах;
- data_bulk.csv — данные о подаче сыпучих материалов (объём);
- data_bulk_time.csv — данные о подаче сыпучих материалов (время);
- data_gas.csv — данные о продувке сплава газом;
- data_temp.csv — результаты измерения температуры;
- data_wire.csv — данные о проволочных материалах (объём);
- data_wire_time.csv — данные о проволочных материалах (время).

*Во всех файлах столбец key содержит номер партии. В файлах может быть несколько строк с одинаковым значением key: они соответствуют разным итерациям обработки.*

# Ход исследования

## ЭТАП Подготовка данных
- преобразование типов данных
- удаление лишних временных меток
- обработка пропусков и выбросов
- анализ событий в течение одного цикла плавки, посчитать информацию по партиям
- оставить только необходимые для обучения признаки
- объединить необходимые данные в одну таблицу по ключам (key)

## ЭТАП Подготовка признаков: создание таблицы со всеми данными для модели, определение целевого признака:
- разделить датасет на тестовую и валидационную выборки
- проверить равномерность выборок

## ЭТАП Выбор регрессионных моделей (CatBoostRegressor, LGBMRegressor и т.д.) и сравнение качества предсказаний^
- RANDOM_STATE = 12092022
- тестовая выборка - 20%
- Будем использовать подбор параметров с использованием GridSearchCV.
- Будем искать лучшую модель на train-выборке (подбор моделей).
- На train-выборке будем проверять ТОЛЬКО лучшую модель.
- Метрика: МАЕ (не более 8.7)

## ЭТАП Оценка эффективности модели на тестовой выборке.

## ЭТАП Выводы

**Дополнитльено на 1 этапе НАЙТИ:**
- Длительность времени между первым и последним замером температуры.
- Суммарное время нагрева электродами, то есть сумму значений по всем промежуткам между запусками нагрева электродов.
- Количество запусков нагрева электродами.
- Среднее соотношение потребления активной и реактивной мощности.
- По всем полученным столбцам вычислите статистики: средние, минимальные и максимальные значения, медиану и величины 25%- и 75%-квартилей.
