# Arkode Experiments

Repository for experimental projects and tools developed at Arkode.

## Projects

### Odoo Implementation Planner

A comprehensive project planning tool for Odoo implementation projects.

**Location**: `./odoo-planner-app/`

**Purpose**: Help Arkode consultants quickly generate structured project plans with task breakdowns for Clarity, Implementation, and Adoption phases.

**Key Features**:
- Interactive questionnaire (10-15 min completion time)
- Automatic task generation based on Odoo 19 modules
- Smart hour allocation across tasks
- CSV export for direct import into Odoo 19 Project module
- 150+ pre-defined tasks across 10 modules

**Quick Start**:
```bash
cd odoo-planner-app
npm install
npm run dev
```

See [odoo-planner-app/README.md](./odoo-planner-app/README.md) for detailed documentation.

## Reference Documentation

- **[ODOO_IMPORT_FORMAT_REFERENCE.md](./ODOO_IMPORT_FORMAT_REFERENCE.md)**: Analysis of Odoo 19 CSV import format and field requirements
- **[task-library.json](./task-library.json)**: Comprehensive task templates for all implementation phases
- **[questionnaire-structure.json](./questionnaire-structure.json)**: Questionnaire definition and structure

## Sample Data

- **[odoo-sample-exports/](./odoo-sample-exports/)**: Exported Odoo 19 project and task data for reference

## Development

All projects in this repository are internal tools for Arkode.

**Branch Structure**:
- `main`: Stable releases
- `claude/*`: Feature development branches

## Contributing

For Arkode team members only. When contributing:

1. Create a feature branch from `main`
2. Make your changes
3. Test thoroughly
4. Create a pull request
5. Get review approval
6. Merge to `main`

## Support

For questions or issues, contact the Arkode development team.
