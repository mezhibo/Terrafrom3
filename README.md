**Задание 1**

1.Изучите проект.

2.Заполните файл personal.auto.tfvars.

3.Инициализируйте проект, выполните код. Он выполнится, даже если доступа к preview нет.

Примечание. Если у вас не активирован preview-доступ к функционалу «Группы безопасности» в Yandex Cloud, запросите доступ у поддержки облачного провайдера. Обычно его выдают в течение 24-х часов.

Приложите скриншот входящих правил «Группы безопасности» в ЛК Yandex Cloud или скриншот отказа в предоставлении доступа к preview-версии.




**Решение 1**

Заполнил все переменные, сделал инициализацию проекта

![alt text](https://github.com/mezhibo/Terrafrom3/blob/faa394a043154747c1ac5e9acef867b89b0dbbf3/IMG/1.jpg)

Применим teraform apply

![alt text](https://github.com/mezhibo/Terrafrom3/blob/faa394a043154747c1ac5e9acef867b89b0dbbf3/IMG/2.jpg)

Видим, что у нас создались группы безопасности

И види внутри группы правила для подключений 

![alt text](https://github.com/mezhibo/Terrafrom3/blob/8f56f1fcfb974fb51e3fabcd3e8704908a064e9f/IMG/3.jpg)


**Задание 2**

1. Создайте файл count-vm.tf. Опишите в нём создание двух одинаковых ВМ web-1 и web-2 (не web-0 и web-1) с минимальными параметрами, используя мета-аргумент count loop. Назначьте ВМ созданную в первом задании группу безопасности.(как это сделать узнайте в документации провайдера yandex/compute_instance )

2. Создайте файл for_each-vm.tf. Опишите в нём создание двух ВМ для баз данных с именами "main" и "replica" разных по cpu/ram/disk_volume , используя мета-аргумент for_each loop. Используйте для обеих ВМ одну общую переменную типа:
```
variable "each_vm" {
  type = list(object({  vm_name=string, cpu=number, ram=number, disk_volume=number }))
}
```

При желании внесите в переменную все возможные параметры. 4. ВМ из пункта 2.1 должны создаваться после создания ВМ из пункта 2.2. 5. Используйте функцию file в local-переменной для считывания ключа ~/.ssh/id_rsa.pub и его последующего использования в блоке metadata, взятому из ДЗ 2. 6. Инициализируйте проект, выполните код.



**Решение 2**


Создадим файл locals.tf с описанием переменной metadata и укажем там путь к нашему открытому ключу, к которому мы будем цепляться после создания ВМ

![alt text](https://github.com/mezhibo/Terrafrom3/blob/fc9a6637a63ce787a723cd48aee3820b1635b406/IMG/4.jpg)


Далее в variables.tf добавим составную map-переменную для создания 2 типов машин с разными ресурсами


![alt text](https://github.com/mezhibo/Terrafrom3/blob/fc9a6637a63ce787a723cd48aee3820b1635b406/IMG/5.jpg)


Создадим файл count_vm.tf где опишем процесс создания 2 идентичных машин с помощью счетчика count

![alt text](https://github.com/mezhibo/Terrafrom3/blob/fc9a6637a63ce787a723cd48aee3820b1635b406/IMG/6.jpg)


В файле variables.tf создадим перменную для описания ресурсов машин баз данных.

![alt text](https://github.com/mezhibo/Terrafrom3/blob/a969a9a00a4be681f86cc6931bf218235d394f71/IMG/8.jpg)

И теперь создадим фаил for_each-vm.tf где опишем создание самих машин для БД

![alt text](https://github.com/mezhibo/Terrafrom3/blob/a969a9a00a4be681f86cc6931bf218235d394f71/IMG/9.jpg)


Применим terraform apply

Перейдем в веб-интерфейс, и видим что у нас созадлось 2 идентичных веб-машины, и 2 машины с разными ресурсами под БД

![alt text](https://github.com/mezhibo/Terrafrom3/blob/f6d5ad9479b8b87f89368cd14896e22d2f97373f/IMG/10.jpg)



**Задание 3**

1.Создайте 3 одинаковых виртуальных диска размером 1 Гб с помощью ресурса yandex_compute_disk и мета-аргумента count в файле disk_vm.tf .

2.Создайте в том же файле одиночную(использовать count или for_each запрещено из-за задания №4) ВМ c именем "storage" . Используйте блок dynamic secondary_disk{..} и мета-аргумент for_each для подключения созданных вами дополнительных дисков.



**Решение 3**


Создадим файлик disk_vm.tf где опишем создание ВМ и 3 дополнительных дисков через count

![alt text](https://github.com/mezhibo/Terrafrom3/blob/c964dd80690d67391cf350522eab8ecde86151df/IMG/11.jpg)


Перейдем в веб-интерфейс и првоерим что создалось там

![alt text](https://github.com/mezhibo/Terrafrom3/blob/c964dd80690d67391cf350522eab8ecde86151df/IMG/12.jpg)

Все отлично

