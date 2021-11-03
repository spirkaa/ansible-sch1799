# Роль Ansible: Domain Join

Роль Ansible для присоединения ОС Linux к домену Active Directory.

## Особенности

Чтобы в приложениях, использующих `krb5.keytab`,
аутентификация Kerberos работала сразу после ввода хоста в домен,
нужно заранее создать объект компьютера и задать servicePrincipalName.
Для этого нужно выполнить команды в Windows от имени пользователя с соответствующими правами в домене (пример):

* `dsadd computer "CN=S-1799-PROXY,OU=Servers,OU=Computers,OU=SCH-1799,OU=Schools,DC=cao,DC=obr,DC=mos,DC=ru"`
* `setspn -s HTTP/sso.licey1799.ru CAO\S-1799-PROXY`

Если ввести хост в домен без создания объекта компьютера,
и потом добавить servicePrincipalName, то в файле `krb5.keytab` эта запись отобразится не сразу.
Для ускорения процесса нужно:

* вывести из домена командой `realm leave cao.obr.mos.ru`
* повторно запустить эту роль
* перезагрузить хост (чтобы не было ошибки `GSS-API Checksum failed`)
* проверить наличие SPN командой `klist -ket /etc/krb5.keytab`
