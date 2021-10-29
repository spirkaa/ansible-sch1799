# Коллекция Ansible: Proxy

Коллекция ролей Ansible для настройки VPN, обратного прокси и шлюза.

* WireGuard VPN: проброс портов на внутренние ресурсы через VPS в интернете
* OpenVPN: подключение к корпоративной сети через интернет
* [jc21/nginx-proxy-manager](https://hub.docker.com/r/jc21/nginx-proxy-manager/tags) + [yobasystems/alpine-mariadb](https://hub.docker.com/r/yobasystems/alpine-mariadb/tags): проксирование внутренних ресурсов
* [portainer/portainer-ce](https://hub.docker.com/r/portainer/portainer-ce/tags): управление контейнерами Docker

## Зависимости

* [kyl191.openvpn](https://galaxy.ansible.com/kyl191/openvpn)
