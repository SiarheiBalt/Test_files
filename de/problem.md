<table>
<thead>
<tr>
  <th>Regel 1111111111111111111 </th>
  <th>Begründung</th>
</tr>
</thead>
<tbody>
<tr>
<td>Als umschließende Tags müssen <code>&lt;html ⚡4ads&gt;</code> oder <code>&lt;html amp4ads&gt;</code> verwendet werden.</td>
<td>Ermöglicht Validatoren, ein kreatives Dokument entweder als allgemeines AMP-Dokument oder als eingeschränktes AMPHTML-Anzeigendokument zu identifizieren und entsprechend zu versenden.</td>
</tr>
<tr>
<td>Muss <code>&lt;script async src="https://cdn.ampproject.org/amp4ads-v0.js"&gt;&lt;/script&gt;</code> als Laufzeitskript anstelle von <code>https://cdn.ampproject.org/v0.js</code> enthalten.</td>
<td>Ermöglicht maßgeschneidertes Laufzeitverhalten für AMPHTML-Anzeigen, die in ursprungsübergreifenden Iframes bereitgestellt werden.</td>
</tr>
<tr>
<td>Darf kein <code>&lt;link rel="canonical"&gt;</code> -Tag enthalten.</td>
<td>Anzeigenmotive haben keine „nicht-AMP-kanonische Version“ und werden nicht unabhängig suchindiziert, sodass eine Selbstreferenzierung nutzlos wäre.</td>
</tr>
<tr>
<td>Kann optionale Meta-Tags im HTML <code>&lt;meta name="amp4ads-id" content="vendor=${vendor},type=${type},id=${id}"&gt;</code> Diese Meta-Tags müssen vor dem Skript <code>amp4ads-v0.js</code> platziert werden. Der Wert von <code>vendor</code> und <code>id</code> sind Zeichenfolgen, die nur [0-9a-zA-Z_-] enthalten. Der Wert von <code>type</code> ist entweder <code>creative-id</code> oder <code>impression-id</code> .</td>
<td>Diese benutzerdefinierten Kennungen können zur Identifizierung der Impression oder des Motivs verwendet werden. Sie können bei der Berichterstellung und beim Debuggen hilfreich sein.<br><br><p> Beispiel:</p>
<pre>
&lt;meta name="amp4ads-id"
content="vendor=adsense,type=creative-id,id=1283474"&gt;
&lt;meta name="amp4ads-id"
content="vendor=adsense,type=impression-id,id=xIsjdf921S"&gt;</pre>
</td>
</tr>
<tr>
<td>Das Sichtbarkeits-Tracking <code>&lt;amp-analytics&gt;</code> kann nur auf den vollständigen Anzeigenselektor über <code>"visibilitySpec": { "selector": "amp-ad" }</code> abzielen, wie in <a href="https://github.com/ampproject/amphtml/issues/4018">Problem Nr. 4018</a> und <a href="https://github.com/ampproject/amphtml/pull/4368">PR Nr. 4368</a> definiert. Insbesondere darf es nicht auf Selektoren für Elemente innerhalb des Anzeigenmotivs abzielen.</td>
<td>In einigen Fällen entscheiden sich AMPHTML-Anzeigen möglicherweise dafür, ein Anzeigenmotiv in einem Iframe darzustellen. In diesen Fällen kann die Analyse der Hostseite ohnehin nur auf den gesamten Iframe abzielen und hat keinen Zugriff auf differenziertere Selektoren.<br><br><p> Beispiel:</p>
<pre>
&lt;amp-analytics id="nestedAnalytics"&gt;
&lt;script type="application/json"&gt;
{
"Anfragen": {
„visibility“: „https://example.com/nestedAmpAnalytics“
},
"löst aus": {
„visibilitySpec“: {
„selector“: „amp-ad“,
„visiblePercentageMin“: 50,
„continuousTimeMin“: 1000
}
}
}
&lt;/script&gt;
&lt;/amp-analytics&gt;
</pre>
<p>Diese Konfiguration sendet eine Anfrage an die URL <code>https://example.com/nestedAmpAnalytics</code> , wenn 50 % der umschließenden Anzeige eine Sekunde lang ununterbrochen auf dem Bildschirm sichtbar waren.</p> </td>
</tr>
</tbody>

</table>


