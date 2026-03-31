# Coffee Churn Prediction

Проект бинарной классификации для прогноза оттока клиентов сервиса доставки кофе (Happy Beans Coffee).

## Цель проекта

Построить интерпретируемую модель, которая прогнозирует вероятность оттока клиента в следующем месяце, чтобы:
- снизить потери выручки из-за ухода клиентов;
- точнее направлять retention-акции;
- эффективнее расходовать маркетинговый бюджет.

## Данные

- Файл: `coffee_churn_dataset.csv`
- Объём: `10 450` строк, `27` колонок
- Целевая переменная: `churn` (`1` — клиент ушёл, `0` — клиент остался)
- Дисбаланс классов: доля оттока около `6%`

## Что сделано

1. Проведён EDA:
- анализ структуры данных, типов и пропусков;
- анализ дисбаланса целевой переменной;
- анализ категориальных признаков и выбросов;
- корреляционный анализ (`PhiK`).

2. Построен пайплайн предобработки:
- удалён технический идентификатор `user_id`;
- импутация пропусков (`median` для числовых, `most_frequent` для категориальных);
- масштабирование числовых признаков (`RobustScaler`);
- кодирование категориальных признаков:
  - `geo_location` -> `frequency encoding`;
  - остальные категориальные -> `OneHotEncoder(handle_unknown='ignore')`.

3. Сгенерированы дополнительные признаки (feature engineering):
- `spent_per_order_month`
- `spent_per_order_week`
- `days_since_last_order_sqrt`
- `app_crashes_last_month_sq`
- `days_since_last_promo_sq`
- `inactivity_ratio`

4. Выполнен подбор гиперпараметров `LogisticRegression` через `GridSearchCV`.

## Лучшая модель

- Алгоритм: `LogisticRegression`
- Параметры:
  - `solver='liblinear'`
  - `penalty='l1'`
  - `C=0.1`
  - `class_weight='balanced'`
  - `max_iter=3000`
  - `random_state=42`

### Кросс-валидация (train)
- `PR AUC = 0.6811`

### Финальные метрики (test)
- `PR AUC = 0.7329`
- `F1 = 0.5201`
- `Precision = 0.3704`
- `Recall = 0.8730`

## Артефакты

Сохранённый pipeline (предобработка + модель):
- `artifacts/churn_model_pipeline.joblib`

Проверка загрузки через `joblib.load(...)` выполнена, метрики после загрузки совпадают с финальными.

## Как запустить

```powershell
cd D:\Dev\coffee-churn-prediction
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter lab
```

Далее открыть ноутбук `coffee-churn-prediction.ipynb` и выполнить ячейки по порядку.

## Стек

- Python
- pandas, numpy
- scikit-learn
- matplotlib, seaborn
- phik
- joblib
