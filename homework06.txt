# Easy Задача-1: В задаче применил наследование.
# Написать класс для фигуры-треугольника, заданного координатами трех точек.
# Определить методы, позволяющие вычислить: площадь, высоту и периметр фигуры.


from math import sqrt

class Side:
     def width(self, A, B): # Ширина
         return round(sqrt(((B[0] - A[0]) ** 2) + ((B[1] - A[1]) ** 2)), 2)
     # round(number[, ndigits]) - округляет число number до ndigits знаков после запятой
     # (по умолчанию, до нуля знаков, то есть, до ближайшего целого).
     # >>> round(2.85, 1)
     # 2.9

class Triangle(Side): # Треугольник


     def __init__(self, A, B, C):
         self.A = A
         self.B = B
         self.C = C

     def perimeter(self):
         return self.width(self.A, self.B) + self.width(self.B, self.C) + self.width(self.A, self.C)

     def square(self):  # Площадь находим по формуле Герона
         half_per = self.perimeter() / 2
         a = self.width(self.A, self.B)
         b = self.width(self.B, self.C)
         c = self.width(self.A, self.C)
         return round(sqrt(half_per * (half_per - a) * (half_per - b) * (half_per - c)), 2)

     def altitude1(self):  # Из площади найдем  высоту №1
         a = self.width(self.A, self.B)
         return round(2 * self.square() / a, 2)

     def altitude2(self):  # Из площади найдем  высоту №2
         b = self.width(self.B, self.C)
         return round(2 * self.square() / b, 2)

     def altitude3(self):  # Из площади найдем  высоту №3
         c = self.width(self.A, self.C)
         return round(2 * self.square() / c, 2)


 el_1 = Triangle((1, 1), (0, 0), (1, 0))


# Задача-2:
# Написать Класс "Равнобочная трапеция", заданной координатами 4-х точек.
# Предусмотреть в классе методы:
# проверка, является ли фигура равнобочной трапецией;
# вычисления: длины сторон, периметр, площадь.

from math import sqrt

class Side:
     def width(self, A, B): # Ширина
         return round(sqrt(((B[0] - A[0]) ** 2) + ((B[1] - A[1]) ** 2)), 2)
     # round(number[, ndigits]) - округляет число number до ndigits знаков после запятой
     # (по умолчанию, до нуля знаков, то есть, до ближайшего целого).
     # >>> round(2.85, 1)
     # 2.9


class Trapeze(Side):
    def __init__(self, A, B, C, D):
        self.A = A
        self.B = B
        self.C = C
        self.D = D

   # Вычислим коэфф. наклона отрезков BC и AD и сравним их (k = (y2 - y1)/(x2 - x1)). А также равенство бок.сторон:
    def is_trapeze(self):
        try:
            k_BC = (self.C[1] - self.B[1]) / (self.C[0] - self.B[0])
            k_AD = (self.D[1] - self.A[1]) / (self.D[0] - self.A[0])
            AB = self.width(self.A, self.B)
            CD = self.width(self.C, self.D)
        except TypeError:
            print('Параметры заданы верно')
            return False
        if k_BC == k_AD:
            if AB == CD:
                return True
            else:
                print('Выполнено не верно')
                return False
        else:
            print('Этот четырехугольник не трапеция')
            return False

    def lenght_sides(self):
        if self.is_trapeze():
            AB = self.width(self.A, self.B)
            BC = self.width(self.B, self.C)
            CD = self.width(self.C, self.D)
            AD = self.width(self.A, self.D)
            return [AB, BC, CD, AD]
        else:
            return []

    def perimeter(self):
        if self.is_trapeze():
            return sum(self.lenght_sides())
        # sum(iter, start=0) - Сумма членов последовательности.

    def square(self):  # Площаль
        if self.is_trapeze():
            BC = self.width(self.B, self.C)
            AD = self.width(self.A, self.D)
            h = self.B[1] - self.A[1]
            return h * (BC + AD) / 2


# Normal Задание-1:
#
# Реализуйте описаную ниже задачу, используя парадигмы ООП:
# В школе есть Классы(5А, 7Б и т.д.), в которых учатся Ученики.
# У каждого ученика есть два Родителя(мама и папа).
# Также в школе преподают Учителя. Один учитель может преподавать
# в неограниченном кол-ве классов свой определенный предмет.
# Т.е. Учитель Иванов может преподавать математику у 5А и 6Б,
# но больше математику не может преподавать никто другой.

# Выбранная и заполненная данными структура должна решать следующие задачи:
# 1. Получить полный список всех классов школы
# 2. Получить список всех учеников в указанном классе
# (каждый ученик отображается в формате "Фамилия И.О.")
# 3. Получить список всех предметов указанного ученика
# (Ученик --> Класс --> Учителя --> Предметы)
# 4. Узнать ФИО родителей указанного ученика
# 5. Получить список всех Учителей, преподающих в указанном классе



class School:
   def __init__(self, school_name, school_adress, teachers, students):
        self._school_name = school_name
        self._school_adress = school_adress
        self._teachers = teachers
        self._students = students

    def get_all_classes(self):  # функция отображает список классов в которых учатся ученики.
        classes = set([student.get_class_room for student in self._students])
        return list(sorted(classes, key=lambda x: int(x[:-1])))
    

    def get_students(self, class_room):  # список студентов в классе.
        # Функция которая в случае совпадения дополнительной переменной class_room с классом конкретного студента
        # создаёт список студентов конкретного класса.
        return [student.get_short_name for student in self._students if
                class_room == student.get_class_room]
       

    def get_teachers(self, class_room):  # функция отображает список учителей, работающих в данном классе.
          # перебирается список учителей, и если у конкретного учителя есть нужный нам класс, то создается список
          # учителей работающих с данным классом.
          return [teacher.get_short_name for teacher in self._teachers if
                class_room in teacher.get_classes]

    def find_student(self, student_full_name):  # Словарь параметров каждого студента.
        for person in self._students:
            # Перебираем каждое имя в списке имен студентов.
            if student_full_name == person.get_full_name:
            # Если полное введенное имя в функцию совпадает с именем из списка имен студентов выполняем код ниже.
                teachers = [teachers.get_short_name for teachers in
                            self._teachers if person.get_class_room in
                            teachers.get_classes]
               
                lessons = [teachers.get_courses for teachers in
                           self._teachers if person.get_class_room in
                           teachers.get_classes]
                # Аналогично, получаем список дисциплин которые учит студент.
                parents = person.get_parents
                # получаем словарь родителей студента.


                return {
                    'full_name': student_full_name,
                    'class_room': person.get_class_room,
                    'teachers': teachers,
                    'lessons': lessons,
                    'parents': parents
                    }

    @property  
    
    def name(self):  #Имя школы
        return 'Муниципальное образовательное учреждение  ''"{}"'.format(self._school_name)

     @property
     def adress(self):  # Адрес школы.
        return '{}'.format(self._school_adress)


class People:
    def __init__(self, last_name, first_name, middle_name):
        self._last_name = last_name
        self._first_name = first_name
        self._middle_name = middle_name

    @property
    def get_full_name(self):
        return '{0} {1} {2}'.format(self._last_name,
                                    self._first_name,
                                    self._middle_name)

    @property
    def get_short_name(self):
        return '{0} {1}.{2}.'.format(self._last_name,
                                     self._first_name[:1],
                                     self._middle_name[:1])


class Student(People):
    def __init__(self, last_name, first_name, middle_name,
                 class_room, mather, father):
        People.__init__(self, last_name, first_name, middle_name)
        self._class_room = class_room
        self._parents = {
            'mather': mather,
            'father': father
            }

    @property
    def get_class_room(self):
        return self._class_room

    @property
    def get_parents(self):
        return self._parents


class Teacher(People):
    def __init__(self, last_name, first_name, middle_name, courses, classes):
        People.__init__(self, last_name, first_name, middle_name)
        self._courses = courses
        self._classes = classes

    @property
    def get_courses(self):
        return self._courses

    @property
    def get_classes(self):
        return self._classes


class Teacher(People):
    def __init__(self, last_name, first_name, middle_name, courses, classes):
        People.__init__(self, last_name, first_name, middle_name)
        self._courses = courses
        self._classes = classes

    @property
    def get_courses(self):
        return self._courses

    @property
    def get_classes(self):
        return self._classes


teachers = [
    Teacher('Жукова', 'Людмила', 'Александровна', 'Математика',
            ['7А', '7Б', '8А', '8Б', '9А', '9Б', '10А', '10Б', '11А', '11Б']),
    Teacher('Белик', 'Вячеслав', 'Валерьевич', 'Информатика',
            ['10А', '10Б', '11А', '11Б']),
    Teacher('Лакина', 'Надежда', 'Фёдоровна', 'История',
            ['7А', '7Б', '8А', '8Б', '9А', '9Б', '10А', '10Б', '11А', '11Б']),
    Teacher('Мордвинова', 'Татьяна', 'Владимировна', 'Литература',
            ['7А', '7Б', '8А', '8Б', '9А', '9Б', '10А', '10Б', '11А', '11Б']),
    Teacher('Лагута', 'Татьяна', 'Ивановна', 'География',
            ['7А', '7Б', '8А', '8Б', '9А', '9Б'])
    ]

students = [
    Student('Юрин', 'Александр', 'Александрович', '10Б',
            'Юрина О. А.', 'Юрин А. В.'),
    Student('Пантелеев', 'Евгений', 'Алексеевич', '11А',
            'Пантелеева Т.В.', 'Пантелеев А.В.'),
    Student('Мотовилов', 'Константин', 'Сергеевич', '11Б',
            'Мотовилова А.Д.', 'Мотовилов С.А.'),
    Student('Патосин', 'Виталий', 'Николаевич', '10А',
            'Патосина А.К.', 'Патосин Н.В.'),
    Student('Бочкин', 'Артём', 'Александрович', '9Б',
            'Бочкина В.А.', 'Бочкин А.Т'),
    Student('Никитин', 'Никита', 'Никитыч', '9А',
            'Никитина Н.А.', 'Никитин Н.С.'),
    Student('Ягодина', 'Дарья', 'Александровна', '8Б',
            'Ягодина А.В.', 'Ягодин А.С.'),
    Student('Панфилов', 'Андрей', 'Васильевич', '8А',
            'Панфилова Е.В.', 'Панфилов В.С.'),
    Student('Андреева', 'Анна', 'Владимировна', '7Б',
            'Андреева А.Д.', 'Андреев В.А.'),
    Student('Сливкин', 'Михаил', 'Аркадьевич', '7А',
            'Сливкина А.Г.', 'Сливкин А.С.'),
    Student('Циганков', 'Александр', 'Сергеевич', '10Б',
            'Цыгакова О. А.', 'Циганков С. В.'),
    Student('Цыганков', 'Алексей', 'Сергеевич', '11А',
            'Цыгакова О. А.', 'Циганков С. В..'),
    Student('Макарова', 'Ирина', 'Александровна', '11Б',
            'Макарова А.Д.', 'Макаров А.А.'),
    Student('Кравченко', 'Александр', 'Владимирович', '10А',
            'Кравченко Е.К.', 'Кравченко В.В.'),
    Student('Беляева', 'Мария', 'Вячеславовна', '9Б',
            'Беляева В.А.', 'Беляев В.Т'),
    Student('Рыбакова', 'Лидия', 'Константиновна', '9А',
            'Рыбакова Н.А.', 'Рыбаков К.С.'),
    Student('Абрамкин', 'Ярослав', 'Александрович', '8Б',
            'Абрамкина А.В.', 'Абрамкин А.С.'),
    Student('Стенькина', 'Анжелла', 'Васильевна', '8А',
            'Стенькина Е.В.', 'Стенькин В.С.'),
    Student('Садыкова', 'Лилия', 'Владимировна', '7Б',
            'Садыкова А.Д.', 'Садыков В.А.'),
    Student('Андросова', 'Анна', 'Сергеевна', '7А',
            'Андросова А.Г.', 'Андросов С.С.'),
    ]

school = School('Гимназия №8', '630068, г.Новосибирск, '
                 'ул.Ученическая 8', teachers, students)

print(school.name)
print(school.adress)
#
print('\nСписок классов школы:')  # Экранированные последовательность \n позволяют вставить
# специальные символ, который сложно ввести с клавиатуры, в данном случае это: новая строка, (перевод строки).

print(', '.join(school.get_all_classes()))  # join метод строк. В результате возвращается строка, полученная
# соединением элементов переданного списка в одну строку, при этом между элементами списка вставляется разделитель,
# равный той строке, к которой применяется метод.

print('\nСписок "10Б" класса:')
print('\n'.join(school.get_students('10Б')))

student = school.find_student('Юрин Александр Александрович')  # student словарь
print('\nУченик: {0}\nУчебный класс: "{1}"\n' 'Учителя: {2}\nПредметы: {3}'.format(
    student['full_name'],
    student['class_room'],
    ', '.join(student['teachers']),
    ', '.join(student['lessons'])))

print('Родители: {0}, {1}'.format(student['parents']['mather'], student['parents']['father']))
# Тут присутствует словарь в словаре.

print('\nКласс: "8А"\nПреподаватели: ''{0}'.format(', '.join(school.get_teachers('8А'))))




# Hard Задание-1:
# Решите задачу (дублированную ниже):
# Дана ведомость расчета заработной платы (файл "data/workers").
# Рассчитайте зарплату всех работников, зная что они получат полный оклад,
# если отработают норму часов. Если же они отработали меньше нормы,
# то их ЗП уменьшается пропорционально, а за заждый час переработки они получают
# удвоенную ЗП, пропорциональную норме.
# Кол-во часов, которые были отработаны, указаны в файле "data/hours_of"
# С использованием классов.
# Реализуйте классы сотрудников так, чтобы на вход функции-конструктора
# каждый работник получал строку из файла

# coding: utf-8
import os


class Person:  # Имя работника.
    # Класс человека с атрибутами: именем и фамилией, а также методами, позволяющии вставлять эти
    # атрибуты совместно или отдельно.
    def __init__(self, string):
        self._full_name = {
            'name': string.split()[0],
            'last_name': string.split()[1]
            }

    def name(self):
        return '{}'.format(self._full_name['name'])

    def lastname(self):
        return '{}'.format(self._full_name['last_name'])

    def full_name(self):
        return '{} {}'.format(self._full_name['last_name'], self._full_name['name'])




class Performance(Person):  # Зарплата работника.
    # Класс дает метод worked, который возвращает количество денег за норму часов работника.
    # string пишем по аналогии со строчками из файла workers от третьего ДЗ.
    def __init__(self, string):
        Person.__init__(self, string)
        self._hours = {
            'worked': string.split()[2]
        }
    # Разбивает строку на части, используя разделитель, и возвращает эти части списком. Направление
    # разбиения: слева направо.
    # примеры:  '   1   2   3   '.split()  # ['1', '2', '3']
    # '1,2,3'.split(',')  # ['1', '2', '3']

    def worked(self):  # метод возвращает часы работ сотрудника
        return '{}'.format(self._hours['worked'])


class PersonCard(Person):  # Все остльные данные из string
    # string пишем по аналогии со строчками из файла workers от третьего ДЗ.
    def __init__(self, string):
        Person.__init__(self, string)
        self._data = {
            'cash': string.split()[2],
            'job': string.split()[3],
            'norm': string.split()[4],
            'cph': (int(string.split()[2]) / int(string.split()[4])),
            }

    def cash(self):
        return '{}'.format(self._data['cash'])

    def job(self):
        return '{}'.format(self._data['job'])

    def norm(self):
        return '{}'.format(self._data['norm'])

    def cph(self):