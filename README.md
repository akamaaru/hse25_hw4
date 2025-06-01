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
Полученные таблицы сбалансированы. Изобразим их на тепловой карте:

|  4DNFIQWCSCVX | 4DNFISBF7WSJ  |
|---|---|
| ![](https://github.com/akamaaru/hse25_hw4/blob/main/img/4DNFIQWCSCVX/heatmap.png) | ![](https://github.com/akamaaru/hse25_hw4/blob/main/img/4DNFISBF7WSJ/heatmap.png) |

## d. получить таблицу в командной строке командой `cooler dump`
### Для `4DNFIQWCSCVX`
Команда:
``` bash
cooler dump -b -t pixels --header --join -r chr4:20,000,000-21,000,000 /content/drive/MyDrive/биоинфа/4DNFIQWCSCVX.mcool::resolutions/5000 | head
```

Вывод:
```
chrom1	start1	end1	chrom2	start2	end2	count	balanced
chr4	20000000	20005000	chr4	20000000	20005000	114	0.106466
chr4	20000000	20005000	chr4	20005000	20010000	59	0.0423429
chr4	20000000	20005000	chr4	20010000	20015000	20	0.0203149
chr4	20000000	20005000	chr4	20015000	20020000	10	0.0107331
chr4	20000000	20005000	chr4	20020000	20025000	6	0.00587755
chr4	20000000	20005000	chr4	20025000	20030000	3	0.00358766
chr4	20000000	20005000	chr4	20030000	20035000	6	0.00600741
chr4	20000000	20005000	chr4	20035000	20040000	3	0.00303769
chr4	20000000	20005000	chr4	20040000	20045000	6	0.00710181
```

### Для `4DNFISBF7WSJ`
Команда:
``` bash
cooler dump -b -t pixels --header --join -r chr4:20,000,000-21,000,000 /content/drive/MyDrive/биоинфа/4DNFISBF7WSJ.mcool::resolutions/5000 | head
```

Вывод:
```
chrom1	start1	end1	chrom2	start2	end2	count	balanced
chr4	20000000	20005000	chr4	20000000	20005000	18	0.0272431
chr4	20000000	20005000	chr4	20005000	20010000	14	0.0228169
chr4	20000000	20005000	chr4	20010000	20015000	12	0.0181249
chr4	20000000	20005000	chr4	20015000	20020000	9	0.0128188
chr4	20000000	20005000	chr4	20020000	20025000	9	0.00722343
chr4	20000000	20005000	chr4	20025000	20030000	4	0.00408337
chr4	20000000	20005000	chr4	20030000	20035000	3	0.00403547
chr4	20000000	20005000	chr4	20035000	20040000	4	0.0053593
chr4	20000000	20005000	chr4	20040000	20045000	2	0.00211754
```

## e. посмотрите таблицу с бинами, какие столбцы там присутствуют?
## f. постройте кривые зависимости число контактов от расстояния для выбранной хромосомы (в логарифмических координатах); сравните их
## g. для выбранного участка найдите `insulation score` и границы ТАДов для обоих файлов
## h. сравните результаты и постройте графики полученных кривых; отобразите на них границы ТАДов
## i. создайте 2 bed файла с границами ТАДов; в поле score добавьте силу границы
