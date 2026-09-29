---
theme: ./theme
colorSchema: light
title: Role Extensibility in openedx-authz
info: |
  ## Role Extensibility in openedx-authz
  In-flight projects and a proposal for Custom Roles.

  by Rodrigo Mendez
drawings:
  persist: false
transition: slide-left
comark: true
duration: 20min
---

# Role Extensibility in <code>openedx-authz</code>

Development Proposal for the AuthZ Custom Roles feature.

**Rodrigo Mendez** - Staff Software Engineer @ **WGU**

---
layout: two-cols-header
---

# Two related in-flight projects

Both extend roles — but for different audiences

::left::

<div v-click v-motion :initial="{ opacity: 0, y: 30 }" :enter="{ opacity: 1, y: 0, transition: { duration: 400 } }" class="rounded-lg border-t-4 border-[#00BBF9] shadow p-5 mr-4" style="background: var(--color-white)">
  <div class="flex items-center gap-3 mb-4">
    <carbon-code class="text-[#00BBF9] font-size-3xl flex-shrink-0" />
    <div>
      <div class="text-lg"><b>AuthZ Extensibility</b></div>
      <a href="https://github.com/openedx/openedx-authz/issues/408" target="_blank" class="text-sm">#408</a>
    </div>
  </div>
  <div class="flex items-center gap-3 mb-3">
    <carbon-user class="text-[#00BBF9] font-size-lg flex-shrink-0" />
    <span><b>Developer</b> oriented</span>
  </div>
  <div class="flex items-center gap-3 mb-3">
    <carbon-document class="text-[#00BBF9] font-size-lg flex-shrink-0" />
    <span>New <b>roles and permissions</b> via YAML files</span>
  </div>
  <div class="flex items-center gap-3 mb-3">
    <carbon-cloud-upload class="text-[#00BBF9] font-size-lg flex-shrink-0" />
    <span>Contributed at <b>deployment time</b></span>
  </div>
  <div class="flex items-center gap-3">
    <carbon-in-progress class="text-[#00BBF9] font-size-lg flex-shrink-0" />
    <span>Fully defined — <b>currently in progress</b></span>
  </div>
</div>

::right::

<div v-click v-motion :initial="{ opacity: 0, y: 30 }" :enter="{ opacity: 1, y: 0, transition: { duration: 400 } }" class="rounded-lg border-t-4 border-[#9D0054] shadow p-5" style="background: var(--color-white)">
  <div class="flex items-center gap-3 mb-4">
    <carbon-user-admin class="text-[#9D0054] font-size-3xl flex-shrink-0" />
    <div>
      <div class="text-lg"><b>AuthZ Custom Roles</b></div>
      <a href="https://github.com/openedx/openedx-authz/issues/465" target="_blank" class="text-sm">#465</a>
    </div>
  </div>
  <div class="flex items-center gap-3 mb-3">
    <carbon-user-multiple class="text-[#9D0054] font-size-lg flex-shrink-0" />
    <span><b>Admin (superuser)</b> oriented</span>
  </div>
  <div class="flex items-center gap-3 mb-3">
    <carbon-user-role class="text-[#9D0054] font-size-lg flex-shrink-0" />
    <span>New <b>roles only</b> — no new permissions</span>
  </div>
  <div class="flex items-center gap-3 mb-3">
    <carbon-touch-interaction class="text-[#9D0054] font-size-lg flex-shrink-0" />
    <span>Defined from a <b>UI, at runtime</b></span>
  </div>
  <div class="flex items-center gap-3">
    <carbon-pen class="text-[#9D0054] font-size-lg flex-shrink-0" />
    <span>In <b>definition and planning</b></span>
  </div>
</div>

<div v-click v-motion :initial="{ opacity: 0, y: 20 }" :enter="{ opacity: 1, y: 0, transition: { duration: 400 } }" class="absolute bottom-14 left-0 right-0 mx-14 flex items-center justify-center gap-3 px-5 py-3 rounded-lg border border-gray-200 border-l-4 border-l-[#9D0054] shadow-sm" style="background: var(--color-white)">
  <carbon-arrow-right class="text-[#9D0054] font-size-xl flex-shrink-0" />
  <span>From here on, these slides focus on <b class="text-[#9D0054]">AuthZ Custom Roles</b>.</span>
</div>

---
layout: two-cols-header
---

# Custom Roles

Product requirements

::left::

<div v-click class="flex items-center gap-3 mb-4">
<carbon-add-alt class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b class="text-[#9D0054]">Superuser:</b> create new roles from the <b>Admin Console</b></span>
</div>

<div v-click class="flex items-center gap-3 mb-4">
<carbon-copy class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b class="text-[#9D0054]">Superuser:</b> <b>copy an existing role</b> as a starting point</span>
</div>

<div v-click class="flex items-center gap-3 mb-4">
<carbon-categories class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b class="text-[#9D0054]">Superuser:</b> assign <b>Courses and Libraries</b> permissions</span>
</div>

<div v-click class="flex items-center gap-3">
<carbon-link class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b class="text-[#9D0054]">Superuser:</b> auto-add <b>required or implied</b> permissions</span>
</div>

::right::

<div v-click class="flex items-center gap-3 mb-4">
<carbon-trash-can class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b class="text-[#9D0054]">Superuser:</b> <b>remove</b> custom roles</span>
</div>

<div v-click class="flex items-center gap-3 mb-4">
<carbon-edit class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b class="text-[#9D0054]">Superuser:</b> <b>edit</b> custom roles</span>
</div>

<div v-click class="flex items-center gap-3 mb-4">
<carbon-warning-alt class="text-[#9D0054] font-size-xl flex-shrink-0" />
<span><b class="text-[#9D0054]">Superuser:</b> <b>warned</b> when a role is already assigned</span>
</div>

<div v-click class="flex items-center gap-3">
<carbon-fit-to-screen class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b class="text-[#9D0054]">Admin:</b> assign any scope to a role that supports both — incl. <b>platform</b> &amp; <b>org</b> level <span class="opacity-70">(when the admin user has enough permissions)</span></span>
</div>

<div v-click class="mt-6 text-sm">
Early mockup — <a href="https://claude.ai/artifact/TpmWLJKdAr2NVASMcJaeUf" target="_blank">assignment wizard</a>
</div>

---

# Development plan

Main epics

<div v-click v-motion :initial="{ opacity: 0, y: 20 }" :enter="{ opacity: 1, y: 0 }" class="flex items-center justify-between gap-4 mb-4 p-4 rounded-lg border border-gray-200 border-l-4 border-l-[#9D0054] shadow-sm" style="background: var(--color-white)">
  <div class="flex items-center gap-3">
    <carbon-scale class="text-[#9D0054] font-size-2xl flex-shrink-0" />
    <div>
      <div><b>Support more than one scope type per role</b></div>
      <div class="text-sm opacity-70">Backend and frontend</div>
    </div>
  </div>
  <span class="text-sm font-bold px-3 py-1 rounded bg-[#9D0054] text-white">Large (?)</span>
</div>

<div v-click v-motion :initial="{ opacity: 0, y: 20 }" :enter="{ opacity: 1, y: 0 }" class="flex items-center justify-between gap-4 mb-4 p-4 rounded-lg border border-gray-200 border-l-4 border-l-[#00BBF9] shadow-sm" style="background: var(--color-white)">
  <div class="flex items-center gap-3">
    <carbon-flow class="text-[#00BBF9] font-size-2xl flex-shrink-0" />
    <div>
      <div><b>Permission dependency mapping</b></div>
      <div class="text-sm opacity-70">Backend</div>
    </div>
  </div>
  <span class="text-sm font-bold px-3 py-1 rounded bg-[#00BBF9] text-white">Small</span>
</div>

<div v-click v-motion :initial="{ opacity: 0, y: 20 }" :enter="{ opacity: 1, y: 0 }" class="flex items-center justify-between gap-4 p-4 rounded-lg border border-gray-200 border-l-4 border-l-[#00BBF9] shadow-sm" style="background: var(--color-white)">
  <div class="flex items-center gap-3">
    <carbon-build-tool class="text-[#00BBF9] font-size-2xl flex-shrink-0" />
    <div>
      <div><b>Custom roles implementation</b></div>
      <div class="text-sm opacity-70">Backend and frontend</div>
    </div>
  </div>
  <span class="text-sm font-bold px-3 py-1 rounded bg-[#00BBF9] text-white">Medium</span>
</div>

---
layout: two-cols-header
---

# More than one scope type per role

<span class="text-sm px-2 py-1 rounded bg-[#9D0054] text-white">Large (?)</span> &nbsp; Tasks draft

::left::

<div v-click class="flex items-center gap-3 mb-3">
<carbon-document class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>ADR</b> on naming conventions</span>
</div>

<div v-click class="flex items-center gap-3 mb-3">
<carbon-document class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>ADR</b> on supporting multiple scope types per role</span>
</div>

<div v-click class="flex items-center gap-3 mb-3">
<carbon-data-table class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span>Model tracking assignments — <b>one assignment → many Casbin rows</b></span>
</div>

<div v-click class="flex items-center gap-3 mb-3">
<carbon-search class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>Spike:</b> identify the APIs that need updating</span>
</div>

<div v-click class="flex items-center gap-3">
<carbon-api class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span>Update APIs to use the new model <i>(size this — could be big)</i></span>
</div>

::right::

<div v-click class="flex items-center gap-3 mb-2">
<carbon-search class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>Spike:</b> what changes in the Admin Console</span>
</div>

<div v-click class="ml-8 text-sm opacity-80 mb-3">
· Scope-assignment logic in the wizard<br/>
· The filters<br/>
· The assignments list views<br/>
· Identify what else
</div>

<div v-click class="flex items-center gap-3 mb-3">
<carbon-application class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>Frontend:</b> implement required changes</span>
</div>

<div v-click class="flex items-center gap-3">
<carbon-help class="text-[#9D0054] font-size-xl flex-shrink-0" />
<span>Alternative approach — <b>change the Casbin model?</b></span>
</div>

---

# Permission dependency mapping

<span class="text-sm px-2 py-1 rounded bg-[#00BBF9] text-white">Small</span> &nbsp; Tasks draft

<div v-click class="flex items-center gap-3 mb-5 mt-4">
<carbon-document class="text-[#00BBF9] font-size-2xl flex-shrink-0" />
<span><b>ADR:</b> extend the YAML schema definition to support this</span>
</div>

<div v-click class="flex items-center gap-3 mb-5">
<carbon-code class="text-[#00BBF9] font-size-2xl flex-shrink-0" />
<span><b>Implementation:</b> extend the YAML schema</span>
</div>

<div v-click class="flex items-center gap-3">
<carbon-data-base class="text-[#00BBF9] font-size-2xl flex-shrink-0" />
<span><b>Extend DB tables</b> to map dependencies</span>
</div>

---
layout: two-cols-header
---

# Custom roles implementation

<span class="text-sm px-2 py-1 rounded bg-[#00BBF9] text-white">Medium</span> &nbsp; Tasks draft

::left::

<div v-click class="flex items-center gap-3 mb-2">
<carbon-document class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>ADR:</b> how custom roles are persisted</span>
</div>

<div v-click class="ml-8 text-sm opacity-80 mb-3">
· Extend <code>authzroledefinition</code>: <code>is_custom</code>, naming rules, <code>created_by</code><br/>
· Use <code>authzrolepermission</code> for permission assignment<br/>
· Audit table + logic, like <code>roleassignmentaudit</code>
</div>

<div v-click class="flex items-center gap-3 mb-2">
<carbon-save class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>Implementation:</b> custom roles persistence</span>
</div>

<div v-click class="flex items-center gap-3 mb-2">
<carbon-document class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>ADR:</b> custom roles API</span>
</div>

<div v-click class="ml-8 text-sm opacity-80">
· Support copying an existing role<br/>
· Permission-dependency identification
</div>

::right::

<div v-click class="flex items-center gap-3 mb-3">
<carbon-api class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>Implementation:</b> custom roles API</span>
</div>

<div v-click class="flex items-center gap-3 mb-3">
<carbon-magic-wand class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>Frontend:</b> <a href="https://claude.ai/artifact/TpmWLJKdAr2NVASMcJaeUf" target="_blank">custom roles creation wizard</a></span>
</div>

<div v-click class="flex items-center gap-3">
<carbon-list-boxes class="text-[#00BBF9] font-size-xl flex-shrink-0" />
<span><b>Frontend:</b> custom roles management view</span>
</div>

---
layout: two-cols-header
---

# Decisions that may change the estimate

::left::

<div v-click v-motion :initial="{ opacity: 0, x: -20 }" :enter="{ opacity: 1, x: 0 }" class="p-5 rounded-lg border border-gray-200 border-l-4 border-l-[#9D0054] shadow-sm mt-4 mr-4" style="background: var(--color-white)">
  <div class="flex items-center gap-3 mb-3">
    <carbon-time class="text-[#9D0054] font-size-2xl flex-shrink-0" />
    <b>Scheduling</b>
  </div>
  <span>Should we <b>delay</b> support for more than one scope type per role?</span>
</div>

::right::

<div v-click v-motion :initial="{ opacity: 0, x: 20 }" :enter="{ opacity: 1, x: 0 }" class="p-5 rounded-lg border border-gray-200 border-l-4 border-l-[#9D0054] shadow-sm mt-4" style="background: var(--color-white)">
  <div class="flex items-center gap-3 mb-3">
    <carbon-branch class="text-[#9D0054] font-size-2xl flex-shrink-0" />
    <b>Approach</b>
  </div>
  <span>Change the <b>Casbin model</b> instead? Avoids a new assignment model and reworking the API.</span>
</div>
