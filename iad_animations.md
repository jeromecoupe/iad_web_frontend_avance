# Animations et transitions

## Vendor prefixes et feature detection

Animations et transitions sont des propriétés relativement récentes et qui ne sont pas toujours implémentées nativement dans certains navigateurs.

Pour différencier les propriétés CSS encore en développement de celles qui font partie des recommandations du W3C, les divers navigateurs ont utilisé des extensions propriétaires qui se placent devant les propriétés CSS. Aujourd'hui, les choses ont changées et [les navigateurs utilisent d'avantage des flags](http://demosthenes.info/blog/217/CSS-Vendor-Prefixes-and-Flags).

Des outils comme [prefixr](http://prefixr.com/) ou Autoprefixer pour [Grunt](@TODO) ou [Gulp](@TODO) peuvent vous aider à être certain de ne rien oublier. Des ressources comme [caniuse](http://caniuse.com/) et [html5please](http://html5please.com/) vous donneront quantités d'informations précieuses.
- `-moz-`: Firefox et autres navigateurs basés sur Geko- `-webkit-`: Safari et autre navigateurs Webkit (Chrome)- `-khtml-`: Konqueror- `-o-`: Opera- `-ms-`: MicrosoftCertains navigateurs ne supportent tout simplement pas ces propriétés. Il est donc important de prévoir des alternatives au cas où. Une librairie telle que [Modernizr](@TODO) vous permettra de détecter le support de ces propriétés par le navigateur du client.  

## Transitions en CSS3
Les [transitions](@TODO MDN) permettent au navigateur de gérer la transition entre deux états d’un élément spécifiés par CSS. Voici [une liste des propriétés CSS pouvant être animées](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties).

Les transitions CSS sont souvent utilisées pour "lisser" les transitions entre deux états d'éléments d'interface: boutons (default et hover ou touch), apparitions successives des éléments d'un menu, etc.Les propriétés permettant de gérer les transitions sont `transition-property`, `transition-duration`, `transition-delay`, `transition-timing-function`.

```css
.myElement
{	transition-property: background-color;
	transition-duration: .25s;
	transition-delay: .1s;
	transition-timing-function: linear;
}
```
Il est également possible d’utiliser une notation courte:```css
.myElement
{	transition: all 2s .2s ease;
}```Vous pouvez éventuellement combiner diverses transitions, en notation étendue comme en notation courte.      
```css
.myElement
{	transition-property: background-color, transform;
	transition-duration: .25s, .5s;
	transition-delay: .1s, .2s;
	transition-timing-function: linear, ease-in;
}```ou￼￼
```css
.myElement
{	transition: background-color .25s .1s linear,
				  transform .5s .2s ease-in;
}```

**Note:** la propriété `transition-timing-function` peut également être exprimée avec des courbes de Bezier pour plus de précision. Lea Verou a réalisé un [outil en ligne](http://cubic-bezier.com/) vous permettant de les calculer et de les visualiser facilement. A voir aussi, [Caeser](http://matthewlein.com/ceaser/) par Matthew Lein.*Exercice: expérimenter avec les transitions en :hover*

## Transformations en CSS3

### Transformations 2D

Avec CSS3, nous disposons maintenant de propriétés nous permettant de faire subir des transformations aux éléments HTML. Ces propriétés sont supportées par [la plupart des navigateurs récents](http://caniuse.com/#feat=transforms2d) mais, dans la plupart des cas, [elles nécessitent d'utiliser des vendor prefixes](http://html5please.com/#transform).

L’ensemble de ces transformations ont lieu après que la page soit rendue et n’influencent donc pas le rendu de la page.

#### Translate

Effectue une translation, soit sur l’axe horizontal, soit sur l’axe vertical, soit sur les deux. Les valeurs spécifiées peuvent être positives ou négatives.

```css
.myElement
{
	transform: translateX(100px);
	transform: translateY(20%);
	transform: translate(100px,20%);
}
```

#### Scale

Effectue une mise à l’échelle. Cette mise à l’échelle peut concerné la hauteur ou la largeur d’un élément ou les deux. L’argument ne prend pas ici d’unité de mesure, il s’agit d’un ratio par rapport à la taille par défaut de l’élément.

```css
.myElement
{
	transform: scaleX(1.5);
	transform: scaleY(1.5);
	transform: scale(1.5);
}
```

#### Rotate

Effectue une rotation. Les valeurs spécifiées peuvent être positives ou négatives. Ces valeurs peuvent être spécifiées en degrés, en @TODO ou en nombre de tours

```css
.myElement
{
	transform: rotate(45deg);
	transform: rotate(2turns);
}
```

#### Skew

Effectue une distorsion de type “skew” spécifiée en degrés. Celle-ci peut être appliquée selon les axes horizontaux ou verticaux ou encore selon les deux à la fois. Les valeurs spécifiées peuvent être positives ou négatives.

```css
.myElement
{
	transform: skewX(30deg);
	transform: skewY(30deg);
	transform: skew(30deg);
}
```

#### Transform-origin

Par défaut, ces transformation prennent généralement le coin supérieur droit de la bounding-box de l’élément comme point de référence. La propriété transform-origin` permet de modifier ce point de référence.

```css
.myElement
{
	transform-origin: 0 0;
	transform-origin: 0 50%;
	transform-origin: 100% 0;
	transform-origin: 100% 100%;
}
```

#### Chaining

Nous pouvons également combiner différentes transformation en les chainant et en les séparant par un espace. Les navigateurs appliquent ces diverses transformations successivement en commençant par la gauche.

```css
.myElement
{
	transform : rotate(45deg) scale(2);
}
```

*Exercice: expérimenter avec les transformation 2D en :hover*

### Transformations 3D

La plupart des transformations 2D ont leur équivalent en 3D. L'une des notions les plus importantes à comprendre est celle de perspective. [Chris Coyier vous en donne un bon aperçu sur CSS Tricks](http://css-tricks.com/almanac/properties/p/perspective/).

En bref, la perspective est la distance la profondeur de l'axe Z, la distance entre un objet situé sur celui-ci et l'utilisateur. Ces valeurs peuvent aller de 1 à 1000. Plus ce chiffre est petit, plus la perspective est importante. Plus ce chiffre est grand, plus l'effet sera subtil.

La perspective peut être spécifiées de deux façons:

Via la propriété `transform` directement sur l'élément concerné. Dans ce cas, chaque élément concerné possède son propre "vanishing point".

```css
.myElement
{
	transform: perspective(600);
}
```

Via la propriété `perspective` sur l'élément parent. Dans ce cas, cela affecte l'ensemble des enfants du parent, qui partagent alors tous le même "vanishing point".

```css
.myParentElement
{
	perspective:600;
}
```

La propriété `transform-style` peut prendre les valeurs `flat`ou `preserve-3d`. La seconde valeur permet de gérer la position des éléments dans un espace 3D. Elle permet également d'étendre le contexte 3D à tous les descendants du parent auquel la `perpective` aura été appliquée.

Avec les transformations 3D, vous pouvez placer certains éléments de telle façon que leur "avant" ne fasse plus face à l'écran. Par exemple avec une rotation:

```css
.flip
{
  transform: rotateY(180deg);
}
```

La propriété `backface-visibility` permet de gérer la visibilité des faces d'un élément lorsqu'elles ne font pas face à l'écran. Les valeurs possible sont `visible` et `hidden`. La dernière est le plus souvent utilisée.

*Exercice: expérimenter avec les transformation 3D, perspective et backface-visibility*

## Animations en CSS3

Voyons maintenant comment créer de véritables animations en CSS3. Le processus comporte deux étapes distinctes.

1. Définir votre animation
2. L'assigner à un ou plusieurs éléments HTML

### Définir vos animations avec @keyframe

Vous pouvez nommer votre animation et décrire les étapes qui la composent en utilisant l'élément `@keyframe`. Celle-ci défini les propriétés qui doivent être modifiées et à quel moment elles doivent l'être dans le déroulement de votre animation.

Les étapes de votre animations peuvent soit être décrites à l'aide des mots-clés `from` et `to`, soit à l'aide de valeurs en pourcentage. Ces dernières sont utiles si vous avez plus de deux étapes et/ou si vous avez besoin d'une progression non-linéaire.

```css
@keyframes move
{	from {transform: translateX(0);}
	to {transform: translateX(400px);}
}
```

```css
@keyframes move
{	0% {transform: translateX(0);}
	20% {transform: translateX(100px);}
	100% {transform: translateX(400px);}
}
```

Note: si vous ne spécifiez pas de de keyframe à 0% ou 100%, les styles originaux appliqués à votre élément seront utilisés.

### Assigner l'animation à un ou plusieur éléments HTML

Vous allez maintenant assigner cette animation à un élément HTML et en définir les caractéristiques pour cet élément. Cela se fait à l'aide des propriétés suivantes

- `animation-name`: spécifie le nom de l'animation à appliquer (celui que vous avez spécifié dans votre élément @keyframe) 
- `animation-duration`: défini la durée de l'animation. Cette propriété est exprimée en secondes `s` ou en milisecondes `ms`.
- `animation-timing-function`: défini le easing de votre animation: `ease-in`, `ease-out`, `linear` ou encore `cubic-bezier(0.1, 0.7, 1.0, 0.1)` sont des valeurs possibles. Par défaut, la valeur est `ease`.
- `animation-iteration-count`: défini le nombre de fois que l'animation doit être effectuée. Par défaut, la valeur est 1. Cette valeur peut également être définie comme `infinite`.

`@keyframe`, `animation-name` et `animation-duration` sont suffisants mais il vaut mieux toujours définir explicitement les deux autres.

*Exercice: réaliser plusieurs nuages qui traversent l'écran (une seule @keyframe) et expérimenter avec les diverses propriétés vues ci-dessus.*

### Différentes autres propriétés

#### `animation-delay`

`animation-delay` défini le délais avant lequel l'animation se déclenche. Spécifié en secondes `s` ou milisecondes `ms`.

#### `animation-fill-mode`

`animation-fill-mode` défini la manière dont se comporte l'élément animé une fois l'animation terminée. Les valeurs possibles sont:

- `none`: l'élément retrouve ses caractéristiques originales une fois l'animation terminée
- `forwards`: l'élément conserve les caractéristiques de la dernière frame de l'animation
- `backwards`: l'élément possède les caractéristiques de la première frame de l'animation pedant la durée de `animation-delay`. Cette valeurs est donc particulièrement utile en travaillant avec des délais.
- `both`: combine les effets de `backwards` et `forwards`.

*Exercice: expérimenter avec les diverses valeurs de `animation-fill-mode`.*

#### `animation-direction`

`animation-direction` défini la direction dans laquelle se déroule l'animation.

- `normal`: l'animation se déroule de la première frame définie jusqu'à la dernière. C'est la valeur par défaut.
- `reverse`:  l'animation se déroule de la dernière frame définie jusqu'à la première.
- `alternate`: ne peut être utilisé que lorsque votre propriété `animation-count` est supérieure à 1. La première fois que l'animation est exécutée, elle se déroule en mode `normal`, la seconde fois en mode `reverse`, etc.
- `alternate-reverse`: identique à `alternate` hormis que l'animation se déroule d'abord en mode `reverse`.

*Exercice: expérimenter avec les diverses valeurs de `animation-direction`.*

## Notation courte et chaining

Une notation courte existe évidemment pour appliquer vos animations à un élément HTML.

```cssanimation: myAnimation .5s ease-in 1s 3;
```

Dans l'ordre: <`animation-name`> <`animation-duration`> <`animation-timing-function`> <`animation-delay`> <`animation-iteration-count`>.

Il est également possible de chaîner plusieurs animations sur un même élément.

```css
animation: myAnimation 1s ease-in-out 2s 4,
           myOtherAnimation 4s ease-out 2s;
```

### Exercices

- Voitures roulant à travers l'écran
- Animation image par image avec un sprite et `steps`

### Démarrer et arrêter une animation avec `animation-play-state`

Vous pouvez facilement définir vos animations, les assigner à vos éléments HTML et en contrôler l'état avec `animation-play-state` qui peut avoir deux valeurs: `running` (default) et `paused`.

Ces propriétés peuvent être modifiées facilement en CSS avec des pseudo-classes comme `:hover` ou en utilisant JavaScript (EventListener et ClassList). @TODO

Exemple avec `:hover`

```css
@keyframes rotate
{
	0% {transform: rotate(0);}	100% {transform: rotate(1turn);}}

.sticker
{	animation: rotate 5s linear infinite;
	animation-play-state: paused;}

.sticker:hover
{	animation-play-state: running;}
```

Exemple avec classes manipulées via JS

```css
@keyframes rotate
{
	0% {transform: rotate(0);}	100% {transform: rotate(1turn);}}

.sticker
{	animation: rotate 5s linear infinite;
	animation-play-state: paused;}

.sticker.is-animated
{	animation-play-state: running;}
```

## Ressources

- [An introduction to CSS 3D Transforms](http://24ways.org/2010/intro-to-css-3d-transforms/) par [David De Sandro](http://desandro.com/)
- Le même article, assorti de [démonstrations sur github](http://desandro.github.io/3dtransforms/docs/introduction.html) par David De Sandro toujours
- [CSS Animations](@TODO): un superbe bouquin pour commencer par Val Head sur Five Simple Steps.
- [Primer on Bézier Curves](http://pomax.github.io/bezierinfo/)
- [Animations and UX Resources](http://rachelnabors.com/animation-ux/) par Rachel Nabors
- [Keyframe Animations Syntax](http://css-tricks.com/snippets/css/keyframe-animation-syntax/) par Chris Coyier sur CSS Tricks.
- [CSS3 Transitions, Transforms, Animation, Filters and more!](http://css3.bradshawenterprises.com/) par Rich Bradshaw. Un superbe résumé de tout ce qu'il faut savoir, exemples inclus.
- [Disney's 12 principles of animations](http://en.wikipedia.org/wiki/12_basic_principles_of_animation)
- [Google Material Design - animation](@TODO)
- [All the right moves](http://vimeo.com/86821694): vidéo expliquant les interactions entre animations CSS et JavaScript par Val Head
