# Продолжаем

**1) Выведите содержимое fstab. Что хранится в fstab?**

```sh
cat /etc/fstab
```

В fstab хранится информация о том, что и куда нужно монтировать. Каждая строчка в файле описывает раздел, который нужно примонтировать к определенной точке.

Хранится информация о:

- UID или device - уникальный идентификатор или устройство (например, /dev/sda1).
- Точка монтирования - каталог, куда будет примонтирован диск (например, /mount).
- Тип файловой системы - например, ext4, ntfs, vfat и т. д.
- Параметры монтирования - такие как defaults, noatime, rw и другие.

![image](https://github.com/user-attachments/assets/c4ae7566-8adc-4491-bd5a-17885b1832d8)

**2) Добавьте в виртуальную машину ещё один диск**

1. Заходим в настройки виртуальной машины.
2. Добавляем новый виртуальный диск.
3. Перезагружаем виртуальную машину.

![image_2024-11-22_22-37-33](https://github.com/user-attachments/assets/529ca539-064a-408e-ab3a-a9ac4515f4e0)


**3) Узнайте как система видит ваш диск - выведите информацию о блочных устройствах**

```sh
lsblk
```

![image_2024-11-22_22-38-19](https://github.com/user-attachments/assets/a2606277-93f5-4212-bb3f-63f29c3a7cb8)


**4) С помощью полученной информации создайте на диске таблицу разделов и файловую систему ext4**

Создаем таблицу разделов и указываем необходимые настройки:

```sh
fdisk /dev/sdb
n
```

![image_2024-11-22_22-43-23](https://github.com/user-attachments/assets/6bbff875-0f81-479d-8462-99ddc2495696)

Задаем файловую систему ext4:

```sh
mkfs.ext4 /dev/sdb2
```

![image_2024-11-22_22-46-45](https://github.com/user-attachments/assets/9e2098dc-9a8c-4b27-85a9-42e0bfa0967a)


**5) Примонитруте диск в каталог /mnt**

```sh
mount [options] /dev/[устройство] [путь, куда надо примонтировать]
```

**Флаги:**

```
-V - вывести версию утилиты;
-h - вывести справку;
-v - подробный режим;
-a, --all - примонтировать все устройства, описанные в fstab;
-F, --fork - создавать отдельный экземпляр mount для каждого отдельного раздела;
-f, --fake - не выполнять никаких действий, а только посмотреть что собирается делать утилита;
-n, --no-mtab - не записывать данные о монтировании в /etc/mtab;
-l, --show-labels - добавить метку диска к точке монтирования;
-c - использовать только абсолютные пути;
-r, --read-only - монтировать раздел только для чтения;
-w, --rw - монтировать для чтения и записи;
-L, --label - монтировать раздел по метке;
-U, --uuid - монтировать раздел по UUID;
-T, --fstab - использовать альтернативный fstab;
-B, --bind - монтировать локальную папку;
-R, --rbind - перемонтировать локальную папку.
```

Более продвинутый вариант:

```sh
mount [options] -t [файловая система] -o [options] /dev/[устройство] [путь, куда надо примонтировать]
```

![image_2024-11-22_22-49-11](https://github.com/user-attachments/assets/3caad96c-2915-4d0b-8e85-786a9c47e29b)


**6) Зайдите в каталог и создайте там файлы**

![image_2024-11-22_22-51-49](https://github.com/user-attachments/assets/80f7a0fc-24aa-4d80-8db8-60748eb6977c)


**7) Отмонтируйте диск и проверьте остались ли файлы**

```sh
umount /mnt
```

Файлов нет, они существуют только на смонтированном разделе, после отмонтирования они исчезнут из /mnt.

![image_2024-11-22_22-55-02](https://github.com/user-attachments/assets/5efe7d8b-845a-4d34-b562-657298137394)

**8) Сделайте так, чтобы диск автоматически подключался при загрузке системы (добавьте информацию о нём в fstab)**

1. Получаем UUID диска:
```sh
blkid /dev/sdb2
```
2. Редачим файлик /etc/fstab:

```sh
UUID=[uuid] /mnt ext4 defaults 0 2
```

**9) Проверьте корретность записанных в fstab данных перед перезагрузкой**

Команда:

```sh
mount -a
```

не ругается, значит, все хорошо.

**10) Перезагрузите систему и убедитесь что диск был подключён к системе**

```sh
reboot now
lsblk
```

![image_2024-11-22_23-06-08](https://github.com/user-attachments/assets/56ea1f59-38e0-4a86-b220-5b709912a429)
