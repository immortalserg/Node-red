***Функции Node-red***

**SunCalc**
функции получения солнечных циклов, использование в функции: var SunCalc = global.get('SunCalc'); // подключение библиотеки, остальное как у автора библиотеки https://github.com/mourner/suncalc

**lib.Clear("топик")**
функция получает имя устройства, удаляет из mqtt топика префикс, который задается в первой строке ноды Прикрепленное изображение и отбрасывает всё что после имени устройства начиная с косой черты.
топик "z2m/Button01/action" будет преобразован в "Button01"

**lib.ConvName("топик")**
функция преобразует топик в имя переменной, удаляет в начале префикс и заменяет все косые черты на подчеркивание.
топик "z2m/Sensor01/temperature" будет преобразован в Sensor01_temperature. Используется для формирования переменных.

**lib.Curtain("Имя устройства", pos, prefi)**
функция управления рольставнями или шторами.
Имя устройства - имя устройства рольставни или шторы.
pos - команда, может быть числом или одной из следующих строк: "UP", "DOWN", "STOP"
prefi - номер префикса в массиве setup.prefix, массив начинается с 0 (по умолчанию 0 - первый префикс)
если pos число то рольставням посылается команда установки позиции pos и формируется топик вида префикс+Имя устройства+"/set/position"
если одна из перечисленных строк тогда:
если рольставни имеют статус "STOP" то посылается команда pos
если рольставни имеют статус = команде pos то посылается команда "STOP"
если рольставни имеют статус противоположный команде pos то выполняется команда pos
публикуется в топик: Префикс+Имя устройства+"/set/state"

**lib.CurtainB("Имя устройства", prefi)**
функция управления рольставнями или шторами с помощью одной кнопки
Имя устройства - имя устройства рольставни или шторы.
prefi - номер префикса в массиве setup.prefix, массив начинается с 0 (по умолчанию 0 - первый префикс)
если рольставни имеют статус "STOP" и рольставни имеют позицию закрыто то посылается команда открыть, если позицию открыто то посылается команда закрыть, если промежуточную позицию то посылается команда противоположная той что была ранее послана через эту функцию перед остановкой.
если рольставни имеют статус не "STOP" то посылается команда "STOP"
публикуется в топик: Префикс+Имя устройства+"/set/state"

**lib.Dimmer ("Имя устройства", pos, state, proc, prefi)**
функция управление диммером
Имя устройства - имя устройства диммер.
pos - (не обязательный параметр) число - номер канала (1,2,3 ...), строка - имя канала ("left", "right")
если pos число то будет формироваться строка Префикс+Имя устройства+"state_l"+pos
если pos нет или "" то будет формироваться строка Префикс+Имя устройства+"state" (для одноканальных диммеров или led контроллеров)
state - (не обязательный параметр) статус, строка ("ON","OFF","TOGGLE", true, false или число от 1 до 255)
если нет то выполняется "TOGGLE", при чем если включен ночной режим (переменная setup_"+Имя устройства+"_night_auto"+pos равна "ON") и статус был "OFF" то примет значения яркости ночного режима который установлен в файловой переменной: "setup"+Имя устройства+"_night_brightness"+pos (от 1 до 100). для устройства c именем "Dimmer02" второй канал переменная для ночного режима этого диммера будет "setup_Dimmer02_night_brightness2"
если state число то устанавливается яркость на заданное число
proc - используется если state число, если есть параметр proc любое значение то state дано в процентах от 1 до 100 которое для управления диммером умножается на 2,54 и округляется
prefi - номер префикса в массиве setup.prefix, массив начинается с 0 (по умолчанию 0 - первый префикс)

**lib.DimmerB ("Имя устройства", pos, state, prefi)**
функция управления диммером режим диммирование
Имя устройства - имя устройства диммер.
pos - (не обязательный параметр) число - номер канала (1,2,3 ...), строка - имя канала ("left", "right")
если pos число то будет формироваться строка Префикс+Имя устройства+"state_l"+pos
если pos нет или "" то будет формироваться строка Префикс+Имя устройства+"state" (для одноканальных диммеров или led контроллеров)
state - число, уровень яркости, если не указан, то 254
prefi - номер префикса в массиве setup.prefix, массив начинается с 0 (по умолчанию 0 - первый префикс)


**lib.Relay ("Имя устройства", pos, state, prefi)**
функция управления реле.
Имя устройства - имя устройства диммер.
pos - (не обязательный параметр) число - номер канала (1,2,3 ...), строка - имя канала ("left", "right")
если pos число то будет формироваться строка Префикс+Имя устройства+"state_l"+pos
state - (не обязательный параметр) статус, строка ("ON","OFF","TOGGLE") если не указан то "TOGGLE"
prefi - номер префикса в массиве setup.prefix, массив начинается с 0 (по умолчанию 0 - первый префикс)


**lib.Battery (уровень заряда батареи)**
формирует html строку с Font Awesome символом батареи и окрашиванием в разные цвета в зависимости от уровня заряда батареи. 

**await lib.sleep(миллисекунды)**
задержка в миллисекундах

**lib.Sirena("Имя устройства", state, melody, prefi)**
функция управления сиреной
Имя устройства - имя устройства сирена, обязательно
state - (обязательный параметр) статус, строка ("ON","OFF") включает/выключает сирену
melody - (не обязательно) мелодия сирены
prefi - номер префикса в массиве setup.prefix, массив начинается с 0 (по умолчанию 0 - первый префикс)



v1.6
- добавлены функции SunCalc - рассчет солнечных циклов, встроена библиотека https://github.com/mourner/suncalc
- исправлено в функции реле режим TOGGLE
- исправлена работа ночного режима в функции Dimmer
  
v1.5
- добавлено управление сиреной
  
v1.4
- добавлена функция sleep
- добавлена возможность задавать несколько префиксов mqtt в переменной setup.prefix в виде массива, при переходе с версии 1.3 необходимо записать переменные как указано в ноде data
  
v1.3
- оптимизирован код
- отдельная функция сохранения данных о батареи

v1.2
- добавлено в функцию Dimmer установку яркости диммера

v1.1
- во все функции где возможно параметры "ON" или "OFF" можно вместо ON и OFF передавать true или false соответственно
- в ноду termostat добавлено указание датчиков темературы
  
v1.0
https://youtu.be/SIi30xbhhH8
