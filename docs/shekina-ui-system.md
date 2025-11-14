# Shekina Legal UI System

Dark legaltech design specification for Shekina Legal Associates. Use this document as the single source of truth for designers and engineers building the product across web and native surfaces.

---

## 1. Design Language

### 1.1 Color Tokens (Dark Legal Theme)

| Token | Hex / RGBA | Usage |
| ----- | ---------- | ----- |
| `--sl-bg-main` | `#020817` | App background, large canvases |
| `--sl-bg-surface` | `#020f1f` | Cards, modals, shells |
| `--sl-bg-elevated` | `#041627` | Popovers, dropdowns, sticky bars |
| `--sl-primary` | `#145DA0` | Buttons, highlights, links |
| `--sl-primary-soft` | `rgba(20,93,160,0.16)` | Background tint for focus/selection |
| `--sl-border-subtle` | `#1f2937` | Card borders, dividers |
| `--sl-text-main` | `#F9FAFB` | Primary text |
| `--sl-text-muted` | `#9CA3AF` | Secondary text, meta data |
| `--sl-danger` | `#EF4444` | Error states, overdue badges |
| `--sl-success` | `#22C55E` | Success banners, paid statuses |
| `--sl-warning` | `#FACC15` | Warning alerts, upcoming deadlines |

Color rules:
- Keep contrasts at WCAG AA; never use pure white backgrounds.
- Use subtle gradients (`#020817` → `#041627`) for hero surfaces; avoid busy textures.
- Danger/success/warning badges always sit on `--sl-bg-elevated` and use `--sl-text-main` for copy.

### 1.2 Typography
- Brandmark: classic serif such as "Cormorant Garamond" or "Playfair Display" for logo lockups.
- UI font stack: `"Inter", "Source Sans 3", "system"`.
- Headings: H1 28–32px semibold; H2 22–24px semibold; H3 18–20px medium.
- Body: 15/24px regular; Muted/meta text 12px medium with `--sl-text-muted`.
- Letter spacing: +4% on uppercase navigation labels for gravitas.

### 1.3 Iconography & Motion
- Use thin-stroke line icons (Lucide or Phosphor). Stroke 1.5px, rounded joins.
- Micro-interactions capped at 200ms; ease-out for entrances, ease-in for exits.
- Focus ring: 2px `--sl-primary` outer ring with `--sl-primary-soft` glow.

### 1.4 Spacing & Radius
- Base grid 4px; standard gutters 24px desktop / 16px tablet.
- Radii: 4px inputs, 10px cards, 999px pills.
- Shadows: `0 12px 30px rgba(2,8,23,0.6)` for elevated surfaces.

---

## 2. Layout Shell

### Sidebar
- Fixed 260px left rail, background `--sl-bg-main`.
- Content: dove icon + lockup, nav groups (Dashboard, Cases, Clients, Calendar, Documents, Billing, Reports, Settings).
- Hover uses `--sl-primary-soft`; active route uses `--sl-primary` text and 2px accent bar.

### Top Bar
- Inside main content region, sticky. Contains breadcrumb, page title, global search (wide input with command icon), notifications bell, avatar dropdown.
- Search uses command-k pattern with keyboard hint.

### Content Area
- Max width 1440px; wrap cards in `max-w-6xl` container for readability.
- Cards use `--sl-bg-surface`, `border` `--sl-border-subtle`, `rounded-xl`, `shadow-lg`.
- Page padding: 32px desktop, 20px tablet, 16px mobile.

---

## 3. Screen Blueprints

### 3.1 Authentication
**Login**
- Split view: left hero (60%) overlay with dove watermark, tagline “Integrity. Precision. Justice.” and CTA to learn more.
- Right card: email + password inputs, remember me toggle, forgot password link, primary sign-in button, social proof footer.

**Forgot Password**
- Single column form, success state shows inline success banner (`--sl-success`).

### 3.2 Dashboard
- KPI row: 4 `StatCard` components with icons, delta labels.
- Middle split: Upcoming Cases list (date badge, title, matter type, advocate initials, court) and Tasks (checkbox list with due badges).
- Bottom: Case List preview table with columns specified and inline `View Case` button. Entire row clickable.

### 3.3 Cases
**List**
- Filters drawer with search, status dropdown, practice area, date range, advocate. Chip summary of applied filters.
- Table columns: Case Number, Case Title, Client Name, Practice Area, Jurisdiction, Stage, Next Hearing, Age.
- Bulk actions bar appears when rows selected: Assign Advocate, Change Status, Export (CSV/PDF).
- Primary CTA top-right: “New Case”.

**Detail**
- Sticky header: case title, number, status badge, actions (Add Note, Upload Document, Add Task, overflow with Close/Archive).
- Main area uses tabs: Overview, Timeline, Documents, Hearings, Tasks, Billing, Notes & Research, Team.
- Overview contains summary card, parties list, key dates, quick metrics.
- Timeline shows chronological events with expandable nodes.
- Documents tab offers list/grid toggle, filters, preview panel.
- Hearings tab table plus “Add Hearing”.
- Tasks tab offers list + optional Kanban board.
- Billing tab: time entries + invoices.
- Notes & Research: rich text editor, pinning.
- Team tab: roster with roles/contact.
- Right sidebar: client info card, risk level (color-coded), quick actions, watchers list.

**Create/Edit Case Form**
- 5-step stepper (Case Basics → Parties & Opposing Counsel → Scheduling → Assignment & Billing → Review & Save).
- Side summary of progress, validation per step, autosave drafts.

### 3.4 Clients
- List filters for Individual/Corporate, Sector, Active/Dormant.
- Columns: Client Name, Type, Primary Contact, Open Cases, Fees billed (YTD), Last activity. CTA “New Client”.
- Detail page header with rating badge (Key Account, Strategic, Standard). Tabs: Overview, Cases, Billing, Documents, Notes.

### 3.5 Calendar
- Month/Week/Day toggle, color-coded events (Hearings blue, Deadlines orange, Meetings green).
- Sidebar filters by advocate, team, practice area.
- Event click opens preview with link to source Case/Client.

### 3.6 Documents
- Left folder tree (Global folders + Case folders). Top filters: search, document type, uploaded by, date range.
- Main list with Name, Linked Case/Client, Type, Version, Last Modified. Actions: Upload, New Folder, Generate from Template. Preview pane for PDFs.

### 3.7 Billing & Invoices
- Tabs: Time Entries, Invoices, Trust Accounts.
- Time entries table (Date, User, Case, Activity, Hours, Rate, Amount, Status). Bulk approval.
- Invoices table with status badges (Paid, Part-Paid, Overdue) and “New Invoice”, “Export Statement” actions.

### 3.8 Reports
- Grid of analytics cards (Fees by Practice Area, Fees by Advocate, Realisation rates, Case cycle time, Court success rate). Each card opens detailed view with filters.

### 3.9 Settings & Administration
- Sectioned forms: Firm Profile, Users & Roles (RBAC), Practice Areas, Court List, Billing Rates, Notification Preferences, Integrations (Email, Calendar, DMS).

---

## 4. Component Library

### Layout & Navigation
- `<Shell>` wraps `<Sidebar />` + `<Topbar />` + `<PageHeader title breadcrumbs actions />`.
- Breadcrumb component with optional icons and overflow collapse.

### Data & Content
- `<StatCard />`, `<DataTable />` with sorting/filtering/pagination, `<Timeline />`, `<Badge variant="status" />` (variants: `open`, `in-progress`, `closed`, `on-hold`, `success`, `warning`, `danger`).

### Forms
- Inputs: `TextInput`, `TextArea`, `Select`, `MultiSelect`, `DatePicker`, `DateRangePicker`, `Toggle`, `Checkbox`, `RadioGroup`, `RichTextEditor`.
- Form layout uses responsive two-column grids; helper text sits beneath labels in muted tone.

### Feedback & Overlays
- `Alert` (success, warning, danger), `Toast`, `Modal/Dialog`, `EmptyState` modules.

### Miscellaneous
- `Avatar` (initials fallback), `Tabs`, `Accordion`, `Stepper`, `Pill`, `ProgressBar`, `Tag`.

---

## 5. Interaction Guidelines
- Keyboard: `Cmd/Ctrl + K` opens command palette; `Shift + /` opens shortcuts modal.
- Table rows clickable; actions appear on hover.
- Drag-and-drop for task kanban and document folders (desktop only).
- Autosave forms every 30 seconds; show last saved timestamp.

---

## 6. Example React + Tailwind Layout

```tsx
// app/(legal)/layout.tsx
import { ReactNode } from "react";
import { Sidebar } from "@/components/layout/sidebar";
import { Topbar } from "@/components/layout/topbar";

export default function LegalLayout({ children }: { children: ReactNode }) {
  return (
    <div className="min-h-screen bg-[#020817] text-slate-50 flex">
      <Sidebar />
      <div className="flex-1 flex flex-col">
        <Topbar />
        <main className="px-6 py-4 lg:px-10 lg:py-8 bg-[#020817]">
          <div className="max-w-6xl mx-auto space-y-6">{children}</div>
        </main>
      </div>
    </div>
  );
}
```

```tsx
// components/layout/sidebar.tsx
import {
  Home,
  Briefcase,
  Users,
  CalendarDays,
  FileText,
  Receipt,
  BarChart2,
  Settings
} from "lucide-react";
import Link from "next/link";

const navItems = [
  { href: "/legal/dashboard", label: "Dashboard", icon: Home },
  { href: "/legal/cases", label: "Cases", icon: Briefcase },
  { href: "/legal/clients", label: "Clients", icon: Users },
  { href: "/legal/calendar", label: "Calendar", icon: CalendarDays },
  { href: "/legal/documents", label: "Documents", icon: FileText },
  { href: "/legal/billing", label: "Billing", icon: Receipt },
  { href: "/legal/reports", label: "Reports", icon: BarChart2 },
  { href: "/legal/settings", label: "Settings", icon: Settings }
];

export function Sidebar() {
  return (
    <aside className="w-64 hidden md:flex flex-col border-r border-[#1f2937] bg-[#020f1f]">
      <div className="h-16 px-6 flex items-center border-b border-[#1f2937]">
        <div className="flex items-center gap-2">
          <span className="inline-flex h-8 w-8 items-center justify-center rounded-full border border-slate-500">
            <span className="text-xs">🕊</span>
          </span>
          <div className="leading-tight">
            <div className="text-sm font-semibold tracking-[0.18em] uppercase">Shekina</div>
            <div className="text-[10px] text-slate-400 tracking-[0.2em] uppercase">Legal Associates</div>
          </div>
        </div>
      </div>
      <nav className="flex-1 px-3 py-4 space-y-1">
        {navItems.map((item) => (
          <Link
            key={item.href}
            href={item.href}
            className="flex items-center gap-3 px-3 py-2 text-sm rounded-lg text-slate-300 hover:text-slate-50 hover:bg-[#041627]"
          >
            <item.icon className="h-4 w-4" />
            <span>{item.label}</span>
          </Link>
        ))}
      </nav>
    </aside>
  );
}
```

Use similar patterns for dashboard content components (`StatCard`, `UpcomingCases`, `TaskList`, `CaseListPreview`).

---

## 7. Data Model (SQL-ready)

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY,
  first_name TEXT NOT NULL,
  last_name TEXT NOT NULL,
  email CITEXT UNIQUE NOT NULL,
  role TEXT CHECK (role IN ('Admin','Partner','Associate','Pupil','Clerk')),
  avatar_url TEXT,
  status TEXT DEFAULT 'active'
);

CREATE TABLE clients (
  id UUID PRIMARY KEY,
  name TEXT NOT NULL,
  type TEXT CHECK (type IN ('Individual','Corporate')),
  sector TEXT,
  primary_contact JSONB,
  rating TEXT,
  last_activity TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE cases (
  id UUID PRIMARY KEY,
  case_number TEXT,
  title TEXT NOT NULL,
  matter_type TEXT,
  practice_area TEXT,
  jurisdiction TEXT,
  stage TEXT,
  status TEXT,
  court TEXT,
  client_id UUID REFERENCES clients(id),
  responsible_advocate UUID REFERENCES users(id),
  opened_at DATE,
  next_hearing DATE,
  limitation_date DATE,
  value_of_claim NUMERIC,
  risk_level TEXT
);

CREATE TABLE case_parties (
  id UUID PRIMARY KEY,
  case_id UUID REFERENCES cases(id),
  name TEXT,
  role TEXT,
  contact JSONB
);

CREATE TABLE hearings (
  id UUID PRIMARY KEY,
  case_id UUID REFERENCES cases(id),
  hearing_date DATE,
  court TEXT,
  judge TEXT,
  purpose TEXT,
  outcome TEXT,
  next_steps TEXT
);

CREATE TABLE tasks (
  id UUID PRIMARY KEY,
  case_id UUID REFERENCES cases(id),
  title TEXT,
  description TEXT,
  owner_id UUID REFERENCES users(id),
  due_date DATE,
  priority TEXT,
  status TEXT,
  linked_entity JSONB
);

CREATE TABLE documents (
  id UUID PRIMARY KEY,
  case_id UUID REFERENCES cases(id),
  client_id UUID REFERENCES clients(id),
  name TEXT,
  doc_type TEXT,
  version INTEGER DEFAULT 1,
  uploaded_by UUID REFERENCES users(id),
  uploaded_at TIMESTAMPTZ DEFAULT now(),
  storage_url TEXT,
  tags TEXT[]
);

CREATE TABLE time_entries (
  id UUID PRIMARY KEY,
  case_id UUID REFERENCES cases(id),
  user_id UUID REFERENCES users(id),
  entry_date DATE,
  activity TEXT,
  hours NUMERIC,
  rate NUMERIC,
  status TEXT DEFAULT 'Unbilled'
);

CREATE TABLE invoices (
  id UUID PRIMARY KEY,
  invoice_no TEXT UNIQUE,
  client_id UUID REFERENCES clients(id),
  case_id UUID REFERENCES cases(id),
  amount NUMERIC,
  issue_date DATE,
  due_date DATE,
  status TEXT,
  pdf_url TEXT
);

CREATE TABLE watchers (
  id UUID PRIMARY KEY,
  case_id UUID REFERENCES cases(id),
  user_id UUID REFERENCES users(id)
);
```

Extend with `notifications`, `audit_logs`, or `trust_accounts` as required.

---

## 8. Implementation Checklist
- [ ] Configure Tailwind tokens (CSS variables) for core palette.
- [ ] Build layout shell (sidebar, topbar, page header) and add responsive breakpoints.
- [ ] Scaffold screen routes (Dashboard, Cases, Clients, Calendar, Documents, Billing, Reports, Settings).
- [ ] Implement component library primitives (StatCard, DataTable, Timeline, Alerts, Stepper, etc.).
- [ ] Integrate forms with validation + autosave.
- [ ] Connect to database schema outlined above; set up seed data for demo purposes.
- [ ] Add accessibility testing (keyboard traps, color contrast, aria labels).

This UI system now provides a full specification for Shekina Legal Associates covering visual language, layout, screen IA, reusable components, and implementable data structures.
