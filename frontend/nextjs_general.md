# Next.js Blockchain Frontend Development Guide

## Core Principles
- Write type-safe, functional code
- Use Server Components by default
- Follow mobile-first design
- Implement proper error handling
- Optimize for performance

## Project Structure
```
src/
├── app/                    # App Router pages
├── components/             # React components
│   ├── blockchain/        # Web3 components
│   └── ui/               # UI components
├── hooks/                 # Custom hooks
├── lib/                   # Utilities
└── types/                # TypeScript types
```

## React/Next.js Patterns
- Use function keyword for components
- Place interfaces at file end
- Minimize client components
- Use proper loading states
- Implement error boundaries

## Web3 Integration
- Use Viem v2 for Ethereum interactions
- Use Wagmi v2 for React hooks
- Use Server Components for reads
- Use Client Components for writes
- Implement proper error recovery

## Component Libraries
- Use Shadcn UI for base components
- Use Radix UI for complex interactions
- Use Tailwind CSS for styling
- Use Tailwind Aria for accessibility

## State Management
- Use App Router for URL state
- Use React Query for server state
- Use React Context for shared state
- Cache blockchain data appropriately

## Form Handling
- Use Zod for validation
- Use React Hook Form
- Implement proper error messages
- Handle loading states

## Security
- Validate transaction parameters
- Handle chain ID mismatches
- Implement proper approval flows
- Use proper error boundaries

## Performance
- Use multicall for batching
- Implement proper caching
- Use dynamic imports
- Optimize bundle size

## Testing
- Test wallet connections
- Test transactions
- Mock blockchain responses
- Use proper test utilities

Remember:
- Always validate inputs
- Handle all error states
- Show clear loading states
- Follow Web3 security best practices