## ANSİBLE ROLE POSTGRESQL

PostgreSQL sunucusunu Centos sunucusuna kurulumu ve konfigürasyonu yapılır.
Ansible ile postgresql kurulumu ve konfigürasyonu yapmak için 2 ayrı Vm kuruldu.

| Controller   |   Postgre    |
|------------  |--------------|
| Ansible Host |  Postgresql  |


### REQUİREMENTS
``` 
 - hosts: postgre
  roles:
    - role: geerlingguy.postgresql
      become: yes
 ``` 
 
### ROLE VARİABLES

Mevcut değişkenler, defaults değerlerle birlikte aşağıda listelenmiştir. (defaults/main.yml)

```
postgresql_enablerepo: ""
```
(CENTOS) PostgreSQL kurulumu için kullanmak üzere bir repo buraya ileterek ayarlayabilirsiniz.

```
postgresql_restarted_state: "restarted"
```

Configuration değişiklikleri yapıldığında service durumu ayarlanmalıdır. Önerilen değerler ``` restarted ``` veya ``` reloated ```.

```
postgresql_python_library: python-psycopg2
```

Ansible tarafından PostgreSQL ile iletişim kurmak için kullanılan kütüphane.

```
postgresql_user: postgres
postgresql_group: postgres
```
PostgreSQL'in altında çalışacağı kullanıcı ve grup.

```
postgresql_unix_socket_directories:
  - /var/run/postgresql
```

PostgreSQL soketinin oluşturulacağı dizinler.

```
postgresql_service_state: started
postgresql_service_enabled: true
```

Postgresql hizmetinin durumunu ve önyükleme zamanında başlayıp başlamayacağını kontrol edilir yukarıda verilen komut ile.

```
postgresql_global_config_options:
  - option: unix_socket_directories
    value: '{{ postgresql_unix_socket_directories | join(",") }}'
```

```postgresql.conf``` içinde ayarlanacak global yapılandırma seçenekleri.



```
postgresql_hba_entries:
  - { type: local, database: all, user: postgres, auth_method: peer }
  - { type: local, database: all, user: all, auth_method: peer }
  - { type: host, database: all, user: all, address: '127.0.0.1/32', auth_method: md5 }
  - { type: host, database: all, user: all, address: '::1/128', auth_method: md5 }

```

```pg_hba.conf``` dosyasında ayarlanacak host based authentication girişleri yapılandırıldı. Şunları içerir:

* type 
* database 
* user 
* address
* ip_address
* ip_mask
* auth_method 
* auth_options 


## İNSTALL POSTGRE
Controller üzerinden client olan postgre vm e postgresql kurulumu ``` installation_postgre.yml``` ile yapıldı.
```
- hosts: postgre
  become: yes
  vars_files:
    - defaults/main.yml
  roles:
    - geerlingguy.postgresql
```

### CREATE DATABASE 
Conroller üzerinden client olan postgre vm de Postgre de database oluşturma ``` db-server-playbook.yml``` ile yapıldı. Database oluşturulan host client olarak seçildi. Oluşturulan database de user ``` db_user: postgre ``` olarak seçildi. Database ismi de ``` db_name: trafikveri ``` olarak seçildi. Database oluşturulmadan önce PostgreSql hizmeti çalıştırıldı. Databaser user oluşturuldu. Oluştrulan kullanıcıya database erişim izinleri verildi.  ``` db-server-playbook.yml```   ``` db_name: trafikveri ```  database oluşturuldu.
 
 ![01](https://user-images.githubusercontent.com/28953086/120711268-d968eb80-c4c7-11eb-8afb-168255079ec5.png)
 
 ### CREATE TABLE
 Table oluşturma işlemi NACİVAT FOR POSTGRE uygulaması ile client olan postgre vm e bağlanıp oluşturuldu.
 
![02](https://user-images.githubusercontent.com/28953086/120711986-bc80e800-c4c8-11eb-95c3-56e26fccc091.png)


![03](https://user-images.githubusercontent.com/28953086/120712240-12559000-c4c9-11eb-88ae-ea3961302459.png)


### ADD DATA TO TABLE





## EXAMPLE PLAYBOOK

```
- hosts: postgre
  become: yes
  vars_files:
    - defaults/main.yml
  roles:
    - geerlingguy.postgresql
    
```

 
 


  


