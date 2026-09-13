# Danwa Studio — Sprint 1 Status & Todo

> Letzte Aktualisierung: 2026-06-15
> Repository: `danwa-studio` (https://github.com/Saga-AI-Labs/danwa-studio)
> Backend: `danwa-core` (https://github.com/Saga-AI-Labs/danwa-core)

## Git Remotes

- `git@github.com:Saga-AI-Labs/danwa-core.git`
- `git@github.com:Saga-AI-Labs/danwa-studio.git`

## Architektur

- **danwa-core**: FastAPI Backend (Port 8000)
- **danwa-studio**: Svelte 5 + Vite Frontend (Port 5174, proxy `/api` → 8000)
- **Shared Packages** (`file:` protocol):
  - `@danwa/api-client` → `danwa-core/packages/api-client/src/index.ts`
  - `@danwa/ui-core` → `danwa-core/packages/ui-core/src/`
  - `@danwa/i18n` → `danwa-core/packages/i18n/src/loader.js`

## Dev Start

```bash
# Terminal 1: danwa-core backend
cd /media/data/coding/danwa-core
poetry run uvicorn backend.main:app --reload --port 8000

# Terminal 2: danwa-studio frontend
cd /media/data/coding/danwa-studio
npm run dev

# Terminal 3: danwa (end-user, unchanged, not needed)
```

## Views / Routes (23 total)

Alle in `src/views/`, registriert in `src/App.svelte` mit Hash-Routing.

| View | Status | Lines |
|------|--------|-------|
| LoginView | ✅ Done | working |
| Sidebar | ✅ Done | working |
| DashboardView | ✅ Stub | - |
| BlueprintCanvasView | ✅ Implemented (from danwa) | full |
| WorkflowTemplatesView | ✅ Implemented | 138 |
| **LLMProfilesView** | ⏳ HALF — table + delete + modal scaffolded, **modal component missing** | 145 |
| **LLMAgentsView** | ⏳ Stub (17 lines) — needs full implementation | 17 |
| **PromptsView** | ⏳ Stub (17 lines) — needs full implementation | 17 |
| RolesView | ⏳ Stub | - |
| ModulesView | ⏳ Stub | - |
| InputComposerView | ⏳ Stub | - |
| OutputComposerView | ⏳ Stub | - |
| WorkflowExecView | ⏳ Stub | - |
| DiffView | ⏳ Stub | - |
| ReplayView | ⏳ Stub | - |
| ProposalsView | ⏳ Stub | - |
| TranslationsView | ⏳ Stub | - |
| TenantsView | ⏳ Stub | - |
| UsersView | ⏳ Stub | - |
| ServerHealthView | ⏳ Stub | - |
| SystemManagementView | ⏳ Stub | - |
| ProfileView | ⏳ Stub | - |
| BYOKManager | ⏳ Stub | - |

## Current Sprint 1 Status

### WorkflowTemplatesView.svelte ✅
- Fully implemented with table, instantiate modal, delete confirmation
- Uses `listWorkflowTemplates`, `getWorkflowTemplate`, `instantiateWorkflowTemplate`, `deleteWorkflowTemplate`
- `TemplateInstantiateModal.svelte` exists and works
- `ConfirmDialog.svelte` works

### LLMProfilesView.svelte ⏳
- Table, delete, and create/edit button logic WRITTEN
- **`LLMProfileModal.svelte` component was being created but write failed** — needs to be created manually:

```svelte
<!-- src/components/blueprint/LLMProfileModal.svelte -->
<!-- Copy pattern from TemplateInstantiateModal.svelte -->
<!-- Uses: createBlueprintLLMProfile, updateBlueprintLLMProfile from ../lib/blueprint/api.js -->
<!-- Props: profile (null = create), visible, onSuccess, onClose -->
<!-- Form fields matching LLMProfileForm.svelte: name, profile_type, provider, model, 
     api_base, api_key_env, account_id_env, temperature, max_tokens, context_window, 
     timeout, protocol, a2a_endpoint, fallback_llm_profile_id -->
```

### LLMAgentsView.svelte ❌
- Only a stub
- API functions exist: `listAgentBlueprints`, `getAgentBlueprint`, `createAgentBlueprint`, `updateAgentBlueprint`, `deleteAgentBlueprint`
- Needs a modal component too (`AgentBlueprintModal.svelte`)
- Form fields: name, role, llm_profile_id, system_prompt, temperature, max_tokens, argumentation_pattern, tone_profile_id, active

### PromptsView.svelte ❌
- Only a stub
- API functions exist: `listPromptTemplates`, `getPromptTemplate`, `createPromptTemplate`, `updatePromptTemplate`, `deletePromptTemplate`
- Needs a modal (`PromptTemplateModal.svelte`)
- Form fields: name, role, variant, content (system/user prompt template text), description, active

## Components status

| Component | Status |
|-----------|--------|
| `ConfirmDialog.svelte` | ✅ Done |
| `Sidebar.svelte` | ✅ Done |
| `Header.svelte` | ✅ Done |
| `TemplateInstantiateModal.svelte` | ✅ Done |
| `LLMProfileForm.svelte` | ✅ Done (Blueprint Inspector context) |
| `LLMProfileModal.svelte` | ❌ NOT created yet |
| `AgentBlueprintModal.svelte` | ❌ NOT created yet |
| `PromptTemplateModal.svelte` | ❌ NOT created yet |
| Blueprint Canvas components | ✅ From danwa |

## i18n

- English SSOT in `src/lib/i18n/loader.js`
- Fallback keys (`enFallback`) updated with config-related keys
- Module loading via fetch from `/modules/i18n-{locale}/ui_strings.json`

## Next Steps (ordered)

1. **Create LLMProfileModal.svelte** — modal for creating/editing LLM profiles
2. **Implement LLMAgentsView.svelte fully** — table + modal for agent blueprints
3. **Create AgentBlueprintModal.svelte** — modal for agent create/edit
4. **Implement PromptsView.svelte fully** — table + modal for prompt templates
5. **Create PromptTemplateModal.svelte** — modal for prompt create/edit
6. **Implement other views** (Roles, Modules, Input/Output Composer, etc.)
7. **Sprint 2**: Module Manager (Manifest-Viewer, Versionierung, Schema-Editor)
8. **Sprint 3**: Prompt Editor with Live-Test-Run + i18n Sync Tool
9. **Sprint 4**: Workflow Exec + HITL Panel + Diff/Replay Views
10. **Sprint 5**: Tenant/Admin Views + Server Health + System Management
11. **Sprint 6**: Module Publishing + Translation Dashboard

## Known Issues

- The LLMProfileModal.svelte file write failed mid-way — consult the git history or re-create it
- All modal forms should follow the pattern of `TemplateInstantiateModal.svelte` (modal overlay, header, body, footer)
- All list views should follow `WorkflowTemplatesView.svelte` (table, loading, empty state, delete confirm)
- Use `ConfirmDialog` for delete confirmations, pass `variant="danger"`
- Import i18n via `import { i18n } from '../lib/i18n/loader.js'`
- Use `$i18n.t('key')` in markup, or `$derived` for bindings
- State management: `$state()`, effects via `$effect()`, derived via `$derived()`
- Error handling: set `errorStore.set(e.message)` from `../lib/stores.js`
