# AISMM Consistency Checks

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.consistency-checks
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

This document defines **repeatable checks** for detecting inconsistencies in an AISMM repository.

Run these checks after any bundle or layer additions, after schema updates, or as part of CI validation.

See also:

- [`aismm-layer-inventory.md`](./aismm-layer-inventory.md) — complete layer inventory
- [`aismm-consistency-rules.md`](./aismm-consistency-rules.md) — source-of-truth and projection rules
- [`aismm-strict-mode.md`](./aismm-strict-mode.md) — enforcement rules and CI integration

---

## Check Categories

### 1. Bundle Structure Checks

| Check | Description | Severity |
|-------|-------------|----------|
| `check_bundle_readme` | Every bundle directory must have a `README.md` or `readme.md` | error |
| `check_bundle_schema` | Every bundle directory must have a `b{N}.schema.json` | warning |
| `check_bundle_in_root_readme` | Every bundle must be listed in the root `README.md` | warning |
| `check_bundle_in_structure_doc` | Every bundle must appear in `aismm-structure.md` layer map | warning |

### 2. Layer File Checks

| Check | Description | Severity |
|-------|-------------|----------|
| `check_layer_in_readme` | Every layer file must appear in the bundle README | warning |
| `check_layer_in_schema` | Every layer file must be represented in the bundle schema | warning |

### 3. Registry and Completeness Checks

| Check | Description | Severity |
|-------|-------------|----------|
| `check_registry_exists` | If distributed model, `aismm.registry.json` should exist | warning |
| `check_registry_product_id` | Registry `product_id` must be a valid UUID | error |
| `check_registry_model_instance_id` | Registry `model_instance_id` must be a valid UUID | error |
| `check_registry_completeness_status` | Registry must declare `completeness_status` | warning |
| `check_source_availability` | Every declared required source must be available or declared restricted | error |
| `check_expected_layers_covered` | Every expected layer must have a real block or an empty layer block | error |
| `check_empty_layer_format` | Empty layer blocks must include `completion_status` and `known_gaps` | warning |

### 4. Product Identity Checks

| Check | Description | Severity |
|-------|-------------|----------|
| `check_block_product_id` | Every AISMM block must declare `product_id` | error |
| `check_block_model_instance_id` | Every AISMM block must declare `model_instance_id` | error |
| `check_product_id_format` | `product_id` must be a valid UUID | error |
| `check_model_instance_id_format` | `model_instance_id` must be a valid UUID | error |
| `check_cross_product_merge` | Blocks with different `product_id` must not be merged without explicit registry mapping | critical |

| `check_schema_layer_has_file` | Every schema layer property must have a corresponding layer file | error |
| `check_layer_metadata_block` | Every layer file must contain a valid `<!-- AISMM:BEGIN -->` metadata block | error |
| `check_layer_id_in_metadata` | The `layer_id` in the metadata block must match the filename prefix | error |
| `check_layer_key_in_metadata` | The `layer_key` in the metadata block must match the filename slug | warning |

### 3. Layer ID Format Checks

| Check | Description | Severity |
|-------|-------------|----------|
| `check_layer_id_format` | Layer IDs must match `^[0-9]{3,}$` | error |
| `check_4digit_layer_ids` | For bundles b10+, layer IDs must be 4+ digits — do not truncate to 3 | critical |
| `check_no_layer_id_collision` | No two layers in the same bundle may share the same ID | critical |
| `check_no_global_id_collision` | No two entities across the repository may share the same global ID | critical |

> **Special rule for b10+ bundles:** Layer IDs `1001`–`1009` are **not** the same as `100`. They are distinct 4-digit identifiers. Any parser that reads only 3 characters of the layer ID will produce incorrect results.

### 4. Cross-Layer Reference Checks

| Check | Description | Severity |
|-------|-------------|----------|
| `check_reference_resolution` | Every cross-layer reference must resolve to an existing entity ID | critical |
| `check_reference_format` | All references must use the global ID format: `b{N}.{layer_id}.{type}.{slug}` | error |
| `check_trace_link_has_type` | All trace links in b9.902 must declare a `relationship_type` | error |

### 5. Metadata Block Checks

| Check | Description | Severity |
|-------|-------------|----------|
| `check_aismm_meta_present` | Every block must include an `aismm_meta` header | error |
| `check_spec_version_present` | Every block must declare a `spec_version` | error |
| `check_status_value` | Status must be one of the canonical lifecycle values | warning |

---

## Pseudo-Code for Key Checks

```python
def check_bundle_readmes(root):
    for bundle_dir in list_bundle_dirs(root):
        if not exists(bundle_dir / "README.md") and not exists(bundle_dir / "readme.md"):
            report_error(f"Missing README in {bundle_dir}")

def check_layer_inventory(root):
    for bundle_dir in list_bundle_dirs(root):
        layer_files = list_layer_files(bundle_dir)  # files matching \d{3,}-*.md
        readme_layers = parse_layers_from_readme(bundle_dir / "README.md")
        for layer in layer_files:
            if layer.id not in readme_layers:
                report_warning(f"Layer {layer} not listed in {bundle_dir}/README.md")

def check_schema_layer_alignment(root):
    for bundle_dir in list_bundle_dirs(root):
        schema = load_schema(bundle_dir)
        layer_files = list_layer_files(bundle_dir)
        schema_layer_ids = extract_layer_ids_from_schema(schema)
        file_layer_ids = {f.layer_id for f in layer_files}
        for lid in file_layer_ids - schema_layer_ids:
            report_warning(f"Layer {lid} not in schema for {bundle_dir}")
        for lid in schema_layer_ids - file_layer_ids:
            report_error(f"Schema references layer {lid} but file not found in {bundle_dir}")

def check_4digit_layer_ids(root):
    """Special check: b10+ bundles must use 4-digit layer IDs without truncation."""
    for bundle_dir in list_bundle_dirs(root):
        bundle_id = parse_bundle_id(bundle_dir)  # e.g. 10, 11, 12
        if bundle_id >= 10:
            for layer_file in list_layer_files(bundle_dir):
                if len(layer_file.layer_id) < 4:
                    report_critical(
                        f"Bundle b{bundle_id} layer ID '{layer_file.layer_id}' "
                        f"should be 4 digits. Expected e.g. '1{layer_file.layer_id}'"
                    )

def check_registry_and_completeness(root):
    registry_path = root / "aismm.registry.json"
    if not exists(registry_path):
        report_warning("No aismm.registry.json found — completeness_status is unknown")
        return
    registry = load_json(registry_path)
    if not is_valid_uuid(registry["product_id"]):
        report_error("registry.product_id is not a valid UUID")
    if not is_valid_uuid(registry["model_instance_id"]):
        report_error("registry.model_instance_id is not a valid UUID")
    for source in registry["sources"]:
        if source["status"] == "missing":
            report_error(f"Required source {source['id']} is missing")

def check_expected_layers_covered(root, registry):
    empty_declared = set(registry.get("declared_empty_layers", []))
    for layer in registry.get("expected_layers", []):
        has_real_block = any_real_block_for_layer(root, layer)
        has_empty_block = layer in empty_declared or has_empty_layer_file(root, layer)
        if not has_real_block and not has_empty_block:
            report_error(f"Expected layer {layer} is missing — no real block and not declared empty")
        elif not has_real_block and has_empty_block:
            report_warning(f"Expected layer {layer} exists as empty — not yet populated")

def check_product_identity(root):
    for block in scan_all_blocks(root):
        if not block.has_field("product_id"):
            report_error(f"Block {block.id} missing product_id in {block.file}")
        elif not is_valid_uuid(block.product_id):
            report_error(f"Block {block.id} product_id is not a valid UUID")
        if not block.has_field("model_instance_id"):
            report_error(f"Block {block.id} missing model_instance_id in {block.file}")
    # check for cross-product merge violations
    product_ids = {b.product_id for b in scan_all_blocks(root) if b.has_field("product_id")}
    if len(product_ids) > 1:
        report_critical(f"Multiple product_ids found in repository: {product_ids}. Cross-product merge without registry mapping.")


    seen_ids = {}
    for entity in scan_all_entities(root):
        if entity.id in seen_ids:
            report_critical(
                f"Duplicate global ID: {entity.id} "
                f"(in {entity.file} and {seen_ids[entity.id]})"
            )
        seen_ids[entity.id] = entity.file

def check_reference_resolution(root):
    all_ids = collect_all_entity_ids(root)
    for ref in scan_all_cross_layer_refs(root):
        if ref.target_id not in all_ids:
            report_critical(f"Broken reference: {ref.target_id} in {ref.source_file}")
```

---

## Suggested Command-Line Interface

```bash
# Run all consistency checks
aismm-check --root . --level strict

# Run only structural checks
aismm-check --root . --checks structural

# Run and output JSON report
aismm-check --root . --output aismm.consistency-report.json

# Run as part of CI
aismm-check --root . --fail-on error
```

---

## Integration with Strict Mode

Consistency checks complement [`aismm-strict-mode.md`](./aismm-strict-mode.md):

| Strict Mode Rule | Consistency Check |
|------------------|-------------------|
| `RULE-ID-001` Unique global IDs | `check_global_id_uniqueness` |
| `RULE-ID-002` Valid ID format | `check_layer_id_format`, `check_4digit_layer_ids` |
| `RULE-REF-001` References resolve | `check_reference_resolution` |
| `RULE-SCH-001` Valid metadata block | `check_aismm_meta_present` |

---

## Layer Inventory Maintenance

After any bundle or layer addition:

1. Run `check_layer_inventory` to detect new `no_schema` or `no_readme` issues.
2. Update the bundle README to list the new layer.
3. Update the bundle schema to include the new layer.
4. Regenerate [`aismm-layer-inventory.md`](./aismm-layer-inventory.md).
5. Update root `README.md` and `aismm-structure.md` if a new bundle was added.

---

## Summary

| Check Function | Detects |
|----------------|---------|
| `check_bundle_readmes` | Missing bundle README files |
| `check_layer_inventory` | Layers not listed in README |
| `check_schema_layer_alignment` | Schema/file mismatches |
| `check_4digit_layer_ids` | b10+ layer ID truncation errors |
| `check_global_id_uniqueness` | Duplicate entity IDs |
| `check_reference_resolution` | Broken cross-layer references |
| `check_aismm_meta_present` | Missing metadata blocks |
