# Warsztaty d3.js
---
### Plan
1. svg, podstawowe elementy, d3js: swiadomosc roznic v3 vs v4
3. osie, dopasowanie wykresu, koordynaty, jak rysowac od 0,0
2. prosty wykres typu barchart, data bidning join, enter, exit, update pattern
4. transormacje atrybutow, animacje (transition)
5. generatory: d3.line, d3.arc
6. rysowanie punktów po okregu, trygonomeria, podstawy, cos, sin, alfa
7. text, kierunek, obrót, anchor, base align
8. customowy path generator
9. responsywność

***
## Materiały

### Część 1
#### Element SVG
poprawny element SVG wygląda następująco:
```html
    <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
```
Domyślne rozmiary tak stworzonego elementu to 300 x 150px.
**Źródło:** https://developer.mozilla.org/en-US/docs/Web/SVG/Element/svg

#### SVG Child nodes
Dynamiczne tworzenie elementów svg z użyciem JS wymaga użycia ``Document.createElementNS()`` zamiast ``Document.createElement``. Jest to o tyle istotne, że wszelkie elementy SVG muszą być stworzone w odpowiedniej przestrzeni:
```javascript
    var ns = 'http://www.w3.org/2000/svg';
    var svg = document.createElementNS(ns, 'svg');
    document.body.appendChild(svg);

    var circle = document.createElementNS(ns, 'circle');
    circle.setAttribute('cx', 100);
    circle.setAttribute('cy', 100);
    circle.setAttribute('r', 10);
    svg.appendChild(circle);
```
link: https://jsfiddle.net/amortka/py1559yp/

### Podstawowe elementy
oraz ich najważniejsze atrybuty

#### circle
Koła lub okręg - w zależności czy zdecydujemy się na wypełnienie. Domyślnie wypełniony na czarno.
```html
<circle cx=100 cy=100 r=10></circle>
```
- **cx** - współrzędna środka x
- **cy** - współrzędna środka y
- **r** - promień 

#### rect
Prostokąt
```html
<rect x=50 y=125 width=250 height=250 />
```
- **x** - współrzędna x górnego lewego wierzchołka
- **y** - współrzędna y górnego lewego wierzchołka
- **width** - długość liczona
- **height** - wysokość

#### line
Linia a właściwie odcinek
```html
<line x1=10 y1=10 x2=200 y2=150 stroke="black"/>
```
- **x1** - współrzędna x punktu początkowego
- **y1** - współrzędna y punktu początkowego
- **x2** - współrzędna x punktu końcowego
- **y2** - współrzędna y punktu końcowego
- **stroke** - kolor linii
### text element
Element tekstowy
```html
<svg>
  <circle cx=50% cy=50% r=50>aaa</circle>
  <text x="50%" y="50%" text-anchor="middle" fill="#fff" dy=".3em">text</text>
</svg>
```
- **x** - współrzędna y punktu zaczepienia
- **y** - współrzędna y punktu zaczepienia
- **text-anchor** - sposób zaczepienia tekstu, możliwe wartości: ``['start', 'middle', 'end']``
- **fill** - kolor wypełnienia
- **dy** - przesunięcie względem punktu zaczepienia

## Współrzędne
Wszystkie współrzędne są renderowane relatywnie od punktu ``(0,0)``, który znajduje się domyślnie w gónym lewym wierzchołku elementu SVG. Zmienia to zachowanie rzędnej ``(OY)``, przez co punkty w systemie odniesienia SVG z wartością dodatnią oddalają się od osi odciętych ``(OX)`` w dół. Tak więc na poniższym przykładzie punkt ``zielony`` o współrzędnych ``(100, 120)`` znajduje się niżej niż punkt ``czerwony`` o współrzędnych ``(100, 80)``:
```javascript
    var svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
    document.body.appendChild(svg);

    var circle = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
    circle.setAttribute('cx', 100);
    circle.setAttribute('cy', 80);
    circle.setAttribute('r', 10);
    circle.setAttribute('id', 'red');
    svg.appendChild(circle);
    
    
    var circle = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
    circle.setAttribute('cx', 100);
    circle.setAttribute('cy', 120);
    circle.setAttribute('r', 10);
    circle.setAttribute('id', 'green');
    svg.appendChild(circle);
```
link: https://jsfiddle.net/amortka/x92m88og/


## D3.js
Dzięki d3 uzyskujemy pewien poziom abstrakcji, dzięki czemu tworzenie elementów oraz tworzenie selekcji oraz ustawianie atrybutów poszczególnych elementów jest proste:
##### html:
```html
<script src="https://d3js.org/d3.v4.min.js"></script>
```
##### **js**:
```javascript
    var svg = d3.select('body').append('svg')
      .attr('width', 800)
      .attr('height', 600);

    var circleEl = svg.append('circle')
      .attr('cx', 100)
      .attr('cy', 100)
      .attr('r', 25)
      .attr('fill', 'red')
```
link: https://jsfiddle.net/amortka/abyuvubg/

## D3.js selection multi
Może się wydawać, że chain pattern jest męczący i podawanie każdego atrybytu osobno jest słabym rozwiązaniem. Czasami faktycznie bywa to męczące, ale z pomocą przychodzi rozszerzenie do d3 `d3-selection-multi`, aby dodać wystarczy załadować rozszerzenie oraz zmienić ``attr`` na ``attrs``:
```html
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://d3js.org/d3-selection-multi.v1.min.js"></script>
```
```javascript
    var svg = d3.select('body').append('svg')
      .attrs({
        'width': 800,
        'height': 600
      });

    var circleEl = svg.append('circle')
      .attrs({
        'cx': 100,
        'cy': 100,
        'r': 25,
        'fill': 'red'
      });
```
link: https://jsfiddle.net/amortka/6x6gmkd4/

Rozszerzenie nie jest częścią domyślnej paczki (d3.js bundle), dlatego też na potrzeby playgrounda ładowane jest osobnym script tagiem. Jednak docelowo wybrane części d3 oraz rozszerzenia powinny być budowane przy użyciu [Rollupa](https://rollupjs.org/) lub [Webpacka](https://webpack.github.io/).
rozszerzenie: https://github.com/d3/d3-selection-multi

---
### Część 2

## Rysowanie punktów na okręgu
Aby rysować punkty po okręgu potrzebne nam są następujące informacje:
- współrzędne punktu wokół którego chcemy rysować punkty,
- promień czyli odległość w jakiej będą znajdować się punkty od punktu centralnego
- kąt w jakim dany punkt powinien się znaleźć.

Korzystając z własności funkcji trygonometrycznych sinus i cosinus, znajac ich przebieg:
![alt text](https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Sine_cosine_one_period.svg/600px-Sine_cosine_one_period.svg.png)
oraz wiedzy, że pełen kąt jest równy 2 * PI, możemy określić przesunięcie punktu x oraz y:
 ```javascript
 let x = r * -Math.sin(alfa);
 let y = r * -Math.cos(alfa);
 ```
to która funkcja jest użyta jako pierwsza zależne jest od miejsca w którym chcemy zacząć. Dla powyższego przykładu dla `alfa === 0` przesunięcie na osi `x` jest równe 0 (`r * -Math.sin(0)`) oraz y = -r (`r * -Math.cos(0)`). To powoduje przesunięcie punktu w pozycję wyjściową na górze oraz obracanie przeciwnie do ruchu wskazówek zegara. Zmieniając znak oraz zamieniając funkcje sin z cos (zawsze muszą występować parami) mamy możliwość ustawienia punktu początkowego w dowolnym miejscu oraz zmienić kierunek obrotu.

Argumentem funkcji `Math.sin` oraz ` Math.cos` jest kąt mierzony w radianach, aby przeliczyć kąt na radiany i odwrotnie posługujemy się następującymi wzorami:
```javascript
function rad2deg(rad) {
  return rad * (180 / Math.PI);
}

function deg2rad(deg) {
  return deg * (PI / Math.180);
}
```

przykłady: 
- https://codepen.io/amortka/pen/dRpvBp
- https://codepen.io/amortka/pen/jmWMqG
- https://codepen.io/amortka/pen/jmWMqG

## Skale
Skale w d3.js to zwykłe funkcje, które dla zadanej wartości zwracają nam nową wartość przeskalowaną w zależności od konfiguracji funkcji. Najczęściej używana jest skala liniowa oraz czasowa.

### scaleLinear
```
const data = [
  0, 2, 3, 4, 5, 6, 7
]

const x =  d3
      .scaleLinear()
      .range([0, 100])
      .domain([0, 10]);

data.forEach(d => {
  console.log(`x(${d}) -> ${x(d)}`);
})
```

Skalę tworzymy korzystając z metody `d3.scaleLinear()`, do jej podstawowej konfiguracji używamy dwóch metod:
- `.domain` - określa domenę skali, wymaga co najmniej dwóch elementów, jeśli podane elementy nie są liczbami będą na nie rzutowane. 
- `.range` - określa zasięg funkcji, czyli jakie wartości będą zwracane przez skalę. Mogą to być dowolne wartości, które są obsługiwane przes interpolator. np `.range(["red", "white", "green"]);`

Przykładowa skala liniowa, która dla wartości od 0 do 10 będzie interpolować do wartości 0, 100.
```javascript
const data = [
  0, 2, 3, 4, 5, 6, 7, 25
]

const x =  d3
      .scaleLinear()
      .range([0, 100])
      .domain([0, 10]);

data.forEach(d => {
  console.log(`x(${d}) -> ${x(d)}`);
})
```
W przypadku wartości wejściowej `25` wynik to `250` zgodnie z przwidywaniami. Aby uniknąć wychodzenia poza zasięg można skorzystać z metody `.clamp(true)`.
Przykład: https://codepen.io/amortka/pen/RgodbM


## Osie

## Linie

## Animacje (transition)

## Sekwencja animacji (transition chaining)

## Zdarzenia

## Zdarzenia animacji (transition.on)

## Zdarzenie endAll


przykład: https://codepen.io/amortka/pen/NgRNYm
---
---
## Przydatne linki:
- [playground codepen](https://codepen.io/amortka/pen/qmLddG?editors=0010)
- [dokumentacja d3.js](https://github.com/d3/d3/blob/master/API.md)
- [svg basics](https://bl.ocks.org/curran/098af28142c664535cdf624d09dd90a8)
- [svg docs](https://developer.mozilla.org/en-US/docs/Web/SVG/Element)
- [svg for begginers](http://unicorn-ui.com/blog/svg-for-beginners.html)
- [svg tutorial](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial)
- [using svg](https://css-tricks.com/using-svg/)

Na koniec parę inspiracji:
- [bl.ocks](https://bl.ocks.org/)
- [Twórca d3.js](https://bost.ocks.org/mike/)
- [Wizualizacje, tematycznie](http://christopheviau.com/d3list/gallery.html)
- [galeria twórcy d3.js](https://bl.ocks.org/mbostock)
- [d3 1000 demos](http://techslides.com/over-1000-d3-js-examples-and-demos)
- [30 amazing data viz galleries](http://visualoop.com/blog/11044/30-amazing-data-viz-galleries-everyone-should-follow)





