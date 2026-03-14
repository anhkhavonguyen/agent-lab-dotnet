# Copilot Workspace Instructions

## Development Checklist

**Before committing any changes, verify:**

- [ ] `dotnet build SocOps/SocOps.csproj` passes with no errors
- [ ] Code follows C# naming conventions (PascalCase for public members, camelCase for private)
- [ ] No unused variables, imports, or async/await issues
- [ ] CSS utilities are used (avoid hard-coded styles)
- [ ] Components follow single responsibility pattern
- [ ] Services handle state management cleanly

---

## Project Overview

**Soc Ops** — A Social Bingo game built with **Blazor WebAssembly** (.NET 10) for in-person mixers and networking events. Players find people matching curated questions, mark squares, and get 5 in a row to win.

**Tech Stack:**
- Blazor WebAssembly (.NET 10)
- Custom CSS utilities (Tailwind-like)
- Event-driven state management
- Browser localStorage via JSInterop

---

## Architecture

```
SocOps/
├── Components/              # Reusable Blazor components (*.razor)
│   ├── StartScreen.razor    # Welcome & instructions
│   ├── GameScreen.razor     # Active game board display
│   ├── BingoBoard.razor     # 5×5 grid container
│   ├── BingoSquare.razor    # Individual clickable square
│   └── BingoModal.razor     # Victory notification modal
├── Models/                  # Data structures
│   ├── BingoSquareData.cs   # Square state (id, question, marked)
│   ├── BingoLine.cs         # Winning line info
│   └── GameState.cs         # Enum: Start, Playing, Bingo
├── Services/                # Business logic
│   ├── BingoGameService.cs  # Orchestrates game lifecycle, state persistence
│   └── BingoLogicService.cs # Pure logic: board generation, win detection
├── Data/                    # Static content
│   └── Questions.cs         # 24 contextual networking questions + FREE SPACE
├── Pages/                   # Routable pages
│   └── Home.razor           # Main page (renders appropriate component)
├── Layout/                  # App shell
│   ├── MainLayout.razor     # Container layout
│   └── NavMenu.razor        # Header/navigation
└── wwwroot/                 # Static assets
    ├── css/app.css          # Custom utility classes
    └── index.html           # Entry point
```

---

## Key Commands

```bash
# Build the project
dotnet build SocOps/SocOps.csproj

# Start development server (port 5166, http://localhost:5166)
dotnet run --project SocOps
```

---

## Styling Guidelines

Use custom CSS utility classes from `wwwroot/css/app.css` exclusively.

### Common Utilities

**Layout:** `.flex`, `.flex-col`, `.grid`, `.grid-cols-5`, `.items-center`, `.justify-center`
**Spacing:** `.p-1` to `.p-6`, `.mb-2` to `.mb-8`, `.mx-auto`
**Colors:** `.bg-accent` (blue), `.bg-marked` (green), `.text-gray-*`, `.text-green-*`
**Typography:** `.text-xs` to `.text-5xl`, `.font-semibold`, `.font-bold`

Reference: `.github/instructions/css-utilities.instructions.md`

---

## State Management Pattern

**BingoGameService** manages centralized state:
- Properties are public-read, private-write
- Components subscribe to `OnStateChanged` event
- State auto-persists to localStorage

---

## Component Patterns

**Event Callback:** Parent passes handler, child invokes with `EventCallback<T>`
**Disposal:** Components subscribe in `OnInitializedAsync()`, unsubscribe in `Dispose()` with `@implements IDisposable`

---

## Questions Data

24 contextual networking questions in `Data/Questions.cs` plus one FREE SPACE. To add questions, extend `QuestionsList`.

---

## Frontend Design

When designing UI, follow `.github/instructions/frontend-design.instructions.md`:
- Use distinctive typography
- Commit to cohesive color scheme
- Add subtle animations for key moments
- Layer gradients for depth
- Avoid generic AI aesthetics

---

## Common Development Tasks

**Adding a Question:** Edit `Data/Questions.cs`, add to `QuestionsList`, run `dotnet build`

**Styling:** Apply classes from `wwwroot/css/app.css`. Add missing utilities following Tailwind-like syntax.

**Debugging:** Use browser DevTools, check localStorage key `bingo-game-state`, verify component subscriptions.

---

## Conventions

- C# naming: `PascalCase` (public), `_camelCase` (private)
- CSS classes: `.kebab-case`
- Razor components: `PascalCase.razor`
- Keep business logic in Services, UI in Components, data in Models, styling in `wwwroot/css/app.css`

---

## Troubleshooting

**Build fails:** Run `dotnet build SocOps/SocOps.csproj` manually, check for unused variables
**Browser doesn't load:** Verify `dotnet run` still running, clear cache, try http://localhost:5166 directly
**State not persisting:** Check browser DevTools LocalStorage for `bingo-game-state`, ensure SaveGameStateAsync() is called
**CSS not applying:** Verify class exists in `app.css`, add missing. Example: `.bg-custom-color` doesn't exist, use existing classes
