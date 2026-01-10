# Automation Hints: {{flow_name}}

## Selector Strategy Guide

### Preferred Selectors (Most Stable)

| Priority | Strategy | Example |
|----------|----------|---------|
| 1 | data-testid | `[data-testid="submit-button"]` |
| 2 | Role + Name | `getByRole('button', { name: 'Submit' })` |
| 3 | aria-label | `[aria-label="Close dialog"]` |
| 4 | Visible text | `getByText('Create Project')` |
| 5 | Placeholder | `[placeholder="Enter email"]` |

### Selectors to Avoid (Brittle)

| Type | Example | Why Avoid |
|------|---------|-----------|
| CSS classes | `.btn-primary-xl-2` | Classes change frequently |
| Absolute paths | `div > div > button:nth-child(3)` | Structure changes |
| Dynamic IDs | `#element-12345` | IDs regenerate |
| Pixel positions | `click(450, 320)` | Layout varies |

---

## Element-Specific Guidance

### Step {{step_id}}: {{step_title}}

**Target Element:** {{element_description}}

**Recommended Approach:**
```typescript
// Best: Use role + accessible name
await page.getByRole('{{element_role}}', { name: '{{accessible_name}}' }).click();

// Alternative: Use test ID if available
await page.getByTestId('{{test_id}}').click();

// Fallback: Use text content
await page.getByText('{{visible_text}}').click();
```

**Wait Strategy:**
```typescript
// Wait for element to be actionable
await page.getByRole('{{element_role}}', { name: '{{accessible_name}}' }).waitFor();

// Or wait for specific state
await expect(page.locator('{{selector}}')).toBeVisible();
await expect(page.locator('{{selector}}')).toBeEnabled();
```

**Edge Cases:**
- {{edge_case_1}}
- {{edge_case_2}}

---

## Common Patterns

### Handling Modals/Dialogs

```typescript
// Wait for modal to appear
await expect(page.getByRole('dialog')).toBeVisible();

// Interact within modal context
await page.getByRole('dialog').getByRole('button', { name: 'Confirm' }).click();

// Wait for modal to close
await expect(page.getByRole('dialog')).not.toBeVisible();
```

### Handling Dropdowns

```typescript
// Click to open dropdown
await page.getByRole('combobox', { name: '{{dropdown_label}}' }).click();

// Select option
await page.getByRole('option', { name: '{{option_text}}' }).click();

// Verify selection
await expect(page.getByRole('combobox')).toHaveValue('{{expected_value}}');
```

### Handling Form Inputs

```typescript
// Fill text input
await page.getByLabel('{{input_label}}').fill('{{input_value}}');

// Or use placeholder
await page.getByPlaceholder('{{placeholder_text}}').fill('{{input_value}}');

// Verify input
await expect(page.getByLabel('{{input_label}}')).toHaveValue('{{input_value}}');
```

### Handling Tables/Lists

```typescript
// Find row by content
const row = page.getByRole('row').filter({ hasText: '{{row_identifier}}' });

// Interact with cell in row
await row.getByRole('button', { name: 'Edit' }).click();
```

---

## Timing & Synchronization

### Recommended Wait Conditions

| Scenario | Wait For |
|----------|----------|
| Page navigation | `page.waitForURL('{{expected_url}}')` |
| API response | `page.waitForResponse('**/api/{{endpoint}}')` |
| Loading complete | `page.waitForLoadState('networkidle')` |
| Element visible | `expect(locator).toBeVisible()` |
| Element enabled | `expect(locator).toBeEnabled()` |

### Avoid These Anti-Patterns

❌ `await page.waitForTimeout(3000)` - Fixed waits are flaky

✅ `await expect(page.locator('{{selector}}')).toBeVisible()` - Condition-based waits

---

## Debugging Tips

### When Tests Fail

1. **Check selector stability:**
   ```bash
   # In browser console
   document.querySelector('{{selector}}')
   ```

2. **Capture state on failure:**
   ```typescript
   test.afterEach(async ({ page }, testInfo) => {
     if (testInfo.status !== 'passed') {
       await page.screenshot({ path: `failure-${testInfo.title}.png` });
     }
   });
   ```

3. **Use trace viewer:**
   ```bash
   npx playwright test --trace on
   npx playwright show-trace trace.zip
   ```

---

## Accessibility Considerations

Ensure selectors work with assistive technologies:

- Prefer `getByRole()` - matches ARIA roles
- Use `getByLabel()` - matches form labels
- Verify `aria-` attributes are present
- Test with screen reader if possible
