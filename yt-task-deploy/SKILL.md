---
name: yt-task-deploy
description: Получает задачу из Яндекс Трекера, активирует рабочую ветку и поднимает docker-контур для проверки. Используется, когда пользователь передает название ветки и задачу из Яндекс Трекера в качестве аргумента для этого skill.
agent_meta:
  branch: agent/task-name
  status_history: []
  plan:
    - ...
  acceptance_criteria:
    - ...
  risks:
    - ...
  dependencies:
    - ...
  last_error:
    stage: ""
    message: ""
  attempts:
    work: 0
    test: 0
    deploy: 0
---
## Контракт
- Вход:
  - `issue_key` (обязательно)
  - `branch` (обязательно)
- Выход:
  - при успехе следующий skill: `yt-task-test`
  - при ошибке следующий skill: `yt-task-work`

# План работы:
1. Проверяем статус "Подтвержден".

2. Переводим в "Деплой".
  - добавляем запись в `status_history`

3. Активируем ветку.
f
4. Поднимаем docker compose.

5. Проверяем:
  - контейнеры поднялись
  - healthcheck OK

6. Если ошибка:
  - фиксируем в agent_meta.last_error
  - увеличиваем attempts.deploy
  - указываем `last_error.stage = "deploy"`
  - если `attempts.deploy > 3`: добавляем к задаче комментарий об ошибке и добавляем "@vitaliy.gusev" и завершаем
  - переводим в "Взять в работу"
  - добавляем запись в `status_history`
  - вызываем yt-task-work

7. Если все ок:
  - очищаем `agent_meta.last_error`
  - переводим в "Можно тестировать"
  - добавляем запись в `status_history`
  - вызываем yt-task-test

# Инструменты:
Skill: yt-task-work
Skill: yt-task-test

MCP Server: yandex-tracker-mcp@latest