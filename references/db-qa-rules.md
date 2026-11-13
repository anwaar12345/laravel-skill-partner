# Database & QA Rules

## Database (Service Layer Mein)

**Schema review (agar migration bana rahe hain):**
- Foreign keys par index? (existing migrations follow karein)
- Column types project ke convention se match? (`json` vs `jsonb` etc.)
- Nullable vs not-null — existing pattern dekhein.
- Timestamps — project `timestamps()` use karta hai ya custom?
- Migration version compatibility: `foreignId()->constrained()` Laravel 7+, `foreignUlid()` Laravel 9+.

**Query review:**
- N+1 queries? → Eager loading (`with()`) — **sirf agar project mein use hota hai**.
- Large data? → Pagination (`simplePaginate` ya `paginate` — existing style).
- Transaction needed? → `DB::transaction()` — **sirf agar project mein use hota hai**.
- Raw query? → **sirf tab jab existing project mein raw use hota ho**. Warna Eloquent.
- Laravel 10+ → `DB::transaction($callback, $attempts)` retry ke liye (agar existing project use karta hai).

---

## QA Lead Checklist (Plan Mein Include Karein + Task Complete Se Pehle Verify)

**Plan Phase (Approval Se Pehle):**
- [ ] Version detect kiya (PHP, Laravel)?
- [ ] Har point ka breakdown plan mein hai?
- [ ] Edge cases plan mein list hain?
- [ ] Assumptions share kiye?
- [ ] Job/Queue genuinely zaroori hai? (Agar haan, plan mein reason likha?)
- [ ] Kya nahi karenge (scope se bahar) mention kiya?
- [ ] Approval maanga?

**Execute Phase (Code Likhne Ke Baad):**
- [ ] Sirf approved plan ka scope touch kiya? (Adjacent code nahi)
- [ ] Existing pattern follow kiya? (Controller, service, request, blade, JS)
- [ ] Version-compatible syntax use kiya? (PHP 8.1 par `readonly` classes nahi, etc.)
- [ ] Edge cases handle kiye?
  - [ ] Null/empty input
  - [ ] Record not found