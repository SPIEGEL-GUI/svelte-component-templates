# svelte-component-templates
DER SPIEGEL templates for Svelte components


## Utility components


### [TextAlign.svelte](./TextAlign.svelte)

Align child elements to the type area of spiegel.de.

*Properties*
``` javascript
// type of the app: iframe or embed
export let type = "iframe";

// disable component (if true, no alignment takes place)
export let disable = false;
```


### [SizeDetector.svelte](./SizeDetector.svelte)

Determine the size of an element without using Svelte iFrames and `bind:clientWidth` etc.

Good for apps that will be embedded directly.

*Properties*
``` javascript
// width and height can be bound from outside
export let width = undefined;
export let height = undefined;
```

*Usage*
```sveltehtml
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
``` javascript
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
``` javascript
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
```sveltehtml
<CardLayout>
    <!-- Markup -->
</CardLayout>
```

*Hinweis:* Durch den Schatten gibt es ein Margin, das im SCSS der Komponente definiert ist und den Card-Content einrückt. 
Diese Margin-Variable sollte in allgemeine Konfiguration gezogen werden, 
um Inhalte ohne Card-Layout (z.B. Headlines) auf gleicher Höhe auszurichten.
```SCSS
$horizontalSpacingCards: 0.2rem !default;
```

### [Header.svelte](./Header.svelte)

Header-Komponente mit Titel und Unterzeile, Titel-Schriftgröße von Fensterbreite abhängig

*Properties*
``` javascript
// Beide Werte optinal; Typ: String
export let title;
export let subtitle;
```

### [Sources.svelte](./Sources.svelte)

Quellen-Komponente mit vorangestelltem SPIEGEL-"S&nbsp;&bull;"

*Properties*
``` JavaScript
// Typ: String; bei Leer-String oder Leerzeichen keine Ausgabe
export let sources;
```

### [Link.svelte](./Link.svelte)

Simpler Artikel-Link (ohne Spiegel-Signet).

*Properties*
``` JavaScript
export let href;
```

### [AutoCompleteSelect.svelte](./AutoCompleteSelect.svelte)

> Hinweis: Diese Komponente ist in der Kategorie "Simple HTML components" nur eingeschränkt richtig platziert.

Dieses Select-Element bietet nicht nur eine Auswahl-/Ausklapp-Liste, sondern auch ein "Autocomplete": 
beim Tippen in das Suchfeld werden die Listen-Einträge nach dem Suchwort gefiltert.

Diese Komponente ist ein Wrapper für das Package [svelte-select](https://www.npmjs.com/package/svelte-select), das ab Version 5 vorausgesetzt wird.
Bei Benutzung der Komponenten-Datei ist also `npm i svelte-select` auszuführen.

>Das Package "svelte-select" wurde im Juni 2023 in der Version `5.6.1` verwendet (`wirtschaft/2023/ladestationen`).
<br>


*Properties*
``` javascript
// A) Datenfluss in die Komponente
export let showSelectLabel = true;
export let selectLabel = "";
export let data = [];
export let selectOptions = {};

// B) Datenfluss aus der Komponente heraus
// aktuell ausgewähltes Listen-Element
export let selectedItem;
// Cursor im Input-Element oder nicht; intern gesetzt
export const selectFocused = writable(false);
// Methode: löscht Inhalt aus Input
export const clearInput
```

*Usage*
```sveltehtml
<AutoCompleteSelect
    selectLabel="Select mit Einträge-Liste komplexer Daten"
    selectOptions="{{
        dataFieldNames: {key: 'iso', label: 'country'},
        additionalSearchFields: ['iso', 'alt']
    }}"
    data="{[{iso: 'alb', country: 'Albanien'}, {iso: 'gbr', country: 'Vereinigtes Königreich', alt: 'uk'}]}"
    bind:clearInput
    bind:selectedItem
></AutoCompleteSelect>
{#if selectedItem}
    <p on:click={() => clearInput()}>Ausgewählt: {selectedItem.label}</p>
{/if}
````

Über dem Select-Element kann ein <b>Label</b> stehen, Text und Sichtbarkeit
werden getrennt gesteuert.

Das <b>Data-Array</b> kann entweder einfache Daten enthalten (String / Number) oder
Objekte. Darin muss ein Feld ID/Key-artig sein, ein weiteres als Label in der Liste dienen. 
Einfache Daten werden intern in Objekte umgewandelt: 
`["myString", 11] => [{id: "myString", label: "myString"}, {id: "11", label: "11"}]`
und als solche mit `selectedItem` zurückgegeben. Bei Objekten im Data-Array ist das vollständige Objekt 
(nicht nur ID, Label, weitere Suchfelder) in `selectedItem.item` zu finden.

In den `selectOptions: {}` können die Namen des ID- bzw. des Label-Datenfeldes festgelegt werden; die Daten 
müssen nicht `id` oder `label` enthalten:
``` javascript
{
  dataFieldNames: {key: "iso3", label: "country"}
}
```
Weitere Felder des Data-Item-Objekts können als zusätzliche Suchfelder (für das Autocomplete) festgelegt werden.
``` javascript
{
  additionalSearchFields: ["iso3", "continent"]
}
```
Mit `swe` könnte im Beispiel der Eintrag mit dem Label `Schweden` gefunden werden, 
mit `afrika` alle Länder, die diesem Kontinent im Datenfeld `continent` zugewiesen sind.

Der Default-Placeholder `Suche` des Input-Feldes kann in den `selectOptions` überschrieben werden:
``` javascript
{
  placeholder: "Flughafen-Suche"
}
```

Weitere Optionen sind im Code der Komponente in `const defaultSelectOptions` nachzulesen.

