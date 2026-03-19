---
name: datatables
description: "Builds and tests DataTables and Editor stacks (Yajra, modular). Activates when creating or editing DataTables, Html builders, Editors, sub-resource tables, testing with DataTable::test(), or when the user mentions dt, DataTable, Yajra, or table CRUD."
license: MIT
metadata:
  author: project
---

# DataTables Architecture

**PURPOSE:** Single authoritative playbook for DataTables + Editor (Yajra, modular). Keep behavior consistent, testable, secure, performant. Copy blocks safe for GPT injection.

## FAST START

```bash
php artisan datatables:make UsersDataTable --model=User --builder
# Also: datatables:editor, datatables:html, datatables:scope, datatables:purge-export
```

**Layout:** `modules/Feature/DataTables/` → `FeatureDataTable.php`, `FeatureDataTableEditor.php`, `FeatureDataTableHtml.php`.

**Objectives:** Separation of concerns (query / HTML / Editor); declarative columns/buttons/editors; eager loading + withCount; **DataTable::test()** only (no manual request arrays); explicit searchable/orderable.

**Class roles:** DataTable = query + row transforms; Editor = CRUD/custom actions, rules, lifecycle (saving/saved/removed); Html = columns(), buttons(), editors(), permission; Controller = thin (render + process).

---

## QUERY AND SELECT

- **No order in query().** Ordering is via Html `->orderBy(columnIndex, 'asc'|'desc')` and user sort; order in query() breaks sorting.
- **With eager load or joins:** `->select($model->getTable().'.*')`; qualify where/whereIn; eager load only needed columns (e.g. `->with(['employee:id,first_name,last_name'])`); qualify main-table column **name** in Html (e.g. `Column::make('email', 'employees.email')`).
- **No eager load/joins:** No select; unqualified where/column names in Html (e.g. `Column::make('email')`).

---

## EDITOR

**Actions:** Set `$actions` (and `$customActions`) to only what the table uses. Full CRUD → default; remove-only → `['remove']`; custom only → `[]` + `$customActions`. Omit unused *Rules/saving.

**Validation, not abort:** Use validation rules + `messages()` for "cannot edit/remove" (e.g. locked). Remove: `DT_RowId` rule (e.g. prohibited) + message; client always sends DT_RowId. Edit: `Rule::prohibitedIf(...)` on a field always in form. Remove validation fails → **400** with **`error`** (not 422). Test remove: include `DT_RowId` in row payload; expect 400. Scope who sees what in DataTable `query()`, not in Editor. Use **`creating()`** for create-only logic (e.g. team_id, status).

**`$model` type in Editor methods:** Do **not** add `/** @var ConcreteModel $model */` casts inside `editRules()`, `removeRules()`, `saving()`, or `saved()`. The `@extends DataTablesEditor<ConcreteModel>` class-level annotation threads the generic through the trait methods (`WithEditAction<TModel>`, `WithRemoveAction<TModel>`) so PHPStan already knows `$model` is `ConcreteModel`. The cast is redundant and should be omitted.

---

## COLUMN AND SEARCH RULES

**Column::make(data, name):** `data` = JSON key for display; `name` = DB column for sort/search. Qualified `table.column` **only when** query has **joins**; with `with()` only → single-table → unqualified. Omit name when same as data. **When the second parameter (name) is empty or omitted, the effective name becomes the first argument (data).** So `Column::make('email')` and `Column::make('email', '')` both use `'email'` as the column name for search/order; `filterColumn` and `orderColumn` must use that same value (e.g. `filterColumn('email', fn ...)`).

**Eager-loaded relation:** `Column::make(relation_snake.column, relationName.column)` (e.g. `salary_component.code`, `salaryComponent.code`). Don’t use computed + filterColumn for that.

**Column types:**
- **ID:** `IDColumn::make('id', 'table.id')` (or `IDColumn::make('id')`); place **last** in columns().
- **Table columns:** `Column::make(data, name)`; searchable/orderable by default unless PII/expensive/JSON.
- **Computed:** `Column::computed(data)`; add `->columnControlSpacer()` when columnControlSearch. Not searchable by default. For columns that are searchable and use the column control dropdown (e.g. enum type), use **Column::make(displayKey, name)** instead so the package handles ordering too; use addColumn to supply the display value (e.g. label) and keep the same **name** for search/order.
- **Enum/cast display:** DataTable `->formatColumn('column', fn)`; Html `Column::formatted(data, name)`.
- **Money (decimal/float):** `MoneyColumn::make()` or `MoneyColumn::computed()`; addColumn returns raw number or null; `->defaultContent('—')` for null. Cents → `DecimalMoneyColumn`.
- **Date/datetime:** `DateColumn::make(...)` / `DateTimeColumn::make(...)`; with column control → `->withColumnControlSearchDate()`.
- **Boolean:** `BooleanColumn::make(data, name)`; column control dropdown → Html `->withColumnControlSearchDropdown()`, DataTable `->with('columnControl', [key => BooleanColumn::columnControlOptions()])` only. **Do not add filterColumn** — the package handles filtering for columns using the dropdown. Boolean columns can be searchable (omit `searchable(false)` and `['search' => false]` when using the dropdown).

**Column control:** Enum → `dataTable(): ->with('columnControl', [name => Options::from($valueToLabel)])`, Html `->withColumnControlSearchDropdown()`. Options: `Options::from([value=>label])`, `Options::yesNo()`, etc. **Do not add a custom filterColumn for any column that uses `withColumnControlSearchDropdown()`** — the package handles filtering (and ordering) automatically. This applies to enums, booleans (Y/N dropdown), and any other column using the dropdown; adding filterColumn is redundant and wrong. For searchable/orderable columns that use the dropdown, use **Column::make()** (not `Column::computed()`): the package can handle both search and ordering when the column has a proper **name** (e.g. `salaryComponent.type` or `salary_components.type`). Use `Column::make(displayKey, nameForSearchOrder)` — set **data** to a key that holds the display value (e.g. from addColumn, like `component_type_label` or `type_label`) and **name** to the relation/table column used for filter and order. For the search to work, use `->name('relationName.column')` for relation columns (e.g. `->name('salaryComponent.type')`) or `->name('table.column')` for direct table columns (e.g. `->name('salary_components.type')`).

**Method chaining:** title, className, width, searchable, orderable, exportable, columnControlSpacer (non-searchable only when columnControlSearch).

**withCount / withSum (relation count and sum):**
- **Best practice:** Load counts and sums in **query()** with `->withCount('relation')` and `->withSum('relation', 'column')` so each row gets `relation_count` and `relation_sum_column` (e.g. `lines_count`, `lines_sum_amount`). Use these attributes in the column definition and in exportRender. **Never** use `$row->relation()->count()` or `$row->relation()->sum('column')` in addColumn or exportRender — that causes N+1.
- **Display:** For count, use `Column::make('relation_count', 'relation_count')` so the value comes from the model and the column has a **name** for server-side ordering. For sum, use either `Column::make` / `MoneyColumn::make` with the withSum attribute name if the display key matches, or `MoneyColumn::computed('display_key', 'Title')->name('relation_sum_column')` so ordering uses the aggregated column.
- **Orderable, not searchable:** withCount/withSum columns are **orderable** (server can sort by the subselect) but must be **not searchable**. Set `->orderable(true)->searchable(false)` and keep `->columnControlSpacer()` when column control is enabled.
- **DataTable:** No addColumn needed for a plain count column when using `Column::make('relation_count', 'relation_count')`; the model attribute is included in the response. Keep addColumn only when the display key differs from the attribute (e.g. `total_amount` from `lines_sum_amount`) or you need formatting.

**Anti-patterns:** No `Column::make('id')` (use IDColumn, last). No Column::make for date/datetime/money (use DateColumn/DateTimeColumn/MoneyColumn). No number_format() from addColumn for money. No columnControlSpacer on searchable columns. No searchable on *_count. **No** `->relation()->count()` or `->relation()->sum()` in addColumn/exportRender — use withCount/withSum in query and the loaded attribute. Use App\Editor\Columns\* and App\Editor\Fields\*.

---

## ROW TRANSFORMS AND EXPORT (addColumn, editColumn, formatColumn, exportRender, exportFormat)

**Variable naming:** With **EloquentDataTable**, the row is the **query model**. Use a variable that matches the model (e.g. `$user` for `User`, `$role` for `Role`) in addColumn, editColumn, formatColumn, and exportRender. In **formatColumn**, type the first parameter (`$value`) to match the model’s cast/column type for that column (enum, bool, string, float|int, array), not `mixed`.

### addColumn (DataTable class)

- **Role:** Adds a **computed** column (not a DB column) to each row.
- **When:** While building the DataTable response (table + JSON).
- **Use for:** Relations, links, concatenations, or any value not stored as one column.

```php
->addColumn('profile_link', function (User $user): string {
    return '<a href="'.e(route('users.show', $user)).'">'.e($user->name).'</a>';
})
```

### editColumn (DataTable class)

- **Role:** Changes how an **existing** column is displayed in the HTML table.
- **When:** While building the DataTable response.
- **Use for:** Wrapping in HTML, formatting dates, enum labels, etc., without adding a new key.

```php
->editColumn('email', function (User $user): string {
    return $value ? '<a href="mailto:'.e($user->email).'">'.e($user->email).'</a>' : '—';
})
```

### formatColumn (DataTable class)

- **Role:** Format a column for **display only**; sort/search use the raw value. Use with **Column::formatted(data, name)** in the Html builder.
- **When:** While building the table output.
- **Use for:** Enum/cast display (e.g. status → label) or "same data, different display".
- **First parameter (`$value`):** Type it to match the **model’s data type or cast** for that column (e.g. enum class for enum casts, `bool` for boolean, `string` for string columns, `float|int` for numeric). Do not use `mixed`; use the actual type so the callback contract is clear.
- **Second parameter (row):** When present, use the query model with a matching variable name (e.g. `$user`, `$attendance`).

```php
// Enum column → $value is the enum (same as model cast)
->formatColumn('status', fn (UserStatus $value, User $user): string => $user->status->label());
->formatColumn('request_type', fn (AttendanceRequestType $value, AttendanceRequest $request): string => $request->request_type->label());

// Boolean column → $value is bool
->formatColumn('locked', fn (bool $value): string => $value ? 'Yes' : 'No');
->formatColumn('late_entry', fn (bool $value): string => $value ? 'Y' : 'N');

// String column
->formatColumn('name', fn (string $value, WithholdingTaxSchedule $schedule): string => ...);

// Numeric (float/int) or array when that’s the model type/cast
->formatColumn('leaves_remaining', fn (float|int $value): string => number_format((float) $value, 1));
->formatColumn('approver_employee_ids', fn (array $value, Department $department): string => ...);

// Html: Column::formatted('status', 'users.status')->title('Status')
```

### exportRender (Html / Column definition)

**You must use exportRender when either:**

1. **`Column::make('data_key', 'table.column')`** and the **data** value was supplied via **addColumn** (i.e. data and name are not the same source). For example: the column displays `employee_name` from `addColumn('employee_name', fn () => ...)` (e.g. a link), but the **name** is `employee.last_name` for search/order. Export has no built-in way to derive the display value, so exportRender must return the plain export value (e.g. employee name as text).
2. **`Column::computed('<data>')`** was used. The column has no DB column; the value comes only from addColumn or a custom attribute. Export needs exportRender to output the correct value.

**Do not use** for standard table columns where the value is a plain DB field (no addColumn, no HTML, no custom logic); default export is enough.

- **Variable:** Same query model with a matching name (e.g. `$user`, `$role`). Second parameter is the default value (e.g. `$value`).

```php
// Column::make with data from addColumn (e.g. HTML link) → use exportRender for plain export
Column::make('employee_name', 'employees.last_name')->title('Employee')
    ->exportRender(fn (Loan $loan, mixed $value): string => $loan->employee?->name ?? '—'),

// Column::computed → use exportRender
Column::computed('loan_type_name')->title('Loan Type')
    ->exportRender(fn (Loan $loan, mixed $value): string => $loan->loanType->name ?? '—'),

// Standard table column (no addColumn, no HTML) → no exportRender
Column::make('name', 'users.name')->title('Name'),
```

### exportFormat (Html / Column definition)

- **Role:** Tells Excel (and similar) how to format the cell (number, date, currency).
- **Use when:** You need a **date**, **money**, or other **Excel-style format** and the column type does not already provide the one you want.
- **Omit when:** The column type already sets an export format (e.g. **DateColumn**, **DateTimeColumn**, **MoneyColumn**). Only set **exportFormat** if you need a **different** format than that default.

```php
// Rely on column default → no exportFormat
MoneyColumn::make('credits', 'users.credits')->title('Credits'),
DateColumn::make('created_at', 'users.created_at')->title('Created'),

// Override only when needed
MoneyColumn::make('credits', 'users.credits')->exportFormat('#,##0.00'),
```

### Quick reference (export)

| Scenario | exportRender? | exportFormat? |
|----------|----------------|----------------|
| **Column::make** but value comes from **addColumn** (data ≠ name; e.g. employee_name link) | **Yes** | Only if date/money |
| **Column::computed** (value only from addColumn / custom) | **Yes** | Only if date/money and not from column type |
| Standard DB column (no addColumn, no HTML, no custom logic) | No | No (or only if overriding column default) |
| Custom attribute / accessor | Yes | Only if date/money and not from column type |
| Column value has HTML | Yes | Only if date/money and not from column type |
| Date/Money column (DateColumn, MoneyColumn, etc.) | No (unless computed) | Only if you need a different format than the column's default |

---

## EDITOR DISPLAY AND FORMS

**Modal width:** `->display('sm'|'md'|'lg'|'xl'|'fullscreen')` (see editor.display.*.js). Choose by horizontal space (field count/columns). >6 fields → grid, max 6 per column.

**Form layout:** Always wrap each `<editor-field>` in a `<div>`. Vertical: `flex flex-col gap-2`; grid: `grid grid-cols-2 gap-4`, one `<div><editor-field></editor-field></div>` per cell. Editor template: `Editor::make()->template('#myForm')->display('sm')->fields([...])`.

---

## EDITOR DATE/DATETIME

Model: cast date-only with `App\Casts\AsEditorDate`, datetime with `App\Casts\AsEditorDateTime`. Editor: `DateField::make(...)` / `DateTimeField::make(...)`. Table: `DateColumn`/`DateTimeColumn` + `->withColumnControlSearchDate()` when column control. Ensures correct serialization and no TZ shift.

---

## EDITOR FIELDS

Use App\Editor\Fields.*. Select from model: `->modelOptions($modelOrQuery, 'labelColumn', 'id')`. Options: prefer `Options::from()`, `Options::yesNo()`. Date/datetime in Editor → model cast AsEditorDate/AsEditorDateTime. Sub-resource: HiddenField default from constructor-injected parent.

---

## SUB-RESOURCE TABLES

Parent show builds Html via `ChildDataTableHtml::make($parent)`. Html constructor takes parent model; `HiddenField::make('parent_id')->default($parent->id)`. AJAX: `route('parents.children.index', $parent)`. Controller: `$table->with('parentKey', $parentModel)` then `$table->render(...)`. DataTable: no real property; use `@property Parent $parent`. Test: `DataTable::test(ChildDataTable::class, with: ['parent' => $parent])`. Don’t override static make() in Html; use render() in controller.

---

## CONTROLLER AND BLADE

When a controller returns `$dataTable->render(...)`, the method return type **must** be **`mixed`**. The render method can return either a View (HTML) or a JsonResponse (AJAX), so a single union or concrete type is inaccurate; use `mixed`.

```php
// Index — return type always mixed
public function index(ExamplesDataTable $dataTable): mixed
{
    return $dataTable->render('examples::index');
}
// Store
return $editor->process();
```

Blade: `<livewire:data-table :data-table="$dataTable" />`. Form partial optional; if used: root ID, only `<editor-field>` inside, each wrapped in `<div>`; reference by `#id`. **Controller:** any action that returns `$dataTable->render(...)` must declare return type `mixed`.

---

## INLINE ROUTE PATTERN (SIMPLE ROUTES ONLY)

Use this pattern **only for simple DataTable/Editor routes** with no custom logic (no extra authorization, view data, multiple actions or sub-resource).

**Pattern:**
- Add `protected ?string $view = 'module::view';` to DataTable
- Route: `Route::get('/module', DataTable::class)` / `Route::post('/module', Editor::class)`
- Delete the thin controller

**Before:**
```php
// Controller
final class ExamplesController extends Controller {
    public function index(ExamplesDataTable $dataTable): mixed {
        return $dataTable->render('examples::index');
    }
    public function store(ExamplesDataTableEditor $editor): JsonResponse {
        return $editor->process();
    }
}
Route::get('examples', [ExamplesController::class, 'index']);
Route::post('examples', [ExamplesController::class, 'store']);
```

**After:**
```php
// DataTable
final class ExamplesDataTable extends DataTable {
    protected ?string $view = 'examples::index';
}

Route::get('examples', ExamplesDataTable::class);
Route::post('examples', ExamplesDataTableEditor::class);
```

**Keep controller when:** Custom authorization, view data, or multiple actions needed.

---

## TEST HARNESS

Use `DataTable::test(Table::class)`; `draw()`, `searchTable()`, `create()`, `edit()`, `remove()`. Never craft raw POST payloads. Auth: call Pest `actingAs($user)` before DataTable so currentTeam() etc. apply. Remove validation: send DT_RowId in row; expect 400, message in `error`. Alias/search errors → fix column flags or filterColumn, not harness.

### Search tests (required for every DataTable)

Every DataTable **must** have at least one test that verifies server-side search works. Add it in the same module/feature test file that already tests that DataTable (e.g. draw or create).

- **Tables with an employee name column:** Use employee name for search. Create two records (e.g. two employees or two rows tied to two employees with distinct names), `draw()`, then `searchTable('<unique part of one employee name>')`, `assertRecordsFilteredEquals(1)`, and optionally `assertTableRowsContains([['id' => $expectedId]])` or another unique column.
- **Tables without employee name:** Use the primary searchable column (e.g. `name`, `code`, `title`). Create two records with distinct values, `draw()`, `searchTable('<unique term>')`, `assertRecordsFilteredEquals(1)`, and optionally `assertTableRowsContains([...])`.
- **Sub-resource tables:** Pass parent context via `with: ['parentKey' => $parent]`; create two child records with distinct searchable values (e.g. two employees in the same bonus), then search and assert.

Pattern:

```php
actingAs($user);
$dt = DataTable::test(SomeDataTable::class, with: [...]);  // omit with if not sub-resource
$dt->draw()->assertOk()->assertRecordsTotalEquals(2);
$dt->searchTable('UniqueSearchTerm')
    ->assertRecordsFilteredEquals(1)
    ->assertTableRowsContains([['id' => $expectedRecord->id]]);  // or another unique key
```

When adding a **new** DataTable, add this search test as part of the same change. When auditing existing DataTables, add a search test for any that lack one.

---

## TESTING AND PERFORMANCE

Assert: draw, recordsTotal, recordsFiltered, data; actingAs + permissions; named routes. **Every DataTable must have a search test** (see Search tests above). Eager load columns used in list; minimal with() column lists; filterColumn for relation/JSON search; avoid N+1.

---

## QUICK REFERENCE

| Scenario | Action |
|----------|--------|
| Eager load/join | select table.*; qualify where/names in Html |
| No eager/join | No select; unqualified names |
| Relation column | Column::make(relation_snake.col, relationName.col) |
| Column::make with empty/omitted name | Effective name = first arg (data); use that in filterColumn/orderColumn (e.g. filterColumn('email', fn ...)) |
| Enum display | formatColumn + Column::formatted |
| Enum + column control | Column::make(displayKey, name) + addColumn(displayKey) for label; columnControl Options + withColumnControlSearchDropdown; name = relationName.column or table.column (package handles search and order) |
| Boolean + column control | BooleanColumn::make + withColumnControlSearchDropdown + columnControlOptions only (no filterColumn) |
| Date/datetime + column control | DateColumn/DateTimeColumn + withColumnControlSearchDate |
| Editor date/datetime | Model AsEditorDate/AsEditorDateTime; Editor DateField/DateTimeField |
| Money | MoneyColumn; addColumn raw number or null |
| withCount / withSum | withCount('rel') / withSum('rel', 'col') in query(); Column::make or MoneyColumn with name for order; orderable(true), searchable(false); never ->relation()->count()/sum() in addColumn/exportRender |
| ID column | IDColumn::make(); last in columns() |
| Controller returning DataTable render | Method return type **must** be `mixed` (render returns View or JsonResponse) |
| Inline route (simple only) | Add `protected ?string $view` to DataTable; use `DataTable::class` / `Editor::class` (no method); delete controller |
| Editor actions | Only $actions/$customActions used; omit unused *Rules |
| Editor "cannot" | Validation rules + messages(); scope in query(); creating() for create-only |
| Editor $model type | No `/** @var */` cast; `@extends DataTablesEditor<Model>` resolves it via generics |
| Form layout | Wrap every editor-field in div; flex/grid |
| addColumn / editColumn / formatColumn / exportRender | Variable = model name; formatColumn $value = model cast/column type (enum, bool, string, etc.); exportRender only for computed/custom/HTML |
| exportFormat | Only when column type doesn’t already provide the format needed |
| Search test | **Required** for every DataTable: test search by employee name (if column exists) or primary searchable column; draw → searchTable → assertRecordsFilteredEquals(1) |

---

## MINIMAL PROMPT SUMMARY

"DataTables: separate DataTable/Editor/Html; Editor: only needed $actions/$customActions, validation not abort, creating() for create-only; no `/** @var ConcreteModel $model */` in editor methods — `@extends DataTablesEditor<ConcreteModel>` resolves the type through generics; DataTable::test() only; **every DataTable must have a search test** (employee name or primary searchable column); IDColumn last; qualify columns only when joins; eager load minimal columns; Column::make/computed/formatted/IDColumn/MoneyColumn/DateColumn/BooleanColumn per type; Column::make with empty name → name = data (use in filterColumn/orderColumn); column control: Options + dropdown (package handles filtering; no custom filterColumn), BooleanColumn options, date withColumnControlSearchDate; Editor dates: AsEditorDate/AsEditorDateTime + DateField/DateTimeField; form: wrap editor-field in div; sub-resource: with('key', parent); remove validation 400 + error, DT_RowId in payload; **inline route pattern**: add `protected ?string $view` to DataTable; use `DataTable::class` in route; delete controller for thin controllers. Row transforms: variable name matches model (\$user, \$role); formatColumn first param (\$value) typed as model cast/column type (enum, bool, string, float|int, array), not mixed; addColumn/editColumn/formatColumn/exportRender receive query model; exportRender required when Column::make data comes from addColumn (data≠name) or Column::computed; exportFormat only when column type doesn't already provide it. withCount/withSum: load in query(); use Column::make / MoneyColumn with name for order; orderable(true), searchable(false); never use ->relation()->count() or ->relation()->sum() in addColumn/exportRender."
