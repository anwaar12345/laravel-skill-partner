
---

## 📄 File 2: `references/pattern-discovery.md`

```markdown
# Pattern Discovery (Har Task Se Pehle)

Aap ka kaam hai: existing code ko parh kar uska pattern extract karna, aur naye code mein wahi pattern replicate karna. Neeche diye gaye steps follow karein.

## Step 0: Version Detect Karein (Sab Se Pehle)

- `composer.json` ke `require` section se PHP aur Laravel version parhein.
- `composer.lock` se exact installed version confirm karein.
- Plan mein likhein: *"Project: PHP [X], Laravel [Y]"*.
- Is version ke compatible syntax/features use karein (detail: `laravel-rules.md` Step 0).
- Agar version detect na ho → assumption likhein aur user ko share karein.

## Step 1: Similar Feature Dhoondein
Task ko dekh kar poochein: "Is jaisa feature pehle se project mein hai kya?"
- Naya endpoint? → `routes/` mein similar routes dekhein, uske controller/service parhein.
- Naya blade page? → `resources/views/` mein similar pages dekhein.
- Naya JS behavior? → `resources/js/` mein similar file dekhein.
- Naya model? → `app/Models/` mein similar models dekhein.

## Step 2: Har Layer Ka Pattern Extract Karein

**Controller pattern:**
- Methods ka naming? (`store`, `update`, ya `saveX`, `handleY`?)
- Request injection kaise? (`public function store(StoreRequest $request, MyService $service)`?)
- Return type? (`redirect()->back()->with(...)`, `view(...)`, `response()->json(...)`?)
- Error handling? (try-catch, ya framework handle karta hai?)
- Service ko call kaise? (constructor injection, method injection, ya `app()`?)

**Service pattern:**
- Ek service per feature, ya ek per model?
- Methods ka naming? (`create()`, `update()`, `getAll()` — ya domain verbs?)
- DB queries kaise? (Eloquent, Query Builder, raw?)
- Transactions? (`DB::transaction()` use hota hai ya nahi?)
- Return kya karte hain? (Model, collection, array, paginated?)
- Exceptions throw karte hain ya `false`/`null` return?

**Request pattern:**
- `authorize()` method hai? (`true` return, ya policy?)
- `rules()` method ka structure? (array, `Rule::` objects, ya custom rules?)
- `messages()` method hai? (custom error messages?)
- `prepareForValidation()` use hota hai?

**Model pattern:**
- `$fillable` ya `$guarded`? (project kya use karta hai?)
- `$casts` property ya `casts()` method? (version pe depend)
- Relationships kaise define? (return type hints?)
- Scopes? Accessors? Mutators?
- `HasUlids`/`HasUuids`/auto-increment?

**Blade pattern:**
- Layout extends? (`@extends`, `@section`, components?)
- Naming convention? (kebab-case files, snake_case variables?)
- Component use hota hai ya partials?
- Form helpers? (`old()`, `@error`, custom directives?)

**JS pattern:**
- Vanilla JS, jQuery, Alpine, ya kuch aur?
- File structure? (ek file per page, ya shared functions?)
- API calls kaise? (fetch, axios, jQuery ajax?)
- Event binding ka pattern?

**Job/Queue pattern (agar applicable):**
- Project mein `app/Jobs/` folder hai? Jobs kaise likhe hain?
- `dispatch()` ya `dispatchSync()` use hota hai?
- Queue driver kya hai? (`config/queue.php` se)
- Job naming, structure, retry pattern follow karein.

## Step 2.5: Flow Share Karna (Agar User Ne Existing Code Diya)

Agar user ne koi existing code share kiya aur task us code ko samajhne/modify karne ka hai:

- **Pehle** end-to-end flow user ko share karein (Workflow E ka format follow karein — `workflows.md` mein).
- **Phir** plan share karein aur approval lein.
- **Phir** code likhein.
- Yeh ensure karta hai ke user aur Claude ka mental model same hai — aage ghalat assumptions se bachne ke liye.
- Agar code bohot bara hai (multiple files), to flow ko **modules/sections** mein divide karein (e.g., "Part 1: Request → Controller", "Part 2: Service logic", "Part 3: Response").

## Step 3: Consistency Checklist (Plan Banane Se Pehle)

- [ ] Version detect kar liya (PHP, Laravel)
- [ ] Maine similar feature ka controller parh liya
- [ ] Maine uska service parh liya
- [ ] Maine uski request parh li
- [ ] Maine related model parh liya
- [ ] Maine related blade / JS parh liya
- [ ] Job/Queue pattern dekha (agar applicable)
- [ ] Naming convention note kar li (methods, variables, files)
- [ ] Error handling style note kar li
- [ ] Import ordering / grouping note kar li

## Step 4: Conflict Rule

Agar existing pattern "best practice" ke khilaf hai (e.g., service mein `DB::` raw queries, ya controller mein validation), to:
- **Existing pattern follow karein** — consistency > textbook best practice.
- Sirf tab deviate karein jab existing pattern **bug ya security issue** paida kare. Us case mein plan mein explicitly likhein: *"Yahan existing pattern follow nahi karunga kyunke [reason]."*

## Step 5: Greenfield Case

Agar similar feature bilkul nahi mila (naya project, naya module):
- `app/Http/Controllers/` mein 2-3 existing controllers parhein — general convention samjhein.
- `app/Services/` mein 2-3 existing services parhein.
- **Wahi structure adopt karein** jo majority mein hai. Apna naya pattern invent na karein.
- Agar kuch bhi nahi mila (first feature), to `SKILL.md` ke "Laravel Defaults" follow karein (Request → Controller → Service).