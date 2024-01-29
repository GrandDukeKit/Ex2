С помощью Vagrant была поднята ВМ nginx.

Ansible playbook делает ряд действий на ВМ:

обновляет пакеты
name: update apt: update_cache=yes tags: - update apt
скачивает веб сервер ...
name: Install nginx apt: name: nginx state: latest notify: - restart nginx tags: - nginx-package
меняет конфигурацинный файл по шаблону
name: NGINX | Create NGINX config file from template template: src: templates/nginx.conf.j2 dest: /etc/nginx/vm_2/default notify: - reload nginx tags: - nginx-configuration
перезапускает сервер и перечитывает конфигурацию handlers:
name: restart nginx systemd: name: nginx state: restarted enabled: yes

name: reload nginx systemd: name: nginx state: reloaded
