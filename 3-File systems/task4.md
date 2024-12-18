# Продолжаем

**1) Raid массивы, что такое и какие бывают?**

RAID (Redundant Array of Independent Disks) — это технология, которая объединяет несколько физических дисков в один логический массив для повышения скорости, надёжности или объёма. RAID делится на несколько уровней, каждый из которых имеет свои особенности.

Основные уровни RAID:

1. RAID 0 (Striping)

Цель: Повышение скорости.

Как работает: Данные разбиваются на блоки и записываются на все диски массива одновременно.

Минус: Нет отказоустойчивости — при отказе одного диска теряются все данные.

2. RAID 1 (Mirroring)

Цель: Отказоустойчивость.

Как работает: Данные зеркально копируются на все диски массива.

Минус: Потеря объёма, так как данные дублируются.

3. RAID 5

Цель: Компромисс между отказоустойчивостью и эффективным использованием дисков.

Как работает: Данные и информация о восстановлении (parity) распределяются по всем дискам. Требуется минимум 3 диска.

Минус: Более медленная запись, восстановление данных после сбоя может быть долгим.

4. RAID 10 (1+0)

Цель: Скорость и отказоустойчивость.

Как работает: Комбинация RAID 1 и RAID 0. Диски зеркалируются и распределяются по полосам.

Минус: Требуется минимум 4 диска, половина объёма теряется.

**2) Добавьте в виртуальную машину 2 диска, отформатируйте их в ext4**

![image_2024-11-22_23-18-49](https://github.com/user-attachments/assets/d6a4b2ae-f1cb-4927-beaa-d017c71bec7c)

```sh
mkfs.ext4 /dev/sdb2
mkfs.ext4 /dev/sdc1
```

![image_2024-11-22_23-25-27](https://github.com/user-attachments/assets/20cd65c8-67e1-4cbd-b8e8-6e8cfd560279)


**3) Создайте из них raid 0 массив.**

```sh
mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdb2 /dev/sdc1
```

![image_2024-11-22_23-29-43](https://github.com/user-attachments/assets/212aec00-aed8-4528-b4ee-2012fcdf987c)

Смотрим статус raid 0 массива:
```sh
cat /proc/mdstat
mdadm --detail /dev/md0
```
Создаем ФС ext4 на raid-массиве:
```sh
mkfs.ext4 /dev/md0
```
Монтируем:
```sh
mount /dev/md0 /mnt
```
Проверяем работоспособность:
```sh
df -h /mnt
```

**4) Проверьте всё ли работает.**

Для проверки создадим файлик в примонтированном Raid-массиве:

```sh
touch /mnt/testfile
```

**5) Удалите raid0 и создайте raid1.**

1. Отмонтируем и останавливаем raid0:
```sh
umount /mnt
mdadm --stop /dev/md0
```
2. Создаем raid1:
```sh
mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb2 /dev/sdc1
```
3. Проверяем:
```sh
cat /proc/mdstat
mdadm --detail /dev/md0
```
4. Форматируем и монтируем:
```sh
mkfs.ext4 /dev/md0
mount /dev/md0 /mnt
```
5. Проверяем работоспособность:
```sh
df -h /mnt
```

**6) В чём между ними разница?**

| **Характеристика** | **RAID 0** | **RAID 1** |
|----------------|--------|--------|
| Цель           | Увеличение скорости   | Отказоустойчивость  |
| Дублирование данных    | Нет   | Есть   |
| Объем дисков    | Полный суммарный объем   | Половина суммарного объема   |
| Отказоустойчивость | Нет | Один диск может выйти из строя |

**7) Есть ли файловые системы, которые поддерживают raid массивы без стороннего ПО?**

Btrfs:
- RAID0/1/10 на уровне ФС.
- Не нужен mdadm для работы.
ZFS:
- Имеет встроенные функции RAID-Z (аналог RAID5/6).
- Обеспечивает управление пулом дисков.

**8) Можно ли создать raid массив во время установки системы?**

Да, RAID можно настроить при установке системы через утилиты. Чаще всего, в установщике этот раздел называется "ручная разметка дисков".
