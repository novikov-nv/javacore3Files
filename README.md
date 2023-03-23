# Задача 1: Установка

## Описание
В данной задаче Вы потренируетесь работать с файлами и каталогами в файловой системе, используя язык Java и класс `File`, и смоделируете процесс установки игры на жесткий диск комьютера.

Предварительно вручную создайте папку `Games` в удобном для Вас месте. Имейте ввиду, что у папки `Games` должны быть права на запись и чтение. Например, в операционной системе macOS можно создать папку по следующему пути `/Users/admin/Games`. В ОС Windows можете создать папку на одном из доступных жестких дисков, например `D://Games`.

Установку программы необходимо производить из Java кода с использованием класса `File`. Процесс будет состоять из следующих этапов:
1. В папке `Games` создайте несколько директорий: `src`, `res`, `savegames`, `temp`. 
2. В каталоге `src` создайте две директории: `main`, `test`. 
3. В подкаталоге `main` создайте два файла: `Main.java`, `Utils.java`. 
4. В каталог `res` создайте три директории: `drawables`, `vectors`, `icons`.
5. В директории `temp` создайте файл `temp.txt`.

Файл `temp.txt` будет использован для записиси в него информации об успешноном или неуспешном создании файлов и директорий.  

## Реализация
Для работы с файлом или директорией необходимо создать для них свой экземпляр класса `File`, передав в конструктор адрес к файлу или директории. Создать директорию можно с помощью метода `mkdir()`, который возвращает `true` или `false` в зависимости от того, была ли директория создана успешно.

Для создания файла можно использовать метод `createNewFile()`, который так же возвращает `boolean`. Обратите внимание, что этот метод пробрасывает `IOException`. Необходимо обернуть его в блок try-catch.

Для сохранения лога используйте экземпляр класса `StringBuilder`. Добавляйте в него всю информацию о создании файлов и каталогов. Далее возьмите из него текст и запишите его в файл `temp.txt` с помощью класса `FileWriter`.

В результате выполнения программы, на жестком диске комьютера в папке `Games` должны появиться вышеуказанные файлы и директории. Файл `temp.txt` должен содержать информацию о том, что все файлы были созданы успешно.


# Задача 2: Сохранение

## Описание
В данной задаче Вы потренируетесь сериализовывать Java класс, используя интерфейс `Serializable`, записывать сериализованные файлы на жесткий диск, используя класс `FileOutputStream`, и упаковывать их в архив с помощью `ZipOutputStream`.

Для дальнейшей работы потребуется создать класс `GameProgress`, хранящий информацию об игровом процессе. Предлагаем следующую реализацию:
```java
public class GameProgress implements Serializable {
    private static final long serialVersionUID = 1L;

    private int health;
    private int weapons;
    private int lvl;
    private double distance;

    public GameProgress(int health, int weapons, int lvl, double distance) {
        this.health = health;
        this.weapons = weapons;
        this.lvl = lvl;
        this.distance = distance;
    }

    @Override
    public String toString() {
        return "GameProgress{" +
                "health=" + health +
                ", weapons=" + weapons +
                ", lvl=" + lvl +
                ", distance=" + distance +
                '}';
    }
}
```

Таким образом, для выполнения задания потребуется проделать следующие шаги:
1. Создать три экземпляра класса `GameProgress`.
2. Сохранить сериализованные объекты `GameProgress` в папку `savegames` из предыдущей задачи.
3. Созданные файлы сохранений из папки `savegames` запаковать в архив `zip`.
4. Удалить файлы сохранений, лежащие вне архива.

## Реализация
Создайте три экземпляра класса `GameProgress`.

Реализуйте метод `saveGame()`, принимающий в качестве аргументов полный путь к файлу типа `String` (например, "/Users/admin/Games/GunRunner/savegames/save3.dat") и объект класса `GameProgress`. Для записи Вам потребуются такие классы как `FileOutputStream` и `ObjectOutputStream`. У последнего есть метод `writeObject()`, подходящий для записи сериализованного объекта . Во избежание утечек памяти, не забудьте либо использовать `try` с ресурсами, либо вручную закрыть файловые стримы (это касается всех случаев работы с файловыми потоками).

Таким образом, вызов метода `saveGame()` должен приводить к созданию файлов сохранений в папке `savegames`.

Далее реализуйте метод `zipFiles()`, принимающий в качестве аргументов `String` полный путь к файлу архива (например, "/Users/admin/Games/GunRunner/savegames/zip.zip") и список запаковываемых объектов в виде списка строчек `String` полного пути к файлу (например, "/Users/admin/Games/GunRunner/savegames/save3.dat"). В методе Вам потребуется реализовать блок `try-catch` с объектами `ZipOutputStream` и `FileOutputStream`. Внутри него пробегитесь по списку файлов и для каждого организуйте запись в блоке `try-catch` через `FileInputStream`. Для этого создайте экземпляр `ZipEntry` и уведомьте `ZipOutputStream` о новом элементе архива с помощью метода `putNextEntry()`. Далее необходимо считать упаковываемый файл с помощью метода `read()` и записать его с помощью метода `write()`. После чего уведомьте `ZipOutputStream` о записи файла в архив с помощью метода `closeEntry()`.

Далее, используя методы класса `File`, удалите файлы сохранений, не лежащие в архиве.
