# Операторно-параметрическая схема "Прибор-Мастер"

## Общая модель ремонта прибора

```mermaid
flowchart TD
title_total[<em>ОПС модели ремонта ПРИБОР-МАСТЕР</em>]

time((ВРЕМЯ))
master((мастер))
trem((Трем))

subgraph TRACK1 [Трек ПРИБОР]
    direction TB
    
    h1["сост:=сломан"] ==> h2{"режим_п=работа"}
    h2 ==> h3["мастер:=занят"]
    h3 ==> h4{"ВРЕМЯ = Трем"}
    h4 ==> h5["сост:=рабочий<br>мастер:=своб<br>Тслом:=func(x)"]
    h5 ==> h1
    
    h1 -.->par3((сост))
    h5 -.->par3
    par_regim_p((режим_п))-.->h2
    h3 -.->master
    h5 -.->master
    h3 -.->trem
    h4 -.->time
    h4 -.->trem
    
    Ini@{shape: braces, label: "I::Тслом = 100"} -.- h1
end

subgraph TRACK2 [Трек МАСТЕР]
    direction TB
    
    m1["режим_м:=работа"] ==> m2{"ВРЕМЯ = Траб"}
    m2 ==> m3["режим_м:=отдых"]
    m3 ==> m4{"ВРЕМЯ = Тотд"}
    m4 ==> m5["режим_м:=работа"]
    m5 ==> m1
    
    m6{"мастер<br>=?"} ==>|"занят"| m7["Трем:=Трем+<br>Траб-ВРЕМЯ"]
    m6 ==>|"свободен"| m8["продолжить"]
    
    m1 ==> m6
    m7 ==> m2
    m8 ==> m5
    
    m1 -.->par_regim_m((режим_м))
    m3 -.->par_regim_m
    m2 -.->time
    m4 -.->time
    m6 -.->master
    m7 -.->trem
    m7 -.->time
    
    Ini_m@{shape: braces, label: "I::Траб = 9ч<br>I::Тотд = 2ч"} -.- m1
end

classDef cond fill:#bee,stroke:#aaa,stroke-width:1px;
classDef state fill:#9e8,stroke:#333,stroke-width:1px;
classDef navig fill:#eda,stroke:#333,stroke-width:1px;

class h2,h4 cond;
class h1,h3,h5 state;
class m2,m4 cond;
class m1,m3,m5,m7,m8 state;
class m6 navig;

style title_total fill:yellow,stroke:red;
style master fill:#fae,stroke:#bbb,stroke-width:2px;
style time fill:#9cf,stroke:#159,stroke-width:2px;
style trem fill:#ccc,stroke:#555,stroke-width:2px;
style par3 fill:#fcc,stroke:#111,stroke-width:2px;
style par_regim_p fill:#fcc,stroke:#111,stroke-width:2px;
style par_regim_m fill:#fcc,stroke:#111,stroke-width:2px;

click master href "https://iu5.bmstu.ru" "переход для Мастера" _blank
click trem href "https://www.google.com" "параметр Трем" _blank
click time href "https://mermaid.js.org" "глобальное время" _blank
