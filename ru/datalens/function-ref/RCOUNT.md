---
editable: false
---

# RCOUNT

_Оконные функции_

#### Синтаксис {#syntax}


```
RCOUNT( value [ , direction ] [ TOTAL | WITHIN [ dim1, ... ] | AMONG [ dim1, ... ] ] )
```

#### Описание {#description}
Возвращает количество значений в рамках окна записей, определяемого порядком сортировки и значением аргумента `direction`:

| `direction`   | Окно                            |
|:--------------|:--------------------------------|
| `"asc"`       | От первой записи до текущей.    |
| `"desc"`      | От текущей записи до последней. |

По умолчанию используется значение `"asc"`.


Аналогичное поведение у оконных функций [RSUM](RSUM.md), [RMIN](RMIN.md), [RMAX](RMAX.md), [RAVG](RAVG.md).

См. также [COUNT](COUNT.md), [MCOUNT](MCOUNT.md).

**Типы аргументов:**
- `value` — `Любой`
- `direction` — `Строка`


**Возвращаемый тип**: `Целое число`

{% note info %}

Значения аргументов (`direction`) должны быть константами.

{% endnote %}

{% note warning %}

Функция зависит от порядка сортировки в чарте. Она будет правильно работать только если:
- в чарте явно задана сортировка;
- в сортировке участвуют __все__ поля, не содержащие оконных функций.

{% endnote %}


#### Примеры {#examples}

```
RCOUNT([Profit])
```

```
RCOUNT([Profit], "asc")
```

```
RCOUNT([Profit] TOTAL)
```

```
RCOUNT([Profit], "desc" WITHIN [Date])
```

```
RCOUNT([Profit] AMONG [Date])
```


#### Поддержка источников данных {#data-source-support}

`Материализованный датасет`, `ClickHouse 1.1`, `Microsoft SQL Server 2017 (14.0)`, `MySQL 5.6`, `PostgreSQL 9.3`.
