# [Bugfix] MenuMediaObject is empty in data release if there is no menu_menus record for the config zone Test Report

## Functional Testing

### 1.Menu Data Release Export File Integrity

#### 1.1.Empty menu_menus parent does not wipe menu_media_objects rows

**Prerequisite(s):**
1. A config zone is prepared where menu_media_objects has multiple parent data sources (e.g., menu_menus, menu_items, menu_attributes).
2. menu_items and menu_attributes have records with media to export, while menu_menus has no records for the config zone.

**Step(s):**
1. Publish menu data for the config zone.
2. Check the generated menu_media_objects.csv export file.
3. Check the data received by the outlet after the data release.

**Reproduction Result(s):**
1. The later empty-parent export rewrites menu_media_objects.csv in write mode with a header only, truncating the media rows already exported from the other parents.
2. The outlet receives an empty menu_media_objects table after the data release.

**Fix Result(s):**
1. menu_media_objects.csv retains all media rows exported from the parents that have data.
2. The empty parent only ensures the CSV header exists and does not wipe rows already written by other parents.
3. The outlet receives a menu_media_objects table containing the exported media rows.

#### 1.2.All parent data sources have records

**Prerequisite(s):**
1. A config zone is prepared where menu_menus, menu_items, and menu_attributes all have records with media to export.

**Step(s):**
1. Publish menu data for the config zone.
2. Check the generated menu_media_objects.csv export file.

**Reproduction Result(s):**
1. Not applicable - this scenario was not affected by the reported issue.

**Fix Result(s):**
1. All media rows from all parent data sources are exported into menu_media_objects.csv.
2. No rows are lost or duplicated.

#### 1.3.All parent data sources have no records

**Prerequisite(s):**
1. A config zone is prepared where menu_menus, menu_items, and menu_attributes all have no records to export.

**Step(s):**
1. Publish menu data for the config zone.
2. Check the generated menu_media_objects.csv export file.

**Reproduction Result(s):**
1. Not applicable - this scenario was not affected by the reported issue.

**Fix Result(s):**
1. A header-only menu_media_objects.csv is produced without errors.
2. The data release completes normally.

#### 1.4.Export file missing creates header-only file from an empty parent

**Prerequisite(s):**
1. The export destination does not contain a menu_media_objects.csv file (the file is missing).
2. A parent data source (e.g., menu_menus) has no records to export.

**Step(s):**
1. Publish menu data for the config zone so that the empty parent is processed first.
2. Check the created menu_media_objects.csv file.

**Reproduction Result(s):**
1. Not applicable - the file-missing scenario was not part of the reported defect behavior.

**Fix Result(s):**
1. The empty parent creates the file with the CSV header only, since the file was missing.
2. Subsequent parents with data append their rows to the file without wiping them.

### 2.Outlet Data Release Result

#### 2.1.Outlet receives non-empty menu_media_objects after data release

**Prerequisite(s):**
1. The config zone scenario from case 1.1 is prepared (menu_menus empty, other parents with media data).
2. The outlet is attached to the config zone and subscribed to the data release.

**Step(s):**
1. Publish menu data for the config zone.
2. On the outlet, wait for the data release to complete and check the menu_media_objects table.

**Reproduction Result(s):**
1. The outlet receives an empty menu_media_objects table after the data release.

**Fix Result(s):**
1. The outlet receives a menu_media_objects table populated with the media rows exported from menu_items and menu_attributes.

#### 2.2.Outlet media data consistency with exported parents

**Prerequisite(s):**
1. The data release of the config zone has completed with the fix applied.

**Step(s):**
1. Compare the menu_media_objects rows on the outlet with the records exported from menu_items and menu_attributes.
2. Verify the media display of the related menu items on the outlet.

**Reproduction Result(s):**
1. Media rows are missing on the outlet and related menu items have no media displayed.

**Fix Result(s):**
1. The media rows on the outlet match the records exported from the parent data sources with no missing or duplicate rows.
2. The related menu items display their media correctly.

### 3.Regression Scope

1. Data release for config zones where all parent data sources have data.
2. Other export tables written by multiple parent data sources.
3. Normal data release with a single parent data source.
4. Outlet data synchronization and menu media display after release.
