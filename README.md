# UX/UI QA Auditor Agent for Claude Code

Stop your AI from breaking your design system. This subagent automatically audits UI changes and reports violations back to your main LLM without polluting its context.

## What It Does

- **Audits** every UI change against your design system
- **Reports** exact violations with file locations and fixes
- **Preserves** your main LLM's context by running separately
- **Enforces** consistent components, colors, typography, and spacing

## Which File to Use?

### 📝 `ux-ui-qa-auditor-template.md` (Recommended Start)
The **template version** with placeholders for you to customize:
- All opinionated defaults removed
- Placeholders ready to replace with your values
- Minimal tool requirements (Glob, Grep, LS, Read, WebFetch, Bash)
- Framework-agnostic

### 🔍 `ux-ui-qa-auditor-example.md` (Reference)
The **example version** showing a complete implementation:
- Fully configured for Shadcn/ui + Tailwind
- Includes advanced components and animations
- Requires MCPs (Supabase, Context7, Serena, Shadcn-ui)
- Shows best practices and possibilities

## Quick Start

### 1. Choose Your File
- **Most users**: Copy `ux-ui-qa-auditor-template.md` to `.clde/agents/`
- **Advanced/Similar stack**: Copy `ux-ui-qa-auditor-example.md` to `.clde/agents/`

### 2. Configure
If using the template, replace ALL placeholders:
(you can ask claude code to do this or do it manually)
placeholder  → example

[YOUR_DESIGN_SYSTEM_ROUTE] → /design-system
[YOUR_COMPONENT_LIBRARY_PATH] → src/components/ui/
[YOUR_STYLE_GUIDE_PATH] → src/design-system/StyleGuide.tsx
[LIST YOUR COMPONENT VARIANTS HERE] → Button: primary, secondary, ghost
[YOUR_MIN_SIZE] → 44x44px
[YOUR_DEV_COMMAND] → npm run dev
[PORT] → 5173

### 3. Enable in claude.md
Add to `.clde/claude.md`:

**Claude Sub-Agents**
- **MANDATORY**: Use Claude sub agent ux-ui-qa-auditor after every change to the UI
- **ux-ui-qa-auditor**: Checks UX and UI design requirements have been followed

### 4. Test
Make any UI change. The agent will auto-trigger and provide an audit report.

## Customization Guide

### Design System Setup
Your project needs a live design system that the agent can reference. This can be:
- A dedicated route (e.g., `/design-system`) 
- Storybook instance
- Component documentation site

### Quality Gates
The agent includes default quality gates. Customize these in the "Quality Gates" section:

1. Component not in design system - FAIL
2. Color not in palette - FAIL
3. External UI library used - FAIL
...

Add your own rules or remove ones that don't apply.

### Component Library
Update the component paths to match your structure:
- Shadcn/ui: `src/components/ui/`
- Custom library: `src/lib/components/`
- Monorepo: `packages/ui/src/`

### Reporting Format
The agent outputs structured reports. Customize the format in the "Reporting Format" section to match your team's preferences.

## Example Output

## UX/UI Design System Audit Report

### ❌ Critical Issues
**Location**: Dashboard.tsx:45
**Issue**: Using hardcoded button with inline styles
**Expected**: Button component with variant="primary"
**Fix**: Import from '@/components/ui/button'

### ✅ Compliant Elements
- Card components correctly use glass variant
- Colors match palette from design system
- Spacing follows 4px grid system

## How It Works

1. **Trigger**: Activates automatically after UI changes
2. **Scan**: Reviews changed files in its own context window
3. **Compare**: Checks against your design system
4. **Report**: Sends specific fixes back to main LLM
5. **Fix**: Main LLM implements corrections

The key innovation: By running as a subagent, it can audit massive codebases without overwhelming your main LLM's memory.

## Requirements

- **Claude Code**: With subagent support
- **Design System**: Live reference (route, Storybook, or docs)
- **Component Library**: Organized UI components
- **Tools**: File system access (Glob, Grep, LS, Read, WebFetch, Bash)
- **MCPs** (only for example file): Supabase, Context7, Serena, Shadcn-ui

## Common Issues

### Agent Not Triggering
- Verify the claude.md configuration includes the MANDATORY directive
- Check agent file is in `.clde/agents/` directory
- Ensure agent name matches exactly: `ux-ui-qa-auditor`

### False Positives
- Update placeholder values to match your exact paths
- Adjust quality gates to your standards
- Verify design system route is accessible

### Missing Components
- Add all your components to the audit list
- Include custom variants and states
- Document any exceptions in the agent prompt

## Advanced Configuration

### Multiple Design Systems
If you have different design systems per project area:
- Marketing pages: `/design-system/marketing`
- App pages: `/design-system/app`
- Admin pages: `/design-system/admin`

### Framework-Specific Setup
- **Next.js**: Update paths to use `@/` imports
- **Vue**: Adjust component syntax checking
- **Angular**: Modify directive validation

### CI/CD Integration
While primarily for development, you can adapt the audit logic for CI pipelines by extracting the validation rules.

## License

MIT with Commons Clause - Free for non-commercial use. Commercial use requires a license.

## Support

Open an issue for bugs, questions, or feature requests.

## Contributing

PRs welcome! Please:
1. Keep the agent framework-agnostic
2. Document any opinionated defaults
3. Include placeholder examples

---

Built to stop AI from breaking your beautiful designs. 🎨
