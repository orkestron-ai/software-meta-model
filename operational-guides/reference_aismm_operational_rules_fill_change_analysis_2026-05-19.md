# Правила заполнения, изменения и анализа метамодели AISMM

## Назначение

Этот документ собирает практические правила работы с метамоделью AISMM в трех режимах:

- заполнение модели
- изменение модели
- анализ модели

Документ основан на эталонной модели из `c:\MetaModel\Git\software-meta-model-main` и переводит governance-спеки в рабочий регламент.

## Нормативная база

Правила ниже собраны из следующих документов эталона:

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
- `aismm-physical-identity-binding.md`
- `aismm-context-security.md`
- `aismm-security.md`
- `aismm-agent-contract-references.md`
- `aismm-layers-prefs.md`

## 1. Правила заполнения метамодели

### 1.1. Наполнять нужно блоками, а не файлами

- Истинная модель AISMM состоит из блоков, а не из файловой структуры.
- Любой файл является контейнером знаний, но не источником идентичности сущностей.
- При заполнении сначала определяется, какой AISMM-блок создается или обновляется, а уже потом выбирается файл-контейнер.

### 1.2. Каждый новый блок должен быть оформлен канонически

При создании или расширении блока нужно обязательно задавать:

- `type`
- `model_instance_id`
- `product_id`
- `layer_id`
- `layer_key`
- `document_id`
- `document_type`
- `module_scope`
- `status`
- `spec_version`

Если документ является layer document, рекомендуется также всегда задавать:

- `completion_status`
- `title`
- `references`

### 1.3. Заполнять только source-of-truth слой

Любой новый факт сначала должен быть привязан к bundle-источнику истины.

Главные правила:

- продуктовая ценность и экономика продукта заполняются в `b0`
- стратегия и гипотезы заполняются в `b1`
- архитектура и структура системы заполняются в `b2`
- реализация и артефакты заполняются в `b3`
- требования и поведение заполняются в `b4`
- UX и пользовательские сценарии заполняются в `b5`
- runtime-факты заполняются в `b6`
- quality, risk, compliance заполняются в `b7`
- изменения, релизы, work items и feedback loops заполняются в `b8`
- provenance, traceability и retrieval заполняются в `b9`
- ownership заполняется в `b11`
- техническая экономика заполняется в `b12`

Нельзя сначала заводить факт в projection-слое, если source-of-truth слой для него не определен.

### 1.4. Нужно различать тип утверждения

Перед добавлением нового содержания его нужно классифицировать как одно из следующих:

- observed fact
- source intent
- proposal
- external inspiration
- legal draft
- open question
- known gap

Рабочее правило:

- observed fact можно вносить в current-state слой
- source intent можно вносить как целевое поведение, но не как подтвержденную реализацию
- proposal нужно отделять от baseline и маркировать как proposal
- external inspiration не должна превращаться в продуктовый факт без отдельного подтверждения
- legal draft нельзя считать действующей политикой без валидации
- open question не превращается в знание без ответа

### 1.5. Каждое новое утверждение должно иметь provenance

При заполнении модели для каждого нового блока или содержательного утверждения нужно понимать:

- откуда взялся источник
- насколько он надежен
- является ли он primary source или derivative source
- относится ли он к current implementation или только к intent

Минимум, который должен быть отражен:

- источник в `b9.903`
- уровень уверенности
- границы применимости источника
- known gaps и ограничения источника

### 1.6. Заполнять нужно с сохранением различий между current state и target state

- Текущее поведение и текущая архитектура должны описываться отдельно от желаемого состояния.
- Proposal не должен попадать в observed sections как уже существующая реализация.
- Если слой содержит и current state, и desired state, это должно быть явно разделено секциями.

### 1.7. Completion status обязателен как смысл, а не только как поле

При заполнении слоя нужно явно выбрать `completion_status`:

- `not_started`
- `empty`
- `partial`
- `complete`
- `not_applicable`
- `deferred`

Дополнительные правила:

- `partial` требует `known_gaps`
- `not_applicable` требует `rationale`
- `complete` допустим только если слой действительно закрывает свою область

### 1.8. Expected layers нельзя оставлять неразличимыми

Если слой ожидается, то он должен быть представлен одним из вариантов:

- реальным AISMM-блоком
- explicit empty layer block
- записью в `declared_empty_layers`

Нельзя оставлять expected layer просто отсутствующим без объяснения.

### 1.9. Новые сущности должны сразу получать стабильные ID

Для сущностей, которые будут ссылаться из других слоев, нужно сразу заводить global ID формата:

`b{bundle}.{layer}.{entity_type}.{slug}`

При заполнении нельзя:

- завязывать ID на путь файла
- использовать множественное число в `entity_type`
- использовать временные slug вроде `temp_fix`, `final_version`, `new_task`

### 1.10. Ссылки при заполнении должны быть машиночитаемыми

- Все cross-layer ссылки нужно задавать через global IDs.
- Нельзя описывать связь только текстом, если это сущностная связь модели.
- Если в модели появляется важная зависимость, ее нужно либо зафиксировать в source layer, либо проецировать в `b9.902`.

### 1.11. Заполнение слоя должно учитывать preferred format, но Markdown остается fallback

- Если для слоя есть preferred representation, ее нужно использовать, когда это реально улучшает читаемость и machine readability.
- Если такого формата нет, допустим Markdown с AISMM-блоками.
- Предпочтительный формат не отменяет необходимость AISMM metadata.

### 1.12. При заполнении чувствительного контента нужно применять context-security правила

- Любой импортированный или внешне полученный материал должен иметь корректный authorship class.
- `imported_untrusted` и `generated_unverified` не должны трактоваться как authoritative.
- Restricted content не должен попадать в открытые или неавторизованные context packages.

### 1.13. Заполнять физическую трассировку нужно через binding, а не через вольный текст

Если нужно связать логическую сущность с кодом, схемой, артефактом или runtime-объектом, следует использовать binding-подход:

- `symbol_binding`
- `schema_binding`
- `artifact_binding`
- `runtime_binding`
- `digest_binding`

### 1.14. При заполнении AI-related взаимодействий нельзя смешивать AISMM и agent contracts

- AISMM не определяет agent contracts.
- AISMM только ссылается на них и фиксирует эффекты их работы.
- Все capability schemas, result schemas и execution contracts должны жить вне AISMM.

## 2. Правила изменения метамодели

### 2.1. Изменять модель нужно как semantic model, а не как текст

Любое изменение должно мыслиться как один или несколько semantic change types:

- `entity_added`
- `entity_removed`
- `entity_changed`
- `entity_moved`
- `entity_renamed`
- `reference_added`
- `reference_removed`
- `trace_link_added`
- `trace_link_removed`
- `trace_link_type_changed`
- `trace_link_weight_changed`
- `confidence_changed`
- `provenance_changed`
- `coverage_changed`
- `schema_changed`
- `layer_added`
- `layer_deprecated`
- `bundle_added`

### 2.2. Сначала менять source-of-truth, потом projection

- Если факт меняется в source bundle, сначала обновляется source-of-truth слой.
- Затем обновляются projections, trace links, summaries и retrieval units.
- Если projection противоречит source, выигрывает source, а projection подлежит регенерации.

### 2.3. ID нельзя менять без сильной причины

ID сохраняется при:

- переписывании текста
- переносе документа
- расширении описания
- смене статуса

ID допустимо менять только если:

- сущность реально переопределена
- сущность разделилась на новые сущности
- прошлый ID был неверным

При изменении ID рекомендуется сохранять:

- `aliases`
- `previous_ids`
- `replaced_by`
- `supersedes`

### 2.4. Изменение требования должно тянуть связанные слои

При изменении requirement нужно перепроверять:

- связанные behavior or domain rules
- trace links
- tests or validation checks
- work items
- release impact

Требование, которое изменилось, не должно оставлять старые тестовые или traceability-связи без ревизии.

### 2.5. Breaking changes требуют migration record

Если изменение является schema-breaking, layer split/merge, field rename или затрагивает совместимость модели, нужно оформлять migration record в `migrations/`.

### 2.6. Изменение temporal semantics нужно фиксировать явно

Если сущность меняется во времени, следует использовать temporal fields:

- `valid_from`
- `valid_to`
- `effective_from_release`
- `effective_until_release`
- `superseded_by`
- `supersedes`
- `archived_at`
- `review_at`
- `temporal_scope`

Это обязательно, если:

- сущность действует не сразу
- сущность депрекейтится
- сущность заменена новой
- анализ должен отвечать на вопрос «что было валидно в конкретный момент времени»

### 2.7. Изменения high-impact областей требуют специальных review gates

Из эталона следуют минимум такие review rules:

- изменение security layers требует security review
- изменение economics model требует review владельца ценности или экономики
- удаление high-criticality trace link требует явного approval
- schema breaking change требует migration record
- удаление сущности без successor создает orphan risk и требует review

### 2.8. Если модель меняет агент, это должно быть видно

При agent-authored изменении блок должен отражать:

- `last_modified_by`
- `authorship_class: agent_authored`
- `agent_contract_reference`

Изменения от агента не должны выглядеть как безличное ручное редактирование.

### 2.9. Изменения должны быть проверяемы через semantic diff

Для review полезно фиксировать:

- какие entities добавлены
- какие entities удалены
- какие trace links изменены
- какие confidence/provenance значения изменились
- были ли schema changes

Текстовый diff недостаточен, если меняется смысл модели.

### 2.10. Изменение completeness тоже является изменением модели

При каждом существенном изменении слоя нужно пересматривать:

- `completion_status`
- `known_gaps`
- `rationale`
- expected layer coverage

Нельзя дополнять слой и оставлять устаревший статус только потому, что поле уже было заполнено.

### 2.11. Изменения модели должны учитывать feedback loops

Если изменение вызвано:

- инцидентом
- customer feedback
- metric degradation
- audit finding
- test failure
- retrieval failure

то это должно быть видно как closed-loop learning path, а не как isolated edit.

## 3. Правила анализа метамодели

### 3.1. Анализ всегда начинается с полноты модели

Перед любым содержательным выводом нужно проверить:

- есть ли `aismm.registry.json`
- каков `completeness_status`
- какие sources доступны
- какие layers отсутствуют
- какие layers declared empty
- какие sources restricted

Без этого нельзя честно утверждать, что анализируется полная модель.

### 3.2. Анализ должен различать missing, empty и partial

- `missing` означает отсутствие слоя без декларации
- `empty` означает ожидаемый, но пока пустой слой
- `partial` означает неполную модель со значимыми пробелами

При анализе нельзя смешивать эти три состояния.

### 3.3. Анализ должен учитывать source-of-truth hierarchy

Если два слоя говорят разное о том же концепте, нужно применять правило:

- source-of-truth wins
- projection must be corrected

Типовые случаи:

- ownership: `b11` выигрывает у `b9`
- architecture fact: `b2` выигрывает у projection elsewhere
- requirement fact: `b4` выигрывает у planning/work item representation
- economics target vs actual: `b0` и `b12` анализируются как разные роли, а не как дубликаты

### 3.4. Анализ должен разделять current state и proposal state

В ходе анализа нужно отдельно помечать:

- что подтверждено кодом или runtime-фактами
- что пришло из product-source как intent
- что является proposal
- что является draft

Нельзя делать вывод о текущей архитектуре, опираясь только на proposal artifact.

### 3.5. Анализ должен проверять provenance и confidence

Для каждого важного вывода нужно проверить:

- какой source его подтверждает
- насколько source надежен
- не является ли источник derivative, imported_untrusted или generated_unverified
- не конфликтуют ли несколько источников

Если provenance слабый, вывод должен быть помечен как hypothesis, intent или unresolved conflict.

### 3.6. Анализ должен проверять traceability

Минимальные вопросы анализа:

- у requirement есть связи с behavior/domain rule?
- у critical requirement есть tests or validation checks?
- у implementation есть связь с requirement?
- у release есть связь с work items?
- у risky changes есть mitigation or approval?

Если связей нет, это не просто пробел документации, а дефект semantic model.

### 3.7. Анализ должен проверять ID и ссылочную целостность

Минимальные проверки:

- нет ли duplicate global IDs
- все ли cross-layer references резолвятся
- соответствует ли ID формату `b{N}.{layer_id}.{entity_type}.{slug}`
- не усечены ли layer IDs для `b10+`

### 3.8. Анализ должен учитывать temporal validity

Если вопрос относится к историческому состоянию модели, нужно анализировать:

- `valid_from`
- `valid_to`
- `effective_from_release`
- `effective_until_release`
- `superseded_by`
- `temporal_scope`

Git history и semantic validity не одно и то же.

### 3.9. Анализ изменений должен опираться на semantic diff, а не только на text diff

При review изменений нужно отвечать на вопросы:

- какие entities добавлены или удалены
- не исчезли ли high-criticality trace links
- не упал ли confidence
- не изменился ли provenance
- не появился ли schema breaking change

### 3.10. Анализ должен учитывать context-security ограничения

При анализе AI-ready контекста нужно смотреть:

- какой у блоков authorship class
- попал ли untrusted content в authoritative path
- не выходит ли restricted content в неразрешенные context packages
- применяется ли redaction policy

### 3.11. Анализ должен быть repeatable

Эталонная модель предполагает повторяемые consistency checks, а не одноразовую экспертную оценку.

Минимальный повторяемый набор:

- bundle structure checks
- layer file checks
- registry and completeness checks
- product identity checks
- layer ID format checks
- cross-layer reference checks
- metadata block checks

### 3.12. Анализ должен фиксировать не только противоречия, но и тип противоречия

Полезно различать:

- source-of-truth conflict
- projection drift
- missing traceability
- proposal leakage into baseline
- weak provenance
- incomplete layer
- temporal ambiguity
- schema inconsistency
- security/classification violation

## 4. Практический регламент работы

### 4.1. Как правильно заполнять новый материал

1. Определить тип материала: fact, intent, proposal, draft, question.
2. Определить source-of-truth layer.
3. Проверить, есть ли уже соответствующий block/document.
4. Добавить или обновить metadata.
5. Прописать provenance и confidence.
6. Добавить IDs и cross-layer references.
7. Обновить `completion_status` и `known_gaps`.
8. При необходимости обновить `b9.903`, `b9.902`, registry и empty-layer declarations.

### 4.2. Как правильно менять существующий материал

1. Определить semantic change type.
2. Проверить, меняется ли source-of-truth или только projection.
3. Сохранить ID, если это возможно.
4. Если ID меняется, сохранить aliases and migration path.
5. Обновить temporal fields, если изменение действует не сразу.
6. Пересмотреть traceability, tests, releases, risks.
7. Проверить semantic diff и review gates.

### 4.3. Как правильно анализировать состояние модели

1. Начать с registry и completeness.
2. Зафиксировать scope анализа.
3. Разделить observed, intent, proposal.
4. Проверить source-of-truth conflicts.
5. Проверить traceability and coverage.
6. Проверить provenance and confidence.
7. Проверить consistency rules и strict-mode нарушения.
8. Сформулировать findings как defects of model, а не как общие впечатления.

## 5. Короткий вывод

Если упростить все правила до операционного минимума, то AISMM требует следующего:

- заполнять только через source-of-truth логику
- отделять факт от намерения и proposal
- хранить идентичность через stable IDs
- всегда держать provenance и completeness под контролем
- менять модель семантически, а не просто текстово
- анализировать модель через consistency, traceability, provenance и temporal validity

Именно это превращает метамодель из набора заметок в управляемую knowledge system.