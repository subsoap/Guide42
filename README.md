Guide42
========================
Guide42 - простая в использовании GUI библиотека для игрового движка [Defold](http://www.defold.com/).

Что доступно сейчас
-----------------------
Сейчас вы можете использовать динамический скролл, для однотипных по размеру объектов, добавлять объекты, удалять объекты а так же узнавать о касании по объекту.

Как использовать 
-------------------
Для примера я покажу, как добавить scroll в свой проект.

Для начала чтобы использовать библиотеку необходимо указать в настройках проекта зависимость dependencies: https://github.com/Bytonaaa/Guide42/archive/master.zip, 
а так же обновить все зависимость Project -> Fetch Libraries

Затем вы должны добавить template из папки Guide42 в свою GUI сцену.
В нашем случае мы добавим шаблон scroll_area.gui.
Необходимо так же подключить библиотеки в вашем файле gui_script

    local scroll_lib = require "Guide42.scroll_area"
    local vars = require "Guide42.guide42_variables"

модуль 'Guide42.scroll_area' отвечает за сам объект scroll, а модуль 'Guide42.guide42_variables' отвечает за константы библиотеки.

Так же, вы должны создать типовой объект, который будет лежать в scroll. Для корректной работы необходимо, чтобы pivot этого объекта находился по центру.

Теперь все готово, чтобы создать наш динамический скролл.
В методе `init` пропишите 

`self.scroll = scroll_lib.init("id шаблона в GUI сцене", settings)`

`settings` - объект, который регулирует настройки нашего скролла

    local settings = {
      clone_name = "box", --НЕОБЯЗАТЕЛЬНЫЙ. Название типового объекта, который будет существовать внутри scroll. По умолчанию 'guide42_scroll".
      horizontal = false, --НЕОБЯЗАТЕЛЬНЫЙ. Будет ли наш scroll вертикальным/горизонтальным. По умолчанию - горизонтальный
      between = 150, --НЕОБЯЗАТЕЛЬНЫЙ. Расстояние между объектами в scroll. По умолчанию 150
		  padding_horizontal = 50, --НЕОБЯЗАТЕЛЬНЫЙ. Расстояние от краев по горизонтали. По умолчанию 50
		  padding_vertical = 50,  --НЕОБЯЗАТЕЛЬНЫЙ. Расстояние от краев по вертикали. По умолчанию 50
		
		  touch_acceleration = 1, --НЕОБЯЗАТЕЛЬНЫЙ. Множитель, который регулирует скорость передвижения scroll при касании. По умолчанию 1
		  max_speed = nil, --НЕОБЯЗАТЕЛЬНЫЙ. Максимальная скорость scroll, которую можно развить. По умолчанию отсутсвует.
		  min_speed = 10, --НЕОБЯЗАТЕЛЬНЫЙ. Минимальная скорость, после которой scroll останавливается. По умолчанию 10
		  acceleration_down = 150, --НЕОБЯЗАТЕЛЬНЫЙ. С каким ускорением scroll будет тормозить. По умолчанию 150
		  touch_speed_border = 5, --НЕОБЯЗАТЕЛЬНЫЙ. Скорость перемещения касания, необходимое, для сдвига scroll. По умолчанию 5
      
      bind = bind -- bind - функция bind(node, data), которая принимает gui_tree_node node, и информацию data. Её задачей является установить правильный вид node, по информации data.
    }
***
Теперь можно вызывать методы через объект `self.scroll`. Доступные методы:

    self.scroll:add_node(data [, i]) -- Добавляет объект с данными data в scroll, в позицию i. По умолчанию добавляет в конец
    
    self.scroll:update(dt [, on_change]) --Должно вызываться из update. dt - время прошедшее с последнего вызова. on_change (value) функция срабатывающая при изменении положения scroll
    и принимающая значение value в диапозоне [1, n]. Если объектов в scroll нет, то on_change не вызывается.
    
    self.scroll:stop() --Останавливает движение scroll
    
    self.scroll:remove_node(i) --Удаляет объект с номером i из scroll
   
    self.scroll:remove_all() --Удаляет все объекты из scroll
    
    local obj = self.scroll:input(vars.touch, action) --Вызывается из метода on_input. Принимает первым аргументом тип касания из модуля "Guide42.guide42_variables", а вторым аргументом информацию о касании.
    Возвращает объект, в поля которых может быть:pressed, если произошло нажатие на скролл
    released, если касание от скролла было завершено 
    key, номер объекта, который был нажат (если не был нажат ни один, тогда параметр не возвращается)
    touched, двигает ли касание scroll или нет.
    
    self.scroll:get_node(i) --Возвращает объект gui_tree_node номера i или nil, если данный объект за пределами видимости.
    
    self.scroll:get_data(i) --Возвращает данные по номеру i
    
    self.scroll:get_size() --Возвращает размер scroll
    
    self.scroll:get_value() --Возвращает позицию scroll в диапозоне [1, n] или nil, если объектов в scroll нет
    
    self.scroll:get_moved() --Движется ли scroll или нет в данный момент.



