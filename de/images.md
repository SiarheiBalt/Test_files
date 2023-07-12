## Bilder

<figure>` blocks, and ideally, use
responsive images with the `scrset` attribute when possible. Be sure you
include `alt` attributes for your images as well.

![sample image](https://placehold.it/350x150)

This caption should be used to describe the image.

For example:

    <figure>
      <img src="https://placehold.it/350x150" alt="sample image">
      <figcaption>This caption should be used to describe the image.</figcaption>
    </figure>

Hinweis: Optimierte Bilder werden automatisch nur für `index.yaml` Seiten bereitgestellt. Stellen Sie einfach eine 2x-Version bereit, und der Server erledigt den Rest.

Sie können `class="screenshot"` auf ein Bild anwenden, um ihm einen Rand zu geben, der es vom Text in der Nähe abhebt. Dies wird normalerweise für Screenshots verwendet, die einen weißen Hintergrund haben und sonst auf der Seite verloren gehen. Verwenden Sie es nicht für Bilder, die es nicht benötigen.

### Floating images on the right

![Alarmdialog](https://placehold.it/350x150)

**Abbildung 1** : Alarmdialog

Das Bild rechts hat auch `class="attempt-right"` , was das Bild auf größeren Bildschirmen nach rechts verschiebt, auf kleineren Bildschirmen, Tablets und kleineren Bildschirmen jedoch ein vertikales Layout erzwingt, wo das Nach-rechts-Schieben Probleme verursachen würde. Ebenfalls verfügbar ist `class="attempt-left"` . Um `attempt-left` und `attempt-right` zusammen zu verwenden, stellen Sie sicher, dass das Element `attempt-left` an erster Stelle steht.

```
<div class="attempt-right">
  <figure>
        <img src="https://placehold.it/350x150" alt="Alert dialog">
        <figcaption><b>Figure 1</b>: Alert dialog</figcaption>
      </figure>
</div>
```

Achtung: Bei der Verwendung von `attempt-left` und `attempt-right` kann es erforderlich sein, einen `<div class="clearfix"></div>` -Block einzuschließen.

### Do- und Do-Not-Bilder

Fügen Sie der Bildunterschrift den `success` oder `warning` hinzu, um auf ein gutes oder schlechtes Beispiel hinzuweisen.

![Alarmdialog](https://placehold.it/350x150)

**TUN** : Das ist das Richtige

![Alarmdialog](https://placehold.it/350x150)

**NICHT** : Das ist falsch

```
<div class="attempt-left">
  <figure>
        <img src="https://placehold.it/350x150" alt="Alert dialog">
        <figcaption class="success">
          <b>DO</b>: This is the right thing to do
         </figcaption>
      </figure>
</div>
<div class="attempt-right">
  <figure>
        <img src="https://placehold.it/350x150" alt="Alert dialog">
        <figcaption class="warning">
          <b>DON'T</b>: This is the wrong thing to do
         </figcaption>
      </figure>
</div>
```
