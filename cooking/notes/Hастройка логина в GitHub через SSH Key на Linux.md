#git #linux 


Клонирование пустого репозитория

```
git clone git@github.com:PSYCHXLAUGH/test.git
```

Создать ssh ключи

```
ssh-keygen
```

Копируем публичный ключ

```
➜  test git:(master) cat ../.ssh/id_rsa.pub 
```

Добавляем ключ в аккаунт 

User -> ssh and gpg keys -> new ssh key


Получаем информацию об удаленном репозитории

```
➜  test git:(master) git remote -v
origin	https://github.com/PSYCHXLAUGH/test.git (fetch)
origin	https://github.com/PSYCHXLAUGH/test.git (push)
```

Если тут нужно поменять на ssh то делаем следующее

```
➜  test git:(master) git remote set-url origin git@github.com:PSYCHXLAUGH/test.git

➜  test git:(master) git remote -v
origin	git@github.com:PSYCHXLAUGH/test.git (fetch)
origin	git@github.com:PSYCHXLAUGH/test.git (push)
➜  test git:(master) 

```

Готово

```
➜  test git:(master) git push origin
Перечисление объектов: 6, готово.
Подсчет объектов: 100% (6/6), готово.
При сжатии изменений используется до 4 потоков
Сжатие объектов: 100% (3/3), готово.
Запись объектов: 100% (6/6), 449 байтов | 112.00 КиБ/с, готово.
Всего 6 (изменения 0), повторно использовано 0 (изменения 0)
To github.com:PSYCHXLAUGH/test.git
 * [new branch]      master -> master
➜  test git:(master) 

```
