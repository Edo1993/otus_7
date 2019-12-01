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
Результирующий spec файл получился таким.
Путь до openssl указываем ДО каталога (внимательнее с openssl-1.1.1d - зависит от скачанной вами версии):
```--with-openssl=/root/openssl-1.1.1d```
