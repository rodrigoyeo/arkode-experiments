# Odoo Implementation Planner

A project planning tool for Arkode's Odoo implementation projects. This application generates structured project plans with task breakdowns for Clarity, Implementation, and Adoption phases, ready to import into Odoo 19's Project module.

## Features

- **Interactive Questionnaire**: 10-15 minute guided questionnaire to capture project details
- **Smart Task Generation**: Automatically generates tasks based on selected modules and phases
- **Odoo 19 Grounded**: All implementation tasks reference real Odoo 19 features and capabilities
- **Hour Allocation**: Distributes quoted hours intelligently across tasks
- **CSV Export**: Generates Odoo-compatible CSV files for direct import
- **10 Module Support**: CRM, Sales, Purchase, Inventory, Manufacturing, Projects, Expenses, Approvals, Accounting, FSM

## Quick Start

### Prerequisites

- Node.js 18+ and npm

### Installation

```bash
cd odoo-planner-app
npm install
```

### Development

```bash
npm run dev
```

The app will be available at `http://localhost:3000`

### Build for Production

```bash
npm run build
```

The production build will be in the `dist/` folder.

## Usage Workflow

### 1. Welcome Screen
- Overview of what the tool does
- Estimated completion time
- Start button

### 2. Questionnaire (7 Sections)
1. **Project Basics**: Client name, timeline, project manager
2. **Phases**: Select Clarity, Implementation, and/or Adoption
3. **Hours Allocation**: Enter quoted hours per phase
4. **Modules**: Select which Odoo modules to implement
5. **Clarity Details**: Current systems, pain points, workshop count
6. **Implementation Details**: Data migration, integrations, customizations
7. **Adoption Details**: User count, training format, go-live support

### 3. Review
- Summary of all answers
- Ability to edit before generating plan

### 4. Plan Generation
- Generates comprehensive task list based on:
  - Selected phases
  - Selected modules
  - Hour allocations
  - Task library templates

### 5. Plan Display & Export
- View all tasks organized by phase
- See task details: title, description, hours, priority, tags
- Export to CSV for Odoo import

## Task Library

The tool uses a comprehensive task library (`../task-library.json`) with:

- **Clarity Phase**: 11 standard tasks (kickoff, stakeholder interviews, process mapping, gap analysis, etc.)
- **Implementation Phase**: 150+ module-specific tasks across 10 modules
- **Adoption Phase**: 10 standard tasks (training, go-live support, hypercare, etc.)

Each task includes:
- Name and description
- Estimated hours
- Priority (High/Medium/Low)
- Category
- Odoo feature reference
- Tags

## CSV Import into Odoo 19

### Import Steps:

1. Generate and download the CSV file from the planner
2. In Odoo 19, navigate to: **Project → Tasks**
3. Click **Favorites** (⭐) → **Import records**
4. Upload the CSV file
5. Map columns if needed (usually auto-detected)
6. Click **Import**
7. Verify imported tasks in your project

### CSV Fields:

The exported CSV includes these fields compatible with Odoo 19:

| Field | Description |
|-------|-------------|
| Title | Task name |
| Project | Project name from questionnaire |
| Assignees | Project manager (can be edited in Odoo) |
| Allocated Time | Hours estimated for the task |
| Deadline | Empty by default (set in Odoo) |
| Stage | Always "New" |
| Priority | High/Medium/Low priority |
| Tags | Comma-separated tags (phase, module, category) |
| Milestone | Phase-based milestone grouping |
| Parent Task | For subtasks (mostly empty in initial generation) |

## Architecture

```
odoo-planner-app/
├── src/
│   ├── App.jsx           # Main application component
│   ├── main.jsx          # React entry point
│   └── index.css         # Tailwind CSS styles
├── public/               # Static assets
├── task-library.json     # Task templates (in parent dir)
├── questionnaire-structure.json  # Questionnaire definition (in parent dir)
├── package.json          # Dependencies
├── vite.config.js        # Vite configuration
└── tailwind.config.js    # Tailwind configuration
```

## Technology Stack

- **React 18**: UI framework
- **Vite**: Build tool and dev server
- **Tailwind CSS**: Styling
- **Lucide React**: Icons
- **PapaParse**: CSV generation

## Customization

### Adding New Modules

1. Edit `../task-library.json`
2. Add a new module entry under `modules`
3. Define implementation tasks with Odoo feature references
4. Update questionnaire to include the new module option

### Modifying Task Templates

1. Edit tasks in `../task-library.json`
2. Adjust hours, priorities, or descriptions
3. Add/remove tasks as needed
4. Restart dev server to see changes

### Customizing Questions

1. Edit `../questionnaire-structure.json`
2. Add/modify sections or questions
3. Update conditional logic as needed
4. Restart dev server

## Data Sources

All task templates and configurations are based on:
- **Odoo 19 Official Documentation**: https://www.odoo.com/documentation/19.0/
- **Arkode Implementation Best Practices**
- **Real Odoo 19 exports** (analyzed field structure)

## Future Enhancements

Potential features for future versions:
- AI-powered task customization via Claude API
- Task editing capability in the UI
- Multiple project manager assignment
- Milestone date calculation
- Budget vs. actual hour tracking
- PDF export of project plan
- Save/load project configurations

## Support

For questions or issues:
- Internal: Contact Arkode's development team
- GitHub: Create an issue in the arkode-experiments repository

## License

Internal tool for Arkode use only.

---

**Version**: 1.0.0
**Last Updated**: 2025-11-06
**Odoo Version**: 19.0
