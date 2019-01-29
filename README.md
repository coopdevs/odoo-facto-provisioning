# Ansible inventories
This repository stores hosts informations and related variables for this specific instance of Odoo.

## Requirements

1. Clone this repo and [odoo-provisioning](https://gitlab.com/femprocomuns/odoo-provisioning) in the same directory
2. Go to `odoo-provisioning` directory and install Ansible dependencies:
   ```
   ansible-galaxy -r requirements.yml
   ```
3. Run `ansible-playbook` command pointing to the `inventory/hosts` file of this repository:
   ```
   ansible-playbook playbooks/provision.yml -i ../odoo-facto-provisioning/inventory/hosts --ask-vault-pass --limit=dev
   ```

## Odoo Core Modules

- [Base]()
- base_automation
- base_address_city
- base_import
- base_iban
- base_setup
- base_vat
- base_vat_autocomplete
- [Project](https://github.com/odoo/odoo/tree/11.0/addons/project)
- crm
- [l10n_es](https://github.com/odoo/odoo/tree/11.0/addons/l10n_es)
- [account](https://github.com/odoo/odoo/tree/11.0/addons/account)
- [analytic]()
- [sale]()
- stock
- sale_management
- stock_account
- crm_project
- hr_timesheet
- mail
- hr_holidays
- account_invoicing
- account_asset
- purchase
- hr
- hr_expense
- account_analytic_default
- account_bank_statement_import
- account_cancel
- auth_crypt
- auth_signup
- barcodes
- board
- bus
- calendar_sms
- contacts
- decimal_precision
- document
- fetchmail
- google_account
- google_calendar
- google_drive
- google_spreadsheet
- hr_contract
- iap
- payment
- payment_transfer
- procurement_jit
- product
- product_margin
- project_timesheet_holidays
- resource
- sale_crm
- sale_expense
- sale_margin
- sale_stock
- sale_timesheet
- sales_team
- sms
- utm
- web
- web_diagram
- web_editor
- web_kanban_gauge
- web_planner
- web_settings_dashboard
- web_tour
- calendar
- portal
- http_routin
- crm_phone_validation
- phone_validation"

## Odoo Community Modules

> Using the `requirements.txt` file in this repository
- account_banking_mandate
- account_banking_pain_base
- account_banking_sepa_credit_transfer
- account_banking_sepa_direct_debit
- account_due_list
- account_financial_report
- account_multicompany_easy_creation
- account_payment_mode
- account_payment_order
- account_payment_partner
- account_tax_balance
- auth_brute_force
- base_bank_from_iban
- base_location
- base_location_geonames_import
- base_technical_features
- contract
- contract_sale_invoicing
- contract_variable_qty_timesheet
- contract_variable_quantity
- date_range
- l10n_es_account_asset
- l10n_es_account_bank_statement_import_n43
- l10n_es_account_invoice_sequence
- l10n_es_aeat
- l10n_es_aeat_mod111
- l10n_es_aeat_mod115
- l10n_es_aeat_mod303
- l10n_es_aeat_mod349
- l10n_es_mis_report
- l10n_es_partner
- l10n_es_toponyms
- mail_tracking
- mail_tracking_mailgun
- mass_editing
- mis_builder
- module_auto_update
- password_security
- project_task_default_stage
- report_xlsx
- sale_order_invoicing_finished_task
- sale_order_line_input
- web_decimal_numpad_dot
- web_favicon
- web_no_bubble
- web_responsive
- web_searchbar_full_width
- web_widget_color

## Instances

* [Facto Coopdevs](https://facto.coopdevs.org)
