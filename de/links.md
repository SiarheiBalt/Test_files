---
title: Links
syntax-id: "Links \nggg"
syntax-summary: "[Linkname](https://www.example.com)"
description: |-
  Um einen Link zu erstellen, schließen Sie den Linktext in Klammern ein (z. B. „[Duck Duck Go]“) und folgen Sie ihm da

  nn direkt in Klammern mit der URL (z. B. „(https://duckduckgo.com)“). Optional können Sie nach der URL in Klammern einen Titel hinzufügen.
examples:
  - markdown: "Meine Lieblingssuchmaschine ist [Duck Duck Go](https://duckduckgo.com „Die beste Suchmaschine für Datenschutz“)."
  html: "Meine Lieblingssuchmaschine ist <a href=\"https://duckduckgo.com\" title=\"Die beste Suchmaschine für Datenschutz\">Duck Duck Go</a>."
additional-examples:
  - name: "URLs und \nE-Mail-Adressen"
  description: Um eine URL oder E-Mail-Adresse schnell in einen Link umzuwandeln, schließen Sie sie in spitze Klammern ein.
  markdown: "<https://www.markdownguide.org><fake@example.com>"
  html: "<a href=\\\"https://www.markdownguide.org\\\">https://www.markdownguide.org</a><a href=\\\"&#x6d;&#97;&#105;& #x6c;&#116;&#x6f;&#58;&#x66;&#x61;&#x6b;&#101;&#64;&#x65;&#120;&#x61;&#x6d ;&#x70;&#108;&#101;&#46;&#99;&#x6f;&#109;\\\">&#x66;&#x61;&#x6b;&#101;&# \n\n\n64;&#x65;&#120;&#x61;&#x6d;&#x70;&#108;&#101;&#46;&#99;&#x6f;&#109;</a>"
  - name: Links formatieren
  description: Um Links hervorzuheben, fügen Sie vor und nach den Klammern und Klammern Sternchen ein.
  markdown: "Ich liebe es, **[EFF](https://eff.org)** zu unterstützen. Dies ist der *[Markdown Guide](https://www.markdownguide.org)*."
  html: "Ich liebe es, <strong><a href=\"https://eff.org\">EFF</a></strong> zu unterstützen. Dies ist der <em><a href=\"https://www.markdownguide.org\">Markdown-Leitfaden</a></em>."
---

Um einen Link zu erstellen, schließen Sie den Linktext in Klammern ein (z. B. `[Duck  Duck Go]` ) und folgen Sie ihm dann direkt in Klammern mit der URL (z. B. `(https://duckduc kgo.com)` ).

```
My favorite search engine is [Duck Duck Go](https://duckduckgo.com).
```

Die gerenderte Ausgabe<br> sieht folgendermaßen aus:

Meine Lieblingssuchmaschine ist [Duck Duck Go](https://duckduckgo.com) .

<div class="alert alert-info">
<i class="fas fa-info-circle"></i><strong>Hinweis:</strong> Informationen zum Verknüpfen mit einem Element auf derselben Seite finden Sie unter <a href="/extended-syntax/#linking-to-heading-ids">Verknüpfen mit Überschriften-IDs</a> . Informationen zum Erstellen eines Links, der in einem neuen Tab oder Fenster geöffnet wird, finden Sie im Abschnitt zu <a href="/hacks/#link-targets">Linkzielen</a> .</div>

### Titel hinzufügen

Sie können optional einen Titel für einen Link hinzufügen. Dies wird als Tooltip angezeigt, wenn der Benutzer mit der Maus über den Link fährt. Um einen Titel hinzuzufügen, schließen Sie ihn nach der URL in Anführungszeichen ein.

```
My favorite search engine is [Duck
Duck Go](https://duckduckgo.com "The best search engine for privacy").
```

Die gerenderte Ausgabe sieht folgendermaßen aus:

Meine Lieblingssuchmaschine ist [Duck Duck Go](https://duckduckgo.com "Die beste Suchmaschine für Privatsphäre") .

### URLs und E-Mail-Adressen

Um eine URL oder E-Mail-Adresse schnell in einen Link umzuwandeln, schließen Sie sie in spitze Klammern ein.

```
<https://www.markdownguide.org>
<fake@example.com>
```

Die gerenderte Ausgabe sieht folgendermaßen aus:

[https://www.markdownguide.org](https://www.markdownguide.org)<br> [fake@example.com](mailto:fake@example.com)

### Links formatieren

Sie vor und nach den Klammern und Klammern Sternchen ein. Um Links als [Code](#code) zu kennzeichnen, fügen Sie Backticks in die Klammern ein.

```
I love supporting the **[EFF](https://eff.org)**.
This is the *[Markdown Guide](https://www.markdownguide.org)*.
See the section on [`code`](#code).
```

Die gerenderte Ausgabe sieht folgendermaßen aus:

Ich liebe es, die **[EFF](https://eff.org)** zu unterstützen.<br> Dies ist der *[Markdown-Leitfaden](https://www.markdownguide.org)* .<br> Siehe den Abschnitt über [`co  de`](#code) .

### Links im Referenzstil

Links im Referenzstil sind eine besondere Art von Links, die das Anzeigen und Lesen von URLs in Markdown erleichtern. Links im Referenzstil bestehen aus zwei Teilen: dem Teil, den Sie in Ihren Text einfügen, und dem Teil, den Sie an einer anderen Stelle in der Datei speichern, damit der Text leicht lesbar bleibt.

#### Formatieren des ersten Teils des Links

Der erste Teil eines Links im Referenzstil wird mit zwei Klammersätzen formatiert. Der erste Satz Klammern umgibt den Text, der verknüpft erscheinen soll. Der zweite Klammersatz zeigt eine Beschriftung an, die auf den Link verweist, den Sie an anderer Stelle in Ihrem Dokument speichern.

Obwohl dies nicht erforderlich ist, können Sie zwischen dem ersten und zweiten Satz Klammern ein Leerzeichen einfügen. Bei der Beschriftung im zweiten Klammersatz wird die Groß-/Kleinschreibung nicht beachtet und sie kann Buchstaben, Zahlen, Leerzeichen oder Satzzeichen enthalten.

Dies bedeutet, dass die folgenden Beispielformate<br> für den ersten Teil des Links ungefähr gleichwertig sind:

- `[hobbit-hole][1]`
- `[hobbit-hole] [1]`

#### Formatieren des zweiten Teils des Links

Der zweite Teil eines Links im Referenzstil ist mit den folgenden Attributen formatiert:

1. Die Bezeichnung in Klammern, direkt gefolgt von einem Doppelpunkt und mindestens einem Leerzeichen (z. B. `[label]:` ).
2. Die URL für den Link, die Sie optional in spitze Klammern einschließen können.
3. Der optionale Titel für den Link, den Sie in doppelte Anführungszeichen, einfache Anführungszeichen oder Klammern setzen können.

Dies bedeutet, dass die folgenden Beispielformate für den zweiten Teil des Links alle ungefähr gleichwertig sind:

- `[1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle`
- `[1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"`
- `[1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle 'Hobbit lifestyles'`
- `[1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle (Hobbit lifestyles)`
- `[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> "Hobbit lifestyles"`
- `[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> 'Hobbit lifestyles'`
- `[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> (Hobbit lifestyles)`

Sie können diesen zweiten Teil des Links an einer beliebigen Stelle in Ihrem Markdown-Dokument platzieren. Manche Leute platzieren sie direkt nach dem Absatz, in dem sie erscheinen, während andere sie am Ende des Dokuments platzieren (wie Endnoten oder Fußnoten).

#### Ein Beispiel für den Zusammenbau der Teile

Angenommen, Sie fügen eine URL als [Standard-URL-Link](#links) zu einem Absatz hinzu und es sieht in Markdown so aus:

```
In a hole in the ground there lived a hobbit. Not a nasty, dirty, wet hole, filled with the ends
of worms and an oozy smell, nor yet a dry, bare, sandy hole with nothing in it to sit down on or to
eat: it was a [hobbit-hole](https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"), and that means comfort.
```

Obwohl sie möglicherweise auf interessante zusätzliche Informationen hinweist, trägt die angezeigte URL nicht viel zum vorhandenen Rohtext bei, außer dass sie ihn schwerer lesbar macht. Um das zu beheben, können Sie die URL stattdessen wie folgt formatieren:

```
In a hole in the ground there lived a hobbit. Not a nasty, dirty, wet hole, filled with the ends


of worms and an oozy smell, nor yet a dry, bare, sandy hole with nothing in it to sit down on or to
eat: it was a [hobbit-hole][1], and that means comfort.

[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> "Hobbit lifestyles"
```

In beiden oben genannten Fällen wäre die gerenderte Ausgabe identisch:

> In einem Loch im Boden lebte ein Hobbit. Kein hässliches, schmutziges, nasses Loch, gefüllt mit Wurmenden und einem klebrigen Geruch, noch ein trockenes, kahles, sandiges Loch, in dem es nichts gab, worauf man sich setzen oder essen konnte: Es war ein <a href="https://en.wikipedia.org/wiki/Hobbit#Lifestyle" title="Lebensstile der Hobbits">Hobbit-Loch</a> , und das bedeutet Trost.

und der HTML-Code für den Link wäre:

```
<a href="https://en.wikipedia.org/wiki/Hobbit#Lifestyle" title="Hobbit lifestyles">hobbit-hole</a>
```

### Verknüpfen Sie Best Practices

Markdown-Anwendungen sind sich nicht einig darüber, wie mit Leerzeichen in der Mitte einer URL umgegangen werden soll. Versuchen Sie aus Kompatibilitätsgründen, alle Leerzeichen mit `%20` per URL zu kodieren. Wenn Ihre Markdown-Anwendung [HTML unterstützt](#html) , können Sie alternativ `a` HTML-Tag verwenden.

<table class="table table-bordered">
  <thead class="thead-light">
    <tr>
      <th>✅ Tun Sie dies</th>
      <th>❌Tu das nicht</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <code class="highlighter-rouge">
        [link](https://www.example.com/my%20great%20page)&lt;br&gt;&lt;br&gt;


        &lt;a href="https://www.example.com/my great page"&gt;link&lt;/a&gt;&lt;br&gt;&lt;br&gt;
        </code>
      </td>
      <td>
        <code class="highlighter-rouge">
        [link](https://www.example

.com/my great page)&lt;br&gt;&lt;br&gt;
        </code>
      </td>
    </tr>
  </tbody>
</table>

Auch Klammern in der Mitte einer URL können problematisch sein. Versuchen Sie aus Kompatibilitätsgründen, die öffnende Klammer ( `(` ) mit `%28` und die schließende Klammer ( `)` ) mit `%29` per URL zu kodieren. Wenn Ihre Markdown-Anwendung [HTML unterstützt](#html) , können Sie alternativ `a` HTML-Tag verwenden.

<table class="table table-bordered">
  <thead class="thead-light">
    <tr>
      <th>✅ Tun Sie dies</th>
      <th>❌Tu das nicht</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <code class="highlighter-rouge">
        [a novel](https://en.wikipedia.org/wiki/The_Milagro_Beanfield_War_%28novel%29)&lt;br&gt;&lt;br&gt;
        &lt;a href="https://en.wikipedia.org/wiki/The_Milagro_Beanfield_War_(novel)"&gt;a novel&lt;/a&gt;&lt;br&gt;&lt;br&gt;
        </code>
      </td>
      <td>
        <code class="highlighter-rouge">
        [a novel](https://en.wikipedia.org/wiki/The_Milagro_Beanfield_War_(novel))
        </code>
      </td>
    </tr>
  </tbody>
</table>


