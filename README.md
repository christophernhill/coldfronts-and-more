# Research Computing Portal Analysis

This repository contains multiple forks of ColdFront and the Unity Web Portal. This document analyzes the unique features in each downstream fork that haven't been pushed upstream to UBCCR, and identifies features unique to the Unity portal.

## Repository Overview

- **ubccr/coldfront** - The upstream ColdFront repository
- **coldfront/** - UC Berkeley's MyBRC/MyLRC User Portal fork
- **harvard/coldfront** - Harvard FASRC's fork
- **wustl/coldfront-wustl-fork** - Washington University in St. Louis fork
- **nerc/** - NERC project repositories (cloud plugin and Kubernetes deployment)
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

### 1.4 NERC (New England Research Cloud) - Cloud-Native ColdFront

NERC provides a **cloud-native deployment** of ColdFront with a comprehensive cloud resource management plugin. This is not a traditional fork but rather a containerized deployment with production-ready Kubernetes manifests and a sophisticated OpenStack/OpenShift integration plugin.

#### Repository Structure

The NERC project consists of two repositories:

1. **coldfront-nerc** - Containerized deployment and Kubernetes manifests
2. **coldfront-plugin-cloud** - Cloud resource allocation plugin

#### Unique Features

1. **Cloud Plugin for ColdFront** ([coldfront-plugin-cloud](https://github.com/nerc-project/coldfront-plugin-cloud))
   
   **OpenStack Integration:**
   - Full OpenStack cloud resource provisioning
   - Automated project (tenant) creation
   - Quota management for:
     - Compute instances, vCPUs, RAM
     - Block storage volumes
     - Object storage (Swift)
     - Floating IPs and networks
   - Keystone federation support (OIDC)
   - Application credential authentication
   - Multi-cloud support (multiple OpenStack instances)
   - Cinder (block storage) client integration
   - Neutron (networking) client integration
   - Nova (compute) client integration
   - Swift (object storage) client integration

   **OpenShift/Kubernetes Integration:**
   - OpenShift project (namespace) provisioning
   - Resource quota management:
     - CPU limits and requests
     - Memory limits and requests
     - Ephemeral storage quotas
     - Persistent volume claims
     - GPU quotas (NVIDIA GPUs)
   - Storage class quotas:
     - NESE storage (Ceph RBD)
     - IBM Spectrum Scale storage
   - Role-based access control (RBAC) automation
   - ClusterRole assignment
   - Default LimitRange configuration
   - Project labels for OpenDataHub/ModelMesh integration
   - Requires external [openshift-acct-mgt API service](https://github.com/cci-moc/openshift-acct-mgt)

   **OpenShift Virtualization Support:**
   - Separate `openshift_vm` resource type
   - GPU-specific quotas for VMs:
     - NVIDIA A100 SXM4 40GB
     - NVIDIA V100 (GV100GL)
     - NVIDIA H100 SXM5 80GB
   - Virtual machine resource management alongside containers

   **ESI (Elastic Secure Infrastructure) Support:**
   - Bare metal provisioning via OpenStack Ironic
   - Network-focused quotas
   - Specialized for bare metal cloud resources

   **Quota Unit System:**
   - "Unit of computing" concept to bundle multiple quotas
   - Single multiplier for multiple resource types
   - Simplifies quota requests in UI
   - Automatic distribution to individual resource attributes

2. **Management Commands:**
   - `add_openstack_resource` - Register OpenStack clouds
   - `add_openshift_resource` - Register OpenShift clusters
   - `register_cloud_attributes` - Setup resource attribute types
   - `calculate_storage_gb_hours` - Storage usage calculation
   - `convert_swift_quota_to_gib` - Swift quota conversion
   - `count_gpu_usage` - GPU utilization tracking
   - `list_cloud_allocations` - Allocation listing
   - `migrate_fields_of_science` - Data migration utility
   - `update_eula` - EULA management
   - `validate_allocations` - Allocation validation

3. **Containerized Deployment** ([coldfront-nerc](https://github.com/nerc-project/coldfront-nerc))
   
   **Multi-stage Docker Build:**
   - Python 3.12 slim-bullseye base
   - Builder stage for compilation dependencies
   - Final minimal runtime image
   - Virtual environment isolation
   - Custom email templates
   - Patch system for upstream fixes:
     - API URLs addition
     - Allocation status fixes
     - Active needs renewal status
     - Allocation change request signals

   **Kubernetes Manifests:**
   - Production-ready Kustomize configurations
   - Base manifests:
     - ColdFront application deployment
     - Redis for caching/queuing
     - Static files serving
     - Invoice cron job
     - Django Q cluster for async tasks
   - Overlay configurations:
     - Development (with MariaDB)
     - Staging environment
     - Production environment with HA PostgreSQL
   - High Availability PostgreSQL:
     - PostgreSQL Operator integration
     - Automated backups to S3
     - PgBackRest configuration
   - Ingress configurations
   - ConfigMaps and Secrets management

4. **Authentication & Authorization:**
   - OpenID Connect (OIDC) via `mozilla-django-oidc`
   - Mokey OIDC plugin for group sync
   - Keycloak integration:
     - User search directly from Keycloak
     - `coldfront_plugin_keycloak_usersearch` plugin
     - Environment-based configuration
   - Multiple identity provider support

5. **Testing Infrastructure:**
   - Comprehensive unit tests
   - Functional tests for:
     - OpenStack allocations
     - OpenShift allocations
     - OpenShift VM allocations
     - ESI allocations
   - Mock testing with functional test mode
   - CI/CD scripts for:
     - DevStack (OpenStack)
     - MicroStack (lightweight OpenStack)
     - Microshift (lightweight OpenShift)
     - Ubuntu setup automation
     - RadosGW testing
   - Vagrant-based development environment
   - Pre-commit hooks with Ruff linter

6. **Production Features:**
   - Invoice generation (cron job)
   - Storage GB-hours calculation for billing
   - GPU usage tracking and reporting
   - Allocation validation commands
   - Field of science migration tools
   - EULA update management
   - S3-compatible storage for invoices and backups

#### Architecture Philosophy

NERC's approach differs from other forks:

- **Cloud-first**: Designed specifically for OpenStack and OpenShift
- **Kubernetes-native**: Full production Kubernetes deployment
- **Plugin-based**: Core ColdFront remains unchanged, all cloud logic in plugin
- **Multi-cloud**: Can manage multiple OpenStack and OpenShift instances
- **Container-first**: Docker-based deployment, not traditional server installation
- **Automated provisioning**: Direct API integration with cloud platforms
- **Standards-compliant**: Uses OpenStack and Kubernetes standard APIs

#### Key Differentiators

1. **Only solution with production OpenShift integration**
2. **Only solution with OpenShift Virtualization support**
3. **Only solution with ESI (bare metal cloud) support**
4. **Most comprehensive Kubernetes deployment manifests**
5. **Fully containerized with multi-stage builds**
6. **High availability PostgreSQL with automated backups**
7. **GPU-aware quota management (specific GPU models)**
8. **Storage class-aware quota management**

---

### 1.5 UBCCR Upstream-Specific Features (Not in Other Forks)

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

| Feature | UBCCR | Berkeley | Harvard | WUSTL | NERC | CCI-MOC | Unity |
|---------|-------|----------|---------|-------|------|---------|-------|
| **Allocations** | ✓ | ✓✓ | ✓ | ✓ | ✓✓ | ✓ | ✗ |
| **Projects** | ✓ | ✓✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| **Grants** | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| **Publications** | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| **Billing** | ✗ | ✓✓ | ✓ (IFX) | ✗ | ✓ | ✗ | ✗ |
| **REST API** | ✗ | ✓✓ | ✓ | ✗ | ✓ | ✗ | ✓ |
| **Cloud Integration** | ✗ | ✗ | ✗ | ✗ | ✓✓ | ✗ | ✗ |
| **Container Deploy** | ✗ | ✗ | ✗ | ✗ | ✓✓ | ✗ | ✗ |
| **SSH Key Mgmt** | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓✓ |
| **PI Self-Service** | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓✓ |

### Plugin Features

| Plugin/Integration | UBCCR | Berkeley | Harvard | WUSTL | NERC | CCI-MOC | Unity |
|-------------------|-------|----------|---------|-------|------|---------|-------|
| **FreeIPA** | ✓ | ✗ | ✓ | ✓ | ✗ | ✓ | ✗ |
| **Slurm** | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | N/A |
| **XDMoD** | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | N/A |
| **iQuota** | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | N/A |
| **LDAP User Search** | ✓ | ✓ | ✓ | ✓ | ✗ | ✓ | N/A |
| **Mokey OIDC** | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| **Departments** | ✗ | ✓✓ | ✓ | ✗ | ✗ | ✗ | ✗ |
| **Hardware Procure** | ✗ | ✓✓ | ✗ | ✗ | ✗ | ✗ | ✗ |
| **Auto Compute Alloc** | ✓✓ | ✗ | ✗ | ✗ | ✗ | ✗ | N/A |
| **Project OpenLDAP** | ✓✓ | ✗ | ✗ | ✗ | ✗ | ✗ | N/A |
| **Qumulo** | ✗ | ✗ | ✗ | ✓✓ | ✗ | ✗ | N/A |
| **OpenStack** | ✗ | ✗ | ✗ | ✗ | ✓✓ | ✗ | N/A |
| **OpenShift** | ✗ | ✗ | ✗ | ✗ | ✓✓ | ✗ | N/A |
| **Keycloak** | ✗ | ✗ | ✗ | ✗ | ✓✓ | ✗ | ✗ |
| **ESI (Bare Metal)** | ✗ | ✗ | ✗ | ✗ | ✓✓ | ✗ | N/A |
| **Kubernetes Deploy** | ✗ | ✗ | ✗ | ✗ | ✓✓ | ✗ | N/A |
| **Isilon** | ✗ | ✗ | ✓✓ | ✗ | ✗ | ✗ | N/A |
| **VAST** | ✗ | ✗ | ✓✓ | ✗ | ✗ | ✗ | N/A |
| **LFS (Lustre)** | ✗ | ✗ | ✓✓ | ✗ | ✗ | ✗ | N/A |
| **IFX Billing** | ✗ | ✗ | ✓✓ | ✗ | ✗ | ✗ | N/A |
| **Starfish** | ✗ | ✗ | ✓✓ | ✗ | ✗ | ✗ | N/A |
| **SlurmREST** | ✗ | ✗ | ✓✓ | ✗ | ✗ | ✗ | N/A |
| **FASRC Custom** | ✗ | ✗ | ✓✓ | ✗ | ✗ | ✗ | N/A |

*Legend: ✓ = Present, ✓✓ = Unique/Enhanced, ✗ = Not present, N/A = Not applicable*

---

## 4. Recommendations for Upstream Contributions

### High Priority - Broadly Useful

These features would benefit the entire ColdFront community:

1. **Integration with Internet2 Tools (NEW - Highest Strategic Priority):**
   - **Grouper integration plugin** - Self-service group management, delegation to PIs
   - **COmanage integration plugin** - Identity lifecycle, external collaborator management
   - Combined architecture for identity → groups → allocations workflow
   - Would address self-service needs across ALL ColdFront deployments
   - Reference implementations needed; NERC architecture provides good model

2. **From Berkeley:**
   - REST API framework (allocation, project, user endpoints)
   - Statistics module for job tracking
   - Allocation renewal request workflow
   - Departments plugin (with generic backends)
   - Hardware procurements plugin (with generic data sources)

3. **From NERC:**
   - **Cloud plugin** - OpenStack/OpenShift integration
   - Kubernetes deployment manifests and Dockerfile
   - Quota unit system concept
   - GPU-aware quota management
   - Storage class-aware quotas
   - Containerization best practices
   - OIDC/Keycloak user search integration

4. **From WUSTL:**
   - Qumulo plugin architecture could be generalized for other storage systems
   - React frontend pattern for modern UI components
   - Service-oriented architecture pattern
   - ITSM integration pattern

5. **From Harvard:**
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

## 5. Potential Integration with Internet2 Identity and Access Management Tools

ColdFront projects could significantly benefit from integration with Internet2's mature identity and access management systems, particularly [Grouper](https://github.com/Internet2/grouper) and [COmanage Registry](https://github.com/Internet2/comanage-registry). These tools address common challenges in research computing environments and could complement ColdFront's allocation management capabilities.

### 5.1 Internet2 Grouper Integration

[Grouper](https://www.internet2.edu/products-services/trust-identity-middleware/grouper/) is an enterprise access management system designed for highly distributed management environments common to universities and research institutions.

#### What Grouper Provides

- **Distributed Group Management**: Enables delegation of group management to PIs, department heads, and project leaders
- **Hierarchical Group Structure**: Supports nested groups and complex organizational structures
- **Single Point of Control**: One change to group membership automatically propagates to all connected applications
- **Role-Based Access Control (RBAC)**: Sophisticated permission models for who can manage groups
- **REST Web Services API**: Modern API for integration with other applications
- **Audit Trail**: Complete history of group changes and membership modifications
- **Multiple Applications Support**: Groups can be synchronized to LDAP, Active Directory, mailing lists, wikis, and more

#### How Grouper Could Enhance ColdFront

1. **Self-Service Group Management**
   - PIs could manage their own research group memberships
   - Project members could be added/removed without admin intervention
   - Automatic synchronization of Grouper groups to ColdFront project memberships
   - Reduces administrative burden on center staff

2. **Complex Membership Scenarios**
   - Support for nested groups (e.g., Department → Lab → Project → Subproject)
   - Composite groups (unions, intersections, exclusions)
   - Automatic group membership based on attributes (e.g., all faculty in a department)
   - Time-limited memberships with automatic expiration

3. **Multi-Level Approval Workflows**
   - PI approves member joining their group in Grouper
   - ColdFront automatically detects new member
   - Resource allocation workflows can leverage existing Grouper approvals
   - Reduces duplicate approval processes

4. **Cross-Institution Collaboration**
   - Grouper excels at federated environments
   - External collaborators can be added to groups via federation
   - ColdFront allocations could extend to federated users
   - Particularly valuable for multi-institutional research projects

5. **Integration Architecture**
   ```
   Grouper (Group Management) 
      ↓ (REST API / LDAP Sync)
   ColdFront (Resource Allocation)
      ↓ (Provisioning)
   Resources (HPC, Storage, Cloud)
   ```

#### Example Use Cases

**Current State**: PI emails admin to add 3 students to their allocation. Admin manually adds them to ColdFront, then provisions accounts on HPC systems.

**With Grouper**: PI adds students to their Grouper group. ColdFront detects the change and automatically initiates allocation addition workflow. Upon approval, provisioning happens automatically.

**Current State**: Department wants all faculty to have access to a shared storage allocation. Admin maintains list manually.

**With Grouper**: Department maintains a "Faculty" group in Grouper with automatic membership rules. ColdFront allocation is linked to this group. New faculty are automatically eligible; departing faculty automatically lose access.

### 5.2 COmanage Registry Integration

[COmanage Registry](https://www.internet2.edu/comanage) is a lifecycle management system and identity registry designed to track complex affiliations and identity relationships within collaborative organizations.

#### What COmanage Registry Provides

- **Identity Lifecycle Management**: Track people through hiring, role changes, and departure
- **Organizational Registry**: Maintain organizational structure and relationships
- **Multiple Affiliations**: Handle complex scenarios (person is staff + student + researcher)
- **Self-Service Enrollment**: People can register themselves with appropriate approvals
- **Collaboration Management**: Manage virtual organizations and research collaborations
- **Identity Linking**: Connect institutional identities with external identities (ORCID, etc.)
- **REST APIs**: Modern API for integration
- **LDAP Provisioning**: Can provision to LDAP directories

#### How COmanage Could Enhance ColdFront

1. **Onboarding Automation**
   - New users enroll in COmanage with PI sponsorship
   - COmanage provisions basic identity information
   - ColdFront receives vetted identity data
   - Eliminates manual account creation processes

2. **Affiliation Management**
   - Track multiple roles (faculty + researcher, student + RA)
   - ColdFront allocation eligibility based on affiliations
   - Automatic removal from allocations when affiliations end
   - Grace periods for departing users

3. **Collaboration Lifecycle**
   - Virtual organizations (VOs) managed in COmanage
   - VO membership automatically reflects in ColdFront
   - Project creation driven by COmanage collaboration setup
   - End of collaboration triggers allocation cleanup

4. **External Collaborator Management**
   - COmanage handles external user vetting and sponsorship
   - Integration with InCommon/eduGAIN federations
   - ColdFront receives validated external identities
   - Compliance with security requirements

5. **Identity Federation**
   - Link institutional accounts with external identities (ORCID, Google, etc.)
   - Support multiple authentication methods
   - Consistent identity across systems
   - Better tracking of researcher outputs

#### Example Use Cases

**Current State**: New postdoc arrives. Manual process to create accounts in HR system, email, HPC, and ColdFront. Takes 2 weeks.

**With COmanage**: PI sponsors postdoc in COmanage. Identity information flows automatically to all systems including ColdFront. Postdoc can access resources on day one.

**Current State**: Multi-institutional grant team needs shared resources. Each institution's members need accounts at the lead institution's HPC center.

**With COmanage**: Virtual organization created in COmanage. External members vetted through federation. ColdFront project automatically includes all VO members regardless of home institution.

### 5.3 Combined Integration Architecture

The most powerful approach combines all three systems:

```
┌─────────────────────────────────────────────────────────────┐
│                     COmanage Registry                       │
│              (Identity & Affiliation Lifecycle)              │
│  • Self-service enrollment                                   │
│  • Institutional & external identities                       │
│  • Virtual organizations                                     │
│  • Affiliation tracking                                      │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ↓ (Identity & VO data via API/LDAP)
┌─────────────────────────────────────────────────────────────┐
│                         Grouper                             │
│                  (Group Management)                          │
│  • PI-managed research groups                                │
│  • Department groups                                         │
│  • Automated group membership from COmanage VOs              │
│  • Nested group structures                                   │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ↓ (Group membership via API/LDAP)
┌─────────────────────────────────────────────────────────────┐
│                       ColdFront                             │
│              (Resource Allocation Management)                │
│  • Project creation from Grouper groups                      │
│  • Allocation requests from PIs                              │
│  • Automatic member provisioning                             │
│  • Usage tracking and reporting                              │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ↓ (Provisioning via APIs/Scripts)
┌─────────────────────────────────────────────────────────────┐
│                    Compute Resources                        │
│  HPC Clusters • Cloud Platforms • Storage Systems           │
│  • Slurm accounts from ColdFront                             │
│  • OpenStack projects from ColdFront (NERC plugin)           │
│  • POSIX groups from Grouper                                 │
└─────────────────────────────────────────────────────────────┘
```

### 5.4 Benefits of Integrated Approach

**For PIs and Researchers:**
- Self-service management of their groups
- Faster onboarding of new team members
- One place to manage membership (Grouper)
- Automatic resource access based on group membership

**For Research Computing Staff:**
- Reduced manual intervention
- Automated provisioning workflows
- Better audit trails
- Fewer support tickets for account management

**For Institutions:**
- Compliance with security requirements
- Proper offboarding when people leave
- Support for multi-institutional collaborations
- Scalable to large user bases

**For External Collaborators:**
- Simplified access to resources
- Federation support (InCommon/eduGAIN)
- No need for separate institutional accounts
- Consistent identity across institutions

### 5.5 Current ColdFront Gaps That Grouper/COmanage Address

1. **Berkeley's Departments Plugin** → Better served by COmanage's organizational registry
2. **Unity Portal's PI Self-Service** → Grouper provides more sophisticated group management
3. **Manual User Approval Workflows** → COmanage handles identity vetting and sponsorship
4. **Limited Federation Support** → COmanage excels at federated identity
5. **Complex Group Hierarchies** → Grouper's nested groups and composite groups
6. **Affiliation Tracking** → COmanage tracks multiple complex affiliations

### 5.6 Implementation Considerations

**Which ColdFront Forks Would Benefit Most?**

1. **Berkeley (MyBRC/MyLRC)**: Already has API infrastructure; could integrate Grouper groups as data source for departments plugin
2. **Harvard (FASRC)**: Could replace custom LDAP integration with standard Grouper→LDAP provisioning
3. **NERC**: COmanage could provide identity vetting for multi-institutional OpenStack/OpenShift projects
4. **WUSTL**: Grouper integration could simplify Active Directory synchronization in Qumulo plugin
5. **UBCCR**: Auto-compute allocation plugin could trigger from Grouper group creation

**Integration Points:**

- **COmanage → ColdFront**: REST API for identity data, affiliation information, VO membership
- **Grouper → ColdFront**: REST API or LDAP sync for group membership
- **ColdFront → Grouper**: Optionally write allocation approvals back as Grouper permissions
- **All Three → LDAP**: Common provisioning target for downstream systems

**Deployment Models:**

1. **Full Stack**: COmanage + Grouper + ColdFront (maximum automation)
2. **Grouper Only**: Add self-service group management to existing ColdFront
3. **COmanage Only**: Improve identity lifecycle without changing group management
4. **Hybrid**: Use COmanage for external collaborators, existing systems for internal users

### 5.7 Recommended Next Steps

For institutions considering this integration:

1. **Pilot with Single Use Case**: Start with one research group using Grouper for self-service
2. **Leverage Existing Infrastructure**: If already using COmanage or Grouper, integrate that first
3. **API-First Approach**: Use REST APIs rather than LDAP sync for better real-time updates
4. **Community Collaboration**: Multiple institutions implementing this could share integration code
5. **Contribute Upstream**: Grouper/COmanage integration plugins could benefit entire ColdFront community

**Reference Implementations:**

While no ColdFront fork currently has full Grouper/COmanage integration, the NERC plugin's architecture (external API integration, automated provisioning) provides a good model for how such integration could work.

---

## 6. Notable Differences in Approach

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

### Grouper/COmanage Philosophy
- **Identity-first**: Build on solid identity foundation
- **Delegation-focused**: Enable distributed management
- **Standards-based**: Use standard protocols and APIs
- **Collaboration-oriented**: Support complex multi-institutional scenarios

### Technology Evolution

- **Berkeley**: Modernizing with comprehensive REST API, expiring tokens
- **WUSTL**: Modern frontend with React, microservices patterns
- **Harvard**: Multiple storage backend integrations, gRPC
- **NERC**: Cloud-native with Kubernetes, containerization
- **UBCCR**: Automation via signals, OpenLDAP integration
- **Unity**: Staying with PHP, leveraging Shibboleth
- **Internet2**: Enterprise-grade identity and group management (Grouper/COmanage)

---

## 7. Conclusions

### ColdFront Ecosystem

The ColdFront ecosystem shows healthy diversity with institutions extending the platform for their specific needs:

1. **Berkeley (MyBRC/MyLRC)** - Most extensive divergence with full REST API, billing, and user management features
2. **Harvard (FASRC)** - Multiple storage system integrations and custom billing (IFX)
3. **WUSTL** - Modern frontend technologies and comprehensive Qumulo integration
4. **NERC** - Cloud-native deployment with OpenStack/OpenShift integration, Kubernetes-first architecture
5. **UBCCR** - Upstream maintains automation features and OpenLDAP integration

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

### ColdFront Forks and Portals

| Institution | Local Path | GitHub Repository |
|-------------|------------|-------------------|
| **UBCCR Upstream** | `/ubccr/coldfront/` | [https://github.com/ubccr/coldfront](https://github.com/ubccr/coldfront) |
| **Berkeley MyBRC/MyLRC** | `/coldfront/` | [https://github.com/ucb-rit/coldfront](https://github.com/ucb-rit/coldfront) |
| **Harvard FASRC** | `/harvard/coldfront/` | [https://github.com/fasrc/coldfront](https://github.com/fasrc/coldfront) |
| **WUSTL** | `/wustl/coldfront-wustl-fork/` | [https://github.com/WashU-IT-RIS/coldfront-wustl-fork](https://github.com/WashU-IT-RIS/coldfront-wustl-fork) |
| **NERC - ColdFront Container** | `/nerc/coldfront-nerc/` | [https://github.com/nerc-project/coldfront-nerc](https://github.com/nerc-project/coldfront-nerc) |
| **NERC - Cloud Plugin** | `/nerc/coldfront-plugin-cloud/` | [https://github.com/nerc-project/coldfront-plugin-cloud](https://github.com/nerc-project/coldfront-plugin-cloud) |
| **CCI-MOC** | `/cci-moc/coldfront/` | [https://github.com/CCI-MOC/coldfront](https://github.com/CCI-MOC/coldfront) |
| **SUM** | `/sum/coldfront_pr_base/` | [https://github.com/SouthernMethodistUniversity/coldfront_pr_base](https://github.com/SouthernMethodistUniversity/coldfront_pr_base) |
| **UMass Unity** | `/umass/unity-web-portal/` | [https://github.com/UnityHPC/unity-web-portal](https://github.com/UnityHPC/unity-web-portal) |

### Related Internet2 Identity and Access Management Tools

| Tool | Purpose | GitHub Repository | Documentation |
|------|---------|-------------------|---------------|
| **Internet2 Grouper** | Enterprise group and access management | [https://github.com/Internet2/grouper](https://github.com/Internet2/grouper) | [Grouper Wiki](https://spaces.at.internet2.edu/display/Grouper/Grouper+Wiki+Home) |
| **COmanage Registry** | Identity lifecycle and collaboration management | [https://github.com/Internet2/comanage-registry](https://github.com/Internet2/comanage-registry) | [COmanage Wiki](https://spaces.at.internet2.edu/display/COmanage/Home) |

---

*Analysis Date: October 2025*
*Repositories analyzed as of their current state in the portals directory*

