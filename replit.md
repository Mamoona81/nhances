# NHANES Interactive Visualization Platform

## Overview

This is a data visualization and exploration platform for NHANES (National Health and Nutrition Examination Survey) data. The application provides interactive tools for researchers and analysts to explore complex survey datasets through knowledge graphs, variable search, statistical visualizations, and bookmarking capabilities. Built with a focus on scientific credibility and educational accessibility, it helps users navigate the multi-component structure of NHANES cycles (Demographics, Dietary, Laboratory, Examination, Questionnaire) and understand survey design requirements for proper statistical analysis.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework:** React 18+ with TypeScript, using Vite as the build tool and development server.

**UI Component System:** Built on shadcn/ui components (Radix UI primitives) with Tailwind CSS for styling. The design follows a "New York" style variant with a neutral color palette and customized theme variables defined in CSS custom properties. Components use a consistent spacing scale and border radius system.

**Design Philosophy:** Scientific/research-focused interface inspired by Material Design principles (as noted in design_guidelines.md referencing MUI patterns), but implemented using shadcn/ui components. Emphasizes data density, clear information hierarchy, and professional aesthetic suitable for academic/research contexts.

**State Management:** React Query (@tanstack/react-query) for server state, React Context API for bookmarks persistence (with localStorage), and local component state using hooks.

**Routing:** Wouter for client-side routing (lightweight React router alternative).

**Key Visualization Libraries:**
- @xyflow/react for interactive knowledge graph/flowchart visualizations
- recharts for statistical charts (bar charts, responsive containers)
- Custom components for dietary statistics and data exploration

**Responsive Design:** Mobile-first approach with breakpoint-based layouts using Tailwind's responsive utilities.

### Backend Architecture

**Server Framework:** Express.js with TypeScript running in ESM mode.

**Development Setup:** Custom Vite middleware integration for HMR (Hot Module Replacement) and development tooling. Production builds bundle the server with esbuild.

**API Structure:** RESTful API design pattern with routes prefixed by `/api`. The routes.ts file serves as the central registration point for all API endpoints.

**Session Management:** Configured to use connect-pg-simple for PostgreSQL-backed session storage (though current implementation uses in-memory storage via MemStorage class).

**Storage Layer Abstraction:** IStorage interface pattern allows swapping between in-memory and database-backed storage implementations without changing route handlers.

### Data Storage Solutions

**Database ORM:** Drizzle ORM configured for PostgreSQL with Neon Database (@neondatabase/serverless) as the provider.

**Schema Management:** Type-safe schema definitions in shared/schema.ts using drizzle-orm and drizzle-zod for validation. Migrations stored in `/migrations` directory.

**Current Data Model:** Minimal schema with users table (id, username, password). The application primarily operates on mock NHANES data defined in shared/nhanesData.ts rather than database-stored survey data.

**Database Configuration:** Connection via DATABASE_URL environment variable, with drizzle-kit configured for schema pushing and migration generation.

**Data Architecture Decision:** NHANES statistical data is currently modeled as TypeScript constants (nhanesCycles, nhanesComponents, nhanesDataFiles, nhanesVariables, dietaryStats) rather than database records. This supports rapid prototyping and provides type safety, though it limits dynamic data updates without code deployment.

### Authentication and Authorization

**Current State:** Basic schema exists (users table with username/password fields) but authentication middleware is not implemented in the current codebase.

**Planned Pattern:** Express session-based authentication with PostgreSQL session store (connect-pg-simple dependency present).

### External Dependencies

**Third-Party Services:**
- Neon Database: Serverless PostgreSQL hosting
- NHANES Documentation Links: External CDC/NHANES website URLs for variable and data file documentation (opened in new tabs via ExternalLink functionality)

**Development Tools:**
- Replit-specific integrations: @replit/vite-plugin-runtime-error-modal, @replit/vite-plugin-cartographer, @replit/vite-plugin-dev-banner (conditional on REPL_ID environment variable)

**Font Loading:** Google Fonts CDN for Roboto and Roboto Mono font families (referenced in client/index.html).

**No External APIs:** The application does not currently fetch NHANES data from CDC APIs; all survey data is statically defined in code.

### Key Architectural Patterns

**Monorepo Structure:** Shared types and schemas between client and server via `shared/` directory with path aliases (@shared/*).

**Type Safety:** Full TypeScript coverage with strict mode enabled. Drizzle-zod bridges ORM schemas to runtime validation.

**Component Organization:** Reusable UI components in client/src/components/ui/ (shadcn pattern), feature components in client/src/components/, and example usage in client/src/components/examples/.

**Export Utilities:** Client-side CSV generation and download capabilities for exporting variables and statistics (no server-side processing required).

**Context-Based Features:** BookmarksContext provides global bookmark state with localStorage persistence, demonstrating the pattern for client-side data persistence needs.

**Progressive Enhancement:** Dark mode toggle functionality built into the UI (theme switching via class-based Tailwind dark mode).