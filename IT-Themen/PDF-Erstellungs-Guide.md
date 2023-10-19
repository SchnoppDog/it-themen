# PDF-Erstellungs-Guide

Dieses Dokument beschreibt wie das Markdown-Dokument `IT-Themenzusammenfassung` als PDF erstellt wird.

- [PDF-Erstellungs-Guide](#pdf-erstellungs-guide)
	- [Hintergrund](#hintergrund)
		- [Markdown All in One](#markdown-all-in-one)
		- [Markdown PDF](#markdown-pdf)
	- [Entscheidung: Markdown AiO oder Mardown PDF](#entscheidung-markdown-aio-oder-mardown-pdf)
	- [Erstellung von Fußnotizen mit Markdown PDF](#erstellung-von-fußnotizen-mit-markdown-pdf)
		- [Verlinkung der Fußnotizen](#verlinkung-der-fußnotizen)
		- [Vorhandenes Konstrukt](#vorhandenes-konstrukt)
	- [Print to PDF](#print-to-pdf)

## Hintergrund

Es gibt zwei Visual Studio Code Plugins, welche dabei helfen sollen, Markdown in PDF zu konvertien:

- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one): 
- [Markdown PDF](https://marketplace.visualstudio.com/items?itemName=yzane.markdown-pdf): 

### Markdown All in One

Markdown All in One unterstützt nativ keine Markdown zu PDF Konvertierung. Es kann jedoch HTML-Dateien erzeugen, welche wiederum als PDF-Dateien gespeichert werden können (Print to PDF).

| Pro | Contra |
| --- | --- |
| Erzeugt aus Markdown-Dateien HTML-Dateien | Kann keine PDFs aus Markdown-Dateien erzeugen |
| Unterstützt das Footnote Markdown-Plugin | Besitzt ein schlechtes CSS für Inline-Code und Codeblocks |

### Markdown PDF

Markdown PDF kann, wie der Name schon zeigt, aus Markdown-Dateien PDF-Dateien erstellen. Zudem besitzt es die Möglichkeit das Markdown-Dokument noch in weiteren Formaten bereitzustellen.

| Pro | Contra |
| --- | --- |
| Unterstützt Print to PDF | Keine Unterstützung für Markdown-Fußnotizen Plugin |
| Besitzt insgesamt ein schönes CSS-Desgin, vor allem für Inline-Code und Codeblocks |  |
| Unterstützt neben PDF noch weitere Formate |  |

## Entscheidung: Markdown AiO oder Mardown PDF

Zwar unterstützt `Markdown All in One` bei der Konvertierung nach HTML das VSCode-Plugin für die Erstellung von Fußnotizen, jedoch besitzt es ein meiner Meinung nach schlechteres CSS-Design, was Codeblocks und Inline-Code angeht. Auf der anderen Seite besitzt `Markdown PDF` zwar keine Unterstützung für das VSCode-Plugin für die Fußnotizen bei der Konvertierung in HTML oder PDF, besitzt jedoch ein deutlich besseres und distinktiveres CSS-Design für Codeblocks und Inline-Code.

Da es für mich jedoch wichtiger ist, dass man Codeblocks gut sichtbar macht und sich Inline-Code distinktiv vom eigentlichen geschriebenen Text visuell unterschieden werden kann, fällt meine Entscheidung, welches Tool ich zum Erstellen der PDF-Dateien benutzen möchte eindeutig auf **Markdown PDF**.

## Erstellung von Fußnotizen mit Markdown PDF

Da ich selbst wenig Ahnung von CSS habe, kann ich leider das CSS-Design nicht anpassen. Zudem würde sich die Zeit und der Aufwand dafür nicht lohnen, das CSS-Design selbst anzupassen.

Um jedoch nicht auf die Fußnotizen verzichten zu müssen, habe ich mir angeschaut wie der HTML-Code von `Markdown All in One` für die Erstellung der Fußnotizen aussieht. Nachfolgende Erklärung soll aufzeigen, wie die Fußnotizen nach der Konvertierung in **HTML** nachbearbeitet werden können.

Markdown-Fußnotizen werden für gewöhnlich wie folgt geschrieben:

```txt
Das hier ist ein toller[^1] Text

[^1]: Identifziert eine Fußnote
```

**Beispiel:**

Das hier ist ein toller[^1] Text

Das Ergebnis sieht nach der Konvertierung in HTML mit Markdown PDF folgendermaßen aus:

```html
<p>[^CPS1]: Ein bestimmter oder unbestimmter Wert, welcher als zusätzlicher Sicherheitsfaktor mit in die Passwortverschlüsselung genommen wird.</p>
<p>[^DSL_1]: Eine Vermittlungsstelle ist ein Knotenpunkt eines Nachrichtennetztes, der die wahlweise Herstellung von Nachrichtenverbindungen ermöglicht.</p>
```

Normalerweise sollten die Paragraph-Elemente als Link deklariert werden und zu den entsprechenden Fußnotizen verlinken. Diese Funktion besitzt Markdown PDF jedoch leider nicht. Um also die Fußnotizen hinzuzufügen ist eigene Handarbeit gefragt!

### Verlinkung der Fußnotizen

Um die Fußnotizen mit einer Verlinkung darstellen zu können, werden `<a href=""></a>` Tags benutzt. Diese werden im `<p></p>` Tag eingebunden. Die Verlinkung zum entsprechenden Textabschnitt erfolgt dabei mittels einer `ID` im `href=""` Konstrukt. Außerdem werden die Fußnotizen nicht mehr als `[^CPS1]` gekennzeichnet, sondern als Zahl wie `[1]`. Diese Zahl wird bei den Fußnotizen durch eine ***ordered List*** `<ol></ol>` dargestellt.

Das Konstrukt für die Fußnotizen sieht also wie folgt aus:

```html
<ol>
    <li>
        <p>Ein bestimmter oder unbestimmter Wert, welcher als zusätzlicher Sicherheitsfaktor mit in die Passwortverschlüsselung genommen wird.<a href="fn1" id="fnref1">↩︎</a></p>
    </li>
    <li>
        <p>Eine Vermittlungsstelle ist ein Knotenpunkt eines Nachrichtennetztes, der die wahlweise Herstellung von Nachrichtenverbindungen ermöglicht.<a href="fn2" id="fnref2">↩︎</a></p>
    </li>
</ol>
```

Im Text muss ebenfalls ein `<a href=""></a>` Tag an der Stelle erstellt werden, wo die Verlinkung zur Fußnotiz stattfinden soll. Als `href=""`-Wert wird hier die `ID` des `<a>`-Tags der Fußnotiz eingetragen. Somit kann man immer zwischen Fußnotiz und Textabschnitt hin und herspringen. Als Identifizierung, dass das Wort im Text eine Fußnotiz besitzt, wird eine einfache Nummerierung in eckigen Klammern `[1]` benutzt. Um diese, wie in vielen Texten bekannt, hochzustellen, wird zusätzlich das HTML-Tag `<sup></sup>` verwendet.

Das Konstrukt im Textabschnitt sieht also wie folgt aus:

```html
<strong>MD5</strong> wird zwar mit einem <strong>zusätzlichem Salt</strong><a href="#fn1" id="fnref1"><sup>[1]</sup></a> versehen, ist dennoch weiterhin ein schnell zu berechenbarer Hash-Algorithmus.

Ein solcher DSLAM ist oftmals in Vermittlungsstellen<a href="#fn2" id="fnref2"><sup>[2]</sup></a>, Gebäuden oder im Außenbereich vorzufinden.
```

### Vorhandenes Konstrukt

Da Markdown PDF die HTML-Datei bei neuer Generierung immer wieder überschreibt, wird hier die entsprechende Konstruktion festgehalten. Diese kann einfach kopiert werden und im HTML-Dokument eingefügt werden.

**Bitte Beachten:** Dieses Konstrukt sollte immer aktuell gehalten werden, sodass ein einfaches Kopieren immer möglich ist und man das Konstrukt nicht mehr selbst schreiben muss. Das kann nämlich zu unschönen Fehlern führen!

**Fußnotizen Ordered List mit Überschrift:**

```html
<h1>Fußnotizen</h1>
<ol>
	<li id="fn1">
		<p>Ein bestimmter oder unbestimmter Wert, welcher als zusätzlicher Sicherheitsfaktor mit in die Passwortverschlüsselung genommen wird.<a href="#fnref1">↩︎</a></p>
	</li>
	<li id="fn2">
		<p>Eine Vermittlungsstelle ist ein Knotenpunkt eines Nachrichtennetztes, der die wahlweise Herstellung von Nachrichtenverbindungen ermöglicht.<a href="#fnref2">↩︎</a></p>
	</li>
	<li id="fn3">
		<p>Zufälliger freier Port in der Port-Range der registrierten Ports. Die Ports 1-1023 sind <strong>standardisierte</strong> und somit nicht dynamisch zuweisbare Ports.<a href="#fnref3">↩︎</a></p>
	</li>
	<li id="fn4">
		<p>Hiermit sind nicht unbedingt Displays als Hardware gemeint. Gemeint können auch virtuelle Desktops sein.<a href="#fnref4">↩︎</a></p>
	</li>
	<li id="fn5">
		<p>Canvas ist bei Computern ein Container, welcher eine Vielzahl an unterschiedlichen Elemente beinhalten kann.<a href="#fnref5">↩︎</a></p>
	</li>
</ol>
```

**Textabschnitte:**

```html
<a href="#fn1" id="fnref1"><sup>[1]</sup></a>
<a href="#fn2" id="fnref2"><sup>[2]</sup></a>
<a href="#fn3" id="fnref3"><sup>[3]</sup></a>
<a href="#fn4" id="fnref4"><sup>[4]</sup></a>
<a href="#fn5" id="fnref5"><sup>[5]</sup></a>
```

## Print to PDF

Zum Abschluss sollte das bearbeitete HTML-Dokument im MS Edge geöffnet werden. Dort kann es mittels `Strg+P` als PDF-Datei gedruckt werden.


[^1]: Identifziert eine Fußnote