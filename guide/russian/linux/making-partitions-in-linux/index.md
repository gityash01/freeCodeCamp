---
title: Partitions
localeTitle: Перегородки
---## ПЕРЕГОРОДКИ

*   Не создавая разделы на жестком диске, мы не можем создать папку.
    
*   Разделы в Linux-
    
    *   **Д.Ф.**
    *   **df -h** (читаемый человеком)
*   Показывает размер в MiB, GiB
    
*   **lvdisplay**
    
*   Показывает информацию о разделе диска -
    
*   **fdisk -l**
    
*   **fdisk -l / dev / sda** (sda - это имя жесткого диска)
    
*   Для практического раздела вставьте виртуальный жесткий диск в Linux с помощью виртуального окна.
    
*   Некоторые моменты, чтобы помнить
    
*   Жесткий диск не понимает GB или MB, это единица является сектором.  
    1 сектор = 512 байт.  
    Чтобы найти фактический размер жесткого диска, найдите число секторов \* 512.  
    Выделяет пространство на жестком диске в секторах.
    
*   На одном жестком диске может быть создано всего 4 раздела.
    

  

## Создание разделов в жестком диске

1.  Открывает приглашение на жесткий диск.
    
    *   **fdisk / dev / sdb**
2.  Печать разделов на жестком диске.
    
    *   **п**
3.  Создайте новый раздел.
    
    *   **N**
4.  Выберите основной раздел.
    
5.  Нажмите 1 (1-й раздел)
    
6.  Начальные некоторые сектора (0-2047 = 2048 секторов = 1 МБ) зарезервированы на жестком диске.
    
7.  Фактическое пространство начинается с 2048-го сектора.
    
8.  **\+ 1G**
    
9.  Создайте 4 раздела, подобные этому.
    
10.  После 4-го раздела вы не можете создавать больше разделов.
    
11.  Нажмите **w** для сохранения.
    
12.  Нажмите **q** для выхода без сохранения раздела. Это приведет к удалению всех разделов, поскольку это временно.
    
13.  Для удаления раздела -
    
    *   **d**

  

## ПОЧЕМУ ОГРАНИЧИВАЕТСЯ 4?

*   Потому что, где мы храним информацию о разделах, метаданные разделов, является фиксированным и имеет 64 байта. Эта информация хранится в таблице разделов.
*   1 требуется 16 байт, поэтому можно создать только 4 раздела.
*   В 1 Мбайт (2048 секторов), зарезервированных на жестком диске, зарезервировано 64 байта для хранения этой информации.
*   Чтобы увидеть таблицу разделов,
*   **fdisk -l**
*   Когда мы впервые используем жесткий диск, он инициализируется или форматируется. Этот формат определяет количество разделов на жестком диске.
*   ОС создает формат жесткого диска, когда он был сначала инициализирован, и этот формат определяет количество разделов.
*   Формат раздела, который мы используем, - это формат DOS = 64 байта
*   Формат GPT = 128 разделов.
*   Таблица разделов -> формат: DOS -> 4 раздела
*   Таблица разделов -> формат: GPT -> 128 разделов

  

## ПОВЫШЕНИЕ ЧИСЛА ПАРТИЙ

*   Создайте новую таблицу разделов на жестком диске.
    
*   P4 будет создан таким образом, что это отдельный жесткий диск.
    
*   Этот раздел является расширенным разделом, в котором мы можем создавать больше разделов.
    
*   Логический раздел занимает пространство в расширенном разделе. Информация или таблица разделов будут сохранены в расширенном разделе.
    
*   Создайте 3 первичных и последний 1 расширенный раздел.
    
*   Всего 64 раздела можно сделать сейчас. 3 первичных + 60 логических + 1 расширенных.
    
*   Но для хранения данных можно использовать 63 раздела (удалите один расширенный раздел).
    
*   Нет никакой разницы между основным и логическим разделами, за исключением того, что никто не контролирует первичный, а логический управляется расширенным. Поэтому, если мы удалим расширенный раздел, все логические разделы будут удалены.
    
*   Чтобы просмотреть информацию о разделах -
    
*   **lsblk** (список блоков устройств)
    
*   Жесткий диск также известен как блочные устройства
    
*   Расширенный раздел не может использоваться для хранения данных, только логические и первичные могут использоваться.
    
*   Если раздел необходимо использовать для хранения, выполните следующие три шага -
    

1.  Создайте физический раздел.
2.  Отформатируйте его.
3.  Активировать / крепление.

  

## ФОРМАТОВАЯ ЧАСТЬ

*   Для более быстрой обработки раздел должен создать индекс для каждого нового файла.
    
*   Всякий раз, когда файл должен быть открыт, найдите этот файл в индексе.
    
*   Этот индекс формируется в разделе при его форматировании в первый раз. Он также называется файловой системой.
    
*   Эта таблица индексов известна как индекс inode (индексный узел).  Таким образом, каждый раздел должен быть отформатирован.
    
*   ОС читает только таблицу inode, чтобы отображать размеры папок, файлов, дисков и т. Д.
    
*   Эта таблица inode может быть изменена, тогда ОС будет показывать разные размеры, а не фактический размер.
    
*   Когда мы удаляем файл, он удаляет только запись inode этого файла.
    
*   Например, удалите файл размером 1 ГБ и 100 ГБ, время будет таким же.
    
*   Когда мы отформатируем раздел, он удаляет только индексную страницу, данные не будут удалены. Таким образом, мы можем восстановить данные из этого раздела.
    
*   Файловая система создает таблицу inode для управления файлами.
    
*   Для форматирования раздела -
    
*   **mkfs.ext4 / dev / sdb1**
    
*   Пример: NTFS, ext2, ext3, ext4, xfs и т. Д.
    

  

## Установка или активация

*   В ОС можно использовать только два вида файлов - Файл и папка
    
*   Мы не можем перейти непосредственно в устройство. Таким образом, созданное устройство должно быть преобразовано в папку или ссылку с папкой или монтировать с папкой, чтобы использовать это.
    
*   **mkdir / data**
    
*   **mount / dev / sdb1 / data** (Эти данные похожи на устройство для установки и размонтирования привода пера)
    
*   **cd / data**
    
*   **cat> adarsh.txt**
    
*   **umount / dev / sdb1**
    
*   **cd / data /**
    
*   **Ls**
    
*   Гора снова
    
*   Чтобы узнать, какой раздел установлен в какой папке.
    
*   **df -h**
    
*   Показывает таблицу inode.
    
*   **ls -l**
    
*   Показывает номер inode.
    
*   ls -il