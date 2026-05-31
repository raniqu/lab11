# Отчет по лабораторной работе 11

## Цель работы
Научиться создавать сеансы совместной разработки с помощью терминального мультиплексора tmux и туннельных сервисов (ngrok/bore).

### 1. Установка tmux

```
$ sudo apt update && sudo apt install -y tmux
$ tmux -V
tmux 3.3a
```

### 2. Попытка использования ngrok (неудачно)

Сначала была предпринята попытка использовать официальный сервис ngrok в соответствии с оригинальной инструкцией.

Ошибка:
```
arina@debian:~$ ngrok tcp 22
ERROR:  failed to start tunnel: You must add a credit or debit card before you can use TCP endpoints on a free account. We require a valid card as a way to combat abuse and keep the internet a safe place. This card will NOT be charged.
ERROR:  Add a card to your account here: https://dashboard.ngrok.com/settings#id-verification.
ERROR:
ERROR:  ERR_NGROK_8013
ERROR:  https://ngrok.com/docs/errors/err_ngrok_8013
ERROR:
arina@debian:~$
```
На бесплатных аккаунтах ngrok теперь требуется привязка банковской карты для использования TCP-туннелей. Это новое требование сервиса, которое делает невозможным использование ngrok в учебных целях без финансовых затрат.

### Использование bore

В качестве бесплатной альтернативы был выбран open-source инструмент bore, который не требует регистрации и привязки карты.

```
arina@debian:~$ rustc --version
rustc 1.96.0 (ac68faa20 2026-05-25)
```

```
arina@debian:~$ sudo systemctl status ssh
[sudo] password for arina:
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-05-31 05:01:05 HST; 2h 16min ago
```

### Запуск туннеля через bore

```
arina@debian:~$ ~/.cargo/bin/bore local 22 --to bore.pub
2026-05-31T17:22:27.557084Z  INFO bore_cli::client: connected to server remote_port=41763
2026-05-31T17:22:27.557424Z  INFO bore_cli::client: listening at bore.pub:41763
```

### Завершение работы

```
arina@debian:~$ tmux kill-session -t session_with_group
```
