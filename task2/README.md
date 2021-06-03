## LOGİCAL VOLUME OLUŞTURMA VE MİNİO KURMA

| Controller   |     VM1      |     VM2      |
|------------  |--------------|--------------|
| Ansible Host | Minio-server | Minio-server |

3 Vm oluşturuldu. Minio-server seçilen vm lere 5 gb lik disk eklendi. 


İlk önce hosts oluşturulup client olan vm ler ve controller ipleri tanımlandı. ```hosts ```

```
[slave]
c1 ansible_host=192.168.1.11
c2 ansible_host=192.168.1.12

[master]
control ansible_host=192.168.1.10
 ```
 
 Firewalld, SELinux disable edildi. Clientlere reboot atıldı.  ``` security.yml ```
 
 Client lere telnet, net-tools ve lvm2 yüklendi.  ```  prepare.yml ```
 
  ```
  ---
- hosts: slave
  become: yes

  tasks:
 
    - name: Install some basic packages
      yum:  
        name: ['telnet', 'net-tools', 'lvm2']
        
   ```
   
   ##LOGİCAL VOLUME
   
 
 Client lere partition oluşturuldu. 8 tane 540mb lik logical volume oluşturuldu. Logical volume ler de xfs file sistem oluşturuldu. Daha sonra oluşturulan file sistemler mount edildi.  ``` logical_volume_managament.yml  ```
 
 
 ![01](https://user-images.githubusercontent.com/28953086/120717527-6021c680-c4d0-11eb-81ed-f05cf65ace06.png)
 
 ![02](https://user-images.githubusercontent.com/28953086/120717560-6dd74c00-c4d0-11eb-9a96-7bc274bd8ab8.png)
 
 ##MİNİO İNSTALLATİON
 
  
 ``` ansible-galaxy install easkay.minio   ```   komutu ile easkay.minio reposundan roleler çekildi.  ```  minio_installation.yml  ```  ile clientlere minio-server kurulumu yapıldı. 
 
 C1
  ![03](https://user-images.githubusercontent.com/28953086/120717560-6dd74c00-c4d0-11eb-9a96-7bc274bd8ab8.png)
  
 C2
  ![04](https://user-images.githubusercontent.com/28953086/120718593-31a4eb00-c4d2-11eb-92cc-912b8d04c3cd.png)
 
 
 
