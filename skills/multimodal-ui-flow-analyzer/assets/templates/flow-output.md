# UI_FLOW: {{flow_name}}

## Metadata
- **App:** {{application_name}}
- **Flow Type:** Static UI Interaction
- **Source:** Annotated screenshots + human-authored text
- **Generated:** {{timestamp}}
- **Total Steps:** {{step_count}}

---

## Flow Overview

{{flow_description}}

### Prerequisites
- {{prerequisite_1}}
- {{prerequisite_2}}

### Expected Outcome
{{expected_outcome}}

---

## Steps

### Step 1: {{step_1_title}}

**Intent:** {{step_1_intent}}

**Action:**
- type: {{action_type}}
- target:
  - role: {{element_role}}
  - text: "{{visible_text}}"
  - location: {{position_description}}

**Preconditions:**
- {{precondition}}

**Postconditions:**
- {{postcondition}}

**Automation Notes:**
- Preferred selector: `{{preferred_selector}}`
- Fallback selector: `{{fallback_selector}}`
- Wait condition: {{wait_condition}}

---

### Step 2: {{step_2_title}}

**Intent:** {{step_2_intent}}

**Action:**
- type: {{action_type}}
- target:
  - role: {{element_role}}
  - text: "{{visible_text}}"
  - location: {{position_description}}

**Preconditions:**
- {{precondition}}

**Postconditions:**
- {{postcondition}}

**Automation Notes:**
- Preferred selector: `{{preferred_selector}}`
- Fallback selector: `{{fallback_selector}}`
- Wait condition: {{wait_condition}}

---

<!-- Repeat Step template for additional steps -->

---

## Flow Validation Checklist

- [ ] All steps have clear preconditions
- [ ] Step ordering is logical and complete
- [ ] No ambiguous UI references remain
- [ ] Each action has a defined resulting state
- [ ] Selectors are stable and semantic

---

## Automation Integration

### Playwright Example

```typescript
import { test, expect } from '@playwright/test';

test('{{flow_name}}', async ({ page }) => {
  // Step 1: {{step_1_title}}
  await page.getByRole('{{element_role}}', { name: '{{visible_text}}' }).click();
  await expect(page.locator('{{postcondition_selector}}')).toBeVisible();

  // Step 2: {{step_2_title}}
  // ... continue for each step
});
```

### Cypress Example

```javascript
describe('{{flow_name}}', () => {
  it('completes the flow successfully', () => {
    // Step 1: {{step_1_title}}
    cy.get('[role="{{element_role}}"]').contains('{{visible_text}}').click();
    cy.get('{{postcondition_selector}}').should('be.visible');

    // Step 2: {{step_2_title}}
    // ... continue for each step
  });
});
```

---

## Notes & Caveats

{{additional_notes}}
