# 3.2. Работа в терминале, лекция 2


## 1 Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.
    Команда cd является встроенной, работает только с окружением оболочки. 
    Если бы она была внешней, то она бы работала только со своим окружением и меняла 
    бы только текущий каталог своего окружения, а на вызвовший shell влияния бы не было. 

## 2 Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. Ознакомьтесь с документом о других подобных некорректных вариантах использования pipe.
    grep <some_string> <some_file> -c

## 3 Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?
    Процесс с PID 1 является systemd
    vagrant@vagrant:~/test$ pstree -p
    systemd(1)─┬─VBoxService(819)─┬─{VBoxService}(820)
               │                  ├─{VBoxService}(821)
               │                  ├─{VBoxService}(822)
               │                  ├─{VBoxService}(823)
               │                  ├─{VBoxService}(824)
               │                  ├─{VBoxService}(825)
               │                  ├─{VBoxService}(826)
               │                  └─{VBoxService}(827)

## 4 Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?

## 5 Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.

## 6 Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?

## 7 Выполните команду bash 5>&1. К чему она приведет? Что будет, если вы выполните echo netology > /proc/$$/fd/5? Почему так происходит?

## 8 Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? Напоминаем: по умолчанию через pipe передается только stdout команды слева от | на stdin команды справа. Это можно сделать, поменяв стандартные потоки местами через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.

## 9 Что выведет команда cat /proc/$$/environ? Как еще можно получить аналогичный по содержанию вывод?

## 10 Используя man, опишите что доступно по адресам /proc/<PID>/cmdline, /proc/<PID>/exe.

## 11 Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью /proc/cpuinfo.

## 12 При открытии нового окна терминала и vagrant ssh создается новая сессия и выделяется pty. Это можно подтвердить командой tty, которая упоминалась в лекции 3.2. Однако:

    vagrant@netology1:~$ ssh localhost 'tty'
    not a tty

    Почитайте, почему так происходит, и как изменить поведение.

## 13 Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись 'reptyr'. Например, так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.

## 14 sudo echo string > /root/new_file не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. Для решения данной проблемы можно использовать конструкцию echo string | sudo tee /root/new_file. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.
