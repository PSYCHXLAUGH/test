#git #linux 


Эти данные будут включаться в каждый коммит. По сути это информация по пользователю

```
git config --global user.name "username"
git config --global user.email "username@mail.ru"
```

Посмотреть конфигурацию можно

```
git config -l
```

Лежит этот файл в домашней директории .gitconfig

```
➜  ~ pwd
/home/tmedvedev
➜  ~ ls -laf | grep git
.gitconfig
```