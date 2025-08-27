# Module Overview

This document describes the major modules in the Carbon monorepo and how they communicate.

## Applications

### `apps/erp`

Main enterprise resource planning (ERP) interface built with Remix and React. Depends on many packages for authentication, database access, UI components and jobs. Communicates primarily with `@carbon/auth`, `@carbon/database`, `@carbon/react`, and background services such as `@carbon/jobs` and `@carbon/stripe`.

### `apps/mes`

Manufacturing execution system (MES) focused on shop‑floor operations. Shares the same infrastructure as the ERP app and reuses authentication, database, and UI packages.

### `apps/starter`

Minimal starter application demonstrating how to bootstrap a new product on top of the Carbon packages. Useful when extracting or building new features.

### `apps/academy`

Training and documentation site that showcases how to use the Carbon platform. Uses the same shared packages as other apps.

## Packages

### `@carbon/auth`

Authentication and user/session management services. Provides server utilities for handling sessions, companies, users and verification. Communicates with `@carbon/database` to persist credentials, `@carbon/kv` for session storage, and `@carbon/react` for client components.

### `@carbon/database`

Database access layer powered by Supabase. Exposes migrations, seed scripts and generated types for other packages. Acts as the central data source used by most modules.

### `@carbon/documents`

PDF and document generation utilities. Each consuming app supplies fonts and can generate reports or invoices. Often used by `@carbon/jobs` to create output files.

### `@carbon/ee`

Enterprise integrations such as Slack, exchange rates, Onshape and Paperless Parts. Provides adapters that other modules can call to interact with these services.

### `@carbon/form`

Reusable form primitives and state management built on React and Remix. Supplies components used across all apps for consistent form handling.

### `@carbon/jobs`

Background job workers built on Trigger.dev. Jobs can access authentication, database, document and notification packages. Enables asynchronous tasks like sending emails or generating PDFs.

### `@carbon/kv`

Key‑value layer backed by Upstash Redis. Offers caching and session storage for other modules, particularly `@carbon/auth` and background workers.

### `@carbon/lib`

Cross‑cutting server libraries, including helpers for sending email via Resend and messaging via Slack. Consumed by jobs, Stripe integration and other server code.

### `@carbon/logger`

Shared logging utilities and configuration. Used across server packages to provide consistent structured logs.

### `@carbon/notifications`

Wrapper around the Novu notification service. Provides an interface for queuing in‑app and email notifications. Commonly triggered from jobs or application code.

### `@carbon/react`

Design system and React component library. Supplies UI primitives, charts, editors and form controls consumed by all applications.

### `@carbon/remix`

Utilities and helpers for Remix applications. Bridges server and client concerns, and provides conventions for routing, forms and data loading.

### `@carbon/stripe`

Stripe payment integration. Handles server‑side webhooks and client helpers. Depends on auth, database, jobs, kv and lib packages to store billing data and trigger follow‑up actions.

### `@carbon/tailwind`

Centralised Tailwind CSS configuration shared by React and app packages to ensure consistent styling.

### `@carbon/tsconfig`

Base TypeScript configuration used throughout the monorepo.

### `@carbon/utils`

General utility functions and schema helpers used across the codebase.

## Module Communication

The applications are thin layers composed from the shared packages. A typical request flows from an app through `@carbon/auth` for session validation, then to `@carbon/database` for persistence. Background jobs are queued via `@carbon/jobs`, which may use `@carbon/documents` for PDF generation or `@carbon/notifications` for messaging. External integrations (Stripe, Slack, Onshape) are accessed through dedicated packages like `@carbon/stripe` or `@carbon/ee`.
