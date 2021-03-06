# 3.2. Работа в терминале, лекция 2


## 1. Какого типа команда cd? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.
    Команда cd является встроенной, работает только с окружением оболочки. 
    Если бы она была внешней, то она бы работала только со своим окружением и меняла 
    бы только текущий каталог своего окружения, а на вызвовший shell влияния бы не было. 

## 2. Какая альтернатива без pipe команде grep <some_string> <some_file> | wc -l? man grep поможет в ответе на этот вопрос. Ознакомьтесь с документом о других подобных некорректных вариантах использования pipe.
    grep -c <some_string> <some_file>

## 3. Какой процесс с PID 1 является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?
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

## 4. Как будет выглядеть команда, которая перенаправит вывод stderr ls на другую сессию терминала?
    ls error 2> /dev/pts/6

## 5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.
    cat <test_1 >test_2

## 6. Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?
    Да, это возможно сделать между терминалами.
    
   ![image1](https://i.ibb.co/VNjGv45/image.png)
   ![image2](https://i.ibb.co/5619NBn/image.png)

## 7. Выполните команду bash 5>&1. К чему она приведет? Что будет, если вы выполните echo netology > /proc/$$/fd/5? Почему так происходит?
    bash 5>&1 - приведт к тому, что дискриптор 5 будет напрвлен на дискриптор 1 (он же stdout)
    echo netology > /proc/$$/fd/5 - выведет дискриптор 5
    
    vagrant@vagrant:~$ bash 5>&1
    vagrant@vagrant:~$ echo netology > /proc/$$/fd/5
    netology
    

## 8. Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? Напоминаем: по умолчанию через pipe передается только stdout команды слева от | на stdin команды справа. Это можно сделать, поменяв стандартные потоки местами через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.
    5>&2 2>&1 1>&5
    дискриптор 5 перенаправит в stderr, далее stderr перенаправит в stdout, после stdout перенапривт в дискриптор 5
## 9. Что выведет команда cat /proc/$$/environ? Как еще можно получить аналогичный по содержанию вывод?
    Получим переменные окружения. Аналогичный вывод можно получить через команды printenv и env

## 10. Используя man, опишите что доступно по адресам /proc/<PID>/cmdline, /proc/<PID>/exe.
    /proc/<PID>/cmdline (строка 243) - файл содержит командую строку, которым был запущен процесс. 
    /proc/<PID>/exe (строка 301) - содержит символическую ссылку на исполняемый файл инициирующий запуск процесса.
    
## 11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью /proc/cpuinfo.
    cat /proc/cpuinfo | grep sse
    sse, sse2, sse4_1, sse4_2, sse4a
    sse4a - наиболее старшая версия
    
## 12. При открытии нового окна терминала и vagrant ssh создается новая сессия и выделяется pty. Это можно подтвердить командой tty, которая упоминалась в лекции 3.2. Однако:

    vagrant@netology1:~$ ssh localhost 'tty'
    not a tty
    
Почитайте, почему так происходит, и как изменить поведение.
    
    Скорее всего нет локального tty. При запросе к подключению к ssh вызывается (ожидается) пользователь и не выделяется pty. 
    Если кратко, запрос к pty происходит раньше чем авторизуется пользователь, из-за этого получаем ответ not a tty.
    
    ssh localhost 'tty' нужно заменить на ssh localhost

## 13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись 'reptyr'. Например, так можно перенести в screen процесс, который вы запустили по ошибке в обычной SSH-сессии.
    reptyr не был предустановлен, поэтому для начала нужно его установить
    Ubuntu - 'sudo apt install reptyr'
    Далее запускаем процесс 'top'
    Теперь необходимо перевести данный процесс в фоновый режим 'CTRL + Z'
    Возобновляем работу в фоновом режиме 'bg'
    Далее смотрим запущенные фоновые задания 'jobs -l' Здесь мы увидим гарантированный PID нашего top '[1]+ 2113 Stopped (signal) top'
    Отказываемся от текущего родителя 'disown top'
    Запускаем screen или любой другой терминальный мультиплексор и подключаемся к процессу 'reptyr 2113'
    Теперь из другого терминала мы можем увидеть что процесс продолжает работать 'ps aux | grep top'
    vagrant@vagrant:~$ ps aux | grep top
    vagrant     2113  0.0  0.3   9248  3836 pts/0    T    09:29   0:00 top
    
    
## 14. sudo echo string > /root/new_file не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без sudo под вашим пользователем. Для решения данной проблемы можно использовать конструкцию echo string | sudo tee /root/new_file. Узнайте что делает команда tee и почему в отличие от sudo echo команда с sudo tee будет работать.
    Команда 'tee' делает вывод одновременно в файл и в stdout 
    В этом примере получим вывод из stdin, перенаправленный через pipe от stdout команды echo. 
    Команда у нас запущена через sudo поэтому она даст права на запись в файл.
    
