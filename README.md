# Учебное пособие по языку программирования Nim [альфа версия]

- [Официальный сайт Nim](https://nim-lang.org/ "Язык программирования Nim.")
- [Репозиторий на GitHub](https://github.com/nim-lang/Nim "GitHub")

>Код в текущем пособии был проверен на Nim 0.18.0

>26 сентября 2018 года вышла новая версия Nim 0.19.0. По этому мануал будет обновлен. [30.09.2018]

### Для кого создано это пособие
Данное пособие предназначено для опытных программистов.
Люди, которые только встали на этот тернистый путь, 
могут не понять некоторые аспекты, которые не будут раскрываться в тексте.

### Устновка компилятора
Для Nim есть удобная утилита [choosenim](https://github.com/dom96/choosenim) (для Windows/Linux/macOS), с помощью которой можно легко устанавливать или обновлять Nim.

Для Linux/macOS достаточно выполнить эту команду (убедитесь, что у вас есть curl и gcc):
```bash
curl https://nim-lang.org/choosenim/init.sh -sSf | sh
```

Теперь добавьте к переменной PATH значение `~/.nimble/bin` в файл запуска вашего шелла (`~/.bashrc` для Bash, `~/.zshrc` для Zsh).

Чтобы убедиться, что вы успешно установили `choosenim` и `nim`, выполните и проверьте, что обе команды вывели свои версии.
```bash
choosenim -v && nim -v
```

### Комментарии

Однострочный комментарий.
```nim
# какой-то комментарий
```
Многострочный комментарий.
```nim
#[
комментарий
на несколько
строк
]#
```

Вложенные мнострочные комментарии.
```nim
#[
вложенный
    #[комментарий]#
    #[на #[несколько]# ]#
строк
]#
```



### Типы данных

`char` - ASCII символ
```nim
var ch: char = 'a'
var tab: char = '\t'
```

`string` - строка, выделяется двойными или тройными кавычками
```nim
var login: string = "wtf"
var mydata: string = """Эта строка - 
многострочная, так как мы использовали тройные кавычки
"""
```

`int` - целое число. Для удобства записи больших чисел можно использовать символ `_` (он игнорируются)
```nim
var age: int = 45
var mysalary  = 1_000_000
```

`uint` - беззнаковое целое.
```nim
var bigNumber: uint = 1999999999999999 
```

`float` - число с плавающей запятой
```nim
var price: float = 45.45
```

`bool` - булев тип, может принимать значения true/false
```nim
var good: bool = true
var bad: bool = false
```

По умолчанию `int` и `float` являются платформозависимыми типами - если вы скомпилируете 32-битную программу, то int, uint и float будут равны типам `int32`, `uint32` и `float32`, 64-битную - `int64`, `uint64` и `float64` соответственно. Если вам необходимо явно указывать размер типов - используйте эти типы. Кроме этих размеров так же доступны `int8`, `uint8`, `int16`, `uint16`

### Переменные
В Nim используется статическая типизация переменных, поэтому у переменной должен быть только один определённый тип, известный во время компиляции.

Существует несколько типов переменных:

`var` - переменная, которую можно изменять во время выполнения программы. Рекомендуется называть такие переменные в `camelCase` стиле:
```nim
var myLogin: string = "valgala"
var myPassword = "mypass" # Компилятор может сам выводить типы переменных
myLogin = "valgala2" # Изменили переменную myLogin
```

`const` - константа. Её значение должно быть известно во время компиляции.
Изменять её нельзя. Константам рекомендуется давать имя в `camelCase` или `PascalCase` стиле:
```nim
const myOs = hostOS
const MyData = 15
```

`let` - переменная с одиночным присваиванием. После присваивания её нельзя изменять. Её рекомендуется называть в том же стиле, что и `var` - `camelCase`
```nim
let myint = 5
```

Для переменных можно использовать секции `var`/`let`/`const`. Эти секции должны быть выделены отступами (так как Nim основан на отступах).
```nim
var
  name = "vera"
  age = 20

const
  count = 15
  info = "Something"
```

### Дополнительные типы
В Nim можно объявлять новые типы. Делается это с помощью секции `type`. Рекомендуется называть типы в стиле `PascalCase`.

`range` позволяет определить диапазон значений, среди которых могут быть целочисленные значения, `char` или перечисления (`enum`).
```nim
type
  SubRange = range[1..12]

var t: SubRange = 1 # входит в диапазон
t = 12 # входит в диапазон
t = 15 # Не входит в диапазон, будет ошибка во время компиляции
```

`enum` позволяет создавать новый тип *перечисления*. Значения у этого типа могут быть только из тех, которые были указаны при создании типа.
```nim
type
  # Создаём наш тип EnumType, который представляет из себя enum с
  # возможными значениями one, two и three
  EnumType = enum
    one, two, three

var a: EnumType = three
var b = two
echo a
echo b
```

Любые значения `enum` можно конвертировать в `int`, так как Nim 
автоматически присваивает значениям числа по порядку, начиная с 0:
```nim
type
  EnumType = enum
    one, two, three

var a: enumType = three
var b = two
echo ord(a) # 2
echo ord(b) # 1
```

Можно использовать свои числовые значения для перечислений, но они должны идти от меньшего к большему:
```nim
type
  EnumType = enum
    one = 5, two = 8

var a = one
echo ord(a) # 5
```

К тому же перечислениям можно присваивать строковые значения:
```nim
type
  EnumType = enum
    one = "Какая-то строка", two = 8

var a = one
echo ord(a) # индекс
echo a # строка
```

Или задать сразу число и строку.
```nim
type
  EnumType = enum
    one = (2, "Какая-то строка"), two = 8

var a = one
echo ord(a) # 2
echo a # Какая-то строка
```

`array` - это массив фиксированного размера и одного типа. Размер массива должен быть известен во время компиляции.
```nim
type
  ArrStr = array[0..2, string] # тип arrStr будет массивом из трех строк
  ArrStr2 = array[3, string] # получим тот же результат
    
var myArr: ArrStr = ["asd", "sdf", "sdfg"]
var myArr2: ArrStr2 = ["asd", "sdf", "sdfg", "123"] # Ошибка

```

Массивы можно создавать без указания размера и типа элементов:
```nim
var arr = [1, 2, 3]
```

Получить доступ к определённому элементу можно по его индексу с помощью оператора `[]`:
```nim
var arr = [1, 2, 3]
echo arr[1]
arr[1] = 5
echo arr[1]

# В данном массиве индекс начинается с 1 (т.к 1..3, а не 0..2)
var anotherArr: array[1..3, string] = ["one", "two", "three"]
echo anotherArr[1] # one
```

`seq` - это последовательность. Она похожа на массив, но отличается тем, что её размер можно изменять во время выполнения программы.
Обьявлять последовательности можно с помошью указания `seq` и типа элементов самой последовательности. Последовательность можно создавать с помощю конструкции `@[ ]`.
```nim
type
  MyStrSeq = seq[string] # последовательность из строк
  MyIntSeq = seq[int] # последовательность из чисел

# Создали переменную с типом MyIntSeq, но не присвоили ей значение
var mySeq: MyIntSeq

mySeq = @[] # Присвоили переменной пустую последовательность
mySeq = @[1, 2, 3] # Присвоили последовательность из чисел 1, 2 и 3

# Можно использовать процедуру newSeq для создания последовательности
var anotherSeq = newSeq[int]()

# Компилятор может выводить тип последовательности автоматически
let data = @["my", "data"] 
```

`tuple` - кортеж. В кортеже может быть любое количество полей любого типа, но их количество нельзя изменять. Элементы в кортеже можно получать с помощью оператора индексирования `[]`.
С помощью секции `type` или с помощью синтаксиса (ключ: значение) можно создавать именнованые кортежи, к элементам которых можно обращаться по названию поля через `.`
```nim
type
  # Именованный кортеж
  Human = tuple[name: string, age: int]
    
var
  me: Human = ("Dima", 30)
  friend: Human
  girl: Human

# Обращение по индексу
friend[0] = "Vova"
friend[1] = 35

# Обращение через .
girl.name = "Ira"
girl.age = 25

# Просто кортеж
let myTuple = (1, 2, "hello", 5.5)

echo me, friend, girl, myTuple
```

### Ветвления кода
Ветвления создаются секциями `if`, `elif`, `else`.

Если условие `if` секции истино (равняется `true`), то выполнится код внутри данной секции.
```nim
if 2 == 2:
  echo "Оказывается, 2 равно 2!"
```

Если условие - ложь (`false`), то выполнится код внутри секции `else` (если она есть).
```nim
if false:
  echo "Эта строка никогда не будет выведена"
else:
  echo "А вот эта будет"
```
Если нужна дополнительная секция для другого условия, используется `elif`.
```nim
if false:
  echo "Эта строка никогда не будет выведена"
elif true:
  echo "А вот эта будет"
else:
  echo "Эта секция кода не отработает, так как выполнился код в секции elif"
```
Еще один способ создавать ветвления на основе значений переменных - конструкция `case`. Выполнится только то ветвление, в значения которого входит переменная, по которой происходит ветвление (в данном случае - `myNumber`).

Если ни одно ветвление не подходит, то выполняется блок `else` - он обязательный только тогда, когда у типа может быть любое значение. Блок `else` не нужен, если ветвление проводится по переменной, в тип которой входит ограниченное кол-во значений (к примеру типы `range`, `enum`), и все эти значения обработаны ветвлениями)
```nim
let myNumber = 2

case myNumber 
of 1:
  echo "Нам попался 1!"
of 2, 3, 4:
  echo "Это либо 2, либо 3, либо 4"
else:
  echo "Ни одно ветвление не подошло"

let myData: range[0..5] = 3
case myData
# В case можно создавать ветвление по диапазону значений
# Это работает для чисел, enum, char
of 0..3: echo "Число от 0 до 3"
of 4..5: echo "4 или 5"

# Данная конструкция case не скомпилируется, так как обработаны не все 
# значения переменной myData
case myData
of 5: echo "Это 5!"
```

### Циклы
В Nim существует несколько типов циклов.

`for` - проход по значения какого-либо итератора:
```nim
for i in 20..25:
  echo i
```

- `..` - это итератор, который возвращает значения от начального числа до конечного (включительно). Он является сокращением к итератору `countup`, так как такие циклы встречаются в программах очень часто.
- `for` - ключевое слово, которое объявляет цикл.
- `i` - переменная, куда будет попадать текущее значение итератора.
- `in` - указывает, по какому итератора проходит цикл.

Ключево слово `break` прерывает цикл.
```nim
for i in 20..30:
  if i == 25:
    break
  echo i
```

Ключевое слово `continue` завершает текущую итерацию.
```nim
for i in countup(20,30):
  if i > 25:
    continue
  echo i
```

Пример цикла `while`
```nim
var counter = 1

while counter < 5:
  echo counter
  inc counter
```

- `while` выполняет тело цикла пока условие не станет равно `false`.
- `inc` - добавляет 1 к переменной counter.

### Процедуры
Процедуры обозначаются ключевым словом `proc`. Рекомендовано называть их в `camelCase` стиле.

Их можно вызывать с помощью `()`:
```nim
proc printSomething =
  echo "la la la"
    
printSomething()
```

Указать *аргументы*, которые принимает процедура, можно, добавив их в скобки после названия процедуры. Нужно указывать название аргумента и его тип.
Такие аргументы - обязательны, без их указания процедуру нельзя будет вызвать.
```nim
proc printSomething(myString: string) =
  echo myString
    
printSomething("Nim - классный язык программирования!")
```

Аргументы *по умолчанию* добавляются после знака `=`. 
Теперь, если данный аргумент не был передан в процедуру, будет использоваться значение по умолчанию.
```nim
# При использовании значения по умолчанию тип указывать не обязательно
proc printSomething(myString = "Математика - супер наука!") =
  echo myString

printSomething()
```

Процедура может принимать *несколько аргументов*, тогда их нужно указывать через ``,`` .
```nim
# myString - обязательный аргумент, needMath - нет (если его не указать),
# то будет использоваться стандартное значение
proc printSomething(myString: string, needMath = "Все знают, что да!") =
  echo myString, needMath

printSomething("Математика нужна?")
```

Пример без значений по умолчанию.
```nim
proc printMe(name: string, year: int) =
  echo "Меня зовут " & name & "."
  echo "Я живу в " & $year & " году."

printMe("Фрай", 3000)
```

- `&` - конкатенация (объединение) строк.
- `$` - оператор преобразования числа в строку.

Можно указать тип сразу для нескольких параметров.
```nim
proc point(x, y, z: int) =
  echo "Координаты в пространстве:"
  echo "x=" & $x
  echo "y=" & $y
  echo "z=" & $z

point(10, 11, 12)
```

Выражение `return` позволяет возвращать что-либо из функции.
Тип возвращаемого значения процедуры обязательно должен быть указан после скобок с аргументами (если они есть), или после названия процедуры
```nim
proc getString: string =
  return "Свободу попугаям!"

echo getString()

proc returnSame(data: int): int = 
  return data

echo returnSame(5) # Выведет 5
```

Кроме того, во всех процедурах, которые что-то возвращают, доступна переменная
*result*. Её можно использовать для того, чтобы указать возвращаемое значение, но не выходить из процедуры (после выполнения кода процедуры переменная *result* автоматически возвратится):
```nim
proc someMath(data: int): int = 
  # Приравниваем результат к аргументу data
  result = data
  # Умножаем результат на 5
  result = result * 5
  if result > 10:
    result = result + 2
  # Прибавляем 3
  result = result + 3

echo someMath(5) # Выведет 30
echo someMath(2) # Выведет 13
```

Можно использовать перегрузку функций (несколько процедур с одинаковым названием, но разными аргументами)
```nim
proc overloadMe(x: int, y: int)=
  echo x, y

proc overloadMe(x: int)=
  echo x

overloadMe(1) 
overloadMe(1, 2)
```

Если у процедуры не указывать название, она будет анонимной: 
```nim
var anonymous = proc(x: string) =
  echo x

anonymous("Я - анонимная процедура :)")
```

Пример *передачи* процедуры в качестве аргумента.
```nim
proc f(): int =
  return 5

proc anotherF(data: float): string = 
  # $ конвертирует float в строку
  return $data

# Передача процедуры без аргументов
proc wrap(fun: proc)=
  echo fun()

proc wrap(fun: proc(data: float): string, info: float) = 
  # Вызываем процедуру fun с аргументом, который нам передали в info
  echo fun(info)

wrap(f)
wrap(anotherF, 5.0)
```

Пример *возврата* процедуры.
```nim
proc get(): proc =
  # В коротких процедурах не обязательно писать "result" или "return"
  # Автоматически возвратится выражение, которое указано в теле процедуры
  proc res(): int = 5
  return res

echo get()() # 5
```

Если мы хотим иметь возможность передавать в одну и ту же процедуру и массив, и последовательность, то можно использовать openArray.

Этот тип данных может использоваться только в качестве аргумента процедуры.
Для openArray нужно обязательно указать тип элементов
```nim
# a будет содержать какое-то количество элементов с типом int
proc echoContainer(a: openArray[int])=
  for i in a:
    echo i

echoContainer(@[1,2,3,4,5]) # Последовательность
echoContainer([1,2,3,4,5]) # Массив
```

Если в процедуру необходимо передавать произвольное число аргументов, то можно
использовать varargs
```nim
# varargs будет содержать элементы типа string
proc printEverything(a: varargs[string])= 
  for i in a:
    echo i

printEverything("as", "sd", "df")
```

varargs позволяет применять ко всем аргументам какую-либо процедуру для того, чтобы привести аргументы разного типа к одному.
```nim
# varargs будет содержать только аргументы типа string, так как все
# другие типы будут сконвертированы в строку при помощи процедуры $
proc convVarargs(a: varargs[string, `$`])= 
  for i in a:
    echo i

convVarargs("as", "sd", 21, 5.0, false)
```

### Итераторы
Если в процедуре заменить `proc` на `iterator` а `return` на `yield`, то получится итератор.
```nim
iterator printItem(): int =
  for i in 1..5:
    yield i
    
for i in printItem():
  echo i
```

### Преобразование типов

[comment]: # (#TODO: сделать эту секцию)
### Исключения
В Nim исключения обрабатываются с помощью секций `try` и `except` (так же, как и в Python). В секцию `try` мы помещаем код, который может вызвать исключение, которое нужно будет обработать. Если происходит исключение, то начинается выполнение кода в секции `except`. В `finally` код выполняется в любом случае.
```nim
try:
  var arr = @[1,2,3]
  echo arr[5]
except:
  echo "У последовательности arr такого индекса нет!"
```

[comment]: # (#TODO: написать о "except Exception as exc" и выводе текста самого исключения)

После `except` мы можем указать, какой тип исключения мы отлавливаем (аналогично Python).
```nim
try:
  var arr = @[1,2,3]
  echo arr[5]
# Отлавливаем только обращения к несуществующему индексу
# Другие ошибки в данном случае не будут пойманы
except IndexError:
  echo "Элемента по такому индексу не существует"
```

Блок `finally` выполняется после `try` и (или) `except`
```nim
try:
  var arr = @[1,2,3]
  echo arr[5]
except IOError:
  echo "Элемента по такому индексу не существует"
finally:
  echo "Я всегда выполняюсь"
```

### Шаблоны

[comment]: # (#TODO: сделать эту секцию)
### Макросы

[comment]: # (#TODO: сделать эту секцию)
### Модули
Nim позволяет разбивать код на модули. Один файл это один модуль. 
Для того, чтобы процедуру/переменную/тип видели другие модули, её нужно пометить с помощью экспортирующего маркера (`*`)
```nim
# Файл one.nim
var name* = "Vova" # Доступна из других модулей
var lastname = "Sidorov" # Недоступна

proc exportedProc* =  # Можно вызывать из других модулей
  echo "exportme"

proc notExportedProc = # Нельзя
  echo "notexportme"
```

Теперь в другом файле вы можете сделать `import one` и пользоваться импортированными переменными, процедурами, типами.
```nim
# Файл two.nim
import one
exportedProc() # Выведет "exportme"
notExportedProc() # Ошибка
```

Если мы не хотим что-либо импортировать из другого модуля, мы можем использовать `except`.
```nim
# Файл two.nim
import one except name # Переменная name не будет импортирована
exportedProc() # Выведет "exportme"
echo name # Ошибка - мы не импортировали name
```

Так же мы можем импортировать только указанные символы с помощью `from`.
```nim
# Файл two.nim
from one import exportedProc
exportedProc()
echo name # Ошибка - мы импортировали только exportedProc
```

При импорте модуля с помощью конструкции ``from module import nil`` доступ к любым символам этого модуля должен быть с указанием названия этого модуля (так же, как в Python при ``import module``)
```nim
# Файл two.nim
from one import nil
one.exportedProc() # Сработает
exportedProc() # Ошибка
```

Можно использовать свои названия для импортированных модулей через `as`.
```nim
# Файл two.nim
# Импортируем модуль one с названием "x" и разрешаем доступ к символам
# только при указании этого имени
from one as x import nil
x.exportedProc()
```

Команда `include` позволяет вставлять содержимое файла в своё местоположение.
Её рекомендуется использовать только тогда, когда большой файл нельзя разделить на модули - тогда можно его разделить с помощью include.
```nim
include one
exportedProc()
```

### Прагмы
Если нам нужно сообщить компилятору какую-либо информацию, то можно воспользоваться прагмами.
Они указываются с помощью фигурных скобок и названия прагмы - `{.somepragma.}`
```nim
# При компиляции - Hint: 'name' is declared but not used [XDeclaredButNotUsed]
var name {.deprecated.} : string 
```

Прагма `{.deprecated.}` сообщает компилятору, что переменная `name` устарела, и будет выведено предупреждение во время компиляции.

Есть прагма `{.hint.}`, которая позволяет выводить какое-то сообщение при компиляции.
```nim
# Во время компиляции выведется сообщение - Hint: какая-то строка [User]
{.hint: "какая-то строка".} 
```

### ООП
Для создания обьектного типа необходимо использовать `object` в блоке `type`.
Обьекты такого типа попадают в стек. Новые объекты можно создавать несколькими способами.
```nim
type
  Person = object
    name: string
    age: int

# 1-й способ - "вызов" типа
var noOne = Person() # Значения полей будут стандартными

var me = Person(name: "Кирилл", age: 27)

# 2-й способ - создание переменной Person и назначение полей
var friend: Person
friend.name = "Вадим"
friend.age = 23

echo "Я - " & $me
echo "Друг - " & $friend
```

Объекты, объявленные с `ref` будут контролироваться сборщиком мусора, и они будут передаваться по ссылке, а не по значению (как обычные объекты).
```nim
type
  Person = ref object 
    name: string
    age: int

var me = Person(name: "Ира", age: 30)
echo me.name
```

Наследование можно осуществлять с помощью ключевого слова `of`.
Нельзя наследоваться от финального обьекта. Для этого объект, от которого будут наследоваться другие, сам должен наследоваться от `RootObj`.
```nim
type
  Person = ref object of RootObj
    name: string
    age: int
  Student = ref object of Person # Наследуемся от Person
    sex: string
    
var me = Student(name: "Иван", age: 15, sex: "мужской")
echo me.sex
```

Nim поддерживает *UFCS* (Unified Function Call Syntax), поэтому в данном примере
``getObj(me)`` означает то же самое, что и ``me.getObj()``
```nim
type
  Person = object

proc getObj(this: Person): string = 
  return "Yes, this is Person"

var me = Person()
# Оба echo выведут одно и то же
echo me.getObj()
echo getObj(me)
```

Если нужны мультиметоды, то вместо `proc` необходимо использовать `method` и прагму `{.base.}` для базового метода.
```nim
type
  Person = object

# Базовый метод
method getObj(this: Person): string {.base.} = 
  return "Person"

var me = Person() # создаем объект
echo me.getObj() # вызываем метод
```

В процедурах используется статическое связывание, оно происходит во время компиляции.
В методах (`method`) используется динамическое связывание, оно происходит во время выполения программы.
Именно динамическое связывание позволяет создавать мультиметоды для реализации наследования (как в традиционном ООП)
```nim
type
  # Тип Person, от которого будем наследовать другие объекты
  Person = object of RootObj
  # Объекты Man и Woman наследованы от Person
  Man = object of Person
  Woman = object of Person

proc getObj(this: Person): string =
  return "Person"

proc getObj(this: Man): string =
  return "Man"

# Статическая привязка (во время компиляции)
proc print(this: Person) = 
  # Мы точно знаем, что процедура получит на вход Person
  echo this.getObj()
    
method getObj2(this: Person): string {.base.} =
  return "Person2"

# А здесь привязка во время компиляции не происходит
method print2(this: Person) {.base.} = 
  # Во время выполения происходит подстановка объектного типа,
  # поэтому данный метод можно вызвать для любого типа, который наследуется
  # от Person (как и для самого Person)
  echo this.getObj2() 

method getObj2(this: Man): string =
  return "Man2"

method getObj2(this: Woman): string =
  return "Woman"

var me = Man()

# Мы вызваем print, и ему чётко ясно, что он работаем с Person
# И уже не важно, что объект типа Man, так как связывание уже прошло
me.print() # Выведет Person

# А print2 сработает хоть с Man, хоть с Woman, хоть с Person
me.print2() # Выведет Men2

var w = Woman()
w.print2() # Выведет Woman
```

Обратиться к атрибуту внутри процедуры можно так:
```nim
type
  Obj = object
    number: int

# Вместо self можно использовать любое другое название аргумента
proc get(self: Obj): int =
  return self.number # Возвращаем атрибут объекта "number"

var o = Obj(number: 15)
echo o.get()
```

Если объектам необходимо присваивать значения, то нужно добавлять `var` к их типу в аргуменах процедуры. При этом объект будет передаваться по ссылке (так же, как и `ref` объект по умолчанию)
```nim
type
  Obj = object
    number: int

proc get(self: Obj): int =
  return self.number 

# В self мы на самом деле получаем ссылку на объект, но Nim позволяет нам
# "не знать" об этом (т.е не нужно никаких других добавлений)
proc set(self: var Obj, x: int) = 
  self.number = x

var o = Obj(number: 15)
o.set(45)
echo o.get() # Выведет 45
```

### Дженерики
Дженерики - это процедуры, которые могут принимать разные типы в качестве аргументов и изменять своё поведение на основе них.

Между `[]` после названия процедуры указываются типы дженерика. `[T]` - любой тип. `[T: int|float]` - только `int` или `float`.
```nim
# echoSomething принимает аргумент x, который может быть любым типом
proc echoSomething[T](x: T) =
  # Однако, если для какого-либо из типов нет процедуры $ для перевода в
  # строку, то будет ошибка во время компиляции
  echo x
    
echoSomething(45)
echoSomething(45.45)
echoSomething("строка")

# intOrFloat принимает аргумент x, который может быть типом int или float
proc intOrFloat[T: int|float](x: T) =
  echo x
    
intOrFloat(45)
intOrFloat(45.45)
intOrFloat("будет ошибка") # Ошибка, intOrFloat не принимает строки

# lol принимает x типа int или float, и y типа char или string
proc lol[T: int|float, D: char|string](x: T, y: D) =
  echo x
  echo y
    
lol(45,'a')
lol(45.45, "sdf")

lol("sdf", 45) # Ошибка, x должен быть типом int или float
```

### Потоки и параллелизм

[comment]: # (#TODO: сделать эту секцию)
### Создание динамических библиотек

[comment]: # (#TODO: сделать эту секцию)
### Компиляция
Для компиляции файла используется команда:
```bash
nim c -r file.nim
```

* `nim` - вызываем компилятор и передаем ему параметры
* `c` - сокращение от `compile`, компилирует файл с помощью стандартного бекенда (Си)
* `-r` - сокращение от `--run`. Скомпилированный бинарник сразу запустится
* `file.nim` - файл с исходным кодом который мы хотим скомпилировать

Для компиляции в релизном режиме (с максимальными оптимизациями) нужно добавить `-d:release` к команде.

### Ссылки
- [https://nim-lang.org/docs/manual.html](https://nim-lang.org/docs/manual.html)
- [https://nim-lang.org/docs/tut1.html](https://nim-lang.org/docs/tut1.html)
- [https://nim-lang.org/docs/tut2.html](https://nim-lang.org/docs/tut2.html)
- [https://nim-lang.org/docs/lib.html](https://nim-lang.org/docs/lib.html)
- [https://nim-by-example.github.io](https://nim-by-example.github.io)
- [http://rosettacode.org/wiki/Category:Nim](http://rosettacode.org/wiki/Category:Nim)
- [http://devdocs.io/nim/](http://devdocs.io/nim/)
- [https://play.nim-lang.org/](https://play.nim-lang.org/)
- [https://github.com/dom96/choosenim](https://github.com/dom96/choosenim)
- [https://nim-by-example.github.io/](https://nim-by-example.github.io/)
- 
