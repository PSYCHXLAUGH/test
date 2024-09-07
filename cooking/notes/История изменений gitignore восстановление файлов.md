#git #linux 

Жизненный цикл файла до пуша в репозиторий

![[Pasted image 20240903210143.png]]

Copy in Local Repository - означает, что у нас уже есть снепшот и мы в любой момент можем откатиться туда

Получить историю всех коммитов (выдаст все коммиты, которые были)

```
git log
```

Посмотреть последний коммит

```
git log -1
```

Посмотреть разницу, которая была в прошлом коммите

```
git log -1 -p
```

Откатить модифицированный файл, который в статусе untracked

```
➜  example git:(master) git status
Текущая ветка: master
нечего коммитить, нет изменений в рабочем каталоге
➜  example git:(master) ls  
file2.txt  file3.txt  file.txt  gitfile.txt
➜  example git:(master) echo "gagagaga" >> gitfile.txt 
➜  example git:(master) ✗ git status
Текущая ветка: master
Изменения, которые не в индексе для коммита:
  (используйте «git add <файл>...», чтобы добавить файл в индекс)
  (используйте «git restore <файл>...», чтобы отменить изменения в рабочем каталоге)
	изменено:      gitfile.txt

индекс пуст (используйте «git add» и/или «git commit -a»)
➜  example git:(master) ✗ git checkout -- gitfile.txt 
➜  example git:(master) git status
Текущая ветка: master
нечего коммитить, нет изменений в рабочем каталоге
➜  example git:(master) 

```

Например мы добавили файл в этап staged и теперь нам нужно проверить что запишется в git log когда сделаем снепшот

```
git diff --staged
```

```
➜  example git:(master) echo "fasfasf" >> gitfile.txt 
➜  example git:(master) ✗ git add . 
➜  example git:(master) ✗ git --staged
unknown option: --staged
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]
➜  example git:(master) ✗ git diff
➜  example git:(master) ✗ git diff --staged
➜  example git:(master) ✗ 
➜  example git:(master) ✗ git diff --staged > test.txt
➜  example git:(master) ✗ cat test.txt 
diff --git a/gitfile.txt b/gitfile.txt
index e1cba14..7e27fd5 100644
--- a/gitfile.txt
+++ b/gitfile.txt
@@ -38,3 +38,4 @@ index 0000000..9daeafb
 +++ b/file.txt
 @@ -0,0 +1 @@
 +test
+fasfasf
➜  example git:(master) ✗ 

```


Игнорирование файлов

```
➜  example git:(master) ✗ mkdir logs; echo "test" > logs/file.txt
➜  example git:(master) ✗ ls logs 
file.txt
➜  example git:(master) ✗ git status
Текущая ветка: master
Изменения, которые будут включены в коммит:
  (используйте «git restore --staged <файл>...», чтобы убрать из индекса)
	изменено:      gitfile.txt

Неотслеживаемые файлы:
  (используйте «git add <файл>...», чтобы добавить в то, что будет включено в коммит)
	logs/
	test.txt

➜  example git:(master) ✗ touch .gitignore
➜  example git:(master) ✗ vim .gitignore 
➜  example git:(master) ✗ cat .gitignore 
*.log
logs/
➜  example git:(master) ✗ git status
Текущая ветка: master
Изменения, которые будут включены в коммит:
  (используйте «git restore --staged <файл>...», чтобы убрать из индекса)
	изменено:      gitfile.txt

Неотслеживаемые файлы:
  (используйте «git add <файл>...», чтобы добавить в то, что будет включено в коммит)
	.gitignore
	test.txt

➜  example git:(master) ✗ 

```