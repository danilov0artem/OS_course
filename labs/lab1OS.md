# Лабораторная работа №1 по предмету "Операционные системы". Преподаватель Данилов А. С.

## Задания

Лабораторная работа №1 состоит из трех частей:
- первая и вторая части являются обязательными для выполнения;
- третья часть по желанию на оценку "отлично".

**Важно: итоговая оценка зависит от вашей "защиты" работы, то есть от ответов на вопросы преподавателя.**

### Требования

Обязательно проверяйте успешность всех вызовов функций операционной системы и не оставляйте ошибки незамеченными.

### Задание 1 — sys-info-win

*Примеры приведены для С++, но вы можете использовать любой другой язык, где можно использовать Win32 API.*

Научиться работать с Windows API для получения системной информации, реализовать структурированный и надёжный код, который:

- Обрабатывает все возможные ошибки корректно,
- Даёт студенту понимание, как устроена система "под капотом".

Напишите программу sys-info-win для ОС Windows, которая бы выводила в консоль информацию о компьютере на котором она запущена:

- Версия операционной системы.
  - Используйте IsWindows10OrGreater() и аналоги из VersionHelpers.h
- Размер виртуальной и физической памяти, а также использование памяти в процентах.
- Количество ядер процессора
- Имя компьютера и имя пользователя
- Архитектура процессора (x86, x64, ARM)
- Размер файла подкачки (функция `GetPerformanceInfo`)
- Список логических дисков + их объёмы

#### Пример выводимой информации

```txt
OS: Windows 10 or Greater
Computer Name: DESKTOP-12345
User: Ivan Ivanov
Architecture: x64 (AMD64)
RAM: 6417MB / 7796MB
Virtual Memory: 16000MB
Memory Load: 47%
Pagefile: 20480MB / 32000MB

Processors: 16
Drives:
  - C:\  (NTFS): 114 GB free / 237 GB total
  - D:\  (NTFS): 80 GB free / 100 GB total
```

#### Подсказки

Начиная с Windows 8.1 Microsoft не рекомендует приложениям привязываться к версии операционной системы,
поэтому функции вроде [`GetVersionEx`](https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-getversionexw)
могут возвращать недостоверную информацию о версии операционной системы.

Используйте [вспомогательные функции Windows](https://learn.microsoft.com/en-us/windows/win32/sysinfo/version-helper-apis),
чтобы узнать информации о версии ОС.
Для получения актуальной информации используйте функцию [RtlGetVersion](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-rtlgetversion?redirectedfrom=MSDN).

Узнать количество процессоров можно функцией [`GetSystemInfo`](https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-getsysteminfo)
и [`GetNativeSystemInfo`](https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-getnativesysteminfo).

Узнать информацию о памяти компьютера можно функций [`GlobalMemoryStatusEx`](https://learn.microsoft.com/en-us/windows/win32/api/sysinfoapi/nf-sysinfoapi-globalmemorystatusex).

Размер файла подкачки можно получить функцией [GetPerformanceInfo](https://learn.microsoft.com/en-us/windows/win32/api/psapi/nf-psapi-getperformanceinfo).

Информацию о дисках можно получить функциями `GetLogicalDriveStrings` и `GetDiskFreeSpaceEx`.

### Задание 2 — sys-info-linux

Напишите программу sys-info-linux для ОС Linux, которая бы выводила в консоль информацию о компьютере, на котором запущена:

- Версия ядра и дистрибутива
  - Используйте `uname()` для ядра
  - Используйте `lsb_release()` или чтение `/etc/os-release`
- Количество свободной и имеющейся оперативной памяти (в мегабайтах)
  - Используйте `sysinfo` или парсинг `/proc/meminfo`.
- Количество логических процессоров (`get_nprocs()`)
- Архитектура процессора
  - Используйте `uname().machine`
- Загрузка процессора (из sysinfo.loads) или `/proc/loadavg`.
- Список подключенных логических дисков
  - прочитать `/proc/mounts` или вызывать `getmntent`.
  - Получить статистику через `statvfs()`
- Информация о текущем пользователе и hostname
  - `getlogin()`, `gethostname()` или `getpwuid(getuid())`.
- Объём доступной виртуальной памяти
  - Через `/proc/meminfo`, поле `VmallocTotal` (если доступно)

#### Пример выводимой информации

```txt
OS: Ubuntu 22.04.1 LTS
Kernel: Linux 5.15.0-86-generic
Architecture: x86_64
Hostname: dev-machine-01
User: student
RAM: 5983MB free / 7796MB total
Swap: 2047MB total / 512MB free
Virtual memory: 134217 MB
Processors: 16
Load average: 0.12, 0.45, 0.91
Drives:
  /          ext4     40GB free / 100GB total
  /mnt/c     fuse     12GB free / 237GB total
```

### Задание 3 — sys-info-crossplatform (не обязательно - на оценку "отлично")

Разработайте класс `SysInfo`, предоставляющий методы для получения информации о системе из заданий:

- Название и версия ОС
- Количество логических процессоров
- Количество свободной и установленной оперативной памяти

Класс должен быть возможно скомпилировать как под Windows, так и под Linux. Публичный интерфейс класса представлен ниже (пример приведен на C++).

```c++
class SysInfo
{
public:
    std::string GetOSName() const;
    std::string GetOSVersion() const;
    uint64_t GetFreeMemory() const;
    uint64_t GetTotalMemory() const;
    unsigned GetProcessorCount() const;
};
```

Разработайте на основе этого класса консольное приложение **sys-info**, выводящее в консоль информацию о компьютере
и операционной системе.

Приложение должно собираться/запускаться как под Windows, так и под Linux.

## Ссылки

- [Список Windows API](https://learn.microsoft.com/en-us/windows/win32/apiindex/windows-api-list)
- [Linux man-pages project](https://www.kernel.org/doc/man-pages/) - поисковик по документации ядра Linux.
- [Intel® 64 and IA-32 Architectures Software Developer’s Manual](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)
  - Для доступа к сайту используйте VPN либо браузер Tor.
- [Linux Pocket Guide](https://linuxpocketguide.com/)