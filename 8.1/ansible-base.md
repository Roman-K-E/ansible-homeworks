# Домашнее задание к занятию 1 «Введение в Ansible»

## Подготовка к выполнению

1. Установлен Ansible на Ununtu 20.04.
2. Создан свой публичный репозиторий на GitHub (Roman-K-E/ansible-homeworks).
3. Скачано содержимое прилагаемой к д.з. директории Playbook.
   
## Решение основной части

1. Попробуйте запустить playbook на окружении из `test.yml`, зафиксируйте значение, которое имеет факт `some_fact` для указанного хоста при выполнении playbook.

![8-1--1.png](https://github.com/Roman-K-E/ansible-homeworks/blob/main/8.1/8-1--1.png)

2. Найдите файл с переменными (group_vars), в котором задаётся найденное в первом пункте значение, и поменяйте его на `all default fact`.
Поиск файла с переменными:
```
root@ub1:/home/r/homeworks/8-1# grep -rnw 'group_vars/' -e 'some_fact'
group_vars/el/examp.yml:2:  some_fact: "el"
group_vars/all/examp.yml:2:  some_fact: 12
group_vars/deb/examp.yml:2:  some_fact: "deb"
```
Новое значение переменной, после изменения в файле `group_vars/all/examp.yml`:  

![8-1--2.png](https://github.com/Roman-K-E/ansible-homeworks/blob/main/8.1/8-1--2.png)

3. Воспользуйтесь подготовленным (используется `docker`) или создайте собственное окружение для проведения дальнейших испытаний.  
Запущены Docker-контейнеры c Ubuntu и CentOS.  
```
root@ub1:/home/r/homeworks/8-1# docker-compose up -d
[+] Running 2/2
 ⠿ Container ubuntu   Started                                                                                      1.3s
 ⠿ Container centos7  Started
```
4. Проведите запуск playbook на окружении из `prod.yml`. Зафиксируйте полученные значения `some_fact` для каждого из `managed host`.  
![8-1--4.png](https://github.com/Roman-K-E/ansible-homeworks/blob/main/8.1/8-1--4.png)

5. Добавьте факты в `group_vars` каждой из групп хостов так, чтобы для `some_fact` получились значения: для `deb` — `deb default fact`, для `el` — `el default fact`.  

Внесены соотв. изменения в переменные `some_fact` файлов examp.yml директорий deb и el:  
```
root@ub1:/home/r/homeworks/8-1# cat group_vars/deb/examp.yml
---
  some_fact: "deb default fact"
root@ub1:/home/r/homeworks/8-1# cat group_vars/el/examp.yml
---
  some_fact: "el default fact"
root@ub1:/home/r/homeworks/8-1# ansible-playbook -i inventory/prod.yml site.yml
```

6.  Повторите запуск playbook на окружении `prod.yml`. Убедитесь, что выдаются корректные значения для всех хостов.  
![8-1--5.png](https://github.com/Roman-K-E/ansible-homeworks/blob/main/8.1/8-1--5.png)

7. При помощи `ansible-vault` зашифруйте факты в `group_vars/deb` и `group_vars/el` с паролем `netology`.
Шифрование и отображение получившегося:  
![8-1--7.png](https://github.com/Roman-K-E/ansible-homeworks/blob/main/8.1/8-1--7.png)

8. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь в работоспособности.
Запушен, запрос пароля срабатывает:  
![8-1--8.png](https://github.com/Roman-K-E/ansible-homeworks/blob/main/8.1/8-1--8.png)

9. Посмотрите при помощи `ansible-doc` список плагинов для подключения. Выберите подходящий для работы на `control node`.
![8-1--9.png](https://github.com/Roman-K-E/ansible-homeworks/blob/main/8.1/8-1--9.png)  
Полагаю, нужный плагин -- "local".  

10. В `prod.yml` добавьте новую группу хостов с именем  `local`, в ней разместите localhost с необходимым типом подключения.
Реализация в файле `prod.yml`:
```
root@ub1:/home/r/homeworks/8-1/inventory# cat prod.yml
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
root@ub1:/home/r/homeworks/8-1/inventory# cat test.yml
---
  inside:
    hosts:
      localhost:
        ansible_connection: local
root@ub1:/home/r/homeworks/8-1/inventory#
```
11. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь, что факты `some_fact` для каждого из хостов определены из верных `group_vars`.
![8-1--11.png](https://github.com/Roman-K-E/ansible-homeworks/blob/main/8.1/8-1--11.png)  

12. Заполните `README.md` ответами на вопросы. Сделайте `git push` в ветку `master`. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым `playbook` и заполненным `README.md`.  

.md заполнен, и в данный момент находится под Вашим просмотром. :)  

13. Предоставьте скриншоты результатов запуска команд.
    
Предоставлены выше ссылками в ответах на соотв. пункты.
