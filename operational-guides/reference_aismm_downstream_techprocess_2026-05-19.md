# Downstream техпроцесс эталонной AISMM

## Назначение

Этот документ описывает **downstream-контур** эталонной метамодели AISMM: как уже одобренное изменение проходит исполнение, тестирование, принятие решения о production readiness, релиз, наблюдение и пост-релизную актуализацию модели.

Bootstrap-версия этого процесса для агентного исполнения вынесена в:

- `00-meta/software-meta-model-main/Bootstraps/reference_aismm_downstream_bootstrap_2026-05-19.md`

В этом документе downstream понимается как часть цикла, где модель должна уметь описать не только intent, но и фактически выполненное и реально доставленное изменение.

Execution-stage должен оставаться прозрачным: на каждом шаге должно быть понятно, что уже реализовано, что еще не реализовано, какие проверки реально выполнены, какие gaps остались и где требуется решение человека.

Downstream не должен считаться завершенным раньше runtime observation и reconciliation back into source-of-truth layers.

## 1. Общая схема downstream

```text
approved_for_execution
  -> orchestration of delivery
  -> development execution
  -> test execution
  -> acceptance decision
  -> release candidate / rollout / rollback control
  -> runtime observation
  -> feedback loop
  -> current-state actualization
```

### 1.1. Execution boundary and transparency

После approval downstream должен сохранять четкую границу исполнения:

- реализуется только approved scope
- любое behavior-changing решение вне явно одобренной границы поднимается на отдельное решение
- AI агент явно показывает текущий шаг, изменяемые объекты, основание изменения, результат проверки и незакрытые gaps
- если implementation reality расходится с approved scope, это расхождение поднимается явно, а не превращается в скрытое расширение волны

### 1.2. Что должно быть выполнено до implementation

До начала implementation downstream должен подтвердить:

- ветка change wave существует и не смешана с независимыми инициативами
- explicit approval уже materialized в связанных change/governance artifacts
- scope локализован по owning change и проверен на cascade impact
- для actor-facing change value articulation уже зафиксирована в `b0.002`
- если во время product execution выявлен defect канона, это не должно молча превращаться в локальное canon change; предпочтителен отдельный upstream/canon wave и синхронизация с reference canon repository, когда он доступен

## 2. Downstream-1: orchestration исполнения и последовательность delivery

### 2.1. Что требуется по смыслу

- разложить approved scope по delivery units
- определить последовательность исполнения задач
- определить, какие работы идут параллельно, а какие блокируют друг друга
- определить, что входит в текущий execution cycle, а что остается в backlog
- определить milestone / iteration / release candidate path
- провести change по стадиям delivery stream

### 2.2. Что это покрывает в эталоне

- `b8.802` задает `plan`, `sprint`, `milestone`, `delivery_stream`, `dependency`, `capacity`
- `b8.801` задает work items как исполнимые единицы change scope
- `b8.804` добавляет привязку milestones и execution outcome к release units
- `b8.808` добавляет stage/gate orchestration на уровне pipeline

### 2.3. Вывод

Оркестрация downstream в эталоне есть и выражена через flow, dependencies, milestones и pipeline stages.

## 3. Downstream-2: выполнение разработки

### 3.1. Что требуется по смыслу

- выполнить work items
- провести change through delivery stages
- прогнать automation gates
- получить build/deploy artifacts
- не подменять approved scope неявным design choice

### 3.2. Что это покрывает в эталоне

- `b8.802` покрывает delivery flow
- `b8.808` покрывает pipeline, stages, automation gates, promotion rules, pipeline runs and artifacts
- `b3.*` служит источником implementation artifacts, на которые downstream опирается
- `b8.801` удерживает связь implementation с approved scope и return-to-rework decision paths

### 3.3. Вывод

Инженерный execution-контур в эталоне представлен хорошо.

Implementation choice, который меняет согласованное поведение и не был явно покрыт approval, не должен приниматься молча во время исполнения.

## 4. Downstream-3: тестирование измененного продукта и acceptance decision

### 4.1. Что требуется по смыслу

- исполнить тесты на измененном продукте
- связать результаты с work items
- принять решение: ready for release / return to development / reject / conditional approval
- не считать change проверенным без фактического evidence

### 4.2. Что это покрывает в эталоне

- `b8.803` покрывает test runs, QA results, validation checks, approvals
- `b7.701` покрывает test definitions и quality gates
- `b11.1103` покрывает approval policy и authority

Важно различать:

- `b7.701` — test basis, quality gates и what must be verified
- `b8.803` — execution evidence, реальные test runs и acceptance outcomes

### 4.3. Сильные стороны эталона

- есть явная сущность `approval`
- есть явная связка `test_run -> validates -> work_item`
- есть явная связка `approval -> enables -> release`
- есть возможность отделять test design от фактически выполненной verification

### 4.4. Слабое место эталона

- нет единой выделенной state machine вида `planned -> in_dev -> in_test -> accepted -> released -> observed -> reconciled`
- переходы между повторной разработкой, повторным тестированием и возвратом на upstream распределены по нескольким слоям

### 4.5. Вывод

Этап acceptance в эталоне представлен сильно на уровне сущностей, но слабее на уровне единой процессной оркестрации.

Downstream не должен маркировать change как проверенный, если релевантные проверки не были реально выполнены и успешно завершены, либо если оставшийся gap не зафиксирован явно.

## 5. Downstream-4: решение о production readiness и rollout

### 5.1. Что требуется по смыслу

- понять, что именно входит в релиз
- принять go/no-go решение
- выполнить rollout
- иметь rollback path

### 5.2. Что это покрывает в эталоне

- `b8.804` покрывает release, release scope, rollout plan, deployment window, rollback plan, success criteria
- `b8.808` покрывает promotion rules, automation gates и rollback triggers
- `b11.1103` покрывает approval policy

### 5.3. Вывод

Release-governance и rollout в эталоне представлены хорошо.

## 6. Downstream-5: post-release observation и feedback

### 6.1. Что требуется по смыслу

- наблюдать поведение уже выпущенного продукта
- выявлять разницу между planned и actual
- запускать новый цикл при инциденте, деградации, жалобе или аудите

### 6.2. Что это покрывает в эталоне

- `b6.*` покрывает runtime side
- `b8.807` формализует feedback loop от сигнала к change request и knowledge summary
- `b8.805` фиксирует history and decisions

### 6.3. Вывод

Feedback-loop контур в эталоне представлен хорошо.

## 7. Downstream-6: актуализация current-state модели после релиза

### 7.1. Что требуется по смыслу

- сравнить intended change и реально выпущенный change
- зафиксировать real release scope
- обновить current-state source-of-truth слои по факту production reality
- пометить устаревшие assumptions, не сбывшиеся intent statements и недоставленные части scope
- не оставлять новую значимую semantics только в code/implementation surfaces

### 7.2. Что это покрывает в эталоне

- `b8.805` покрывает change history
- `b9.902` покрывает traceability graph и impact paths
- `b9.903` покрывает verification status и evidence
- `b9.904` покрывает staleness, missing context и inconsistency detection
- `b8.807` покрывает feedback from actual operation

Если execution подтвердил новую product semantics, ее owning layers обычно обновляются так:

- actor-facing value и anti-value — `b0.002`
- подтвержденные процессы — `b1.103`, когда именно этот слой владеет процессной семантикой
- user routes и UX logic — `b5.501`
- requirements — `b4.401`
- domain rules и invariants — `b4.402` и `b4.406`
- data and information semantics — `b2.202`
- implementation realization и artifacts — `b3.302`/`b3.303`

Новая значимая semantics не должна оставаться только в `b3.*`, если ее owning source-of-truth layer уже определим.

### 7.3. Слабое место эталона

- нет отдельного слоя или first-class сущности, которая бы задавала reconciliation как самостоятельный обязательный процесс
- обязанность обновить current-state после реального релиза выводится из нескольких слоев сразу

### 7.4. Вывод

Пост-релизная актуализация в эталоне присутствует концептуально сильно, но как отдельный process asset представлена слабо.

Именно поэтому downstream-cycle нельзя считать честно закрытым только по merge, release readiness или локальному deploy outcome без `D5` и `D6`.

## 8. Рекомендуемая repository discipline после approval

После approval downstream может рекомендовать, но не жестко навязывать, следующую repository discipline:

- Pull Request рекомендуется как основной путь к merge readiness
- разделение implementation, test evidence и trace sync commit-ов рекомендуется, когда это улучшает auditability
- единый commit `Verify and sync <scope>` допустим, когда verification и traceability реально не разделяются
- approval не должен задним числом растворяться внутри implementation-commit
- exact commit grammar остается repository-local; канон не требует одной жесткой последовательности commit names

## 9. Что в downstream-контуре эталона покрыто хорошо

- orchestration исполнения через delivery flow, dependencies, milestones и pipeline stages
- execution of work items и automation gates
- execution of tests и acceptance decisions
- release, rollout, rollback и promotion logic
- CI/CD как first-class SDLC entity
- feedback loops от runtime и quality signals назад в change cycle
- history, traceability, evidence, staleness и consistency

## 10. Что в downstream-контуре эталона покрыто слабо

### 9.1. Нет одного canonical downstream state flow

Эталон собирает downstream из `802`, `803`, `804`, `805`, `807`, `808`, `902`, `903`, `904`, но не дает один отдельный layer/process specification, где весь маршрут был бы описан как единая state machine.

### 9.2. Возврат на новый downstream cycle или обратно в upstream задан логически, но не как один унифицированный transition model

Из `803`, `804` и `807` понятно, что change может:

- пройти в release
- уйти на доработку
- породить новый feedback-driven cycle

Но единый явный transition grammar для этих возвратов не выделен.

### 9.3. Слабо оформлен контур `planned -> actual -> reconciled`

Эталон хорошо знает про release, runtime evidence, history и feedback, но не выделяет в отдельный самостоятельный слой reconciliation planned-state и deployed-state.

### 9.4. Пост-релизная актуализация source-of-truth слоев не зафиксирована как самостоятельный обязательный gate

В эталоне есть все кирпичи, чтобы требовать post-release model update, но сам этот шаг пока собирается аналитически, а не задается одной отдельной спецификацией процесса.

## 11. Итог по downstream

Эталонная AISMM хорошо покрывает downstream execution, test, release, feedback и post-release evidence.

Главный недостаток downstream-части не в отсутствии основных сущностей, а в отсутствии двух явно выделенных process assets:

- canonical downstream state flow
- reconciliation gate between intended and actual product state

При этом уже сейчас downstream должен трактоваться строго: implementation не выходит за approved scope, verification опирается на фактическое evidence, а cycle closure не наступает раньше runtime observation и reconciliation.