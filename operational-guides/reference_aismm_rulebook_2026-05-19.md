# Свод правил эталонного AISMM

## Назначение

Этот документ собирает в одном месте нормативные правила эталонной модели AISMM из репозитория `c:\MetaModel\Git\software-meta-model-main`.

Документ не заменяет исходные governance-спеки эталона. Он нужен как единый русскоязычный конспект правил, ограничений и требований к модели.

## Нормативные источники эталона

- `aismm-structure.md`
- `aismm-unified-id-strategy.md`
- `aismm-model-registry.md`
- `aismm-empty-layer-standard.md`
- `aismm-strict-mode.md`
- `aismm-consistency-rules.md`
- `aismm-consistency-checks.md`
- `aismm-versioning-and-conformance.md`
- `aismm-physical-identity-binding.md`
- `aismm-context-security.md`
- `aismm-security.md`
- `aismm-layer-inventory.md`

## 1. Базовые структурные правила AISMM

### 1.1. Модель block-based, а не file-based

- AISMM определяется блоками, а не файлами.
- Любой файл может быть контейнером AISMM-блоков или AISMM-совместимых представлений.
- Путь файла не является источником идентичности сущности.

### 1.2. Канонические маркеры блока

Каждый AISMM-блок должен использовать маркеры:

- `AISMM:BEGIN`
- `AISMM:META_END`
- `AISMM:END`

### 1.3. Обязательные метаданные блока

Блок должен иметь как минимум:

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

### 1.4. Рекомендуемый формат метаданных

- Секция metadata должна быть YAML-compatible.
- Метаданные должны парситься отдельно от человекочитаемого тела.

### 1.5. Допустимые типы блоков

Базовые типы:

- `layer_specification`
- `layer_document`
- `entity_definition`
- `rule_definition`
- `constraint_definition`
- `assumption_definition`
- `schema_reference`
- `knowledge_unit`
- `trace_link`
- `prompt`

Специализированные типы допустимы, например:

- `value_model`
- `economics_model`
- `event_definition`
- `control_definition`
- `api_contract`
- `data_contract`
- `decision_record`
- `risk_record`
- `test_case`
- `release_record`
- `retention_policy`
- `retrieval_unit`

### 1.6. Правила layer_id

- Для `b0–b9` layer ID должны быть 3-значными.
- Для `b10+` layer ID должны быть 4-значными или длиннее.
- Рекомендуемый regex: `^[0-9]{3,}$`.
- Нельзя усекать `1001`, `1104`, `1206` до трех цифр.

### 1.7. Правила bundle и module scope

- Канонический диапазон bundle: `b0–b12`.
- `module_scope` должен быть либо `root`, либо `module:<module_id>`.
- Корневой AISMM описывает продукт целиком.
- Модульный AISMM описывает ограниченную часть продукта и может специализировать root-level контекст.

### 1.8. Правила document_id

- Каждый блок должен иметь стабильный `document_id`.
- `document_id` должен быть уникален в рамках репозитория или продуктовой модели.
- `document_id` не должен зависеть от расположения файла.

## 2. Правила идентичности и ID-стратегии

### 2.1. Главный принцип

- Идентичность не должна зависеть от местоположения файла.

### 2.2. Канонический глобальный формат ID

Рекомендуемый формат:

`b{bundle}.{layer}.{entity_type}.{slug}`

Примеры:

- `b4.401.requirement.user_login`
- `b2.201.component.auth_service`
- `b8.801.work_item.fix_login_timeout`
- `b9.902.trace_link.requirement_to_component`

### 2.3. Правила slug

- только lowercase
- только latin letters, digits, underscores
- должен начинаться с буквы
- должен быть коротким, но осмысленным
- не должен включать статус, дату или версию, если это не часть идентичности

### 2.4. Local ID против Global ID

- Local ID допустим для удобства авторинга.
- Global ID обязателен для traceability и cross-layer ссылок.

### 2.5. Правила ссылок

- Любое поле, которое ссылается на сущность из другого слоя, должно использовать global ID.

### 2.6. Правила имен entity_type

- `entity_type` должен быть в единственном числе.
- Запрещены множественные формы типа `requirements`, `components`, `work_items`.

### 2.7. Правила стабильности ID

ID не должен меняться из-за:

- изменения title
- изменения description
- переноса файла
- смены формата представления
- детализации сущности
- смены статуса сущности

ID может меняться только если:

- сущность разделена на несколько новых
- смысл сущности существенно изменился
- исходная сущность была неверно идентифицирована

### 2.8. Алиасы и миграция ID

При переименовании или переносе рекомендуется сохранять:

- `aliases`
- `previous_ids`
- `replaced_by`
- `supersedes`

### 2.9. Версию нельзя без нужды зашивать в ID

- Версию стоит хранить отдельным полем.
- В ID версия допустима только если версия сама является частью идентичности сущности.

### 2.10. Product-local identity

- Глобальные AISMM ID являются product-local по умолчанию.
- Одинаковый `b4.401.requirement.user_login` может существовать в разных продуктах как разные сущности.
- Контекст идентичности задается через пару `product_id + model_instance_id`.

### 2.11. Cross-product ссылки

Для ссылок на чужой продукт или модель используются qualified formats:

- `product:<product_id>/...`
- `model:<model_instance_id>/...`
- `repo:<repo_id>/...`

## 3. Правила registry-first discovery

### 3.1. Канонический registry file

- Канонический registry-файл: `aismm.registry.json`.
- Он должен лежать в корне primary repository.
- Если registry существует, парсер обязан начинать с него.

### 3.2. Обязательные поля registry

Registry должен включать:

- `model_instance_id`
- `product_id`
- `aismm_version`
- `registry_version`
- `primary_repository`
- `sources`
- `completeness_status`

Рекомендуются также:

- `product_key`
- `expected_bundles`
- `expected_layers`
- `declared_empty_layers`

### 3.3. Канонические типы source

- `local_path`
- `git_repository`
- `git_submodule`
- `external_url`
- `artifact_registry`
- `restricted_repository`
- `encrypted_source`
- `module_repository`

### 3.4. Канонические статусы source

- `available`
- `unavailable`
- `restricted`
- `encrypted`
- `optional`
- `deprecated`
- `missing`
- `unknown`

### 3.5. Канонические статусы completeness

- `complete`
- `partial`
- `partial_authorized`
- `unknown`
- `invalid`

### 3.6. Правила полноты модели

- Если registry есть, discovery должен идти от него.
- Если registry нет, completeness должен быть `unknown`.
- Если required source недоступен, статус должен быть `partial` или `invalid`.
- Если source restricted, но известен, допустим `partial_authorized`.
- Парсер обязан сообщать, какие bundles/layers загружены, а какие нет.
- AI context packages обязаны включать `model_completeness_status`.
- Strict mode должен падать, если required layers отсутствуют и не объявлены как empty.

### 3.7. Expected layer coverage

Каждый expected layer должен быть закрыт одним из трех способов:

1. есть хотя бы один реальный AISMM-блок слоя
2. есть explicit empty layer block
3. слой перечислен в `declared_empty_layers`

## 4. Правила empty layer и completion status

### 4.1. Missing vs Empty

- `missing layer` означает, что неизвестно, существует ли слой, и нет ни файла, ни декларации.
- `empty layer` означает, что слой ожидается, но еще не наполнен сущностями.

### 4.2. Канонические completion_status

- `not_started`
- `empty`
- `partial`
- `complete`
- `not_applicable`
- `deferred`

### 4.3. Обязательные поля empty layer block

Empty layer block должен содержать:

- `type: layer_document`
- `model_instance_id`
- `product_id`
- `layer_id`
- `layer_key`
- `document_id`
- `document_type: layer_document`
- `module_scope`
- `status`
- `spec_version`
- `completion_status: empty`

### 4.4. Presence rule

Каждый expected layer в registry должен быть закрыт либо real block, либо empty block, либо `declared_empty_layers`.

### 4.5. `not_applicable` rule

- Слой со статусом `not_applicable` обязан содержать `rationale`.

### 4.6. `partial` rule

- Слой со статусом `partial` обязан содержать хотя бы один `known_gap`.

### 4.7. Strict-mode поведение для completion

- required layer missing without empty declaration → `error`
- required layer empty with valid empty block → `warning`
- `not_applicable` without rationale → `warning`
- `partial` without known gaps → `warning`
- `complete` without satisfying completeness criteria → `error`

## 5. Правила strict mode

### 5.1. Уровни валидации

- `advisory`
- `recommended`
- `strict`

### 5.2. Identity rules

- `RULE-ID-001`: каждый global ID должен быть уникален в репозитории
- `RULE-ID-002`: каждый entity ID должен соответствовать формату `b{N}.{layer_id}.{entity_type}.{slug}` и правилу `^[0-9]{3,}$` для layer_id
- `RULE-ID-003`: ни одна сущность не должна ссылаться на несуществующий ID

### 5.3. Product and model instance identity rules

- `RULE-PID-001`: каждый AISMM-блок обязан объявлять `product_id`
- `RULE-PID-002`: каждый AISMM-блок обязан объявлять `model_instance_id`
- `RULE-PID-003`: `product_id` должен быть валидным UUID
- `RULE-PID-004`: `model_instance_id` должен быть валидным UUID
- `RULE-PID-005`: блоки с разными `product_id` нельзя мержить в одну модель без явного registry mapping
- `RULE-PID-006`: блоки с разными `model_instance_id` нельзя мержить без разрешения registry

### 5.4. Registry and completeness rules

- `RULE-REG-001`: если есть `aismm.registry.json`, его надо использовать как discovery root
- `RULE-REG-002`: если registry отсутствует, `completeness_status` должен быть `unknown`
- `RULE-REG-003`: если expected required source недоступен, `completeness_status` должен быть `partial` или `invalid`
- `RULE-REG-004`: AI context packages обязаны содержать `model_completeness_status`
- `RULE-REG-005`: парсер обязан сообщать, какие bundles/layers загружены и какие отсутствуют

### 5.5. Empty and missing layer rules

- `RULE-LAYER-001`: required layer без real block и без empty declaration считается missing и дает validation error
- `RULE-LAYER-002`: required layer с valid empty block считается empty и дает только warning
- `RULE-LAYER-003`: `not_applicable` обязан содержать rationale
- `RULE-LAYER-004`: `partial` обязан содержать `known_gaps`
- `RULE-LAYER-005`: `complete` обязан удовлетворять completeness criteria

### 5.6. Cross-layer reference rules

- `RULE-REF-001`: каждая cross-layer reference должна резолвиться в существующую сущность
- `RULE-REF-002`: каждый trace link обязан объявлять `relationship_type`
- `RULE-REF-003`: каждый high-criticality trace link обязан объявлять `confidence` и `provenance`

### 5.7. Requirement traceability rules

- `RULE-REQ-001`: каждое requirement должно быть связано хотя бы с одним behavior или domain rule
- `RULE-REQ-002`: каждое critical requirement должно быть связано хотя бы с одним test или validation check
- `RULE-REQ-003`: изменение requirement должно запускать переоценку связанных тестов

### 5.8. Release and work item rules

- `RULE-REL-001`: каждый release обязан ссылаться на включенные work items
- `RULE-REL-002`: каждый work item должен ссылаться на value source, requirement или risk
- `RULE-REL-003`: каждое high-risk change должно ссылаться на mitigation или approval record

### 5.9. Knowledge and context rules

- `RULE-CTX-001`: каждый context package должен ссылаться на source retrieval units
- `RULE-CTX-002`: restricted content не должен попадать в unauthorized context packages
- `RULE-CTX-003`: блоки с `authorship_class: imported_untrusted` не должны считаться authoritative

### 5.10. Schema rules

- `RULE-SCH-001`: каждый block обязан включать валидный `aismm_meta` header
- `RULE-SCH-002`: каждый block обязан объявлять `spec_version`
- `RULE-SCH-003`: extension fields должны следовать namespace pattern `x-<org>.<domain>.<entity>`

### 5.11. Правила CI и кастомизации

- В `strict` режиме `critical` и `error` блокируют CI и merge.
- В `recommended` режиме `critical` блокируют CI, остальные нарушения логируются.
- Локальные overrides допустимы, но нельзя ослаблять security-critical правила `RULE-CTX-002` и `RULE-CTX-003`.

## 6. Правила source-of-truth и согласованности bundle

### 6.1. У каждого концепта один source of truth

Каноническое распределение владения:

- `b0` владеет product value и economics
- `b1` владеет strategy и hypotheses
- `b2` владеет system structure и architecture
- `b3` владеет physical implementation и artifacts
- `b4` владеет product behavior и requirements
- `b5` владеет UX и user interaction
- `b6` владеет runtime facts
- `b7` владеет quality, risk, compliance policy
- `b8` владеет change execution и lifecycle orchestration
- `b9` владеет только traceability/index/projection, но не фактами
- `b11` владеет ownership
- `b12` владеет technical cost model

### 6.2. Граница b2 / b3 / b10

- `b2` определяет, что такое data model
- `b3` определяет, как она реализована и задеплоена
- `b10` определяет эволюцию, data lifecycle, observability, AI/ML governance

### 6.3. Граница b0 / b12

- `b0` владеет product-level economics
- `b12` владеет technical economics

### 6.4. Граница b11 / b9

- `b11` является source of truth для ownership
- `b9` может содержать ownership only as traceability projection
- если `b11` и `b9` расходятся, выигрывает `b11`

### 6.5. Правила проекции для b9

- `b9` не владеет business или system facts
- `b9` может только индексировать, линковать, скорить и проектировать факты из source bundles
- при перестроении `b9` обязан брать данные из source bundles, а не изобретать новые факты

### 6.6. Допустимое дублирование

Разрешено:

- summary projections
- reference duplication by ID
- relationship records в `b9.902`

Запрещено:

- независимо определять одну и ту же сущность в двух bundles
- независимо поддерживать одну и ту же сущность в двух bundles
- независимо определять ownership в `b11` и `b9`

### 6.7. Reference-only duplication

Если bundle хочет упомянуть факт из другого bundle, он должен ссылаться на него по ID, а не копировать атрибуты.

### 6.8. Правило разрешения конфликтов

- Если projection расходится с source, выигрывает source.
- Projection должен быть regenerated или corrected.

### 6.9. Lifecycle orchestration rule

- `b8` является lifecycle orchestrator.
- `b6` владеет runtime facts и incidents.
- `b7` владеет quality/risk/compliance policy.
- `b9` владеет traceability projection.

## 7. Правила версионирования и conformance

### 7.1. AISMM versioning

- AISMM использует semantic versioning: `MAJOR.MINOR.PATCH`.
- Current reference version: `2.0.0`.

### 7.2. Правила изменения версии

- `MAJOR`: breaking schema change, new mandatory fields, rename/remove bundles/layers
- `MINOR`: new optional fields/layers/bundles without breaking changes
- `PATCH`: wording fixes, examples, clarifications, docs improvements

### 7.3. Версия должна быть задекларирована

Репозиторий AISMM должен объявлять:

- `aismm_version`

### 7.4. Правила совместимости

- Consumers для `2.x.x` должны принимать все `2.MINOR.x` документы.
- Breaking changes допустимы только на `MAJOR`.
- Tooling должно игнорировать unknown optional fields.

### 7.5. Канонические layer lifecycle status

- `draft`
- `proposed`
- `accepted`
- `stable`
- `deprecated`
- `superseded`
- `archived`
- `withdrawn`

### 7.6. Разрешенные переходы статусов

- `draft -> proposed | withdrawn`
- `proposed -> accepted | draft | withdrawn`
- `accepted -> stable | deprecated | withdrawn`
- `stable -> deprecated`
- `deprecated -> superseded | archived`
- `superseded -> archived`

Любые другие переходы требуют explicit governance approval.

### 7.7. Migration policy

Значимые изменения должны иметь migration record в `migrations/`.

Поддерживаемые change types:

- `layer_renamed`
- `layer_split`
- `layer_merged`
- `layer_extension`
- `field_renamed`
- `entity_type_moved`
- `bundle_added`
- `bundle_deprecated`
- `schema_breaking`
- `schema_additive`

### 7.8. Extension namespace

Extension field должен следовать формату:

`x-<organization>.<domain>.<entity>`

Правила:

- extension fields начинаются с `x-`
- organization slug lowercase, alphanumeric, hyphen allowed
- extension field всегда optional
- AISMM tooling должно игнорировать unknown extension fields

### 7.9. Conformance levels

#### L1 — Block Format

Минимум:

- используются AISMM-compatible blocks
- есть `aismm_meta` header хотя бы с `document_type`, `spec_version`
- нет naming convention violations

#### L2 — Typed Entities

Минимум:

- выполнен L1
- значимые entities имеют stable `id`
- entity types явно объявлены
- layer assignment явно указан

#### L3 — Traceability Graph

Минимум:

- выполнен L2
- cross-layer links используют typed relationships
- bundle `b9` присутствует и заполнен
- requirements хотя бы минимально трассируются к implementation, implementation к tests

#### L4 — RAG-Ready Context

Минимум:

- выполнен L3
- у каждого блока есть `summary`
- блоки bounded to RAG-friendly chunk size, рекомендуется не больше 1500 tokens
- заполнен `b9.905`
- есть retrieval metadata вроде `context_tags` или `retrieval_hints`

#### L5 — Agent-Grade Governance

Минимум:

- выполнен L4
- каждый mutable block имеет `last_modified_by`
- change history ведется в `b8`
- объявлены agent-visible и agent-writable scopes
- declared source provenance в `b9.903`
- нет orphaned entities

## 8. Правила physical identity binding

### 8.1. Назначение binding

- Logical AISMM IDs должны быть связаны с реальными физическими артефактами.
- Это нужно, чтобы traceability переживал переносы файлов, refactoring и infra changes.

### 8.2. Канонические binding types

- `identity_binding`
- `artifact_binding`
- `symbol_binding`
- `commit_binding`
- `schema_binding`
- `runtime_binding`
- `digest_binding`

### 8.3. Обязательные поля binding record

- `id`
- `logical_entity`
- `binding_type`
- `repository`
- `path`
- `status`

Условно обязательны:

- `symbol` для `symbol_binding`

Опциональны:

- `commit`
- `digest`

### 8.4. Binding status lifecycle

- `active`
- `stale`
- `broken`
- `archived`

## 9. Правила context security

### 9.1. AISMM сам является attack surface

Контекст AISMM должен защищаться от:

- prompt injection
- context poisoning
- unauthorized retrieval
- malicious block edits
- untrusted imported content
- false authority claims

### 9.2. Каждый block должен иметь authorship class

Канонические classes:

- `human_verified`
- `agent_authored`
- `imported_trusted`
- `imported_untrusted`
- `generated_unverified`
- `external_reference`

### 9.3. Security rules

- Все `imported_untrusted` blocks должны быть явно помечены и не считаться authoritative.
- Third-party content не наследует authority AISMM автоматически.
- Restricted blocks не должны попадать в unauthorized context packages.
- High-impact blocks должны поддерживать signing или attestation.
- Перед выдачей контекста должен применяться retrieval redaction.
- Untrusted blocks должны быть fenced от instruction-processing context.

### 9.4. Context access policy

Context access policy должна:

- разрешать trusted authorship classes
- запрещать `imported_untrusted` и `generated_unverified`
- редактировать/редактировать чувствительные поля через redaction policy

### 9.5. Attestation

High-impact blocks могут иметь attestation record с полями:

- `id`
- `block_id`
- `attested_by`
- `attested_at`
- `method`
- `signature`

## 10. Правила repository security

### 10.1. Главный принцип

- Git не поддерживает безопасный per-file access control.
- Поэтому AISMM security должна реализовываться вне core-механики Git.

### 10.2. MUST rules

- Sensitive AISMM data должны храниться в отдельных private repositories.
- Access control должен enforced на repository level.

### 10.3. SHOULD rules

- Использовать module isolation.
- Подключать sensitive AISMM components через Git submodules.
- Классифицировать AISMM blocks.

### 10.4. Encryption rule

- Если separation невозможна, необходимо использовать encryption.

Рекомендуемые инструменты:

- `git-crypt`
- `Mozilla SOPS`
- `age`
- `GPG`

### 10.5. CODEOWNERS не является security boundary

- CODEOWNERS полезен для governance и review enforcement.
- CODEOWNERS не ограничивает read access.

### 10.6. Context aggregator responsibilities

Любой AISMM editor или context aggregator должен:

- поддерживать несколько репозиториев
- мержить AISMM graphs из разных sources
- уважать access boundaries
- fail gracefully если часть данных недоступна

### 10.7. Data classification

Каждый AISMM block рекомендуется классифицировать как:

- `public`
- `internal`
- `confidential`
- `restricted`

## 11. Правила consistency checks и операционные проверки

### 11.1. Bundle structure checks

- у каждого bundle directory должен быть `README.md` или `readme.md`
- у каждого bundle directory желательно должен быть `b{N}.schema.json`
- каждый bundle должен быть перечислен в root `README.md`
- каждый bundle должен быть перечислен в `aismm-structure.md`

### 11.2. Layer file checks

- каждый layer file должен быть перечислен в bundle README
- каждый layer file должен быть представлен в bundle schema

### 11.3. Registry and completeness checks

- если модель distributed, желательно должен существовать `aismm.registry.json`
- `registry.product_id` должен быть UUID
- `registry.model_instance_id` должен быть UUID
- registry должен объявлять `completeness_status`
- каждый declared required source должен быть доступен, restricted или явно обработан
- каждый expected layer должен иметь real block или empty block
- empty layer blocks должны содержать `completion_status` и `known_gaps`

### 11.4. Product identity checks

- каждый AISMM block должен иметь `product_id`
- каждый AISMM block должен иметь `model_instance_id`
- форматы обоих должны быть UUID
- cross-product merge без explicit registry mapping запрещен

### 11.5. Layer ID format checks

- `layer_id` должен соответствовать `^[0-9]{3,}$`
- для `b10+` layer IDs должны быть 4+ digits
- в рамках одного bundle не должно быть collisions по layer ID
- в рамках всего repository не должно быть collisions по global IDs

### 11.6. Cross-layer reference checks

- каждая cross-layer reference должна резолвиться
- каждая ссылка должна использовать global ID format
- trace links в `b9.902` должны содержать `relationship_type`

### 11.7. Metadata block checks

- каждый block должен иметь `aismm_meta`
- каждый block должен иметь `spec_version`
- `status` должен быть одним из canonical lifecycle values

### 11.8. Maintenance rules

После добавления bundle или layer необходимо:

1. прогнать consistency checks
2. обновить bundle README
3. обновить bundle schema
4. регенерировать `aismm-layer-inventory.md`
5. при необходимости обновить root `README.md` и `aismm-structure.md`

## 12. Канонический inventory-правило эталона

- Эталонная модель содержит 13 bundles и 82 layers.
- Layer inventory должен оставаться согласованным с README и schema.
- Для `b10+` layer IDs четырехзначные и не должны усекаться.

## 13. Короткий итог

Эталонный AISMM требует одновременно соблюдать:

- block-based structure
- стабильную ID-стратегию
- registry-first discovery
- явную модель completeness
- строгую validation discipline
- source-of-truth boundaries между bundles
- версионирование и conformance levels
- привязку logical IDs к physical artifacts
- context security и repository security
- постоянные consistency checks и maintenance discipline

Иначе AISMM превращается обратно в набор документов, а не в управляемую продуктовую knowledge model.