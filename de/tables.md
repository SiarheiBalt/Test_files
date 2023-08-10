## Tische

Tabellen werden mit Standard-Markup unterstützt. Hier ist eine typische Tabelle mit einer Überschriftenzeile, mehreren regulären Zeilen und einer als `<tr class="alt">` markierten Zeile, die einen dunkleren Hintergrund erzeugt, der als alternative Überschrift verwendet werden kann.

Eins | Zwei | Drei
--- | --- | ---
1,0 | 2.0 | 3,0
1.1 | 2.1 | 3.1
Hier kommen einige Zahlen, die auf .2 enden! |  |
1.2 | 2.2 | 3.2

```
<table>
  <tr><th>One</th><th>Two</th><th>Three</th></tr>
  <tr><td>1.0</td><td>2.0</td><td>3.0</td></tr>
  <tr><td>1.1</td><td>2.1</td><td>3.1</td></tr>
  <tr class="alt"><td colspan="3">Here come some numbers that end in .2!</td></tr>
  <tr><td>1.2</td><td>2.2</td><td>3.2</td></tr>
</table>
```

### Responsive Tabellen

Um eine Tabelle responsiv zu machen, fügen Sie der Tabelle die `responsive` Klasse hinzu.

Parameter |
--- | ---
`value` | `String` den Wert der Auswahl ein, den die Befragten beim Anzeigen des Formulars als Beschriftung sehen
`navigationType` | `PageNavigationType` ist der Navigationstyp der Auswahl

- Die Tabelle darf ***nur zwei Spalten*** enthalten: die erste Spalte für die zu definierenden Dinge (den Schlüssel) und die zweite Spalte für alle Informationen zu diesem Schlüssel, bei Bedarf in mehreren Zeilen. Diese Zwei-Spalten-Einschränkung bedeutet, dass responsive Tabellen nicht für wirklich zweidimensionale tabellarische Daten und einen auf Häkchen basierenden Merkmalsvergleich verwendet werden können, sie eignen sich jedoch gut für Referenzinformationen (oder alle anderen Daten, die vernünftigerweise stattdessen durch eine Definitionsliste ausgedrückt werden könnten). ein Tisch).
- Wenn es mehrere Zeilen mit Informationen zum Schlüssel gibt – beispielsweise einen Typ und eine Beschreibung –, brechen Sie jede Zeile in `<p>` ein, um Zeilenumbrüche zu erzwingen (anstelle von `<br>` ).
- Die Kopfzeile darf nur eine Zelle enthalten. Verwenden Sie `<th colspan="2">` um zu erzwingen, dass es sich über beide Spalten erstreckt. Um Sie an dieses Verhalten zu erinnern, verbergen wir automatisch jedes `<th>` nach dem ersten (was absichtlich sehr kaputt aussieht).

<!---->

```
<table class="responsive">
  <tbody>
    <tr>
      <th colspan=2>Parameters</th>
    </tr>
    <tr>
      <td>
        <code>value</code>
      </td>
      <td>
        <code>String</code><br>
        the choice's value, which respondents see as a label when viewing the form
      </td>
    </tr>
    <tr>
      <td>
        <code>navigationType</code>
      </td>
      <td>
        <code>
              <a href="#">PageNavigationType</a>
            </code>
        <br>the choice's navigation type
      </td>
    </tr>
  </tbody>
</table>
```

### Unsichtbare Tische

Mit `<table class="columns">...</table>` können Sie Text in Spalten anordnen oder eine Tabelle auf andere Weise unsichtbar machen. Dies wird typischerweise zum Anordnen langer, schmaler Listen verwendet.

|                              |                                   |                                 |
|------------------------------|-----------------------------------|---------------------------------|
| `auto` `break` `case` `char` | `const` `continue` `default` `do` | `double` `else` `enum` `extern` |

```
<table class="columns">
  <tr>
    <td>
      <code>auto</code><br />
      <code>break</code><br />
      <code>case</code><br />
      <code>char</code>
    </td>
    <td>
      <code>const</code><br />
      <code>continue</code><br />
      <code>default</code><br />
      <code>do</code>
    </td>
    <td>
      <code>double</code><br />
      <code>else</code><br />
      <code>enum</code><br />
      <code>extern</code>
    </td>
  </tr>
</table>
```
