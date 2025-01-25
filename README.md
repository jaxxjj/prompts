# AI Prompt Management Guide

This repository contains a collection of structured prompts designed to enhance AI interactions. While AI is powerful, current context management has limitations and AI may lose context when the chat gets long. Having standardized prompts helps maintain consistency and provides clear guidelines, especially for newer technologies like Solidity and organization-specific patterns.

## Structure

The repository is organized by technology domains, with each domain having its own directory:

```
.
├── examples/                 # Example implementations
│   ├── go_error/            # Go error handling examples
│   │   ├── Makefile
│   │   └── .gitmodules.example
│   └── [other examples]
│
├── go/
│   ├── go_general.md        # General Go development guidelines
│   ├── go_error.md         # Error handling patterns
│   ├── go_pagination.md    # Pagination implementation
│   ├── go_unit_test.md     # Unit testing guidelines
│   ├── go_concurrency.md   # Concurrency patterns
│   └── go_log.md          # Logging best practices
│
├── solidity/
│   ├── solidity_general.md  # General Solidity guidelines
│   ├── design_patterns/     # Design pattern documentation
│   └── attack_patterns/     # Security & attack patterns
│
├── sqlc/
│   ├── sqlc_general.md     # General SQLC guidelines
│   └── sqlc_golden_test.md # Golden test patterns
│
└── thinking.md             # Core thinking protocol
```

Each technology directory contains:

- **{tech}_general.md**
  - Core principles and best practices
  - Standard requirements and guidelines
  - Recommended for inclusion in Cursor settings
  - Example: [`go/go_general.md`](go/go_general.md)

- **Specific Pattern Files**
  - Focused on particular aspects (error handling, testing, etc.)
  - Can be referenced using @ syntax
  - Example: [`go/go_pagination.md`](go/go_pagination.md)

## Usage Methods

There are three main ways to use these prompts:

1. **Cursor Settings Integration**
   - Core prompts that should be consistently available, at least includes [`thinking.md`](thinking.md)
   - General best practices and requirements
   - Fundamental development guidelines
   - Set once and always active in your environment

2. **@ Reference in Codebase**
   - Create a `/prompt` folder in the root directory
   - Reference using `@filename` syntax
   - Example: [`@solidity_behavioral_patterns.md`](solidity/design_patterns/solidity_behavioral_patterns.md), [`@go_error.md`](go/go_error.md)

3. **Direct Chat Paste**
   - Prompts that can be pasted directly into chat
   - Useful for specific scenarios & sections
   - Flexible and context-specific guidance

## Usage Examples

1. Design Pattern Query:
```
@solidity_behavioral_pattern.md Which design patterns are applicable for implementing a decentralized lottery?
```

2. Error Handling Revision:
```
Help me rewrite the error handling in galxe error pattern @go_error.md
```

3. Unit Test Generation:
```
Help me write a unit test for this function using @go_unit_test.md pattern
```

## Additional Resources

For more information on prompt engineering:
- [Prompt Engineering](https://www.bilibili.com/video/BV1u463YxE1a?spm_id_from=333.788.videopod.episodes&vd_source=86ac9233cf81dee0922f01bfebb1f5fe&p=3)

