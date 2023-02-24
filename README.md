# Домашнее задание к занятию "Работа в терминале. Лекция 1"

## Инструкция к заданию

1. Установите средство виртуализации [Oracle VirtualBox](https://www.virtualbox.org/).
```
$ VBoxManage -version
7.0.6r155176
```

2. Установите средство автоматизации [Hashicorp Vagrant](https://hashicorp-releases.yandexcloud.net/vagrant/).
```
$ vagrant -v
Vagrant 2.3.4
```

3. В вашем основном окружении подготовьте удобный для дальнейшей работы терминал. Можно предложить:

	* ~~iTerm2 в Mac OS X~~
	* Терминал в MacOS
	* ~~Windows Terminal в Windows~~
	* выбрать цветовую схему, размер окна, шрифтов и т.д.
	* почитать о кастомизации PS1/применить при желании.
  
![Окно терминала в MacOS + настроенный zsh](https://github.com/Alexey-Tatakin/devops-netology/blob/base-admin-term-lec-1/screen1.png "zsh in MacOS")
	
### Задание

1. С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:

	* Создайте директорию, в которой будут храниться конфигурационные файлы Vagrant. В ней выполните `vagrant init`. Замените содержимое Vagrantfile по умолчанию следующим **(это мой вариант)**:

		```bash
		Vagrant.configure("2") do |config|
		  config.vm.box = "bento/ubuntu-20.04"
		  config.vm.provider "virtualbox"  do |vb|
	  	    vb.name = "Netology-Ubuntu"
	        vb.memory = "1024"
            vb.cpus = 1
		  end
		end
		```

	* Выполнение в этой директории `vagrant up` установит провайдер VirtualBox для Vagrant, скачает необходимый образ и запустит виртуальную машину.

	* `vagrant suspend` выключит виртуальную машину с сохранением ее состояния (т.е., при следующем `vagrant up` будут запущены все процессы внутри, которые работали на момент вызова suspend), `vagrant halt` выключит виртуальную машину штатным образом.

1. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?

1. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: [документация](https://www.vagrantup.com/docs/providers/virtualbox/configuration.html). Как добавить оперативной памяти или ресурсов процессора виртуальной машине?

1. Команда `vagrant ssh` из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.
~~~
$ vagrant ssh    
Welcome to Ubuntu 20.04.5 LTS (GNU/Linux 5.4.0-135-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri 24 Feb 2023 11:17:53 AM UTC

  System load:  0.01               Processes:             109
  Usage of /:   12.2% of 30.34GB   Users logged in:       0
  Memory usage: 25%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Fri Feb 24 10:49:51 2023 from 10.0.2.2
vagrant@vagrant:~$ 
~~~
1. Ознакомьтесь с разделами `man bash`, почитайте о настройках самого bash:
    * какой переменной можно задать длину журнала `history`, и на какой строчке manual это описывается? <br/>
     [HISTSIZE] на 823 строке, по умолчанию 500 .
    * что делает директива `ignoreboth` в bash?

		*[HISTCONTROL]<br/>
		Если имеет значение ignorespace, строки, начинающиеся символом
		пробела, не попадают в список выполненных команд. Если имеет значение
		ignoredups, строки, совпадающие с последней выполненной командой, в список
		выполненных команд не попадают. Значение `ignoreboth` сочетает действие
		обеих представленных опций. Если переменной нет или она имеет какое-то другое значение, кроме перечисленных выше, все строки, прочитанные синтаксическим анализатором, сохраняются в списке истории, с учетом значения переменной HISTIGNORE.* 

2. В каких сценариях использования применимы скобки `{}` и на какой строчке `man bash` это описано?<br/>
	{ список; }<br/>
	Список просто выполняется в среде текущего командного интерпретатора.
	Список должен завершаться переводом строки или точкой с запятой.
	Эту команду называют командой группировки. Статусом возврата
	является статус выхода списка.<br/>
	*249 строка*

3. С учётом ответа на предыдущий вопрос, как создать однократным вызовом `touch` 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?

	**touch file{1..100000}<br/>
	300000 нельзя сделать из за ошибки "Argument list too long"<br/>
	Проверить текущее состояние этого параметра можно командой<br/>
	~# getconf ARG_MAX<br/>
	2097152**

4. В man bash поищите по `/\[\[`. Что делает конструкция `[[ -d /tmp ]]`<br/>
	Возвращает статус 0 или 1 в зависимости от значения указанного
	условного выражения. `-d` Истинно, если файл существует и является каталогом. В данном примере возвращает 0 т.к. дериктория существует. Но на самом деле по умолчанию на экран результат не выводится, пришлось прибегнуть к echo $?

5. Сделайте так, чтобы в выводе команды `type -a bash` первым стояла запись с нестандартным путем, например bash is ... 
Используйте знания о просмотре существующих и создании новых переменных окружения, обратите внимание на переменную окружения PATH 

	```bash
	bash is /tmp/new_path_directory/bash
	bash is /usr/local/bin/bash
	bash is /bin/bash
	```

	(прочие строки могут отличаться содержимым и порядком)
    В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.

	~~~
	~# PATH=/opt:$PATH
	~# export PATH
	~# ln -s /bin/bash /opt/bash
	~# type -a bash
	bash is /opt/bash
	bash is /usr/bin/bash
	bash is /bin/bash
	~~~

1. Чем отличается планирование команд с помощью `batch` и `at`?
   **Команда `at` используется для назначения выполнения разового задания в определённое время. Команда `batch` используется для назначения команды или последовательности команд которые будут выполенения при средней загрузке ниже 0.8**

2. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.<br/>
   *vagrant destroy*

*В качестве решения дайте ответы на вопросы свободной форме* 

---

### Правила приема домашнего задания

- В личном кабинете отправлена ссылка на .md файл в вашем репозитории.

### Критерии оценки

Зачет - выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку - задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки. 

[https://github.com/Alexey-Tatakin/devops-netology/blob/base-admin-term-lec-1/screen1.png]: https://github.com/Alexey-Tatakin/devops-netology/blob/base-admin-term-lec-1/screen1.png "zsh in MacOS"