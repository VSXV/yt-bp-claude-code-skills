---
name: yt-task-work
description: Изучает полученную задачу из Яндекс Трекера и название гит ветки (опционально), выполняет работу по задаче и фиксирует результат в описании этой задачи. Используется, когда пользователь передает задачу из Яндекс Трекера в качестве аргумента для этого skill.
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
  - `branch` (опционально, иначе берем из `agent_meta.branch`)
- Выход:
  - коммиты в `agent_meta.branch`
  - обновленный `agent_meta`
  - следующий skill: `yt-task-review`

# План работы:
1. Проверяем статус "Взять в работу". Иначе — выход.

2. Увеличиваем agent_meta.attempts.work
  - если `attempts.work > 3`: добавляем к задаче комментарий об ошибке и добавляем "@vitaliy.gusev" и завершаем

3. Переводим задачу в "В работе".
  - добавляем запись в `status_history`

4. Проверяем:
  - есть ли ветка (agent_meta.branch)
  - если нет → создаем

5. Синхронизируемся с main.

6. Выполняем задачу по plan + acceptance criteria.

7. Коммитим изменения.

8. Обновляем описание задачи (что сделано, какие файлы изменены).
  - очищаем `agent_meta.last_error`, если ранее были ошибки

9. Переводим в "Готово к ревью".
  - добавляем запись в `status_history`

10. Вызываем yt-task-review

# Инструменты:
Skill: yt-task-review

MCP Server: yandex-tracker-mcp@latest