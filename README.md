<h1 align="center">Документация</h1>

## Где располагается репозиторий с кодом, как посмотреть существующий код

Здесь:
https://github.com/ZerruKun/SF-Project-2

## Настройка среды
- Установка VSCode
- Установка Git
- Идентификация и алиас (либо --local, если предполагаются иные настройки для других репозиториев):
```
git config --global user.name "Фамилия Имя"
git config --global user.email "your_email@example.com"
git config --global --add alias.history '!git log --graph'
```
- Генерация и отправка ключа:
```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
или
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
При указании имени нужно указывать абсолютный путь!
- Вывод ключа
### Linux
```
cat ~/.ssh/*Имя ключа*.pub
```
### Windows
```
C:\Users\Zalmo\.ssh\*Имя ключа*.pub -> открыть блокнотом
```
или
```
Get-Content C:\Users\*Пользователь*\.ssh\*Имя ключа*.pub | Set-Clipboard
```
Старт SSH и добавление ключа
```
Get-Service -Name ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
ssh-add C:\Users\*Пользователь*\.ssh\*Имя ключа*.pub
```
Создании фалов:
C:\Users\*Пользователь*\.ssh\config
```
Host github.com
 Hostname github.com
 IdentityFile ~/.ssh/*Имя закрытого ключа*
```

C:\Users\Zalmo\.bash_profile
```
test -f ~/.profile && . ~/.profile
test -f ~/.bashrc && . ~/.bashrc
```

C:\Users\Zalmo\.bashrc
```
# Start SSH Agent
#----------------------------

SSH_ENV="$HOME/.ssh/environment"

function run_ssh_env {
  . "${SSH_ENV}" > /dev/null
}

function start_ssh_agent {
  echo "Initializing new SSH agent..."
  ssh-agent | sed 's/^echo/#echo/' > "${SSH_ENV}"
  echo "succeeded"
  chmod 600 "${SSH_ENV}"

  run_ssh_env;

  ssh-add ~/.ssh/*Имя закрытого ключа*;
}

if [ -f "${SSH_ENV}" ]; then
  run_ssh_env;
  ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || {
    start_ssh_agent;
  }
else
  start_ssh_agent;
fi
```

- Далее необходимо отправить публичную чать ключа владельцу репозитория для предоставления доступа.
- Клонировать репозиторий
```
git clone git@github.com:ZerruKun/SF-Project-2.git
```

## Правило именования веток

*категория*/*функционал*

## Список категорий

- bug - ошибка
- feature - новый функционал
- fix - исправления
- release - номер версии

## Как устроен процесс внесения своих изменений в основную кодовую базу

- Клонирование:
```
git clone git@github.com:ZerruKun/SF-Project-2.git
```
- Создание ветки:
```
git checkout -b *категория*/*функционал* *ветка*
```
- Наблюдение и изменение:
```
git add *Файл или "."*
git commit -m "комментарий"
```
- Отправка изменения в удалённый репозиторий
```
git push *репозиторий* *ветка*
```
- Создать Pull Request на проверку кода
- Слить ветки
```
git checkout *ветка*
git pull origin *ветка*
git merge --no-ff *категория*/*функционал*
git push origin *ветка*
```