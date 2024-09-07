#git #linux 


Создаем директорию где будет лежать локальный репозиторий
```
➜  ~ ls -laf | grep example
example
```

Заходим в папку и создаем базу данных git ```git init <path>```

```
➜  example git init
Инициализирован пустой репозиторий Git в /home/tmedvedev/example/.git/
➜  example git:(master) ls -laf 
.git  .  ..
```

Получить информацию по  индексации изменений

```
➜  example git:(master) git status
Текущая ветка: master

Еще нет коммитов

нечего коммитить (создайте/скопируйте файлы, затем запустите
«git add», чтобы отслеживать их)

```

Проверить неотслеживаемые файлы

```
➜  example git:(master) echo "test" > file.txt
➜  example git:(master) ✗ git status
Текущая ветка: master

Еще нет коммитов

Неотслеживаемые файлы:
  (используйте «git add <файл>...», чтобы добавить в то, что будет включено в коммит)
	file.txt

индекс пуст, но есть неотслеживаемые файлы
(используйте «git add», чтобы проиндексировать их)
```

Добавить неотслеживающие файлы и сохранить версию (сделать снимок текущего состояния)

```
➜  example git:(master) ✗ git add *
➜  example git:(master) ✗ git status
Текущая ветка: master

Еще нет коммитов

Изменения, которые будут включены в коммит:
  (используйте «git rm --cached <файл>...», чтобы убрать из индекса)
	новый файл:    file.txt

```

```
➜  example git:(master) ✗ git commit -m "new_message"
[master (корневой коммит) 503e1fb] new_message
 1 file changed, 1 insertion(+)
 create mode 100644 file.txt
➜  example git:(master) git status
Текущая ветка: master
нечего коммитить, нет изменений в рабочем каталоге
```

Модифицированные файлы

```
➜  example git:(master) ✗ echo "somechanges" >> file.txt 
➜  example git:(master) ✗ git status
Текущая ветка: master
Изменения, которые не в индексе для коммита:
  (используйте «git add <файл>...», чтобы добавить файл в индекс)
  (используйте «git restore <файл>...», чтобы отменить изменения в рабочем каталоге)
	изменено:      file.txt

Неотслеживаемые файлы:
  (используйте «git add <файл>...», чтобы добавить в то, что будет включено в коммит)
	file2.txt

индекс пуст (используйте «git add» и/или «git commit -a»)
➜  example git:(master) ✗ 

```

Файл file.txt уже существовал у него был снепшот, но он поменялся и отображается в "модифицированные" также был добавлен новый файл file2.txt
