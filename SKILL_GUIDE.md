# Odoo OCA Developer Skill - Complete Guide

## Overview

Successfully created the **odoo-oca-developer** skill for GitHub Copilot. This skill provides expert guidance for developing, migrating, and maintaining Odoo modules following OCA (Odoo Community Association) conventions.

## What This Skill Does

The skill enables you to:

1. **Create new Odoo modules** from OCA template with proper structure
2. **Migrate modules between Odoo versions** (14.0-19.0) using OpenUpgrade patterns
3. **Extend core Odoo modules** following OCA conventions
4. **Ensure OCA compliance** with validation tools
5. **Structure module files correctly** with naming conventions and best practices

## Installation

The skill is automatically installed at:
```
~/.copilot/skills/odoo-oca-developer.skill
```

## How to Use It

Simply ask Copilot any of these questions:

### Module Creation
- "I need to create a new Odoo module"
- "Create an Odoo 17 module following OCA conventions"
- "Generate a module structure for my custom feature"

### Module Migration
- "How do I migrate this module from Odoo 16 to 18?"
- "Help me migrate this module using OpenUpgrade"
- "Guide me through version migration from 17 to 19"

### Module Extension
- "Extend the 'event' module to add custom functionality"
- "Help me inherit from the sale.order model"
- "Extend an existing Odoo module"

### Validation & Guidelines
- "Check if my Odoo module follows OCA conventions"
- "What are the OCA naming conventions?"
- "Validate my module structure"

## Core Features

### 1. Module Initialization
```bash
python scripts/init_oca_module.py my_module --version 17.0
```
Creates complete OCA-compliant module structure with:
- Proper directory layout
- Pre-configured `__manifest__.py`
- README structure
- Example models and views
- Security configuration

### 2. Module Validation
```bash
python scripts/validate_module.py path/to/module
```
Checks:
- Required files and keys
- OCA author attribution
- License compliance
- Version format
- File naming conventions
- Directory structure

### 3. OCA Conventions Reference
Comprehensive guide covering:
- Module naming patterns
- Directory structure
- File naming conventions
- `__manifest__.py` requirements
- Python code structure
- XML conventions
- Git commit message format
- Installation hooks
- Field and method naming

### 4. OpenUpgrade Migration Guide
Complete migration patterns:
- Pre/post migration scripts
- Field/model renaming
- Table restructuring
- Data transformation
- Value mapping
- XML changes
- Best practices and common patterns

### 5. Module Template
Full OCA-compliant template including:
- All required directories
- Example files
- Security configuration
- Test structure
- Static assets organization
- Report examples
- Migration examples

## Package Contents

### Scripts
- `init_oca_module.py` - Create new modules
- `validate_module.py` - Validate module compliance

### References
- `oca_conventions.md` - Complete OCA guidelines (module structure, naming, code patterns)
- `openupgrade_migration.md` - Migration guide with examples and best practices

### Assets
- `module_template/` - Official OCA module template for rapid development

## Key OCA Conventions Covered

### Naming
- Singular module names: `sale_order_import`
- Prefix base modules: `base_location`
- Prefix localization: `l10n_es_pos`
- Extend with parent name: `mail_forward`

### Structure
```
module_name/
├── models/
├── views/
├── security/
├── data/
├── demo/
├── migrations/
├── readme/
├── tests/
└── __manifest__.py
```

### Manifest
```python
{
    'name': 'Module Name',
    'version': '17.0.1.0.0',
    'category': 'Sales',
    'license': 'AGPL-3',
    'author': 'Company, Odoo Community Association (OCA)',
    'website': 'https://github.com/OCA/<repo>',
    'depends': ['base', 'sale'],
    'data': ['security/ir.model.access.csv', 'views/model_name_views.xml'],
    'installable': True,
}
```

### Python Code Structure
- Imports ordered by type and alphabetically
- Models with clear method patterns
- Compute methods: `_compute_<field_name>`
- Action methods: `action_<name>`
- Constraint methods: `_check_<name>`
- Field naming with suffixes: `_id`, `_ids`

### XML Conventions
- 4-space indentation
- Model names follow patterns: `<model>_view_<type>`
- Actions: `<model>_action`
- Demo records: suffix with `_demo`
- Inherit views with xpath

### Migration
- Pre-migration for schema changes
- Post-migration for data transformation
- Use OpenUpgrade helpers, not direct SQL
- Test on database copy first

## Workflow Examples

### Create a New Module
1. Run: `python scripts/init_oca_module.py my_feature --version 17.0`
2. Edit `__manifest__.py` with module details
3. Create models in `models/` directory
4. Create views in `views/` directory  
5. Add security in `security/`
6. Validate: `python scripts/validate_module.py path/to/module`
7. Update readme and documentation

### Migrate Module from 16.0 to 18.0
1. Check OpenUpgrade documentation for breaking changes
2. Create: `migrations/18.0.1.0.0/`
3. Write `pre-migration.py` for schema changes
4. Write `post-migration.py` for data transformation
5. Test on copy of production database
6. Document changes in README

### Extend a Core Module
1. Create new module with core module in dependencies
2. Use `_inherit` to extend models
3. Use `inherit_id` to extend views
4. Follow OCA naming: `<core>_<feature>`
5. Keep changes minimal and focused

## Tips & Best Practices

- Always use OCA template as starting point
- One file per model is better than consolidated files
- Validate your module regularly
- Test migrations thoroughly on copies
- Keep code readable over concise
- Use meaningful variable names
- Document your code with docstrings
- Follow PEP8 and OCA guidelines
- Include comprehensive tests
- Use semantic versioning

## Troubleshooting

**Module not appearing in Apps**
- Check `'installable': True`
- Verify __init__.py imports
- Reinstall module

**Import errors**
- Add try-except for external dependencies
- Document in README
- Add to requirements.txt

**Migration fails**
- Verify pre-migration runs first
- Check table/column names
- Test on copy database

## Next Steps

1. **Try the skill**: Ask any Odoo development question
2. **Use the scripts**: Initialize and validate modules
3. **Reference the guides**: Check OCA conventions and migration docs
4. **Share feedback**: How can the skill be improved?

## Resources

- OCA Guidelines: https://github.com/OCA/odoo-community.org/blob/master/website/Contribution/CONTRIBUTING.rst
- OCA Maintainer Tools: https://github.com/OCA/maintainer-tools
- OpenUpgrade: https://github.com/OCA/OpenUpgrade
- OpenUpgrade Docs: https://oca.github.io/OpenUpgrade/

## File Location

The skill is stored in two places:

**Development copy:**
```
~/Workspaces/odoo-oca-convention-skill/odoo-oca-developer/
```

**Global installation:**
```
~/.copilot/skills/odoo-oca-developer.skill
```

Both contain the complete skill with all scripts, references, and assets.

---

**Enjoy developing Odoo modules with OCA conventions! 🚀**
