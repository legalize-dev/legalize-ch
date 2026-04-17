# Legalize CH

### Schweizerische Bundesgesetzgebung in Markdown, versioniert mit Git.

Jedes Gesetz ist eine Datei. Jede Revision ist ein Commit.

**Offizielle Quelle:** [Fedlex](https://www.fedlex.admin.ch) — die Publikationsplattform der Schweizerischen Bundeskanzlei für die Systematische Rechtssammlung (SR), die Amtliche Sammlung (AS) und das Bundesblatt (BBl).

Teil des Projekts [Legalize](https://github.com/legalize-dev/legalize) · [legalize.dev](https://legalize.dev)

> **Anfangsphase** — Dieses Repository befindet sich in aktiver Entwicklung. Dateistruktur, Commit-Historie und Inhalt können sich erheblich ändern, einschliesslich vollständiger Regenerationen.

## Schnellstart

```bash
# Schweizer Bundesgesetzgebung klonen
git clone https://github.com/legalize-dev/legalize-ch.git

# In der Bundesverfassung suchen
grep -A 5 "Art. 1" ch/cc-1999-404.md

# Revisionsverlauf eines Gesetzes einsehen
git log --oneline -- ch/cc-1999-404.md

# Schweizerisches Zivilgesetzbuch lesen
less ch/cc-24-233_245_233.md
```

## Umfang (v1)

Die Systematische Rechtssammlung (SR / RS), d.h. der konsolidierte geltende Bestand des Bundesrechts: Bundesverfassung, Bundesgesetze, Bundesbeschlüsse, Verordnungen von Bundesrat, Departementen und Ämtern, sowie ratifizierte völkerrechtliche Verträge.

Dieses Repository erfasst:

- **5.827 Normen** mit mindestens einer Konsolidierung in Akoma Ntoso XML ODER PDF-A (alle Formate, die Fedlex pro SR-Nummer veröffentlicht)
- **Sprache: Deutsch** (weitere Sprachversionen als parallele Repositories geplant)
- **Historietiefe**: pro Norm so weit zurück, wie Fedlex irgendeinen Volltext vorhält — XML für Konsolidierungen ab ~2021, PDF-A für ältere Stände. Siehe das Frontmatter-Feld `history_from` pro Datei.

Für Normen, die teils als XML, teils als PDF vorliegen, ist der Übergang in der Commit-Historie nahtlos: Artikelnummerierung, Absatzstruktur und Fussnoten folgen dem gleichen Template (``**Art. N** Title``, ``<sup>N</sup> Absatz``, ``[^N]`` Fussnoten), unabhängig davon, aus welchem Quellformat der jeweilige Stand generiert wurde. Einziger sichtbarer Unterschied: die XML-Fussnoten tragen Hyperlink-Querverweise zu `AS` / `BBl`, die im PDF-Quellformat nicht vorhanden sind.

## Dateistruktur

```
ch/
  cc-1999-404.md             — Bundesverfassung der Schweizerischen Eidgenossenschaft
  cc-24-233_245_233.md       — Schweizerisches Zivilgesetzbuch (ZGB, SR 210)
  cc-220.md                  — Obligationenrecht (OR)
  cc-311_0.md                — Strafgesetzbuch (StGB)
  cc-1991-1184_1184_1184.md  — Bundesgesetz über die direkte Bundessteuer (DBG)
  ...
```

Die Struktur ist **flach** — ein Verzeichnis pro Land, keine Unterverzeichnisse nach Rangstufe. Die Normart steht im YAML-Frontmatter jeder Datei (Feld `rank`).

Legacy-IDs mit Unterstrichen (z.B. `cc-24-233_245_233`) werden unverändert aus der Fedlex-ELI übernommen.

## Format

Jede Datei ist Markdown mit einem YAML-Frontmatter-Block:

```yaml
---
title: "Bundesverfassung der Schweizerischen Eidgenossenschaft vom 18. April 1999"
identifier: "cc-1999-404"
country: "ch"
rank: "bundesverfassung"
publication_date: "1999-04-18"
last_updated: "2024-03-03"
status: "in_force"
source: "https://fedlex.data.admin.ch/eli/cc/1999/404"
department: "Bundeskanzlei"
short_title: "BV"
sr_number: "101"
entry_into_force: "2000-01-01"
applicability_date: "2024-03-03"
title_fr: "Constitution fédérale de la Confédération suisse du 18 avril 1999"
title_it: "Costituzione federale della Confederazione Svizzera del 18 aprile 1999"
title_en: "Federal Constitution of 18 April 1999 of the Swiss Confederation"
short_title_fr: "Cst."
short_title_it: "Cost."
short_title_en: "Cst."
authoritative: "true"
---
# Bundesverfassung der Schweizerischen Eidgenossenschaft vom 18. April 1999
...
```

## Commit-Konvention

Jeder Commit entspricht einer gesetzgeberischen Änderung an einer SR-Nummer:

- `[bootstrap]` — Erstimport einer Norm
- `[reforma]` — Teilrevision (`jolux:dateApplicability` wird zum Commit-Datum)
- `[nueva]` — Neue Norm
- `[derogacion]` — Aufhebung
- `[correccion]` — Amtliche Berichtigung

Jeder Commit-Trailer enthält:

- `Source-Id: <eli/cc/...>` — ELI der verursachenden Änderung
- `Source-Date: YYYY-MM-DD` — `jolux:dateApplicability` des resultierenden Standes
- `Norm-Id: <eli/cc/...>` — ELI der betroffenen Norm

Autor der Commits: `Legalize <legalize@legalize.dev>` (Projektbot).

## Lizenz

Die offiziellen Rechtstexte der Schweizerischen Eidgenossenschaft sind gemäss **Art. 5 Abs. 1 lit. a–c des Urheberrechtsgesetzes (URG)** vom Urheberrechtsschutz ausgenommen. Sie stehen somit im Gemeingut und können frei kopiert, veröffentlicht und umgearbeitet werden.

Die Aufbereitung dieses Repositories (Parser-Code, Commit-Struktur, Frontmatter) steht unter der **MIT-Lizenz** — siehe [LICENSE](LICENSE). Die amtliche Quelle ist [Fedlex](https://www.fedlex.admin.ch).

## Pipeline

Die XML- und PDF-A-Daten werden automatisch aus dem [SPARQL-Endpunkt der Bundeskanzlei](https://fedlex.data.admin.ch/sparqlendpoint) und dem öffentlichen Filestore abgeholt und mit [legalize-pipeline](https://github.com/legalize-dev/legalize-pipeline) in Markdown umgewandelt. Der Fetcher verwendet die gleiche JOLux-Ontologie wie das luxemburgische Legilux.
