# Research Computing Portal Analysis

This repository contains multiple forks of ColdFront and the Unity Web Portal. This document analyzes the unique features in each downstream fork that haven't been pushed upstream to UBCCR, and identifies features unique to the Unity portal.

## Repository Overview

- **ubccr/coldfront** - The upstream ColdFront repository
- **coldfront/** - UC Berkeley's MyBRC/MyLRC User Portal fork
- **harvard/coldfront** - Harvard FASRC's fork
- **wustl/coldfront-wustl-fork** - Washington University in St. Louis fork
- **cci-moc/coldfront** - CCI MOC fork (appears to be standard)
- **sum/coldfront_pr_base** - SUM fork (appears to be close to upstream)
- **umass/unity-web-portal** - UMass Unity Portal (PHP-based, completely different architecture)

---

## 1. Downstream ColdFront Fork Features Not in UBCCR Upstream

### 1.1 UC Berkeley (MyBRC/MyLRC) - `coldfront/`

This is the most significantly diverged fork with extensive custom development for Berkeley Research Computing and Lawrence Berkeley National Laboratory Research Computing programs.

#### Unique Core Modules

1. **Account Module** (`coldfront/core/account/`)
   - User account lifecycle management
   - Login activity tracking
   - Account adapter for authentication integration
   - Custom admin interfaces for account management

2. **Billing Module** (`coldfront/core/billing/`)
   - Comprehensive billing system with multiple utilities:
     - `billing_month.py` - Monthly billing cycles
     - `billing_record_utils.py` - Billing record management
     - `billing_report_utils.py` - Report generation
     - `computing_allowance_interface.py` - Computing resource allowances
     - `export_queries.py` - Data export for billing
     - `job_costing.py` - Job-based cost calculation
   - Templates for billing administration
   - Multiple test suites for billing functionality

3. **Statistics Module** (`coldfront/core/statistics/`)
   - Job statistics tracking and reporting
   - Job submission eligibility checking
   - Forms for statistical data entry
   - Management commands for stats operations

4. **Socialaccount Module** (`coldfront/core/socialaccount/`)
   - Social authentication integration
   - Custom adapter and signals for social login
   - Support for federated identity

5. **Enhanced Allocation Module**
   - Allocation periods and renewal requests
   - Allocation addition requests
   - Secure directory management:
     - `secure_dir_forms.py`
     - `SecureDirRequest` model and workflow
   - Cluster access request workflow
   - User attribute tracking on allocations
   - MOU (Memorandum of Understanding) file attachments
   - Renewal survey capabilities

6. **Enhanced Project Module**
   - Project user removal requests
   - Secure directory project integration
   - Split views and utils into organized submodules
   - Enhanced project creation with computing allowances

#### Unique Plugins

1. **Departments Plugin** (`plugins/departments/`)
   - Department management system
   - User-department associations
   - LDAP backend integration (CalNet LDAP)
   - Management commands:
     - `load_departments.py`
     - `load_user_departments.py`
   - Dummy backend for testing
   - Template for updating user departments

2. **Hardware Procurements Plugin** (`plugins/hardware_procurements/`)
   - Hardware procurement tracking and management
   - Google Sheets data source integration
   - Cached data source backend
   - Templates for:
     - Procurement detail view
     - Procurement list view
     - Request list tables
     - Status badges
   - Refresh cache management command
   - Comprehensive test suite with pytest

#### Comprehensive REST API

The Berkeley fork has the most complete REST API implementation:

1. **API Structure** (`coldfront/api/`)
   - **Allocation API**:
     - Full CRUD operations on allocations
     - Allocation attributes and history
     - Allocation user management
     - Allocation user attributes
     - Cluster access requests
   - **Billing API**:
     - Billing activities tracking
     - Billing record queries
   - **Project API**:
     - Project management endpoints
     - Project user removal request API
   - **Statistics API**:
     - Job submission eligibility checks
     - Job statistics endpoints
   - **User API**:
     - User management
     - Identity linking requests
     - Custom authentication with ExpiringToken
   - **Resource API**:
     - Resource serialization
   - Comprehensive test coverage for all API endpoints

2. **API Features**:
   - Token-based authentication
   - Expiring tokens for security
   - IP-based access control support
   - Filtering capabilities
   - Pagination support
   - Detailed permissions system

#### User Experience Enhancements

1. **Enhanced User Module**:
   - Identity linking system
   - Email notification management
   - User profile customization
   - Split forms and views for better organization

2. **Development Tools**:
   - Docker-based development environment
   - Vagrant VM support
   - Comprehensive testing infrastructure
   - Ansible deployment configurations

---

### 1.2 Harvard FASRC - `harvard/coldfront/`

Harvard's fork is heavily customized for their internal systems and workflows.

#### Unique Core Modules

1. **Department Module** (`coldfront/core/department/`)
   - Department management at the core level
   - Department member tracking
   - Historical tracking with django-simple-history
   - Templates for department management

#### Unique Plugins

1. **FASRC Plugin** (`plugins/fasrc/`)
   - FASRC-specific data imports
   - Management commands:
     - `id_import_new_allocations.py` - Import allocations
     - `import_quotas.py` - Quota data imports
     - `pull_resource_data.py` - Resource data synchronization
   - Integration with internal FASRC systems
   - Test data fixtures for FASRC workflows

2. **FASRC Monitoring Plugin** (`plugins/fasrc_monitoring/`)
   - Admin dashboard for monitoring
   - Database view checks
   - Pipeline monitoring
   - Management command: `run_view_db_checks.py`
   - Monitoring templates and views

3. **IFX Plugin** (`plugins/ifx/`)
   - Comprehensive billing and financial integration
   - Calculator for resource costs
   - Product usage tracking
   - Management commands:
     - `calculateColdfrontBillingRecords.py`
     - `createProductUsages.py`
     - `getResourceAllocAuthData.py`
     - `processIfxappsMessages.py`
     - `pruneOrganizations.py`
     - `updateAffiliations.py`
     - `updateProjectOrganizations.py`
   - Custom models:
     - `ProductAllocation`
     - `ProjectOrganization`
     - `SUUser`
   - REST API viewsets for IFX data
   - Custom permissions
   - Report generation

4. **Isilon Plugin** (`plugins/isilon/`)
   - Dell EMC Isilon storage integration
   - Quota management from Isilon
   - Management command: `pull_isilon_quotas.py`
   - Signals for Isilon automation
   - Test suite

5. **LDAP Plugin** (`plugins/ldap/`)
   - LDAP project management (different from ldap_user_search)
   - Group membership updates
   - Management commands:
     - `add_projects.py`
     - `id_add_new_projects.py`
     - `update_group_membership.py`

6. **LFS Plugin** (`plugins/lfs/`)
   - Lustre File System integration
   - gRPC client for LFS communication
   - Protocol buffer definitions
   - Management command: `add_lfs_quotas.py`
   - Modern microservices architecture

7. **SFtoCF Plugin** (`plugins/sftocf/`)
   - Starfish to ColdFront integration
   - Data synchronization from Starfish storage analytics
   - Management commands:
     - `add_coldfront_usage.py`
     - `import_missing_pi_account.py`
     - `import_missing_projects.py`
     - `update_BillingActivity.py`
     - `update_SfData.py`
   - Test data and test suites
   - Environmental configuration via `servers.json`

8. **SlurmREST Plugin** (`plugins/slurmrest/`)
   - Slurm REST API integration (modern alternative to direct Slurm commands)
   - Management commands:
     - `add_accounts_to_resources.py`
     - `add_slurm_accounts.py`
     - `pull_slurm_account_names.py`
   - Signal-based automation

9. **VAST Plugin** (`plugins/vast/`)
   - VAST Data storage system integration
   - Management command: `pull_vast_quotas.py`
   - Task automation

10. **API Plugin** (`plugins/api/`)
    - Different from Berkeley's full REST API
    - Serializers for data export
    - Custom views and URLs
    - Test suite

#### Additional Features

- Enhanced test infrastructure with `test_helpers/` including FASRC-specific factories
- Custom utilities in `core/utils/fasrc.py`
- Integration with multiple external systems (IFX billing, Starfish, Isilon, VAST, LFS)

---

### 1.3 Washington University in St. Louis - `wustl/coldfront-wustl-fork/`

WUSTL's fork focuses on Qumulo storage management with modern web technologies.

#### Unique Plugin

1. **Qumulo Plugin** (`plugins/qumulo/`)
   - **Most comprehensive single-storage-system integration in any fork**
   - Modern React-based frontend:
     - Built with Vite
     - 18 JSX components
     - Custom CSS styling
     - Located in `frontend/react/`
   - Extensive API layer:
     - Active Directory member management
     - Allocation API
   - Custom Django forms:
     - `AllocationForm`
     - `AllocationTableSearchForm`
     - `CreateSubAllocationForm`
     - `ProjectCreateForm`
     - `TriggerMigrationsForm`
     - `UpdateAllocationForm`
   - Services architecture:
     - `allocation_service.py` - Allocation management
     - `file_system_service.py` - File system operations
     - ITSM integration:
       - `itsm_client.py` - ServiceNow/ITSM integration
       - `itsm_client_handler.py`
       - `migrate_to_coldfront.py` - Data migration from ITSM
       - Custom fields for ITSM data
   - Comprehensive utilities:
     - `aces_manager.py` - Access control management
     - `acl_allocations.py` - ACL handling
     - `active_directory_api.py` - AD integration
     - `billing_query_generator.py` - Billing query creation
     - `billing_report.py` - Report generation
     - `billing_result_set.py` - Result processing
     - `eib_billing.py` - EIB billing integration
     - `prepaid_billing.py` - Prepaid billing support
     - `qumulo_api.py` - Qumulo REST API client
     - `storage_controller.py` - Storage orchestration
     - `update_user_data.py` - User synchronization
   - 25 management commands for various operations
   - Custom templatetags and widgets
   - Extensive test coverage:
     - Unit tests for all components
     - Integration tests (run with VPN)
     - Test fixtures and helper classes
   - Migration support from external systems
   - Email templates for notifications
   - Static assets including migration mappings

#### Testing Infrastructure
   - Separate integration test suite (`tests_integration/`)
   - Environment-based configuration
   - VPN-dependent integration tests
   - Comprehensive coverage of API, services, utils, validators, and views

---

### 1.4 UBCCR Upstream-Specific Features (Not in Other Forks)

While this is the "upstream", it has some features not present in all downstream forks:

1. **Auto Compute Allocation Plugin** (`plugins/auto_compute_allocation/`)
   - Automatic allocation creation on project creation
   - Django signals-based automation
   - Configurable allocation parameters:
     - Core hours gauges
     - Accelerator hours gauges
     - Training project support
     - End date delta
     - Changeable/locked status
   - Slurm account naming formats with templating
   - Institution-based fairshare
   - Cluster filtering
   - Custom slurm attributes
   - Detailed in extensive README

2. **Project OpenLDAP Plugin** (`plugins/project_openldap/`)
   - OpenLDAP integration for project groups
   - Creates per-project OUs and posixGroups
   - Membership synchronization via django signals
   - Management commands:
     - `project_openldap_sync.py` - Synchronization
     - `project_openldap_check_setup.py` - Setup verification
   - Archive OU support
   - Configurable GID ranges
   - Mermaid diagrams for documentation
   - Comprehensive security configuration
   - SSL/TLS support

---

## 2. Unity Web Portal (UMass) - Completely Different Architecture

The Unity Web Portal is **not a ColdFront fork** - it's a completely separate system with different goals and architecture.

### Technology Stack Differences

| Feature | ColdFront | Unity Portal |
|---------|-----------|--------------|
| **Language** | Python | PHP |
| **Framework** | Django | Custom PHP |
| **Database** | Various (PostgreSQL/MySQL) | MariaDB |
| **Identity** | Various (FreeIPA/LDAP/OIDC) | LDAP (central) |
| **Authentication** | Multiple backends | Shibboleth SP |
| **Frontend** | Django templates/Bootstrap | Custom PHP + WYSIWYG |

### Unity Portal Unique Features

#### 1. SSH Key Management
- **No password authentication** - Key-based only
- Multiple key input methods:
  - GitHub import
  - File upload
  - Paste public key
  - Generate and download private key
- User self-service key management
- Keys stored and synced to LDAP

#### 2. PI Group Self-Service
- Users can request to become PIs
- PI approval workflow (admin must approve PI requests)
- PIs approve/deny users joining their groups
- PIs can remove members from their groups
- Self-service group management

#### 3. Login Shell Management
- Users can change their default login shell
- Self-service shell selection

#### 4. LDAP-Centric Architecture
- **LDAP is the primary data store** (not just authentication)
- Automatic LDAP updates reflect current state
- Synchronization of:
  - Users
  - Groups
  - Organizations
  - PI groups
- Worker processes for LDAP synchronization

#### 5. Cluster Notices System
- Added to front page
- Email notifications
- REST API exposure
- WYSIWYG HTML editor for content

#### 6. Multi-Domain Branding
- Simultaneous multi-domain support
- Per-domain configuration overrides
- Custom branding per domain
- Domain-specific logos and styling

#### 7. Custom UID/GID Mappings
- Custom UIDNumber/GIDNumber mappings for specific users
- CSV-based custom mappings
- Override default numbering schemes

#### 8. Account Management
- Account deletion requests
- Audit logging
- User last login tracking
- Site variables system

#### 9. Admin Features
- Login as another user (su functionality)
- Mailing system
- Content management for pages
- Terms of service management
- Account policy pages

#### 10. Simplified Focus
Unity is focused on:
- User onboarding
- Group membership
- SSH access management
- LDAP as source of truth

It does **NOT** handle:
- Resource allocations (beyond group membership)
- Billing
- Publications/Grants
- Field of science
- Reviews/Renewals
- Storage quotas (expects external scripts to handle LDAP changes)

### Integration Philosophy

Unity explicitly states: *"The scope of this project ends at being responsible for the LDAP user database."*

External scripts are expected to:
- Detect LDAP changes
- Create home directories
- Add Slurm account database records
- Handle other cluster provisioning

---

## 3. Feature Comparison Matrix

### Core Features

| Feature | UBCCR | Berkeley | Harvard | WUSTL | CCI-MOC | Unity |
|---------|-------|----------|---------|-------|---------|-------|
| **Allocations** | ✓ | ✓✓ | ✓ | ✓ | ✓ | ✗ |
| **Projects** | ✓ | ✓✓ | ✓ | ✓ | ✓ | ✗ |
| **Grants** | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| **Publications** | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| **Billing** | ✗ | ✓✓ | ✓ (IFX) | ✗ | ✗ | ✗ |
| **REST API** | ✗ | ✓✓ | ✓ | ✗ | ✗ | ✓ |
| **SSH Key Mgmt** | ✗ | ✗ | ✗ | ✗ | ✗ | ✓✓ |
| **PI Self-Service** | ✗ | ✗ | ✗ | ✗ | ✗ | ✓✓ |

### Plugin Features

| Plugin/Integration | UBCCR | Berkeley | Harvard | WUSTL | CCI-MOC | Unity |
|-------------------|-------|----------|---------|-------|---------|-------|
| **FreeIPA** | ✓ | ✗ | ✓ | ✓ | ✓ | ✗ |
| **Slurm** | ✓ | ✓ | ✓ | ✓ | ✓ | N/A |
| **XDMoD** | ✓ | ✓ | ✓ | ✓ | ✓ | N/A |
| **iQuota** | ✓ | ✓ | ✓ | ✓ | ✓ | N/A |
| **LDAP User Search** | ✓ | ✓ | ✓ | ✓ | ✓ | N/A |
| **Mokey OIDC** | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| **Departments** | ✗ | ✓✓ | ✓ | ✗ | ✗ | ✗ |
| **Hardware Procure** | ✗ | ✓✓ | ✗ | ✗ | ✗ | ✗ |
| **Auto Compute Alloc** | ✓✓ | ✗ | ✗ | ✗ | ✗ | N/A |
| **Project OpenLDAP** | ✓✓ | ✗ | ✗ | ✗ | ✗ | N/A |
| **Qumulo** | ✗ | ✗ | ✗ | ✓✓ | ✗ | N/A |
| **Isilon** | ✗ | ✗ | ✓✓ | ✗ | ✗ | N/A |
| **VAST** | ✗ | ✗ | ✓✓ | ✗ | ✗ | N/A |
| **LFS (Lustre)** | ✗ | ✗ | ✓✓ | ✗ | ✗ | N/A |
| **IFX Billing** | ✗ | ✗ | ✓✓ | ✗ | ✗ | N/A |
| **Starfish** | ✗ | ✗ | ✓✓ | ✗ | ✗ | N/A |
| **SlurmREST** | ✗ | ✗ | ✓✓ | ✗ | ✗ | N/A |
| **FASRC Custom** | ✗ | ✗ | ✓✓ | ✗ | ✗ | N/A |

*Legend: ✓ = Present, ✓✓ = Unique/Enhanced, ✗ = Not present, N/A = Not applicable*

---

## 4. Recommendations for Upstream Contributions

### High Priority - Broadly Useful

These features would benefit the entire ColdFront community:

1. **From Berkeley:**
   - REST API framework (allocation, project, user endpoints)
   - Statistics module for job tracking
   - Allocation renewal request workflow
   - Departments plugin (with generic backends)
   - Hardware procurements plugin (with generic data sources)

2. **From WUSTL:**
   - Qumulo plugin architecture could be generalized for other storage systems
   - React frontend pattern for modern UI components
   - Service-oriented architecture pattern
   - ITSM integration pattern

3. **From Harvard:**
   - Department module in core
   - SlurmREST plugin (modern alternative to slurm plugin)
   - Storage plugin architecture (generalizing Isilon/VAST/LFS patterns)
   - FASRC monitoring plugin pattern

### Medium Priority - Institution-Specific but Generalizable

1. **From Berkeley:**
   - Billing module (requires significant abstraction)
   - Socialaccount integration
   - Secure directory management workflow
   - Account lifecycle management

2. **From Harvard:**
   - Plugin monitoring dashboard pattern
   - Data import/export command patterns
   - Integration testing frameworks

### Lower Priority - Highly Site-Specific

1. **From Harvard:**
   - IFX billing integration (Harvard-specific)
   - FASRC-specific import commands

2. **From Berkeley:**
   - CalNet LDAP specifics
   - Berkeley-specific billing logic

### Unity Portal Features

Unity Portal features are generally **not applicable to ColdFront** due to architectural differences. However, concepts that could inspire ColdFront enhancements:

1. SSH key management (self-service)
2. PI self-service group management UI/UX patterns
3. Multi-domain branding approach
4. Cluster notices system

---

## 5. Notable Differences in Approach

### ColdFront Forks - Common Philosophy
- **Allocation-centric**: Focus on managing resource allocations
- **Admin-driven**: Admins control most workflows
- **Integration-heavy**: Connect to many external systems
- **Reporting-focused**: Track grants, publications, impact

### Unity Portal Philosophy
- **User-centric**: Focus on user self-service
- **PI-driven**: PIs manage their own groups
- **LDAP-centric**: LDAP is source of truth
- **Access-focused**: Getting users onto the cluster

### Technology Evolution

- **Berkeley**: Modernizing with comprehensive REST API, expiring tokens
- **WUSTL**: Modern frontend with React, microservices patterns
- **Harvard**: Multiple storage backend integrations, gRPC
- **UBCCR**: Automation via signals, OpenLDAP integration
- **Unity**: Staying with PHP, leveraging Shibboleth

---

## 6. Conclusions

### ColdFront Ecosystem

The ColdFront ecosystem shows healthy diversity with institutions extending the platform for their specific needs:

1. **Berkeley (MyBRC/MyLRC)** - Most extensive divergence with full REST API, billing, and user management features
2. **Harvard (FASRC)** - Multiple storage system integrations and custom billing (IFX)
3. **WUSTL** - Modern frontend technologies and comprehensive Qumulo integration
4. **UBCCR** - Upstream maintains automation features and OpenLDAP integration

### Unity Portal Position

Unity Portal serves a **different niche** entirely:
- Pre-allocation user onboarding
- Simple group-based access control
- SSH key management focus
- Lighter weight for institutions not needing full allocation management

It's more comparable to COmanage or similar identity/group management tools than to ColdFront.

### Integration Opportunities

There could be value in:
1. ColdFront importing users from Unity (Unity handles onboarding, ColdFront handles allocations)
2. Unity reading group information from ColdFront (ColdFront as source of truth for allocations)
3. Both systems updating the same LDAP backend but managing different aspects

However, running both systems in parallel might create confusion about which is the "source of truth" for different data elements.

---

## Appendix: Repository Locations

| Institution | Local Path | GitHub Repository |
|-------------|------------|-------------------|
| **UBCCR Upstream** | `/ubccr/coldfront/` | [https://github.com/ubccr/coldfront](https://github.com/ubccr/coldfront) |
| **Berkeley MyBRC/MyLRC** | `/coldfront/` | [https://github.com/ucb-rit/coldfront](https://github.com/ucb-rit/coldfront) |
| **Harvard FASRC** | `/harvard/coldfront/` | [https://github.com/fasrc/coldfront](https://github.com/fasrc/coldfront) |
| **WUSTL** | `/wustl/coldfront-wustl-fork/` | [https://github.com/WashU-IT-RIS/coldfront-wustl-fork](https://github.com/WashU-IT-RIS/coldfront-wustl-fork) |
| **CCI-MOC** | `/cci-moc/coldfront/` | [https://github.com/CCI-MOC/coldfront](https://github.com/CCI-MOC/coldfront) |
| **SUM** | `/sum/coldfront_pr_base/` | [https://github.com/SouthernMethodistUniversity/coldfront_pr_base](https://github.com/SouthernMethodistUniversity/coldfront_pr_base) |
| **UMass Unity** | `/umass/unity-web-portal/` | [https://github.com/UnityHPC/unity-web-portal](https://github.com/UnityHPC/unity-web-portal) |

---

*Analysis Date: October 2025*
*Repositories analyzed as of their current state in the portals directory*

