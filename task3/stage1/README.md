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


## EXAMPLE PLAYBOOK

```
- hosts: postgre
  become: yes
  vars_files:
    - defaults/main.yml
  roles:
    - geerlingguy.postgresql
    
```


 
 
 


  


