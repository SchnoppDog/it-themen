# PDF-Erstellungs-Guide

Dieses Dokument beschreibt wie das Markdown-Dokument `IT-Themenzusammenfassung` als PDF erstellt wird.

- [PDF-Erstellungs-Guide](#pdf-erstellungs-guide)
	- [Hintergrund](#hintergrund)
		- [Markdown All in One](#markdown-all-in-one)
		- [Markdown PDF](#markdown-pdf)
	- [Entscheidung: Markdown AiO oder Mardown PDF](#entscheidung-markdown-aio-oder-mardown-pdf)
	- [HTML Export mit custom Stylesheet](#html-export-mit-custom-stylesheet)
	- [Informationen zum CSS-Template](#informationen-zum-css-template)

## Hintergrund

Es gibt zwei Visual Studio Code Plugins, welche dabei helfen sollen, Markdown in PDF zu konvertien:

- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one): 
- [Markdown PDF](https://marketplace.visualstudio.com/items?itemName=yzane.markdown-pdf): 

### Markdown All in One

Markdown All in One unterstützt nativ keine Markdown zu PDF Konvertierung. Es kann jedoch HTML-Dateien erzeugen, welche wiederum als PDF-Dateien gespeichert werden können (Print to PDF).

| Pro                                       | Contra                                                    |
| ----------------------------------------- | --------------------------------------------------------- |
| Erzeugt aus Markdown-Dateien HTML-Dateien | Kann keine PDFs aus Markdown-Dateien erzeugen             |
| Unterstützt das Footnote Markdown-Plugin  | Besitzt ein schlechtes CSS für Inline-Code und Codeblocks |
| Unterstützt Mermaid-Diagramme             |                                                           |
| Unterstützt mathematische Ausdrücke       |                                                           |

### Markdown PDF

Markdown PDF kann, wie der Name schon zeigt, aus Markdown-Dateien PDF-Dateien erstellen. Zudem besitzt es die Möglichkeit das Markdown-Dokument noch in weiteren Formaten bereitzustellen.

| Pro                                                                                | Contra                                             |
| ---------------------------------------------------------------------------------- | -------------------------------------------------- |
| Unterstützt Print to PDF                                                           | Keine Unterstützung für Markdown-Fußnotizen Plugin |
| Besitzt insgesamt ein schönes CSS-Desgin, vor allem für Inline-Code und Codeblocks | Keine Unterstützung für mathematische Ausdrücke    |
| Unterstützt neben PDF noch weitere Formate                                         |                                                    |

## Entscheidung: Markdown AiO oder Mardown PDF

**Gewinner:** Markdown All in One

Aufgrunddessen, dass vermehrt Fußnotizen und mathematische Ausdrücke in der IT-Themenzusammenfassung auftreten bzw. auftreten können und eine manuelle Erstellung der Fußnotizen ein großer Zeitfresser ist, soll **Markdown All in One** für die Erstellung der PDF-Dateien verwendet werden.

Zwar besitzt `Markdown All in One` ein im Vergleich zu `Markdown PDF` schlechteres CSS-Design für die Aufbereitung der HTML-Seite, jedoch wird dies durch ein entsprechendes CSS-Template ersetzt. Somit wird ein eigenes CSS-Design verwendet, welches zudem nach Belieben geändert werden kann.

## HTML Export mit custom Stylesheet

Um das Dokument nach HTML und anschließend nach PDF zu konvertieren, wird die Funktion `Print current document to HTML` von Markdown All in One benutzt, welche unter drücken der F1-Taste gefunden werden kann. Nach dem Export in HTML wird das CSS-Design entsprechend angepasst.

Dabei wird **fast alles** im `<style>`-Tag, bis auf den KaTeX-Inhalt, welcher für das Anzeigen der mathematischen Ausdrücke wichtig ist, gelöscht und durch folgendes ersetzt:

```html
<link rel="stylesheet" href="css/github-markdown.css">
```

Zusätzlich wird wird im `<body>`-Block die CSS-Klasse `vscode-body` zu `markdown-body` geändert:

```html
<body class="markdown-body vscode-light">
	<span id="markdown-mermaid" aria-hidden="true"
			data-dark-mode-theme="dark"
			data-light-mode-theme="default"></span>
```

Der Rest bleibt so wie er ist.

Anschließend kann das HTML-Dokument in einem entsprechenden Browser geöffnet werden. Hier kann mittels dem Shortcut `Strg+P` die HTML-Seite zu einer PDF-Seite konvertiert / gespeichert werden.

> **Wichtig:**
>
> Aufgrund von unbekannten Problemen funktionieren interne Dokumenten-Links wie Überschriften-Verweise o.ä. im PDF-Format nicht! Zudem wird das CSS-Design nicht richtig gerendert! Deshalb ist es empfohlen die HTML-Seite zur Betrachtung zu verwenden, da hier alle Funktionen unterstützt sind!

## Informationen zum CSS-Template

Das CSS-Template wurde **nicht von mir erstellt**, sondern von [sindresorhus/github-markdown-css](https://github.com/sindresorhus/github-markdown-css/tree/main) entnommen. Dabei wurde **kein NPM** zur Installation verwendet, sondern lediglich die CSS-Datei kopiert und in der HTML-Datei eingefügt.

Zusätzlich zur Datei `github-markdown.css` wurde der Viewport wie im Readme.md des Repositories beschrieben entsprechend hinzugefügt:

```css
.markdown-body {
	box-sizing: border-box;
	min-width: 200px;
	max-width: 980px;
	margin: 0 auto;
	padding: 45px;
}

@media (max-width: 767px) {
	.markdown-body {
		padding: 15px;
	}
}
```