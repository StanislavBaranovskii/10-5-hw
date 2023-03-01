# Домашнее задание к занятию "`10.5. Балансировка нагрузки. HAProxy/Nginx`" - `Барановский Станислав`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

## Задание 1

Что такое балансировка нагрузки и зачем она нужна?

*Приведите ответ в свободной форме.*
```
```
**Балансировка нагрузки**

это распределение нагрузке между пулом приложений.
Используется для оптимизации нагрузки ресурсов приложений, увеличение пропускной способности, уменьшение времени отклика и предотвращение перегрузки одного ресурса. Кроме того, она может повысить доступность приложений за счет совместного использования рабочей нагрузки в пределах избыточных вычислительных ресурсов.

---

## Задание 2

Чем отличаются алгоритмы балансировки Round Robin и Weighted Round Robin? В каких случаях каждый из них лучше применять?

*Приведите ответ в свободной форме.*
```
```
**Round Robin**

Запросы распределяются по пулу сервером последовательно.
Может быть применим в случае использования в пуле серверов одинаковой мощности.

**Weighted Round Robin**

Тот же Round Robin плюс вес сервера.
Вес сервера указывает балансировщику, сколько трафика отправлять на тот или иной сервер из пула.
Целесообразно применить в случае использования в пуле серверов не одинаковой мощности.

---

## Задание 3

Установите и запустите Haproxy.

*Приведите скриншот systemctl status haproxy, где будет видно, что Haproxy запущен.*

```
curl https://haproxy.debian.net/bernat.debian.org.gpg | sudo gpg --dearmor -o /usr/share/keyrings/haproxy.debian.net.gpg
echo deb "[signed-by=/usr/share/keyrings/haproxy.debian.net.gpg]" http://haproxy.debian.net buster-backports-2.4 main | sudo tee /etc/apt/sources.list.d/haproxy.list > /dev/null
sudo apt update
sudo apt install -y haproxy=2.4.\*
sudo systemctl status haproxy.service
```

![Скриншот systemctl status haproxy](https://github.com/StanislavBaranovskii/10-5-hw/blob/main/img/10-5-3.png "Скриншот systemctl status haproxy")

---

## Задание 4

Установите и запустите Nginx.

*Приведите скриншот systemctl status nginx, где будет видно, что Nginx запущен.*

```
sudo apt install -y nginx
sudo systemctl status nginx
sudo nano /etc/nginx/nginx.conf
sudo nginx -t
```

![Скриншот systemctl status nginx](https://github.com/StanislavBaranovskii/10-5-hw/blob/main/img/10-5-4.png "Скриншот systemctl status nginx")

---

## Задание 5

Настройте Nginx на виртуальной машине таким образом, чтобы при запросе:
`curl http://localhost:8088/ping`
он возвращал в ответе строчку:
"nginx is configured correctly".

*Приведите конфигурации настроенного Nginx сервиса и скриншот результата выполнения команды `curl http://localhost:8088/ping`.*

**nginx.conf**
В блочную директиву `http {}` добавляем следующий код:
```
server {
	listen 8088;
	location / {
		return 200 "\nnginx is configured correctly\n\n";
	}
}
```

![Скриншот выполнения команды curl http://localhost:8088/ping](https://github.com/StanislavBaranovskii/10-5-hw/blob/main/img/10.5.5.png "Скриншот выполнения команды curl http://localhost:8088/ping")

[**Файл nginx.conf**](https://github.com/StanislavBaranovskii/10-5-hw/blob/main/data/nginx.conf)

---

## Задание 6*

Настройте Haproxy таким образом, чтобы при ответе на запрос:
`curl http://localhost:8080/ping`
он проксировал его в Nginx на порту 8088, который был настроен в задании 5 и возвращал от него ответ:
"nginx is configured correctly".

*Приведите конфигурации настроенного Haproxy и скриншоты результата выполнения команды curl `http://localhost:8080/`.*
```

```
![Скриншот выполнения команды curl http://localhost:8088/](https://github.com/StanislavBaranovskii/10-5-hw/blob/main/img/10-5-6.png "Скриншот выполнения команды curl http://localhost:8080/")

---
