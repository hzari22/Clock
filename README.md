# Clock Projekt

Dieses Projekt zeigt eine Uhr mit zwei Anzeigen:

- links eine digitale Uhrzeit
- rechts eine analoge Uhr mit Stunden-, Minuten- und Sekundenzeiger

Die komplette Uhr befindet sich in einer einzigen `index.html` Datei. Darin stehen HTML, CSS und JavaScript zusammen.

## Dateien

### `index.html`

Diese Datei enthält alles, was die Uhr braucht:

- den HTML-Aufbau
- das Design mit CSS
- die Uhr-Logik mit JavaScript

## Aufbau Der Seite

Im `<body>` gibt es zwei Hauptbereiche.

Der erste Bereich ist die digitale Uhranzeige:

```html
<div class="div-time">
  <h1 class="time">00:00:00</h1>
</div>
```

Dieser Bereich steht links auf der Seite. Der Text in `.time` wird später mit JavaScript jede Sekunde aktualisiert.

Der zweite Bereich ist die analoge Uhr:

```html
<div class="clock">
  <div class="clock-face">
    <div class="hand hour-hand"></div>
    <div class="hand min-hand"></div>
    <div class="hand second-hand"></div>
    <div class="circle"></div>
  </div>
</div>
```

In `.clock` ist die runde Uhr. In `.clock-face` liegen die drei Zeiger und der schwarze Punkt in der Mitte.

## Design Mit CSS

Der `body` benutzt Flexbox:

```css
body {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
```

Dadurch stehen die digitale Anzeige links und die analoge Uhr rechts.

Die analoge Uhr wird durch `.clock` rund gemacht:

```css
.clock {
  width: 30rem;
  height: 30rem;
  border: 20px solid white;
  border-radius: 50%;
}
```

`border-radius: 50%` macht aus dem Viereck einen Kreis.

## Mittelpunkt Der Uhr

Der schwarze Punkt in der Mitte ist dieses Element:

```html
<div class="circle"></div>
```

Er wird mit CSS genau in die Mitte gesetzt:

```css
.circle {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

`top: 50%` und `left: 50%` setzen den Punkt ungefähr in die Mitte.  
`transform: translate(-50%, -50%)` verschiebt ihn dann exakt auf seinen eigenen Mittelpunkt.

## Zeiger Der Uhr

Alle Zeiger haben die Klasse `.hand`.

```css
.hand {
  position: absolute;
  top: 50%;
  left: 0;
  transform-origin: 100% 50%;
}
```

`top: 50%` setzt die Zeiger auf die vertikale Mitte der Uhr.  
`transform-origin: 100% 50%` sorgt dafür, dass sich die Zeiger um ihr rechtes Ende drehen. Dieses Ende liegt in der Mitte der Uhr.

Der Stundenzeiger ist kürzer:

```css
.hour-hand {
  width: 35%;
  left: 15%;
}
```

Dadurch reicht er auch bis zur Mitte, ist aber nicht so lang wie Minuten- und Sekundenzeiger.

## JavaScript Logik

Am Anfang werden die wichtigen Elemente aus dem HTML geholt:

```js
const secondHand = document.querySelector(".second-hand");
const minsHand = document.querySelector(".min-hand");
const hourHand = document.querySelector(".hour-hand");
const timeDisplay = document.querySelector(".time");
```

Die Funktion `setDate()` holt die aktuelle Zeit:

```js
const now = new Date();
```

Danach werden Sekunden, Minuten und Stunden in Grad umgerechnet. Diese Gradzahl bestimmt, wie weit sich ein Zeiger drehen muss.

Beispiel für den Sekundenzeiger:

```js
const seconds = now.getSeconds();
const secondsDegrees = (seconds / 60) * 360 + 90;
secondHand.style.transform = `translateY(-50%) rotate(${secondsDegrees}deg)`;
```

Eine ganze Runde hat `360deg`.  
Eine Minute hat `60` Sekunden.  
Darum wird gerechnet:

```js
(seconds / 60) * 360
```

Die `+ 90` sind nötig, weil der Zeiger im CSS am Anfang waagerecht liegt.

## Digitale Uhrzeit

Die digitale Zeit wird so aktualisiert:

```js
timeDisplay.textContent = now.toLocaleTimeString("de-DE", {
  hour: "2-digit",
  minute: "2-digit",
  second: "2-digit",
});
```

Dadurch erscheint die Uhrzeit im deutschen Format, zum Beispiel:

```text
14:35:09
```

## Automatische Aktualisierung

Diese Zeile führt `setDate()` jede Sekunde aus:

```js
setInterval(setDate, 1000);
```

`1000` bedeutet 1000 Millisekunden, also 1 Sekunde.

Am Ende wird `setDate()` einmal direkt gestartet:

```js
setDate();
```

So wird die Uhr sofort angezeigt und nicht erst nach einer Sekunde.

## Responsive Verhalten

Auf kleineren Bildschirmen werden die Elemente untereinander angezeigt:

```css
@media (max-width: 760px) {
  body {
    flex-direction: column;
  }
}
```

Das sorgt dafür, dass die Uhr auch auf kleineren Bildschirmen besser aussieht.

## Projekt Öffnen

Öffne einfach die Datei `index.html` im Browser.

Wenn du die Uhr in deinem eigenen Ordner benutzen möchtest, kannst du den Inhalt der `index.html` kopieren und in deine eigene Datei einfügen.

## Was Du Leicht Anpassen Kannst

Die Größe der analogen Uhr:

```css
.clock {
  width: 30rem;
  height: 30rem;
}
```

Die Farbe des Sekundenzeigers:

```css
.second-hand {
  background: rgb(134, 39, 2);
}
```

Die Größe der digitalen Uhrzeit:

```css
.time {
  font-size: clamp(4rem, 8vw, 9rem);
}
```

Den Abstand zwischen digitaler und analoger Uhr:

```css
body {
  gap: 6rem;
}
```
