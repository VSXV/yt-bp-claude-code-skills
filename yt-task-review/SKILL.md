---
name: yt-task-review
description: Изучает полученную задачу из Яндекс Трекера и название гит-ветки, проверяет работу по задаче и принимает решение о качестве разработанного кода. Используется, когда пользователь передает название ветки и задачу из Яндекс Трекера в качестве аргумента для этого skill.
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
  - при успехе следующий skill: `yt-task-deploy`
  - при ошибке следующий skill: `yt-task-work`

# План работы:
1. Проверяем статус "Готово к ревью".

2. Переводим в "Ревью".
  - добавляем запись в `status_history`

3. Проверяем:
  - покрытие acceptance criteria
  - нет лишних изменений
  - код проходит линтер
  - нет критичных ошибок

4. Если есть проблемы:
  - фиксируем их в agent_meta.last_error
  - указываем `last_error.stage = "review"`
  - переводим в "Взять в работу"
  - добавляем запись в `status_history`
  - вызываем yt-task-work

5. Если все ок:
  - очищаем `agent_meta.last_error`
  - переводим в "Подтвержден"
  - добавляем запись в `status_history`
  - вызываем yt-task-deploy

# Инструменты:
Skill: yt-task-work
Skill: yt-task-deploy

MCP Server: yandex-tracker-mcp@latest