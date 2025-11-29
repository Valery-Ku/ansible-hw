# Ansible WordPress LAMP Setup — Homework

## Файлы проекта
- inventory.yaml: YAML inventory (localhost or remote host).
- wordpress-playbook.yaml: Playbook for LAMP + WordPress (latest stable).
- ansible-run.log: Full verbose logs from run (PLAY RECAP ok=18 changed=7).
- README.md: This file.

## Последовательность действий по применению сценария
Предполагаем хост — external VM IP (e.g., 192.168.56.10, from Vagrant or bridged network; user vagrant, SSH enabled via sudo apt install openssh-server).

1. **Подготовка inventory (YAML)**:
   - Edit inventory.yaml: Добавь remote host.
     ```
     all:
       children:
         webservers:
           hosts:
             192.168.56.10:  # IP хоста
               ansible_connection: ssh
               ansible_user: vagrant
               ansible_ssh_private_key_file: ~/.vbox_priv  # Or password
               ansible_python_interpreter: /usr/bin/python3
     ```
   - If password: ansible_ssh_pass: "vagrant" in hosts (or -k flag in run).

2. **Подготовка VM хоста** (one-time):
   - SSH access: sudo apt update; sudo apt install openssh-server; sudo systemctl start ssh.
   - Ansible in host: If no, install via playbook or manual (curl get-pip.py | python3 --user).

3. **Run playbook**:
   - From control node (your host or another VM): cd to playbook folder.
   - Команда: `ansible-playbook -i inventory.yaml wordpress-playbook.yaml -vvv -become -k`
     - **Ключи**:
       - -i inventory.yaml: Use custom inventory (YAML).
       - -v (or -vvv): Verbose (details tasks, apt output, errors).
       - -become: Sudo for apt/service (yes in playbook, but explicit OK).
       - -k: Prompt for SSH password (if no key; enter "vagrant").
       - --syntax-check: Test YAML (dry-run, no changes).
       - -e: Extra vars (e.g., -e "mysql_root_password=newpass").
   - Logs: `ansible-playbook ... -v > ansible-run.log 2>&1` — save to file.
   - Expected: 5-10 мин, PLAY RECAP ok=14+ changed=5+ (LAMP install, WP deploy).

4. **Test**:
   - SSH to хост: curl http://192.168.56.10/wordpress — WP HTML.
   - Setup: http://IP/wordpress (wizard: DB "wordpress", user "wpuser", pass "wppassword").

## Run results (localhost test)
- LAMP: Apache2 active, MariaDB running, PHP 8.3+.
- WordPress: /var/www/html/wordpress, wp-config.php ready.
- Logs: See ansible-run.log (excerpt: PLAY RECAP ok=18 changed=7 ignored=6).

Git: git init, add ., commit, push to GitHub.