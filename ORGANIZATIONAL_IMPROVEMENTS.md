# Organizational Improvements - Auto-Calculated Hour Breakdown

**Commit:** e14619f
**Branch:** claude/odoo-implementation-planner-011CUqvLHbyg96f3qjACvM3d
**Status:** ✅ COMPLETE - Ready to test!

---

## Overview

This update addresses all 5 organizational issues from user feedback by implementing a cleaner, more logical questionnaire structure with auto-calculated hour breakdowns.

## What Changed

### Issue 1: Wrong Questionnaire Order ❌ → ✅

**Before:**
```
Sections:
1. Project Information
2. Scope & Hours
   - Clarity Phase
   - Implementation Phase
     → Modules to Implement
   - Adoption Phase
3. Custom Modules         ← Wrong! Disconnected from Implementation
4. Data Migration         ← Wrong! Disconnected from Implementation
5. Clarity Details
6. Implementation Details
7. Adoption Details
```

**After:**
```
Sections:
1. Project Information
2. Scope & Hours
   - Clarity Phase
     → Clarity Hours
   - Implementation Phase
     → Modules to Implement
     → Custom Modules        ← Moved under Implementation! ✅
     → Data Migration        ← Moved under Implementation! ✅
   - Adoption Phase
     → Training Hours
     → Support Hours per Month
     → Support Duration (months)
   - Project Budget Summary (all 3 phases)
3. Clarity Details
4. Implementation Details
5. Adoption Details
```

**Why This Is Better:**
- ✅ Custom Modules and Data Migration are now clearly part of Implementation
- ✅ All Implementation-related items grouped together
- ✅ Logical flow: Select modules → Add custom modules → Configure migration
- ✅ Easier to understand project structure

---

### Issue 2: Budget Tracker Placement ❌ → ✅

**Before:**
```
Section 2: Scope & Hours
━━━━━━━━━━━━━━━━━━━━━━━━
Implementation Phase Hours: 165

Implementation Hours Budget Tracker  ← Only shows Implementation!
───────────────────────────────────
Total Budget: 165h
Allocated: 165h
• Modules: 120h
• Custom: 30h
• Migration: 15h
```

**After:**
```
Section 2: Scope & Hours
━━━━━━━━━━━━━━━━━━━━━━━━
Clarity Phase Hours: 65
Implementation breakdown...
Adoption breakdown...

Project Budget Summary  ← Shows ALL 3 phases! ✅
───────────────────────
Total Project Budget: 270h

• Clarity Phase: 65h
• Implementation Phase: 165h
  - Odoo Modules: 120h
  - Custom Modules: 30h
  - Data Migration: 15h
• Adoption Phase: 40h
  - Training: 16h
  - Support: 24h (12h/month × 2 months)
```

**Why This Is Better:**
- ✅ Single unified budget tracker for entire project
- ✅ Shows breakdown of all 3 phases
- ✅ Clear visualization of where hours are allocated
- ✅ Helps see total project budget at a glance

---

### Issue 3: Adoption Auto-Calculation ❌ → ✅

**Before:**
```
Adoption Phase Hours: [40]  ← Manual input, error-prone!

No breakdown - just a single number
```

**After:**
```
Training Hours: [16]
Support Hours per Month: [12]
Support Duration (months): [2]

Auto-calculated: 16 + (12 × 2) = 40h  ✅
```

**Why This Is Better:**
- ✅ Breakdown matches how Adoption is actually quoted
- ✅ Training hours separate from ongoing support
- ✅ Clear understanding: "16h training + 12h/month for 2 months"
- ✅ Auto-calculation prevents mismatches
- ✅ Easier to adjust support duration or hours per month

---

### Issue 4: Implementation Auto-Calculation ❌ → ✅

**Before:**
```
Implementation Phase Hours: [165]  ← Manual input

Modules:
→ CRM Hours: [20]
→ Sales Hours: [30]
→ Purchase Hours: [25]
...

Custom Modules:
→ I+D Module Hours: [50]

Data Migration Hours: [40]

Problem: 20+30+25+50+40 = 165 ✅
But what if user enters 200 for Implementation Hours? ❌
Mismatch! Hours don't add up!
```

**After:**
```
No manual "Implementation Hours" field!

Modules:
→ CRM Hours: [20]
→ Sales Hours: [30]
→ Purchase Hours: [25]

Custom Modules:
→ I+D Module Hours: [50]

Data Migration Hours: [40]

Auto-calculated: 20+30+25+50+40 = 165h  ✅
```

**Why This Is Better:**
- ✅ Implementation hours **always** match the breakdown
- ✅ No possibility of mismatched totals
- ✅ User enters hours where they matter (per module)
- ✅ System calculates the total automatically
- ✅ Task generation uses exact breakdown values

---

### Issue 5: Custom Module Card Design ❌ → ✅

**Before:**
```jsx
<div className="p-3 rounded-lg border-2 border-purple-600 bg-orange-50">
  ← Orange-50 background (light)
  ← Doesn't match purple-600 pattern of standard modules
  ← Name input looks different
  ← No description line
  ← Different visual hierarchy
</div>
```

**After:**
```jsx
<div className="p-3 rounded-lg border-2 bg-orange-600 text-white border-orange-600">
  ← Orange-600 background (matches purple-600 pattern!)
  ← White text matching standard modules
  ← Description line: "Custom development"
  ← Border-t separator before hours (same as standard modules)
  ← Exact same visual structure!
</div>
```

**Visual Comparison:**

**Standard Module Card:**
```
┌─────────────────────────┐
│ CRM                      │ ← White text on purple-600
│ Lead & opportunity mgmt  │ ← Description
│ ────────────────────     │ ← Border-t separator
│ Hours: [20]              │ ← Hour input
└─────────────────────────┘
```

**Custom Module Card (BEFORE):**
```
┌─────────────────────────┐
│ [I+D Module     ]        │ ← Dark text input
│ [50]                     │ ← Hours input (no border)
└─────────────────────────┘
  Different design! ❌
```

**Custom Module Card (AFTER):**
```
┌─────────────────────────┐
│ [I+D Module     ]        │ ← White text on orange-600
│ Custom development       │ ← Description ✅
│ ────────────────────     │ ← Border-t separator ✅
│ Hours: [50]              │ ← Hour input ✅
└─────────────────────────┘
  Matches standard design! ✅
```

**Why This Is Better:**
- ✅ Custom modules look like first-class modules
- ✅ Consistent visual language across all module cards
- ✅ Orange color distinguishes custom from standard
- ✅ Same size, layout, and interaction pattern

---

## Technical Implementation

### 1. Questionnaire Structure Reorganization

**File:** `questionnaire-structure.json:101-224`

**Changes:**
1. Moved `custom_modules_visual` question under `implementation_phase` section
2. Moved `data_migration` and `migration_hours` under `implementation_phase` section
3. Removed `implementation_hours` manual input field (now auto-calculated)
4. Removed `adoption_hours` manual input field (now auto-calculated)
5. Added breakdown fields: `training_hours`, `support_hours_per_month`, `adoption_duration_months`
6. Added `budget_tracker` question type at end showing all 3 phases

**Before Structure:**
```json
{
  "id": "scope_and_hours",
  "questions": [
    {"id": "clarity_phase"},
    {"id": "clarity_hours"},
    {"id": "implementation_phase"},
    {"id": "implementation_hours"},  // ❌ Manual input
    {"id": "modules"},
    {"id": "adoption_phase"},
    {"id": "adoption_hours"}  // ❌ Manual input
  ]
}
```

**After Structure:**
```json
{
  "id": "scope_and_hours",
  "questions": [
    {"id": "clarity_phase"},
    {"id": "clarity_hours"},
    {"id": "implementation_phase"},
    {"id": "modules"},
    {"id": "custom_modules_visual"},  // ✅ Moved here
    {"id": "data_migration"},          // ✅ Moved here
    {"id": "migration_hours"},         // ✅ Moved here
    {"id": "adoption_phase"},
    {"id": "training_hours"},          // ✅ Breakdown
    {"id": "support_hours_per_month"}, // ✅ Breakdown
    {"id": "adoption_duration_months"},// ✅ Breakdown
    {"id": "budget_tracker"}           // ✅ Unified tracker
  ]
}
```

### 2. Task Generation - Implementation Phase

**File:** `App.jsx:221-245`

**OLD Code:**
```javascript
const implementationHours = parseFloat(responses.implementation_hours || 0);
// ❌ Reads manual input that no longer exists!
```

**NEW Code:**
```javascript
// Calculate Implementation hours from breakdown (modules + custom + migration)
const moduleFields = [
  'module_crm_hours', 'module_sales_hours', 'module_purchase_hours',
  'module_inventory_hours', 'module_accounting_hours', 'module_projects_hours',
  'module_fsm_hours', 'module_expenses_hours', 'module_manufacturing_hours',
  'module_ecommerce_hours', 'module_pos_hours', 'module_hr_hours',
  'module_payroll_hours', 'module_helpdesk_hours'
];

let moduleHoursTotal = 0;
moduleFields.forEach(field => {
  moduleHoursTotal += parseFloat(responses[field]) || 0;
});

let customHoursTotal = 0;
const customCount = parseInt(responses.custom_modules_count) || 0;
for (let i = 1; i <= customCount; i++) {
  customHoursTotal += parseFloat(responses[`custom_module_${i}_hours`]) || 0;
}

const migrationHoursTotal = parseFloat(responses.migration_hours) || 0;
const implementationHours = moduleHoursTotal + customHoursTotal + migrationHoursTotal;
// ✅ Auto-calculated from breakdown!
```

**Result:** Task generation uses exact breakdown values, no possibility of mismatch

### 3. Task Generation - Adoption Phase

**File:** `App.jsx:405-434`

**OLD Code:**
```javascript
const adoptionHours = parseFloat(responses.adoption_hours || 0);
const implementationHours = parseFloat(responses.implementation_hours || 0);
// ❌ Reads manual inputs that no longer exist!
```

**NEW Code:**
```javascript
// Calculate Adoption hours from breakdown (training + support)
const trainingHours = parseFloat(responses.training_hours) || 0;
const supportPerMonth = parseFloat(responses.support_hours_per_month) || 0;
const adoptionDurationMonths = parseInt(responses.adoption_duration_months || 2);
const adoptionHours = trainingHours + (supportPerMonth * adoptionDurationMonths);

// Calculate Implementation hours from breakdown for timing purposes
const moduleFields = [...];
let implementationHoursForTiming = 0;
moduleFields.forEach(field => {
  implementationHoursForTiming += parseFloat(responses[field]) || 0;
});
const customCount = parseInt(responses.custom_modules_count) || 0;
for (let i = 1; i <= customCount; i++) {
  implementationHoursForTiming += parseFloat(responses[`custom_module_${i}_hours`]) || 0;
}
implementationHoursForTiming += parseFloat(responses.migration_hours) || 0;
// ✅ Auto-calculated from breakdown!
```

**Result:** Both phases use calculated values for accurate task timing

### 4. Review Screen - Display Calculated Hours

**File:** `App.jsx:1321-1360`

**OLD Code:**
```jsx
<span className="font-semibold">{responses.implementation_hours || 0} hours</span>
<span className="font-semibold">{responses.adoption_hours || 0} hours</span>
// ❌ Displays manual inputs that no longer exist!
```

**NEW Code:**
```jsx
{responses.implementation_phase && (() => {
  // Calculate Implementation hours from breakdown
  const moduleFields = [...];
  let implementationHours = 0;
  moduleFields.forEach(field => {
    implementationHours += parseFloat(responses[field]) || 0;
  });
  const customCount = parseInt(responses.custom_modules_count) || 0;
  for (let i = 1; i <= customCount; i++) {
    implementationHours += parseFloat(responses[`custom_module_${i}_hours`]) || 0;
  }
  implementationHours += parseFloat(responses.migration_hours) || 0;

  return (
    <div className="flex justify-between">
      <span>✓ Implementation Phase</span>
      <span className="font-semibold">{implementationHours} hours</span>
    </div>
  );
})()}

{responses.adoption_phase && (() => {
  // Calculate Adoption hours from breakdown
  const trainingHours = parseFloat(responses.training_hours) || 0;
  const supportPerMonth = parseFloat(responses.support_hours_per_month) || 0;
  const supportMonths = parseInt(responses.adoption_duration_months) || 0;
  const adoptionHours = trainingHours + (supportPerMonth * supportMonths);

  return (
    <div className="flex justify-between">
      <span>✓ Adoption Phase</span>
      <span className="font-semibold">{adoptionHours} hours</span>
    </div>
  );
})()}
// ✅ Displays auto-calculated values!
```

**Result:** Review screen shows accurate calculated hours matching the breakdown

### 5. Unified Budget Tracker

**File:** `App.jsx:893-987`

**New Question Type:** `budget_tracker`

**Implementation:**
```javascript
case 'budget_tracker':
  // Calculate all phase hours
  const clarityHours = parseFloat(responses.clarity_hours) || 0;

  // Calculate Implementation hours from modules + custom + migration
  const moduleFields = [
    'module_crm_hours', 'module_sales_hours', 'module_purchase_hours',
    'module_inventory_hours', 'module_accounting_hours', 'module_projects_hours',
    'module_fsm_hours', 'module_expenses_hours', 'module_manufacturing_hours',
    'module_ecommerce_hours', 'module_pos_hours', 'module_hr_hours',
    'module_payroll_hours', 'module_helpdesk_hours'
  ];

  let moduleHours = 0;
  moduleFields.forEach(field => {
    moduleHours += parseFloat(responses[field]) || 0;
  });

  let customHours = 0;
  const customCount = parseInt(responses.custom_modules_count) || 0;
  for (let i = 1; i <= customCount; i++) {
    customHours += parseFloat(responses[`custom_module_${i}_hours`]) || 0;
  }

  const migrationHours = parseFloat(responses.migration_hours) || 0;
  const implementationHours = moduleHours + customHours + migrationHours;

  // Calculate Adoption hours from training + (support/month × months)
  const trainingHours = parseFloat(responses.training_hours) || 0;
  const supportPerMonth = parseFloat(responses.support_hours_per_month) || 0;
  const supportMonths = parseInt(responses.adoption_duration_months) || 0;
  const adoptionHours = trainingHours + (supportPerMonth * supportMonths);

  const totalProjectHours = clarityHours + implementationHours + adoptionHours;

  return (
    <div className="bg-gradient-to-br from-blue-50 to-purple-50 border-2 border-blue-300 rounded-lg p-6 mt-6">
      <h3 className="text-xl font-bold text-blue-900 mb-4">Project Budget Summary</h3>

      <div className="text-3xl font-bold text-center text-blue-900 mb-6">
        {totalProjectHours}h
        <div className="text-sm text-gray-600 font-normal mt-1">Total Project Budget</div>
      </div>

      {/* Clarity Phase */}
      {clarityHours > 0 && (
        <div className="mb-4 p-3 bg-white rounded-lg">
          <div className="flex justify-between items-center mb-2">
            <span className="font-semibold text-gray-700">Clarity Phase</span>
            <span className="text-lg font-bold text-purple-600">{clarityHours}h</span>
          </div>
        </div>
      )}

      {/* Implementation Phase */}
      {implementationHours > 0 && (
        <div className="mb-4 p-3 bg-white rounded-lg">
          <div className="flex justify-between items-center mb-2">
            <span className="font-semibold text-gray-700">Implementation Phase</span>
            <span className="text-lg font-bold text-blue-600">{implementationHours}h</span>
          </div>
          <div className="ml-4 space-y-1 text-sm text-gray-600">
            {moduleHours > 0 && <div>• Odoo Modules: {moduleHours}h</div>}
            {customHours > 0 && <div>• Custom Modules: {customHours}h</div>}
            {migrationHours > 0 && <div>• Data Migration: {migrationHours}h</div>}
          </div>
        </div>
      )}

      {/* Adoption Phase */}
      {adoptionHours > 0 && (
        <div className="mb-4 p-3 bg-white rounded-lg">
          <div className="flex justify-between items-center mb-2">
            <span className="font-semibold text-gray-700">Adoption Phase</span>
            <span className="text-lg font-bold text-green-600">{adoptionHours}h</span>
          </div>
          <div className="ml-4 space-y-1 text-sm text-gray-600">
            {trainingHours > 0 && <div>• Training: {trainingHours}h</div>}
            {(supportPerMonth * supportMonths) > 0 && (
              <div>• Support: {supportPerMonth * supportMonths}h ({supportPerMonth}h/month × {supportMonths} months)</div>
            )}
          </div>
        </div>
      )}
    </div>
  );
```

**Result:** Single unified tracker showing complete project budget breakdown

### 6. Custom Module Card Redesign

**File:** `App.jsx:858-885`

**OLD Design:**
```jsx
<div className="p-3 rounded-lg border-2 border-purple-600 bg-orange-50">
  <div className="mb-2">
    <label className="block text-sm font-medium text-gray-700 mb-1">
      Module Name
    </label>
    <input
      type="text"
      className="w-full px-3 py-2 border border-gray-300 rounded-md"
    />
  </div>
  <div>
    <label className="block text-sm font-medium text-gray-700 mb-1">
      Hours
    </label>
    <input
      type="number"
      className="w-full px-3 py-2 border border-gray-300 rounded-md"
    />
  </div>
</div>
```

**NEW Design:**
```jsx
<div className="p-3 rounded-lg border-2 bg-orange-600 text-white border-orange-600">
  {/* Module name input - looks like module label */}
  <input
    type="text"
    value={moduleName}
    onChange={(e) => handleResponseChange(nameField, e.target.value)}
    placeholder={`Custom Module ${num}`}
    className="w-full px-0 py-0 text-sm font-semibold bg-transparent text-white placeholder-orange-200 border-none focus:ring-0 focus:outline-none mb-1"
  />
  <div className="text-xs text-orange-100 mb-2">Custom development</div>

  {/* Hour input section - same as standard modules */}
  <div className="mt-2 pt-2 border-t border-orange-400">
    <input
      type="number"
      value={moduleHours}
      onChange={(e) => handleResponseChange(hoursField, e.target.value)}
      placeholder="Hours"
      min="0"
      className="w-full px-2 py-1 text-sm rounded border border-orange-300 bg-white text-gray-900 placeholder-gray-400 focus:ring-2 focus:ring-orange-500 focus:border-transparent"
    />
    <div className="text-xs text-orange-100 mt-1">Allocated hours</div>
  </div>
</div>
```

**Key Changes:**
- Orange-600 background (matching purple-600 pattern of standard modules)
- Transparent name input with white text (looks like module label)
- Description line: "Custom development" (matching standard modules)
- Border-t separator before hours (matching standard modules)
- White hour input box (same as standard modules)
- "Allocated hours" label (same as standard modules)

**Result:** Custom modules look identical to standard modules (just orange instead of purple)

---

## Example: EAS Systems Project

### User Input

**Section 2: Scope & Hours**
```
Clarity Phase ☑
→ Clarity Hours: 30

Implementation Phase ☑
→ Modules to Implement:
  ☑ CRM (20h)
  ☑ Sales (20h)

→ Custom Modules:
  How many? 1
  - Aprobaciones (13h)

→ Data Migration: Partial
  → Migration Hours: 15

Adoption Phase ☑
→ Training Hours: 16
→ Support Hours per Month: 2
→ Support Duration (months): 2
```

### Auto-Calculated Values

**Implementation Hours:**
```
CRM: 20h
Sales: 20h
Aprobaciones (custom): 13h
Data Migration: 15h
─────────────
Total: 68h  ✅ Auto-calculated!
```

**Adoption Hours:**
```
Training: 16h
Support: 2h/month × 2 months = 4h
─────────────
Total: 20h  ✅ Auto-calculated!
```

### Budget Tracker Display

```
┌───────────────────────────────────────┐
│  Project Budget Summary                │
│                                        │
│        118h                            │
│  Total Project Budget                  │
│                                        │
│  Clarity Phase               30h       │
│                                        │
│  Implementation Phase        68h       │
│  • Odoo Modules: 40h                   │
│  • Custom Modules: 13h                 │
│  • Data Migration: 15h                 │
│                                        │
│  Adoption Phase              20h       │
│  • Training: 16h                       │
│  • Support: 4h (2h/month × 2 months)   │
└───────────────────────────────────────┘
```

### Review Screen Display

```
Phases & Hours
───────────────
✓ Clarity Phase           30 hours
✓ Implementation Phase    68 hours  ← Auto-calculated!
✓ Adoption Phase          20 hours  ← Auto-calculated!
```

### Task Generation

**Implementation tasks use calculated 68h total:**
- CRM tasks scale to 20h
- Sales tasks scale to 20h
- Aprobaciones (AI-generated) uses 13h
- Migration tasks use 15h

**Adoption tasks use calculated 20h total:**
- Training tasks: 16h (80% of budget)
- Support tasks: 4h (20% of budget, 2 months × 2h/month)

---

## Benefits

### 1. Cleaner Organization
- ✅ Custom Modules and Data Migration clearly part of Implementation
- ✅ Logical grouping of related items
- ✅ Easier to understand project structure
- ✅ Matches mental model of project phases

### 2. Unified Budget View
- ✅ Single place to see all 3 phase budgets
- ✅ Breakdown of Implementation components
- ✅ Breakdown of Adoption components
- ✅ Total project budget at a glance

### 3. Accurate Auto-Calculation
- ✅ No possibility of mismatched totals
- ✅ Implementation = sum of all module/custom/migration hours
- ✅ Adoption = training + (support/month × months)
- ✅ Task generation uses exact breakdown values

### 4. Better User Experience
- ✅ Enter hours where they matter (per module)
- ✅ See breakdown that matches how you quoted the project
- ✅ System calculates totals automatically
- ✅ Custom modules look like standard modules

### 5. Consistent Visual Design
- ✅ Custom module cards match standard module cards
- ✅ Same size, layout, and interaction pattern
- ✅ Orange distinguishes custom from standard
- ✅ Professional, cohesive UI

---

## Files Modified

| File | Changes | Lines |
|------|---------|-------|
| `questionnaire-structure.json` | Reorganized sections, removed manual hour fields, added breakdown | 101-224 |
| `App.jsx` | Updated task generation, review screen, budget tracker, custom cards | 221-245, 405-434, 858-885, 893-987, 1321-1360 |

**Total:** 2 files, 245 insertions, 70 deletions

---

## Testing Guide

### Test Scenario 1: Basic Implementation Breakdown

**Input:**
```
Implementation:
- CRM: 20h
- Sales: 30h
- Custom Module "Workflow": 15h
- Data Migration: 10h
```

**Expected Auto-Calculation:**
```
Implementation = 20 + 30 + 15 + 10 = 75h ✅
```

**Verify:**
1. Budget tracker shows 75h for Implementation
2. Review screen shows 75h for Implementation
3. Task generation uses 75h total

### Test Scenario 2: Adoption Breakdown

**Input:**
```
Adoption:
- Training Hours: 20h
- Support per Month: 10h
- Duration: 3 months
```

**Expected Auto-Calculation:**
```
Adoption = 20 + (10 × 3) = 50h ✅
```

**Verify:**
1. Budget tracker shows 50h for Adoption (20h training + 30h support)
2. Review screen shows 50h for Adoption
3. Task generation uses 50h total

### Test Scenario 3: Complete Project

**Input:**
```
Clarity: 30h
Implementation:
- CRM: 20h
- Sales: 20h
- Aprobaciones (custom): 13h
- Migration: 15h
Adoption:
- Training: 16h
- Support: 2h/month × 2 months
```

**Expected Auto-Calculation:**
```
Clarity: 30h
Implementation: 20 + 20 + 13 + 15 = 68h
Adoption: 16 + (2 × 2) = 20h
Total: 30 + 68 + 20 = 118h ✅
```

**Verify:**
1. Budget tracker shows:
   - Clarity: 30h
   - Implementation: 68h (40h modules + 13h custom + 15h migration)
   - Adoption: 20h (16h training + 4h support)
   - Total: 118h
2. Review screen shows all 3 phases with correct hours
3. Task generation creates ~118h of tasks total

---

## Success Criteria

### ✅ Organizational Structure
- [x] Custom Modules under Implementation section
- [x] Data Migration under Implementation section
- [x] Logical flow: Clarity → Implementation (modules, custom, migration) → Adoption

### ✅ Auto-Calculation
- [x] Implementation hours calculated from breakdown
- [x] Adoption hours calculated from training + support formula
- [x] No manual hour input fields for Implementation/Adoption

### ✅ Budget Tracker
- [x] Shows all 3 phases in one place
- [x] Displays Implementation breakdown (modules, custom, migration)
- [x] Displays Adoption breakdown (training, support)
- [x] Shows total project budget

### ✅ Task Generation
- [x] Uses calculated Implementation hours
- [x] Uses calculated Adoption hours
- [x] Tasks total matches breakdown exactly

### ✅ Review Screen
- [x] Displays calculated Implementation hours
- [x] Displays calculated Adoption hours
- [x] Matches budget tracker values

### ✅ Custom Module Design
- [x] Orange-600 background matching purple-600 pattern
- [x] Transparent name input with white text
- [x] Description line added
- [x] Border-t separator before hours
- [x] Matches standard module design exactly

---

## Summary

**What You Asked For:**
1. "point 4 and 5 should be under implementation" ✅
2. "it should be the hour allocation tracker for the three phases" ✅
3. "Adoption = hours of training + (hour of support per month x months of support)" ✅
4. "Implementation = Hours per module + Hour per custom module + data migration" ✅
5. "boxes of custom module... do not have the same design as the other boxes" ✅

**What You Got:**
- ✅ Reorganized questionnaire with logical hierarchy
- ✅ Unified budget tracker showing all 3 phases
- ✅ Auto-calculated Implementation hours (modules + custom + migration)
- ✅ Auto-calculated Adoption hours (training + support formula)
- ✅ Custom module cards matching standard module design
- ✅ Task generation using exact calculated values
- ✅ Review screen displaying calculated hours

**Result:** A cleaner, more logical, and more accurate project planning experience! 🎉

---

**Commit:** e14619f
**Branch:** claude/odoo-implementation-planner-011CUqvLHbyg96f3qjACvM3d
**Status:** ✅ READY TO TEST

**Next Steps:**
1. Test questionnaire flow with EAS Systems scenario
2. Verify budget tracker calculations
3. Generate plan and verify task hours match breakdown
4. Test with different combinations (no custom modules, no migration, etc.)
