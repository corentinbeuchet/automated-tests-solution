# 🔒 Solution prof – Exercice 3 « automated-tests »

> Dépôt **privé**, réservé à l'enseignant. Ne pas rendre public ni le lier depuis un support étudiant.

Chaque étape de l'énoncé est une **branche**, et chaque branche a sa **Pull Request ouverte vers `main`** (jamais mergée) : l'onglet **Checks** de la PR montre le résultat exact que doit voir l'étudiant à cette étape.

- `main` = projet tel que généré par Spring Initializr (étapes 1-2), avec `gradlew` **sans** droit d'exécution, comme s'il avait été committé depuis Windows.
- Les branches se suivent : chacune part de la précédente **verte**. Les branches « rouge » sont des impasses volontaires.

| Étape(s) README | PR | Branche | Résultat CI (vérifié le 12/11/2026) | Ce qu'on voit dans les logs |
|---|---|---|---|---|
| 4 → 8 | [#1](https://github.com/corentinbeuchet/automated-tests-solution/pull/1) | `etape-08a-ci-rouge-permission-denied` | [🔴 `build`](https://github.com/corentinbeuchet/automated-tests-solution/actions/runs/43217926214) | `./gradlew: Permission denied` (exit 126) — piège 1, Windows |
| 8 (correction) | [#2](https://github.com/corentinbeuchet/automated-tests-solution/pull/2) | `etape-08b-ci-verte` | [🟢 `build`](https://github.com/corentinbeuchet/automated-tests-solution/actions/runs/43218026647) | `git update-index --chmod=+x gradlew` |
| 9 | [#3](https://github.com/corentinbeuchet/automated-tests-solution/pull/3) | `etape-09-regression-rouge` | [🔴 `build`](https://github.com/corentinbeuchet/automated-tests-solution/actions/runs/43218088649) | `HelloControllerTest > leMessageHelloDoitEtreCorrect() FAILED` · 2 tests completed, 1 failed |
| 10 → 11 | [#4](https://github.com/corentinbeuchet/automated-tests-solution/pull/4) | `etape-11-livrable` | [🟢 `build`](https://github.com/corentinbeuchet/automated-tests-solution/actions/runs/43218228780) | Artifacts `app` (app.jar) et `rapports` |
| 12 → 13 | [#5](https://github.com/corentinbeuchet/automated-tests-solution/pull/5) | `etape-13a-checkstyle-rouge` | [🔴 `build`](https://github.com/corentinbeuchet/automated-tests-solution/actions/runs/43218276965) | `HelloControllerTest.java:4:47 … [AvoidStarImport]` — piège 2 (rapports quand même publiés) |
| 13 (correction) | [#6](https://github.com/corentinbeuchet/automated-tests-solution/pull/6) | `etape-13b-checkstyle-vert` | [🟢 `build`](https://github.com/corentinbeuchet/automated-tests-solution/actions/runs/43218332894) | import `Assertions.assertEquals` |
| 14 → 15 | [#7](https://github.com/corentinbeuchet/automated-tests-solution/pull/7) | `etape-15-performance` | [🟢 `build` + `performance`](https://github.com/corentinbeuchet/automated-tests-solution/actions/runs/43218438620) | k6 : thresholds respectés, 0 % d'erreurs |
| 16 | [#8](https://github.com/corentinbeuchet/automated-tests-solution/pull/8) | `etape-16-performance-rouge` | [🟢 `build`, 🔴 `performance`](https://github.com/corentinbeuchet/automated-tests-solution/actions/runs/43218492909) | `p(95)=3.59ms` > 1 ms, 0 % d'erreurs, ≈14 400 req/s → `Test failed with code: 99` |

**Solution finale** = `etape-15-performance` (l'étape 16 se termine en remettant `p(95)<200`).

## Hors dépôt (à faire dans les Settings, non reproductible ici)
- Étape 3 / 8 : règle de protection de `main` (PR obligatoire, Require approvals décoché, check `build`, no bypass). Sur un dépôt **privé** GitHub Free, la protection de branche n'est pas disponible : c'est pourquoi l'énoncé exige un dépôt public.
- Bonus 16 : ajouter `performance` aux checks obligatoires.

## Versions utilisées
Spring Boot 4.1.2 · Gradle 9.9.1 · Java 25 (Temurin) · Checkstyle 14.4.0 · `ubuntu-26.04` · `actions/checkout@v7`, `actions/setup-java@v6`, `actions/upload-artifact@v7`, `actions/download-artifact@v8`, `grafana/setup-k6-action@v1`, `grafana/run-k6-action@v1`.
