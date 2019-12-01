# otus_7
Размеcтить свой RPM в своем репозитории

1) Создать свой RPM пакет
Для данного задания установим пакеты (т.к vm "голая" добавляю vim + gcc):
```
yum install -y \redhat-lsb-core \wget \rpmdevtools \rpm-build \createrepo \yum-utils \vim \gcc
```
Для примера возьмем пакет NGINX и соберем его с поддержкой openssl. Загрузим SRPM пакет NGINX для дальнейшей работы над ним:
```
wget https://nginx.org/packages/centos/7/SRPMS/nginx-1.14.1-1.el7_4.ngx.src.rpm
```
При установке такого пакета в домашней директории создается древо каталогов для сборки:
```
rpm -i nginx-1.14.1-1.el7_4.ngx.src.rpm
```
Также нужно скачать и разархивировать последний исходники для openssl - он потребуется при сборке:
```
wget https://www.openssl.org/source/latest.tar.gz
tar -xvf latest.tar.gz
```
Заранее поставим все зависимости, чтобы в процессе сборки не было ошибок:
```
yum-builddep rpmbuild/SPECS/nginx.spec
```
Поправить сам spec файл, чтобы NGINX собирался с необходимыми опциями. 
Результирующий spec файл получился таким ![таким](https://github.com/Edo1993/otus_7/blob/master/nginx.spec).
Путь до openssl указываем ДО каталога (внимательнее с openssl-1.1.1d - зависит от скачанной вами версии):
```
--with-openssl=/root/openssl-1.1.1d
```
Приступаем к сборке RPM пакета:
```
rpmbuild -bb rpmbuild/SPECS/nginx.spec
```
Если всё ок - получаем длинную портянку текста, заканчивающуюся следующим:
![Image alt](https://github.com/Edo1993/otus_7/raw/master/11.png)
Убедимся что пакеты создались:
```
ll rpmbuild/RPMS/x86_64/
```
![Image alt](https://github.com/Edo1993/otus_7/raw/master/12.png)
Теперь можно установить наш пакет и убедиться, что nginx работает
```
yum localinstall -y /root/rpmbuild/RPMS/x86_64/nginx-1.14.1-1.el7_4.ngx.x86_64.rpm
systemctl start nginx
systemctl status nginx
```
![Image alt](https://github.com/Edo1993/otus_7/raw/master/13.png)
