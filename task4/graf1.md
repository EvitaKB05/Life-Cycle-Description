# Операторно-параметрическая схема "Прибор-Мастер"

## Общая модель ремонта прибора

```mermaid
flowchart TD
title_total[<em>ОПС модели ремонта ПРИБОР-МАСТЕР</em>]

subgraph TRACK1 [Трек ПРИБОР]
    direction TB
    
    h1(["BPEMЯ=Tслом"]) ==> h2["сост:=<br>сломан"]
    h2 e11@==> h3(["режим=<br>работа"])
    h3 ==> h4(["мастер<br>=своб"])
    h4 ==> h5["мастер:=занят<br>Tрем:=funс(x)"]
    h5 e20@==> h6(["ВРЕМЯ=Трем"])
    h6 ==> h7["сост:=рабочий<br>мастер:=своб<br>Tслом:=func(x)"]
    h7 e10@==> h1
    
    h2 -.->par3((сост))
    h7 -.->par3
    par1((режим))-.->h3
    par2((мастер))-.->h4
    h5 -.->par2
    h7 -.->par2
    h5 -.->par4((Трем_П))
    par4 -.-> h6
    
    Ini@{shape: braces, label: "I::"} -.- h1
    HTf@{shape: braces, label: "I::Tслом = 100"} -.- h1
    
    e10@{ curve: linear}
    e11@{ curve: natural}
    e20@{ curve: stepAfter}
end

subgraph TRACK2 [Трек МАСТЕР]
    direction TB
    
    m1(["ВРЕМЯ=Траб"]) ==> m2["режим:=работа"]
    m2 ==> m3(["ВРЕМЯ=Тотд"])
    m3 ==> m4["режим:=отдых"]
    m4 ==> m5(["ВРЕМЯ=Траб"])
    m5 ==> m1
    
    m2 -.- par_regim((режим))
    m4 -.- par_regim
    par_master((мастер)) -.- m2
    par_master -.- m4
    
    m15{"мастер<br>=..."} ==>|"занят"| m16["Трем:=Трем+<br>Траб-ВРЕМЯ"]
    m15 ==>|"свободен"| m11["режим:=работа"]
    
    m2 ==> m15
    m16 ==> m3
    m11 ==> m2
    
    m16 -.- par_trem_m((Трем_М))
    
    Ini_m@{shape: braces, label: "I::Траб = 9ч"} -.- m1
end

par_master -.- h5
par_master -.- h7
par_trem_m -.- m16
par4 -.- m16

classDef cond fill:#bee,stroke:#aaa,stroke-width:1px;
classDef state fill:#9e8,stroke:#333,stroke-width:1px;
classDef navig fill:#eda,stroke:#333,stroke-width:1px;

class h1,h3,h6 cond;
class h2,h5,h7 state;
class m1,m3,m5 cond;
class m2,m4,m16,m11 state;
class m15 navig;

style title_total fill:yellow,stroke:red;
style par1 fill:#fcc,stroke:#111,stroke-width:2px;
style par2 fill:#fae,stroke:#bbb,stroke-width:2px;
style par4 fill:#ccc,stroke:#555,stroke-width:2px;
style par_regim fill:#fcc,stroke:#111,stroke-width:2px;
style par_master fill:#fae,stroke:#bbb,stroke-width:2px;
style par_trem_m fill:#ccc,stroke:#555,stroke-width:2px;

click par2 href "https://iu5.bmstu.ru" "переход для Мастера" _blank
click par4 href "https://www.google.com" "параметр Трем прибора" _blank
click par_master href "https://mermaid.js.org" "документация" _blank
