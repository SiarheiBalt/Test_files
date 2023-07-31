<table>
<thead>
<tr>
  <th>Rule

1111111111111111111

</th>
  <th>Rationale</th>
</tr>
</thead>
<tbody>
<tr>
<td>Must use <code>&lt;html ⚡4ads></code> or <code>&lt;html amp4ads></code> as its enclosing tags.</td>
<td>Allows validators to identify a creative document as either a general AMP doc or a restricted AMPHTML ad doc and to dispatch appropriately.</td>
</tr>
<tr>
<td>Must include <code>&lt;script async src="https://cdn.ampproject.org/amp4ads-v0.js">&lt;/script></code> as the runtime script instead of <code>https://cdn.ampproject.org/v0.js</code>.</td>
<td>Allows tailored runtime behaviors for AMPHTML ads served in cross-origin iframes.</td>
</tr>
<tr>
<td>Must not include a <code>&lt;link rel="canonical"></code> tag.</td>
<td>Ad creatives don't have a "non-AMP canonical version" and won't be independently search-indexed, so self-referencing would be useless.</td>
</tr>
<tr>
<td>Can include optional meta tags in HTML head as identifiers, in the format of <code>&lt;meta name="amp4ads-id" content="vendor=${vendor},type=${type},id=${id}"></code>. Those meta tags must be placed before the <code>amp4ads-v0.js</code> script. The value of <code>vendor</code> and <code>id</code> are strings containing only [0-9a-zA-Z_-]. The value of <code>type</code> is either <code>creative-id</code> or <code>impression-id</code>.</td>
<td>Those custom identifiers can be used to identify the impression or the creative. They can be helpful for reporting and debugging.<br><br><p>Example:</p><pre>
&lt;meta name="amp4ads-id"
  content="vendor=adsense,type=creative-id,id=1283474">
&lt;meta name="amp4ads-id"
  content="vendor=adsense,type=impression-id,id=xIsjdf921S"></pre></td>
</tr>
<tr>
<td><code>&lt;amp-analytics></code> viewability tracking may only target the full-ad selector, via  <code>"visibilitySpec": { "selector": "amp-ad" }</code> as defined in <a href="https://github.com/ampproject/amphtml/issues/4018">Issue #4018</a> and <a href="https://github.com/ampproject/amphtml/pull/4368">PR #4368</a>. In particular, it may not target any selectors for elements within the ad creative.</td>
<td>In some cases, AMPHTML ads may choose to render an ad creative in an iframe.In those cases, host page analytics can only target the entire iframe anyway, and won’t have access to any finer-grained selectors.<br><br>
<p>Example:</p>
<pre>
&lt;amp-analytics id="nestedAnalytics">
  &lt;script type="application/json">
  {
    "requests": {
      "visibility": "https://example.com/nestedAmpAnalytics"
    },
    "triggers": {
      "visibilitySpec": {
      "selector": "amp-ad",
      "visiblePercentageMin": 50,
      "continuousTimeMin": 1000
      }
    }
  }
  &lt;/script>
&lt;/amp-analytics>
</pre>
<p>This configuration sends a request to the <code>https://example.com/nestedAmpAnalytics</code> URL when 50% of the enclosing ad has been continuously visible on the screen for 1 second.</p>
</td>
</tr>
</tbody>
