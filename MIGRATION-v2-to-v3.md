# Migration Guide — AISMM v2 → v3

```yaml
aismm_meta:
  document_type: migration_guide
  document_id: aismm.migration.v2-to-v3
  spec_version: 3.0.0
  status: accepted
  from_version: 2.0.0
  to_version: 3.0.0
  created: "2026-05-23"
```

Машиночитаемая карта миграции: [`migrations/migration.aismm_v2_to_v3.json`](./migrations/migration.aismm_v2_to_v3.json).
Полный список изменений: [`CHANGELOG.md`](./CHANGELOG.md).

> **v2 для истории:** полный снимок версии 2 сохранён в ветке [`v2.0.0`](https://github.com/orkestron-ai/software-meta-model/tree/v2.0.0). `main` после мержа этого PR содержит v3.

---

## 1. Что меняется в v3 (кратко)

v3 — **MAJOR**. Он не добавляет новые бандлы, а делает модель самоуправляемой в части того, **как её наполняют и кому доверяют**:

1. Ось классификации записи `kind_class` (`normative` / `descriptive` / `projection`) + продуктовый нормативный дом `00-policies/`.
2. Ретроспектива разделена на оценку **результата** (`b1.104`) и оценку **процесса** (`b8.810`); добавлен feedback loop 7.
3. Общий baseline стандартов + контракт наследования (`inherits_from`/`override`).
4. Слой предметных областей `b0.007` (проблемное пространство), отделён от решенческих `b2.207`/`b4.402`/`b9.906`.
5. Наполнение агентами через MCP: recipe-промпт (normative, в `00-policies`) + связки и прогоны в `b9.907`/`b9.903`.
6. Обязательный owner-validation gate для производных записей (`derivation` + `validation_status`).

Новые слои: `b0.007`, `b1.104`, `b8.810`, `b9.907` (82 → 86).
Новые governance-доки: `aismm-content-classification.md`, `aismm-standards-and-inheritance.md`, `aismm-ingestion-and-source-binding.md`, `aismm-owner-validation-and-attestation.md`.

---

## 2. Ломающие изменения

| Изменение | Эффект | Действие |
|---|---|---|
| Поле `kind_class` становится ожидаемым | strict mode может требовать его | проставить по умолчанию (см. шаг 1) |
| Owner-validation gate | производная запись не может стать `stable` и попасть в авторитетный контекст без валидации владельцем источника | добавить `derivation` + `validation_status` |
| Сквозное нормативное → `00-policies` | политики/методики уезжают из `aismm/` data-слоёв | перенести (см. шаг 2) |

Для уровней соответствия L1/L2 эти требования можно отложить; для L4/L5 — обязательны.

---

## 3. Пошаговая миграция продуктовой модели

**Шаг 1. `kind_class`.** Проставьте поле во всех записях. Значения по умолчанию по типу записи:
`policy/rule/proc/standard/method/recipe → normative`; `trace/link/edge/index/coverage → projection`; всё остальное → `descriptive`.

**Шаг 2. `00-policies/`.** Создайте каталог рядом с `00-meta/` и `aismm/`. Перенесите туда сквозные правила/методики/политики/recipe-промпты из data-слоёв `aismm/`. Доменно-локальные политики оставьте в их бандле, но пометьте `kind_class: normative` и сгруппируйте в подпапке `_normative/`.

**Шаг 3. Новые слои.** Заведите директории `aismm/b0-.../007-...`, `aismm/b1-.../104-...`, `aismm/b8-.../810-...`, `aismm/b9-.../907-...`. Где ещё не наполнено — используйте explicit empty-layer блок (`completion_status: empty`).

**Шаг 4. Деривация и валидация.** Каждой записи, созданной агентом из источника, добавьте блок `derivation` (`derived_from`, `produced_by`, `derivation_type`) и `validation_status: unvalidated`. Маршрутизируйте валидацию на `owner_of_source` через связки `b9.907` + владение `b11.1102`.

**Шаг 5. Ingestion bindings.** Для каждого слоя, который наполняется агентом, создайте binding в `b9.907`, ссылающийся на версионированный recipe в `00-policies`.

**Шаг 6. Стандарты.** Подключите общий baseline через `inherits_from: standard:<id>@<version>`; фиксируйте только отклонения как `override` с `reason`.

**Шаг 7. Таксономия связей.** Расширьте `b9.906`: `automates`, `realizes_domain`, `registered_as`, `evaluates`, `assesses_outcome_of`, `derived_from`, `ingested_from`, `attested_by`, `validates`, `disputes`, `inherits_from`, `paired_with`.

**Шаг 8. Гейты.** Обновите strict mode и context-security: запретить `unvalidated`/`owner_disputed`/`expired` производным записям попадать в авторитетные context packages и в `status: stable`.

---

## 4. Открытые решения (развилки)

| # | Развилка | Рекомендация v3 |
|---|---|---|
| D1 | дом нормативного контента: только `00-policies` ↔ гибрид | гибрид |
| D2 | стандарты: landscape/AIPLMM-репозиторий ↔ локальное зеркало | landscape |
| D3 | сила owner-validation gate: жёсткий на L5 ↔ рекомендательный ниже | жёсткий на L5 |

---

## 5. Чеклист

- [ ] `kind_class` проставлен (шаг 1)
- [ ] `00-policies/` создан, сквозные политики перенесены (шаг 2)
- [ ] 4 новых слоя заведены или объявлены empty (шаг 3)
- [ ] производные записи имеют `derivation` + `validation_status` (шаг 4)
- [ ] ingestion bindings созданы (шаг 5)
- [ ] baseline-стандарты подключены через наследование (шаг 6)
- [ ] таксономия связей `b9.906` расширена (шаг 7)
- [ ] strict mode / context-security обновлены под гейт (шаг 8)
- [ ] версия в README/манифесте поднята до `3.0.0`
- [ ] ветка `v2.0.0` сохранена для истории
