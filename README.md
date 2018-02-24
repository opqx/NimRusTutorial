# Учебное пособие по языку Nim [ альфа версия ]

- [сайт проекта](https://nim-lang.org/ "Язык программирования Nim.")
- [репозиторий проекта](https://github.com/nim-lang/Nim "Git Nim.")

>Текущая версия Nim 0.17.2

### Для кого это пособие
Данное пособие предназначается для опытных программистов, которые уже умеют программировать.
Люди которые только встали на этот путь могут не понять многие аспекты, которые в тексте не будут раскрыты.

### Устновка компилятора
Для nim есть удобная утилита choosenim

- https://github.com/dom96/choosenim

Она позволяет удобно установить компилятор, обновить версию или выбрать нужную.
С помощью этой команды можно ее установить на unix подобные системы.
Предварительно убедитесь что у вас установлены curl и gcc.

curl https://nim-lang.org/choosenim/init.sh -sSf | sh

Теперь добавьте к переменной PATH значение `/.nimble/bin` в один из ваших файлов ~/.profile или ~/.bashrc file .
У вас установлены и choosenim, и nim, последней стабильно версии. Убедитесь командой `choosenim -v && nim -v` .

### Комментарии

Однострочный комментарий.

    # какой-то комментарий

Многострочный комментарий.

    #[
    комментарий
    на несколько
    строк
    ]#

Вложенные мнострочные комментарии.

    #[
    вложенный
     #[комментарий]#
     #[на #[несколько]# ]#
    строк
    ]#



### Типы данных

В одинарных кавычках помещают символы.

    var ch: char = 'a'
    var tab: char = '\t'

**String** - это строка, нужно писать в двойных кавычках ""

    var login: string = "wtf"

**Int** - число.
Пробелы игнорируются.

    var age: int = 45
    var mysalary  = 1_000_000

**Float** - число с плавающей запятой

    var price: float = 45.45

**Bool** - булев тип, куда ж без него.

    var good: bool = true
    var bad: bool = false


### Переменные
У Nim статическая типизация по этому нужно указывать тип переменной.

Переменные объявляются ключевым словом **var**. 
Затем идет имя переменной. 
А после **:** указывают тип. 
Потом **=** и значение.

    var login: string = "valgala"

Компилятор больно умный и может сам определять тип. 
По этому такая записть тоже правильная. Без указания типа. 

    var login = "valgala"

**Const** - константа вычисляется во время компиляции.
Потом изменить нельзя.

    const myos = hostOS

**Let** - переменная с одноразовым присваиванием. 
Нельзя повторно присвоить значение будет ошибка.

    let myint = 5

Блоки кода разделяются пробелами. Табы не работают, будут ошибки.

    var
      name = "vera"
      age = 20

### Дополнительные типы
В Nim можно определить новые типы. Делается это с помощью **type**.

**range** - позволяет определить диапазон значений, среди которых могут быть челочисленные
значения, *char* или перечисления.

    type
      subrange = range[1..12]

    var t: subrange = 1 # входит в диапазон
    t = 12 # входит в диапазон
    t = 15 # не входит в диапазон будет ошибка

**enum** - позволяет создавать новый тип *перечисления*.
Значения у этого типа могут быть только из числа перечисленных.
	 
    type
      enumType = enum # создаем тип enum
        one, two, three # указываем возможные значения
    
    var a: enumType = three
    var b = two
    echo a
    echo b
    
Значения упорядочены. Их номер можнопосмотреть через **ord**.

    type
      enumType = enum
        one, two, three
    
    var a: enumType = three
    var b = two
    echo ord(a) # 2
    echo ord(b) # 1
    
Можно задать свои номера, но они должны идти по возрастанию.
Возможны промежутки *5 - 8* и это не будет ошибкой.

    type
      enumType = enum
        one = 5, two = 8
    
    var a = one
    echo ord(a) # 5
    
Так же можно присвоить строковое значение.

    type
      enumType = enum
        one = "Какая-то строка", two = 8
    
    var a = one
    echo ord(a) # индекс
    echo a # строка
    
Или задать сразу индекс и строку.

    type
      enumType = enum
        one = (2,"Какая-то строка"), two = 8
    
    var a = one
    echo ord(a) # индекс
    echo a # строка
    
**array** - это массив фиксированного размера и одного типа.

    type
      arrStr = array[1..3, string] # тип arrStr будет массивом из трех строк
      arrStr2 = array[3, string] # получим тот же результат
      
    var myArr: arrStr = [ "asd", "sdf", "sdfg" ]
    var myArr2: arrStr = [ "asd", "sdf", "sdfg", "123" ] # будет ошибка
    var myArr3: arrStr = [ "asd", "sdf", ] # тоже ошибка
    
Так же массив можно создать с помощью скобок **[ ]**.

    var arr = [1,2,3]

Получить доступ к элементу можно можно через все теже скобки.

    var arr = [1,2,3]
    
    echo arr[1]
    arr[1] = 5
    echo arr[1]
    
**seq** - это последовательность. Она похожа на массив, только переменной длины.
Обьявить последовательность можно с помошью **seq** и указания типа. Создается последовательность
с помощю конструкции **@[ ]**.

    type
      myStrSeq = seq[string] # последовательность из строк
      myIntSeq = seq[int] # последовательность из чисел
    
    var mySeq: myIntSeq # указали тип (будет числовая последоватьельность seq[int]) 
    mySeq = @[] # присвоили пустую последовательность
    mySeq = @[1,2,3] # присвоили последовательность из 1,2,3
    
**tuple** - кортеж, это упорядоченные значения вида ключ: значение.
Доступ к элементам возможен через `.` или `[ ]`.

    type
      people = tuple[name: string, age: int]
      
    var
      me: people = ("Dima", 30)
      friend: people
      girl: people
    
    friend[0] = "Vova"
    friend[1] = 35
    
    girl.name = "Ira"
    girl.age = 25
    
    echo me, friend, girl 
    


### Ветвления
Ветвление создается ключевыми словами **if**, **elif**, **else** плюс **:**

Если условие между `if` и `:` верно `true` то выполниться следующий блок кода. 

    if 2 == 2:
      echo "распечатает эту строку"
    
Если условие не верно, то выполеняется альтернативный блок кода.

    if false:
      echo "эта процедура не отработает потому что false"
    else:
      echo "а эта строка будет напечатана"
    
Если нужно дополнительное условие используется `elif`.

    if false:
      echo "эта процедура не отработает потому что false"
    elif true:
      echo "а эта строка будет напечатана"
    else:
      echo "этот блок кода не отработает"
    
Еще один способ сделать ветвление через конструкцию `case`.
Между **case** и **:** пишем значение, между **of** и **:** с чем будем сравнивать,
через запятую можно перечислить несколько значений.
Где происходит совпадение тот блок кода и будет выполняться. Если все разное, то выполняется
блок **else** - он обязательный.

    case 2
    of 1:
      echo "1"
    of 2,3,4:
      echo "2,3,4"
    else:
      echo "ни один случай не подошел"
    


### Циклы
Пример **for** цикла по итератору.

    for i in countup(20,25):
      echo i

- **countup(20,25)** - это итераток от 20 к 25.
- **for** - ключевое слово которое объявляет цикл.
- **i** - переменная куда будет попадать элемент итератора.
- **in** - показывает из какого итератора значения.
- **echo** - функция вывода на стандартный поток вывода.

Ключево слово **break** прерывает цикл.

    for i in countup(20,30):
      if i == 25:
        break
      echo i


Ключевое слово **continue** завершает текущую итерацию.

    for i in countup(20,30):
        if i > 25:
            continue
        echo i


Пример **while** цикла.

    var
      counter = 1
      n = 5
    while n != counter :
        echo counter
        inc(counter)

- **while** проверяет условие и если оно возвращает true делает итерацию цикла.
- **inc** - добавляет 1 к переменной counter.
- **echo** - функция вывода на стандартный поток вывода.

### Процедуры
Процедуры обозначаются словом **proc**.
Вызов делается через добавления **()** к имени процедуры.

    proc print_something =
      echo "la la la"
    
    print_something()

Передать *параметры* в процедуру можно добавив их в скобки после названия с указанием типа.
Не указал тип будет ошибка, не передал параметр в процедуру - ошибка.

    proc print_something(my_string: string) =
      echo my_string
    
    print_something("Nim классный язык программирования!")

Параметры *по умолчанию* добавляются после знака **=**. 
Теперь можно не передавать параметры в процедуру.

    proc print_something(my_string: string = "Математика супер наука !") =
      echo my_string
    
    print_something()

*Несколько параметров* указываются через **`,`** .
Если мы присваиваем значения по умолчанию, то можно пропустить тип.
Мы же помним что компилятор умный.

    proc print_something(
        my_string: string = "Математика супер наука !",
        need_math = " Все знают что она нужна программисту !"
      ) =
    
      echo my_string, need_math
    
    print_something()

Пример без значений по умолчанию.

    proc print_something(name: string, year: int) =
      echo "Меня зовут " & name & "."
      echo "Я живу в " & $year & " году."
    
    print_something("Фрай", 3000)

- **&** - конкатенация (объединение) строк.
- **$** - оператор преобразования в строку.

Можно указать тип сразу для нескольких параметров.

    proc point(x,y,z: int) =
      echo "координаты в пространстве"
      echo "x=" & $x
      echo "y=" & $y
      echo "z=" & $z
    
    point(10, 11, 12)

Переменная **return** позволяет вернуть значение из функции. 
Но мы обязательно должны указать тип возвращаемого значения.
После имени процедуры и параметров если они есть нужно поставить знак `:`
и до знака `=` указать тип.

    proc get_string: string =
      return "Свободу попугаям !"
    
    echo get_string()

Возможно делать перегрузку в процедурах.

    proc overloadMe(x: int, y: int)=
      echo x,y
    
    proc overloadMe(x: int)=
      echo x
    
    overloadMe(1) 
    overloadMe(1, 2)
    
Если у процедуры неуказать имя она будет анонимной.

    var anonymous = proc(x: string)=
      echo x
    
    anonymous("я анонимная процедура :)")
    

Пример *передачи* процедуры в качестве аргумента.

    proc f(): int =
      return 5
      
    proc wrap(function: proc)=
      echo function()
      
    wrap(f)
    
Пример *возврата* процедуры в качестве аргумента.

    proc get(): proc =
      proc res(): int =
        return 5
      return res
      
    echo get()()

Если мы хотим передать массив или последовательность в процедуру для этого нам понадобиться openArray.
Этот тип данных может использоваться только в качестве параметра процедуры.
Oбязательно надо указать какой тип данных будет переданны в открытом массиве

    proc opArray(a: openArray[int])= # открытый массив будет содержать int
      for i in a:
        echo i
    
    opArray(@[1,2,3,4,5])
    opArray([1,2,3,4,5])
    
Если надо передать произвольное число параметров используйте varargs. Он работает аналогично openArray.

    proc opArray(a: varargs[string])= # varargs будет содержать string
      for i in a:
        echo i
    
    opArray("as", "sd", "df")

Так же varargs позволяет сделать преобразование типов.

    proc opArray(a: varargs[string, `$`])= # varargs будет содержать string,
                                          # но если попадется другой тип он будет преобразовать в строку
      for i in a:
        echo i
    
    opArray("as", "sd", 21)



### Итераторы
Если в процедуре заменить **proc** на **iterator** а **return** на **yield** то получиться итератор.

    iterator printItem(): int =
      for i in [1,2,3,4,5]:
        yield i
        
    for i in printItem():
      echo i
    

### Преобразование типов
### Исключения
В Nim исключения помещаются между **try** и **except**. В секцие *try* мы отслеживаем исключения
и если таковые имеются то происходит переключение на *except*. В **finally** код выполняется в любом случае.

    try:
      var arr = @[1,2,3]
      echo arr[5]
    except:
      echo "нет такого индекса"

Мы обратились к несуществующему индексу, код скомилировался и отраболала ветка *except*.

После *except* мы можем указать какой тип исключения мы отлавливаем.

    try:
      var arr = @[1,2,3]
      echo arr[5]
    except IndexError: # отлавливается только обращение к несуществующему индексу
      echo "нет такого индекса"

Другие ошибки не будут пойманы.

Блок **finally** выполняется всегда даже если у нас ошибка.

    try:
      var arr = @[1,2,3]
      echo arr[5]
    except IOError:
      echo "нет такого индекса"
    finally:
      echo "fynally - работет всегда."



### Шаблоны
### Макросы
### Модули
Nim позволяет разбивать код на модули. Один файл один модуль. 
Что бы процедуру или переменну можно было импортировать ее нужно пометить ``*``.

    # file one.nim
    var name* = "Vova" # будет экспортироваться
    var lastname = "Sidorov" # не будет экспортироваться
    
    proc exportme* =  # будет экспортироваться
      echo "exportme"
    proc notexportme = # не будет экспортироваться
      echo "notexportme"
    
Теперь в другом файле вы можете сделать `import one` и пользоваться импортированными
переменными и процедурами.

    # file two.nim
    import one
    exportme() # буде работать
    notexportme() # будет ошибка

Если мы не хотим что-либо экспортировать мы можем использовать `except`.

    # file two.nim
    import one except name # переменная name не будет импортироваться
    exportme() # буде работать
    notexportme() # будет ошибка

Так же мы можем импортировать только указанные символы с помощью `from`.

    # file two.nim
    from one import exportme
    exportme() # буде работать
    echo name # будет ошибка

Можно потребовать обязательное указание квалификатора *имя модуля*, для это нужно указать `nil`.

    # file two.nim
    from one import nil # теперь обязательно нужно указывать имя модуля
    one.exportme() # работает
    exportme() # неработает

Можно задать псевдонымы для модулей через `as`.

    # file two.nim
    from one as x import nil
    x.exportme() # x это наш псевдоним

Команда `include` позволяет вставить на свое место содержимое файла.

    include one.nim
    exportme()



### Сообщения компилятора (прагмы)
Если нам нужно сообщить компилятору какую-либо информацию можно воспользоваться прагмами.
Они делаются с помощью фигурных скобок и названия прагмы `{. pragma .}`

    var name {. deprecated .} : string # Hint: 'name' is declared but not used [XDeclaredButNotUsed]

Прагма `{. deprecated .}` сообщает компилятору что *name* устарело, и будет выведена заметка во время компиляции.

Прагма может принимать сообщение.

    {.hint: "какая-то строка".} # во время компиляции выведется сообщение - Hint: какая-то строка [User]


### Ооп
Для создания обьектного типа необходимо исользовать **object** в блоке **type**.
Обьекты такого типа попадают в стек.

    type
      Person = object
        name: string
        age: int


Так можно создать новый обьект.

    type
      Person = object # такие обьекты попадуть в стек
        name: string
        age: int
    
    var me = Person() # создание обьекта
    echo me

Так можно использовать конструктор.

    type
      Person = object
        name: string
        age: int
    var me = Person(name:"Ira", age:30) # конструктор - позволяет сразу задать значения атрибутов
    echo me

Мы можем задать значения после создания обьекта.

    type
      Person = ref object 
        name: string
        age: int
     
    var me = Person()
    me.name = "Ira"
    me.age = 20
    echo me.name
    echo me.age

Если использовать **ref** такие обьекты попадут в кучу.

    type
      Person = ref object # такие обьекты попадут в кучу 
        name: string
        age: int
     
    var me = Person(name:"Ira", age:30)
    echo me.name

Наследование можно сделать с помощью ключевого слова **of**.
Нельзя наследоваться от финального обьекта. По этому используется **RootObj**.

    type
      Person = ref object of RootObj # наследуемся от RootObj что бы небыло финального обьекта 
        name: string
        age: int
      Man = ref object of Person # наследуемся от Person
        sex: string
     
    var me = Man(name:"Ira", age:30, sex: "female")
    echo me.sex

Для использования метода нужно создать процедуру и задать ей в качестве типа наш обьект.

    type
      Person = object
    
    proc getObj(this: Person): string = # создаем процедуру которая принимает наш объект
      return "Person"
    
    var me = Person() # создаем объект
    echo me.getObj() # вызываем метод

Если нужны мультиметоды вместо **proc** надо использовать **method** и прагму **{.base.}** для базового метода.

    type
      Person = object
    
    method getObj(this: Person): string {.base.} = # создаем метод с прагмой который принимает наш объект
      return "Person"
    
    var me = Person() # создаем объект
    echo me.getObj() # вызываем метод

**proc** - говорит что мы хотим использовать статическое связывание, оно происходит во время компиляции.
**method** - говорит что мы хотим использовать динамическое связывание, оно происходит во время выполения программы.
Именно этот способ позволяет создавать мультиметоды для реализации наследования.

    type
      Person = object of RootObj # создали объектный тип Person от которого будем наследоваться
      Men = object of Person # наследуемся от Person
      Women = object of Person # наследуемся от Person
    
    
    proc getObj(this: Person): string =
      return "Person"
    proc print(this: Person) = # здесь происходить привязка во время компиляции,
      echo this.getObj() # мы берем известный нам тип Person
      # мы четко знаем что метод отработает с Person
    
    proc getObj(this: Men): string =
      return "Men"
    
    
    method getObj2(this: Person): string {.base.} =
      return "Person2"
    method print2(this: Person) {.base.} = # а здесь во время компиляции ничего не происходит,
      echo this.getObj2() # только во время выполения происходит подстановка объектного типа
      # по этому мы не знаем с каким объектом будет работать метод
      # у какого объекта вызовем с тем и будет работать
    
    method getObj2(this: Men): string =
      return "Men2"
    
    method getObj2(this: Women): string =
      return "Women"
    
    
    var me = Men()
    
    # мы вызваем print, и нам четко ясно что отработаем с Person
    # и уже не важно что объект типа Men
    # связывание уже прошло
    me.print() # выведет Person
    
    # а тут не понятно какого типа объект
    # что дадут с тем и будем работать
    # мы даем Men, по этому c ним и будем работать
    me.print2() # выведет Men2
    
    var w = Women()
    # а тут передаем Women - значит с ним работаем
    w.print2() # выведет Women
    
Обратиться к атрибуту внутри процедуры можно так.

    type
      Obj = object
        number: int
    proc get(self: Obj): int = # указываем в self тип оъекта
      # на месте self можно написать любое слово (self,this,obj,main и т.д.)
      return self.number # обращаемся к атрибуту объекта
    
    var o = Obj(number: 15)
    echo o.get()
    
Если надо присвоить значение нужно дополнительно указать `var`.

    type
      Obj = object
        number: int
    proc get(self: Obj): int =
      return self.number 
    
    proc set(self: var Obj, x:int)= # необходимо указать var перед типом
      # если будет присваивание
      self.number = x 
    
    var o = Obj(number: 15)
    o.set( x=45 )
    echo o.get()



### Дженерики
Дженерики это процедуры которые могут принимать указанные типы в качестве параметров.
Между `[]` мы указываем нужные типы. `[T]` - любой тип. `[T: int|float ]` - только **int** или **float**.

    # one
    proc dj[T](x: T)=
      echo x
      
    dj(45)
    dj(45.45)
    dj("строка будет принята")


    # two
    proc dj[T: int|float](x: T)=
      echo x
      
    dj(45)
    dj(45.45)
    dj("будет ошибка") # ошибка не соотвествия типа

    # three
    proc dj[T: int|float, D: char|string](x: T, y: D)=
      echo x
      echo y
      
    dj(45,'a')
    dj(45.45, "sdf")
    
    dj("sdf", 45) # ошибка не соотвествия типа


### Потоки и параллелизм
### Создание динамических библиотек
### Компиляция
Для компиляции файла с исходниками нужно выполнить такую команду в консоле.

    nim c -r --verbosity:0 helloworld.nim

* nim - вызываем компилятор и передаем ему параметры
* c - синоним *compile* говорит о том что мы хотим скомпилировать файл
* -r - сообщает что мы хотим сразу запустить скомпилированный файл
* --verbosity:0 - какую информацию выводить на поток вывода, на сколько она подробная
* helloworld.nim - файл с исходным кодом который мы хотим скомпилировать


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
  