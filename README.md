# TypeScript használata

## **1. lépés**
Készíts egy GitHub repo-t, klónozd a gépedre és lépj bele.

Ne felejtsd el a Node-os **`.gitignore`** fájlt opciót kiválasztani.

<br>


> terminál
```
$ git clone https://github.com/blaiseludvig/ts-alap.git
$ cd node ts-alap
```

Futtasd le az **`npm init`** parancsot a mappában.

Csak a package nevét fontos megadni, minden más maradhat alapértelmezett.

<br>


> terminál
```
$ npm init
...
package name: (ts-alap) ts-alap
```

Ez létrehozza a **`package.json`** fájlt.

<br>

## **2.lépés**
Nyissuk meg a **`package.json`**-t.

Adjuk hozzá a **`"type": "module",`** sort. Ez a sor teszi lehetővé az **`import`** használatát.



> package.json előtte
```json
{
  "name": "ts-alap",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}

```
<br>

> package.json utána
```json
{
  "name": "ts-alap",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

<br>

Hozzunk létre egy **`tsconfig.json`** fájlt, és másoljuk bele ezt az előre elkészített konfigurációt:

```json
{
    "compilerOptions": {
        "target": "ES2015",
        "module": "commonjs",
        "strict": true,
        "esModuleInterop": true,
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true
    },
    "$schema": "https://json.schemastore.org/tsconfig",
    "include": [
        "src/**/*"
    ],
    "exclude": [
        "node_modules",
        "**/*.spec.ts"
    ]
}
```

<br>

## **3. lépés**

Telepítsük a szükséges npm packageket.

> A **`--save-dev`** opció megadásával a parancs csak fejlesztéshez tölti le a packageket, ezzel megakadályozva azt hogy belekerülhessenek a kliens oldali kódunkba feleslegesen.

> terminál
```
$ npm install --save-dev webpack webpack-cli typescript
```

Ez a parancs letölti a megadott packageket, és hozzáadja őket a **`"devDependencies"`** listához a **`package.json`**-ban.

<br>

## **4. lépés**

A **`webpack`** és a **`TypeScript`** is az **`src`** mappában fogja keresni a fájlokat.

> tsconfig.json
```json
{
...
    "include": [
        "src/**/*"
    ],
...
}
```

<br>

Nincsen **`webpack.config.cjs`** fájl, de mivel most nem használunk mást amihez webpack kell pl.: **style-loader** vagy ilyesmi, ezért így is működni fog.

<br>

Állítsuk át a **`package.json`**-ban a **`main`**-t **`src/index.js`**-re

> package.json
```json
{
  ...
  "main": "src/index.js",
  ...
}
```

<br>

Viszont mivel **`TypeScript`**-et szeretnénk használni, ezért nem **`.js`** hanem **`.ts`** lesz az index fájl neve.

> src/index.ts
```ts
console.log("Hello World!");
```

<br>

A TypeScript fájl magában nem tud lefutni, mivel a TypeScript csak JavaScript-re képés lefordítani a kódot.

Az **`npx tsc`** parancs legenerálja az **`index.js`** fájlt amit futtatni tudsz.

Szeretnénk **`webpack`**-et is használni, hogy majd ne csak konzolban használhassuk a programunkat.

Az **`npx webpack`** parancs beolvassa az **`index.js`** fájlt amit a TypeScript generált, és elkészíti a **`dist`** mappában a **`main.js`**-t.

Ezt a **`main.js`** fájlt fogjuk futtatni, és böngészőből is ezt a fájlt tudjuk majd használni.

> terminál
```
$ node dist/main.js
Hello World!
```

## **5. lépés**

Oldjuk meg hogy módosításnál mindez automatikusan megtörténjen!

Ehhez 3 külön terminálra lesz szükségünk. A **vscode**-ban jobb oldalt van egy **+** gomb ami megnyit egy új terminált.

Az első terminálban futtassunk egy **`npx tsc --watch`** parancsot. Ez minden változtatásnál újragenerálja az **`index.js`** fájlunkat.

> 1. terminál
```
$ npx tsc --watch
[13:14:34] Starting compilation in watch mode...

[13:14:35] Found 0 errors. Watching for file changes.

█
```

<br>

A második terminálban az **`npx webpack watch`** parancs fog futni, ami az **`index.js`** fáljból fogja legenerálni a **`dist/main.js`** fájlt.

> 2. terminál
```
$ npx webpack watch
webpack 5.74.0 compiled with 1 warning in 220 ms
█
```

Miközben ezek a parancsok futnak, azokat a terminálokat nem lehet másra használni.
_**Ctrl + C**_-vel lehet leállítani őket hogy visszakapd a terminált.

<br>

A harmadik terminál arra kell hogy le tudjuk futtatni a fájlt.

> 3. terminál
```
$ node dist/main.js
Hello World!
```

<br>

## **6. lépés**

Próbáljuk ki a TypeScript funkcióit.

Vegyünk példának egy klasszikus JavaScript hibát.

```js
console.log("3" + 1);
console.log("3" - 1);
> 31
> 2
```

Kettő különböző típusú változót próbálunk összeadni vagy kivonni, és ez egy váratlan hibához vezet.

<br>

A TypeScript észreveszi ezt, és hibát jelez.

```bash
src/index.ts:2:13 - error TS2362: The left-hand side of an arithmetic operation must be of type 'any', 'number', 'bigint' or an enum type.

2 console.log("2" - 1);
              ~~~
```

<br>

A TypeScript lehetővé teszi, hogy megjelöljük a változók típusát a JavaScript-ben, és ezzel segít csökkenteni a fejlesztői hibákat.

Szintakszisa:

```ts
javascript változó kulcsszó, lehet const, var, vagy let
 |
 |  változó neve
 |   |
 |   |    váltzó típusa
 |   |     |
 |   |     |       változó értéke
 |   |     |        |
 🡓   🡓     🡓        🡓
let nev: string = "béla";
let kor: number;
kor = 18;
```

<br>

HTML elemeknél a `HTMLElement` nevű típust kell használni.

```ts
let szovegmezo: HTMLElement = document.getElementById("szovegmezo");
```

Ez még mindig nem jó, mivel a `getElementById`-vel keresett elem nem biztos hogy létezik. A TypeScript jelzi ezt a hibát:

```ts
src/index.ts:5:5 - error TS2322: Type 'HTMLElement | null' is not assignable to type 'HTMLElement'.
  Type 'null' is not assignable to type 'HTMLElement'.

1 let szovegmezo: HTMLElement = document.getElementById("szovegmezo");
      ~~~~~~~~~~
```

A `getElementById` egy `'HTMLElement | null'` típusú objektumot ad vissza. Ez a `'HTMLElement | null'` kifejezés azt jelenti, hogy vagy `HTMLElement` vagy `null` lesz, mivel nem biztos hogy van ilyen `id`-val rendelkező HTML elem.

<br>

```ts
let szovegmezo: HTMLElement | null = document.getElementById("szovegmezo");
szovegmezo!.textContent = "asd";
```

A `!.` operátor a **"non-null assertion operator"**. Ezzel megjelölheted azt, hogy egy típus biztosan nem `null`.

Ez az egyik helyes szintakszis a HTML elemekhez.

<br>

A másik helyes módszer a **``type casting``**-os megoldás:

```ts
let szovegmezo: HTMLElement = (document.getElementById("szovegmezo") as HTMLElement);

// ha nem akarjuk változóba menteni
(document.getElementById("szovegmezo") as HTMLElement).textContent = "asd";
```

vagy

```ts
let szovegmezo: HTMLElement = (<HTMLElement> document.getElementById("szovegmezo"));

// ha nem akarjuk változóba menteni
(<HTMLElement> document.getElementById("szovegmezo")).textContent = "asd";
```

Az **`as`** kulcsszó átalakítja `HTMLElement`-re a `getElementById` által visszaadott `'HTMLElement | null'` típusú értéket.

A `<>` szintakszisban pedig a szögletes zárojelben adjuk meg a típust.

<br>

A TypeScript attól is véd, hogy nem létező `property`-t próbáljunk használni egy HTML elemen:

> html
```html
<input type="number" id="szambevitel">
```

<br>

> typescript
```ts
(document.getElementById("szambevitel") as HTMLElement).value = "42";
```

<br>

> typescript terminál
```ts
src/index.ts:1:57 - error TS2339: Property 'value' does not exist on type 'HTMLElement'.

1 (document.getElementById("szambevitel") as HTMLElement).value = "42";
                                                          ~~~~~
```

<br>

Minden HTML elemnek van külön típusa a TypeScript-ben, és hibának jelzi, ha nem létezik a `property` azon az elemen. A `value` property csak az `input` elemeken létezik.

Ha megváltoztatjuk a típusát `HTMLInputElement`-re, akkor működni fog:

```ts
(document.getElementById("szambevitel") as HTMLInputElement).value = "42";
```

<br>

Egy pár TypeScript HTML elem:

* **`<div>`**: `HTMLDivElement`
* **` <a>`**: `HTMLAnchorElement`
* **`body`**: `HTMLBodyElement`
* **`<button>`**: `HTMLButtonElement`
* **`img`**: `HTMLImageElement`

További elemek listája: <https://html.spec.whatwg.org/multipage/indices.html#elements-3>
