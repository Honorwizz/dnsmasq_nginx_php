# Инструкция по выполнению задач с Ansible

## 📌 Описание
Этот шаблон Ansible Playbook/Role автоматизирует развертывание и настройку инфраструктуры с использованием Ansible. Включает установку и настройку DNS-сервера, управление пользователями, настройку веб-серверов с PHP и обработку конфигурационных изменений.

## ✅ Выполненные задачи

### 1️⃣ Установка и настройка кеширующего DNS-сервера dnsmasq (dnsmasq_setup.yml)
**Шаги:**
- Установлен и запущен сервис dnsmasq.
- Настроен конфигурационный файл `/etc/dnsmasq.conf`.
- Добавлены внешние DNS-серверы (Google DNS).
- Служба включена и запущена автоматически при загрузке системы.

📌 **Используемые модули:** `apt`, `template`, `systemd`

---
### 2️⃣ Создание группы пользователей superusers с правами sudo (superusers_setup.yml)
**Шаги:**
- Создана группа `superusers`.
- Добавлены пользователи `user2` и `user3` в группу `superusers`.
- Настроены права доступа в файле `/etc/sudoers.d/superusers`, позволяющие пользователям использовать `sudo`.
- Применена валидация файла sudoers через `visudo`.

📌 **Используемые модули:** `group`, `user`, `lineinfile`

---
### 3️⃣ Установка и настройка веб-сервера nginx + php-fpm (nginx_php_setup.yml)
**Шаги:**
- Установлены `nginx` и `php-fpm`.
- Настроен конфигурационный файл nginx (`/etc/nginx/sites-available/default`).
- Создана директория для веб-контента `/var/www/html`.
- Создан файл `index.php` с `phpinfo()`.
- Включены и запущены службы `nginx` и `php-fpm`.

📌 **Используемые модули:** `apt`, `template`, `file`, `copy`, `systemd`

---
### 4️⃣ Улучшение конфигурации nginx + php-fpm (nginx_php_advanced.yml)
**Шаги:**
- Изменён `DocumentRoot` на `/opt/nginx/ansible`.
- Перенесён `index.php` в новую директорию.
- Добавлен обработчик для автоматического перезапуска `nginx` при изменении конфигурации.

📌 **Используемые модули:** `template`, `file`, `copy`, `systemd`

---

## 🔹 Пример файла `vault.yml` для хранения паролей:
```yaml
admin_password: "SuperSecurePassword123"
```
Для шифрования используйте команду:
```bash
ansible-vault encrypt vault.yml
```

---
## 🔹 Создание файла `hosts.yml` для управления серверами:
```yaml
all:
  hosts:
    web_server:
      ansible_host: 192.168.1.100
      ansible_user: your_user
      ansible_ssh_private_key_file: ~/.ssh/id_rsa
```

---
## 📌 Команды для работы с Ansible:

### Проверка доступности хостов:
```bash
ansible all -i hosts.yml -m ping
```

### Выполнение команды на всех серверах:
```bash
ansible all -i hosts.yml -m command -a "uptime"
```

### Установка и удаление nginx + php-fpm (nginx_php_manage.yml)
**Шаги:**
- При `--tags install_nginx_php` устанавливается и запускается `nginx` и `php-fpm`.
- При `--tags remove_nginx_php` удаляется `nginx` и `php-fpm`.

📌 **Используемые модули:** `apt`, `systemd`

---
## 📌 Запуск Playbook'ов
### Установка и настройка dnsmasq:
```bash
ansible-playbook -i hosts.yml dnsmasq_setup.yml
```
### Добавление пользователей superusers:
```bash
ansible-playbook -i hosts.yml superusers_setup.yml
```
### Установка nginx + php-fpm:
```bash
ansible-playbook -i hosts.yml nginx_php_setup.yml
```
### Изменение DocumentRoot и обновление конфигурации nginx:
```bash
ansible-playbook -i hosts.yml nginx_php_advanced.yml
