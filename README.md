# yach-skills

A collection of specialized agent skills for the Yach ecosystem, designed to be used with the [Standard Skills Protocol](https://github.com/instruction-fine-tuning/skills).

## Available Skills

### [TypeScript + React Reviewer](./client-typescript-react-reviewer/SKILL.md)
**Name:** `typescript-react-reviewer`

Expert code reviewer for TypeScript + React 17 applications. Focuses on:
- 🚫 Identifying critical anti-patterns (e.g., side-effects in render, state mutation)
- ⚛️ React 17 & Hooks best practices (`useEffect` usage, custom hooks)
- 🔄 Redux & Redux-Saga state management patterns
- 🛡️ TypeScript type safety and maintainability

## Usage

You can use these skills with any agent that supports the Standard Skills Protocol (like Antigravity, Cline, etc.).

### Install All Skills visually
```bash
npx skills add .
```

### Install Specific Skill
```bash
npx skills add ./client-typescript-react-reviewer

npx skills add https://github.com/lqsyyy/yach-skills.git --skill client-typescript-react-reviewer
```

## Contributing

To add a new skill:
1. Create a new directory for your skill.
2. Add a `SKILL.md` file with the required YAML frontmatter:
   ```markdown
   ---
   name: my-new-skill
   description: Brief description of what this skill does
   ---
   
   # Detailed Instructions
   ...
   ```
