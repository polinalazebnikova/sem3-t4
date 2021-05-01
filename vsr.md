# Вариативная самостоятельная работа

### [4.1. Создание таблицы со сравнительным анализом библиотек для тестирования.](https://www.dropbox.com/s/vlih2tlz1x8kcde/%D0%A2%D0%B5%D0%BC%D0%B0%204%20%D0%B2%D1%81%D1%80%204.1.docx?dl=0)
[Ссылка](https://www.dropbox.com/s/vlih2tlz1x8kcde/%D0%A2%D0%B5%D0%BC%D0%B0%204%20%D0%B2%D1%81%D1%80%204.1.docx?dl=0)
### [4.3. Разработать программу для считывания данных из JSON-файла и вывода их в табличном виде на экран и протестировать работоспособность с использованием unittest.](https://replit.com/@PolinaLazebniko/Tema4-VSR-43#main.py)
```python
"""
    Лазебникова Полина 
    ИВТ 2 курс
    группа 1.1

    Вариативная самостоятельная работа 
    4.3. Разработать программу для считывания данных из JSON-файла и вывода их в 
    табличном виде на экран и протестировать работоспособность с использованием unittest.
"""

def my_print(line = []):
    """
    Функция вывода таблицы
    
    В функцию передается массив, состоящий из ключа и значений, каждый из которых является массивом. 
    Каждый массив, переданного массива, состоит из одинакогово количества элементов.
    В функции составляется в цикле новый массив, состоящий из i-ых значений каждого начального массива. 
    Затем созданный массив выводится в виде строки таблицы.
    """
    lenCol = 30
    for i in range(len(line[0])):
        newArray = []
        for i2 in range(len(line)):
            newArray += [line[i2][i]]
        for i3 in range(len(newArray)):
            print(newArray[i3].ljust(lenCol,' '), end = "")
            print("|", end = "")
        print()
    print('-'*(lenCol + 1)*len(line))

def changeStr(str = ''):
    """
    Функция деления строки
    
    В функцию передается строка. Производится проверка ее длины, если длина переданной строки 
    больше заданного числа, то строка делится,на соответствующие части записываются в массив. 
    Если длина строки меньше или равна заданному числу, то функция возвращается массив, элементом 
    которого является изначальная строка.
    """
    lenStr = 30
    arrayStr = []
    if len(str) > lenStr:
        i = 0
        while i <= len(str):
            arrayStr += [str[i:i+lenStr]]
            i += lenStr
        return arrayStr
    else:
        return [str]

def addArray(array = []):
    """
    Функция для корректировки колличества элементов массива
    
    В функцию передается массив из массивов. Считаются минимальное и
    максимальное значения длин массивов. Если минимальное и максимальное значения не равны, 
    то каждый внутренний массив дополняется необходимым количеством элементов. Иначе возвращается изначальный массив.
    """
    if type(array[0][0]) == str:
        min = len(array[0])
        max = 0
        for i in array:
            if len(i) > max:
                max = len(i)
            elif len(i) < min:
                min = len(i)
        if min != max:
            for i in range(len(array)):
                if len(array[i]) < max:
                    for k in range(max-min):
                        array[i] += ['']
    elif type(array[0]) == list:
        for i in range(len(array)):
            array[i] = addArray(array[i])
    return array

def listKeys(dictList):
    """
    Функция для выделения ключей словаря в массив
    
    В функцию передаются ключи словаря.
    """
    keyList = []
    for key in dictList:
        keyList += [key]
    return keyList

import pytest
def test_digit(string):
    with pytest.raises(ValueError):
        int(string)

def is_digit(string):
    """
    Функция для определения является ли переданное значение числом
    
    В функцию передается строка.
    """
    if string.isdigit():
       return True
    else:
        try:
            float(string)
            return True
        except ValueError:
            return False

def testingType(line):
    """
    Функция для опрделения типа переданного значения и переделывание его в строку/массив

    В функцию передается значение неопределенного типа.
    """
    # если значение - логический тип
    if is_digit(str(line)) == True:
        return str(line)
    else:
        if line == True:
            line = 'Yes'
            return line
        elif line == False:
            line = 'No'
            return line

    # если значение - список или  кортеж или множество
    if isinstance(line, (list, tuple, set)):
        if type(line[0]) == dict:
            newList = []
            for each in line:
                newList += [testingType(each)]
            line = newList
            return line
        else:
            line = [str(i) for i in line]
            return line

    # если значение - словарь
    if type(line) == dict:
        keyDict = listKeys(line)
        newVal = []
        for each in keyDict:
            newVal += [testingType(line.get(each))]
        line = newVal

    return(line)

def changeArr(line):
    """
    Функция для преобразование массива с вложенными массивами
    
    В функцию передается массив из массивов. Функция возвращает массив, упрощенный на один уровень.
    """
    arr = []
    for i in range(len(line)):
        arr += line[i]
    return arr

def readFile():
    """
    Функция для считывания json

    В функции преобразуется исходный файл к виду, удобному для вывода в виде таблицы.
    """
    #считывание файла
    try:
        fileName = 'data.json'
        fileOpen = open(fileName)
        try:
            import json
        except ImportError as e:
            import json
            print("Problem with import json")
        data = json.load(fileOpen)
        #список всех ключей
        keyList = listKeys(data[0].keys())
        # преобразование файла
        for eachPart in data:
            for key in keyList:
                eachPart[key] = testingType(eachPart[key])
                if type(eachPart[key]) == str:
                    eachPart[key] = changeStr(eachPart[key])
                elif type(eachPart[key]) == list:
                    for i in range(len(eachPart[key])):
                        if type(eachPart[key][i]) == list:
                            for i2 in range(len(eachPart[key][i])):
                                eachPart[key][i][i2] = changeStr(eachPart[key][i][i2])
                        else:
                            [eachPart[key][i]] = changeStr(eachPart[key][i])

        for eachKey in keyList:
            array = [[eachKey]]
            for eachPart in data:
                if type(eachPart[eachKey][0]) == list:
                    while (type(eachPart[eachKey][0]) != str):
                        eachPart[eachKey] = changeArr(eachPart[eachKey])
                array += [eachPart[eachKey]]
            my_print(addArray(array))

    except FileNotFoundError as e:
        print("File not found")
    except IOError as e:
        print("Problem with input or output")
    except (KeyError, IndexError) as e:
        print("Problem with keys or indexes")

# readFile()

if __name__ == '__mane__':
    import unittest

    class TestOutputJson(unittest.TestCase):

        def test_changeArr(self):
            self.assertEqual(changeArr([[[1, 3, 4]], [[3, 6, 7]]]), [[1, 3, 4], [3, 6, 7]])
            self.assertEqual(changeArr([([1, 3, 4]), ([3, 6, 7])]), [1, 3, 4, 3, 6, 7])

        def test_is_digit(self):
            self.assertFalse(is_digit("Jin"))
            self.assertTrue(is_digit("97"))
            self.assertTrue(is_digit("9.7"))

        def test_changeStr(self):
            self.assertIs(changeStr('123456789123'), ['123456789123'])
            self.assertIs(changeStr('123456789123456789123456789123456789'), ['123456789123456789123456789123', '456789'])


    unittest.main(verbosity=1)
```
