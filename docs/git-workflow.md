# Git Workflow, Branching, PRs & Tags

> Keep the repository clean: **never commit directly to `main`**, keep **at most 1–2 active branches** at a time, merge via Pull Request, and **delete merged branches** immediately (locally and on GitHub).

---

## 1. Branching Strategy (Lean & Low-Clutter)

| Branch | Purpose | Lifetime |
| :--- | :--- | :--- |
| `main` | Protected source of truth (approved docs, approved designs, production code) | Permanent |
| `design/figma-ui` | **Single branch for all Phase 0 Figma designs** (Design System + all pages, Desktop & Mobile SVGs) | Merged to `main` once all Figma designs are approved, then deleted |
| `feat/<milestone>` | Phase 1+ feature/milestone branches (e.g., `feat/monorepo-setup`, `feat/api-core`, `feat/storefront`, `feat/admin-panel`) | Merged to `main` via PR, then deleted |
| `fix/<issue>` | Targeted bug fixes | Merged to `main` via PR, then deleted |

### Rules
1. **No branch-per-page for Figma:** All Figma SVG designs and `docs/figma-workflow.md` updates live on `design/figma-ui` until Phase 0 is complete.
2. **One active branch per milestone:** Finish, PR, merge, and delete the current branch before starting the next milestone branch.
3. **Never commit directly to `main`.**

---

## 2. Conventional Commits

Format: `<type>: <concise summary>`

| Type | When to Use |
| :--- | :--- |
| `feat:` | New application feature, endpoint, or page |
| `design:` | Figma SVGs, `design/tokens.json`, `docs/design-system.md` |
| `docs:` | `AGENTS.md`, `README.md`, `docs/*.md` |
| `fix:` | Bug fix |
| `refactor:` | Code restructuring without behavior change |
| `test:` | Unit, integration, or E2E tests |
| `chore:` | Tooling, dependencies, configs, repo maintenance |

---

## 3. Pull Request & Branch Cleanup Loop

1. Push active branch: `git push -u origin <branch>`
2. Open Pull Request targeting `main` on GitHub (`SHOKH-BD`).
3. Review & Merge PR into `main`.
4. **Delete old branch** on GitHub and locally:
   ```bash
   git checkout main
   git pull origin main
   git branch -d <branch>
   git push origin --delete <branch>
   git fetch --prune
   ```

---

## 4. Release & Milestone Tags

Use annotated Git tags on `main` after major milestones are merged:

| Tag | Milestone |
| :--- | :--- |
| `v0.1.0-docs` | Project plan, architecture, DB schema, and synced Design System docs |
| `v0.2.0-figma` | All MVP Figma designs (Desktop `1440px` & Mobile `390px`) approved |
| `v0.3.0-api` | Monorepo + Express API + MongoDB Repository Layer ready |
| `v1.0.0-mvp` | Full SHOKH MVP (Storefront + COD Checkout + Admin Panel) live on Vercel |

Tagging command (run on `main` after PR merge):
```bash
git tag -a v0.2.0-figma -m "Milestone: All MVP Figma designs approved"
git push origin v0.2.0-figma
```
