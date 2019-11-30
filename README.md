В репозитории приведен набор соглашений по оформлению кода на языке Kotlin. 

Этот список правил расширяет предложенные [Google](https://android.github.io/kotlin-guides/style.html) и [командой разработки Kotlin](https://kotlinlang.org/docs/reference/coding-conventions.html) гайды и пересматривает в них некоторые неоднозначные моменты.

# Содержание
1. [Длина строки](#linelength)
2. [Правила именования](#naming)
3. [Порядок следования модификаторов](#modifier_order)
4. [Форматирование выражений](#expression_formating)
5. [Функции](#function)
    * 5.1 [Функции с одним выражением](#function_expression)
    * 5.2 [Форматирование вызова функции](#formating_function_calling)
    * 5.3 [Форматирование описания функции](#formating_function_declaration)
    * 5.4 [Вызов переменной функционального типа](#calling_function_variable)
6. [Классы](#classes)
7. [Аннотации](#annotation)
8. [Структура класса](#class_member_order)
9. [Форматирование лямбда-выражений](#lambda_formating)
10. [Использование условных операторов](#condition_operator)
11. [Template header](#template_header)
12. [Файлы](#files)


# <a name='linelength'>Длина строки</a>
- Максимальная длина строки: 120 символов.

# <a name='naming'>Правила именования</a>
Следуем тому, что Kotlin JVM совместимый язык. Все ниже приведенные правила именования взяты из общепринятых стандартов, например [отсюда](https://google.github.io/styleguide/javaguide.html).
- Неизменяемые поля в (Companion) Object и compile-time константы именуются в стиле SCREAMING_SNAKE_CASE
- Для полей View из Kotlin Extension используется стиль lower_snake_case
- Любые другие поля именуются в стиле lowerCamelCase
- Функции именуются в стиле lowerCamelCase
- Классы именуются в стиле UpperCamelCase
- Имена пакетов всегда в нижнем регистре, не использовать символ подчеркивания в имени пакета(org.example.project). Не использовать имена из нескольких слов в пакете.

### BAD:

```kotlin
return_to_call
```

### BAD:

```kotlin
returnToCall
```

### BAD:

```kotlin
returntocall
```

### GOOD:

Создать пакет call, внутри него создать пакет pickup
```kotlin
├─ call
│  ├─ pickup
```
	
- Другое (Misc)

В коде, аббевиатуры, следует воспринимать как слова. Например:

### BAD:

```kotlin
XMLHTTPRequest
URL: String? 
findPostByID
```

### GOOD:

```kotlin
XmlHttpRequest
url: String
findPostById
```

# <a name='modifier_order'>Порядок следования модификаторов</a>

1) override
2) public / protected / private / internal
3) abstract / final / open / const
4) lateinit
5) suspend
6) enum / annotation / sealed / data
7) companion
8) inner
9) inline
10) infix
11) operator


# <a name='expression_formating'>Форматирование выражений</a>

При переносе на новую строку цепочки вызова методов символ `.` или оператор `?.` переносятся на следующую строку, property при этом разрешается оставлять на одной строке:
```kotlin
val collectionItem = source.collectionItems
                ?.dropLast(10)
                ?.sortedBy { it.progress }
```
Элвис оператор `?:` при разрыве выражения также переносится на новую строку:
```kotlin
val promoItemDistanceTradeLink: String = promoItem.distanceTradeLinks?.appLink
            ?: String.EMPTY
```
При описании переменной с делегатом, не помещающимися на одной строке, оставлять описание с открывающейся фигурной скобкой на одной строке, перенося остальное выражение на следующую строку:
```kotlin
private val promoItem: MarkPromoItem by lazy {
        extractNotNull(BUNDLE_FEED_UNIT_KEY) as MarkPromoItem
}
```

# <a name='function'>Функции</a>
## <a name='function_expression'>Функции с одним выражением</a>
* Позволительно использовать функцию с одним выражением только в том случае, если она помещается в одну строку.

## <a name='formating_function_calling'>Форматирование вызова функции</a>
* Использование именованного синтаксиса аргументов остается на усмотрение разработчика. Стоит руководствоваться сложностью вызываемого метода: если вызов метода с переданными в него параметрами понятен и очевиден, нет необходимости использовать именованные параметры.
При написании именованных аргументов делать перенос каждого аргумента на новую строку с двойным отступом и переносом закрывающейся круглой скобки на следующую строку:

```kotlin
runOperation(
		method = operation::run,
		consumer = consumer,
		errorHandler = errorHandler,
		tag = tag,
		cache = cache,
		cacheMode = cacheMode
)
```

## <a name='formating_function_declaration'>Форматирование описания функции</a>

* При необходимости разрыва строки осуществляется перенос каждого аргумента функции на новую строку с двойным отступом и переносом закрывающей круглой скобки на следующую строку.

## <a name='calling_function_variable'>Вызов переменной функционального типа</a>

* Всегда использовать полный вариант с написанием `invoke` у переменной вместо использования сокращенного варианта:

```kotlin
fun runAndCall(expression: () -> Unit): Result {
        val result = run()

        //Bad
        expression()
        //Good
        expression.invoke()

        return result
}
```

# <a name='classes'>Классы</a>
- При необходимости разрыва строки осуществляется перенос каждого параметра класса на новую строку с двойным отступом и переносом закрывающейся круглой скобки на следующую строку:

### BAD:

```kotlin
data class CategoryStatistic(val id: String, val title: String, val imageUrl: String, val percent: Double) : Serializable
```

### BAD:

```kotlin
data class CategoryStatistic(val id: String, val title: String,
                		val imageUrl: String, val percent: Double
) : Serializable
```

### BAD:

```kotlin
data class CategoryStatistic(
                val id: String,
                val title: String,
                val imageUrl: String,
                val percent: Double) : Serializable
```

### GOOD:

```kotlin
data class CategoryStatistic(
                val id: String,
                val title: String,
                val imageUrl: String,
                val percent: Double
) : Serializable
```

### GOOD:

```kotlin
data class CategoryStatistic(val id: String, val title: String) : Serializable
```

- Если в описании класса родительский класс не помещается на одной строке, также осуществляется перенос каждого из его параметров на новую строку с переносом закрывающей круглой скобки на следующую строку.
- Если описание класса не помещается в одну строку и реализует несколько интерфейсов, то применять стандартные правила переносов, т.е. делать перенос только в случае, когда не помещается на одну строку, и продолжать перечисление интерфейсов на следующей строке.
- Использование именованного синтаксиса аргументов остается на усмотрение разработчика. Стоит руководствоваться сложностью используемого конструктора класса: если конструктор с переданными в него параметрами понятен и очевиден, нет необходимости использовать именованные параметры.

# <a name='annotation'>Аннотации</a>
- Аннотации как правило располагаются над описанием класса/поля/метода, к которому они применяются.
- Если к классу/полю/методу есть несколько аннотаций, размещать каждую аннотацию с новой строки:
```kotlin
@JsonValue
@JvmField
var promoItem: PromoItem? = null
```
- Если к полю/методу применяется только одна аннотация без параметров, указывать ее над полем/методом.
- Аннотации, относящиеся к файлу, располагаются сразу после комментария к файлу, и перед package, с разделителем в виде пустой строки.

# <a name='class_member_order'>Структура класса</a>
1) Поля: abstract, override, public, internal, protected, private
2) Блок инициализации: init, конструкторы
3) Абстрактные методы
4) Переопределенные методы родительского класса(желательно в том же порядке, в каком они следуют в родительском классе)
5) Реализации методов интерфейсов(желательно в том же порядке, в каком они следуют в описании класса, соблюдая при этом порядок описания этих методов в самом интерфейсе)
6) public методы
7) internal методы
8) protected методы
9) private методы
10) inner, sealed, data классы
11) companion object

# <a name='lambda_formating'>Форматирование лямбда-выражений</a>

- При возможности оставлять лямбда-выражение на одной строке, используя `it` в качестве аргумента.
- При использовании лямба-функции в качестве аругмента выносить её за скобки если этот параметр единственный.
- Если выражение возможно написать с передачей метода по ссылке, передавать метод по ссылке (Доступно с 1.1):
```kotlin
viewPager.adapter = QuestAdapter(quest, this::onQuestClicked)
```
- При написании лямбда-выражения более чем в одну строку всегда использовать именованный аргумент, вместо `it`:
```kotlin
viewPager.adapter = QuestAdapter(quest, { quest ->
        onQuestClicked(quest)
})
```
- Неиспользуемые параметры лямбда-выражений всегда заменять символом `_`.
	
		Обратите внимание на второй параметр, при определении BiFunction
### BAD:

```kotlin
fun createBiFunction(defaultSchema: String): BiFunction {
	return BiFunction { value, schema ->
			Bundle().apply {
			    putString("value", value)
			    putString("schema", defaultSchema)
			}
		    }
}
```

### GOOD:

```kotlin
fun createBiFunction(defaultSchema: String): BiFunction {
	return BiFunction { value, _ ->
			Bundle().apply {
			    putString("value", value)
			    putString("schema", defaultSchema)
			}
		    }
}
```

- Опускается один неиспользуемый параметр в лямба-выражениях.

### BAD:

```kotlin
button.setOnClickListener( _ -> doSomething())
```

### GOOD:

```kotlin
button.setOnClickListener(doSomething())
```
- Избегать использования Destrucion Declaration в лямбда-выражениях.

# <a name='condition_operator'>Использование условных операторов</a>
Не обрамлять `if` выражения в фигурные скобки только если условный оператор `if` помещается в одну строку.
При возможности использовать условные операторы, как выражение:
```kotlin
return if (condition) foo() else bar()
```
У оператора `when` для коротких выражениях ветвей условия размещать их на одной строке с условием без фигурных скобок:
```kotlin
when (somenCondition) {
        0 -> fooFunction()
        1 -> barFunction()
        else -> exitFunction()
}
```
Если хоть в одной из ветвей есть фигурные скобки, обрамлять ими все остальные ветки.
У оператора `when` для блоков с выражениями, которые состоят более чем из одной строки использовать для этих блоков фигурные скобки и отделять смежные case-блоки пустой строкой:
```kotlin
when (feed.type) {
        FeedType.PERSONAL -> {
        	with(feed as PersonalFeed) {
        		datePopupStart = dateBegin
        		datePopupEnd = dateEnd
        	}
        }


        FeedType.SUM -> {
        	with(feed as SumFeed) {
        		datePopupStart = dateBegin
        		datePopupEnd = dateEnd
        	}
        }

        FeedType.CARD -> {
        	with(feed as CardFeed) {
        		datePopupStart = dateBegin
        		datePopupEnd = dateEnd
        	}
        }

        else -> {
        	Feed.EMPTY
        }
}
```

# <a name='template_header'>Template header</a>

- Использовать Template Header для классов. Template:
/**
* Created by ${USER} on ${DATE} ${TIME}
*/

# <a name='files'>Файлы</a>

- Возможно описывать несколько классов в одном файле только для `sealed` классов. В остальных случаях для каждого класса необходимо использовать отдельный файл (не относится к `inner` классам).
