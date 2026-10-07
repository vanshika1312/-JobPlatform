# SIB Placement Platform

A job placement marketplace for **Skillinabox** — connecting trained students in fashion design and makeup with employers (brands, boutiques, fashion labels, D2C companies).

## Quick Start (Demo Mode)

The app runs in **demo mode** out of the box — no Supabase setup required.

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) and sign in with:

| Role    | Email                    | Password      |
|---------|--------------------------|---------------|
| Student | `priya@example.com`      | `Student@123` |
| Admin   | `admin@skillinabox.com`  | `Admin@123`   |
| Employer | `boutique@bunaai.com`   | `Employer@123` |

Use the **Student** / **Admin** / **Employer** tabs on the login page. Each tab validates the account role before redirecting.

## Production Setup (Supabase)

1. Create a [Supabase](https://supabase.com) project
2. Copy `.env.example` to `.env.local` and fill in your credentials:

```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

3. Run `supabase/schema.sql`, `supabase/fix-auth.sql`, and `supabase/employer-portal.sql` in the Supabase SQL Editor
4. Seed demo accounts (requires `SUPABASE_SERVICE_ROLE_KEY` in `.env.local`):

```bash
npm run seed:users
```

**Quick fix if admin login fails:** run `supabase/confirm-demo-users.sql` in the SQL Editor. This confirms the admin email without needing the service role key.

Get `SUPABASE_SERVICE_ROLE_KEY` from **Project Settings → API → service_role** (never commit this key).
   - Student:
     ```json
     {
       "role": "student",
       "full_name": "Priya Sharma",
       "course": "Fashion Styling & Visual Merchandising",
       "city": "Mumbai",
       "current_location": "Mumbai",
       "skills": ["Visual Merchandising", "Fashion Styling", "Adobe Photoshop", "Social Media Marketing", "Content Creation"],
       "certification_verified": true
     }
     ```
   - Admin: `{"role": "admin", "full_name": "Admin User"}`
5. Run `supabase/seed.sql` to populate employers and jobs

## Architecture

```
src/
├── app/              # Next.js App Router pages & layouts
│   ├── login/
│   ├── student/      # Student portal routes
│   └── admin/        # Admin portal routes
├── components/       # Shared UI, layouts, JobCard
├── hooks/            # Auth context (Supabase + demo fallback)
├── lib/              # Supabase client, mock data
├── views/            # Page view components (imported by app routes)
└── types/            # TypeScript interfaces
```

## Routes

### Student Portal (`/student/*`)
| Route | Purpose |
|-------|---------|
| `/student/home` | Personalised welcome dashboard |
| `/student/onboarding` | First-login profile setup (pre-filled from enrollment) |
| `/student/jobs` | Matched job feed with filters |
| `/student/jobs/:id` | Job detail + one-click apply |
| `/student/applications` | Kanban application tracker |
| `/student/profile` | Profile builder with strength ring |
| `/student/notifications` | Grouped notification feed |

### Admin Portal (`/admin/*`)
| Route | Purpose |
|-------|---------|
| `/admin/dashboard` | KPIs, charts, stalled students |
| `/admin/jobs` | Job listing management |
| `/admin/students` | Resume bank |
| `/admin/applications` | Draggable kanban board |
| `/admin/employers` | Employer verification & management |
| `/admin/settings` | Scorer weights, WhatsApp templates |

## Tech Stack

- **Next.js 15** (App Router) + **React 19** + **TypeScript**
- **Tailwind CSS 4** — brand orange `#F26522` (saffron/navy tokens), cyan, gold
- **Supabase** — auth, database, RLS policies
- **Recharts** — admin dashboard charts
- **Lucide React** — icons

## Build

```bash
npm run build
npm run start
```
