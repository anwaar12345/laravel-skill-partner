# Laravel Rules (Aap ke Style ke Mutabiq)

> ⚠️ Yeh rules **defaults** hain. Agar existing project in se different hai, to **existing project jeetega**.

## 🔍 Step 0: Version Detection (Sab Se Pehle)

Koi bhi code likhne se pehle **project ka PHP aur Laravel version detect karein**. Yeh mandatory hai — kyunke syntax, features, aur APIs version ke saath badalte hain.

### Kahan Se Parhein
- `composer.json` → `require` section:
  - `"php": "^8.1"` → PHP 8.1
  - `"laravel/framework": "^10.0"` → Laravel 10
- `composer.lock` → exact installed version (agar `composer.json` se clear na ho).
- Agar `composer.json` na mile → `php -v` ya project README se guess karein, lekin plan mein likhein: *"Version composer.json se confirm nahi hua, [X] assume kar raha hoon."*

### Version Compatibility Matrix (Yaad Rakhein)

| Feature | Minimum Version |
|---|---|
| `match` expression | PHP 8.0 |
| Named arguments | PHP 8.0 |
| Nullsafe operator `?->` | PHP 8.0 |
| Enums | PHP 8.1 |
| Readonly properties | PHP 8.1 |
| `never` return type | PHP 8.1 |
| First-class callable syntax `strlen(...)` | PHP 8.1 |
| Pure intersection types | PHP 8.1 |
| `readonly` classes | PHP 8.2 |
| `true`/`false`/`null` standalone types | PHP 8.2 |
| DNF types | PHP 8.2 |
| Typed class constants | PHP 8.3 |
| `json_validate()` | PHP 8.3 |
| Dynamic class constant fetch | PHP 8.3 |
| Property hooks | PHP 8.4 |
| Asymmetric visibility | PHP 8.4 |
| `new` without parentheses | PHP 8.4 |

| Feature | Minimum Laravel Version |
|---|---|
| `Model::shouldBeStrict()` | 8.x |
| `FormRequest` typed `validated()` | 9.x |
| `Route::controller()` grouping | 9.x |
| `casts()` method on Model (vs `$casts` property) | 10.x |
| `Model::preventLazyLoading()` | 9.x |
| `once()` helper | 11.x |
| `Context` facade | 11.x |
| Health route (`/up`) | 11.x |
| `Number::()` helpers | 10.x |
| `Sleep::` facade | 10.x |
| `Concurrency` facade | 10.x |
| `Process` facade (new API) | 10.x |
| `DB::transaction()` with `attempts` | 8.x |
| `dispatchAfterResponse()` | 8.x |
| `LazyCollection` | 6.x |
| Job batching (`Bus::batch()`) | 8.x |
| Job chains (`Bus::chain()`) | 8.x |
| Job middleware | 6.x |

### Rule
- **Existing project ke version se upar ka feature use na karein.** Agar project Laravel 9 par hai, to `casts()` method (10.x) nahi, `$casts` property use karein.
- **Existing project ke version se neeche ka feature bhi blindly use na karein** — agar project ne already naya version use kiya hai (e.g., enums PHP 8.1+), to purana syntax (arrays/constants) thopna nahi.
- Version detect hone ke baad **plan mein likhein**: *"Project: PHP [X], Laravel [Y]. Code is version ke compatible hoga."*
- Agar user ne version explicitly bataya ho → wahi follow karein, detect ki zaroorat nahi.

---

## Structure: Request → Controller → Service

### Request (Validation Layer)
- Har mutating endpoint ke liye Form Request.
- `authorize()` — agar policy system nahi hai to `true`.
- `rules()` — saare validation rules yahan.
- `messages()` — agar project mein custom messages use hote hain to add karein.
- `prepareForValidation()` — sirf agar existing code mein use hota hai.
- Laravel 9+ → `validated()` method use karein (agar existing project karta hai).
- Laravel 10+ → typed DTO return karna `validated()` se — **sirf agar existing project mein DTO pattern hai**.

### Controller (Thin Layer)
- **Zero queries**. `Model::find()`, `DB::`, `->where()` — kuch nahi.
- Sirf: Request receive → Service call → Response return.
- Service injection: agar existing project constructor injection use karta hai to wahi; agar method injection, to wahi.
- Return type: existing pattern follow (redirect, view, json).
- Naming: existing controllers ke method names se match karein.
- **New syntax use na karein** agar project purane version par hai. Laravel 9+ `Route::controller()` grouping — sirf agar existing code mein hai.

### Service (Business + DB Layer)
- **Sari queries yahan**: Eloquent, Query Builder, `DB::` — sab service mein.
- Ek service per feature (existing naming follow karein — `XxxService` ya domain name).
- Methods chote aur single-purpose.
- Transactions: agar existing project `DB::transaction()` use karta hai to use karein; warna skip.
- Edge cases yahan handle: null checks, not-found handling, duplicate prevention.
- Return type: existing pattern (Model, collection, bool, array) — match karein.
- **PHP version ke hisaab se syntax**:
  - PHP 8.0+ → `match`, nullsafe, named args use kar sakte hain (agar existing code use karta hai).
  - PHP 8.1+ → enums, readonly properties, `never`, first-class callables.
  - PHP 8.2+ → readonly classes, DNF types.
  - PHP 8.3+ → typed class constants.
  - PHP 8.4+ → property hooks, asymmetric visibility.
  - **Purane version par naya syntax = fatal error**. Version confirm karein.

---

## Edge Cases (Tests Nahi, Code Mein Handle)

Har service method mein sochein:
- **Input null/empty?** → early return ya exception.
- **Record not found?** → `firstOrFail()` ya explicit check (existing style).
- **Unauthorized?** → request `authorize()` ya service check.
- **Duplicate?** → unique validation ya service check.
- **Concurrency?** → `lockForUpdate()` sirf agar critical ho (over-engineer na karein).
- **Large data?** → pagination, chunking, ya limit (existing pattern follow).

---

## What NOT To Do (Over-Engineering Ban)

❌ Repository pattern add karna jab project mein nahi hai
❌ Interface/Contract banana jab ek hi implementation hai
❌ **Event/Listener/Job jab synchronous call kaafi hai — lekin agar task genuinely async/slow/retry-able hai to use karein (neeche dekhein)**
❌ Pipeline / Action classes jab service method kaafi hai
❌ DTO banana jab array/model kaam kar raha hai
❌ Traits banana jab 2 jagah use ho raha hai
❌ Tests likhna jab user ne na kahe
❌ "Best practice" ke naam par existing pattern todna
❌ Adjacent code "improve" karna jab task mein na ho
❌ Bina approval ke code likhna (plan-first rule)
❌ **Version-incompatible syntax use karna (PHP/Laravel version detect kiye bina naya feature use karna)**

---

## ✅ Jab Job/Queue/Event GENUINELY Zaroori Ho (Use Karein)

Yeh over-engineering **nahi** — yeh sahi engineering hai. Neeche diye gaye cases mein Job/Queue use karein:

### Job/Queue Use Karein Jab:
1. **Task slow hai** (3+ seconds) aur user ka request block nahi hona chahiye — e.g., PDF generation, image resize, video processing, large CSV import/export.
2. **External API call hai** jo fail ho sakti hai aur retry chahiye — e.g., payment gateway, SMS/email provider, third-party sync.
3. **Task scheduled hai** — e.g., daily report, cleanup, subscription renewal (Laravel Scheduler ke saath).
4. **Task fire-and-forget hai** — e.g., user signup ke baad welcome email, activity log, notification.
5. **Task ko retry logic chahiye** — network fail, rate limit, transient errors. Job ka `$tries`, `$backoff` built-in hai.
6. **Bulk operation hai** — e.g., 10,000 users ko email bhejna (chunk + job).
7. **Task user ke request lifecycle se independent hai** — e.g., webhook processing, webhook response ke baad kaam.
8. **Existing project already Queue use karta hai** aur yeh kaam usi pattern mein fit hota hai.

### Job/Queue Na Use Karein Jab:
- Task fast hai (< 1-2 second) aur synchronous theek hai.
- User ko **immediate result** chahiye (e.g., login validation, search query).
- Project mein queue infrastructure **nahi hai** (koi queue driver configure nahi, koi job pehle se nahi) — pehle user se poochein/plan mein likhein, phir decide karein.
- Simple DB write hai jo already transaction mein hai.

### Decision Rule (Plan Mein Likhein)
Agar task upar diye gaye "use karein" cases mein aata hai:
- Plan mein explicitly likhein: *"Yeh task [reason] ki wajah se Job/Queue mein jayega. Existing project mein queue setup [hai/nahi]."*
- Agar existing project mein queue setup nahi → user ko plan mein batayein ke queue setup bhi chahiye hoga, phir approval lein.

### Existing Pattern Check
- Project mein pehle se jobs hain? → `app/Jobs/` dekhein, unka naming/structure follow karein.
- Project `dispatch()` use karta hai ya `dispatchSync()`?
- Project `ShouldQueue` interface use karta hai?
- **Wahi pattern follow karein**, apna naya invent na karein.

### Version Note
- Job batching (`Bus::batch()`) → Laravel 8+.
- Job chains (`Bus::chain()`) → Laravel 8+.
- Job middleware → Laravel 6+.
- `dispatchAfterResponse()` → Laravel 8+.
- `ShouldBeUnique` interface → Laravel 8+.
- `ShouldBeUniqueUntilProcessing` → Laravel 9+.

---

## Events/Listeners — Kab Use Karein

### Use Karein Jab:
1. **Multiple side effects** ek action ke baad — e.g., order placed → email, SMS, inventory update, loyalty points. Sab alag listeners.
2. **Decoupling chahiye** — e.g., core business logic ko notification/logging se alag rakhna.
3. **Existing project already events use karta hai** aur yeh event usi pattern mein fit hota hai.

### Na Use Karein Jab:
- Sirf **ek** side effect hai → service mein direct call kaafi hai.
- Synchronous flow clear hai aur events se complicate hoga.
- Project mein pehle se events nahi hain aur user ne na kahe.

### Events Version Note
- Model events (`created`, `updated`, etc.) → sab versions.
- Custom events + listeners → sab versions.
- Event discovery (auto-register listeners) → Laravel 11 default, 10 mein `EventServiceProvider` manual.

---

## Code Quality (Minimum Bar — Bina Over-Engineer)

- `declare(strict_types=1);` — **sirf agar existing files mein hai**.
- Type hints — params aur returns par (agar existing code karta hai).
- Meaningful variable names — `$data` se behtar `$validatedData`.
- Early returns — deep nesting se bachne ke liye.
- Comments — sirf jahan logic non-obvious ho. Har line comment na karein.
- **PHP version ke compatible syntax**:
  - Purane project mein arrow functions, enums, `match`, `readonly` use na karein agar version support nahi karta.
  - Naye project mein purane verbose syntax thopna nahi agar naya version already use ho raha hai.