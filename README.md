### Эпигеномика
# Домашнее задание №4. Отчёт
В данном практическом задании проведена работа с данными Hi-C с помощью инструментов cooler и cooltools.

Работа производилась в [блокноте Google Colab](https://colab.research.google.com/drive/1ckXPVOC6pwCE0RM9bvP4kelChfN6x1FL?usp=sharing).

- Для задания выбрано 2 файла результатов Hi-C в формате mcool с сайта https://data.4dnucleome.org/: `4DNFIQWCSCVX` и `4DNFISBF7WSJ`
- Исследуемый участок генома: `chr4:20,000,000-21,000,000` для обоих файлов

Скриншоты Hi-Glass для выбранного участка:

|  4DNFIQWCSCVX | 4DNFISBF7WSJ  |
|---|---|
| ![](https://github.com/akamaaru/hse25_hw4/blob/main/img/4DNFIQWCSCVX/hi-glass.png) | ![](https://github.com/akamaaru/hse25_hw4/blob/main/img/4DNFISBF7WSJ/hi-glass.png) |

## a. получить информацию и атрибуты матрицы Hi-C с помощью `cooler.info`
Для обоих объектов cooler вывело следующее:
|  |  4DNFIQWCSCVX | 4DNFISBF7WSJ  |
|---|---|---|
| bin-size |np.int32(5000) |np.int64(5000) |
| bin-type | 'fixed'|'fixed' |
| creation-date | '2024-12-06T12:08:24.142385' |'2024-03-02T13:12:01.815078' |
| format | 'HDF5::Cooler' |'HDF5::Cooler' |
| format-url | 'https://github.com/open2c/cooler' | 'https://github.com/mirnylab/cooler'|
| format-version | np.int64(3) |3 |
| generated-by |'cooler-0.9.3' | 'cooler-0.8.3'|
| genome-assembly |'unknown' |'unknown' |
| metadata |{} |{} |
| nbins | np.int64(617665)|np.int64(545114) |
| nchroms | np.int64(24)|np.int64(21) |
| nnz |np.int64(332296443) | np.int64(175072550)|
| storage-mode | 'symmetric-upper'| 'symmetric-upper'|
| sum |np.int64(604633594) | np.int64(227741310)|

## b. открыть объект `cooler` как сбалансированную матрицу для внутрихромосомных контактов
Откроем объекты `cooler` как сбалансированную матрицу, использовав следующий код:
``` python
start = 20_000_000
end = start + 1_000_000

region = ('chr4', start, end)
m1 = clr1.matrix(as_pixels=True, balance=True).fetch(region)
m2 = clr2.matrix(as_pixels=True, balance=True).fetch(region)
```

## c. получить таблицу с координатами и контактами; они сбалансированные или нет?
## d. получить таблицу в командной строке командой `cooler dump`
## e. посмотрите таблицу с бинами, какие столбцы там присутствуют?
## f. постройте кривые зависимости число контактов от расстояния для выбранной хромосомы (в логарифмических координатах); сравните их
## g. для выбранного участка найдите `insulation score` и границы ТАДов для обоих файлов
## h. сравните результаты и постройте графики полученных кривых; отобразите на них границы ТАДов
## i. создайте 2 bed файла с границами ТАДов; в поле score добавьте силу границы
