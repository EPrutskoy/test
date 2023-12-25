# Проект: Прогнозирование оттока клиентов в сети отелей «Как в гостях»
## Описание проекта
    Заказчик этого исследования — сеть отелей "Как в гостях".
С целью привлечения клиентов, эта сеть отелей добавила на свой сайт возможность забронировать номер без предоплаты. Однако, если клиент отменял бронирование, то компания терпела убытки. Сотрудники отеля могли, например, закупить продукты к приезду гостя или просто не успеть найти другого клиента.
### Цель проекта
Разработать систему, которая предсказывает отказ от брони. Если модель покажет, что бронь будет отменена, то клиенту предлагается внести депозит. Размер депозита — 80% от стоимости номера за одни сутки и затрат на разовую уборку. Деньги будут списаны со счёта клиента, если он всё же отменит бронь.

### Бизнес-метрика и другие данные
Основная бизнес-метрика для любой сети отелей — её прибыль. \
Прибыль отеля — это разница между стоимостью номера за все ночи и затраты на обслуживание: как при подготовке номера, так и при проживании постояльца. В отеле есть несколько типов номеров. В зависимости от типа номера назначается стоимость за одну ночь. Есть также затраты на уборку. Если клиент снял номер надолго, то убираются каждые два дня.

**Стоимость номеров отеля:**
* **категория A**: 
    * за ночь — 1 000, 
    * разовое обслуживание — 400;
    
* **категория B**: 
    * за ночь — 800,
    * разовое обслуживание — 350;
* **категория C**: 
    * за ночь — 600, 
    * разовое обслуживание — 350;
* **категория D**: 
    * за ночь — 550, 
    * разовое обслуживание — 150;
* **категория E**: 
    * за ночь — 500, 
    * разовое обслуживание — 150;
* **категория F**: 
    * за ночь — 450, 
    * разовое обслуживание — 150;
* **категория G**: 
    * за ночь — 350,
    * разовое обслуживание — 150.

* **В ценовой политике отеля используются сезонные коэффициенты**: 
    * весной и осенью цены повышаются на 20%, 
    * летом — на 40%. 
* **Убытки отеля в случае отмены брони номера** — это стоимость одной уборки и одной ночи с учётом сезонного коэффициента. 
* **На разработку системы прогнозирования заложен бюджет** — `400'000`.\
При этом необходимо учесть, что внедрение модели должно окупиться за тестовый период. Затраты на разработку должны быть меньше той выручки, которую система принесёт компании.
### Описание данных
В таблицах `hotel_train` и `hotel_test` содержатся одинаковые столбцы:

* `id` — номер записи;
* `adults` — количество взрослых постояльцев;
* `arrival_date_year` — год заезда;
* `arrival_date_month` — месяц заезда;
* `arrival_date_week_number` — неделя заезда;
* `arrival_date_day_of_month` — день заезда;
* `babies` — количество младенцев;
* `booking_changes` — количество изменений параметров заказа;
* `children` — количество детей от 3 до 14 лет;
* `country` — гражданство постояльца;
* `customer_type` — тип заказчика:
   * *Contract* — договор с юридическим лицом;
   * *Group* — групповой заезд;
   * *Transient* — не связано с договором или групповым заездом;
   * *Transient-party* — не связано с договором или групповым заездом, но связано с бронированием типа *Transient*.
* `days_in_waiting_list` — сколько дней заказ ожидал подтверждения;
* `distribution_channel` — канал дистрибуции заказа;
* `is_canceled` — отмена заказа;
* `is_repeated_guest` — признак того, что гость бронирует номер второй раз;
* `lead_time` — количество дней между датой бронирования и датой прибытия;
* `meal` — опции заказа:
   * *SC* — нет дополнительных опций;
   * *BB* — включён завтрак;
   * *HB* — включён завтрак и обед;
   * *FB* — включён завтрак, обед и ужин.
* `previous_bookings_not_canceled` — количество подтверждённых заказов у клиента;
* `previous_cancellations` — количество отменённых заказов у клиента;
* `required_car_parking_spaces` — необходимость места для автомобиля;
* `reserved_room_type` — тип забронированной комнаты;
* `stays_in_weekend_nights` — количество ночей в выходные дни;
* `stays_in_week_nights` — количество ночей в будние дни;
* `total_nights` — общее количество ночей;
* `total_of_special_requests` — количество специальных отметок.

*Построение и Оценка Моделей**
* Разработка Пайплайнов: Пайплайны были созданы для обеспечения эффективной предобработки данных и последовательного применения моделей. Это включало масштабирование числовых данных, кодирование категориальных переменных и интеграцию моделей машинного обучения.

* Выбор Моделей: Исследовались различные модели, включая логистическую регрессию и другие алгоритмы, с целью нахождения наиболее подходящей модели для нашей задачи. Особое внимание уделялось оценке полноты (recall), так как целью было максимизировать правильное идентифицирование отмен бронирования.

* Настройка Гиперпараметров: С помощью методов, таких как RandomizedSearchCV, была проведена тонкая настройка гиперпараметров моделей для достижения наилучшей производительности.

* Оценка Производительности: Модели были оценены с использованием метрик, таких как точность, полнота, F1-мера и AUC-ROC. Это помогло оценить их способность точно идентифицировать отмены бронирований, а также общий баланс между различными метриками.

* Визуализация Результатов: Для наглядного представления результатов были построены графики, включая ROC-кривые и графики важности признаков. Это помогло лучше понять вклад отдельных признаков в принятие решений моделью.

**Выбор Итоговой Модели**
* После тщательного анализа различных моделей, их производительности и важности признаков была выбрана итоговая модель, которая лучше всего соответствовала целям проекта:

* Модель 2 (Логистическая Регрессия) была выбрана как итоговая модель, поскольку она демонстрировала хороший баланс между полнотой и точностью, а также имела удовлетворительную общую точность.
* Эта модель показала высокую полноту для класса отмен бронирований, что важно для минимизации пропущенных случаев отмен.
* При этом модель также обеспечивала приемлемую точность, что важно для избежания чрезмерного количества ложноположительных результатов.

**Общий Вывод**

Процесс построения и оценки моделей был тщательным и многоступенчатым, что позволило не только выбрать оптимальную модель для задачи, но и глубоко понять вклад отдельных признаков и особенности работы моделей. Итоговая модель логистической регрессии обеспечивает хороший баланс между необходимостью правильно идентифицировать отмены и поддержанием общей точности предсказаний.

#### Портрет ненадежного клиента:

Исходя из важности признаков по финальной модели, а также графиков распределения переменных и долей таргет-принзака в них можно сказать:
* Ненадежный клинт отменял бронь в прошолом.(склонен отменять еще)
* Ненадежжный клиент бронирует сильно заранее даты заездаю. (больше времени отменить)
* У ненадежного клиента скорее всего есть дети (путешествие с ними полно сюрпризов)
* Ненадежный клиент не запрашивает место для парковки (скорее всего путешествие на машине планируется очень строго и планы не склонны меняться)
* Ненадежный клиент имеет малое кол-во или не имеет особых запросов вообще (не особо "парится" по поводу брони)
* также ненадежный клиент не меняет брони (также не особо волнуется)
* У ненадежного клиента нет подтвержденных броней в прошлом


# Общий вывод
В ходе нашей работы мы провели обширный анализ данных и построили серию моделей машинного обучения для предсказания отмены бронирований в сети отелей. 

**Исследовательский Анализ Данных**
* Анализ данных: Был проведен тщательный анализ данных, включая оценку распределения ключевых признаков и целевой переменной. Это позволило лучше понять структуру и особенности данных.

* Визуализация данных: С помощью графиков мы визуализировали важные зависимости и распределения, что дало наглядное представление о данных и помогло в выявлении потенциальных путей для построения модели.

* Построение и Оценка Моделей
Подготовка данных: Данные были тщательно подготовлены для моделирования, включая обработку пропущенных значений, кодирование категориальных признаков и масштабирование числовых данных.

* Выбор и настройка моделей: Были исследованы различные модели, включая логистическую регрессию. С помощью методов, таких как RandomizedSearchCV, была проведена настройка гиперпараметров для достижения наилучшей производительности.

* Оценка моделей: Модели были оценены с использованием различных метрик, включая точность, полноту, F1-меру и AUC-ROC. Особое внимание уделялось полноте для максимизации правильного идентифицирования отмен бронирований.

* Визуализация результатов: Графики ROC-кривой и важности признаков были использованы для оценки качества и интерпретируемости моделей.

**Финансовый Анализ**
* Оценка финансового влияния: Мы рассчитали общую прибыль/убытки как до, так и после внедрения модели, учитывая внесение депозитов клиентами при предсказании отмены бронирования.

* Экономическая эффективность: Анализ показал, что внедрение модели может быть экономически выгодным, особенно если модель эффективно предсказывает отмены бронирований.

**Общие Выводы**

Модель, которую мы разработали, обладает способностью эффективно предсказывать отмены бронирований, что может быть полезно для минимизации убытков отеля. Анализ финансового влияния показал потенциальную экономическую выгоду от внедрения модели. Таким образом, наша работа предоставляет ценные входные данные для принятия решений по управлению бронированиями в сети отелей и может быть использована для повышения их эффективности и прибыльности.
