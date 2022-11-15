# Markdown2Presentation

Ein Gedankenspiel zur Verwaltung von Lehrveranstaltungs-Unterlagen.

## Einleitung

Bei der Suche nach einer geeigneten Methode zur raschen Erstellung von Vorlesungsfolien mit Markdown bin ich über eine Node-JS Library gestolpert, die den Zweck überaschend gut und tatsächlich sehr einfach erfüllt.

<aside>
💡 Die Ur-Idee war - das muss ich ergänzend erwähnen - dass man aus einem Skriptum, welches in Markdown oder LaTeX verfasst wird, automatisiert Folien erzeugen kann.

</aside>

### Die Anforderungen an meine Slides waren klar:

- Source Code Snippets müssen sauber darstellbar sein
- Die Slides sollen mit Markdown geschrieben werden können
- Theming muss möglich sein, da ich sowohl an der FH als auch an der Academy unterrichte und überschneidende Unterlagen habe
- Die Unterlagen sollen mittels GIT Repository Versionierbar sein

## Was ist MARP und MARPIT

Bei meiner Recherche stieß ich letzten Endes auf [marp](https://marp.app/) und [marpIt](https://marpit.marp.app/markdown). Marp ist dabei das Markdown Presentation Ecosystem das Markdown entsprechend erweitert und MarpIt ist ein Framework mit welchem man mittels Node-Package-Manager aus dem Markdown gepaart mit CSS im Handumdrehen PDF Slides oder HTML mit Presentation Modus erstellen kann.

### Mit Marp und Marpit arbeiten

Um mit dem Ecosystem zu arbeiten, benötigt man als Basis eine node CLI Installation und ein node Projekt. Im Projektordner befinden sich die Markdown Files in denen die Slides geschrieben werden.

Hier ist ein einfaches Beispiel eines md Files mit 2 Slides:

```markdown
---
marp: true
author: Helmuth Lammer, MSc
version: 1.0.0
size: 4:3
paginate: true
theme: technikum
---

# Slide Headline (Slide 1)
## Slide Subheadline

Inhalt der Slide

---
# Slide Headline (Slide 1)

- Bullet List Item 1
- Bullet List Item 2
- Bullet List Item 3
```

In den Metainformationen am Beginn der Datei aktiviert man unter anderem `marp` und definiert das Seitenverhältnis. Hier kann man auch CSS einbinden. Ich verwende in meinen aktuellen Foliensätzen z.b. das Framework Tailwind.

Man kann dort auch den Header und Footer für die Folien definieren. Mehr Details sind in meinem Fallbeispiel, dass ich am Ende des Artikels verlinke.

Im Dokument selber findet sich bekannte Markdown Syntax. Besonders sind die 3 Bindestriche: mit diesen teilt man mit, dass eine neue Slide beginnt. Wichtig ist eine Leerzeile davor nicht zu vergessen.

***Was müssen wir auf der CLI wissen?***

```bash
$ npm i @marp-team/marp-cli #installiert marp cli in unser Projekt
$ npx marp slides/ --theme themes/technikum.css --allow-local-files --html --pdf
```

Die zweite Zeile bedarf mehr Erklärung und ich möchte die Abschnitte Schrittweise erläutern:

- `npx marp` führt die Verwandlung von Markdown in ein PDF oder eine HTML Datei aus
- `slides/` bezeichnet den Ordner, in welchem die `*.md` Files liegen werden (Ja, man kann hier auch einzelne Files angeben)
- mittels `--theme` teilt man mit, welches CSS zusätzlich geladen werden soll und so zu sagen das Theme darstellt.
- MIttels `--allow-loca-files` ermöglicht man, dass man Bilder, die in einem lokalen Ordner gespeichert sind, ins PDF hinein übertragen kann. Ansonsten muss man URLs angeben die public erreichbar sind.
- `--html` hat zwei funktionen:
    - ein HTML FIle wird erzeugt
    - aber wichtiger (!) ab jetzt ist HTML im Markdown erlaubt und wird auch verstanden (In Kombination mit Tailwind ist dieser Modus wirklich sehr hilfreich)
- `--pdf` teilt `marp` mit, dass eben PDF Dateien erzeugt werden sollen.

Zusätzlich gibt es auch einen “watch” Modus, bei dem es möglich ist an der Datei zu arbeiten und parallel nach Änderungen automatisiert, das PDF oder HTML erzeugt bekommt. Ich persönlich fand es am angenehmsten in diesem Modus mit HTML Files zu arbeiten, da es etwas schneller geht als mit PDFs (man beachte die Option `-w`, im Beispiel wird nur einen Datei beobachtet):

```bash
$ npx marp -w slides/01_kickoff.md --theme themes/technikum.css --allow-local-files --html
```

In meinem Fallbeispiel finden sich weitere Details, wie Kommentare, die nur für den Lektor sichbar sind, eine Idee für ein Technikum und ein Academy Theme und so weiter.

## Vorteile und Nachteile

- Das automatisierte erstellen einheitlicher Slides, die ich in Git Versionieren kann stellt für mich den größsten Vorteil dar. Zusätzlich kann ich jetzt in meinem GitRepo Tickets für Verbesserungsvorschläge erstellen und ggf. auch mit Kolleginnen und Kollegen an einem Slide Deck so kollaborieren, wie ich es aus der Software Entwicklung gewöhnt bin.
- Durch das Theming ist es für mich sehr leicht, bereits verfasste Slides für verschiedene Plattformen und Lehrveranstaltungen zu verwenden und gleichzeitig keinen Fork und damit zwei Versionen pflegen zu müssen.
- Markdown ist - zumindest für mich - eine sehr gängige Sprache und das Erstellen von Slides ohne Point and Click macht das Arbeiten wesentlich schneller.
- Die Lernkurve war für mich wirklich flach und es fühlte sich an, als würde das Ganze “out-of-the-box” funktionieren. Als Neuling im Thema node oder auch dem Arbeiten mit Sourcecode, könnte aber genau das einer der Nachteile sein, die marp hat.
- Ein weiterer Nachteil ist, dass die Darstellung zwischen HTML und PDF etwas variiert. Insbesondere was die Menge an Content auf einer Slide betrifft. So kann es passieren, dass in der HTML Darstellung die Schriftgröße optimiert wird im PDF der Text aber einfach über das Folienende drüberläuft. Man muss also noch einen abschließenden Check machen, ob man ggf. noch Slides auf zwei Seiten aufteilen muss.
- Und der größte Nachteil, es gibt noch kein Theme, welches den aktuellen LV Vorlagen für das FH CI genau entspricht.

## Fazit - Weiter gedacht

Ich habe einige meiner LVs erstmal auf dieses System umgestellt und habe in mein Repository neben einem Ortner für die Folien auch eine Struktur angelegt in der ich alle Aufgaben, Beispiele, Bewertungschemata und Unterlagen mit in die Versionierung aufgenommen habe. Bisher habe ich schon einige Helmuth interne KVP Tickets direkt im Anschluss an LVs erstellt die in einem Kanbanboard auf meinem persönlichen GitLab gesammelt werden.

Und das bringt mich schon zum Gedanken an “Weiteres”. Man kann durch diese Art die Folien zu erstellen im Grunde auch so etwas wie Continuos Delivery und Deployment mit Pipelines organisieren. Man könnte auch, da wir uns jetzt ja in node befinden, ein Skcript programmieren, welches einen kompletten Moodle Import aus den Slides + einem Konfigurations yml oder json erstellt. Man stelle sich vor, man gibt die Anzahl der Studierenden, die Datums der LV Termine als Configuration hinein und bekommt eine fertig geplante LV mit Abschnitten für die Einheiten inklusive Abgaben und Deadlines, Gruppenbildung, usw. heraus.

Und 🙂 auch meine Ur-Idee sollte mithilfe einer erweiterten Markdown Syntax und einem entsprechenden Node-Script umsetzbar sein.

Viel Spass beim Ausprobieren meines Fallbeispiels auf GitHub und Freude an diesem Gedankenexperiment.
