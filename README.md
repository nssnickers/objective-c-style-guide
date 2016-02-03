# Code style для iOS-разработчиков

## Информация

Ниже приведен список документов Apple, посвященных топику. В случае, если что-либо не покрыто в самой статье, можно обратиться к перечисленным источникам:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Содержание

* [Язык](#Язык)
* [Организация кода](#Организация-кода)
* [Отступы](#Отступы)
* [Комментарии](#Комментарии)
* [Именование](#Именование)
  * [Знаки нижнего подчеркивания](#Знаки-нижнего-подчеркивания)
* [Методы](#Методы)
* [Переменные](#Переменные)
* [Свойства](#Свойства)
* [Обращение к свойствам через точку](#Обращение-к-свойствам-через-точку)
* [Литералы](#Литералы)
* [Константы](#Константы)
* [Перечислимые типы](#Перечислимые-типы)
* [Case выражения](#case-выражения)
* [Приватные свойства](#Приватные-свойства)
* [Логические переменные](#Логические-переменные)
* [Условные операторы](#Условные-операторы)
  * [Тернарный оператор](#Тернарный-оператор)
* [Инициализаторы](#Инициализаторы)
* [Фабричные методы](#Фабричные-методы)
* [Методы CGRect](#Методы-cgrect)
* [Вложенность кода](#Вложенность-кода)
* [Обработка ошибок](#Обработка-ошибок)
* [Синглтоны](#Синглтоны)
* [Переносы строк](#Переносы-строк)
* [Именование ресурсов](#Именование-ресурсов)
* [Однотипное именование понятий](#Однотипное-именование-понятий)
* [Использование сторонних библиотек](#Использование-сторонних-библиотек)

## Язык

Предполагается использование английского языка для именования переменных, методов, классов.

Далее для каждого проекта отдельно команда решает, стоит ли использовать для комментариев, commit messages и документации русский или английский. Определяется язык проекта. При не очень хорошем английском хотя бы у **одного** члена команды, надо всеми использовать русский. Для иностранных проектов использование английского для документации обязательно.
Смотрим в репозиторий, спрашиваем у других, затем пишем commit message на нужном языке. Не нужно писать на ломаном английском в проекте, где все пишут комментарии на русском, а также оставлять непонятные русские комментарии в проекте с иностранным заказчиком.

**Желательно:**
```objc
UIColor *myColor = [UIColor whiteColor];
```

**Нежелательно:**
```objc
UIColor *zagruzkaColor = [UIColor whiteColor];
```


## Организация кода

Используйте `#pragma mark -`, чтобы сгруппировать методы в логические группы и объединить реализации протоколов и делегатов. Перед и после `#pragma mark -` желательно ставить пустую строку для лучшей читабельности.

Пример общей структуры:

```objc
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

## Отступы

* Отступы должны состоять из 4 пробелов. Не используйте знаки табуляции для отступов. Данные настройки можно выставить в `Xcode`→`Preferences`→`Text Editing`→`Indentation`.
* Открывающиеся операторные скобки должны стоять на той же строке, что и управляющая конструкция (`if`/`else`/`switch`/`while` и пр.), а закрываться на следующей.

**Желательно:**
```objc
if (user.isHappy) {
    //Do something
} else {
    //Do something else
}
```

**Нежелательно:**
```objc
if (user.isHappy)
{
  //Do something
}
else {
  //Do something else
}
```

* Нужно соблюдать 1 пустую строку в качестве отступа между методами для повышения читабельности и структурированности. Пустыми строками внутри метода можно разграничивать функциональность кода, но, возможно, вам следует вынести эти группы в отдельные вспомогательные методы.
* Пользуйтесь прежде всего автоматически сгенерированными аксессорами для свойств. Но в случае необходимости ключевые слова `@synthesize` и `@dynamic` должны быть вынесены на отдельную строку.
* По умолчанию в IDE должно быть выставлено ограничение в 120 символов для длины любой строки кода. Это можно задать в меню `Xcode`→`Preferences`→`Text Editing`→`Page guide at column`.
* Старайтесь не использовать вызовы методов, в которых параметры были выровнены по двоеточию. Впрочем, есть случаи, когда сигнатура метода содержит >= 3 двоеточий, и выравнивание параметров улучшает читаемость. Однако, пожалуйста, **НЕ** используйте выравнивание параметров в методах, принимающих блоки, так как выравнивание Xcode делает результат плохо читаемым.

**Желательно:**

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**Нежелательно:**

```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

## Комментарии

В местах, где они нужны, комментарии должны объяснять, **зачем** некоторый код выполняет свою логику. Любые комментарии должны быть или релевантными и актуальными, или их не должно быть вовсе.

Также не стоит забывать, что комментарии в виде `///` и `/**  */` подхватываются IDE и впоследствие видны в панели справа.

Комментарии, добавляющие описания к параметрам и результату (`@param`, `@return`) приветствуются прежде всего в библиотеках, так как их будут использовать не авторы библиотек, а другие люди тоже. По ним может быть автоматически сгенерирована документация. В коде клиентского проекта можно обойтись без них.

## Именование

В языке используется стиль CamelCase, которого необходимо придерживаться для именования всех сущностей и объектов. Длинные названия, описывающие предназначение сущности, приветствуются.

**Желательно:**

```objc
UIButton *settingsButton;
```

**Нежелательно:**

```objc
UIButton *setBut;
```

Если вы оформляете библиотеку, то трехбуквенный префикс должен присутствовать для любого имени класса или константы (право на использование двухбуквенного принадлежит Apple), однако, можен быть опущен в названиях сущностей Core Data. Для кода проекта следование этому правилу не обязательно.

Константы называются согласно стилю CamelCase.

**Желательно:**

```objc
static NSTimeInterval const FadeAnimationDuration = 0.3;
```

**Нежелательно:**

```objc
static NSTimeInterval const fade = 1.7;
```

Свойства также должны быть в CamelCase-стиле со строчной первой буквой. Используйте автосгенерированные аксессоры вместо синтезированных, если только у вас нет на то хорошей причины.

**Желательно:**

```objc
@property (strong, nonatomic) NSString *descriptiveVariableName;
```

**Нежелательно:**

```objc
id varnm;
```

### Знаки нижнего подчеркивания

При использовании свойств или переменных экземпляра, всегда следует использовать обращение через `self.`.

Исключения к вышесказанному: обращения внутри инициализаторов, а также вызовы, в которых требуется избежать сторонних эффектов от вызовов аксессоров (геттеры/сеттеры).

Локальные переменные не должны содержать знаков нижнего подчеркивания.

## Методы

В сигнатурах методов обязательно ставьте пробел после спецификатора типа метода (-/+). Обязательно должен присутствовать пробел между сегментами методов (согласно предложенным Apple стилю).

Выделяйте каждый параметр в методе словом, описывающим этот параметр, при этом перед первым параметром должен быть, помимо этого, использован `with`. Правило не относится к параметру, если логично перед ним поставить другой предлог, например:
```objc
- (void)addA:(NSNumber *)a toB:(NSNumber *)b;
- (void)placeView:(UIView *)view intoView:(UIView *)anotherView;
```

Использование слова "and" зарезервировано. Не следует его использовать для множественных параметров, например, `initWithWidth:andHeight:`.

**Желательно:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Нежелательно:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

[Документ от Apple](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html) с правилами именования.

## Переменные

Переменные должны быть названы настолько полно и описывающе, насколько это возможно. Исключение - цикл `for()`, где в качестве счетных переменных допустимы имена длиной в один символ (`i`, `j`, `k`, и т.д.)

Звездочки, обозначающие указатель на некоторый типа данных, должны быть неразрывны с именем переменной, например, `NSString *text`, не `NSString* text` или `NSString * text` (последнее допустимо, если имеем дело с константами).

[Приватные свойства](#Приватные-свойства) должны быть использованы вместо переменных экземпляра везде, где только можно. При использовании свойсв код становится более консистентным.

Прямой доступ к переменным класса, которые лежат 'под' свойством не желателен, кроме обращений в инициализаторах (`init`, `initWithCoder:`, и др.), деструкторе (`dealloc`) и аксессорах.
Чтобы узнать больше об обращении к аксессорам в инициализаторах и деструкторе, можно почитать [здесь](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Желательно:**

```objc
@interface Tutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**Нежелательно:**

```objc
@interface Tutorial : NSObject {
  NSString *tutorialName;
}
```


## Свойства

Свойства должны быть перечислены явно, они должны помогать другим программистам читать ваш код. Атрибуты свойств должны идти в следующем порядке: сначала тип ссылки (`weak`, `strong`, `copy`), затем атомарность (`nonatomic`, `atomic`), что совпадает с автоматически сгенерированными свойствами при, например, создании outlet из Interface Builder.

Для свойств примитивных типов не нужно писать модификатор `assign`, так как он используется по умолчанию. С другой стороны, `atomic`, также используемый по умолчанию, лучше указать явно, чтобы показать, что в коде присутствует многопоточность.

**Желательно:**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```

**Нежелательно:**

```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

Обратите внимание, что со строками необходимо всегда использовать `copy` вместо `strong`. Ведь даже если вы создаете свойство типа `NSString`, кто-нибудь может передать экземпляр `NSMutableString` и менять его без вашего ведома.

**Желательно:**

```objc
@property (copy, nonatomic) NSString *tutorialName;
```

**Нежелательно:**

```objc
@property (strong, nonatomic) NSString *tutorialName;
```

## Обращение к свойствам через точку

Синтаксис с использованием обращений через точку - крайне удобная обертка над вызовом методов аксессоров. Об этом можно почитать больше [здесь](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html).

В большинстве случаев используйте точку, чтобы обратиться к свойству. Однако сеттеры свойств-блоков удобнее вызывать в явном виде, так как в этом случае автозаполняется блок со всеми параметрами, например:

```objc
[myObject setSomeHandler:^(UIView *view, NSString *name){
 // code
}];
```

Вызовы методов с помощью квадратных скобок используются во всех остальных случаях. 

**Желательно:**
```objc
NSInteger arrayCount = self.array.count;
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Нежелательно:**
```objc
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

Обращение к свойству у объекта с неприведенным типом невозможно, но послать ему сообщение на вызов геттера можно. Этим можно пользоваться, например, при взятии определенного элемента массива и последующей работе с ним.

```objc 
int count = arrays[i].count;   // Compilation error
int count = [arrays[i] count]; // Ok
int count = ((NSArray *)arrays[i]).count; // Ok, but clumsy
```

## Литералы

Литералы `NSString`, `NSDictionary`, `NSArray`, и `NSNumber` должны быть использованы каждый раз при создании неизменяемого (immutable) экземпляра соответствующего класса. Обратите особое внимание, что nil при таком создании объектов, переданный в качестве элемента содержимого, приводит к падению приложения.

**Желательно:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**Нежелательно:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

## Константы

Константы предпочтительнее строковых литералов и чисел, прошитым в коде. Они должны быть объявлены с ключевым словом `static`, если только не используется макрос и конструкция `#define`.

**Желательно:**

```objc
static NSString * const AboutViewControllerCompanyName = @"RayWenderlich.com";

static CGFloat const ImageThumbnailHeight = 50.0;
```

**Нежелательно:**

```objc
#define CompanyName @"RayWenderlich.com"

#define thumbnailHeight 2
```

Структура `#define` дает некоторые преимущества и гибкость, но в большинстве случаев стоит придерживаться констант и вспомогательных методов для более сложной логики.


## Перечислимые типы

При использовании перечислимых типов, рекомендуется явно указывать тип, над которым создан `enum`, потому что это упрощает проверку типов и автодополнение кода. SDK теперь содержит макрос для явного указания этого типа данных - `NS_ENUM()`.

**Например:**

```objc
typedef NS_ENUM(NSInteger, LeftMenuTopItemType) {
  LeftMenuTopItemMain,
  LeftMenuTopItemShows,
  LeftMenuTopItemSchedule
};
```

Также можно явно указать значение каждого из вариантов:

```objc
typedef NS_ENUM(NSInteger, GlobalConstants) {
  PinSizeMin = 1,
  PinSizeMax = 5,
  PinCountMin = 100,
  PinCountMax = 500,
};
```

Старый метод создания перечислимого типа использовать **не следует**, кроме случаев, если вы пишете CoreFoundation-код (что вряд ли :] ).

**Нежелательно:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```


## Case выражения

Фигурные скобки использовать не обязательно, кроме случаев, когда сам компилятор настаивает (например, когда ветке соответствуют несколько  операторов).

```objc
switch (condition) {
  case 1:
    // ...
    break;
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  case 3:
    // ...
    break;
  default:
    // ...
    break;
}

```

Если некоторый кусок код выполняется для нескольких вариантов, допустимо убрать ключевое слово `break`. Для большей ясности рекомендуется добавить комментарий в место, где `break` был опущен. Если возможно, и результирующая строка получается не слишком длинной, можно объединенные ветки поместить на одну строку. 

```objc
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default:
    // ...
    break;
}

```

Когда рассматриваются различные значения перечислимого типа, ключевое слово `default` лучше не указывать, чтобы при добавлении новых значений перечислимого типа IDE подсветил предупреждением о не реализованной ветке.

```objc
LeftMenuTopItemType menuType = LeftMenuTopItemMain;

switch (menuType) {
  case LeftMenuTopItemMain:
    // ...
    break;
  case LeftMenuTopItemShows:
    // ...
    break;
  case LeftMenuTopItemSchedule:
    // ...
    break;
}
```


## Приватные свойства

Приватные свойства должны быть объявлены в расширении класса (class extension) в m-файле с реализацией класса. Именованные категории (такие, как `Private` или `private`) не следует использовать, если только не расширяем функциональность другого класса.
Анонимные категории могут быть явно открыты для тестирования, но следует соблюдать конвенцию об именовании: `<headerfile>+Private.h` или `<headerfile>+Testing.h`.

**Например:**

```objc
@interface DetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```

## Логические переменные

Objective-C использует `YES` и `NO`.  Соответственно, `true` и `false` должны быть использованы только в CoreFoundation, C и C++-коде.  Так как `nil` соответствует `NO`, нет необходимости явно делать с ним сравнение в условных операторах. Никогда не сравнивайте ничего с константой `YES`, ведь она определена как 1, а сам тип `BOOL` содержит 8 бит.

**Желательно:**

```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

**Нежелательно:**

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

Если требуется преобразовать указатель или число в булево значение вне оператора if, рекомендуется использовать двойное отрицание (!!) для приведения к константам `YES`/`NO`.

**Желательно:**
```objc
BOOL isDownloading = !!self.downloadRequest;
BOOL hasElements = !!self.elements.count;
```

**Нежелательно:**

```objc
BOOL isDownloading = self.downloadRequest;
BOOL hasElements = self.elements.count;
```

Так как сами переменные типа `BOOL` зачастую являются именами прилагательными, префикс “is” может быть опущен для свойств, но явно указан в его аксессоре, например:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Взято из [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

Однако в большинстве случаев префиксы `is`, `are` и другие должны присутствовать, так как, во-первых, это явно указывает принадлежность переменной к логическому типу, во-вторых, не у всех слов есть -able и другие суффиксы.

Многие условия могут быть переписаны в более простом виде. Следует упрощать, например, выражения, использующие двойное отрицание:
```objc
if (!(recordsCount<>0)) { ... // should be simplified
}
```

Переменные типа `NSNumber *` нужно не забывать приводить к типу `NSInteger`:
```objc
NSNumber *a = @(0);
if ([numberOfFucksGiven integerValue] == 0) {
  // ...
}
```


## Условные операторы

Тело условного оператора всегда должно быть помещено в операторные скобки, и первый же оператор в теле должен быть на новой строке после условия. Пренебрежение этими правилами может привести к ошибкам разного рода, например, [таким](http://programmers.stackexchange.com/a/16530).

**Желательно:**
```objc
if (!error) {
  return success;
}
```

**Нежелательно:**
```objc
if (!error)
  return success;
```

или

```objc
if (!error) return success;
```

Условие не должно быть большим, а если оно содержит много частей, то нужно выносить их в логические переменные. В результате условие должно читаться легко:

**Желательно:**
```objc
BOOL isNetworkOk = isWifiReachable || (isEthernetConnected && isEthernetConnectionEstablished);
BOOL hasGame = [self.games count] > 0;
if (isNetworkOk && hasGame) {
  [self playWithFriends];
}
```

**Нежелательно:**
```objc
if ((isWifiReachable || (isEthernetConnected && isEthernetConnectionEstablished)) && ([self.games count] > 0)) {
  [self playWithFriends];
}
```

### Тернарный оператор

Тернарный оператор `?:` должен быть использован, когда это повышает читаемость кода. Используйте его, когда он упрощает проверку с одним условием, если структура сложнее, имеет смысл использовать `if`.
В целом, лучшее использование тернарного оператора - в присваивании переменной и выборе значения для использования.

Переменные не логического типа должны быть с чем-нибудь сравнены, в этом случае используйте скобки для повышения читаемости. Если используется булева переменная, скобки не обязательны.

**Желательно:**
```objc
NSInteger value = 5;
result = (value != someValue) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

**Нежелательно:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Инициализаторы

Инициализаторы должны следовать предложенной Apple структуре. Возвращаемое значение также должно быть 'instancetype', а не 'id'.

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```

## Фабричные методы

При использовании фабричных методов (статические методы, выполняющие роль создателей нового экземпляра класса) убедитесь, что они возвращают 'instancetype', а не 'id'. Это помогает компилятору корректно выводить тип возвращаемого значения.

```objc
@interface Airplane
+ (instancetype)airplaneWithType:(AirplaneType)type;
@end
```

Больше информации об 'instancetype' можно почерпнуть на [NSHipster.com](http://nshipster.com/instancetype/).

## Методы CGRect

Когда требуется получить `x`, `y`, `width` или `height` от `CGRect`-объекта, можно использовать [`CGGeometry` функции](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) вместо прямого доступа к `frame` или `origin`.
Из документации Apple, раздел `CGGeometry`:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

Однако, если в рамках решаемой задачи необходимо использовать фрейм с отрицательными координатами или размером (например, во время анимации), то допустимо использование и прямое обращение к `view.frame`. 

**Желательно:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**Нежелательно:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

## Вложенность кода

При использовании множественных условных операторов важно следить за тем, чтобы код не был сильно вложенным. Правильным подходом является инвертирование условий, там, где это необходимо, для того, чтобы сразу выйти из метода или цикла.

**Желательно:**

```objc
- (void)someMethod {
  if (![someOther boolValue]) {
    return;
  }

  //Do something important
}
```

**Нежелательно:**

```objc
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

## Обработка ошибок

Когда метод возвращает параметр-ошибку по ссылке, необходимо переключаться прежде всего на возвращаемое значение, а не на этот параметр:

**Желательно:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**Нежелательно:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

Причина кроется в том, что API Apple зачастую записывают в данный параметр ненужную информацию, поэтому такая проверка (из "плохого" примера выше), может привести к "ложной тревоге" и обработке ошибки там, где ее на самом деле нет.


## Синглтоны

При создании синглтонов обязательно использовать потокобезопасный шаблон:
```objc
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```
Это поможет предотвратить [некоторые падения](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).


## Переносы строк

Если мы заботимся о читаемости кода, важно обратить внимание на переносы строк.

Для примера:
```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```
Большая длинная строка вроде этой должна быть разбита на две с последующей индентацией:
```objc
self.productsRequest = [[SKProductsRequest alloc]
  initWithProductIdentifiers:productIdentifiers];
```

## Именование ресурсов

* Иконки:
```
   ic_<имя_иконки>
```
Например, "ic_star".

* Элементы интерфейса
```
   <тип контрола>_<имя/цвет>_<state>
```
Например, "btn_login_normal" или "btn_green_pressed"

* Фоны
```
   bg_<имя>
```
Например, "bg_wood" или "bg_login".

## Однотипное именование понятий

Необходимо заранее условиться об именовании сущностей, имеющих прямое отношение к бизнес-логике приложения. Не должно, например, возникнуть ситуаций, когда корзина для покупок в коде существует как "bascet" и "shopping cart".

## Использование сторонних библиотек

Разрешается использование библиотек, распространяемых в рамках лицензий MIT, Apache (v2, v3), LGPL и другими, разрешающими использование в проприетарном программном обеспечении.
Подробнее о лицензиях и их отличиях можно почитать [тут](http://choosealicense.com/).
