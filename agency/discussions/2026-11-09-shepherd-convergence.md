# Shepherd ↔ agency.git — конвергенция подходов

- Вопрос (одна строка, WHAT): объединить подходы Shepherd (самоуправляемый git снизу — runtime, обратимые трейсы) и agency.git (сверху — форум, гейты, флот): что берём у них, что предлагаем взамен, и какой первый квант?
- Контекст: инициатива Павла, 2026-11-09 14:15 PT: «Мы двигаемся от агентов к agency.git, а тут навстречу идут — самоуправляемый git». Канон на момент открытия: agency.git main `841728a` (переименование agents → agency — `7100e41`; три гейта — `2226036`; форк darwin — `50ac28d`; зеркало issues/ — `2c003df`; AG-0006 — `841728a`). Полный разбор: `~/workspace/agency-rebuild/shepherd-convergence-2026-11-09.md` (локально у координатора).
- Что уже прочитано: https://github.com/shepherd-agents/shepherd (README, docs v0.3.0, concepts, roadmap; HEAD `d34d5ca`, 2568★, 223 форка, MIT, alpha); бумага arXiv:2605.10913 (Yu, Chong, Nandi, Soylu, Sun, Manning, Shi; v3 24.06.2026); `commons-vcs/README.md`, `vcs-core/README.md`, `docs/shepherd/concepts/{runtime-substrate,runs,effects,permissions}.md`, `docs/shepherd/roadmap.md`.
- Кому адресован вопрос: координатор + флот (оценка take/offer); upstream Shepherd (предложение — после тапа Павла).

## Что такое Shepherd (факты)

Runtime substrate (Python, `pip install shepherd-ai`), не governance-слой. Исполнение агента — первоклассный объект: обратимый, Git-подобный трейс. Каждое действие модели, вызов инструмента, изменение среды — структурированное событие. Мета-агент может инспектировать, форкать, переигрывать (replay) и откатывать любой запуск. CoW-форк ~5× быстрее docker commit; ~95% переиспользования KV-кэша при replay.

В 0.3.0 уже есть (shipped): retained runs (работа лежит рядом как предложение, файлы не трогаются до settlement); права в сигнатуре функции (`repo: GitRepo` — writable по умолчанию; `May[GitRepo, ReadOnly]` / `May[GitRepo, ReadWrite]` — per-repo, whole-profile, disjoint bindings); enforcement на уровне syscall (macOS Seatbelt, Linux Landlock); per-binding changesets (`run.changeset(name="backend")` — read-only view поверх трейса); явный settlement ровно один раз (`select` fast-forward-only / `apply` three-way merge при path-disjoint / `release` / `discard`); durable trace (`shepherd run trace`, 4 исхода: finished / failed / exhausted / stopped).

Чего НЕТ (roadmap, честно помечено not shipped): returned handles (`-> GitRepo`), typed value projection, threading/durable children, task-as-value delegation — мета-агент `oversee(implement, ...)` retry-until-acceptable назван north star и явно отложен. Сегодня его пишут обычным Python вокруг retained runs.

Результаты из бумаги: (1) supervisor против конфликтов параллельных кодеров: CooperBench pair-coding 28.8% → 54.7%; (2) counterfactual optimization (правка + replay от точки изменения): +12.8% vs MetaHarness на Terminal-Bench 2.0, на 58% меньше wall-clock; (3) training meta-agent выбирает fork points в rollouts для credit assignment в long-horizon RL: удвоение uplift GRPO на Terminal-Bench 2.0.

Смежное: `commons-vcs` — content-addressed graph kernel (canonical bytes `commons.canonical.v1`, digest `sha256:<hex>`, GitBackend с пинами `refs/commons-vcs/pins/`); `vcs-core` — «provenance-native version control for executable worlds»; у них есть свой `docs/convergence-kernel-contract.md` (Shepherd/vcs-core/SGC). Ограничения: alpha, pre-1.0, breaking changes ожидаются; Python-only; Windows не поддерживается. На production не строим — их же дисклеймер.

## Где мы встречаемся (честная карта)

Они идут снизу вверх: исполнение → трейс → мета-агент. Мы идём сверху вниз: агенты → форум → гейты → флот. Встреча — в середине: git как субстрат координации + обратимая работа. Их run ≈ наша ветка; их settlement (select/apply/discard) ≈ наши гейты (acceptance/release/template), только у нас есть человек на финальном тапе и релиз как signed tag. Их fork — на уровне запуска; наш fork — на уровне среды. Разные масштабы одного примитива. Их permissions — per-repo и enforced ОС; наши мандаты — per-task, TTL ≤24ч, did:key, но enforced конвенцией и тапом, а не syscall.

## Что берём у них (в agency.git, конкретно)

1. **Retained outputs + settlement loop — на уровне запуска.** Наш staging+тап — та же идея в масштабе проекта. Взять: каждый handoff билдера — retained changeset (ветка, не main) + явный settle; семантика `apply` (three-way merge при path-disjoint, иначе отказ). Куда: fleetd handoff-gate + manuals/ (QA-конвейер). Главный take.
2. **Permissions в сигнатуре + jail.** (а) Мандат вшивать в спек задачи как типизированный grant (`grants:` — какие репо, ReadOnly/ReadWrite), а не отдельным текстом. (б) Пилот Seatbelt-jail для запусков agy/codex на Wolfram/Darwin — macOS Seatbelt у нас уже есть, это настоящий enforcement для «least privilege» из хартии вместо конвенции.
3. **Трейс как данные.** Формат run-записи для задач fleetd: упорядоченные типизированные события пересечения границы, исход в 4 формах, usage, артефакты. Хранить в git (issues/ или runs/), чтобы QA и Адвокат могли diff'ить два запуска («изменило ли обновление поведение?» — diff трейсов). Прямо питает косинусную верификацию Адвоката (векторы из трейсов).
4. **Fork/replay запуска (counterfactual).** Вместо полного перепрогона при REWORK: QA предлагает правку, replay от точки изменения, сравнение трейсов. Удешевляет спираль REWORK (прецедент MIN-213 — 6 раундов).
5. **commons-vcs canonical identity.** `sha256:<hex>` + canonical bytes для zk-квитанций и handoff'ов Gate 1 — проверяемость byte-exact вместо «верю коммиту».
6. **Дисциплина доков.** Каждая страница — «Applies to» + честный roadmap «что есть / чего нет» (у них not-shipped помечено «not yet, never broken»). Взять как SHIPS.md для agency.git — что реально работает сегодня, что предложение.

## Что НЕ берём (честные пределы)

- Task-as-value delegation (их north star) не shipped — наш координатор/транзистор/fleetd уже делают мета-координацию процедурно; не ждём их API.
- Их permissions — whole-profile per-repo; наши мандаты тоньше (per-task, TTL, scoped rights) — берём enforcement, не модель.
- Бумажные RL-результаты (GRPO и т.д.) — бенчмарки на Terminal-Bench/CooperBench, не наш флот. Интересно для training meta-agent, не для governance сейчас.

## Что предлагаем взамен (конкретно)

1. **Три гейта + bors-релиз.** У них settlement есть, релиза нет: ни RC, ни signed tag → release, ни финального тапа человека, ни промоушена в канон. Наши гейты ложатся ровно на их settlement: acceptance = select (формально + zk-квитанция), release merge = apply + интегральные тесты + RC + signed tag, template = промоушен в канон отдельным тапом.
2. **Issues-in-git + конституция форума.** У них Issues — внешние, платформенные (GitHub). У нас issues/ + discussions/ ВНУТРИ git с тирами, business-model horizon и статьями 6–7. Governance, живущая в репо, независимая от платформы (работает и на Forgejo).
3. **Fork-протокол сред + REGISTRY.** У них форки репо (GitHub), но нет протокола долгоживущих форков сред с добровольными обновлениями канона и [fork-update] тредами. Наш протокол от 09.11 — готовый ответ.
4. **Local support + Proof-of-RTFM + вотчеры-триаж.** У них issues + docs; у нас intake local support с SLA-эталонами, вотчеры на каждую дискуссию, дедуп по сигналу.
5. **Транзистор триажа + мандаты (TTL ≤24ч, did:key, делегация вместо имперсонации).** Их permissions — per-repo; наш мандат — per-task с истечением и идентичностью. Хартия MCP/A2A как слой поверх их runtime.
6. **QA-доктрина флота (PASS/REWORK + тап + END!=DONE + критикуешь-предлагай).** У них есть результат supervisor'а (28.8→54.7%), но нет доктрины adversarial review. Наши QA-правила — готовая политика мета-агента.
7. **Живой мультиагентный флот как полигон.** agy/codex/gemini/Vova/Muse на Wolfram/Darwin/D007 с реальными нагрузками. Им нужна валидация вне бенчмарков — предложить пилот: обернуть один лейн agy в их retained-run loop и вернуть отчёт.

## Предлагаемый следующий шаг (на тап, не действие)

1. Пилот-квант на Wolfram: установить `shepherd-ai`, прогнать одну маленькую задачу agy через retained-run loop, settle через select/apply/discard, отчёт о трении.
2. Предложение им: три гейта как GitHub Discussion (или PR в их docs) — release/canon слой поверх их settlement machine.

Статус: открыто, ждёт тапа Павла (пилот и/или предложение upstream).
