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
```html
<circle cx=100 cy=100 r=10></circle>
```
- **cx** - współrzędna środka x
- **cy** - współrzędna środka y
- **r** - promień 

#### rect
```html
<rect x=50 y=125 width=250 height=250 />
```
- **x** - współrzędna x górnego lewego wierzchołka
- **y** - współrzędna y górnego lewego wierzchołka
- **width** - długość liczona
- **height** - wysokość

#### line
```html
<line x1=10 y1=10 x2=200 y2=150 stroke="black"/>
```
- **x1** - współrzędna x punktu początkowego
- **y1** - współrzędna y punktu początkowego
- **x2** - współrzędna x punktu końcowego
- **y2** - współrzędna y punktu końcowego
- **stroke** - kolor linii
### text element
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



