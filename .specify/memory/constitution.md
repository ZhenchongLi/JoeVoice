<!--
Sync Impact Report:
- Version change: Template → 1.0.0 (Initial constitution creation)
- Added principles: All 5 core principles (new)
  - I. Privacy First
  - II. Issue-Driven Development
  - III. Branch-Based Development
  - IV. Code Review Mandatory
  - V. Release Discipline
- Added sections: Development Workflow and Privacy & Security (new)
- Templates requiring updates:
  ✅ Updated .specify/templates/plan-template.md (Constitution Check section)
  ✅ Updated .specify/templates/tasks-template.md (Added constitutional compliance tasks)
  ✅ Updated constitution.md
  ✅ No changes needed for spec-template.md (no constitutional references)
  ✅ No changes needed for agent-file-template.md (generic template)
- Follow-up TODOs: None
-->

# JoeVoice Constitution

## Core Principles

### I. Privacy First
100% offline processing is non-negotiable. All voice transcription must occur locally using whisper.cpp or equivalent on-device models. No audio data, transcripts, or user content may be transmitted to external servers without explicit user consent for optional features (AI enhancement only). User data sovereignty and local processing capability are fundamental requirements that cannot be compromised.

### II. Issue-Driven Development
Every feature, bug fix, and enhancement MUST begin with a GitHub Issue. Issues provide documentation, requirements clarity, and progress tracking. No code changes may proceed without first documenting the problem or requirement in an issue. This ensures all development work is purposeful, traceable, and aligned with project goals.

### III. Branch-Based Development
All development MUST occur on dedicated feature or fix branches created from main. Branch naming follows the pattern `feat/feature-name` or `fix/issue-description`. Direct commits to main branch are prohibited. This maintains code stability and enables proper review processes.

### IV. Code Review Mandatory
All code changes MUST be submitted via Pull Request and reviewed before merging to main. PRs must reference the originating GitHub Issue and include comprehensive descriptions of changes. No self-merging allowed. This ensures code quality, knowledge sharing, and collective ownership.

### V. Release Discipline
Production releases MUST follow semantic versioning (MAJOR.MINOR.PATCH) with proper release notes, DMG packaging, and Sparkle framework integration for auto-updates. Version numbers must be updated in both Xcode project settings and appcast.xml. Each release must close all associated GitHub Issues.

## Development Workflow

### Mandatory Process Flow
1. **Issue Creation**: Document problem/feature in GitHub Issues with clear acceptance criteria
2. **Branch Creation**: Create `feat/` or `fix/` branch from latest main
3. **Development**: Implement changes following Swift/SwiftUI best practices
4. **Testing**: Verify functionality using Xcode testing framework (Cmd+U)
5. **Pull Request**: Submit PR with issue reference and detailed description
6. **Code Review**: Mandatory review by project maintainers
7. **Merge**: Approved PRs merge to main branch only
8. **Release**: Version bump, DMG creation, appcast update, GitHub release
9. **Issue Closure**: Close originating GitHub Issue upon release

### Quality Gates
- All code must compile without warnings in Xcode
- Required app permissions (microphone, accessibility) must be properly declared
- whisper.xcframework dependency must be correctly linked
- macOS 14.0+ compatibility must be maintained
- Auto-update mechanism must function correctly

## Privacy & Security

### Data Handling Requirements
- Audio files must be processed locally and cleaned up automatically
- Temporary files must be securely deleted after processing
- Optional AI enhancement features may use external APIs only with explicit user consent
- No telemetry or usage data collection without clear opt-in consent
- All entitlements in JoeVoice.entitlements must be justified and minimal

### Security Standards
- App signing and notarization required for all releases
- Secure storage of user preferences using macOS keychain when appropriate
- Input validation for all user-provided data (custom vocabulary, AI API keys)
- Regular dependency updates for security patches

## Governance

### Amendment Process
Constitution changes require documentation in GitHub Issues, implementation via Pull Request, and approval by project maintainers. All amendments must include rationale and impact analysis on existing development processes.

### Version Management
Constitution versions follow semantic versioning. MAJOR versions indicate process changes requiring developer workflow updates. MINOR versions add new requirements or sections. PATCH versions clarify existing guidance.

### Compliance Review
All Pull Requests must verify constitutional compliance, particularly Privacy First and Issue-Driven Development principles. Code reviews must validate that changes align with project principles and maintain the offline-first architecture.

### Development Guidance
Runtime development guidance is maintained in CLAUDE.md at repository root for AI-assisted development contexts.

**Version**: 1.0.0 | **Ratified**: 2025-09-29 | **Last Amended**: 2025-09-29