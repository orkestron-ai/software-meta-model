# Регламенты работы с продуктовой метамоделью AISMM

## Назначение

Этот документ описывает не статические правила AISMM, а **динамику работы с продуктовой метамоделью**:

- как инициируется изменение модели
- как модель заполняется новыми знаниями
- как изменения согласуются
- как модель валидируется
- как проводится анализ качества и целостности модели
- как изменения замыкаются в историю, релизы и feedback loops

Документ собран из эталонной метамодели AISMM и переводит ее governance-логику в формат технологических процессов.

## Нормативная база

Процессы ниже основаны на следующих документах эталона:

- `aismm-structure.md`
- `aismm-unified-id-strategy.md`
- `aismm-model-registry.md`
- `aismm-empty-layer-standard.md`
- `aismm-strict-mode.md`
- `aismm-consistency-rules.md`
- `aismm-consistency-checks.md`
- `aismm-versioning-and-conformance.md`
- `aismm-semantic-diff.md`
- `aismm-temporal-validity.md`
- `aismm-feedback-loops.md`
- `b8-change-execution/801-work-items-and-change-requests.md`
- `b8-change-execution/802-planning-and-delivery-flow.md`
- `b8-change-execution/803-testing-and-acceptance-execution.md`
- `b8-change-execution/804-release-version-and-rollout-management.md`
- `b8-change-execution/805-change-history-and-decision-log.md`
- `b8-change-execution/807-feedback-loops-and-learning-cycles.md`
- `b8-change-execution/808-ci-cd-pipeline-and-automation.md`
- `b11-organization-ownership-governance/1103-decision-rights-and-governance.md`
- `b9-knowledge-traceability/904-context-coverage-and-consistency.md`
- `aismm-context-security.md`
- `aismm-security.md`
- `aismm-physical-identity-binding.md`

## 1. Роли в процессах

Эталонная метамодель не навязывает конкретные должности, но из нее выводятся обязательные процессные роли.

### 1.1. Обязательные процессные роли

- **инициатор изменения**: приносит новый сигнал, проблему, источник или proposal
- **редактор модели**: оформляет изменение в AISMM-блоках и слоях
- **владелец source-of-truth области**: подтверждает корректность факта в своем bundle
- **архитектор / системный владелец**: согласует архитектурные изменения
- **security owner**: согласует изменения security/privacy и чувствительных источников
- **value owner / product owner**: согласует изменения экономики, стратегии, product value
- **валидатор / CI gate**: проверяет strict-mode, consistency, completeness и ссылочную целостность
- **knowledge steward**: отвечает за provenance, traceability, completion status и history hygiene

### 1.2. Правило распределения authority

Право решения определяется не тем, кто редактирует файл, а тем, кто владеет областью истины.

Примеры:

- архитектурный факт согласуется владельцем `b2`
- ownership-факт согласуется владельцем `b11`
- requirement/behavior-факт согласуется владельцем `b4`
- privacy/security-факт согласуется владельцем `b7`
- traceability в `b9` не может утвердить новый факт вопреки source bundle

## 2. Upstream/Downstream lifecycle в эталонной AISMM

Для process-view эталонную метамодель удобнее читать не как линейный список слоев, а как два связанных контура.

### 2.1. Upstream

Upstream отвечает за то, что система **собирается сделать**, почему это нужно, как это будет проверяться и кто это согласовал.

```text
signal / source / incident / idea
   -> change request
   -> task decomposition and planning
   -> scenario and requirement clarification
   -> test design and quality gates
   -> approval to execute
```

В терминах эталона upstream в первую очередь покрывается слоями:

- `b0.002` — value articulation для actor-facing changes
- `b8.801` — change request, work items, acceptance scope, decision reason
- `b8.802` — planning, priorities, dependencies, delivery flow
- `b5.501` — user scenarios and flows
- `b7.701` — test strategy, test cases, quality gates, coverage
- `b11.1103` — approval policy и decision authority
- `b9.903` и `b9.904` — provenance, confidence, consistency, missing context

### 2.2. Downstream

Downstream отвечает за то, что система **реально сделала**, как это было протестировано, выпущено, подтверждено в runtime и замкнуто в историю.

```text
development execution
   -> test execution
   -> acceptance decision
   -> release candidate / rollout / rollback control
   -> runtime observation
   -> feedback loop
   -> post-release model actualization
```

В терминах эталона downstream в первую очередь покрывается слоями:

- `b8.803` — test runs, acceptance runs, approval decisions
- `b8.804` — release, version, rollout, rollback
- `b8.805` — change history and decision log
- `b8.807` — feedback loops and learning cycles
- `b8.808` — CI/CD pipeline and automation gates
- `b6.*` — runtime signals and post-release operational evidence
- `b9.902`, `b9.903`, `b9.904` — traceability, evidence, staleness, consistency

Для execution-stage после approval действуют дополнительные практические правила:

- implementation идет только внутри approved scope
- AI агент обязан явно показывать execution boundary, изменяемые объекты, результаты проверок и незакрытые gaps
- `b7.701` хранит test basis и quality gates, а `b8.803` хранит реальные verification results и acceptance evidence
- если execution подтверждает новую semantics, она должна быть обновлена в owning source-of-truth layers, а не оставаться только в code surfaces
- локально менять canon в product execution wave не рекомендуется; если найден canon defect, предпочтителен отдельный upstream/canon wave и синхронизация с reference canon repository, когда он доступен
- downstream-cycle не считается завершенным раньше runtime observation и reconciliation

### 2.3. Ключевой принцип разделения

До production release upstream-модель в значительной степени описывает:

- intent
- planned change
- expected behavior
- planned tests
- planned release path

После production release authoritative current-state должен уточняться по факту реально выпущенного и реально наблюдаемого состояния.

То есть эталонную AISMM нужно читать так:

- **до релиза**: модель может содержать plan, intent, proposal, acceptance scope и release intent
- **после релиза**: current-state слои должны быть актуализированы по реально доставленному продукту, runtime evidence и change history

Иначе AISMM остается моделью намерения, а не моделью реально работающего продукта.

### 2.4. Repository preconditions for agent-assisted work

- в корне product repository должен существовать `AGENTS.md`
- активная работа по изменению AISMM должна вестись вне `main`, если только нет явного governance-исключения
- `AGENTS.md` задает локальные инструкции агенту, но не заменяет `aismm.registry.json` и не меняет source-of-truth routing
- Pull Request и разделение implementation / verification / trace sync commit-ов могут рекомендоваться локальным процессом, но текущий канон не требует одной жесткой commit-sequence grammar

## 3. Процесс инициирования изменения модели

### 3.1. Триггеры процесса

Изменение модели может стартовать от одного из триггеров:

- новый исходный материал
- изменение продукта или архитектуры
- изменение требований
- runtime incident
- customer feedback
- audit finding
- test failure
- discovery / research result
- предложение по развитию модели
- выявленное противоречие или пробел в AISMM

### 3.2. Входы процесса

- источник или сигнал
- описание проблемы или возможности
- контекст изменения
- предполагаемый scope

### 3.3. Обязательные шаги

1. Зарегистрировать change intent.
2. Оформить work item / change request в логике `b8.801`.
3. Кратко описать:
   - что меняется
   - почему меняется
   - что является источником
   - какие альтернативы уже известны
4. Привязать изменение к связанным AISMM-сущностям, если они уже существуют.
5. Если изменение actor-facing, зафиксировать или обновить value articulation в `b0.002` до закрытия upstream-спецификации.

### 3.4. Выход процесса

- оформленный change request
- определенный scope затронутых bundle/layer
- определенные владельцы согласования

## 4. Процесс маршрутизации изменения по source-of-truth

### 4.1. Цель процесса

Определить, **в каком слое изменение должно быть оформлено как первичный факт**, а где оно будет только отражено как ссылка, projection или следствие.

### 4.2. Обязательные шаги

1. Классифицировать изменение:
   - observed fact
   - desired state
   - proposal
   - legal draft
   - external reference
   - open question
2. Определить source-of-truth bundle.
3. Зафиксировать, какие bundles являются только потребителями или projection layers.
4. Проверить, не возникает ли дублирование факта в нескольких местах.

### 4.3. Гейт процесса

Изменение нельзя вносить в `b9` как новый факт, если не определен source-of-truth слой.

### 4.4. Выход процесса

- карта затронутых слоев
- список owner-review точек
- решение: current fact / target state / proposal

## 5. Процесс заполнения модели новым материалом

### 5.1. Когда применяется

Применяется, когда в модель добавляется новый источник, новый слой знаний или новые сущности.

### 5.2. Входы

- source material
- тип материала
- source-of-truth bundle
- product_id и model_instance_id текущей модели

### 5.3. Шаги процесса

1. Определить, создается новый документ, новый блок или дополняется существующий.
2. Создать или обновить AISMM metadata block.
3. Если появляются новые сущности, сразу присвоить stable IDs.
4. Добавить content только в корректный source-of-truth слой.
5. Отдельно пометить observed facts, intent и proposals.
6. Зафиксировать provenance:
   - source_id
   - source_type
   - confidence
   - evidence boundaries
   - known gaps
7. Обновить `completion_status`, `known_gaps`, `rationale` при необходимости.
8. Если слой ожидается, но еще пуст, оформить empty layer semantics, а не оставлять его missing.
9. Если нужен preferred format, сослаться на него или использовать его.

### 5.4. Дополнительные шаги для новых источников

Если добавляется новый источник знания:

1. Завести его в `b9.903`.
2. Описать границы достоверности.
3. Указать, authoritative ли источник, intent-level ли это источник, или proposal-only.
4. Если источник внешне небезопасен, указать `authorship_class` и restrictions.

### 5.5. Выходы

- обновленные source-of-truth слои
- зарегистрированный provenance
- обновленный completion status

## 6. Процесс изменения уже существующего знания

### 6.1. Когда применяется

Используется, когда меняется уже существующая сущность, а не просто добавляется новая.

### 6.2. Входы

- existing entity or document
- semantic reason for change
- affected references and projections

### 6.3. Обязательные шаги

1. Классифицировать semantic change type:
   - entity_added
   - entity_removed
   - entity_changed
   - entity_moved
   - entity_renamed
   - reference_added / removed
   - trace_link change
   - confidence / provenance / schema change
2. Сохранить ID, если смысл сущности не изменился фундаментально.
3. Если ID меняется, задокументировать `aliases`, `previous_ids`, `replaced_by`, `supersedes`.
4. Обновить source-of-truth слой.
5. Затем обновить projections, references и traceability.
6. Если изменение действует не сразу или ограничено по времени, обновить temporal fields.
7. Если изменение breaking, оформить migration record.

### 6.4. Обязательный контрольный вопрос

После изменения нужно ответить:

`Это изменило только текст, или реально изменило модель?`

Если изменена модель, то обновление должно быть отражено не только в документе, но и в:

- traceability
- provenance
- change history
- validations

### 6.5. Выходы

- source layer updated
- projections corrected
- semantic diff identifiable
- temporal continuity preserved

## 7. Процесс согласования изменения

### 7.1. Цель процесса

Убедиться, что изменение подтверждено владельцами истины и governance-гейтами, а не только редактором модели.

### 7.2. Когда согласование обязательно

Обязательно, если изменение затрагивает:

- security/privacy layers
- architecture decisions
- economics and cost models
- ownership and governance
- high-criticality trace links
- schema breaking changes
- удаление сущностей без successor
- любые cross-bundle source-of-truth conflicts

### 7.3. Шаги процесса согласования

1. Определить тип решения и authority.
2. Определить approval policy.
3. Собрать reviewers по затронутым областям.
4. Подготовить semantic impact summary.
5. Если требуется, приложить migration plan.
6. Получить required approvals.

### 7.4. Минимальные review-гейты из эталона

- change in `b7.703` or `b7.705–707` -> security owner review required
- change in `b0.006` or `b12.*` -> value/economics owner review required
- architecture decision change in `b8.805` -> architect review required
- removal of high-criticality trace link -> explicit approval required
- schema breaking change -> migration record required

### 7.5. Выходы

- approved / rejected / conditionally approved change
- зафиксированный approver set
- escalation path, если согласование не достигнуто

## 8. Процесс валидации продуктовой модели

### 8.1. Цель процесса

Проверить, что модель после изменения остается структурно валидной, связной и безопасной.

### 8.2. Входы

- измененная AISMM-модель
- registry
- expected bundles and layers
- current validation level

### 8.3. Основные этапы валидации

1. Проверка registry-first discovery.
2. Проверка product_id и model_instance_id.
3. Проверка structure and metadata blocks.
4. Проверка expected layers against real blocks and empty declarations.
5. Проверка global ID uniqueness и ID format.
6. Проверка reference resolution.
7. Проверка traceability rules.
8. Проверка completion-status logic.
9. Проверка context-security rules.
10. Формирование validation artifact.

### 8.4. Набор обязательных проверок

- bundle structure checks
- layer file checks
- registry and completeness checks
- product identity checks
- layer ID format checks
- cross-layer reference checks
- metadata block checks

### 8.5. Mandatory strict-mode вопросы

- все ли blocks имеют `product_id` и `model_instance_id`?
- все ли cross-layer references резолвятся?
- нет ли duplicate IDs?
- все ли required layers покрыты real или empty blocks?
- есть ли `known_gaps` у `partial` layers?
- есть ли `rationale` у `not_applicable` layers?

### 8.6. Выходы

- `aismm.validation.json` или его эквивалент
- статус `passed / warnings / failed`
- список violations по severity

## 9. Процесс semantic review изменения

### 9.1. Цель процесса

Понять, **как изменилась модель как модель**, а не только как текст.

### 9.2. Входы

- base version
- changed version
- semantic diff artifact или ручная семантическая сводка

### 9.3. Шаги

1. Посчитать сущности добавленные, удаленные, измененные, перенесенные, переименованные.
2. Проверить trace link additions/removals.
3. Проверить confidence and provenance changes.
4. Проверить schema changes.
5. Отметить изменения, требующие review gates.
6. Подготовить semantic impact summary для согласования.

### 9.4. Выходы

- semantic diff summary
- список high-impact changes
- список required approvals

## 10. Процесс release / publication изменения модели

### 10.1. Цель процесса

Довести validated and approved model change до принятого состояния в основной ветке или продуктовой версии модели.

### 10.2. Входы

- validated change
- approvals
- semantic diff summary

### 10.3. Шаги

1. Привязать изменение к work item / change request.
2. Привязать результаты проверки к acceptance/approval.
3. Зафиксировать, входит ли изменение в release package модели.
4. При необходимости отметить effective release или temporal validity.
5. Обновить release/change-execution слои, если change model is versioned as part of SDLC.

### 10.4. Выходы

- accepted model state
- release-ready model update
- traceability from change to release/history

## 11. Процесс фиксации истории и решений

### 11.1. Цель процесса

Сделать изменение объяснимым задним числом.

### 11.2. Когда применяется

После каждого содержательного изменения, особенно если оно:

- меняет архитектуру
- меняет границы требований
- меняет риск или контроль
- меняет траекторию продукта

### 11.3. Шаги

1. Завести change event.
2. Если было решение, оформить decision log.
3. Зафиксировать rationale, constraints, assumptions, alternatives.
4. Связать change event с work item, release и затронутыми сущностями.
5. При необходимости завести architecture decision.

### 11.4. Выходы

- актуализированный `b8.805`
- сохраненный контекст причин изменения

## 12. Процесс feedback-loop-driven обновления модели

### 12.1. Когда применяется

Если изменение модели возникло не от планового feature-work, а от сигнала:

- incident
- customer feedback
- metric degradation
- audit finding
- test failure
- RAG/retrieval failure

### 12.2. Шаги

1. Зафиксировать feedback signal.
2. Создать feedback loop.
3. Привязать signal к root cause.
4. Превратить результат в improvement action.
5. Обновить requirement / risk / control / knowledge, если это требуется.
6. После внедрения связать outcome с release.
7. Зафиксировать knowledge summary.

### 12.3. Выходы

- feedback_loop record
- связанный change_request
- knowledge captured in history/summary

## 13. Процесс периодического анализа качества модели

### 13.1. Назначение

Проверять не конкретное изменение, а состояние модели как knowledge system.

### 13.2. Периодичность

Эталон не диктует жесткую частоту, но такой анализ должен выполняться регулярно после заметных изменений и как часть health checks модели.

### 13.3. Шаги анализа

1. Проверить completeness status модели.
2. Проверить покрытие expected layers.
3. Выявить missing context.
4. Выявить inconsistencies между source-of-truth и projections.
5. Проверить staleness и temporal ambiguity.
6. Проверить traceability coverage.
7. Проверить provenance and confidence weaknesses.
8. Проверить security/classification violations.
9. Зафиксировать список remediation actions.

### 13.4. Выходы

- coverage map
- missing context report
- inconsistency report
- remediation backlog

## 14. Процесс добавления нового слоя, bundle или structural extension

### 14.1. Когда применяется

Если меняется сама структура метамодели, а не только ее содержание.

### 14.2. Шаги

1. Оформить change request на structural change.
2. Проверить, additive change это или breaking change.
3. Обновить соответствующий governance/spec документ.
4. Обновить bundle schema.
5. Обновить bundle README.
6. Обновить root README и `aismm-structure.md`, если меняется карта bundles.
7. Обновить registry expected layers/bundles.
8. Регенерировать layer inventory.
9. Если change breaking, оформить migration record.

### 14.3. Выходы

- synchronized structure docs
- updated schemas
- updated inventory
- migration guidance if needed

## 15. Процесс работы с proposal в метамодели

### 15.1. Цель процесса

Не допустить утечки proposal state в baseline current-state модель.

### 15.2. Шаги

1. Явно пометить материал как proposal.
2. Не вносить его в observed sections как подтвержденную реализацию.
3. Хранить proposal отдельно от current-state facts.
4. Зафиксировать decision state: under consideration / approved / rejected / superseded.
5. Если proposal принят, провести отдельный change process перевода proposal -> current state.

### 15.3. Выходы

- proposal tracked without contaminating baseline

## 16. Процесс работы с sensitive и untrusted материалами

### 16.1. Шаги

1. Классифицировать source и block по authorship class.
2. Определить data classification: public / internal / confidential / restricted.
3. При необходимости изолировать материал в отдельный repo/module/submodule.
4. Если isolation невозможна, использовать encryption.
5. Не допускать попадания untrusted content в authoritative path без verification.
6. Применять redaction и access policy для context packages.

### 16.2. Выходы

- безопасно включенный или безопасно изолированный материал

## 17. Минимальный технологический процесс изменения модели

Если собрать эталонный AISMM в один короткий обязательный маршрут, получается такой baseline process:

1. Получить сигнал или источник.
2. Оформить change request.
3. Определить source-of-truth слой.
4. Внести изменение в source layer.
5. Обновить provenance, completion и traceability.
6. Обновить projections и связанные слои.
7. Прогнать validation and consistency checks.
8. Подготовить semantic diff summary.
9. Пройти approval gates.
10. Зафиксировать change history and decision log.
11. При необходимости связать изменение с release и feedback loop.

## 18. Короткий вывод

Эталонная метамодель требует работать с продуктовой моделью как с управляемой системой изменений.

Это означает:

- любое изменение должно иметь входной сигнал
- любое знание должно попасть в правильный source-of-truth слой
- любое значимое изменение должно пройти валидацию и semantic review
- high-impact изменения должны пройти согласование
- результат должен быть закреплен в history, traceability и, при необходимости, feedback loop

Именно это превращает работу с AISMM в технологический процесс, а не в ручное ведение документации.

## 19. Отдельные техпроцессы

Чтобы не перегружать этот обзорный документ длинным разбором полного SDLC-контура, upstream и downstream вынесены в отдельные файлы.

### 19.1. Upstream техпроцесс

Содержит:

- прием сигнала
- постановку изменения
- planning/decomposition
- подготовку acceptance basis
- связь сценариев, требований и тестов
- оценку полноты upstream-контура в эталоне

Файл:

- `00-meta/software-meta-model-main/operational-guides/reference_aismm_upstream_techprocess_2026-05-19.md`

### 19.2. Downstream техпроцесс

Содержит:

- orchestration исполнения
- delivery flow и sequencing
- execution, testing, acceptance
- release / rollout / feedback
- post-release actualization
- оценку полноты downstream-контура в эталоне

Файл:

- `00-meta/software-meta-model-main/operational-guides/reference_aismm_downstream_techprocess_2026-05-19.md`

### 19.3. Разделение ролей документов

- этот файл остается обзорным регламентом процессов работы с метамоделью
- upstream/downstream вынесены в отдельные техпроцессы
- rulebook и operational rules остаются отдельными нормативными документами

### 19.4. Bootstrap-документы для агентов

Для агентной автоматизации upstream и downstream дополнительно вынесены в отдельные bootstrap-файлы с жесткой последовательностью шагов, входами, выходами, гейтами и handoff-пакетами.

Файлы:

- `00-meta/software-meta-model-main/Bootstraps/reference_aismm_upstream_bootstrap_2026-05-19.md`
- `00-meta/software-meta-model-main/Bootstraps/reference_aismm_downstream_bootstrap_2026-05-19.md`