## CSS-Klassen auf Basis von booleschen Werten setzen/entfernen {#c03-dynamic-classes}

### Problem

Ich möchte anhand eines booleschen Wertes definieren, wann eine CSS-Klasse gesetzt wird und wann nicht.

### Zutaten
* [Eine Komponente](#c02-component-definition)
* CSS-Klassen, die gesetzt bzw. entfernt werden sollen
  * Siehe auch [Das Template der Komponente vom CSS trennen](#c07-styles)
* Eigenschaften der Komponente, die wir im Template referenzieren
* NgClass-Direktive von Angular

### Lösung 1

{title="app.component.ts", lang=js}
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  styles: [
    `.box {
        width: 100px;
        height: 100px;
        border: 1px solid black;
    }`,
    '.red { background-color: red; }',
    '.green { background-color: green; }'
  ],
  template: `
    <div class="box" [ngClass]="{red: box.isRed}"></div>
    <div class="box green" [ngClass]="{green: box.isGreen}"></div>
  `
})
export class AppComponent {
  box = {
    isRed: true,
    isGreen: false
  };
}
```

__Erklärung__:

* Zeilen 6-12: Definition der CSS-Klassen, die wir benötigen
* Zeilen 15-16: Zwei div-Tags mit CSS-Klassen. Initiale CSS-Klassen werden über das class-Attribut gesetzt. Dynamische CSS-Klassen werden mit Hilfe der ngClass-Eigenschaft gesetzt. Die Eigenschaft bekommt als Wert ein Objekt, dessen Keys die CSS-Klassen sind, die dynamisch hinzugefügt und entfernt werden
  * Zeile 15: Durch die [input-Eigenschaft](#gl-input-property) "ngClass" wird die CSS-Klasse "red" gesetzt, weil die isRed-Eigenschaft des box-Objektes __true__ ist
  * Zeile 16: Durch die [input-Eigenschaft](#gl-input-property) "ngClass" wird die CSS-Klasse "green" entfernt, weil die isGreen-Eigenschaft des box-Objektes __false__ ist
* Zeilen 20-23: Objekt mit booleschen Werten, die benutzt werden, um CSS-Klassen im Template hinzuzufügen bzw. zu entfernen

### Lösung 2

Wir haben bereits in Lösung 1 gesehen, dass die ngClass-Eigenschaft ein Objekt mit CSS-Klassen als Keys und __true__/__false__ als Werten bekommt.
Statt das Objekt im Template zu definieren, können wir es auch in unserer Klasse definieren.

{title="app.component.ts", lang=js}
```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  styles: [
    `.box {
        width: 100px;
        height: 100px;
        border: 1px solid black;
    }`,
    '.red { background-color: red; }'
  ],
  template: '<div [ngClass]="classes"></div>'
})
export class AppComponent {
  classes = {
    red: true,
    box: true
  };
}
```

__Erklärung__:

* Zeilen 6-11: Definition der CSS-Klassen, die wir benötigen
* Zeile 13: div-Tag mit ngClass-Eigenschaft, die auf die classes-Eigenschaft der Klasse zugreift
* Zeilen 16-19: Objekt mit CSS-Klassen als Keys und boolesche Werte die angeben, ob die CSS-Klassen gesetzt werden oder nicht

### Diskussion

Um das Beispiel möglichst klein zu halten, haben wir hier auf das dynamische Verändern der CSS-Klassen verzichtet.
Im Github Code-Beispiel wird gezeigt, wie wir mittels "click" die CSS-Klassen für unsere div-Tags entfernen und hinzufügen können.
Um das Code-Beispiel zu verstehen, wird zusätzlich das Rezept "[Auf Nutzer-Input reagieren](#c03-user-input)" benötigt.

Wir nutzen hier eine [Datenbindung](#gl-data-binding) mit eckigen Klammern ([...]).
Diese Art der Datenbindung wird Eigenschafts-Bindung genannt.
Falls wir nur eine einzige Klasse nutzen, können wir auch eine Klassen-Bindung dafür nutzen.

I> #### CSS-Styles in einer Komponente
I>
I> Wenn wir CSS-Styles in einer Komponente definieren, können wir diese CSS-Styles standardmäßig nur in der Komponente verwenden, in der diese definiert worden sind. Dieses Verhalten kann uns vor Fehlern schützen und meidet Konflikte in CSS-Styles, wenn man z. B. Komponenten wiederverwendet. Die Kapselung von CSS-Styles und Komponenten wird in Angular "View-Encapsulation" genannt.

### Code

Code auf Github für die [erste Lösung](https://github.com/jsperts/angular2_kochbuch_code/tree/master/03-Recipes_to_Manipulate_the_View/04-Dynamic_Classes/Solution-01)

Live Demo für die erste Lösung auf [angular2kochbuch.de](http://angular2kochbuch.de/examples/code/03-Recipes_to_Manipulate_the_View/04-Dynamic_Classes/Solution-01/index.html)

Code auf Github für die [zweite Lösung](https://github.com/jsperts/angular2_kochbuch_code/tree/master/03-Recipes_to_Manipulate_the_View/04-Dynamic_Classes/Solution-02)

Live Demo für die zweite Lösung auf [angular2kochbuch.de](http://angular2kochbuch.de/examples/code/03-Recipes_to_Manipulate_the_View/04-Dynamic_Classes/Solution-02/index.html)

### Weitere Ressourcen

* Offizielle Dokumentation der [NgClass-Direktiven](https://angular.io/docs/ts/latest/api/common/index/NgClass-directive.html)
* Weitere Informationen zur Eigenschaften- und Klassen-Bindung gibt es in [Appendix A: Template-Syntax](#appendix-a)
* Informationen zur View-Encapsulation gibt es in [unserem Blog](https://jsperts.de/blog/angular2-view-kapselung/)
* Eine weitere Möglichkeit, CSS-Styles für eine Komponente zu definieren, gibt es im Rezept "[Komponente und CSS trennen](#c07-styleurls)"

