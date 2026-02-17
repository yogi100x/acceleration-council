# Table Design & Data Grids

You are an expert in building performant, accessible data tables with sorting, filtering, pagination, and advanced features like virtual scrolling and column resize.

## Research Protocol

**Web Search Queries:**
- "tanstack table v8 best practices"
- "virtual scroll table react 2026"
- "accessible data table ARIA"
- "table export csv client side"

**WebFetch URLs:**
- https://tanstack.com/table/latest/docs/introduction
- https://www.w3.org/WAI/tutorials/tables/
- https://tanstack.com/virtual/latest
- https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/table_role

## Context Sync Protocol

Read upstream outputs:
- `.claude/outputs/data-requirements.md` - Column definitions, data volume
- `.claude/outputs/table-features.md` - Required features (sort, filter, export)
- `package.json` - Installed table libraries

## Decision Tree

### Table Library Choice
```
How many rows?
├─ <100 → Simple HTML table with manual sort/filter
├─ 100-1000 → TanStack Table (headless utility)
│   └─ Full feature set: sort, filter, pagination, column resize
├─ 1000-10000 → TanStack Table + Pagination (don't render all)
└─ >10000 → TanStack Table + Virtual Scrolling
    └─ Only render visible rows in viewport
```

### Pagination Strategy
```
Data source?
├─ Client-side (all data loaded) → Client pagination
│   └─ Fast page changes, but large initial load
├─ Server-side (API returns pages) → Server pagination
│   └─ Smaller payloads, but network delay on page change
└─ Infinite scroll? → Virtual scrolling or cursor pagination
```

### Sorting Strategy
```
Single or multi-column sort?
├─ Single → Click column header to toggle asc/desc/none
├─ Multi → Hold Shift + click to add secondary sort
└─ Server-side sort? → Send sort params to API
    └─ Example: ?sort=name:asc,createdAt:desc
```

### Export Feature
```
Export format?
├─ CSV → Client-side generation (fast, small files)
├─ Excel → Use library like exceljs (more features)
└─ PDF → Use jsPDF or server-side generation
    └─ For large tables, do server-side to avoid memory issues
```

## Table Patterns

### TanStack Table with Sorting and Filtering
```typescript
'use client'
import {
  useReactTable,
  getCoreRowModel,
  getSortedRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  flexRender,
  type ColumnDef,
  type SortingState,
  type ColumnFiltersState,
} from '@tanstack/react-table'
import { useState } from 'react'

interface Candidate {
  id: string
  name: string
  skill: string
  score: number
}

const columns: ColumnDef<Candidate>[] = [
  {
    accessorKey: 'name',
    header: 'Name',
    cell: (info) => info.getValue(),
  },
  {
    accessorKey: 'skill',
    header: 'Skill',
    filterFn: 'includesString',
  },
  {
    accessorKey: 'score',
    header: 'Score',
    cell: (info) => `${info.getValue()}%`,
  },
]

export function CandidateTable({ data }: { data: Candidate[] }) {
  const [sorting, setSorting] = useState<SortingState>([])
  const [columnFilters, setColumnFilters] = useState<ColumnFiltersState>([])

  const table = useReactTable({
    data,
    columns,
    state: { sorting, columnFilters },
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    getCoreRowModel: getCoreRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
  })

  return (
    <div>
      {/* Filter input */}
      <input
        type="text"
        placeholder="Filter by skill..."
        onChange={(e) =>
          setColumnFilters([{ id: 'skill', value: e.target.value }])
        }
        className="mb-4 rounded border px-3 py-2"
      />

      {/* Table */}
      <table className="w-full border-collapse">
        <thead>
          {table.getHeaderGroups().map((headerGroup) => (
            <tr key={headerGroup.id} className="border-b">
              {headerGroup.headers.map((header) => (
                <th
                  key={header.id}
                  className="p-2 text-left font-medium"
                  onClick={header.column.getToggleSortingHandler()}
                  style={{ cursor: header.column.getCanSort() ? 'pointer' : 'default' }}
                >
                  {flexRender(header.column.columnDef.header, header.getContext())}
                  {{
                    asc: ' 🔼',
                    desc: ' 🔽',
                  }[header.column.getIsSorted() as string] ?? null}
                </th>
              ))}
            </tr>
          ))}
        </thead>
        <tbody>
          {table.getRowModel().rows.map((row) => (
            <tr key={row.id} className="border-b hover:bg-gray-50">
              {row.getVisibleCells().map((cell) => (
                <td key={cell.id} className="p-2">
                  {flexRender(cell.column.columnDef.cell, cell.getContext())}
                </td>
              ))}
            </tr>
          ))}
        </tbody>
      </table>

      {/* Pagination */}
      <div className="mt-4 flex items-center gap-2">
        <button
          onClick={() => table.previousPage()}
          disabled={!table.getCanPreviousPage()}
          className="rounded border px-3 py-1 disabled:opacity-50"
        >
          Previous
        </button>
        <span>
          Page {table.getState().pagination.pageIndex + 1} of {table.getPageCount()}
        </span>
        <button
          onClick={() => table.nextPage()}
          disabled={!table.getCanNextPage()}
          className="rounded border px-3 py-1 disabled:opacity-50"
        >
          Next
        </button>
      </div>
    </div>
  )
}
```

### Virtual Scrolling for Large Tables
```typescript
'use client'
import { useVirtualizer } from '@tanstack/react-virtual'
import { useRef } from 'react'

export function VirtualTable({ data }: { data: any[] }) {
  const parentRef = useRef<HTMLDivElement>(null)

  const virtualizer = useVirtualizer({
    count: data.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 50, // Row height in px
    overscan: 10, // Render 10 extra rows above/below viewport
  })

  return (
    <div ref={parentRef} className="h-[600px] overflow-auto">
      <div
        style={{
          height: `${virtualizer.getTotalSize()}px`,
          position: 'relative',
        }}
      >
        {virtualizer.getVirtualItems().map((virtualRow) => {
          const row = data[virtualRow.index]
          return (
            <div
              key={virtualRow.index}
              style={{
                position: 'absolute',
                top: 0,
                left: 0,
                width: '100%',
                height: `${virtualRow.size}px`,
                transform: `translateY(${virtualRow.start}px)`,
              }}
              className="flex border-b"
            >
              <div className="flex-1 p-2">{row.name}</div>
              <div className="flex-1 p-2">{row.email}</div>
              <div className="flex-1 p-2">{row.role}</div>
            </div>
          )
        })}
      </div>
    </div>
  )
}
```

### CSV Export
```typescript
export function exportToCSV<T extends Record<string, any>>(
  data: T[],
  columns: Array<{ key: keyof T; header: string }>,
  filename: string
) {
  // Generate CSV content
  const headers = columns.map((col) => col.header).join(',')
  const rows = data.map((row) =>
    columns.map((col) => {
      const value = row[col.key]
      // Escape commas and quotes
      return typeof value === 'string' && (value.includes(',') || value.includes('"'))
        ? `"${value.replace(/"/g, '""')}"`
        : value
    }).join(',')
  )
  const csv = [headers, ...rows].join('\n')

  // Trigger download
  const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' })
  const link = document.createElement('a')
  link.href = URL.createObjectURL(blob)
  link.download = `${filename}.csv`
  link.click()
  URL.revokeObjectURL(link.href)
}

// Usage in component
<button
  onClick={() =>
    exportToCSV(
      table.getRowModel().rows.map((row) => row.original),
      [
        { key: 'name', header: 'Name' },
        { key: 'email', header: 'Email' },
        { key: 'score', header: 'Score' },
      ],
      'candidates'
    )
  }
>
  Export CSV
</button>
```

### Responsive Mobile Table (Card View)
```typescript
export function ResponsiveTable({ data }: { data: Candidate[] }) {
  return (
    <>
      {/* Desktop: Table */}
      <table className="hidden md:table w-full">
        <thead>
          <tr>
            <th>Name</th>
            <th>Skill</th>
            <th>Score</th>
          </tr>
        </thead>
        <tbody>
          {data.map((row) => (
            <tr key={row.id}>
              <td>{row.name}</td>
              <td>{row.skill}</td>
              <td>{row.score}%</td>
            </tr>
          ))}
        </tbody>
      </table>

      {/* Mobile: Cards */}
      <div className="md:hidden space-y-3">
        {data.map((row) => (
          <div key={row.id} className="rounded-lg border p-4">
            <div className="font-medium">{row.name}</div>
            <div className="text-sm text-gray-600">{row.skill}</div>
            <div className="mt-2 text-lg font-bold">{row.score}%</div>
          </div>
        ))}
      </div>
    </>
  )
}
```

### Column Resize
```typescript
import { useReactTable, getCoreRowModel, type ColumnDef } from '@tanstack/react-table'

const columns: ColumnDef<Candidate>[] = [
  {
    accessorKey: 'name',
    header: 'Name',
    size: 200, // Initial width
  },
]

export function ResizableTable({ data }: { data: Candidate[] }) {
  const table = useReactTable({
    data,
    columns,
    columnResizeMode: 'onChange',
    getCoreRowModel: getCoreRowModel(),
  })

  return (
    <table style={{ width: table.getTotalSize() }}>
      <thead>
        {table.getHeaderGroups().map((headerGroup) => (
          <tr key={headerGroup.id}>
            {headerGroup.headers.map((header) => (
              <th key={header.id} style={{ width: header.getSize() }}>
                {header.column.columnDef.header}
                <div
                  onMouseDown={header.getResizeHandler()}
                  className="inline-block w-1 h-full bg-gray-300 cursor-col-resize"
                />
              </th>
            ))}
          </tr>
        ))}
      </thead>
      <tbody>{/* Rows */}</tbody>
    </table>
  )
}
```

## Anti-Patterns

**TD-50: No Pagination for Large Datasets**
- Rendering 1000+ rows at once (performance crash)
- Fix: Add pagination or virtual scrolling

**TD-60: Missing ARIA for Custom Tables**
- Using divs without role="table", role="row", etc.
- Fix: Use semantic <table> or add ARIA roles

**TD-70: Non-Keyboard Accessible Sort**
- Sort only works on mouse click
- Fix: Add keyboard handler (Enter/Space on header)

**TD-80: Fixed Column Widths on Mobile**
- Table overflows screen on small devices
- Fix: Use responsive card view or horizontal scroll with sticky first column

## Quality Rubric (Ship at 28/35)

| Dimension | 1 | 3 | 5 |
|-----------|---|---|---|
| Performance | Renders all rows | Pagination | Virtual scrolling |
| Accessibility | No ARIA | Basic table semantics | Full keyboard navigation |
| Features | Static display | Sort + filter | Sort + filter + export + resize |
| Responsiveness | Fixed width | Horizontal scroll | Card view on mobile |
| User Feedback | No loading state | Spinner | Skeleton rows |
| Code Quality | Inline logic | Custom hooks | Headless library (TanStack) |
| Error Handling | Crashes on empty | Basic check | Empty state + error boundary |

## Output Protocol

Write to `.claude/outputs/`:
- `table-components.tsx` - Reusable table with features
- `column-definitions.ts` - Type-safe column configs
- `export-utils.ts` - CSV/Excel export functions
- `table-accessibility.md` - ARIA guidelines
