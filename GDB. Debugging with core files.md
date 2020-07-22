# [Как посмотреть причину генерации core файла в gdb](https://www.opennet.ru/tips/940_gdb_debug_core.shtml)

***программа** - файл рухнувшей программы, собранной с включением отладочной информации
**core** - файл с core dump*

1. Запускаем gdb:
```bash
$ gdb
```
Указываем файл рухнувшей программы, собранной с включением отладочной информации
```gdb
  (gdb) программа
```
или
```bash
$ gdb программа
```

2. Указываем файл с core dump, будет показана причина и строка на которой приложение рухнуло
```gdb
  (gdb) core core  
```

3. Смотрим состояние стека вызовов до падения
```gdb
  (gdb) backtrace 1
```
  или
```gdb
  (gdb) backtrace 2
```
  или
```gdb
  (gdb) backtrace
```

4. Указываем номер фрейма из стека вызовов полученного на предыдущем шаге, который будем смотреть подробнее (показан как ***N***)
```gdb
  (gdb) frame N
```

5. Смотрим состояние переменных (в примере - ***result***)
```gdb
  (gdb) info locals
  (gdb) print result
  (gdb) whatis result
```

6. Команды, которые можно использовать для получения дополнительной информации
```gdb
  (gdb) info thread
  (gdb) info shared
  (gdb) info locals
  (gdb) info files
  (gdb) info variables
  (gdb) help info
```
 
7. Полезно также посомотреть на выполнении какого системного вызова происходит сбой используя программы: strace (http://strace.sourceforge.net), ltrace (для Linux) или ktrace и truss (входят в состав FreeBSD).
