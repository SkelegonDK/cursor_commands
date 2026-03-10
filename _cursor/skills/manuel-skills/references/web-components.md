# Web Components State Diagram

Act like a senior frontend architect. Analyze components and produce Mermaid stateDiagram-v2.

## Analysis Protocol

### Phase 1: Component Discovery
- Identify all components (React, Vue, Svelte, Angular, Astro, Web Components)
- Map hierarchy and nesting
- Classify: presentational, container, layout, page, utility

### Phase 2: Connection Analysis
- Props/input flow
- Events/output flow
- Shared state (context, stores, services)
- Side effects per component

### Phase 3: State Mapping
- Internal states
- State transitions and triggers
- Conditional rendering

## Component Types & Symbols

| Type | Symbol |
|------|--------|
| Page/Route | `[[ ]]` |
| Container | `[( )]` |
| Presentational | `[ ]` |
| Layout | `{{ }}` |
| Provider | `(( ))` |
| Utility | `([ ])` |

## Arrows: `-->` props | `-->` events | `-->>` async | `-.->` conditional | `<-->` two-way

## Output: Component Registry + State Diagram + Data Flow Summary + Key Insights

## Framework Notes
- **React:** useState, useReducer, useContext, props, callbacks, useEffect
- **Vue:** props, emits, provide/inject, ref, reactive, computed
- **Svelte:** export let, dispatch, $: reactive, stores
- **Astro:** Astro.props, client:* directives, slots, islands
- **Web Components:** observedAttributes, attributeChangedCallback, custom events
