# Перенаправляем
**1) Как работают команды >, >>?**

\> - перенаправление вывода, например, в файл. Содержимое файла будет стерто.

Пример:
```sh
echo 'Hello!' > [file]
```
\>> - перенаправление вывода, но содержимое не будет стерто. Новая информация будет помещена в конец файла.
```sh
echo 'Hello!' >> [file]
```

**2) Что такое перенаправление ввода/вывода (stderr/stdout)?**

- stdin – 0, стандартный ввод (клавиатура).
- stdout – 1, стандартный вывод (экран).
- stderr – 2, стандартная ошибка (вывод ошибок на экран).

**3) Вывести содержание файла, не используя текстовые редакторы.**

```sh
cat [options] [file1] [file2]
```
*Флаги:*
```
-b - нумеровать только непустые строки.
-E - показывать символ $ в конце каждой строки.
-n - нумеровать все строки.
-s - удалять пустые повторяющиеся строки.
-T - отображать табуляции в виде ^I.
-h - отобразить справку.
-v - версия утилиты.
```

**4) Создать файл с содержимым, не используя текстовые редакторы.**

Запись в файл через перенаправление вывода:
```sh
echo "Some text" > [file]
```
С вводом с клавиатуры:
```sh
cat > [file]
```

**5) Перенаправить stdout в stderr и обратно на примере команды kinit, ping, tracert.**

stdout в stderr:
```sh
echo "Это сообщение стандартного вывода" 1>&2
```
stderr в stdout:
```sh
ls non_existing_file 2>&1
```
stdout в stderr через kinit:
```sh
kinit 1>&2
```
stderr в stdout через ping и tracert:
```sh
ping -c 4 example.com 2>&1
tracert google.com > output.txt 2>&1
```

**6) Чем отличаются stdout и stderr?**

stdout используется для вывода нормальной информации о выполнении команд (результаты выполнения), а stderr – для вывода сообщений об ошибках.

**7) Что такое stdin?**

stdin – стандартный поток ввода, который обычно используется для получения данных от пользователя. Обычно это клавиатура, но его можно перенаправить, например, из файла. Пример:
```sh
cat < input.txt
```

**8) Как отправить весь вывод команды в пустоту?**

```sh
command > /dev/null
```

**9) Можно ли отправить одновременно stdin и stdout в пустоту?**

```sh
cat 1>/dev/null
```