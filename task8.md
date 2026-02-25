Как обычно, не во всем уверен, но - Получившиеся (даже ведь вроде бы что-то получилось) АСД по сформированным на предыдущем шаге кластерам (модулям):

```java
package org.skillsmart.core;

public abstract class GameEngine {

    // конструкторы

    // команды

    // Запуск игры
    // предусловие: игра не запущена (статус "NOT_STARTED")
    // постусловие: игра запущена (статус "USER_TURN_WAIT")
    public abstract void start();

    // окончание игры
    // предусловие: игра запущена (статус из блока "действующих" статусов)
    // постусловие: игра не запущена /или остановлена/ (статус "NOT_STARTED" или "ENDED")
    public abstract void stop();

    // обработка хода игрока
    // предусловие: статус (пока так думаю) ход в обработке (статус "TURN_IS_CHECKING")
    // постусловие: статус ожидание хода или игра завершена (статус "USER_WAITING" или "ENDED")
    public abstract void userTurnProcess(FieldCoordinate coordinateFrom, FieldCoordinate coordinateTo);

    // запросы

    // получение статуса игры
    // предусловие:
    public abstract int getGameStatus();

    // проверка хода игрока
    // предусловие: статус ход в обработке "TURN_IS_CHECKING"
    public abstract boolean isTurnValid(FieldCoordinate coordinateFrom, FieldCoordinate coordinateTo);

    // проверка кончились ли ходы игрока
    // предусловие: игра запущена
    public abstract boolean isGameOver();
}
```

```java
package org.skillsmart.control;

public abstract class CommandHandler {

    // конструкторы

    // команды

    // выполнить команду
    // предусловие:
    // постусловие:
    public abstract void execute(GameCommand command);

    // отменить последний ход
    // предусловие: статус "TURN_IS_CHECKING"
    // постусловие: статус "USER_WAITING"
    public abstract void undoLastTurn();

    // запросы
    // получить историю команд
    public abstract GameCommand[] getCommandHistory();

}

public abstract class GameCommand {

    // конструкторы

    // команды

    // выполнить команду
    // предусловие:
    // постусловие: в историю внесена запись о выполненной команде
    public abstract void execute();

    // отменить команду
    // предусловие: в истории присутсвует хотя бы одна запись о выполненной команде.
    // постусловие: из истории удалена последняя запись о команде
    public abstract void undo();

    // запросы
    // предусловие:

}
```

```java
package org.skillsmart.field;

public abstract class GameField {

    // конструкторы

    // команды

    // Заполнить поле
    // предусловие: на поле есть пустые клетки
    // постусловие: сгенерированы новые элементы, поле заполнено и "спокойно" (статус "USER_WAITING")
    public abstract void fill();

    // Перераспределить элементы на поле, чтобы появился хотя бы один ход, создающий комбинацию
    // предусловие: на поле нет пустых клеток; на поле нет хода, чтобы создать комбинацию
    // постусловие: есть ход, дающий комбинацию, статус "USER_WAITING"
    public abstract void refill();

    // запросы

    // возможен ли ход (есть ли комбинации на поле)
    // предусловие: поле заполнено
    public abstract boolean is_turn_possible();

}

public abstract class GameFieldElement {

    // конструкторы
    // постусловие: сгенерирован игровой элемент
    //public GameFieldElement();
    // команды

    // разместить элемент на поле
    // предусловие:
    // постусловие: элементу присвоены координаты на поле
    public abstract void setElementToField(FieldCoordinate coordinate);

    // удалить элемент с поля
    // предусловие: элементу присвоены координаты на поле
    // постусловие: элемент удален (координаты элемента очищены/удален вообще из памяти)
    public abstract void removeElementFromField();

    // запросы

    // получить тип элемента
    // предусловие:
    public abstract GameElementType gameElementType();

    // получить координаты элемента
    // предусловие:
    public abstract FieldCoordinate getElementCoordinate();

}
```

```java
package org.skillsmart.rules;

public abstract class Combinations {

    // конструкторы

    // команды

    // удаление комбинации
    // предусловие: на поле есть комбинации
    // постусловие: элементы поля очищены и/или заменены на бонус
    public abstract void delete_combination(Figure combination);

    // запросы

    // определить, есть ли хотя бы одна комбинация на поле
    // предусловие: поле заполнено (статусы "refilled", "ход сделан")
    public abstract boolean has_matches();

    // получить все комбинации на поле
    // предусловие: на поле есть комбинации
    public abstract Figure[] get_combinations();

    // получить очки за комбинацию
    // предусловие: на поле есть комбинации
    public abstract long getScore(Figure combination);

}
```

```java
package org.skillsmart.data;

public abstract class ScoreCounter {

    // конструкторы

    // команды

    // добавить очки
    // предусловие:
    // постусловие: сумма очков увеличилась на score
    public abstract void addScore(long score);

    // отобрать очки (возможно за что-то)
    // предусловие:
    // постусловие: сумма очков уменьшилась на score, но не меньше 0
    public abstract void removeScore(long score);

    // сбросить очки
    // предусловие:
    // постусловие: сумма очков уменьшилась до 0
    public abstract void resetScore();

    // запросы

    // получить очки
    // предусловие:
    public abstract long getScore();

}

public abstract class GameStatistics {

    // конструкторы

    // команды

    // записать статистическую информацию
    // предусловие: 
    // постусловие: к статистике добавлена новая информация
    public abstract void appendStat(StatObj stat);

    // очистить статистическую информацию
    // предусловие: 
    // постусловие: статистика обнулена
    public abstract void deleteStat();

    // запросы
    // получить статистическую информацию
    // предусловие: 
    public abstract StatObj[] getStat(StatObj stat);

}
```