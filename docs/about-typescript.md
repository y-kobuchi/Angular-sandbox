# Typescriptについて

Angularを使う上で前提知識としてTypeScriptの知識が必要となる。  
Typescriptを扱う上でJavascriptの知識も必要となるが、ここでは割愛する。  

[JavaScript プログラマーのための TypeScript](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)をベースにまとめていく。

## TypeScriptとは

TypeScriptはJavaScript と独特な関係にあり、JavaScriptのすべての機能を提供し、さらにその上にTypeScriptの型システムという追加レイヤーを備えている。  
たとえば、JavaScriptはstringやnumberのような言語プリミティブを提供するが、これらが一貫して割り当てられているかどうかはチェックしない。TypeScriptはそれらをチェックする。  
既存の動作中のJavaScriptコードもTypeScriptコードになる。TypeScriptの主な利点は、コード内の予期しない動作を強調表示して、バグの可能性を低減できることにある。

### 推論による型

TypeScriptでは多くの場合、型を生成する。
例として変数を作成し、特定の値を割り当てるとその値を型として使用する。

例）  

```typescript
let helloWorld = "Hello World";
let helloWorld: string
```

### 型定義

Javascriptでは一部のデザインパターンでは型推論が難しい。(動的プログラミングをしようするパターンなど)  
このようなケースに対応するために、TypeScriptでは型を定義するInterfaceを提供している。

例えば`name: string`、`id: number`の型をもつオブジェクトを作成するには次のように定義する。

通常の定義

```typescript
const user = {
  name: "Hayes",
  id: 0,
};
```

型定義用のInterface。Userという名前でオブジェクトの型を定義

```typescript
interface User {
  name: string;
  id: number;
}
```

定義した型定義を指示する。

```typescript
const user: User = {
  name: "Hayes",
  id: 0,
};
```

もし指定した型定義と一致しないオブジェクトを指定した場合、警告を表示する。

```typescript
interface User {
  name: string;
  id: number;
}
 
const user: User = {
  username: "Hayes",
`Object literal may only specify known properties, and 'username' does not exist in type 'User'.`
  id: 0,
};
```

Javascriptではクラスとオブジェクト指向をサポートしており、TypeScriptでも同様に使用可能。クラスを用いることでインターフェース宣言をすることができる。

```typescript
interface User {
  name: string;
  id: number;
}
 
class UserAccount {
  name: string;
  id: number;
 
  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}
 
const user: User = new UserAccount("Murphy", 1);
```

インターフェースを使用して、パラメータに注釈を付け、関数に値を返すことができる。

```typescript
function deleteUser(user: User) {
  // ...
}
 
function getAdminUser(): User {
  //...
}
```

JavaScriptで利用可能なプリミティブ型は、すでにboolean、bigint、null、number、string、symbol、undefinedという小さなセットがあり、これらはインターフェイスで利用できる。TypeScriptはこのリストをさらに拡張し、any（何でも許す）、unknown（この型を使用する人が、その型が何であるかを宣言することを保証する）、never（この型が起こる可能性はない）、void（未定義を返す、または戻り値を持たない関数）などがある。

型の構築にはインターフェイスと型の2つの構文があることがわかる。  
通常はインターフェイスを使い、特定の機能が必要な場合は型を使用するとよい。

### 型の構成

単純な型の組み合わせにより、複雑な型を作成することができる。unionsとgenericsと呼ばれる一般的な方法がある。

#### unions

unionsを使用すると型が複数の型のいずれかであることを宣言できる。例えばboolanのtrue/falseのいずれかとして記述ができる。

```typescript
type MyBool = true | false;
```

ユニオン型の一般的な使用例は、値が許可される文字列リテラル数字のセットを記述することにある。

```typescript
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

unionsは異なる型を処理する方法も提供している。例えば、配列または文字列を受け取る関数があるとする。

```typescript
function getLength(obj: string | string[]) {
  return obj.length;
}
```

変数の型を知りたいときは`typeof`を使用する。

|Type|Predicate|
|---|---|
|string | typeof s === "string"|
|number |typeof n === "number" |
|boolean | typeof b === "boolean"|
|undefined |typeof undefined === "undefined"|
|function | typeof f === "function"|
|array |Array.isArray(a)|

例えば、ある関数に文字列を渡すか配列を渡すかによって、異なる値を返すようにすることができる

```typescript
function wrapInArray(obj: string | string[]) {
  if (typeof obj === "string") {
    return [obj];
            
`(parameter) obj: string`
  }
  return obj;
}
```

#### generics

genericsは型に変数を提供する。よくある例は配列でgenericsのない配列は何でも含むことができる。ジェネリックを持つ配列は、配列が含む値を記述することができる。

```typescript
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;
```

genericsを使用する独自の型を宣言することができる

```typescript
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}
 
// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
// ↓翻訳
// この行はTypeScriptに`backpack`という定数があることを伝え、それがどこから来たのかを気にしないようにするためのショートカットである。
declare const backpack: Backpack<string>;
 
// object is a string, because we declared it above as the variable part of Backpack.
// ↓翻訳
// オブジェクトは文字列で、上でBackpackの変数として宣言したからだ。
const object = backpack.get();
 
// Since the backpack variable is a string, you can't pass a number to the add function.
// ↓翻訳
// バックパック変数は文字列なので、add関数に数値を渡すことはできない。
backpack.add(23);
`Argument of type 'number' is not assignable to parameter of type 'string'.`
```

### 構造タイプシステム

TypeScriptの基本原則のひとつは、型チェックは値が持つ形に注目するというものだ。これは「ダック型付け」や「構造型付け」と呼ばれることもある。

構造型システムでは、2つのオブジェクトが同じ形をしていれば、それらは同じ型であるとみなされる。

```typescript
interface Point {
  x: number;
  y: number;
}
 
function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
 
// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
```

point変数がPoint型であることは宣言されていない。しかし、TypeScriptは型チェックでpointの形状とPointの形状を比較する。両者は同じ形をしているので、コードはパスする。

形状の一致は、オブジェクトのフィールドの一部だけが一致すればよい。point変数がPoint型であることは宣言されていない。しかし、TypeScriptは型チェックでpointの形状とPointの形状を比較する。両者は同じ形をしているので、コードはパスする。

形状の一致は、オブジェクトのフィールドの一部だけが一致すればよい。

```typescript
const point3 = { x: 12, y: 26, z: 89 };
logPoint(point3); // logs "12, 26"
 
const rect = { x: 33, y: 3, width: 30, height: 80 };
logPoint(rect); // logs "33, 3"
 
const color = { hex: "#187ABF" };
logPoint(color);
`Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'.`
  `Type '{ hex: string; }' is missing the following properties from type 'Point': x, y`

```

クラスとオブジェクトの形状への適合性に違いはない。

```typescript
class VirtualPoint {
  x: number;
  y: number;
 
  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}
 
const newVPoint = new VirtualPoint(13, 56);
logPoint(newVPoint); // logs "13, 56"
```

オブジェクトやクラスが必要なプロパティをすべて持っている場合、TypeScriptは実装の詳細にかかわらず、それらが一致すると判断する。
