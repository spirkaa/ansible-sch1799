# Роль Ansible: Keycloak

Роль Ansible, которая устанавливает и настривает [Keycloak](https://www.keycloak.org/).

## Выгрузка полной конфигурации реалма

```text
docker exec -it keycloak /opt/jboss/keycloak/bin/standalone.sh \
-Djboss.socket.binding.port-offset=100 -Dkeycloak.migration.action=export \
-Dkeycloak.migration.provider=singleFile \
-Dkeycloak.migration.realmName=school \
-Dkeycloak.migration.usersExportStrategy=REALM_FILE \
-Dkeycloak.migration.file=/data/realm-export.json
```
