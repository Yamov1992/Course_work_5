# Курс 5. Курсовая

![male-g53127fd07_1280.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eb711335-95c2-416d-b37f-9b4348705c25/male-g53127fd07_1280.png)

**Привет, дорогие друзья!**

Мы подошли к заключительному этапу 5-го курса — курсовой работе.

Здесь мы закрепим навыки по ООП, типизации данных, использованию датаклассов и паттернов, а также описанию Docker-контейнеров, настройке nginx и Gunicorn.

<aside>
💡 ВАЖНО! Сначала **изучите материалы всех пройденных уроков**, а только потом приступайте к выполнению курсовой работы.

</aside>

**Краткое описание задачи**

Задание будет представлять собой маленькую игру с веб-интерфейсом о битве героев в стиле олдскульных браузерных игр.

### Шаблон для выполнения задания

[GitHub - skypro-008/coursework_5](https://github.com/skypro-008/coursework_5)

Е*сли возникли сложности - в проекте есть папка “help files” которая поможет вам сориентироваться с чего начать*

## **Знания для разработки проекта**

- [ ]  Основы объектно-ориентированного программирования.
- [ ]  Работа с датаклассами.
- [ ]  Навыки функционального программирования, 
использование библиотек marshmallow и marshmallow_dataclasses.
- [ ]  Основы типизации данных.
- [ ]  Работа с данными в  формате JSON.
- [ ]  Основы работы с Flask, использование шаблонов во Flask.
- [ ]  Основы работы с Docker и nginx

## **Этапы разработки проекта**

- Разработка логики проекта.
- Подключение Flask-приложения.

## Как работает готовый проект – “флоу” проекта

1. При переходе на основную страничку с игрой пользователь видит название и кнопку **«Начать игру»** (используем шаблон `index.html`).
2. При нажатии кнопки «Начать игру» пользователь переходит по адресу `/choose-hero/`.

На экране появляется форма, которая принимает следующие данные 
(используем шаблон `choose-hero.html`):

- Название страницы (тег `h2`) («Выберите героя» или «Выберите врага»).
- Предложение ввести имя героя (тег `input`).
- Предложение выбрать класс героя (тег`select`), сформированный на основе названий классов, доступных в программе.
- Предложение выбрать оружие (тег`select`), сформированный на основе имеющихся видов оружия, доступных в программе (исходя из сведений файла `equipment.json`, находится в папке data).
- Предложение выбрать броню (тег`select`), сформированный на основе имеющихся видов брони, доступных в программе (также исходя из сведений файла `equipment.json`).
- Кнопка для отправки заполненных данных.
    
    ![Screenshot 2022-01-08 at 18.23.21.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7734d67c-d376-4dde-8458-f2498b2cfc50/Screenshot_2022-01-08_at_18.23.21.png)
    
1. После отправки данных появляется аналогичная форма для выбора параметров противника (используем шаблон `choose_hero.html`).
2. После того, как основные параметры выбраны, начинается самое интересное!
(Используем шаблон `fight.html`.)

Вторая кнопка для отправки данных перенаправляет нас по адресу `/fight/`, и появляется экран битвы:

![Экран битвы.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/de67b369-96b1-46bb-9797-b3dcf61c347e/Экран_битвы.png)

Здесь мы видим данные о герое (слева) и данные о противнике (справа). Данные как о герое, так и о противнике делятся на блоки. Рассмотрим их более подробно. 

![Экран битвы 2.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12210223-d67b-4a53-aa32-d07b37053ed7/Экран_битвы_2.png)

### Характеристики персонажей

Персонажи в данной игре будут иметь три блока с характеристиками:

- Характеристика класса персонажа.
    
    **Характеристика класса персонажа включает в себя:**
    
    - Название (Вор, Воин) (name).
    - Максимальное число очков здоровья (max_health).
    - Максимальное число очков выносливости (max_stamina).
    - Модификатор атаки (attack).
    - Модификатор защиты (armor).
    - Модификатор выносливости (stamina).
    - Умение (skill).
    
    * Подробнее предлагаемые классы можно посмотреть [***здесь](https://www.notion.so/5-a322e6b0b9df49bf8fbcb822cc2f90fe).***
    
    *Кроме того, в характеристике класса имеется подблок «Умение», который характеризуется тремя переменными:*
    
    - имя,
    - урон,
    - требуемая выносливость.
    
    * Подробнее предлагаемые умения можно посмотреть ***[здесь](https://www.notion.so/5-a322e6b0b9df49bf8fbcb822cc2f90fe)***
    
- Характеристика оружия персонажа.
    
    ***Характеристика оружия персонажа включает в себя:***
    
    - Название (name).
    - Минимальный урон (min_damage).
    - Максимальный урон (max_damage).
    - Количество затрачиваемой выносливости за удар (stamina_per_hit).
    
    Данные с предлагаемым оружием хранятся в формате JSON в файле equipment.json и выглядят так:
    
    ```json
    {
        "id": 1,
        "name": "топорик",
        "min_damage": 2.5,
        "max_damage": 4.1,
        "stamina_per_hit": 2
    }
    ```
    
- Характеристика брони персонажа.
- ### **Механика боя**

Бой состоит из ходов. В течение 1 хода игрок может нанести удар, воспользоваться умением или пропустить ход.

Каждый новый ход выносливости игроков увеличивается на 3 ед. (Не может превышать максимальное значение.)

Бой происходит с компьютерным противником. После действия игрока компьютер отвечает тем же. 

Как игрок, так и компьютер могут использовать умение только один раз за бой.

Когда компьютер выполняет ответное действие, он имеет 10%-ный шанс воспользоваться умением. Если шанс срабатывает, а умение уже было применено, то компьютер наносит обычный удар.

Битва окончена, если у одного из персонажей очки здоровья меньше нуля.

Предполагается 3 исхода битвы:

- Игрок победил.
- Игрок проиграл.
- Ничья.

 

Экран имеет 4 кнопки.

### 1. **Кнопка «Нанести удар»**

При нажатии на кнопку происходит проверка, достаточно ли у персонажа выносливости, чтобы нанести удар. Если выносливости недостаточно, пропускаем ход. Если достаточно, тогда переходим к расчету количества урона.
Урон рассчитывается следующим образом:
`УРОН = УРОН_АТАКУЮЩЕГО - БРОНЯ_ЦЕЛИ`

Формула для расчета значения БРОНЯ_ЦЕЛИ приведена [ниже](https://www.notion.so/5-a322e6b0b9df49bf8fbcb822cc2f90fe).

`урон_от_оружия = случайное число в диапазоне (min_damage - max_damage)`
`УРОН_АТАКУЮЩЕГО = урон_от_оружия * модификатор_атаки_класса`

Далее перед тем, как рассчитать броню цели, необходимо провести проверку на выносливость. 
Как мы помним, броня имеет характеристику `stamina_per_turn`.

Если у персонажа, которого атакуют, достаточно очков выносливости для использования своей брони, тогда рассчитываем показатель брони цели следующим образом:

       `БРОНЯ_ЦЕЛИ = надетая_броня * модификатор_брони_класса`

Если у персонажа, которого атакуют, очков выносливости для использования своей брони недостаточно, тогда его броня игнорируется.
После того, как все расчеты произведены, вычитаем из очков здоровья цели показатель рассчитанного урона.

**Как рассчитывается выносливость в бою**
В каждом бою у нас будет задана константа, сколько очков выносливости персонаж восстанавливает за ход.

Таким образом, в целом расчет выносливости выглядит следующим образом:

- У атакующего вычитаем количество затраченной выносливости на удар (`stamina_per_hit`).
- У защищающегося вычитаем количество затраченной выносливости за защиту (`stamina_per_turn`).

**Восстанавливаем выносливость атакующего и защищающего:**

- Прибавляем к очкам выносливости атакующего константу, умноженную на модификатор выносливости **атакующего.**
- Прибавляем к очкам выносливости цели константу, умноженную на модификатор выносливости **цели.**

После того, как все расчеты произведены, компьютерный противник наносит ответный удар, который рассчитывается аналогичным образом.

**2 ~~простых~~ примера механики боя:**

- **Пример №1**
Представим, что ваш герой наносит удар по врагу.
    
    > Характеристики вашего героя:
    > 
    > 
    > `очки выносливости героя` 10/10
    > `модификатор выносливости` = 0.9
    > 
    > `модификатор атаки` = 1.1
    > 
    > **Параметры оружия:**
    > 
    > `min_damage` = 2.2
    > `max_damage` = 3.1
    > `stamina_per_hit` = 2
    > 
    > (остальные показатели для расчета неважны)
    > 
    
    > Характеристики противника:
    > 
    > 
    > `очки выносливости врага` 10/10
    > `очки здоровья врага` 10/10
    > 
    > `модификатор выносливости` = 1.1
    > 
    > `модификатор брони`  = 0.8
    > **Параметры брони противника:**
    > `defence` = 2
    > 
    > `stamina_per_turn` = 1.5
    > (остальные показатели для расчета неважны)
    > 
    
    **Расчеты**
    
    Выполняем проверку выносливости атакующего:
    10 больше, чем  2 (`stamina_per_hit`), считаем дальше.
    
    `Урон от оружия = 2.4 (случайное значение от 2.2 до 3.1)`
    
    умножаем на `модификатор атаки` класса, в итоге получаем:
    
    `урон атакующего = 2.4 * 1.1 = 2.6`  (*округлено до одного знака после запятой*).
    
    Выполняем проверку на выносливость цели:
    
    10 больше, чем 1.5 (`stamina_per_turn`), считаем дальше:
    
    `БРОНЯ_ЦЕЛИ = 2 * 0.8 = 1.6`,
    
    где:
    
    `2` — параметр защиты брони,
    
    `0.8` — модификатор брони класса,
    
    `1.6` —  броня цели с учетом модификатора класса.
    
    `УРОН = 2.6 - 1.6 = 0.8`,
    
    где:
    `2.6` — урон атакующего,
    
    `1.6` — броня цели,
    `0.8` — итоговый урон (запомним это значение).
    
    **Рассчитываем выносливость**
    
    Константа (базовое восстановления выносливости за ход) = 1.
    
    **Как в течение хода изменится выносливость нашего героя (который атакует)**
    
    Герой обладает полной выносливостью (10/10),
    выносливость после действия  = `10 - 2 + 1 * 0.9` = `8.9`,
    
    где:
    
    `10` — общее количество выносливости,
    
    `2` — выносливость для удара (`stamina_per_hit`),
    `1` — константа (базовое восстановление выносливости за ход),
    
    `0.9` — модификатор выносливости класса,
    
    `8.9` — выносливость персонажа после действия.
    
    **Как в течение хода изменится выносливость противника (героя, который защищается)**
    
    Противник обладает выносливостью (10/10),
    
    выносливость после действия  = `10 - 1.5 + 1 * 1.1` = `9.6`,
    
    где:
    
    `10` — общее количество выносливости,
    
    `1.5` — выносливость для защиты (`stamina_per_turn`),
    `1` — константа (базовое восстановление выносливости за ход),
    
    `1.1` — модификатор выносливости класса,
    
    `9.6` — выносливость персонажа после действия.
    
    В итоге характеристики врага после нанесения удара выглядят так:
    
    > очки здоровья врага: 9.2/10   `(10 - 0.8)`
    очки выносливости врага: 9.6/10 `(10 - 1.5 + 1 * 1.1)`
    > 
    
    Как изменятся характеристики нашего героя:
    
    > очки выносливости нашего героя: 8.9/10 `(10 - 2 + 1 * 0.9)`
    > 
    
    После этой логики следует ответный удар от врага, и все расчеты повторяются.
    
- **Пример № 2**
    
    > Характеристики вашего героя:
    > 
    > 
    > `очки выносливости героя` 10/10
    > `модификатор выносливости` = 0.9
    > 
    > `модификатор атаки` = 1.1
    > 
    > **Параметры оружия:**
    > 
    > `min_damage` = 2.2
    > `max_damage` = 3.1
    > `stamina_per_hit` = 2
    > 
    > (остальные показатели для расчета не важны)
    > 
    
    > Характеристики противника (теперь у противника не осталось выносливости):
    > 
    > 
    > `очки выносливости врага` 0.8/10
    > `очки здоровья врага` 10/10
    > 
    > `модификатор выносливости` = 1.1
    > 
    > `модификатор брони`  = 0.8
    > **Параметры брони противника**
    > `defence` = 2
    > 
    > `stamina_per_turn` = 1.5
    > (остальные показатели для расчета не важны)
    > 
    
    **Расчеты**
    
    Выполняем проверку выносливости атакующего:
    10 больше, чем  2 (`stamina_per_hit`), считаем дальше.
    
    `Урон от оружия = 2.4 (случайное значение от 2.2 до 3.1)`
    
    умножаем на `модификатор атаки` класса, в итоге получаем:
    
    `урон атакующего = 2.4 * 1.1 = 2.6`  (*округлено до одного знака после запятой*).
    
    Выполняем проверку на выносливость цели:
    
    0.8 меньше, чем 1.5 (`stamina_per_turn`) — броня цели игнорируется:
    
    `БРОНЯ_ЦЕЛИ = 0`
    
    `УРОН = 2.4 - 0 = 2.4`,
    
    где:
    `2.4` — урон от оружия,
    
    `0` — броня цели (игнорируется из-за нехватки выносливости),
    `2.4` — итоговый урон (запомним это значение).
    
    **Рассчитываем выносливость**
    
    Константа (базовое восстановления выносливости за ход) = 1.
    
    **Как в течение хода изменится выносливость нашего героя (который атакует)**
    
    Герой обладает полной выносливостью (10/10),
    выносливость после действия  = `10 - 2 + 1 * 0.9` = `8.9`,
    
    где:
    
    `10` — общее количество выносливости,
    
    `2` — выносливость для удара (`stamina_per_hit`),
    `1` — константа (базовое восстановление выносливости за ход),
    
    `0.9` — модификатор выносливости класса,
    
    `8.9` — выносливость персонажа после действия.
    
    **Как в течение хода изменится выносливость противника (героя, который защищается)**
    
    Противник обладает выносливостью (10/10),
    
    выносливость после действия  = `0.8 + 1 * 1.1` = `1.9`,
    
    где:
    
    `0.8` — общее количество выносливости,
    `1` — константа (базовое восстановление выносливости за ход),
    
    `1.1` — модификатор выносливости класса,
    
    `1.9` — выносливость персонажа после действия.
    
    В итоге характеристики врага после нанесения удара выглядят так:
    
    > очки здоровья врага: 7.6/10   `(10 - 2.4)`
    очки выносливости врага: 1.9/10 `(0.8 + 1 * 1.1)`
    > 
    
    Как изменятся характеристики нашего героя:
    
    > очки выносливости нашего героя: 8.9/10 `(10 - 2 + 1 * 0.9)`
    > 
    
    После этой логики следует ответный удар от врага, и все расчеты повторяются
    
- **Видео проекта в действии**
    
    [Флоу в действии.mov](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5eba7b90-b2a5-468d-b8bf-8ca8d90e1a47/Флоу_в_действии.mov)
    

### 2. **Кнопка «Использовать умение»**

При использовании умения происходит только проверка, достаточно ли выносливости у персонажа, использующего умение. Все остальные проверки игнорируются.

Если проверка пройдена успешно, тогда вычитаем очки здоровья цели в соответствии с примененным умением.

### 3. **Кнопка «Пропустить ход»**

Игрок пропускает ход. После чего компьютерный противник совершает действие.

### 4. **Кнопка «Завершить бой»**

Переходим на страницу с кнопкой «Начать игру» и начинаем всё сначала =)
