# Ansible Project: WordPress LAMP on Debian VM

## Последовательность действий (при наличии хоста)
Предполагаем хост — localhost (manual VM). Если external IP (e.g., 192.168.56.10):
1. Edit inventory.yaml: Добавь hosts: 192.168.56.10 (ansible_connection: ssh, ansible_user: vagrant, ansible_ssh_private_key_file: ~/.vbox_priv).

2. Run: `ansible-playbook -i inventory.yaml wordpress-playbook.yaml -v -k` ( -k for SSH password if needed; -vvv for more verbose).

3. Ключи: -i (inventory), -v (verbose), -vvv (debug), --syntax-check (check only), -become (sudo, default in playbook).

## Файлы
- inventory.yaml: YAML inventory for localhost (local connection).
- wordpress-playbook.yaml: Playbook for LAMP + WordPress (vars: DB pass "password", WP DB "wordpress/wppuser/wppassword").
- ansible-run.log: Logs from run (PLAY RECAP ok=18 changed=7).

## Run results
- LAMP: apache2, mariadb-server, php 8.3+ installed.
- MySQL: DB "wordpress", user "wpuser" with ALL privs.
- WordPress: Downloaded latest, in /var/www/html/wordpress, wp-config.php configured, permissions www-data.
- Test: curl http://localhost/wordpress — WP HTML OK.
- Logs excerpt:
  PLAY RECAP: ok=18 changed=7 failed=0 ignored=6
  Apache: active (running)
  MariaDB: active (running)

## Setup
1. VM: Manual Debian 13 ISO in VirtualBox (vagrant/vagrant, sudo).
2. Ansible: pip3 install --user ansible --break-system-packages.
3. Repo: Trixie main + security (apt update no conflicts).
4. Shared: /mnt/shared mounted from host ansible-hw folder.

Git: git init, add ., commit -m "Ansible WP Complete".