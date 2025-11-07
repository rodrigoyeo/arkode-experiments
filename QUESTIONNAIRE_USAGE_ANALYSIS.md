# Questionnaire Questions - Usage Analysis

This document shows **exactly how each question is used** in plan generation, and which questions are currently **NOT being used** (opportunities for improvement!).

---

## ✅ Questions ACTIVELY USED in Plan Generation

### Project Information Section

| Question | Field ID | How It's Used | Code Location |
|----------|----------|---------------|---------------|
| **Project Name** | `project_name` | Used as CSV filename and project title in exports | App.jsx:680 |
| **Client Name** | `client_name` | Used in task descriptions and AI prompts | App.jsx:548 |
| **Language** | `language` | Controls task names (uses `name_es` vs `name`) and milestone names | App.jsx:230, 371, 502 |
| **Project Manager** | `project_manager` | Assigns PM to project management tasks | App.jsx:256 |
| **Team Members** | `team_members` | Assigns tasks based on roles (Process Consultant, Developer) | App.jsx:256 (assignTaskToTeamMember) |
| **Project Start Date** | `project_start_date` | Calculates all task start/due dates and milestones | App.jsx:206-208, 234-235 |
| **Enable AI** | `enable_ai_customization` | Determines whether AI generates custom tasks | App.jsx:227, 536-649 |

---

### Scope & Hours Section

| Question | Field ID | How It's Used | Code Location |
|----------|----------|---------------|---------------|
| **Clarity Phase Hours** | `clarity_hours` | **Scales ALL Clarity tasks proportionally** | App.jsx:222-229 |
| **Modules to Implement** | `modules` | **Filters which module tasks to include** from task-library.json | App.jsx:338-402 |
| **Custom Modules** | `custom_modules_visual` | **Generates 8 implementation tasks per custom module** (Requirements, Design, Dev, Testing, etc.) | App.jsx:407-461 |
| **Data Migration** | `data_migration` | Determines if migration tasks are included | App.jsx:183-200 |
| **Training Hours** | `training_hours` | Scales training phase tasks | App.jsx:489 |

---

### Discovery & Requirements Section (Clarity Details)

| Question | Field ID | How It's Used | Code Location |
|----------|----------|---------------|---------------|
| **Current Systems** | `current_systems` | **Sent to AI** to generate integration assessment tasks | App.jsx:550 |
| **Pain Points** | `pain_points` | **Sent to AI** to generate problem-solving tasks | App.jsx:551 |

---

### Technical Implementation Details Section

| Question | Field ID | How It's Used | Code Location |
|----------|----------|---------------|---------------|
| **Customization Scope** | `customization_scope` | **Sent to AI** to generate custom module implementation tasks | App.jsx:554, 582 |
| **Integration List** | `integration_list` | **Sent to AI** to generate integration tasks | App.jsx:584 |

---

## ❌ Questions NOT Currently Used (Wasted Opportunities!)

These questions are asked but **NOT used anywhere** in the code:

### Project Information - NOT USED

| Question | Field ID | Why It Could Be Useful |
|----------|----------|------------------------|
| **Industry** | `industry` | Could tailor AI prompts (e.g., "Manufacturing" → MRP focus, "Retail" → POS focus) |
| **Country** | `country` | Could affect localization tasks (tax setup, accounting standards) |
| **Project Duration** | `project_duration_weeks` | Could distribute tasks across timeline |
| **Project Deadline** | `project_deadline` | Could adjust task scheduling to meet deadline |

---

### Discovery & Requirements - NOT USED

| Question | Field ID | Why It Could Be Useful |
|----------|----------|------------------------|
| **Core Business Processes** ⚠️ | `core_processes` | **Should be used!** Could generate process-specific mapping tasks |
| **Workshop Count** | `workshop_count` | Could multiply workshop/interview tasks |

**Example Issue:** User enters "Quote-to-Cash, Procure-to-Pay, Make-to-Order" as core processes → System does NOTHING with this info!

**What Should Happen:**
- Generate "Quote-to-Cash Process Mapping" task (12 hours)
- Generate "Procure-to-Pay Process Mapping" task (10 hours)
- Generate "Make-to-Order Process Design Workshop" task (8 hours)

---

### Technical Implementation - NOT USED

| Question | Field ID | Why It Could Be Useful |
|----------|----------|------------------------|
| **Data Migration Scope** | `data_migration_scope` | Could generate data-specific migration tasks (customers, products, financials) |
| **Multi-company** | `multi_company` | Could add multi-company configuration tasks |
| **Company Count** | `company_count` | Could scale company setup tasks |
| **Multi-warehouse** | `multi_warehouse` | Could add warehouse configuration tasks |
| **Warehouse Count** | `warehouse_count` | Could scale warehouse setup tasks |

**Example Issue:** User selects "Yes" to multi-warehouse with 5 warehouses → System does NOTHING!

**What Should Happen:**
- Add "Warehouse Configuration - Warehouse 1" (3 hours)
- Add "Warehouse Configuration - Warehouse 2" (3 hours)
- ... (5 total tasks = 15 hours)
- Add "Inter-warehouse Transfer Configuration" (4 hours)

---

### Training & Support - NOT USED

| Question | Field ID | Why It Could Be Useful |
|----------|----------|------------------------|
| **Total Number of Users** ⚠️ | `user_count` | **Should scale training hours!** More users = more training sessions |
| **User Breakdown** ⚠️ | `user_breakdown` | **Should generate role-specific training!** |
| **Training Format** | `training_format` | Could affect task names/descriptions (on-site vs remote) |
| **Go-Live Support Days** | `golive_support_days` | Could scale go-live support task hours |

**Example Issue:** User enters "Ventas (5), Operaciones (5), Servicios (3), Admin (2)" → System does NOTHING!

**What Should Happen:**
- Generate "Sales Team Training (5 users)" (6 hours)
- Generate "Operations Team Training (5 users)" (6 hours)
- Generate "Services Team Training (3 users)" (4 hours)
- Generate "Admin Training (2 users)" (3 hours)

---

## Why These Questions Matter

### 1. Core Business Processes - CRITICAL MISS! 🚨

**Current Behavior:**
- System generates generic "Process Mapping" task (12 hours)
- User writes "Quote-to-Cash, Procure-to-Pay, Make-to-Order" → Ignored

**Better Behavior:**
```javascript
// Parse core_processes and create specific tasks
if (responses.core_processes) {
  const processes = responses.core_processes.split(',').map(p => p.trim());

  processes.forEach(processName => {
    plan.tasks.push({
      title: `${processName} - Current State Mapping`,
      allocated_hours: 8,
      milestone: 'Process Mapping',
      ...
    });

    plan.tasks.push({
      title: `${processName} - Future State Design`,
      allocated_hours: 6,
      milestone: 'Process Design',
      ...
    });
  });
}
```

**Impact:** User gets specific, relevant tasks instead of generic ones!

---

### 2. User Breakdown by Role - CRITICAL MISS! 🚨

**Current Behavior:**
- Generic "User Training - End Users" (8 hours)
- User writes detailed breakdown → Ignored

**Better Behavior:**
```javascript
// Parse user_breakdown and create role-specific training
if (responses.user_breakdown) {
  // Parse "Ventas (5), Operaciones (5), Servicios (3)"
  const rolePattern = /([^(]+)\((\d+)\)/g;
  const roles = [...responses.user_breakdown.matchAll(rolePattern)];

  roles.forEach(([, roleName, userCount]) => {
    const hours = Math.ceil(parseInt(userCount) / 3) * 2; // 2 hours per 3 users

    plan.tasks.push({
      title: `${roleName.trim()} Training (${userCount} users)`,
      allocated_hours: hours,
      milestone: 'Training',
      ...
    });
  });
}
```

**Impact:** Specific training tasks for each department!

---

### 3. Total Number of Users - Scaling Factor

**Current Behavior:**
- Training tasks have fixed hours regardless of 5 users or 50 users

**Better Behavior:**
```javascript
// Scale training based on user count
if (responses.user_count) {
  const userCount = parseInt(responses.user_count);
  const trainingMultiplier = userCount > 20 ? 1.5 : userCount > 10 ? 1.2 : 1.0;

  // Apply multiplier to all training tasks
  trainingTasks.forEach(task => {
    task.allocated_hours = Math.round(task.allocated_hours * trainingMultiplier);
  });
}
```

**Impact:** Realistic training hours!

---

### 4. Industry - AI Context Enhancement

**Current Behavior:**
- AI prompts don't mention industry
- Generic tasks regardless of Manufacturing vs Healthcare

**Better Behavior:**
```javascript
// Enhance AI prompts with industry context
const aiPrompt = `
Generate tasks for a ${responses.industry} company implementing Odoo.

Industry-specific considerations:
${responses.industry === 'Manufacturing' ? '- MRP and production planning\n- Bill of materials\n- Work orders' : ''}
${responses.industry === 'Healthcare' ? '- HIPAA compliance\n- Patient records\n- Appointment scheduling' : ''}
...
`;
```

**Impact:** AI generates industry-relevant tasks!

---

### 5. Multi-warehouse - Configuration Tasks

**Current Behavior:**
- User selects 5 warehouses → No warehouse-specific tasks generated

**Better Behavior:**
```javascript
if (responses.multi_warehouse === 'Yes' && responses.warehouse_count > 1) {
  const warehouseCount = parseInt(responses.warehouse_count);

  // Add setup task for each warehouse
  for (let i = 1; i <= warehouseCount; i++) {
    plan.tasks.push({
      title: `Warehouse ${i} - Configuration`,
      allocated_hours: 3,
      milestone: 'Inventory Implementation',
      ...
    });
  }

  // Add inter-warehouse transfer setup
  plan.tasks.push({
    title: 'Inter-warehouse Transfer Routes Configuration',
    allocated_hours: warehouseCount * 2,
    milestone: 'Inventory Implementation',
    ...
  });
}
```

**Impact:** Realistic warehouse setup scope!

---

## Summary: Impact Assessment

### High-Impact Missed Opportunities (Should Implement!)

1. **Core Business Processes** → Generate process-specific mapping tasks
2. **User Breakdown** → Generate role-specific training tasks
3. **User Count** → Scale training hours
4. **Multi-warehouse** → Generate warehouse configuration tasks
5. **Industry** → Enhance AI prompts with industry context

### Medium-Impact (Nice to Have)

6. **Data Migration Scope** → Generate data-type-specific migration tasks
7. **Workshop Count** → Multiply workshop tasks
8. **Multi-company** → Add company setup tasks
9. **Go-live Support Days** → Scale support task hours

### Low-Impact (Optional)

10. **Training Format** → Adjust task descriptions
11. **Country** → Add localization tasks
12. **Project Duration** → Distribute tasks across timeline

---

## Recommendations

### Priority 1: Implement These NOW

1. **Use `core_processes`** to generate specific process mapping tasks
2. **Use `user_breakdown`** to generate role-based training tasks
3. **Use `user_count`** to scale training hours

**Why:** Users take time to answer these questions - we should USE them!

### Priority 2: Enhance AI Prompts

4. **Use `industry`** in AI prompts for industry-specific tasks
5. **Use `data_migration_scope`** in AI prompts for migration tasks
6. **Use `multi_warehouse` + `warehouse_count`** for configuration tasks

### Priority 3: Future Enhancements

7. Use `project_duration_weeks` to spread tasks across timeline
8. Use `country` for localization considerations

---

## Current Questions That ARE Used Well

These questions have good integration:

✅ **`language`** - Perfectly controls Spanish vs English output
✅ **`modules`** - Directly filters task library
✅ **`custom_modules`** - Generates structured custom dev tasks
✅ **`team_members`** - Intelligent role-based assignment
✅ **`current_systems`** - Sent to AI for integration tasks
✅ **`pain_points`** - Sent to AI for problem-solving tasks
✅ **`customization_scope`** - Sent to AI for custom module details

These are working great! Keep them.

---

## Next Steps

1. **Implement high-priority question usage** (core_processes, user_breakdown, user_count)
2. **Enhance AI prompts** with industry, multi-warehouse, etc.
3. **Consider removing unused questions** OR implement them properly

Don't waste the user's time asking questions you won't use!
