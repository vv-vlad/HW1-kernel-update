# HW1-kernel-update
Обновить ядро в базовой системе  
Цель:  
получить навыки работы с Git, Vagrant, Packer;
публиковать готовые образы в Vagrant Cloud.

1) Обновить ядро ОС из репозитория ELRepo
2) Создать Vagrant box c помощью Packer
3) Загрузить Vagrant box в Vagrant Cloud

методичка:
https://docs.google.com/document/d/12sC884LuLGST3-tZYQBDPvn6AH8AGJCK/edit?usp=share_link&ouid=107126378526912727172&rtpof=true&sd=true

## Особенности/ошибки, исправленные в методичке по ДЗ №1 по результатам работы группы.  
### В файле centos.json   
увеличиваем тайм-аут подключения по ssh до 60 мин (это моё - для старого ноута):  
"ssh_timeout": "60m"  
Меняем url загрузочного образа CentOs:   http://mirror.linux-ia64.org/centos/8-stream/isos/x86_64/CentOS-Stream-8-20230308.3-x86_64-boot.iso  
Команду выключения определяем как:  
"shutdown_command": "echo 'vagrant' | sudo -S /sbin/halt -h -p", (передаём имя пользователя в команду, чтобы не было ожидания ввода)  
Команду на исполнение определяем как:  
"execute_command": "{{.Vars}} echo 'vagrant' | sudo -S -E bash '{{.Path}}'",  (также имя пользователя в команду, чтобы не было ожидания ввода)  

### В файл ks.cfg  
firewall --disabled (два минуса, вместо тире и минус - опечатка)  
добавляем перед перезагрузкой строки, чтобы установка не останавливалась на ожидание выбора компонентов, выбирается минимальное окружение:  
%packages --ignoremissing  
@^minimal-environment  
%end  
reboot  

### В первом скрипте stage-1-kernel-update.sh   
команду перезагрузки меняем на выключение виртуалки, т.к., иначе процесс не продолжается (до второго скрипта), почему так - вопрос остался открытым:  
shutdown -h +3  
## В итоге по факту получаем систему с 6-ой версией ядра  

## Рекомендуемые источники  
Репозиторий manual_kernel_update - https://github.com/dmitry-lyutenko/manual_kernel_update/blob/master/manual/manual.md  
Статья о GitHub - https://ru.wikipedia.org/wiki/GitHub   
Elrepo HomePage - http://elrepo.org/tiki/HomePage   
Packer Docs - https://www.packer.io/docs   
Параметры автоконфигурации от RedHat - https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/performing_an_advanced_rhel_installation/kickstart-commands-and-options-reference_installing-rhel-as-an-experienced-user  

