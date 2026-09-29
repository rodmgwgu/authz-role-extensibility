# Custom Roles Development Proposal

*A document companion to the slides. Purpose: give the community enough context
to weigh in on the open decisions before implementation begins.*

Slides (live): https://rodmgwgu.github.io/authz-role-extensibility/

---

## TL;DR

Two projects extend roles in `openedx-authz`, for different audiences and at
different times:

- **AuthZ Extensibility** ([#408](https://github.com/openedx/openedx-authz/issues/408))
  lets developers contribute new roles **and** permissions via YAML at
  deployment time. Fully defined, in progress.
- **AuthZ Custom Roles** ([#465](https://github.com/openedx/openedx-authz/issues/465))
  lets an admin compose new roles (from **existing** permissions) at runtime,
  from the Admin Console. In definition and planning — this document is about
  this one.

Custom Roles breaks into three epics. **Two decisions gate the estimate and the
shape of the work**, and we want community input on them before we commit:

1. Do we delay support for more than one scope type per role?
2. Do we solve multi-scope by adding a new assignment model, or by changing the
   Casbin model directly?

Jump to [Decisions to make first](#decisions-to-make-first).

---

## Two in-flight projects

Both projects are about making roles extensible, but they target different
audiences and operate at different points in the lifecycle. They are
complementary, not competing.

### AuthZ Extensibility — [#408](https://github.com/openedx/openedx-authz/issues/408)

- **Audience:** developers.
- **What it does:** define new **roles and permissions** via YAML files.
- **When:** contributed at **deployment time**.
- **Status:** fully defined, currently in progress.

This is the definition layer around Casbin: contribution/discovery, metadata,
validation, and role-agnostic APIs. New scope types and new subject types are
explicitly out of scope for #408.

### AuthZ Custom Roles — [#465](https://github.com/openedx/openedx-authz/issues/465)

- **Audience:** admins / superusers.
- **What it does:** define new **roles only** — no new permissions. A custom
  role is a new grouping of permissions that already exist.
- **When:** created dynamically **at runtime**, from a UI, with no redeploy.
- **Status:** in definition and planning.

**From here on, this document is about AuthZ Custom Roles.**

---

## Custom Roles — product requirements

Written as user stories. The actor is the **superuser** except where noted.

- As a superuser, I should be able to create new roles from the Admin Console.
- As a superuser, I should be able to copy an existing role as a starting point
  for my custom role.
- As a superuser, I should be able to assign both **Courses** and **Libraries**
  permissions to my custom role.
- As a superuser, when adding a permission that requires or implies another, the
  other permission should be automatically added.
- As a superuser, I should be able to remove custom roles.
- As a superuser, I should be able to edit custom roles.
- As a superuser, I should be warned when editing or removing a custom role that
  is already assigned to users.
- As an admin user, I should be able to assign any of the supported scopes to a
  given role and user, **including at platform level and org level** (assuming I
  have enough permissions to do so).

Early mockup for the assignment wizard:
https://claude.ai/artifact/TpmWLJKdAr2NVASMcJaeUf

---

## Development plan — the epics

| Epic | Area | Size |
| --- | --- | --- |
| Support more than one scope type per role | Backend + frontend | Large (?) |
| Permission dependency mapping | Backend | Small |
| Custom roles implementation | Backend + frontend | Medium |

The "Large (?)" on the first epic is deliberate — its size, and whether we need
it at all right now, is one of the [open decisions](#decisions-to-make-first).

### Epic 1 — Support more than one scope type per role

Today a role assignment is tied to a single scope type. Custom roles that mix
Courses and Libraries permissions need an assignment that can span more than one
scope type. Draft tasks:

- ADR on naming conventions.
- ADR on supporting more than one scope type per role.
- Create a model to track assignments (one assignment → many Casbin rows).
- **Spike:** identify the APIs that would need updating.
- Update APIs to use the new model. *(Needs sizing — this could be large.)*
- **Spike:** what needs to change in the Admin Console to support the new model:
  - the logic to assign scopes in the assignment wizard,
  - the filters,
  - the assignments list views,
  - identify what else.
- Frontend: implement the required changes.
- Open question: should we consider an alternative approach — **changing the
  Casbin model** — instead? (See the decisions section.)

### Epic 2 — Permission dependency mapping

Some permissions require or imply others; the UI has to add those automatically.
Draft tasks:

- ADR: extend the YAML schema definition to support dependency declarations.
- Implementation: extend the YAML schema.
- Extend the DB tables to map the dependencies.

### Epic 3 — Custom roles implementation

The core of the feature: persisting custom roles and exposing an API and UI to
manage them. Draft tasks:

- **ADR:** how custom roles are persisted.
  - Extend `openedx_authz_authzroledefinition`: add `is_custom`, enforce custom
    naming conventions, and perhaps add a `created_by` column.
  - Use `openedx_authz_authzrolepermission` for permission assignment.
  - Define an audit table and logic — a history table similar to
    `roleassignmentaudit`.
- Implementation: custom roles persistence.
- **ADR:** the custom roles API.
  - Support copying an existing role.
  - Support permission-dependency identification.
- Implementation: the custom roles API.
- Frontend: implement the
  [custom roles creation wizard](https://claude.ai/artifact/TpmWLJKdAr2NVASMcJaeUf).
- Frontend: implement the custom roles management view.

---

## Decisions to make first

These two decisions define the rest of the work. We want to settle them —
ideally with community input — before committing to the estimate, because each
one changes which tasks above are needed and how big they are.

### Decision 1 — Scheduling: do we delay multi-scope support?

**Should we delay support for more than one scope type per role?**

This is the "Large (?)" epic. Deferring it would let the smaller, better-defined
epics (permission dependency mapping, custom roles implementation) land sooner,
at the cost of custom roles initially being limited to a single scope type.

- **Delay it:** faster first delivery; custom roles start single-scope.
- **Keep it in scope:** custom roles support Courses + Libraries from day one,
  but the first release is larger and later.

### Decision 2 — Approach: new assignment model vs. changing the Casbin model

**Should we support multiple scope types by adding a new assignment model, or by
changing the Casbin model directly?**

- **New assignment model** (the current plan): introduce a model that tracks one
  assignment mapping to many Casbin rows. This is additive but requires reworking
  the APIs that read and write assignments.
- **Change the Casbin model:** adjust the underlying Casbin model so a single
  assignment can already carry more than one scope type. This could **avoid
  creating a new model and avoid much of the API rework** — at the cost of
  changing a lower-level, more fundamental piece.

The two decisions interact: if we change the Casbin model (Decision 2), the size
and even the necessity of parts of Epic 1 change, which feeds back into whether
we delay it (Decision 1).

For more detail on the specific implementation options for supporting more than
one scope type per role, see
[this comment on #466](https://github.com/openedx/openedx-authz/issues/466#issuecomment-5840208438).

---

## What we're asking the community

1. On **Decision 1** — is single-scope-first an acceptable first release, or is
   multi-scope a hard requirement for the initial version?
2. On **Decision 2** — are there reasons to prefer (or avoid) changing the Casbin
   model that we should weigh before choosing the assignment-model approach?
3. Anything in the requirements or epics that's missing, mis-scoped, or conflicts
   with other in-flight work.

*Please leave comments on this page, or on
[#465](https://github.com/openedx/openedx-authz/issues/465).*
