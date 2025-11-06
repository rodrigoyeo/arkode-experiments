# Odoo 19 Project & Task Import Format Reference

## Analysis Date: 2025-11-06

Based on actual exports from Odoo 19 instance, this document defines the CSV import format for the Odoo Implementation Planner tool.

---

## 📊 Export vs Import: Key Difference

**IMPORTANT:** The files you exported contain **42 project fields** and **45 task fields**, but most are **read-only/computed fields** (like ratings, progress %, activities, etc.).

For **importing**, we only need the **essential user-editable fields**.

---

## 📋 Project Import Fields (project.project)

### Required Fields
| Field Name | Type | Example | Notes |
|------------|------|---------|-------|
| `Display Name` or `name` | Text | "AGR - S00066 - Sales Order" | Project title (REQUIRED) |

### Recommended Fields
| Field Name | Type | Example | Notes |
|------------|------|---------|-------|
| `Customer` | Many2one | "Deco Addict" | Client name (reference or External ID) |
| `Project Manager` | Many2one | "Mitchell Admin" | User name (reference) |
| `Start Date` | Date | "2025-10-20 00:00:00" | Format: YYYY-MM-DD HH:MM:SS |
| `Expiration Date` | Date | "2025-12-15 00:00:00" | Project deadline |
| `Allocated Time` | Float | 85 | Total hours budgeted |
| `Tags` | Many2many | "Construction,Design,Interior" | Comma-separated tags |
| `Use Tasks as` | Selection | "Tasks" or "Services" | Task type |

---

## ✅ Task Import Fields (project.task)

### Required Fields
| Field Name | Type | Example | Notes |
|------------|------|---------|-------|
| `Title` | Text | "Energy Certificate" | Task name (REQUIRED) |
| `Project` | Many2one | "Office Design" | Project reference (REQUIRED) |

### Highly Recommended Fields
| Field Name | Type | Example | Notes |
|------------|------|---------|-------|
| `Assignees` | Many2one | "Mitchell Admin" | User assigned to task |
| `Allocated Time` | Float | 15 | Estimated hours |
| `Deadline` | Date | "2025-11-07 20:04:17" | Task due date (YYYY-MM-DD) |
| `Stage` | Many2one | "New", "In Progress", "Done" | Task stage/status |
| `Priority` | Selection | "Low priority", "Medium priority", "High priority" | Task priority |
| `Tags` | Many2many | "New Feature,Bug" | Comma-separated tags |

### Optional But Useful Fields
| Field Name | Type | Example | Notes |
|------------|------|---------|-------|
| `Milestone` | Many2one | "Final Phase - 11/06/2027" | Link to milestone |
| `Parent Task` | Many2one | "Customer Meeting" | For creating subtasks |
| `Customer` | Many2one | "YourCompany, Joel Willis" | Client contact |
| `Start date` | Date | "2025-11-11 20:04:17" | Task start date |

---

## 🎯 Minimal CSV Template for Task Import

Based on the analysis, here's the **minimal viable CSV** for importing tasks:

```csv
Title,Project,Assignees,Allocated Time,Deadline,Stage,Priority,Tags
"Clarity: Process Mapping Workshop","Client XYZ Implementation","Mitchell Admin",8,2025-02-15,"New","Medium priority","Clarity"
"Implementation: CRM Setup","Client XYZ Implementation","Marc Demo",12,2025-03-01,"New","High priority","Implementation,CRM"
"Adoption: End-User Training","Client XYZ Implementation","Mitchell Admin",16,2025-04-01,"New","Medium priority","Adoption,Training"
```

---

## 📝 Observations from Your Export

### Projects in Your System
Your export shows diverse project types:
- **Sales-linked projects**: "AGR - S00066 - Sales Order", "DECO - S00068 - Sales Order"
- **Service projects**: "After-Sales Services", "Field Service"
- **Construction projects**: "Home Construction", "Office Design", "Renovations"
- **Internal projects**: "Research & Development"

### Tasks in Your System
Common task patterns:
- **Hierarchical tasks**: Parent tasks with subtasks (e.g., "Customer Meeting" → "Preparation", "Daily Meetings summary", "Minutes")
- **Milestone-linked**: Tasks grouped under milestones like "Final Phase - 11/06/2027"
- **Time tracking enabled**: Most tasks have "Allocated Time" and "Time Remaining"
- **Tagged categorization**: Tags like "New Feature", "Bug", "External", "Internal"

---

## 🔧 Import Format Guidelines

### Text Format
- **Dates**: Use `YYYY-MM-DD HH:MM:SS` or `YYYY-MM-DD`
- **Booleans**: Use `True`/`False`
- **Numbers**: Plain numbers (no currency symbols)
- **References** (Many2one): Use display name as shown in Odoo UI
- **Multiple values** (Many2many): Comma-separated (e.g., "Tag1,Tag2,Tag3")

### External IDs (Advanced)
For programmatic imports, you can use External IDs:
- `Project/id`: `__export__.project_project_17_c6738c29`
- `Assignees/id`: `base.user_admin`
- `Stage/id`: `project.project_stage_0`

**For your tool**, we'll use **display names** (simpler for non-technical users).

---

## 🚀 Recommended CSV Structure for Planner Tool

Our tool will generate CSV with these columns:

```csv
Title,Project,Assignees,Allocated Time,Deadline,Stage,Priority,Tags,Milestone,Parent Task
```

This provides:
- ✅ All required fields
- ✅ Enough detail for project management
- ✅ Simple format (no complex External IDs)
- ✅ Compatible with Odoo 19 import

---

## 📌 Next Steps

1. ✅ **Format validated** - We know the exact structure Odoo expects
2. 🔄 **Create task library** - Build templates for Clarity/Implementation/Adoption phases
3. 🔄 **Build questionnaire** - Design questions to gather project details
4. 🔄 **Implement CSV export** - Generate files in this exact format
5. 🔄 **Test import** - Validate with your Odoo instance

---

*This reference document will guide the development of the Odoo Implementation Planner tool.*
