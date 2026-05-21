---
name: vitest-testing
description: "Vue 3 + TypeScript testing skill using Vitest and Vue Test Utils. Detects the project's existing test setup before generating tests."
categories:
  - testing
  - vue3
  - typescript
tags:
  - Vitest
  - VueTestUtils
  - Testing
  - Composables
  - Components
  - Stores
---

# Vue Testing Skill

Generates unit tests for Vue 3 + TypeScript projects using **Vitest** and **@vue/test-utils**. Before writing any test, always detect what the project already has.

---

## 1. Stack Detection

Before generating tests, inspect the project to identify:

### Check `package.json` devDependencies for:

| Tool | Detect by | Notes |
|---|---|---|
| **Test runner** | `vitest`, `jest`, `@jest/core` | Default: Vitest |
| **Component testing** | `@vue/test-utils`, `@testing-library/vue` | Default: Vue Test Utils |
| **Mocking** | `vitest` (built-in), `jest-mock`, `msw` | Vitest has built-in `vi` |
| **HTTP mocking** | `msw`, `axios-mock-adapter`, `nock` | Use what's present |
| **Coverage** | `@vitest/coverage-v8`, `@vitest/coverage-istanbul` | Check vitest.config |
| **State library** | `pinia`, `vuex` | Affects store testing strategy |

### Check config files:
- `vitest.config.ts` / `vite.config.ts` → `test` block (globals, environment, setupFiles)
- `jest.config.ts` / `jest.config.js`
- `vitest.setup.ts` or `setupTests.ts` → existing global setup

### Infer from structure:
- `src/**/__tests__/` or `src/**/*.spec.ts` → test colocation pattern
- `tests/unit/` → separate test folder pattern

> **Do not introduce new testing dependencies** unless the project has none. Adapt examples to the detected stack.

---

## 2. Project Setup (Default: Vitest)

Only suggest this if no test runner is detected.

```bash
npm install -D vitest @vue/test-utils @vitest/coverage-v8 jsdom
```

### `vitest.config.ts` (when using standalone config)
```ts
import { defineConfig } from 'vitest/config'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: ['./vitest.setup.ts'],
    coverage: {
      provider: 'v8',
      reporter: ['text', 'lcov'],
      include: ['src/**/*.{ts,vue}'],
      exclude: ['src/**/*.d.ts', 'src/main.ts']
    }
  }
})
```

### `vitest.setup.ts`
```ts
import { config } from '@vue/test-utils'
// Add global stubs, plugins, or directives here
```

---

## 3. Test Structure Conventions

- **Location**: colocate next to source — `src/features/agents/composables/__tests__/useAgentForm.spec.ts`
- **Naming**: `{unit}.spec.ts`
- **Pattern**: Arrange → Act → Assert (AAA)
- **Test name format**: `describe('useAgentForm') > it('returns validation errors when name is empty')`

---

## 4. Composable Tests

Composables are the primary unit — test them directly without mounting a component.

```ts
import { describe, it, expect, beforeEach } from 'vitest'
import { useAgentForm } from '../useAgentForm'

describe('useAgentForm', () => {
  it('initializes with empty form data', () => {
    const { formData } = useAgentForm()
    expect(formData.name).toBe('')
  })

  it('returns validation errors when name is empty', () => {
    const { formData, validate, errors } = useAgentForm()
    formData.name = ''
    const isValid = validate()
    expect(isValid).toBe(false)
    expect(errors.name).toBeTruthy()
  })

  it('clears errors after valid input', () => {
    const { formData, validate, errors } = useAgentForm()
    formData.name = 'My Agent'
    validate()
    expect(errors.name).toBeFalsy()
  })
})
```

---

## 5. Component Tests

Use `@vue/test-utils` `mount` for component behavior. Prefer `shallowMount` for isolation.

```ts
import { describe, it, expect } from 'vitest'
import { mount } from '@vue/test-utils'
import AgentList from '../AgentList.vue'
import type { Agent } from '../../types/agent'

const agents: Agent[] = [
  { id: '1', name: 'Agent Alpha', status: 'active' },
  { id: '2', name: 'Agent Beta', status: 'inactive' }
]

describe('AgentList', () => {
  it('renders one row per agent', () => {
    const wrapper = mount(AgentList, {
      props: { agents }
    })
    expect(wrapper.findAll('[data-testid="agent-row"]')).toHaveLength(2)
  })

  it('emits select event with agent id on click', async () => {
    const wrapper = mount(AgentList, { props: { agents } })
    await wrapper.find('[data-testid="agent-row"]').trigger('click')
    expect(wrapper.emitted('select')?.[0]).toEqual(['1'])
  })

  it('shows empty state when agents list is empty', () => {
    const wrapper = mount(AgentList, { props: { agents: [] } })
    expect(wrapper.find('[data-testid="empty-state"]').exists()).toBe(true)
  })
})
```

---

## 6. Store Tests (Pinia)

Test stores independently using `createPinia` + `setActivePinia`.

```ts
import { describe, it, expect, beforeEach } from 'vitest'
import { setActivePinia, createPinia } from 'pinia'
import { useAgentStore } from '../useAgentStore'

describe('useAgentStore', () => {
  beforeEach(() => {
    setActivePinia(createPinia())
  })

  it('starts with empty agents list', () => {
    const store = useAgentStore()
    expect(store.agents).toEqual([])
  })

  it('sets agents on setAgents', () => {
    const store = useAgentStore()
    store.setAgents([{ id: '1', name: 'Alpha', status: 'active' }])
    expect(store.agents).toHaveLength(1)
  })
})
```

> For **Vuex**, use `createStore` with the same isolate-per-test pattern.

---

## 7. Service / API Tests (with Mocking)

### With `vi.fn()` (no HTTP library mock needed)
```ts
import { describe, it, expect, vi, beforeEach } from 'vitest'
import { useAgentService } from '../agentService'

const mockFetch = vi.fn()
global.fetch = mockFetch

describe('agentService', () => {
  beforeEach(() => {
    mockFetch.mockReset()
  })

  it('calls correct endpoint on getAgents', async () => {
    mockFetch.mockResolvedValueOnce({
      ok: true,
      json: async () => [{ id: '1', name: 'Alpha' }]
    })
    const service = useAgentService()
    const result = await service.getAgents()
    expect(mockFetch).toHaveBeenCalledWith('/api/agents')
    expect(result).toHaveLength(1)
  })
})
```

### With MSW (if present in project)
```ts
import { http, HttpResponse } from 'msw'
import { server } from '../../mocks/server'

server.use(
  http.get('/api/agents', () => HttpResponse.json([{ id: '1', name: 'Alpha' }]))
)
```

---

## 8. Async & Loading State Tests

```ts
it('sets isLoading true during fetch and false after', async () => {
  mockFetch.mockResolvedValueOnce({ ok: true, json: async () => [] })
  const { isLoading, load } = useAgentList()

  const promise = load()
  expect(isLoading.value).toBe(true)
  await promise
  expect(isLoading.value).toBe(false)
})
```

---

## 9. Must-Test Checklist

For each feature, ensure coverage of:

- [ ] Happy path (valid input, successful response)
- [ ] Validation errors (required fields, format checks)
- [ ] Loading state (`isLoading` true → false)
- [ ] Error state (API failure → `errors.general` set)
- [ ] Empty state (empty list → empty-state element visible)
- [ ] User interactions (click, submit, input events → correct emits or state changes)

---

## 10. Running Tests

```bash
# Run all tests
npx vitest run

# Watch mode
npx vitest

# With coverage
npx vitest run --coverage

# Filter by file
npx vitest run src/features/agents
```

Add to `package.json`:
```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:coverage": "vitest run --coverage"
  }
}
```
