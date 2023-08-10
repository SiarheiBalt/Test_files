---
"$title": Erstellen des Deckblatts
"$order": '4'
description: 'Um eine Seite zu erstellen, fügen Sie das Element <amp-story-page> als untergeordnetes Element von amp-story hinzu. Weisen Sie der Seite eine eindeutige ID zu. Weisen wir unserer ersten Seite, dem Deckblatt, eine eindeutige Deckblatt-ID zu: ...'
author: bpaduch
---

Eine Seite innerhalb einer AMP-Story wird durch die Komponente `<amp-story-page>` dargestellt. Innerhalb einer [`amp-story`](../../../../documentation/components/reference/amp-story.md) können Sie eine oder mehrere `<amp-story-page>` -Komponenten haben, die jeden einzelnen Bildschirm einer Story enthalten. Die erste Seite, die Sie in der Dokumentreihenfolge angeben, ist die erste Seite, die in der Story angezeigt wird.

 `amp-story-pag <em>ext </em>`text

```html
<amp-story standalone
    title="Joy of Pets"
    publisher="AMP tutorials"
    publisher-logo-src="assets/AMP-Brand-White-Icon.svg"
    poster-portrait-src="assets/cover.jpg">
   <amp-story-page id="cover">
   </amp-story-page>
</amp-story>
```

Jetzt haben wir die Hülle für unser Deckblatt. Allerdings ist unsere Geschichte immer noch nicht gültig. Auf unserer Seite müssen wir mindestens eine **Ebene** angeben. {{ image('/static/img/docs/tutorials/amp_story/cover_layers.png', 416, 679, alt='Deckblatt hat zwei Ebenen', align='rechtes Drittel' ) }}

## Ebenen auf einer Seite

Wie Ebenen in Grafiken können Sie Ebenen in AMP-Story-Seiten verwenden, um visuelle Effekte zu erzeugen. Schichten werden übereinander gestapelt, sodass die erste Schicht die unterste Schicht ist und die nächste Schicht darüber liegt und so weiter.

Unser Deckblatt besteht eigentlich aus zwei Ebenen:

- **Ebene 1** : Ein Bild, das als Hintergrund dient
- **Ebene 2** : Der Titel und die Byline der Geschichte

### Ebene 1 erstellen

Fügen wir unsere erste Ebene zu unserem Deckblatt hinzu. Die Ebene enthält ein Bild, das den Bildschirm ausfüllt.

Erstellen Sie die Ebene, indem Sie das Element `<amp-story-grid-layer>` als untergeordnetes Element von `<amp-story-page>` hinzufügen. Da wir möchten, dass das Bild den Bildschirm ausfüllt, geben Sie das Attribut `template="fill"` für den `amp-story-grid-layer` an. Fügen Sie innerhalb der Ebene ein [`amp-img`](../../../../documentation/components/reference/amp-img.md) Element für die Datei `cover.jpg` hinzu und stellen Sie sicher, dass es responsiv ist (z. B. `layout="responsive"` ) und die Bildgröße 720 x 1280 Pixel beträgt. So sieht unsere Ebene aus:

```html
<amp-story-page id="cover">
  <amp-story-grid-layer template="fill">
    <amp-img src="assets/cover.jpg"
        width="720" height="1280"
        layout="responsive">
    </amp-img>
  </amp-story-grid-layer>
</amp-story-page>
```

Mal sehen, wie die Seite angezeigt wird. Öffnen Sie die Seite in Ihrem Browser: <a href="http://localhost:8000/pets.html">http://localhost:8000/pets.html</a> .

So sollte es aussehen:

{{ image('/static/img/docs/tutorials/amp_story/pg0_layer1.jpg', 720, 1280, align='centerthird' ) }}

### Ebene 2 erstellen

Wir haben also unseren Hintergrund, aber jetzt brauchen wir die zweite Ebene, die sich über dem Hintergrund befindet und unsere Überschrift und Verfasserzeile enthält. Um unsere zweite Ebene hinzuzufügen, führen wir die gleichen Aufgaben aus, die wir für Ebene 1 ausgeführt haben, verwenden jedoch anstelle der `fill` die **`vertical`** Vorlage. Bevor wir jedoch weitermachen, lernen wir etwas über Vorlagen und wie wir AMP- und HTML-Elemente in einem `<amp-story-grid-layer>` anordnen können.

#### Elemente mit einer Vorlage anordnen

Das `<amp-story-grid-layer>` -Element ordnet seine untergeordneten Elemente in einem Raster an (basierend auf dem [CSS-Grid](https://www.w3.org/TR/css-grid-1/) ). Um anzugeben, wie die untergeordneten Elemente angeordnet werden sollen, müssen Sie eine der folgenden Layoutvorlagen angeben:

<table class="noborder">
<tr>
    <td colspan="2"><h5 id="fill">Vorlage: Füllen</h5></td>
</tr>
<tr>
    <td width="65%">Die <strong>Füllvorlage</strong> füllt den Bildschirm mit dem ersten untergeordneten Element in der Ebene. Alle anderen untergeordneten Elemente in dieser Ebene werden nicht angezeigt. Die Füllvorlage eignet sich gut für Hintergründe, einschließlich Bilder und Videos. <code class="nopad">&lt;pre&gt;PRE_BLOCK_0&lt;/pre&gt;</code>     </td>
    <td>     {{ image('/static/img/docs/tutorials/amp_story/layer-fill.png', 216, 341) }}     </td>
</tr>
<tr>
    <td colspan="2"><h5 id="vertical">Vorlage: Vertikal</h5></td>
</tr>
<tr>
    <td width="65%">Die <strong>vertikale</strong> Vorlage ordnet die untergeordneten Elemente entlang der y-Achse an. Die Elemente werden am oberen Bildschirmrand ausgerichtet und nehmen entlang der x-Achse den gesamten Bildschirm ein. Die vertikale Vorlage funktioniert gut, wenn Sie Elemente direkt hintereinander vertikal stapeln möchten. <code class="nopad">&lt;pre&gt;PRE_BLOCK_1&lt;/pre&gt;</code>     </td>
    <td>{{ image('/static/img/docs/tutorials/amp_story/layer-vertical.png', 216, 341) }}     </td>
</tr>
<tr>
    <td colspan="2"><h5 id="horizontal">Vorlage: Horizontal</h5></td>
</tr>
<tr>
    <td width="65%">Die <strong>horizontale</strong> Vorlage ordnet die untergeordneten Elemente entlang der x-Achse an. Die Elemente werden am Anfang des Bildschirms ausgerichtet und nehmen entlang der y-Achse den gesamten Bildschirm ein. Die horizontale Vorlage funktioniert gut, wenn Sie Elemente direkt hintereinander horizontal stapeln möchten. <code class="nopad">&lt;pre&gt;PRE_BLOCK_2&lt;/pre&gt;</code>     </td>
    <td>     {{ image('/static/img/docs/tutorials/amp_story/layer-horizontal.png', 216, 341) }}     </td>
</tr>
<tr>
    <td colspan="2"><h5 id="thirds">Vorlage: Terzen</h5></td>
</tr>
<tr>
<td width="65%"> Die <strong>Thirds-</strong> Vorlage unterteilt den Bildschirm in drei gleich große Reihen und ermöglicht es Ihnen, Inhalte in jeden Bereich einzufügen. Sie können auch einen benannten <code>grid-area</code> angeben, um anzugeben, in welchem ​​Drittel sich Ihr Inhalt befinden soll – im <code>upper-third</code> , <code>middle-third</code> oder <code>lower-third</code> . Benannte Rasterbereiche sind nützlich, um das Standardverhalten der Anzeige von Elementen zu ändern. Wenn Sie beispielsweise zwei Elemente in der Ebene haben, können Sie angeben, dass sich das erste Element in <code>grid-area="upper-third"</code> und das zweite Element in <code>grid-area="lower-third"</code> befinden soll. <code class="nopad">&lt;pre&gt;PRE_BLOCK_3&lt;/pre&gt;</code> </td>
<td>{{ image('/static/img/docs/tutorials/amp_story/layer-thirds.png', 216, 341) }}</td>
</tr>
</table>

### Vervollständigung unserer Titelseite

Nachdem Sie nun die Ebenenvorlagen verstanden haben, vervollständigen wir unsere zweite Ebene für das Deckblatt.

Für Ebene 2 möchten wir, dass die Überschrift und die Verfasserzeile oben stehen und die Elemente nacheinander folgen, also geben wir die `vertical` Vorlage an. Unsere zweite `amp-story-grid-layer` folgt der ersten, etwa so:

```html
<amp-story-grid-layer>
 <!--our first layer -->
</amp-story-grid-layer>
<amp-story-grid-layer template="vertical">
  <h1>The Joy of Pets</h1>
  <p>By AMP Tutorials</p>
</amp-story-grid-layer>
```

Aktualisieren Sie Ihren Browser und überprüfen Sie Ihre Arbeit. Unser Deckblatt ist fertig.

{{ image('/static/img/docs/tutorials/amp_story/pg0_cover.png', 720, 1280, align='centerthird', alt='Completed cover page' ) }}
