# Лабораторная работа #3
# Тема: Объединения и перечисления

# Автор: Григорьев Артём        Группа: ПОО

# Задача 1 - указатель на объединение
## постановка задачи
Напишите программу, которая использует указатель на некоторое объединение (union). Создайте и проинициализируйте переменные в объединении через указатель, затем выведите их значения на экран.

## математическая модель

отсутствует

### Индетифткаторы
| Имя переменной | Тип данных | Смысловое обозначение |
|---------------|------------|----------------------|
| union Data | union | Объединение для разных типов данных |
| data | union Data | Переменная объединения |
| ptr | union Data* | Указатель на объединение |
| i | int | Целочисленное поле объединения |
| f | float | Вещественное поле объединения |
| str | char[20] | Строковое поле объединения |


# КОД
```c
#include <stdio.h>
#include <string.h>


union Data {
    int i;
    float f;
    char str[20];
};

int main() {
    union Data data;       
    union Data *ptr = &data; 
    
    printf("Работа с указателем на объединение:\n\n");
    
    
    ptr->i = 10;
    printf("Целое число: %d\n", ptr->i);
    

    ptr->f = 3.14;
    printf("Вещественное число: %.2f\n", ptr->f);
    
  
    strcpy(ptr->str, "Hello, World!");
    printf("Строка: %s\n", ptr->str);
    
    
    printf("\nДемонстрация разделяемой памяти:\n");
    ptr->i = 65; // ASCII код символа 'A'
    printf("Записали int: %d\n", ptr->i);
    printf("Читаем как char: %c\n", ptr->str[0]); // Первый байт целого числа
    
    return 0;
}
```
## результат
<img width="360" height="291" alt="image" src="https://github.com/user-attachments/assets/b3e54302-7abd-4765-92a3-cade51b4f453" />

# Задача 2 - побайтовая распечатка числа

## постановка задачи
Напишите программу, которая использует объединение (union) для побайтовой распечатки значения переменной типа
unsigned long. Каждый байт должен быть выведен отдельно через указатель на char.

## математическая модель

Побайтовый доступ: unsigned long value = 0x12345678 → байты: 0x78, 0x56, 0x34, 0x12

### Индетификаторы
| Имя переменной | Тип данных | Смысловое обозначение |
|---------------|------------|----------------------|
| union Number | union | Объединение для доступа к байтам |
| data | union Number | Переменная объединения |
| ptr | union Number* | Указатель на объединение |
| num | unsigned long | Число для распаковки |
| bytes | unsigned char[4] | Массив байтов числа |
| bytePtr | unsigned char* | Указатель для доступа к байтам |


# Код
```c
#include <stdio.h>


union Number {
    unsigned long num;      
    unsigned char bytes[4]; 
};

int main() {
    union Number data;
    union Number *ptr = &data;
    
    ptr->num = 0x12345678; 
    
    printf("Исходное число: 0x%lX\n", ptr->num);
    printf("Распаковка байтов через указатель:\n");
    
    
    unsigned char *bytePtr = (unsigned char*)&ptr->num;
    
    printf("Порядок байтов в системе:\n");
    for(int i = 0; i < 4; i++) {
        printf("Байт %d: 0x%02X\n", i, bytePtr[i]);
    }
    
   
    printf("\nЧерез массив в объединении:\n");
    for(int i = 0; i < 4; i++) {
        printf("Байт %d: 0x%02X\n", i, ptr->bytes[i]);
    }
    
    return 0;
}

```
# результат

Исходное число: 0x12345678

Распаковка байтов через указатель:

Порядок байтов в системе:

Байт 0: 0x78

Байт 1: 0x56

Байт 2: 0x34

Байт 3: 0x12


Через массив в объединении:

Байт 0: 0x78

Байт 1: 0x56

Байт 2: 0x34

Байт 3: 0x12


# Задача 3 - перечисление дней недели

## постановка задачи

Создайте перечислимый тип данных (enum) для семи дней недели. Реализуйте программу, которая выводит на экран
значения каждого дня недели как целое число.

## математическая модель

отсутствует

### индеификаторы
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| enum Day | enum | Перечисление дней недели |
| today | enum Day | Текущий день недели |
| MONDAY | int | Понедельник |
| TUESDAY | int | Вторник |
| WEDNESDAY | int | Среда |
| THURSDAY | int | Четверг |
| FRIDAY | int | Пятница |
| SATURDAY | int | Суббота |
| SUNDAY | int | Воскресенье |

# КОД
```c
#include <stdio.h>

// Перечисление дней недели
enum Day {
    MONDAY = 1,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
};

int main() {
    enum Day today;
    
    printf("Дни недели и их числовые значения:\n");
    
    
    today = MONDAY;
    printf("Понедельник: %d\n", today);
    
    today = TUESDAY;
    printf("Вторник: %d\n", today);
    
    today = WEDNESDAY;
    printf("Среда: %d\n", today);
    
    today = THURSDAY;
    printf("Четверг: %d\n", today);
    
    today = FRIDAY;
    printf("Пятница: %d\n", today);
    
    today = SATURDAY;
    printf("Суббота: %d\n", today);
    
    today = SUNDAY;
    printf("Воскресенье: %d\n", today);
    
    return 0;
}
```
# результат

<img width="326" height="221" alt="image" src="https://github.com/user-attachments/assets/2a396f14-595d-438a-a535-d96f000da0be" />


# Задача 4 - размеченное объединение

## постановка задачи
Создайте размеченное объединение (union), которое заключено в структуру. Структура должна также содержать перечисление (enum), служащее индикатором того, какой тип данных хранится в объединении на текущий момент. Создайте динамический массив таких структур и реализуйте функцию для распечатки их содержимого на экран.

## математическая модель 

отсутствует

### индетификаторы
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| enum DataType | enum | Тип данных в объединении |
| INT_TYPE | int | Целочисленный тип |
| FLOAT_TYPE | int | Вещественный тип |
| CHAR_TYPE | int | Символьный тип |
| struct TaggedUnion | struct | Размеченное объединение |
| type | enum DataType | Индикатор типа данных |
| data | union | Объединение для значений |
| int_value | int | Целочисленное значение |
| float_value | float | Вещественное значение |
| char_value | char | Символьное значение |
| tu1 | struct TaggedUnion | Первая переменная объединения |
| tu2 | struct TaggedUnion | Вторая переменная объединения |
| tu3 | struct TaggedUnion | Третья переменная объединения |
| print_union | function | Функция для печати содержимого |






# КОД
```c
#include <stdio.h>


enum DataType {
    INT_TYPE,
    FLOAT_TYPE, 
    CHAR_TYPE
};


struct TaggedUnion {
    enum DataType type;
    union {
        int int_value;
        float float_value;
        char char_value;
    } data;
};


void print_union(struct TaggedUnion *tu) {
    switch(tu->type) {
        case INT_TYPE:
            printf("Тип: INT, Значение: %d\n", tu->data.int_value);
            break;
        case FLOAT_TYPE:
            printf("Тип: FLOAT, Значение: %.2f\n", tu->data.float_value);
            break;
        case CHAR_TYPE:
            printf("Тип: CHAR, Значение: %c\n", tu->data.char_value);
            break;
    }
}

int main() {
    struct TaggedUnion tu1, tu2, tu3;
    
  
    tu1.type = INT_TYPE;
    tu1.data.int_value = 42;
    
    tu2.type = FLOAT_TYPE;
    tu2.data.float_value = 3.14f;
    
    tu3.type = CHAR_TYPE;
    tu3.data.char_value = 'A';
    
  
    printf("Содержимое размеченных объединений:\n");
    print_union(&tu1);
    print_union(&tu2);
    print_union(&tu3);
    
    return 0;
}
```
# результат
<img width="377" height="136" alt="image" src="https://github.com/user-attachments/assets/e6a9c116-1ee7-4894-9572-72638f2ca1a0" />


# Задача 5 - ввод и хранение данных о студентах

## постановака задачи
Создайте структуру, в которой используется объединение для хранения различных типов данных: например, структура с отдельным полем для имени студента и отдельное поле – целое число для его возраста либо строка его возраста
словами. Реализуйте программу для динамического ввода данных о студентах и вывода их на экран.

## математическая модель

нету!!!!!!!!!!!!!!1

### индетификаторы
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| union AgeData | union | Объединение для возраста |
| age_int | int | Возраст как число |
| age_str | char[20] | Возраст как строка |
| struct Student | struct | Структура студента |
| name | char[50] | Имя студента |
| age_type | int | Тип хранения возраста |
| age | union AgeData | Данные о возрасте |
| students | struct Student[3] | Массив студентов |
| n | int | Количество студентов |
| i | int | Счетчик цикла |

# КОД

```c
#include <stdio.h>
#include <string.h>


union AgeData {
    int age_int;
    char age_str[20];
};


struct Student {
    char name[50];
    int age_type; // 0 - число, 1 - строка
    union AgeData age;
};

int main() {
    struct Student students[3];
    int n;
    
    printf("Введите количество студентов: ");
    scanf("%d", &n);
    getchar(); 
    
    for(int i = 0; i < n; i++) {
        printf("\nСтудент %d:\n", i + 1);
        
        printf("Введите имя: ");
        fgets(students[i].name, 50, stdin);
        students[i].name[strcspn(students[i].name, "\n")] = 0; 
        
        printf("Выберите формат возраста (0 - число, 1 - строка): ");
        scanf("%d", &students[i].age_type);
        getchar(); 
        
        if(students[i].age_type == 0) {
            printf("Введите возраст (число): ");
            scanf("%d", &students[i].age.age_int);
        } else {
            printf("Введите возраст (строка): ");
            fgets(students[i].age.age_str, 20, stdin);
            students[i].age.age_str[strcspn(students[i].age.age_str, "\n")] = 0;
        }
        getchar(); 
    }
    
    printf("\nДанные студентов:\n");
    for(int i = 0; i < n; i++) {
        printf("\nСтудент %d:\n", i + 1);
        printf("Имя: %s\n", students[i].name);
        if(students[i].age_type == 0) {
            printf("Возраст: %d лет\n", students[i].age.age_int);
        } else {
            printf("Возраст: %s\n", students[i].age.age_str);
        }
    }
    
    return 0;
}
```
# результат

<img width="373" height="578" alt="image" src="https://github.com/user-attachments/assets/5fd3405e-da76-48d9-a6e8-3de484887bcb" />


# Задача 6 - управление состояниями системы через enum

## постановка задачи
Используйте перечисление (enum) для управления состояниями некоторой условной системы, например, старт, стоп,
пауза. Напишите программу, которая изменяет состояние системы и выводит текущее состояние на экран.

## математическая модель

отсутствует

### индетификаторы 
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| enum SystemState | enum | Перечисление состояний системы |
| START | int | Состояние старт |
| STOP | int | Состояние стоп |
| PAUSE | int | Состояние пауза |
| RUNNING | int | Состояние выполнение |
| ERROR | int | Состояние ошибка |
| IDLE | int | Состояние ожидание |
| current_state | enum SystemState | Текущее состояние системы |
| choice | int | Выбор пользователя |


# КОД

```c
#include <stdio.h>


enum SystemState {
    START,
    STOP, 
    PAUSE,
    RUNNING,
    ERROR,
    IDLE
};

int main() {
    enum SystemState current_state = START;
    int choice;
    
    printf("🚀 Управление состояниями системы! 🚀\n\n");
    
    do {
       
        printf("📊 Текущее состояние системы: ");
        switch(current_state) {
            case START: printf("START 🟢\n"); break;
            case STOP: printf("STOP 🔴\n"); break;
            case PAUSE: printf("PAUSE ⏸️\n"); break;
            case RUNNING: printf("RUNNING 🏃\n"); break;
            case ERROR: printf("ERROR ❌\n"); break;
            case IDLE: printf("IDLE 💤\n"); break;
        }
        
        
        printf("\nВыберите новое состояние:\n");
        printf("1 - START 🟢\n");
        printf("2 - STOP 🔴\n");
        printf("3 - PAUSE ⏸️\n");
        printf("4 - RUNNING 🏃\n");
        printf("5 - ERROR ❌\n");
        printf("6 - IDLE 💤\n");
        printf("0 - Выход\n");
        printf("Ваш выбор: ");
        scanf("%d", &choice);
        
       
        switch(choice) {
            case 1: current_state = START; break;
            case 2: current_state = STOP; break;
            case 3: current_state = PAUSE; break;
            case 4: current_state = RUNNING; break;
            case 5: current_state = ERROR; break;
            case 6: current_state = IDLE; break;
            case 0: printf("Выход из программы! 👋\n"); break;
            default: printf("Неверный выбор! ❌\n"); break;
        }
        printf("\n");
        
    } while(choice != 0);
    
    return 0;
}

```

# результат

🚀 Управление состояниями системы! 🚀

📊 Текущее состояние системы: START 🟢

Выберите новое состояние:

1 - START 🟢

2 - STOP 🔴

3 - PAUSE ⏸️

4 - RUNNING 🏃

5 - ERROR ❌

6 - IDLE 💤

0 - Выход

Ваш выбор: Выход из программы! 👋


# Задача 7 - оптимизация памяти для хранения данных о температуре и влажности

## постановка задачи
Создайте структуру с битовыми полями для хранения показаний температуры (в градусах Цельсия) и влажности (в
процентах). Реализуйте ввод данных с клавиатуры и их корректное хранение в структуре с минимальным потреблением памяти.

## математичекая модель
Битовые поля:

Температура: диапазон -50..+50 → 101 значение → 7 бит (2⁷=128)

Влажность: диапазон 0..100 → 101 значение → 7 бит (2⁷=128)

### индетификаторы
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| struct SensorData | struct | Структура с битовыми полями |
| temperature | int : 7 | Температура (-50 до +50) |
| humidity | unsigned int : 7 | Влажность (0-100%) |
| sensor | struct SensorData | Переменная структуры |
| temp_input | int | Ввод температуры |
| hum_input | int | Ввод влажности |

# КОД
```c
#include <stdio.h>


struct SensorData {
    int temperature : 7;   
    unsigned int humidity : 7;
};

int main() {
    struct SensorData sensor;
    int temp_input;
    int hum_input;
    
    printf("🌡️ Введите данные датчика:\n");
    
    printf("Температура (-50 до +50): ");
    scanf("%d", &temp_input);
    
    
    if (temp_input < -50 || temp_input > 50) {
        printf("Ошибка: температура вне диапазона!\n");
        return 1;
    }
    sensor.temperature = temp_input;
    

    printf("Влажность (0 до 100%%): ");
    scanf("%d", &hum_input);
    
   
    if (hum_input < 0 || hum_input > 100) {
        printf("Ошибка: влажность вне диапазона!\n");
        return 1;
    }
    sensor.humidity = hum_input;
    
    
    printf("\n📊 Данные сохранены в структуре:\n");
    printf("Температура: %d°C\n", sensor.temperature);
    printf("Влажность: %d%%\n", sensor.humidity);
    printf("Размер структуры: %zu байт\n", sizeof(sensor));
    
    return 0;
}
```
# результат

<img width="301" height="171" alt="image" src="https://github.com/user-attachments/assets/616005c4-c917-4d4d-bd6e-8b92deacf295" />


# Задача 8 - ввод и вывод через enum и union

## постановка задачи
Создайте программу, которая позволяет пользователю вводить и выводить информацию с различными типами данных
через перечисления и объединения. Например, пользователь может выбрать ввод данных как числа или строки, и
программа корректно сохранит и отобразит эти данные.

## математическая модель

нет

### индентификаторы
| Имя переменной | Тип данных | Смысловое обозначение |
|---|---|---|
| enum InputType | enum | Тип вводимых данных |
| NUMBER | int | Целочисленный тип |
| STRING | int | Строковый тип |
| FLOAT | int | Дробный тип |
| union DataValue | union | Объединение для значений |
| number | int | Целочисленное значение |
| string | char[100] | Строковое значение |
| decimal | float | Дробное значение |
| struct DataContainer | struct | Контейнер данных |
| type | enum InputType | Тип данных |
| value | union DataValue | Значение данных |
| data | struct DataContainer | Основная переменная |
| choice | int | Выбор пользователя |

# КОД

```c
#include <stdio.h>
#include <string.h>


enum InputType {
    NUMBER,
    STRING,
    FLOAT
};


union DataValue {
    int number;
    char string[100];
    float decimal;
};


struct DataContainer {
    enum InputType type;
    union DataValue value;
};

int main() {
    struct DataContainer data;
    int choice;
    
    printf("🎯 Ввод и вывод данных через enum и union! 🎯\n\n");
    
    
    printf("Выберите тип данных для ввода:\n");
    printf("1 - Целое число 🔢\n");
    printf("2 - Текст 📝\n");
    printf("3 - Дробное число 📊\n");
    printf("Ваш выбор: ");
    scanf("%d", &choice);
    getchar(); // очистка буфера
    
    
    switch(choice) {
        case 1:
            data.type = NUMBER;
            printf("Введите целое число: ");
            scanf("%d", &data.value.number);
            break;
            
        case 2:
            data.type = STRING;
            printf("Введите текст: ");
            fgets(data.value.string, 100, stdin);
            data.value.string[strcspn(data.value.string, "\n")] = 0; // удаляем \n
            break;
            
        case 3:
            data.type = FLOAT;
            printf("Введите дробное число: ");
            scanf("%f", &data.value.decimal);
            break;
            
        default:
            printf("Неверный выбор! ❌\n");
            return 1;
    }
    
    
    printf("\n📤 Результат:\n");
    printf("Тип данных: ");
    switch(data.type) {
        case NUMBER:
            printf("Целое число\n");
            printf("Значение: %d 🔢\n", data.value.number);
            break;
        case STRING:
            printf("Текст\n");
            printf("Значение: \"%s\" 📝\n", data.value.string);
            break;
        case FLOAT:
            printf("Дробное число\n");
            printf("Значение: %.2f 📊\n", data.value.decimal);
            break;
    }
    
    return 0;
}

```

# результат

<img width="247" height="194" alt="image" src="https://github.com/user-attachments/assets/d163bea0-8f72-4ae1-bca6-90699e8f2883" />











