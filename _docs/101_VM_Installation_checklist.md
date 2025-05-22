## VM Preparation
### VM1 (Odoo Web Application)
    • Static IP assigned.
    • Required ports opened:
    • 5050 (PgAdmin4)
    • 8069, 7072 (Odoo)
    • 9000, 9443 (Portainer)

### VM2 (PostgreSQL Database)
    • Static IP assigned.
    • Port 5432 (PostgreSQL) opened.

### Network and Integration
    • VM1 ↔ VM2: Ensure connectivity (VM1 should reach VM2 on port 5432).

### External Integrations:
    • VM1 → BioStar 2 Server: Confirm access for integration.
    • VM1 → LDAP/AD: Open port 389 for authentication and sync.
    • VM1 → SMTP Server: Allow outbound email traffic for communication.
    • VM1 → GitHub & Docker Hub: Ensure access to download necessary images and dependencies.

### Email Mapping

Requested Field

Active Directory Attribute

department_name

department

employee_name (Full Display Name)

displayName

entry_uuid (GUID)

objectGUID

work_mobile

mobile

work_phone

telephoneNumber

email (login | username)

mail

manager_uuid (GUID)

N/A

manager_name

manager

job_position

title

employee_no

employeeID