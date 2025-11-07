# Testing Guide - Questionnaire v2

## 🎯 What Changed

### Fixed Issues:
1. ✅ **Hour allocation fields now visible** - Added >=, <=, >, < operator support
2. ✅ **Improved UX** - Reduced from 9 sections to 6 logical sections
3. ✅ **Multiple custom modules** - Support up to 10 modules with individual names/hours
4. ✅ **Integrated hour allocation** - No longer buried in "Advanced" section

## 📋 New Questionnaire Structure

### 6 Sections (down from 9):

1. **Project Information** - Basic details (name, industry, country, language)
2. **Scope & Hours** - Phases, hours, custom modules, Odoo modules ⭐ NEW!
3. **Discovery & Requirements** - Clarity phase details
4. **Technical Implementation Details** - Data migration, integrations, customizations
5. **Training & Support** - Adoption phase details
6. **Additional Information** - Optional notes

## 🧪 Test Scenario: EAS Systems Project

### Step 1: Fill Project Information

```
Section 1: Project Information
━━━━━━━━━━━━━━━━━━━━━━━━━━━
Project Name: EAS Systems Odoo Implementation
Industry: Professional Services
Country: Mexico
Language: Spanish
```

### Step 2: Fill Scope & Hours (⭐ THIS IS THE KEY SECTION!)

```
Section 2: Scope & Hours
━━━━━━━━━━━━━━━━━━━━━━━━━━━

Which phases are included?
☑ Clarity Phase (Discovery & Requirements)
  → Clarity Phase Hours: 65

☑ Implementation Phase (System Build)
  → Implementation Phase Hours: 165

  How many custom modules do you need? 2

  ⭐ WATCH: These fields should appear dynamically:

  → Custom Module 1 Name: I+D Module
  → Custom Module 1 Hours: 80

  → Custom Module 2 Name: Custom Workflow
  → Custom Module 2 Hours: 40

  Which Odoo modules? (select these)
  ☑ CRM
  ☑ Sales
  ☑ Purchase
  ☑ Inventory
  ☑ Accounting
  ☑ Projects
  ☑ Field Service (FSM)
  ☑ Expenses

☑ Adoption Phase (Training & Go-Live)
  → Adoption Phase Hours: 40
```

### Step 3: Fill Discovery & Requirements

```
Section 3: Discovery & Requirements
━━━━━━━━━━━━━━━━━━━━━━━━━━━
Current systems: ASPEL (SAE y COI)
Pain points: Sistemas desconectados, reportes manuales
Core processes: Ventas, compras, inventario, contabilidad
```

### Step 4: Fill Technical Implementation Details

```
Section 4: Technical Implementation Details
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Data Migration Needed? Yes
  → Scope: Migración de clientes, proveedores, productos, inventario desde ASPEL

Integrations Required? No

Custom Development Required:
Módulo de I+D para gestión de productos con especificaciones técnicas (SKU, fabricante, especificaciones, integración con compras)

Custom Workflow para seguimiento de proyectos de innovación

Multi-company: No
Multi-warehouse: No
```

### Step 5: Fill Training & Support

```
Section 5: Training & Support
━━━━━━━━━━━━━━━━━━━━━━━━━━━
User Count: 25
User Breakdown: 5 ventas, 3 compras, 2 contabilidad, 15 operativos
Training Format: Hybrid (presencial + virtual)
```

### Step 6: Generate Plan

Click **"Generate Implementation Plan"**

## ✅ Expected Results

### Task Count:
- **OLD:** 145 tasks | 424h
- **NEW:** ~80-90 tasks | ~270-280h ✅

### Task Breakdown:
```
Clarity Phase: ~16 tasks | ~65h
  - 13 native template tasks
  - 3 AI-generated discovery tasks

Implementation Phase: ~50-60 tasks | ~165h
  - Module configuration tasks (CRM, Sales, Purchase, etc.)
  - 4-6 AI tasks for "I+D Module" (80h)
  - 3-5 AI tasks for "Custom Workflow" (40h)
  - Migration tasks (ASPEL)

Adoption Phase: ~11 tasks | ~40h
  - Training tasks
  - Go-live support
```

### Task Names Should Be Specific:
✅ "Módulo de I+D - Diseño de estructura de base de datos" (16h)
✅ "Módulo de I+D - Configuración de campos RFID" (12h)
✅ "Módulo de I+D - Integración con compras" (20h)
✅ "Custom Workflow - Diseño de flujo de proyectos" (15h)
✅ "Migración de datos desde ASPEL (SAE y COI) a Odoo" (30h)

❌ NOT: "Desarrollo de módulo personalizado"
❌ NOT: "Configuración de campo específico"

### Milestones Should Be Module-Based:
```
Clarity Phase:
1. Mapeo de Procesos (2 semanas)
2. Hallazgos, Oportunidades y TO-BE (1 semana)
3. Master of Implementation (1 semana)

Implementation Phase:
4. Implementación del módulo de CRM
5. Implementación del módulo de Sales
6. Implementación del módulo de Purchase
7. Implementación del módulo de Inventory
8. Implementación del módulo de Accounting
9. Implementación del módulo de Projects
10. Implementación del módulo de FSM
11. Implementación del módulo de Expenses
12. Implementación de Módulos Personalizados (I+D Module, Custom Workflow)
13. Migración de Datos (ASPEL)

Adoption Phase:
14. Capacitación y Go-Live
```

## 🔍 What to Check in Console

### AI Prompt:
```
📋 AI PROMPT FOR IMPLEMENTATION PHASE:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Custom Modules to Develop:
1. I+D Module (80h allocated)
2. Custom Workflow (40h allocated)

Detailed Requirements:
Módulo de I+D para gestión de productos con especificaciones técnicas...
Custom Workflow para seguimiento de proyectos de innovación

CRITICAL - Task naming requirements:
- Use the EXACT module names provided above
- Break down each custom module into 3-5 subtasks
```

### Duplication Check:
```
✅ Should see: "Skipping hardcoded custom dev template (AI will generate custom tasks)"
❌ Should NOT see: Both hardcoded AND AI custom dev tasks
```

## 🐛 Troubleshooting

### If Custom Module Fields Don't Appear:
1. Check that "How many custom modules do you need?" has value >= 1
2. Check browser console for errors
3. Verify questionnaire-structure.json loaded correctly

### If Hours Explode:
1. Check that hardcoded custom dev template is skipped (see console)
2. Verify AI tasks total to ~120h (80h + 40h), not 221h
3. Check that module tasks distribute evenly among selected modules

### If Task Names Are Generic:
1. Check AI prompt in console - should show "I+D Module (80h allocated)"
2. Verify AI response includes specific module names
3. Check that module names were entered correctly in questionnaire

## 📊 Success Criteria

### ✅ Pass if:
- [ ] Custom module fields appear when count >= 1
- [ ] Total hours ~270-280h (within 10% of input)
- [ ] Task names include "I+D Module" and "Custom Workflow"
- [ ] Milestones show "Implementación del módulo de CRM", etc.
- [ ] Console shows "Skipping hardcoded custom dev template"
- [ ] No duplication between hardcoded and AI tasks

### ❌ Fail if:
- [ ] Custom module fields don't appear
- [ ] Total hours > 300h (still exploding)
- [ ] Task names are generic ("Desarrollo de módulo personalizado")
- [ ] Milestones show "Clarity Phase Complete"
- [ ] Console shows both hardcoded AND AI custom dev tasks

## 🚀 How to Run Test

```bash
# 1. Pull latest changes
git pull origin claude/odoo-implementation-planner-011CUqvLHbyg96f3qjACvM3d

# 2. Install dependencies (if needed)
cd odoo-planner-app
npm install

# 3. Start dev server
npm run dev

# 4. Open browser
http://localhost:3000

# 5. Fill questionnaire with EAS Systems data (see above)

# 6. Check console output for AI prompts and task generation

# 7. Export CSV and verify in Odoo 19
```

## 📝 Additional Test Cases

### Test Case 1: No Custom Modules
```
Custom modules: 0
Expected: No custom module fields visible
Expected: Standard module tasks only
```

### Test Case 2: Single Custom Module
```
Custom modules: 1
  Module 1: "I+D Module" (80h)
Expected: Fields for Module 1 only
Expected: AI generates 4-6 tasks for I+D Module
```

### Test Case 3: Three Custom Modules
```
Custom modules: 3
  Module 1: "I+D Module" (80h)
  Module 2: "Custom Workflow" (40h)
  Module 3: "Reporting Dashboard" (30h)
Expected: Fields for all 3 modules
Expected: AI generates tasks for all 3, total ~150h
```

### Test Case 4: Spanish vs English
```
Language: Spanish
Expected: Task names in Spanish ("Módulo de I+D - ...")
Expected: Milestones in Spanish ("Implementación del módulo de...")
```

---

**Latest Commit:** 332115c (questionnaire v2)
**Branch:** claude/odoo-implementation-planner-011CUqvLHbyg96f3qjACvM3d
**Status:** Ready for testing! 🚀
