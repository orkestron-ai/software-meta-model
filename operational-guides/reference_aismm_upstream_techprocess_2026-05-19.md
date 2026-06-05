# Upstream техпроцесс эталонной AISMM

## Назначение

Этот документ описывает **upstream-контур** эталонной метамодели AISMM: как сигнал превращается в оформленное, спланированное и проверяемое изменение.

Bootstrap-версия этого процесса для агентного исполнения вынесена в:

- `00-meta/software-meta-model-main/Bootstraps/reference_aismm_upstream_bootstrap_2026-05-19.md`

Для actor-facing изменений upstream должен явно зафиксировать value impact в `b0.002` до того, как сценарно-требовательная часть считается завершенной.

В этом документе upstream понимается как часть цикла, где система еще не меняет production state, а формирует:

- intent
- change scope
- delivery plan
- value articulation для actor-facing change
- acceptance basis
- test basis
- approval to execute

## 1. Общая схема upstream

```text
signal / source / incident / idea
  -> change intake
  -> change request formalization
  -> decomposition and planning
  -> value articulation for actor-facing change
  -> scenario / requirement clarification
  -> acceptance basis
  -> test design and quality gates
  -> approval to execute
```

## 2. Upstream-1: прием сигнала и постановка изменения

### 2.1. Что требуется по смыслу

- принять сигнал
- понять, почему он важен
- оформить его как change request
- связать с требованиями, компонентами, рисками и контекстом

### 2.2. Что это покрывает в эталоне

- `b8.801` покрывает это хорошо
- `b9.903` добавляет provenance и confidence
- `b9.904` добавляет missing context и inconsistency detection
- `b8.807` позволяет заводить изменение не только от идеи, но и от incident, audit, test failure, metric degradation

### 2.3. Вывод

Этап входа сигнала в изменение в эталоне представлен сильно.

## 3. Upstream-2: planning и decomposition

### 3.1. Что требуется по смыслу

- превратить change request в исполнимые work items
- разложить работу по зависимостям, приоритетам и этапам
- определить, что пойдет в текущий цикл, а что уйдет дальше
- определить delivery constraints

### 3.2. Что это покрывает в эталоне

- `b8.801` задает work items и change requests
- `b8.802` задает plans, sprints, milestones, delivery streams, dependencies, capacity

### 3.3. Вывод

Planning/decomposition в эталоне представлен хорошо.

## 4. Upstream-3: уточнение сценариев и требований

### 4.1. Что требуется по смыслу

- если change actor-facing, явно зафиксировать value outcome и anti-value в `b0.002`
- зафиксировать user scenarios
- уточнить interaction flows
- связать сценарии с требованиями и бизнес-правилами
- определить, что именно должно быть подтверждено downstream-контуром

### 4.2. Что это покрывает в эталоне

- `b0.002` задает value articulation для actor-facing change
- `b5.501` задает user scenarios и interaction flows
- `b4.*` задает requirement и business-rule basis
- `b8.801` содержит acceptance scope

### 4.3. Вывод

Основание для acceptance формируется в эталоне достаточно хорошо, хотя распределено между несколькими слоями.

## 5. Upstream-4: подготовка acceptance basis и тестов

### 5.1. Что требуется по смыслу

- преобразовать user scenarios в test scenarios и test cases
- определить quality gates
- заранее зафиксировать критерии готовности к следующему этапу

### 5.2. Что это покрывает в эталоне

- `b7.701` задает test strategy, test cases, test suites, coverage и quality gates
- `b8.801` содержит acceptance scope
- `b5.501` дает сценарную основу для тестов

### 5.3. Сильные стороны эталона

- есть связь `scenario -> test_case`
- есть связь `requirement/business_rule -> test_case`
- есть связь `quality_gate -> release`

### 5.4. Слабое место эталона

- нет отдельного first-class процесса, который явно описывает генерацию тестов из сценариев как самостоятельный технологический шаг
- трансформация `scenario -> tests -> acceptance basis` в эталоне задана семантически, но не оформлена как отдельный orchestration artifact

### 5.5. Вывод

Тестовый upstream-контур в эталоне представлен сильно, но сам процесс сборки test basis выражен неявно.

## 6. Upstream-5: approval to execute

### 6.1. Что требуется по смыслу

- понять, кто имеет authority на запуск исполнения
- зафиксировать approval policy
- не передавать change в downstream execution без требуемых согласований

### 6.2. Что это покрывает в эталоне

- `b11.1103` покрывает decision authority и approval policy
- `b8.801` хранит decision context
- `b9.903` и `b9.904` дают trustworthiness basis для решения

### 6.3. Вывод

Governance-гейт перед downstream execution в эталоне представлен хорошо.

## 7. Что в upstream-контуре эталона покрыто хорошо

- вход сигнала в change request
- formalization work items и change scope
- planning, priorities, dependencies и delivery flow
- сценарная и requirement-basis для acceptance
- test model, quality gates и coverage model
- approval policy и decision rights
- provenance, confidence и consistency checks

## 8. Что в upstream-контуре эталона покрыто слабо

### 8.1. Нет отдельного process asset для трансформации `scenario -> tests`

Связи есть, но нет отдельной технологической спецификации этого перехода.

### 8.2. Нет одного canonical upstream state flow

Эталон хорошо описывает upstream сущности, но не дает одну явную state machine вида:

```text
signal_received -> scoped -> planned -> specified -> test_ready -> approved_for_execution
```

## 9. Итог по upstream

Эталонная AISMM хорошо покрывает upstream как контур intake, planning, specification и approval.

Главный недостаток upstream-части не в отсутствии сущностей, а в отсутствии одного явного orchestration view, который бы связывал signal, scenario, acceptance basis и approval в один формальный flow.