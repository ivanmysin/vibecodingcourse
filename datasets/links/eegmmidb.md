# ЭЭГ: EEG Motor Movement/Imagery Dataset

Карточка тяжёлого датасета для урока 1.2. Файлы в репозитории не лежат —
скачайте по ссылке ниже.

## Что скачать

Три файла субъекта S001, по одному на каждое условие:

| Файл | Условие | Размер |
|---|---|---|
| `S001R02.edf` | глаза закрыты — фон, альфа-ритм выражен | ≈ 1,2 МБ |
| `S001R03.edf` | реальное движение кистью | ≈ 2,5 МБ |
| `S001R04.edf` | мысленное движение кистью | ≈ 2,5 МБ |

Кладите их в `data/raw/S001/` своей рабочей папки.

## Откуда

Первоисточник: **PhysioNet**, `EEG Motor Movement/Imagery Dataset` v1.0.0
<https://physionet.org/content/eegmmidb/1.0.0/>

Порядок действий:

1. Открыть ссылку выше.
2. Скачать каталог `S001/` (или отдельные файлы `S001R02.edf`,
   `S001R03.edf`, `S001R04.edf`).
3. Положить в `data/raw/S001/`.

Зеркало на Kaggle (если PhysioNet недоступен):
<https://www.kaggle.com/datasets/brianleung2020/eeg-motor-movementimagery-dataset>

## Контрольные суммы (SHA-256)

Проверить, что скачалось то, что нужно:

```bash
sha256sum data/raw/S001/S001R02.edf
sha256sum data/raw/S001/S001R03.edf
sha256sum data/raw/S001/S001R04.edf
```

```
31a95e0a880e6c3d89960d9d62c144f24cc4e9f5d7e93c7f864ef61cd49e847e  S001R02.edf
3427c8d01bff1380bc9ab9f27a35ece2af5dfadf3e291bbc05eb66e4dadbfe2e  S001R03.edf
3d161f88e1c00632585287d2ce584c2bc0f08862438eb255ea8723e00fac693d  S001R04.edf
```

## Что внутри файла

- 64 канала ЭЭГ; события движения — в аннотациях EDF, отдельного канала нет
- частота дискретизации 160 Гц
- длительность: runs 1–2 (фон) — 61 с, runs 3–14 — 125 с
- формат EDF+ (European Data Format)

Каналы названы по схеме 10-10: `Fc5.`, `Fc3.`, `C5..`, `Cz.` и так далее.
Точка в конце имени — часть формата EDF, не опечатка. MNE читает такие
имена как `Fc5.`, `C5..`; перед построением топокарты их нужно привести
к стандартному виду (`FC5`, `C5`) и повесить монтаж:

```python
import re
raw.rename_channels(lambda c: re.sub(r'[^A-Za-z0-9]', '', c).upper())
raw.set_montage('standard_1005', match_case=False)
```

Проверено: все 64 канала сопоставляются с `standard_1005` без пропусков.

## Про runs

У каждого субъекта 14 записей:

| Runs | Что это |
|---|---|
| 1, 2 | фон: глаза открыты / глаза закрыты |
| 3, 7, 11 | реальное движение: левый или правый кулак |
| 4, 8, 12 | воображение движения: левый или правый кулак |
| 5, 9, 13 | реальное движение: обе кисти или обе стопы |
| 6, 10, 14 | воображение: обе кисти или обе стопы |

Runs 3, 7, 11 — это **три повтора одного и того же условия**, а не три
разные задачи. Поэтому в уроке взяты 02, 03 и 04: фон, движение и
воображение.

## Лицензия и цитирование

Датасет распространяется по лицензии **Open Data Commons Attribution
License v1.0 (ODC-By 1.0)**. При использовании указывайте источник.

Если используете данные в работе, цитируйте:

> Schalk, G., McFarland, D.J., Hinterberger, T., Birbaumer, N., Wolpaw, J.R.
> BCI2000: A General-Purpose Brain-Computer Interface (BCI) System.
> *IEEE Transactions on Biomedical Engineering* 51(6):1034-1043, 2004.

и стандартную ссылку на PhysioNet:

> Goldberger AL, Amaral LAN, Glass L, Hausdorff JM, Ivanov PCh, Mark RG,
> Mietus JE, Moody GB, Peng C-K, Stanley HE. PhysioBank, PhysioToolkit,
> and PhysioNet: Components of a New Research Resource for Complex
> Physiologic Signals. *Circulation* 101(23):e215-e220, 2000.

Монтаж электродов (схема 10-10) — файл `64_channel_sharbrough.pdf`
в материалах PhysioNet: <https://archive.physionet.org/pn4/eegmmidb/64_channel_sharbrough.pdf>
