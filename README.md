# Лабораторная работа 1

## ГАИ - Задание 19
Имеются автомобили (марка автомобиля, серия и номер технического паспорта, государственный номер, номер двигателя, номер кузова, владелец, адрес владельца), водители (фамилия, имя и отчество, адрес, серия и номер водительского удостоверения) и нарушения (название нарушения). Накапливается информация о нарушителях, являющихся водителями определенных автомобилей, совершивших определенное нарушение в определенную дату. 

**Выходные документы:**
- Выдать список автомобилей с указанием количества нарушений за определенный интервал дат, отсортированный по маркам и ФИО владельцев 
- Выдать список водителей, совершивших определенный вид нарушения на автомобилях определенной марки, отсортированный по ФИО водителей и номерам автомобилей.

**Er-диаграмма**
![](photos/ГАИ.jpg)

# Лабораторная работа 2

## Логическая модель
**Сущности:**
    PK - первичный ключ, FK - внешний ключ.
    
    Автомобили (venicle):
        - автомобиль (venicleId, PK)
        - марка автомобиля (make)
        - серия технического паспорта (seriesPassport)
        - номер технического паспорта (numberPassport)
        - государственный номер (stateNumber)
        - номер двигателя (engineNumber)
        - номер кузова (bodyNumber)
        - водитель (driverId, FK к сущности "Водители")
        - адрес владельца (driverAdress)
        
    Водители (driver):
        - водитель (driverId, PK)
        - имя (firstName)
        - фамилия (lastName)
        - отчество (patronymic)
        - адрес (address)
        - серия водительского удостоверения (seriesLicence)
        - номер водительского удостоверения (numberLicence)
        
    Нарушения водителя (driver_violation):
        - нарушения водителя (driver_violationId, PK)
        - нарушение (violationId, FK к сущности "Нарушения")
        - автомобиль (venicleId, FK к сущности "Автомобили")
        - водитель (driverId, FK к сущности "Водители")
        - дата (date)
        
    Нарушения (violation):
        - нарушение (violationId, PK)
        - название нарушения (name)

    Взаимосвязи:
        - "Водители" и "Автомобили": Связь один-ко-многим (один водитель может быть связан с несколькими автомобилями).
        - "Автомобили" и "Нарушения водителя": Связь ноль-ко-многим (один автомобиль может быть не связан или связан с несколькими нарушениями водителя).
        - "Водители" и "Нарушения водителя": Связь ноль-ко-многим (один водитель может быть не связан или связан с нарушениями водителя).
        - "Нарушения" и "Нарушения водителя": Связь ноль-ко-многим (один нарушение может быть не связан или связан с несколькими нарушениями водителя).

## Физическая модель
```sql
CREATE TABLE driver (
	driver_id SERIAL PRIMARY KEY,
	firstName VARCHAR(20),
	lastName VARCHAR(20),
	patronymic VARCHAR(20),
	address VARCHAR(60),
	seriesLicence VARCHAR(10),
	numberLicence VARCHAR(10)
);

CREATE TABLE vinecle (
	venicle_id SERIAL PRIMARY KEY,
	make VARCHAR(20),
	seriesPassport VARCHAR(20),
	numberPassport VARCHAR(20),
	stateNumber VARCHAR(20),
	engineNumber VARCHAR(40),
	bodyNumber VARCHAR(20),
	driver_id SERIAL,
	FOREIGN KEY (driver_id) REFERENCES driver(driver_id),
	driverAdress VARCHAR(60)
);

CREATE TABLE violation (
	violation_id SERIAL PRIMARY KEY,
	name TEXT NOT NULL
);

CREATE TABLE driver_violation (
	driver_violation_id SERIAL PRIMARY KEY,
	violation_id SERIAL NOT NULL,
	FOREIGN KEY (violation_id) REFERENCES violation(violation_id),
	venicle_id SERIAL NOT NULL,
	FOREIGN KEY (venicle_id) REFERENCES vinecle(venicle_id),
	driver_id SERIAL NOT NULL,
	FOREIGN KEY (driver_id) REFERENCES driver(driver_id),
	date DATE NOT NULL
);
```

## DLL-запросы:
### Создание таблиц
![](photos/createTables.jpg)

### Заполнение таблиц:
**driver**
![](photos/driver.jpg)

**vinecle**
![](photos/vinecle.jpg)

**violation**
<br>
![](photos/violation.jpg)
    
**driver_violation**
![](photos/driver_violation.jpg)

### Проверка наличия таблиц:
**driver**
![](photos/driverSelect.jpg)

**vinecle**
![](photos/venicleSelect.jpg)

**violation**
<br>
![](photos/violationSelect.png)
    
**driver_violation**
<br>
![](photos/driver_violationSelect.jpg)

## SELECT-запросов:
### Выдать список автомобилей с указанием количества нарушений за определенный интервал дат, отсортированный по маркам и ФИО владельцев:
![](photos/firstRequest.jpg)

### Выдать список водителей, совершивших определенный вид нарушения на автомобилях определенной марки, отсортированный по ФИО водителей и номерам автомобилей:
![](photos/secondRequest.jpg)

# Лабораторная работа 3

## Представления для выходных данных:
![](photos/views.jpg)
### Выдать список автомобилей с указанием количества нарушений за определенный интервал дат, отсортированный по маркам и ФИО владельцев:
![](photos/view1.jpg)
### Выдать список водителей, совершивших определенный вид нарушения на автомобилях определенной марки, отсортированный по ФИО водителей и номерам автомобилей:
![](photos/view2.jpg)

## Процедуры:
![](photos/procedures.jpg)
### Вывод водителя из выходных данных
![](photos/procedure_driver_from_violations_period.jpg)
### Добавление водителя в базу данных
![](photos/procedure_add_to_driver.jpg)

![](photos/procedure_add_to_driver_complete.jpg)
### Удаление водителя из базы данных
![](photos/procedure_remove_from_driver.jpg)

![](photos/procedure_remove_from_driver_complete.jpg)
### Варианты работ процедур при ELSE
![](photos/procedure_add_remove_else.png)

## Представление сложных запросов при помощи представления:
![](photos/view1_optimization.jpg)
![](photos/view2_optimization.jpg)

# Лабораторная работа 4
## Генератор driver:
![](photos/generator_driver.jpg)
![](photos/generator_driver_call.jpg)
![](photos/generator_driver_complete.jpg)

## Генератор venicle:
![](photos/generator_venicle.jpg)
![](photos/generator_venicle_call.jpg)
![](photos/generator_venicle_complete.jpg)

## Генератор violation:
![](photos/generator_violation.jpg)
![](photos/generator_violation_call.jpg)
![](photos/generator_violation_complete.jpg)

## Генератор driver_violation:
![](photos/generator_driver_violation.jpg)
![](photos/generator_driver_violation_call.jpg)
![](photos/generator_driver_violation_complete.jpg)


## Анализ планов выполнения запросов
### Первый SELECT-запрос выходных данных (время выполнения 22ms)
![](photos/explain_before_optimization.jpg)
### Создание индексов для оптимизации
![](photos/createIndexes.jpg)
### SELECT-запрос после оптимизации (время выполнения 11ms)
![](photos/explain_after_optimization.jpg)

# Лабораторная работа 5



