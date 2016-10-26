# Самые лучшие практики по написанию кода на Python

Перевод популярного gist - [The Best of the Best Practices (BOBP) Guide for Python][original]


## О главном

### Цитаты и ценности

- "Создавай другим такие инструменты, какими сам хотел бы пользоваться." - Kenneth Reitz
- "Простота всегда лучше функциональности." - Pieter Hintjens
- "Соответсвуй требованиям хотя бы на 90%. Игнорируй скептиков." - Kenneth Reitz
- "Красиво лучше, чем уродливо." - [PEP 20][]
- Пиши так, как-будто пишешь проект с открытыми исходниками (даже если проект таким не является).

### Основные рекомендации

- "Явное лучше, чем неявное." - [PEP 20][]
- "Читабельность важна." - [PEP 20][]
- "Кто угодно может поправить что угодно." - [Khan Academy Development Docs][]
- Исправляй каждый [недочет](http://www.artima.com/intv/fixit2.html) (неудачный дизайн, неправильное решение, неудачный код) *как только заметил его*.
- "Лучше сейчас, чем никогда." - [PEP 20][]
- Тестируй самым тщательным образом. Пиши документацию на каждую новую фишку.
- Even more important that Test-Driven Development--*Human-Driven Development*
- Эти рекомендации могут и, конечно, будут меняться.

## В частностях

### Стиль

Следуй стандарту [PEP 8][], когда возможно.

#### Имена

- Переменные, функции, методы, пакеты, модули
    - `маленькие_буквы_с_подчеркиванием_между_словами`
- Классы и исключения
    - `СБольшойБуквы`, `CamelCase`
- Protected-методы внутренние функции
    - `_начиная_с_нижнего_подчеркивания(self, ...)`
- Private-методы
    - `__начиная_с_двойного_подчеркивания(self, ...)`
- Константы
    - `ВСЕМИ_ЗАГЛАВНЫМИ_БУКВАМИ`

###### [1] Основные рекомендации по именованию

Избегай имен из одной буквы (особенно `l`, `O`, `I`). 

*Исключение*: В очень коротких блоках кода, когда значение явно видно из контекста:

**Сойдёт**
```python
for e in elements:
    e.mutate()
```

Избегай излишних ярлыков.

**Да**
```python
import audio

core = audio.Core()
controller = audio.Controller()
```

**Нет**
```python
import audio

core = audio.AudioCore()
controller = audio.AudioController()
```

Используй "обратную запись".

**Да**
```python
elements = ...
elements_active = ...
elements_defunct = ...
```

**Нет**
```python
elements = ...
active_elements = ...
defunct_elements ...
```

Избегай get-/set- методы.

**Да**
```python
person.age = 42
```

**Нет**
```python
person.set_age(42)
```

#### [2] Отступы

Используй 4 пробела. Табы - никогда. Достаточно разговоров.

#### Импорты

Импортируй модули целиком вместо отдельных его атрибутов. К примеру, модуль верхнего уровня `canteen` который содержит файл `canteen/sessions.py`,

**Да**

```python
import canteen
import canteen.sessions
from canteen import sessions
```

**Нет**

```python
from canteen import get_user  # Атрибут из canteen/__init__.py
from canteen.sessions import get_session  # Атрибут canteen/sessions.py
```

*Исключение*: Сторонние (third-party) модули, в документации которых явно указано импортировать отдельный атрибут. 

*Основание*: Предотвращает циклическое импортирование. Смотреть [тут](https://sites.google.com/a/khanacademy.org/forge/for-developers/styleguide/python#TOC-Imports).

Размещай все импорты вверху страницы в трёх разделах, каждый из который отделён пустой строкой в таком порядке:

1. Системные модули
2. Сторонние модули
3. Локальные (проектные) модули

*Основание*: Сразу видно откуда берётся модуль.

#### Документация

Следуй рекомендациям [PEP 257][PEP 257] по строкам документации. [reStructured Text](http://docutils.sourceforge.net/docs/user/rst/quickref.html) и [Sphinx](http://sphinx-doc.org/) помогут придерживаться этого стандарта.

Используй однострочное документирование для очевидных функций.

```python
"""Return the pathname of ``foo``."""
```

Многострочное документирование должно состоять из

- Краткое описание
- Варианты использования, при необходимости.
- Аргументы
- Возвращаемый тип и его семантика, за исключением когда возвращается ``None``

```python
"""Train a model to classify Foos and Bars.

Usage::

    >>> import klassify
    >>> data = [("green", "foo"), ("orange", "bar")]
    >>> classifier = klassify.train(data)

:param train_data: A list of tuples of the form ``(color, label)``.
:rtype: A :class:`Classifier <Classifier>`
"""
```

Примечания

- Используй глаголы ("Return") вместо описательных слов ("Returns").
- Делай описание `__init__` методов в строках документации классов.

```python
class Person(object):
    """A simple representation of a human being.

    :param name: A string, the person's name.
    :param age: An int, the person's age.
    """
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

##### [3] Про комментарии

Используй их в умеренном количестве. Читаемость самого кода важнее чем множество комментарий к нему. Зачастую, короткие методы эффективнее чем комментарии.

*Нет*

```python
# If the sign is a stop sign
if sign.color == 'red' and sign.sides == 8:
    stop()
```

*Да*

```python
def is_stop_sign(sign):
    return sign.color == 'red' and sign.sides == 8

if is_stop_sign(sign):
    stop()
```

Когда всё таки пишешь комментарии, помни про "Strunk and White apply." - [PEP 8][]

#### Длина строки

Не переживай по этому поводу. 80-100 символов будет просто супер.
Используй круглые скобки для продолжения строки.

```python
wiki = (
    "The Colt Python is a .357 Magnum caliber revolver formerly manufactured "
    "by Colt's Manufacturing Company of Hartford, Connecticut. It is sometimes "
    'referred to as a "Combat Magnum". It was first introduced in 1955, the '
    "same year as Smith & Wesson's M29 .44 Magnum."
)
```

### Тесты

Стремись к 100% охватываемости кода, но не будь одержим этим показателем.
Strive for 100% code coverage, but don't get obsess over the coverage score.

#### Основные рекомендации к тестам

- Используй длинные описательные имена. Зачастую это исключает необходимость писать строки документации в методах.
- Тесты должны быть изолированые. Не взаимодействуй с реальной базой данных или с сетью. Используй для этого отдельную базу данных которую можно удалить либо фиктивный (mock) объект.
- Используй лучше [фабрики](https://github.com/rbarrois/factory_boy) чем фикстуры.
- Никогда не давай незаконченным тестам успешно завершиться, иначе ты рискуешь просто забыть про них. Вместо этого, добавь заполнитель типа `assert False, "TODO: finish me"`.

#### Юнит тесты

- Сфокусированы на небольшом кусочке функциональности.
- Должны быть быстрые. Однако, медленные тесты лучше чем вообще без них.
- Имеет смысл делать один класс тест-кейса для одного класса модели.

```python
import unittest
import factories

class PersonTest(unittest.TestCase):
    def setUp(self):
        self.person = factories.PersonFactory()

    def test_has_age_in_dog_years(self):
        self.assertEqual(self.person.dog_years, self.person.age / 7)
```

#### [4] Функциональные тесты

Функциональные тесты это высокоуровневые тесты которые максимально отображают как конечный пользователь будет взаимодействовать с приложением. Они обычно используются для веб приложений и приложений с графическим интерфейсом.

- Пиши тесты как сценарий. Тест-кейсы и их методы должны звучать как описание к сценарию.
- Пиши комментарии как маленькие истории, *до написания кода тестирования*.

```python
import unittest

class TestAUser(unittest.TestCase):

    def test_can_write_a_blog_post(self):
        # Goes to the her dashboard
        ...
        # Clicks "New Post"
        ...
        # Fills out the post form
        ...
        # Clicks "Submit"
        ...
        # Can see the new post
        ...
```

Заметь, как в итоге звучит название тест-кейса и его метода "Test A User can write a blog post".


## Навеяно из...

- [PEP 20 (The Zen of Python)][PEP 20]
- [PEP 8 (Style Guide for Python)][PEP 8]
- [The Hitchiker's Guide to Python][python-guide]
- [Khan Academy Development Docs][]
- [Python Best Practice Patterns][]
- [Pythonic Sensibilities][]
- [The Pragmatic Programmer][]
- а так же из других байтов и битов...

[Pythonic Sensibilities]: http://www.nilunder.com/blog/2013/08/03/pythonic-sensibilities/
[Python Best Practice Patterns]: http://youtu.be/GZNUfkVIHAY
[python-guide]: http://docs.python-guide.org/en/latest/
[PEP 20]: http://www.python.org/dev/peps/pep-0020/
[PEP 257]: http://www.python.org/dev/peps/pep-0257/
[PEP 8]: http://www.python.org/dev/peps/pep-0008/
[Khan Academy Development Docs]: https://sites.google.com/a/khanacademy.org/forge/for-developers
[The Pragmatic Programmer]: http://www.amazon.com/The-Pragmatic-Programmer-Journeyman-Master/dp/020161622X/ref=sr_1_1?ie=UTF8&qid=1381886835&sr=8-1&keywords=pragmatic+programmer
[original]: https://gist.github.com/sloria/7001839
