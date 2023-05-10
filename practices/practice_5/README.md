# Абстрактная фабрика

```
class WindowFactory:
    @classmethod
    def create_window(cls, name):
        return cls.Window(name)
    @classmethod
    def create_button(cls, name):
        return cls.Button(name)

class MacOsFactory(WindowFactory):
    class Window:
        def __init__(self, name):
            self.name = name
            self.button = []
            self.style = 'Mac Os window style'
        def add_button(self, btn):
            self.button.append(btn.name)
        def show(self):
            print( '{} - {} and {}'.format(self.name, self.style, self.button))
    class Button:
        def __init__(self, name):
            self.name = name
            self.style = 'Mac Os button style'

class LinuxFactory(WindowFactory):
    class Window:
        def __init__(self, name):
            self.name = name
            self.button = []
            self.style = 'Ubuntu window style'
        def add_button(self, btn):
            self.button.append(btn.name)
        def show(self):
            print( '{} - {} and {}'.format(self.name, self.style, self.button))
    class Button:
        def __init__(self, name):
            self.name = name
            self.style = 'Ubuntu button style'

def create_dialog(factory):
    wind = factory.create_window('Form1')
    button = factory.create_button('Button1')
    wind.add_button(button)
    return wind


w = create_window(LinuxFactory)
w.show()
```
Диаграмма UML:

![](https://github.com/Smipos/TMP/blob/main/practices/practice_5/abstract_fb/fabrk.jpg)

# Builder

```
class Builder(object):
    def build_body(self):
        raise NotImplementedError()
 
    def build_lamp(self):
        raise NotImplementedError()
 
    def build_battery(self):
        raise NotImplementedError()
 
    def create_flashlight(self):
        raise NotImplementedError()
 
 
class Flashlight(object):
    def __init__(self, body, lamp, battery):
        self._shine = False  # излучать свет
        self._body = body
        self._lamp = lamp
        self._battery = battery
 
    def on(self):
        self._shine = True
 
    def off(self):
        self._shine = False
 
    def __str__(self):
        shine = 'on' if self._shine else 'off'
        return 'Flashlight [%s]' % shine
 
 
class Lamp(object):
    """Лампочка"""
 
 
class Body(object):
    """Корпус"""
 
 
class Battery(object):
    """Батарея"""
 
 
class FlashlightBuilder(Builder):
    def build_body(self):
        return Body()
 
    def build_battery(self):
        return Battery()
 
    def build_lamp(self):
        return Lamp()
 
    def create_flashlight(self):
        body = self.build_body()
        lamp = self.build_lamp()
        battery = self.build_battery()
        return Flashlight(body, lamp, battery)
 
 
builder = FlashlightBuilder()
flashlight = builder.create_flashlight()
flashlight.on()
print flashlight  # Flashlight [on]

```

# Адаптер

```
from abc import ABCMeta, abstractmethod
class IA(metaclass=ABCMeta):
    "An interface for an object"
    @staticmethod
    @abstractmethod
    def method_a():
        "An abstract method A"
class ClassA(IA):
    "A Sample Class the implements IA"
    def method_a(self):
        print("method A")
class IB(metaclass=ABCMeta):
    "An interface for an object"
    @staticmethod
    @abstractmethod
    def method_b():
        "An abstract method B"
class ClassB(IB):
    "A Sample Class the implements IB"
    def method_b(self):
        print("method B")
class ClassBAdapter(IA):
    "ClassB does not have a method_a, so we can create an adapter"
    def __init__(self):
        self.class_b = ClassB()
    def method_a(self):
        "calls the class b method_b instead"
        self.class_b.method_b()

ITEMS = [ClassA(), ClassB()]
for item in ITEMS:
    if isinstance(item, ClassB):
        item.method_b()
    else:
        item.method_a()

ITEMS = [ClassA(), ClassBAdapter()]
for item in ITEMS:
    item.method_a()
```

Диаграмма:
![](https://github.com/Smipos/TMP/blob/main/practices/practice_5/adapter/adap.jpg)