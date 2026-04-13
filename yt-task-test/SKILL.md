---
name: yt-task-test
description: Получает задачу из Яндекс Трекера и тестирует работоспособность и функциональность. Используется, когда пользователь передает название ветки и задачу из Яндекс Трекера в качестве аргумента для этого skill.
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
  - при успехе следующий skill: `yt-task-mr`
  - при ошибке следующий skill: `yt-task-work`

# План работы:
1. Проверяем статус "Можно тестировать".

2. Увеличиваем agent_meta.attempts.test
  - если `attempts.test > 3`: добавляем к задаче комментарий об ошибке и добавляем "@vitaliy.gusev" и завершаем

3. Переводим в "Тестируется".
  - добавляем запись в `status_history`

4. Запускаем тесты.

5. Если тесты упали:
  - фиксируем ошибку в `agent_meta.last_error` (`last_error.stage = "test"`)
  - переводим в "Взять в работу"
  - добавляем запись в `status_history`
  - останавливаем docker
  - вызываем yt-task-work

6. Если все ок:
  - очищаем `agent_meta.last_error`
  - останавливаем docker
  - переводим в "Готово к релизу"
  - добавляем запись в `status_history`
  - вызываем yt-task-mr

# Инструменты:
Skill: yt-task-work
Skill: yt-task-mr

MCP Server: yandex-tracker-mcp@latest