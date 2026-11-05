# SPEC.md – Kleist Events

**Team:** K3  
**Kundin:** Sabine Kleist (Kleist Events)  
**Stand:** 05.11.2026 – finale Anforderungsfassung nach Meeting #1 (12/12) und Meeting #2 (8/8)  
**Quellenkürzel:** `M1-Fn` = Antwort auf Frage n in Meeting 1; `M2-Fn` = Antwort auf Frage n in Meeting 2.  
**Modellierungsgrundsatz:** Kundenwünsche werden von technischen Modellierungsentscheidungen unterschieden. Nicht spezifizierte Details bleiben offen.

## 1. Ausgangslage und Ziel

Sabine plant als Einzelunternehmerin insbesondere Firmenveranstaltungen und Hochzeiten. Eventdaten, Dienstleisterabsprachen, Budgets und Aufgaben liegen heute in Excel, E-Mails und Notizbüchern. Daraus entstehen Doppelbuchungen, verspätete Budgetüberschreitungen, vergessene Aufgaben und unvollständige Unterlagen. Die Anwendung soll die Planung pro Veranstaltung übersichtlich und nachvollziehbar machen. [M1-F1, M1-F2, M1-F5, M1-F11]

**Rollen:** Sabine verantwortet Auswahl, Verhandlungen, Budgetfreigaben und Buchungsstatus. Ihr Mann übernimmt Buchhaltungsaufgaben und darf Buchungen als „bezahlt“ kennzeichnen. Die Tochter hilft gelegentlich vor Ort, ohne administrative Entscheidungsrechte. Eine umfassende Mehrbenutzerverwaltung ist für V1 nicht ausdrücklich als Pflicht bestätigt. [M1-F6, M2-F3, M2-F7]

## 2. Umfang und Prioritäten

### 2.1 Verbindlicher Kernumfang – Version 1

1. **Eventübersicht und Dienstleisterbuchungen:** Kunden, Kontakte, Veranstaltungsdaten, eventbezogene Hinweise sowie Dienstleister mit vereinbartem Preis und Buchungsstatus zentral anzeigen und bearbeiten. [M1-F2, M1-F4, M1-F11, M2-F1]
2. **Budgettracking:** Eventbudget und bestätigte Buchungskosten vergleichen; reservierte Kosten separat prognostizieren; Warnung bei 85 %; tatsächlichen Rechnungsbetrag direkt an der Buchung erfassen, Abweichungen und deren Wirkung auf die Gesamtauslastung sichtbar machen; Stornokosten statt des ursprünglichen Buchungsbetrags berücksichtigen. [M1-F5, M2-F1, M2-F4, M2-F5, M2-F8]
3. **Aufgabenliste pro Event:** Titel, Deadline, Status (offen/erledigt/blockiert), Priorität und Verantwortlichkeit; Warnung bei überfälligen, nicht erledigten Aufgaben. Verantwortlich können Sabine, ihr Mann, eine Aushilfe oder ein Dienstleister sein. [M1-F11, M2-F2, M2-F7]

**Begleitende V1-Anforderungen:** Warnungen bei möglichen Dienstleister-Terminkonflikten, Bestätigung ohne erfasste Anzahlung und später Bestätigung nach der Drei-Wochen-Grenze; keine automatische Buchungssperre. Dokumentation von Stornodatum, Stornokosten und Stornonotiz. Ansprechpartnerhistorie einschließlich Hauptkontakt je Event. [M1-F8, M1-F9, M2-F3, M2-F6–F8]

### 2.2 Spätere Versionen / nicht Teil des V1-Kerns

- Vollständige Rechnungsverwaltung: Dokumente, Zahlungsabwicklung, ausgehende Rechnungen und vollständiges Rechnungsstatus-Tracking; V1 erfasst **nur den tatsächlich berechneten Betrag je Buchung**. [M2-F1, M2-F5]
- Vertragsablage und Vertragsstatus mit Unterzeichnungsdatum. [M1-F11, M2-F1]
- Separater Eventkalender. [M2-F1]
- **Eine neue Zukunftsanforderung:** wiederverwendbare **Eventvorlagen für Stammkunden** mit üblichen Dienstleistern, Standardaufgaben und Budgetrichtwert, die bei neuen Events übernommen und angepasst werden können. Nicht Teil von V1. [M1-F12, M2-F1]

### 2.3 Fachliche Rahmenbedingungen

- Veranstaltungsarten unterscheiden sich in Dienstleisterbedarf und Vorlauf; Hochzeiten werden typischerweise mindestens zwölf Monate, Firmenveranstaltungen oft drei bis sechs Monate im Voraus geplant. [M1-F3]
- Bei Hochzeiten nutzt Sabine normalerweise feste Kontakte für Floristik und Torte, lässt auf Wunsch aber auch kundeneigene Anbieter zu. [M1-F3]
- Für Kunden sind grundsätzlich 30 % Anzahlung nach Bestätigung und weitere 70 % spätestens 14 Tage vor dem Event vorgesehen; die App erfasst für die Buchungswarnung, ob die Anzahlung eingegangen ist. Ein vollständiges Zahlungsmodul gehört nicht zu V1. [M1-F7, M1-F8, M2-F1]
- Stornokonditionen variieren je Vertrag; es wurde **kein allgemeiner Stornoprozentsatz** vereinbart. Bei Stamm-Dienstleistern können Gebühren nach mündlicher Absprache entfallen. [M1-F10, M2-F8]

## 3. Fachliches ER-Modell – genau 8 Entitäten

Technische IDs und einzelne Hilfsfelder sind **Modellierungsentscheidungen**, nicht wörtlich von Sabine vorgegebene Bezeichnungen. Zusammengesetzte Angaben sind bewusst gruppiert, um das geforderte konzeptionelle Modell kompakt zu halten.

| Nr. | Entität / Primärschlüssel | Fachliche Attribute (zusätzlich zur ID) | Begründung |
|---|---|---|---|
| E1 | **Kunde** (`kunde_id`) | `name`, `kundentyp` (Firma/privat), `stammkunde` (ja/nein) | Auftraggeber als dauerhafte Einheit; Stammkunden-Ausnahmen. [M1-F2, M1-F8, M2-F6] |
| E2 | **Ansprechpartner** (`kontakt_id`) | `name`, `telefon`, `email`, `aktivstatus` | Mehrere Kontakte je Kunde, auch ausgeschiedene behalten. [M2-F6] |
| E3 | **Veranstaltung** (`event_id`) | `titel`, `datum_und_ort`, `gaestezahl`, `art_und_status`, `budget` | Event-Stammdaten; eventbezogene Hinweise sind zusätzlich erforderlich (siehe offene Modellierungsdetails). [M1-F2] |
| E4 | **EventKontakt** (`event_kontakt_id`) | `ist_hauptkontakt`, `zugeordnet_am`, `historienhinweis` | Verbindet mehrere Kontakte mit einem Event; genau einer ist Hauptkontakt. [M2-F6, M2-F7] |
| E5 | **Dienstleister** (`dienstleister_id`) | `name`, `gewerk`, `kontaktdaten`, `stammanbieter`, `kapazitaetshinweis` | Anbieter kann mehrere Events bedienen; Kapazität variiert. [M1-F3, M1-F9] |
| E6 | **Buchung** (`buchung_id`) | `status`, `vereinbarter_preis`, `tatsaechlicher_betrag`, `stornokosten`, `stornodaten_und_notiz` | Verbindet Event und Dienstleister; Kosten und Storno gehören zur einzelnen Zuordnung. [M2-F3–F5, M2-F8] |
| E7 | **Aufgabe** (`aufgabe_id`) | `titel`, `deadline`, `status`, `prioritaet`, `verantwortlichkeit` | Konkrete Aufgaben je Event, auch externe Verantwortliche möglich. [M2-F2, M2-F7] |
| E8 | **Benutzer** (`benutzer_id`) | `name`, `rolle`, `berechtigungsprofil`, `aktivstatus` | Abbildung von Sabine/Buchhaltung und unterschiedlichen Statusrechten; Konten für Tochter/Dienstleister nicht vorausgesetzt. [M1-F6, M2-F3, M2-F7] |

**Hinweis zur Granularität:** `datum_und_ort`, `art_und_status` und `stornodaten_und_notiz` sind fachliche Attributgruppen, die bei der technischen Umsetzung in Einzelfelder aufgeteilt werden. Eventnotizen, Eingangsstatus der Anzahlung und Buchungszeitraum müssen in der Implementierung zusätzlich erfasst werden; sie sind hier bewusst nicht als weitere Entitäten ausgegeben. Der Aufgabenverantwortliche kann zunächst als beschreibende Angabe geführt werden, auch wenn keine Anmeldung existiert. [M1-F2, M1-F8, M2-F7]

## 4. Beziehungen – genau 8 mit Kardinalitäten

| Nr. | Beziehung | Kardinalität | Erläuterung / Beleg |
|---|---|---|---|
| R1 | Kunde — Ansprechpartner | **1 : n** | Ein Kunde hat mehrere Kontakte; jeder Kontakt ist in diesem Modell genau einem Kunden zugeordnet. [M2-F6] |
| R2 | Kunde — Veranstaltung | **1 : n** | Ein Kunde kann mehrere Veranstaltungen beauftragen; jedes Event hat einen Auftraggeber. [M1-F2, M1-F12] |
| R3 | Veranstaltung — EventKontakt | **1 : n** | Ein Event hat mindestens einen zugeordneten Kontakt. [M2-F7] |
| R4 | Ansprechpartner — EventKontakt | **1 : n** | Ein Kontakt kann an mehreren Events beteiligt sein. [M2-F6, M2-F7] |
| R5 | Veranstaltung — Buchung | **1 : n** | Ein Event kann viele Buchungen haben; jede Buchung gehört zu einem Event. [M1-F4] |
| R6 | Dienstleister — Buchung | **1 : n** | Ein Dienstleister kann bei vielen Events gebucht sein. Über Buchung entsteht **Veranstaltung ↔ Dienstleister n:m**. [M1-F4, M1-F9] |
| R7 | Veranstaltung — Aufgabe | **1 : n** | Ein Event hat mehrere Aufgaben; jede Aufgabe gehört zu einem Event. [M2-F2] |
| R8 | Benutzer — Aufgabe | **1 : n, optional auf Aufgabenseite** | Ein Benutzer kann viele Aufgaben verantworten; Aufgaben dürfen alternativ einen externen/nicht angemeldeten Verantwortlichen als Text tragen. **Technischer Modellvorschlag**, da Sabine keine Benutzerkonten für alle Zuständigen verlangt. [M2-F2, M2-F7] |

**Zusätzliche Integritätsbedingung:** Unter den EventKontakt-Zuordnungen einer Veranstaltung muss **genau eine** als `ist_hauptkontakt = true` markiert sein. Auch frühere Kontakte bleiben gespeichert. [M2-F6, M2-F7]

## 5. Genau 5 Business Rules

Die fünf nachstehenden Regeln sind die **ausgewählten** Business Rules für das Bewertungsmodell. Weitere Anforderungen in Abschnitt 2 und 6 bleiben funktionale Anforderungen, werden aber nicht als zusätzliche nummerierte Business Rules gezählt.

**BR1 – Anzahlung und Bestätigung:** Vor der verbindlichen Dienstleisterbestätigung soll die 30-%-Anzahlung erfasst sein. Fehlt sie, zeigt die App eine Warnung; Sabine darf die Bestätigung bewusst trotzdem durchführen. Bei Neukunden ist Sabines fachliche Praxis strikt gegen eine solche Ausnahme. [M1-F7, M1-F8, M2-F3]

**BR2 – Buchungsfrist:** Dienstleister sollen spätestens drei Wochen vor dem Event verbindlich bestätigt sein. Bei späterer Bestätigung zeigt die App eine deutliche Risikowarnung, blockiert aber nicht. [M1-F4, M2-F3]

**BR3 – Budgetwarnung und Freigabe:** Erreichen die Kosten **bestätigter Buchungen** mindestens 85 % des Eventbudgets, wird gewarnt; über zusätzliche Ausgaben entscheidet Sabine persönlich. Reservierungen werden getrennt als mögliche Kosten dargestellt. [M1-F5, M2-F4]

**BR4 – Terminkonflikte:** Ist ein Dienstleister am selben Tag bereits für ein anderes Event eingeplant, weist die App auf den möglichen Konflikt hin, sperrt die Buchung jedoch nicht. Sabine entscheidet nach Gewerk und Kapazität. [M1-F9]

**BR5 – Stornierte Buchungen im Budget:** Bei Status „storniert“ zählt nicht mehr der ursprüngliche Buchungspreis, sondern die unmittelbar erfassten tatsächlichen Stornokosten. Ein Gebührenverzicht (auch 0 €) ist mit Notiz dokumentierbar. [M2-F8]

## 6. Weitere präzise Funktionsanforderungen (keine zusätzlichen BR-Nummern)

- **Sechs Buchungsstatus, exakt wie genannt:** `angefragt`, `reserviert`, `bestätigt`, `durchgeführt`, `bezahlt`, `storniert`. Sabine darf Status ändern; ihr Mann darf ausschließlich `bezahlt` setzen; Tochter nicht. [M2-F3]
- **Budget:** Die 85-%-Schwelle basiert auf bestätigten Buchungen, nicht auf erst später eintreffenden Rechnungen. Reservierungen werden separat als mögliche Kosten angezeigt. Pro Buchung wird der vereinbarte mit dem tatsächlich berechneten Betrag verglichen; Abweichungen sind sichtbar und verändern die dargestellte Gesamtbudgetwirkung. [M2-F4, M2-F5]
- **Aufgaben:** Titel, Deadline, Status `offen`/`erledigt`/`blockiert`, Priorität und Zuständigkeit; Hinweis bei überschrittener Deadline für nicht erledigte Aufgaben. [M2-F2]
- **Ansprechpartner:** Mehrere pro Event, **genau einer als Hauptkontakt**. Frühere Ansprechpartner und Kontaktdaten bleiben nachvollziehbar. [M2-F6, M2-F7]
- **Stornierung:** Datum, Kosten und erläuternde Notiz, auch bei mündlich vereinbartem Gebührenverzicht. [M2-F3, M2-F8]
- **Eventhinweise:** Besondere kundenspezifische oder eventbezogene Hinweise sollen nicht in privaten Notizzetteln verloren gehen. [M1-F2, M1-F12]
- **Änderungsnachvollziehbarkeit:** Sabine wünscht bei Eventänderungen Zeitpunkt, Person, Grund sowie Anzeige betroffener Buchungen/Fristen. Der genaue Umfang eines automatischen Änderungsprotokolls in V1 ist **noch nicht entschieden**. [M1-F10]

## 7. Genau 2 dokumentierte Widersprüche und ihre Auflösung

**W1 – Verbindliche Buchung vor oder nach Anzahlung**  
- **Aussage A:** Dienstleister werden nach Kundenzusage fest bestätigt. [M1-F4]  
- **Aussage B:** Ohne 30-%-Anzahlung beginnt keine verbindliche Planung. [M1-F7]  
- **Klärung:** Offizielle Reihenfolge: erst Anzahlung, dann Bestätigung. In der Praxis werden bei vertrauten Stammkunden vorab Reservierungen vorgenommen; bei einer Bestätigung ohne Zahlung soll die App **warnen, nicht sperren**. Sabine entscheidet, Neukunden behandelt sie strikt. [M1-F8, M2-F3]

**W2 – Alles sofort oder priorisierter Startumfang**  
- **Aussage A:** Sabine empfindet alle Funktionen als sofort wichtig. [M1-F11]  
- **Aussage B:** Sie priorisiert Eventübersicht/Buchungen, Budget und Aufgaben gegenüber Rechnungen und Verträgen. [M1-F11]  
- **Klärung:** Sabine bezeichnet das selbst ausdrücklich als Widerspruch und bestätigt die drei Kernfunktionen für V1; Verträge, vollständige Rechnungsverwaltung, Kalender und Eventvorlagen folgen später. [M2-F1]

## 8. Zukunftsanforderung – genau eine

**Z1: Wiederverwendbare Eventvorlagen für Stammkunden.** Sabine möchte bei wiederkehrenden Firmenveranstaltungen eine Vorlage mit typischen Dienstleistern, Standardaufgaben und Budgetrichtwert auswählen, für ein neues Event übernehmen und anschließend anpassen. Diese Funktion gehört **nicht** zu V1. [M1-F12, M2-F1]

## 9. Offene technische und fachliche Details

Diese Punkte wurden nicht eindeutig festgelegt und dürfen nicht als bestätigte Kundenregeln ausgegeben werden:

1. Exakte Erfassung und Speicherung der Anzahlung in V1 (z. B. einfacher Zahlungseingangsmarker statt Zahlungsmodul). [M1-F8, M2-F1]
2. Konkrete Statusübergänge, insbesondere aus welchen Buchungsstatus storniert werden darf. [M2-F3]
3. Ob bei der Budget-Gesamtwirkung nachträglich berechnete Ist-Beträge die ursprünglichen Buchungsbeträge ersetzen oder zusätzlich in einer getrennten Sicht gezeigt werden; **keine Doppelzählung** vorsehen. [M2-F4, M2-F5]
4. Behandlung von bereits durchgeführten/bezahlten Buchungen in der 85-%-Basis und exakte Zeitlogik für Terminüberschneidungen. [M2-F3, M2-F4, M1-F9]
5. Technische Umsetzung des Zugriffs für den Ehemann sowie der Zuständigkeit für externe Dienstleister und gelegentliche Aushilfen. [M1-F6, M2-F7]
6. Detailtiefe der Änderungshistorie, Speicherung von Eventnotizen und fachlicher Vertragsmetadaten außerhalb des V1-Kerns. [M1-F2, M1-F10, M1-F11]

## 10. Abnahmekriterien für den V1-Kern

- Ein Event kann mit Kunde, mehreren Ansprechpartnern (genau einem Hauptkontakt), Dienstleisterbuchungen und Aufgaben dargestellt werden.
- Jede Buchung hat einen der sechs festgelegten Status und einen vereinbarten Preis; tatsächliche Kosten und Stornokosten sind erfassbar.
- Bei fehlender Anzahlung, später Bestätigung und möglichem Dienstleister-Terminkonflikt wird gewarnt, aber nicht automatisch gesperrt.
- Die Budgetansicht trennt verbindliche Kosten von reservierten möglichen Kosten, warnt ab 85 % und zeigt Ist-Abweichungen sowie Stornokosten ohne Doppelzählung.
- Aufgaben haben Titel, Deadline, Priorität, Status und Verantwortlichkeit; überfällige unerledigte Aufgaben werden sichtbar hervorgehoben.
- Eine vollständige Rechnungs- oder Vertragsablage sowie Eventvorlagen werden für V1 **nicht** vorausgesetzt.

---

**Formale Kontrolle:** 8 Entitäten · 8 Beziehungen (einschließlich n:m) · genau 5 ausgewählte Business Rules · genau 2 dokumentierte Widersprüche · genau 1 neue Zukunftsanforderung · Quellenbelege aus beiden Meetings.
