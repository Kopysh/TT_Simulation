# TT_Simulation

# Проект “Cимуляция”
Суть проекта - пошаговая симуляция 2D мира, населённого травоядными и хищниками. Кроме существ, мир содержит ресурсы (траву), которым питаются травоядные, и статичные объекты, с которыми нельзя взаимодействовать - они просто занимают место.

2D мир представляет из себя матрицу NxM, каждое существо или объект занимают клетку целиком, нахождение в клетке нескольких объектов/существ - недопустимо.

# Дизайн классов
**Entity**
Корневой абстрактный класс для всех существ и объектов существующих в симуляции.

**Grass, Rock, Tree**
Rock, Tree - статичные объекты. Grass - ресурс для травоядных.

**Creature**
Абстрактный класс, наследуется от Entity. Существо, имеет скорость (сколько клеток может пройти за 1 ход), количество HP. Имеет абстрактный метод makeMove() - сделать ход. Наследники будут реализовывать этот метод каждый по-своему.

**Herbivore**
Травоядное, наследуется от Creature. Стремятся найти ресурс (траву), может потратить свой ход на движение в сторону травы, либо на её поглощение.

**Predator**
Хищник, наследуется от Creature. В дополнение к полям класса Creature, имеет силу атаку. На что может потратить ход хищник:

Переместиться (чтобы приблизиться к жертве - травоядному)
Атаковать травоядное. При этом количество HP травоядного уменьшается на силу атаки хищника. Если значение HP жертвы опускается до 0, травоядное исчезает
Map
Карта, содержит в себе коллекцию для хранения существ и их расположения. Советую не спешить использовать двумерный массив или список списков, а подумать какие ещё коллекции могут подойти.

**Simulation**
Главный класс приложения, включает в себя:

- Карту
- Счётчик ходов
- Рендерер поля
- Actions - список действий, исполняемых перед стартом симуляции или на каждом ходу (детали ниже)

Методы:
- nextTurn() - просимулировать и отрендерить один ход
- startSimulation() - запустить бесконечный цикл симуляции и рендеринга
- pauseSimulation() - приостановить бесконечный цикл симуляции и рендеринга

**Actions**
Action - действие, совершаемое над миром. Например - сходить всеми существами. Это действие итерировало бы существ и вызывало каждому makeMove(). Каждое действие описывается отдельным классом и совершает операции над картой. Симуляция содержит 2 массива действий:
- initActions - действия, совершаемые перед стартом симуляции. Пример - расставить объекты и существ на карте
- turnActions - действия, совершаемые каждый ход. Примеры - передвижение существ, добавить травы или травоядных, если их осталось слишком мало

**Поиск пути**
Советую писать алгоритм поиска пути полностью с нуля, используя в качестве источника описание алгоритма на википедии. Проще всего начать с алгоритма поиска в ширину. Он относительно простой в реализации, но может работать медленно на больших полях, для которых лучше подойдет алгоритм A*.

**Рендерер**
Рендерер ответственен за визуализацию состояния поля, его отрисовку. По желанию студента интерфейс приложения может быть консольным, либо графическим).

**Конечная цель**
Реализовать симуляцию и подобрать различные значения так, чтобы взаимодействия внутри мира получились максимально интересными:

**Размер поля**
- Диапазоны HP и скорости существ
- Диапазон атаки хищников

**Опциональные идеи для усложнения проекта:**
- Механика размножения существ
- Механика голода, когда от отсутствия пищи у них начинает уменьшаться HP
