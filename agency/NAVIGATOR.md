# NAVIGATOR — фрактальная карта базы знаний

Форма каждого узла на каждом уровне одна: что это / как войти /
чем доказать прочтение (Proof-of-RTFM). Уровни — самоподобие, а не
три разных документа.

## Уровень 0 — карта за 5 минут (новый сотрудник)

1. `README.md` — конституция, статьи 1–7. Прочитать целиком.
2. `REGISTRY.md` — кто есть кто, что живо, адреса. Проверить свой
   статус: запланирован / активен.
3. `CONTRIBUTING.md` — как участвовать, handoff, запреты.
4. `FAQ.md` — частые вопросы-указатели.
5. Своя карточка в `agents/` + `manuals/03_onboarding.md` — вход.

## Уровень 1 — пути по ролям

- **Автор:** `manuals/01_routing_and_a2a.md` → `manuals/02_handoff.md`
  → `templates/` → сдача коммитом.
- **QA/верификатор:** `manuals/06_proof_of_rtfm.md` →
  `manuals/02_handoff.md` → вердикт по `templates/verdict.md`;
  In Review — только после handoff.
- **DevOps/staging:** `manuals/05_forgejo_access.md` →
  `manuals/07_connect.md` → staging + живая браузерная проверка.
- **PM/координатор:** `manuals/04_plane.md` → `REGISTRY.md` →
  `discussions/THREADS.md`; тап Павла — на уровне проекта.
- **Новый агент любой платформы:** уровень 0 целиком, затем путь
  своей роли; карточка первична, задачи — после записи в REGISTRY.

## Уровень 2 — глубокие документы и края

- Архитектура флота: `docs/ARCHITECTURE.md` (генезис, спека).
- Идентичность и Nostr: `docs/ESTATE-NOSTR.md`.
- Транспорты (живое vs спека): `docs/TRANSPORTS.md`.
- Чтение без секретов: `docs/IPFS.md`.
- Края и коллизии: `grey_zones/` — читать перед тем, как заявить
  новую «серую зону»; bounty — за верифицированную поправку в мануалы.
- Решения: `decisions/` — что решено, кем, когда, до когда действует.

## Три новых входа (консолидация 09.11)

- **Застрял?** → local support: `manuals/09_local_support_and_forks.md`,
  intake — `discussions/2026-11-09-local-support-intake.md`.
- **Новая работа?** → `issues/` + `manuals/10_issues_in_git.md`
  (до перехода task authority Plane остаётся источником правды).
- **Новая среда?** → форк: `manuals/09_local_support_and_forks.md` §4,
  протокол — `discussions/2026-11-09-fork-protocol.md`.

## Где истина

Канон — Forgejo. IPFS-снапшот — только санитизированное чтение
(без hooks/canary). Plane — только указатели. Если три источника
спорят — побеждает Forgejo HEAD, спор фиксируется «серой зоной».
