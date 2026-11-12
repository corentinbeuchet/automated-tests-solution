# 🔒 Solution prof – Exercice 3 « automated-tests »

> Dépôt **privé**, réservé à l'enseignant. Ne pas rendre public ni le lier depuis un support étudiant.

Chaque étape de l'énoncé est une **branche**, et chaque branche a sa **Pull Request ouverte vers `main`** (jamais mergée) : l'onglet **Checks** de la PR montre le résultat exact que doit voir l'étudiant à cette étape.

- `main` = projet tel que généré par Spring Initializr (étapes 1-2), avec `gradlew` **sans** droit d'exécution, comme s'il avait été committé depuis Windows.
- Les branches se suivent : chacune part de la précédente **verte**. Les branches « rouge » sont des impasses volontaires.

| Étape(s) README | Branche | Résultat CI attendu | Ce qu'on doit voir |
|---|---|---|---|
| 4 → 8 | `etape-08a-ci-rouge-permission-denied` | 🔴 `build` | `./gradlew: Permission denied` (piège 1, Windows) |
| 8 (correction) | `etape-08b-ci-verte` | 🟢 `build` | `git update-index --chmod=+x gradlew` |
| 9 | `etape-09-regression-rouge` | 🔴 `build` | `HelloControllerTest > leMessageHelloDoitEtreCorrect() FAILED` |
| 10 → 11 | `etape-11-livrable` | 🟢 `build` | Artifacts `app` (app.jar) et `rapports` |
| 12 → 13 | `etape-13a-checkstyle-rouge` | 🔴 `build` | `[AvoidStarImport]` sur `HelloControllerTest.java` (piège 2) |
| 13 (correction) | `etape-13b-checkstyle-vert` | 🟢 `build` | import `Assertions.assertEquals` |
| 14 → 15 | `etape-15-performance` | 🟢 `build` + `performance` | `p(95)` ≈ quelques ms, `http_req_failed` 0 % |
| 16 | `etape-16-performance-rouge` | 🟢 `build`, 🔴 `performance` | seuil `p(95)<1` franchi |

**Solution finale** = `etape-15-performance` (l'étape 16 se termine en remettant `p(95)<200`).

## Hors dépôt (à faire dans les Settings, non reproductible ici)
- Étape 3 / 8 : règle de protection de `main` (PR obligatoire, Require approvals décoché, check `build`, no bypass). Sur un dépôt **privé** GitHub Free, la protection de branche n'est pas disponible : c'est pourquoi l'énoncé exige un dépôt public.
- Bonus 16 : ajouter `performance` aux checks obligatoires.

## Versions utilisées
Spring Boot 4.1.2 · Gradle 9.9.1 · Java 25 (Temurin) · Checkstyle 14.4.0 · `ubuntu-26.04` · `actions/checkout@v7`, `actions/setup-java@v6`, `actions/upload-artifact@v7`, `actions/download-artifact@v8`, `grafana/setup-k6-action@v1`, `grafana/run-k6-action@v1`.
