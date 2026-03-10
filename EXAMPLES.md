# Practical Examples - Odoo OCA Developer Skill

## Example 1: Create a new module

### Scenario: You need to create a module to add custom functionality to sales

**Ask Copilot:**
```
"I need to create a new Odoo 17 module to extend sales orders
following OCA conventions"
```

**Copilot will provide:**
1. Instructions to use `init_oca_module.py`
2. Correct directory structure
3. `__manifest__.py` configuration
4. Examples of models and views

**Commands to run:**
```bash
# 1. Create module
python ~/.copilot/skills/odoo-oca-developer/scripts/init_oca_module.py \
    sale_order_custom --version 17.0 --path /path/to/addons

# 2. Validate structure
python ~/.copilot/skills/odoo-oca-developer/scripts/validate_module.py \
    sale_order_custom/
```

**Result:**
```
sale_order_custom/
├── __init__.py
├── __manifest__.py
├── models/
│   ├── __init__.py
│   └── sale_order.py
├── views/
│   └── sale_order_views.xml
├── security/
│   ├── ir.model.access.csv
│   └── sale_order_security.xml
├── data/
│   └── sale_order_data.xml
├── readme/
│   ├── DESCRIPTION.rst
│   ├── USAGE.rst
│   └── CONTRIBUTORS.rst
└── tests/
    ├── __init__.py
    └── test_sale_order.py
```

---

## Example 2: Migrate a module from Odoo 16 to 18

### Scenario: You have a module in Odoo 16 and need to migrate it to 18

**Ask Copilot:**
```
"Help me migrate my 'custom_reports' module from Odoo 16 to 18 using OpenUpgrade"
```

**Steps Copilot suggests:**

1. **Create migration structure:**
```bash
mkdir -p custom_reports/migrations/18.0.1.0.0
```

2. **Create `pre-migration.py`:**
```python
from openupgradelib import openupgrade

@openupgrade.migrate()
def migrate(env, version):
    # Rename fields that changed names
    openupgrade.rename_fields(env, [
        ('report.custom', 'report_custom', 'old_field_name', 'new_field_name'),
    ])
    
    # Rename models if necessary
    openupgrade.rename_models(env.cr, [
        ('old.model.name', 'new.model.name'),
    ])
```

3. **Create `post-migration.py`:**
```python
from openupgradelib import openupgrade

@openupgrade.migrate()
def migrate(env, version):
    # Update values
    openupgrade.map_values(
        env.cr,
        openupgrade.get_legacy_name('state'),
        'state',
        [('draft', 'pending'), ('done', 'completed')],
        table='report_custom',
    )
    
    # Recompute calculated fields
    env['report.custom'].search([])._compute_totals()
```

4. **Update `__manifest__.py`:**
```python
{
    'name': 'Custom Reports',
    'version': '18.0.1.0.0',  # New version
    'category': 'Reports',
    'license': 'AGPL-3',
    'author': 'Your Company, Odoo Community Association (OCA)',
    'website': 'https://github.com/OCA/<repo>',
    'depends': ['base', 'report_xlsx'],  # Update dependencies if changed
    'installable': True,
}
```

**Validate migration:**
```bash
# Test on database copy
# 1. Backup database
# 2. Run upgrade
odoo-bin -u custom_reports -d test_database

# 3. Verify data
```

---

## Example 3: Extend an Odoo core module

### Scenario: You want to extend the `event` module to add custom fields

**Ask Copilot:**
```
"I want to extend the 'event' module in Odoo to add a maximum budget field.
Help me follow OCA conventions"
```

**Solution Copilot provides:**

1. **Create extension module:**
```bash
python ~/.copilot/skills/odoo-oca-developer/scripts/init_oca_module.py \
    event_budget --version 17.0
```

2. **Extend model (`models/event.py`):**
```python
from odoo import fields, models

class Event(models.Model):
    _inherit = 'event.event'
    
    max_budget = fields.Float(
        string='Maximum Budget',
        help='Maximum budget allowed for this event'
    )
    
    budgeted_cost = fields.Float(
        string='Budgeted Cost',
        compute='_compute_budgeted_cost',
        store=True,
    )
    
    @api.depends('registration_ids.cost')
    def _compute_budgeted_cost(self):
        for event in self:
            event.budgeted_cost = sum(
                event.registration_ids.mapped('cost')
            )
```

3. **Extend view (`views/event_views.xml`):**
```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo>
    <record id="event_view_form" model="ir.ui.view">
        <field name="model">event.event</field>
        <field name="inherit_id" ref="event.view_event_form"/>
        <field name="arch" type="xml">
            <xpath expr="//field[@name='name']" position="after">
                <field name="max_budget"/>
                <field name="budgeted_cost"/>
            </xpath>
        </field>
    </record>
</odoo>
```

4. **Update `__manifest__.py`:**
```python
{
    'name': 'Event Budget Management',
    'version': '17.0.1.0.0',
    'category': 'Event Management',
    'license': 'AGPL-3',
    'author': 'Your Company, Odoo Community Association (OCA)',
    'website': 'https://github.com/OCA/<repo>',
    'depends': ['event'],  # Dependency on extended module
    'data': [
        'views/event_views.xml',
    ],
    'installable': True,
}
```

---

## Example 4: Validate OCA compliance

### Scenario: You finished a module and want to ensure it follows OCA standards

**Command:**
```bash
python ~/.copilot/skills/odoo-oca-developer/scripts/validate_module.py my_module/
```

**Expected output:**
```
Validating module: my_module
============================================================

📄 Manifest Validation
------------------------------------------------------------
✅ __manifest__.py is valid
✅ author includes 'Odoo Community Association (OCA)'
✅ License 'AGPL-3' is standard for OCA

📁 Structure Validation
------------------------------------------------------------
✅ README.rst found
✅ Found directories: models, views, security

📝 Naming Convention Validation
------------------------------------------------------------
✅ File naming conventions are correct

============================================================
✅ Module validation passed!
```

---

## Example 5: Create OCA-compliant tests

### Scenario: You need to write tests for your module

**Ask Copilot:**
```
"How do I write tests following OCA conventions for my Odoo 17 module?"
```

**Example test (`tests/test_sale_order_custom.py`):**
```python
from odoo.tests import TransactionCase, tagged

@tagged('post_install', '-at_install')
class TestSaleOrderCustom(TransactionCase):
    
    @classmethod
    def setUpClass(cls):
        super().setUpClass()
        cls.partner = cls.env['res.partner'].create({
            'name': 'Test Partner',
            'email': 'test@example.com',
        })
        cls.product = cls.env['product.product'].create({
            'name': 'Test Product',
            'list_price': 100.0,
        })
    
    def test_sale_order_custom_field(self):
        """Test custom field in sale order"""
        sale_order = self.env['sale.order'].create({
            'partner_id': self.partner.id,
            'order_line': [(0, 0, {
                'product_id': self.product.id,
                'product_uom_qty': 1,
                'price_unit': 100.0,
            })],
        })
        
        self.assertEqual(sale_order.partner_id, self.partner)
        self.assertEqual(len(sale_order.order_line), 1)
```

---

## Example 6: Follow Git conventions

### Scenario: You make changes and need to create OCA-compliant commit messages

**Correct examples:**
```bash
# Create new feature
git commit -m "[ADD] sale_order_custom: add budget tracking

Add budget fields and computed total to sale orders for better cost control"

# Fix bug
git commit -m "[FIX] sale_order_custom: prevent negative budget

Prevent users from entering negative values in budget field"

# Refactoring
git commit -m "[REF] sale_order_custom: split module code

Separate budget logic into dedicated module for better maintenance"

# Version migration
git commit -m "[MIG] sale_order_custom: migrate to Odoo 18

Update field types and API calls for Odoo 18 compatibility"
```

---

## Resources Within the Skill

**To learn more, consult:**

1. **Complete OCA Conventions:**
   - Location: `~/.copilot/skills/odoo-oca-developer/references/oca_conventions.md`
   - Covers: names, structure, Python, XML, Git

2. **OpenUpgrade Migration Guide:**
   - Location: `~/.copilot/skills/odoo-oca-developer/references/openupgrade_migration.md`
   - Covers: scripts, patterns, examples

3. **Module Template:**
   - Location: `~/.copilot/skills/odoo-oca-developer/assets/module_template/`
   - Usage: reference for complete structure

---

## Additional Tips

✅ **Always use the template**: It's faster than creating from scratch

✅ **Validate frequently**: Detect issues early

✅ **Read the references**: Learn all conventions

✅ **Follow semantic versioning**: `{odoo_version}.major.minor.patch`

✅ **Document your code**: Write docstrings for methods and classes

✅ **Write tests**: Improve quality and confidence

✅ **Keep commits clean**: One logical change per commit

---
