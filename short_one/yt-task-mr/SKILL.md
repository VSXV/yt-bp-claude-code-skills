---
name: yt-task-mr
description: Получает задачу из Яндекс Трекера и название ветки, на основании этих данных создает merge request. Используется, когда пользователь передает название ветки и задачу из Яндекс Трекера в качестве аргумента для этого skill.
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
---
## Контракт
- Вход:
  - `issue_key` (обязательно)
  - `branch` (обязательно)
- Выход:
  - при ошибке следующий skill: `yt-task-work`

# План работы:
1. Проверяем статус "Подтвержден".

2. Создаем MR:
  - из agent_meta.branch
  - в main
  - с описанием:
    - что сделано
    - acceptance criteria
    - как тестировали
  - Ответственный: vitaliy.gusev

3. Завершаем работу.

# Инструменты:
Skill: claude-glab-skill

MCP Server: yandex-tracker-mcp@latest