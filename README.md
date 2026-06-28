# lab-2-itmo-java

Лабораторная работа по ООП (ИТМО) на тему **наследования и полиморфизма**, выполненная с использованием учебного фреймворка `ru.ifmo.se.pokemon` — симулятора покемон-баттлов, в котором студент описывает покемонов и их атаки, а фреймворк сам разыгрывает бой между двумя командами.

## Идея задания

Нужно построить несколько иерархий классов покемонов (`Pokemon` → подвид → эволюция), наделить их статами, типами и атаками (`Move`), а затем устроить между ними бой. Вся логика самого боя (порядок ходов, расчёт урона, статусы) реализована во фреймворке — задача лабораторной — корректно смоделировать данные через наследование.

## Структура проекта

```
src/lab2_work/
├── batlle.java          # точка входа: формирует команды и запускает бой
├── mypokemons/           # иерархии покемонов
│   ├── Ralts.java        → Kirlia.java → Gardevoir.java
│   ├── Tyrunt.java       → Tyrantrum.java
│   └── Sableye.java
└── attack/               # атаки (Move) покемонов
    ├── DrainingKiss, DazzlingGleam, Charm, EnergyBall      (линия Ralts)
    ├── DragonClaw, Crunch, TailWhip, BrutalSwing           (линия Tyrunt)
    └── FoulPlay, RockTomb, Leer, NightShade                (Sableye)
```

### Иерархии покемонов

| Линия эволюции | Тип(ы) | Атаки на каждой стадии |
|---|---|---|
| `Ralts` → `Kirlia` → `Gardevoir` | Fairy / Psychic | `Ralts`: DrainingKiss, DazzlingGleam · `Kirlia`: +Charm · `Gardevoir`: +EnergyBall |
| `Tyrunt` → `Tyrantrum` | Rock / Dragon | `Tyrunt`: DragonClaw, Crunch, TailWhip · `Tyrantrum`: +BrutalSwing |
| `Sableye` | Dark / Ghost | FoulPlay, RockTomb, Leer, NightShade |

Каждый потомок вызывает конструктор родителя (`super(name, lvl)`), переопределяет статы через `setStats(...)` и добавляет новую атаку через `addMove(...)`, не теряя унаследованные — классический пример полиморфизма через `extends`.

Каждая атака — отдельный класс, наследующий `PhysicalMove` (физический урон) либо `StatusMove` (статус-эффект, например `Charm` снижает атаку противнику через `setMod(Stat.ATTACK, -2)`), с переопределённым `describe()` для текстового вывода во время боя.

### Точка входа — `batlle.java`

```java
Battle b = new Battle();
b.addAlly(p4); b.addAlly(p1); b.addAlly(p2);
b.addFoe(p6);  b.addFoe(p3);  b.addFoe(p5);
b.go();
```

Команда союзников: `Ralts`, `Sableye`, `Tyrunt`. Команда противников: `Gardevoir`, `Tyrantrum`, `Kirlia`. Бой запускается вызовом `b.go()` — дальнейшая симуляция (выбор атак, расчёт урона, лог боя) полностью на стороне фреймворка.

## Сборка и запуск

Проект оформлен как IntelliJ IDEA модуль (`lab2.iml`). Для запуска нужно:

1. Подключить к classpath библиотеку `ru.ifmo.se.pokemon` (предоставляется курсом ИТМО отдельно, в репозитории её нет).
2. Открыть проект в IntelliJ IDEA и запустить `lab2_work.batlle.main`.

Либо вручную через `javac`/`java`, указав путь к jar-файлу библиотеки в `-cp`.
