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

Особенности/ошибки, исправленные в методичке по ДЗ 1.
В файле centos.json увеличиваем тайм-аут подключения по ssh до 60 мин (для старого ноута):

"ssh_timeout": "60m"

В файл ks.cfg добавляем перед перезагрузкой строки, чтобы установка не останавливалась на ожидание выбора компонентов, выбирается минимальное окружение:

%packages --ignoremissing
@^minimal-environment
%end
reboot

В первом скрипте stage-1-kernel-update.sh команду перезагрузки меняем на выключение виртуалки, т.к. иначе процесс не продолжается (до второго скрипта), почему так - вопрос остался открытым:

shutdown -h +3
