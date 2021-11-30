# Роль Ansible: RAID Monitor

Роль Ansible, которая настраивает мониторинг дисковой подсистемы на сервере Proxmox с помощью storcli, nrpe и [LibreNMS Services](https://docs.librenms.org/Extensions/Services/).

## StorCLI

Последняя версия storcli: <https://www.broadcom.com/support/download-search?dk=storcli>

Пакет DEB получен с помощью пересборки из RPM:

```bash
apt install alien
alien -k --scripts storcli.rpm
```

## NRPE

Скрипт для мониторинга и инструкция по установке <https://www.thomas-krenn.com/en/wiki/LSI_RAID_Monitoring_Plugin_setup>

## LibreNMS Services

После выполнения плейбука нужно в LibreNMS вручную добавить localhost для выполнения опроса NRPE, а также сам опрашиваемый сервис для каждого сервера Proxmox.

1. Перейти в LibreNMS → Devices → Add Device

    * Hostname or IP: localhost
    * SNMP: OFF

2. Перейти в LibreNMS → Services → Add Service

    * Name: Check RAID имя сервера
    * Device: localhost
    * Type: nrpe
    * Description: Check RAID имя сервера
    * IP Address: адрес сервера
    * Parameters: -c check_lsi_raid
