# svelte-component-templates
DER SPIEGEL templates for Svelte components


## Utility components


### [TextAlign.svelte](./TextAlign.svelte)

Align child elements to the type area of spiegel.de.

*Properties*
```{JavaScript}
// type of the app: iframe or embed
export let type = 'iframe';

// disable component (if true, no alignment takes place)
export let disable = false;
```


### [SizeDetector.svelte](./SizeDetector.svelte)

Determine the size of an element without using Svelte iFrames and `bind:clientWidth` etc.

Good for apps that will be embedded directly.

*Properties*
```{JavaScript}
// width and height can be bound from outside
export let width = undefined;
export let height = undefined;
```

*Usage*
```{JavaScript}
<SizeDetector
    bind:width={width}
    bind:height={height}
>
    <div>stuff to be measured</div>
</SizeDetector>
```


## Animation and transition components


### [NumberSwitcher.svelte](./NumberSwitcher.svelte)

Animate the transition between two numbers (or characters).

Example: [Silvester countdown](https://interactive.spiegel.de/int/pub/ressort/hp/2021/silvester-countdown/v0/widget.14.html).

*Properties*
```{JavaScript}
// the number (character) itself
export let number;
// y-offset for the transition (in px)
export let offset = 50;
// transition duration (in ms)
export let duration = 600;
// delay (in ms)
export let delay = 0;
// number(character) width (in px), needed for non-shifting layout
export let width = 32;
// deactivate transition
export let deactivate = false;
```


## Simple HTML components 


### [Button.svelte](./Button.svelte)

A simple "DER SPIEGEL"-Button with event dispatcher

Example: [75 Jahre DER SPIEGEL](https://www.spiegel.de/geschichte/75-jahre-spiegel-welche-ausgabe-lag-bei-ihrer-geburt-am-kiosk-a-ffa0d302-19a9-487a-85b5-1ab42be4a45e)

*Properties*
```{JavaScript}
// the text shown on the Button
export let text;
// dispatch the click event
on:buttonClicked
```

### [CardLayout.svelte](./CardLayout.svelte)

Erzeugt einen Schatten um einen Inhalt, sodass dieser wie auf einer Karte präsentiert wird 

Example: [Wahlergebnis der Präsidentschartswahl in Frankreich 2022](https://interactive.spiegel.de/int/pub/ressort/politik/wahlen/france_president/v0/stichwahl.html?round=2)

*Properties: Keine*

*Unnamed Slot*
```{HTML}
<CardLayout>
    <!-- Markup -->
</CardLayout>
```

*Hinweis:* Durch den Schatten gibt es ein äußeres Margin, das im SCSS der Komponente definiert ist und den Card-Content einrückt. 
Diese Margin-Variable sollte in allgemeine Konfiguration gezogen werden, 
um Inhalte ohne Card-Layout (z.B. Headlines) in gleichem Maße einzurücken.
```{SCSS}
$horizontalSpacingCards: 0.2rem !default;
```

### [Header.svelte](./Header.svelte)

Header-Komponente mit Titel und Unterzeile, Titel-Schriftgröße von Fensterbreite abhängig

*Properties*
```{JavaScript}
// Beide Werte optinal; Typ: String
export let title;
export let subtitle;
```


### [Sources.svelte](./Sources.svelte)

Quellen-Komponente mit vorangestelltem SPIEGEL-"S&nbsp;&bull;"

*Properties*
```{JavaScript}
// Typ: String; bei Leer-String oder Leerzeichen keine Ausgabe
export let sources;
```


### [Link.svelte](./Link.svelte)

Simpler Artikel-Link (ohne Spiegel-Signet).

*Properties*
```{JavaScript}
export let href;
```
