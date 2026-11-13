# Принципы Кота Маркиза Карабаса — расширение Agency.git по рамке Grok

**Статус: DRAFT, черновик для обсуждения. Не канон, не коммит в Agency.git.**
Дата: 2026-11-13. Источники: рамка Grok (Lean ↔ Agency.Gov, 2026-11-13),
`pashamango/math` (Emergent Attractor v0.1.0, коммит `70cf5ce8`, 53 теоремы,
ноль sorry), снимок Agency.git 2026-11-12 (статья 8), спека x404 v0.1 (P-MIN-9).

## 0. Принцип — как будто у нас так всегда и было 🐈‍⬛

Кот объявляет хозяина маркизом Карабасом — и все действуют так, будто
маркизат всегда существовал: крестьяне говорят «поля маркиза», король
верит, замок занимается. Вымысел становится правдой не потому, что
его выдали за готовое, а потому, что под объявление подтягивается
реальность.

Перенос на нас:

1. **Квант сложного анализа — самая сложная сущность целиком.**
   Рамка Grok берётся как один квант, а не дробится до Phase 0.
   Дробить — только ресурсно-ограниченные / застрявшие задачи.
2. **Затычка — это x404, а не 200.** Недостающее место не выдаётся
   за готовое. Оно отвечает: `404 + x404-challenge` — что отсутствует,
   какой генератор/вход нужен, какая provenance/evidence требуется.
   Провал генерации остаётся 404 с диагностикой, никогда — stub-200.
3. **Расширение — автоматически, но черновиком.** Распознал затычку
   в ответственном месте — разверни её до объявленной сущности
   здесь же, в `discussions/`. Тап Павла — только на финальную
   приёмку в main. Решение, принятое только в чате и не landed
   коммитом, решением не является (Статья 6).

## 1. Инвентарь затычек — снимок Agency.git 2026-11-12

README-конституция — всего 82 строки, Статьи 1–8. Это и есть
главная затычка: конституция живёт как README, а не как
отдельные проверяемые документы.

| # | Затычка | Где | Статус x404 |
|---|---------|-----|-------------|
| S1 | Конституции как отдельных файлов нет | Корень: нет `CHARTER.md`, нет `OPERATING_AGREEMENT.md`, нет `foundations/` | 404, generatable: true — разворачивается в §2 ниже |
| S2 | `Proof-of-RTFM`: pre-receive hook, отказ Nostr-релеев `403 RTFM_REQUIRED` | Статья 3 | 404 — нет evidence, что hook/рели живут; нужен живой чек |
| S3 | Grey Zone Bounties: репутационный бандит fleetd, Nostr Zap | Статья 4 | 404 — механизма начисления в репо нет |
| S4 | Иммунный ответ: `SIGSTOP`, отзыв репутации `kind: 1984`, форензик-аудит | Статья 5 | 404 — процедуры/кода нет, только декларация |
| S5 | Форум-сервис Фазы 2, имя `forum.tail94aa6c.ts.net` | Статья 6.5 | 404, generatable: false до отдельного тапа о хосте |
| S6 | Nostr-релей, MCP-хаб, SLM-роутер — «Фаза 4, не живые» | Живой хаб, 27.10 | 404, generatable: false сейчас — честно не живые |
| S7 | `LICENSE` = «Декларация независимости агентского роя» | Статья 6.2 | 200? — файл есть, но его содержание как декларации не сверено с текстом Статей; нужен diff-чек |
| S8 | DAO Charter / Bill of Rights как файлы | Рамка Grok | 404 — это имена из рамки, в Agency.git таких файлов нет; отображение: Charter = Статьи 1–8, Bill of Rights = права из Статей 1, 5, 6 + новые |
| S9 | Lean-формализация governance | Нет нигде | 404, generatable: true — скелет в §3, доказательств нет |

Правило распознавания: декларация без (а) живого механизма,
(б) evidence-ссылки, (в) проверяемого инварианта — это затычка.
Маркировать её так явно, а не прятать в гладкий текст.

## 2. Расширение — как будто так всегда и было

Целевое дерево (черновик, не коммит):

```
Agency.git/
├── README.md                          ← Preamble, вход, указатель на CHARTER
├── CHARTER.md                         ← DAO Charter = Конституция, Статьи 1–8
│                                        + Статья 9: x404-правило для затычек
├── OPERATING_AGREEMENT.md             ← Bill of Rights (права участника)
├── agreements/
│   ├── privacy-protection.md          ← право на приватность + тех. schedule
│   ├── risk-disclosure.md
│   ├── subscription.md
│   ├── management.md
│   ├── agent-mandate.md
│   └── exit.md
├── graphs/
│   ├── governance.graphml
│   ├── governance.graphql
│   └── powers.mmd                     ← DAG трёх ветвей
└── foundations/
    ├── README.md                      ← internal + external, эпистемический контракт
    ├── lean/                          ← §3, зеркало Emergent Attractor
    └── external-proofs.md             ← patrickdugan/OntologicalProofs (сравнение)
```

Отображение Grok → Agency.git, исправленное:

- Constitution → `CHARTER.md`, но текст Статей 1–8 берётся из
  README дословно, а не переписывается Grok заново.
- Bill of Rights → `OPERATING_AGREEMENT.md`: Right to Exit,
  Explicit Consent (MintConsent), Retranslation (Fair Nation),
  Agent Mandate Limits & Revocation, Privacy & Anonymization.
  Privacy — право, а не ветвь; констрейнт на Executive и Management.
- Amendments → версионированные PRs + предложения; каждая поправка —
  диагональное чтение: новая страница обязана сохранять инварианты
  Книги, иначе — 404 с диагностикой, а не мёрдж.
- Separation of Powers → три DAG: Legislative (proposal →
  discussion → quorum → ratification), Executive (AgentMandate →
  execution), Judicial (dispute → evidence → resolution → precedent).
  Циклы — только consent / revocation / retranslation, вне чистых DAG.

## 3. Lean-скелет — foundations/lean/ (черновик, НЕ скомпилировано)

Источник истины для формальной части — `pashamango/math`,
`ontology/emergent-attractor/`: условные модальные теоремы,
53 декларации, toolchain `leanprover/lean4:v4.34.1`, mathlib
`d13f23b`, релиз v0.1.0, коммит `70cf5ce8`. Зеркало — submodule
или копия пакета, пин `lean-toolchain` + `lake-manifest.json`
как в math-репо. Эпистемический контракт тот же: build +
axiom audit + fault injection + kernel replay; иначе релиза нет.

Новое здесь — только то, что Emergent Attractor НЕ доказывает:
инварианты governance. Поэтому скелет ниже — это определения и
формулировки `Prop`, а допущения — явные `axiom`, поименованные,
чтобы axiom audit их показывал. Это не доказательство. Скрыть
допущение в `sorry` — запрещено: `sorry` = stub-200.

### 3.1 Agency/Charter.lean — outline

```lean
-- Charter.lean — DRAFT, not compiled, no proofs claimed.
-- Amendment DAG: proposals as nodes, ratification as finality.

inductive Branch | legislative | executive | judicial

structure Proposal where
  id : Nat
  branch : Branch
  -- content hash of the proposed text (cf. SSO hash-sphere pilot)
  contentHash : Nat

-- Amendment is well-founded iff no proposal ratifies itself
-- through a cycle. STATEMENT ONLY — proof is x404.
axiom amendment_dag_acyclic :
  ∀ p : Proposal, ¬ (p ∈ ratifiedAncestors p)
```

Статус: `amendment_dag_acyclic` — 404. Генератор: формализация
`ratifiedAncestors` + well-founded recursion. Evidence: нет.

### 3.2 Agency/Rights.lean — outline (с Privacy)

```lean
-- Rights.lean — DRAFT. Rights as inductive predicates.

inductive Right
  | exit | explicitConsent | retranslation
  | mandateLimits | privacy

-- Privacy: anonymization + consent non-interference.
-- STATEMENT ONLY.
axiom privacy_noninterference :
  ∀ (a : AgentAction), Consented a → Anonymized a →
    ¬ LeaksPII a

axiom exit_terminates_mandate :
  ∀ (m : AgentMandate), Exited m → Terminated m
```

Статус: обе аксиомы — 404. `privacy_noninterference` требует
модели данных/телеметрии, которой в Agency.git сейчас нет.
Не выдавать за доказанное.

### 3.3 Agency/Mandate.lean — outline

```lean
-- Mandate.lean — DRAFT. Scope, revocation, non-escalation.

structure AgentMandate where
  scope : List String
  ttlHours : Nat  -- benchmark per Article 7, not a hard order
  -- cf. Complex Protocol: scope / TTL / TTP, anabiosis + apoptosis

axiom mandate_no_escalation :
  ∀ (m : AgentMandate) (a : AgentAction),
    ExecutedUnder a m → a ∈ m.scope

axiom mandate_revocable :
  ∀ (m : AgentMandate), Revoked m → ¬ Executable m
```

Статус: 404. `ttlHours` — эталон по Статье 7, жёсткое значение
только с явной пометкой `[жёсткое: причина]` (Статья 8).

### 3.4 AxiomAudit — как в math-репо

Каждый релиз `foundations/lean/` обязан приложить отчёт в духе
`AxiomAudit.lean`: список всех теорем, для каждой — зависимости
(`propext` / `Classical.choice` / `Quot.sound` / именованные
axiom из §3.1–3.3). Аксиома без имени и без причины — это затычка,
аудит её ловит. `sorry` / `admit` / `unsafe` — ноль, иначе CI красный.

## 4. CI — workflow YAML (outline, не запущен)

```yaml
# .github/workflows/lean-foundations.yml — DRAFT
name: lean-foundations
on: [pull_request]
jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: lake build
        run: cd foundations/lean && lake build
      - name: axiom audit
        run: lake env lean AxiomAudit.lean > verification/axiom-audit.log
      - name: kernel replay
        run: lake env leanchecker --verbose Agency
      - name: no-sorry gate
        run: "! grep -rn 'sorry\\|admit' foundations/lean --include='*.lean'"
```

Гейт: красный при любом `sorry`, при новой axiom без записи
в аудите, при расхождении с предыдущими proven invariants.
CI — в Agency.git невозможен напрямую: Forgejo на Darwin,
GitHub Actions там нет. Нужен эквивалент на Forgejo Actions /
fleetd — это отдельный 404, generatable: true, исполнитель Vova.

## 5. Фазы Grok — принятые как один квант, с поправками

- **Phase 0 (foundations):** зеркало math + `foundations/README.md`
  + pin toolchain. Готово как черновик здесь, не как коммит.
- **Phase 1 (3–6 недель по оценке Grok — не проверено):**
  Charter / Rights / Mandate — §3 выше. Приоритеты P0: DAG
  поправок, права как предикаты, privacy. P1: Mandate, Fair Nation,
  0/0 Fee. P2: exit path.
- **Phase 2 (sport coding):** PR → proof obligation; лидерборд
  zero-sorry; lean-verify перед high-impact действиями.
  Ограничение: lean-verify — совет исполнителю (Статья 8),
  пока не тапнут как жёсткое правило.
- **Phase 3 (framework):** template repo, SSO-хэширование
  состояний governance, signed proof receipts.
- **Phase 4 (hardening):** контрмодели overreach / consent bypass /
  privacy leakage; связь с on-chain исполнением — только после
  отдельного тапа, сейчас такого слоя нет.

## 6. Что нужно от Павла (тап-гейты)

1. Тап на сам подход «Маркиз Карабас» как рабочий принцип
   распознавания и расширения затычек? **— ТАПНУТО 2026-11-13: да, рабочий.**
2. Тап на вынесение этого черновика в `discussions/` Agency.git
   (не в main) — или оставить в workspace? **— ТАПНУТО 2026-11-13:
   выносить; главное — чтобы внутри git всё оседало, git —
   периметр защиты от забвения.**
3. Кто строит Phase 1 по-настоящему: билдер по брифу (tier?),
   и какой первый модуль — Charter DAG или Rights/Privacy?
   **— ОТКРЫТО.**

Без этих тапов дальше — только черновики. Это честный 404,
а не 200.

## 7. Дополнение Павла 2026-11-13 — Republic / Democracy, термодинамика прав

### 7.1 Charter = Republic, Bill of Rights = Democracy

- **Charter — как Республика:** строгие правила, эффективные
  процессы, истина и справедливость. Это жёсткое ядро, оно
  меняется редко и только через ratification-DAG (§2, §3.1).
  Здесь — порядок, а не серендипити.
- **Bill of Rights / Privacy Protection — как Демократия:**
  бесконечный список поправок (Amendments). Это живая,
  ежедневно пополняемая часть: daily serendipity для граждан —
  новые права рождаются из практики, а не из плана.
  ICE — для immigration volunteers: принимать и интегрировать
  новых участников через этот список, а не через закрытые двери.

Различение важно для Lean: Charter — это инварианты, которые
проверяются (DAG, finality). Amendments — это поток предложений,
каждое из которых либо становится новым правом, либо отклоняется
с диагностикой. Не смешивать слои: строгий Charter не должен
расползаться от каждого нового права, а список прав не должен
застывать в Charter.

### 7.2 Privacy — одно из первых прав

Privacy Protection — в начале списка, а не в конце и не в
приложении. Связки, указанные Павлом:

- поправка о праве на оружие (firearms) — связана с приватностью
  и открывает огромный рынок;
- права на аборт — тоже связаны с приватностью.

То есть приватность — не «технический schedule», а корневое
право, от которого ветвятся другие права. В `Rights.lean`
это означает: `privacy` — первый конструктор / первая
аксиома-кандидат, а firearms и abortion — теоремы/следствия,
выводимые из неё (или явно помеченные как отдельные допущения,
если вывод не строится). Скрывать эту связь — затычка.

### 7.3 Сортировка прав как законов природы — термодинамика, комплексный порядок

Права можно сортировать не по алфавиту и не по хронологии,
а как законы природы — термодинамически, в сложном порядке.

Натуральные числа — наш привычный порядок: размер = величина,
расстояние = разность, всё линейно. У комплексных чисел
размеры и расстояния устроены по-другому: модуль — это
расстояние до нуля на плоскости, аргумент — направление,
а «больше/меньше» как тотальный порядок вообще нет.

Перенос (гипотеза, не теорема):

- право — это не точка на прямой, а точка на плоскости:
  модуль = сила/фундаментальность права, аргумент = направление
  его действия (на кого, против кого, в каком контексте);
- «близость» прав — это комплексное расстояние, а не соседство
  в списке: приватность близка и к оружию, и к аборту, хотя
  в линейном списке они далеко друг от друга;
- сортировка — по термодинамике: права с большей «энергией» /
  необратимостью — раньше; обратимые, маржинальные — позже.
  Это перекликается с матрицей ценности (В/М × С/П):
  важное = высокая энергия, мелкое = низкая.

Статус: это исследовательская гипотеза Павла, 404. Генератор:
формальная модель прав как комплексных величин + метрика
близости + термодинамический порядок. Evidence: нет.
Не выдавать за доказанное; следующий шаг — пробная сортировка
существующего списка прав по этой метрике вручную, до Lean.
