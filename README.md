### Запуск Gitlab

При запуске в докере гитлаба будут созданы папки  `confgi` `data` `logs` - эти папки подключены к контейнеру с gitlab по сути это файлы gitlab которым можно редакторовать на хосте при надобности, но смотреть чтоб сохранялись на папки и файлы права (обычно root юзера)

Для запуска нашего контейнера с Gitlab нужно поднастроить переменные окружения.

Из файла `.env.template` создаем файл `.env` в котором меняем путь до папки с gitlab:

```
GITLAB_HOME=/Users/Path/to/Gitlab/folder
```
далее 
```
docker-compose up
```

Ждем когда на адресе `localhost:8081` запустится gitlab
он может это долго делать. В идеаеле смотреть что в консоли вываливает, бывает что не стартует потому-что пароль в docker-compose.yaml задан слабый. Тот который установлен отвечает всем требованиям.


### Gitlab запустился на http://localhost:8081

Заходим под `root@local / 123qweASDF#`

Создаем проект или группу и проект в ней.

Регистрируем для проекта или для группы gitlab-runner.

Вот тут описан процесс установки: [Офф док](https://docs.gitlab.com/runner/install/osx.html)

**Самое основное**

Качаем файл раннера сразу складируем его в `/usr/local/bin/gitlab-runner`:

For Intel-based systems:

```shell
sudo curl --output /usr/local/bin/gitlab-runner "https://s3.dualstack.us-east-1.amazonaws.com/gitlab-runner-downloads/latest/binaries/gitlab-runner-darwin-amd64"
```

For Apple Silicon-based systems:
```shell
sudo curl --output /usr/local/bin/gitlab-runner "https://s3.dualstack.us-east-1.amazonaws.com/gitlab-runner-downloads/latest/binaries/gitlab-runner-darwin-arm64"
```


Даем ему пправа на исполнение:
```shell
sudo chmod +x /usr/local/bin/gitlab-runner
```

Устанавливаем его чтоб раннер прописался в автозапуск:

```shell
cd ~
gitlab-runner install
gitlab-runner start
```

Если нужно -
Reboot your system.

### Как проверит что раннер установился корректно и все ок
```shell
gitlab-runner stop
gitlab-runner start
gitlab-runner status
gitlab-runner verify
```
### Если есть проблемы
то мне помогла вот эта ссылка https://gitlab.com/gitlab-org/gitlab-runner/-/issues/29383

### Если все ок, то можно присупать к регистрации раннера.

Идем в проекты и Ranners 

Там находим кнопку New project runner

![New project runner](<images/new_project-runner.png>)

Если после нажатия кнопки получаем пустую страницу скорее всего порт не указан в url - добавьте.

![alt text](<images/Снимок экрана 2024-05-11 в 18.07.48.png>)

Берем параметры регистрации - в адресе должен быть указан порт!
![alt text](<images/Снимок экрана 2024-05-11 в 18.08.11.png>)

Exeuter = shell

![alt text](<images/Снимок экрана 2024-05-11 в 18.08.11.png>)

Если все ок, то увидим надпись на странице регистрации раннера

![alt text](<images/Снимок экрана 2024-05-11 в 18.08.35.png>)

Если нет, повторяем процесс. Добиваемся такого результата.

![alt text](<images/Снимок экрана 2024-05-11 в 18.09.01.png>)

