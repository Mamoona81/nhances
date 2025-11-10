# NHANES Interactive Visualization Platform - Design Guidelines

## Design Approach: Design System (MUI-Based Scientific)
**Rationale**: Data-heavy, research-focused application requiring consistency, accessibility, and professional scientific aesthetic. Following Material Design principles with customizations for scientific data presentation.

## Core Design Principles
1. **Scientific Credibility**: Clean, professional interface that conveys authority and accuracy
2. **Information Clarity**: Data-dense layouts with clear hierarchy and scannable content
3. **Educational Focus**: Progressive disclosure to guide non-expert researchers through complex data
4. **Interaction Confidence**: Clear affordances for interactive elements (clickable nodes, zoomable graphs)

## Typography
- **Primary Font**: Roboto (via MUI default) - excellent for data-dense interfaces
- **Monospace Font**: 'Roboto Mono' or 'Courier New' for data file names, variable codes (e.g., DEMO_J, LBXTC)
- **Hierarchy**:
  - Page Titles: variant="h4", fontWeight 700
  - Section Headers: variant="subtitle1", fontWeight 600
  - Body Text: variant="body2" for descriptions
  - Data Labels: variant="caption" or "body2" for chart labels
  - Code/Variables: component="code", fontFamily 'monospace', fontSize '0.875rem'

## Layout System
**Spacing**: Use MUI's spacing scale (theme.spacing) consistently:
- Section spacing: `spacing={2}` (16px) between cards/components
- Internal card padding: `p: { xs: 2, sm: 3, md: 4 }` (16px/24px/32px)
- Component gaps: `gap: 2` or `gap: 3`
- Responsive grid: Use Grid2 with size={{ xs: 12, lg: 6/8/4 }}

**Constraints**: 
- Max width for content sections: none (full-width data displays)
- Table containers: maxHeight: 600 for scrollable tables
- Chart containers: min-height: 400px, responsive to viewport

## Component Library

### Navigation & Structure
**Knowledge Graph / Flowchart Component**:
- React Flow canvas with clean white background
- Node styling: Rounded rectangles (borderRadius: 12px) with subtle shadows
- Color coding by component type:
  - Demographics: blue (#1976d2)
  - Dietary: green (#388e3c)  
  - Laboratory: purple (#7b1fa2)
  - Examination: orange (#f57c00)
  - Questionnaire: red (#d32f2f)
- Edge styling: Smooth bezier curves, gray (#9e9e9e), 2px width
- Interactive states: Hover enlarges nodes slightly, shows tooltip with description
- Zoom controls: MUI IconButtons positioned bottom-right
- Clickable nodes open NHANES documentation in new tab

### Data Displays
**Statistical Dashboard Cards**:
- MUI Paper with elevation={0}, variant="outlined"
- Border: 1px solid divider color
- Chart containers: Use Recharts with consistent color palette matching component colors
- Mean/Median displays: Large bold numbers (variant="h3") with labels (variant="caption")
- Comparison layouts: Side-by-side cards in Grid, equal heights

**Tables** (consistent with PageDataVariables pattern):
- Sticky headers with primary.main background
- Alternating row colors (nth-of-type(even) with grey.50)
- Hover state on rows with action.hover background
- Monospace styling for data file names in chips/badges
- "Load More" pattern for pagination

### Search & Discovery
**Variable Search Component**:
- Search TextField with SearchIcon endAdornment
- Autocomplete dropdown with grouped suggestions (by component)
- Filter chips for active filters (component, cycle)
- Results in card grid showing: variable name (monospace), description, component badge
- Click to navigate or expand details inline

### Interactive Elements
**Buttons**:
- Primary actions: variant="contained" with primary color
- Secondary actions: variant="outlined"
- Icon buttons for utilities (copy, download, zoom)
- Small size for inline actions
- Action groups in horizontal Stack with spacing={1.5}

**Chips & Badges**:
- Component tags: size="small", colored by category
- Count indicators: Chip with count number, fontWeight 600
- Status indicators: Dot badges for loading/active states

### Loading States
**Skeletons**: 
- Table rows: 5-10 Skeleton rows with variant="rectangular"
- Cards: Skeleton with height 200-300px, borderRadius matching card
- Charts: Skeleton with aspect ratio 16:9
- Centered CircularProgress for full-component loading

**Empty States**:
- Centered content with emoji/icon (scientific beaker ⚗️, chart 📊)
- Explanatory text (variant="body2", color="text.secondary")
- Suggested action button

## Accessibility
- Maintain WCAG AA contrast ratios (4.5:1 for text, 3:1 for UI components)
- ARIA labels for all interactive graph nodes
- Keyboard navigation support for React Flow canvas
- Alt text for all chart elements via Recharts accessibility props
- Focus indicators on all interactive elements

## Animations
**Minimal & Purposeful**:
- Fade-in for cards on load (Fade component, timeout 500ms)
- Smooth transitions for chart updates (Recharts default)
- Hover scale on graph nodes (transform: scale(1.05))
- No distracting ambient animations

## Color Palette
Do not specify exact hex values - use MUI theme tokens:
- Primary: theme.palette.primary.main (blue tone for scientific authority)
- Secondary: theme.palette.secondary.main (accent for highlights)
- Component categories: Define semantic colors as above
- Backgrounds: paper, grey.50 for subtle fills
- Borders: divider color
- Text: text.primary, text.secondary

## Images
**No hero images required** - this is a data/tool-focused application.

**Icons**: Use Material Icons library via MUI imports:
- Dataset icons: Science, Biotech, LocalDining, Monitor, Assessment
- Action icons: Search, ZoomIn, ZoomOut, Download, ContentCopy, OpenInNew

## Integration Pattern
**File Structure**:
```
src/
  pages/
    PageNHANESVisualization.tsx (new main page)
  components/
    nhanes/
      KnowledgeGraph.tsx
      DietaryStatsChart.tsx  
      VariableSearchPanel.tsx
      CycleOverviewCard.tsx
```

**Consistent Props Interface**:
```
{ dataset: string, cycle: string, loading?: boolean, error?: string | null }
```

**Styling**: Import MUI components, use sx prop for all styling (no external CSS), optionally add Tailwind utilities for rapid spacing adjustments where MUI verbose.