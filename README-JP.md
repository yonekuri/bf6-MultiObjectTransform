# bf6-MultiObjectsTransform
[*Here is the English description of this repository.](https://github.com/yonekuri/bf6-MultiObjectsTransform/blob/main/README.md)

このスクリプトはBF6 Portalでのオブジェクトの移動/任意軸での回転や、それらを合成した運動をサポートします。
さらに、他のオブジェクトとの親子関係を設定してオブジェクトを生成することで複合オブジェクトを作成し、複数のオブジェクトを効率的に動かすことが可能です。<br>
これらの機能を用いれば、BF1の飛行船などの巨大兵器の再現や、オブジェクトのアニメーションによるBF4のレボリューションの再現、ある程度の物理演算を実装すればオブジェクトによるサッカーまで様々な機能が実装できると考えています。
この機能を使用していただけた場合は教えていただけると喜んで見に行きます。

## 使い方
[MultiObjectsTransform.ts](https://github.com/yonekuri/bf6-MultiObjectsTransform/blob/main/MultiObjectsTransform.ts "スクリプト")の内容をスクリプトの末尾にコピー&ペーストしてください。

## 機能
このスクリプトではRuntimeObjectクラスのみを追加します。<br>
以下のように引数を指定してクラスのインスタンスを生成すると、ゲーム内にもオブジェクトがスポーンします。
```typescript
let obj = new RuntimeObject(prefabEnum, pos, offset, axis, angle, scale);
```
各引数の意味は以下の通りです。

`prefabEnum`:

スポーンさせるオブジェクトを指定します。
指定できる内容は現状公式で用意されている`SpawnObject`の引数と同様です。<br>
また、`undefined`を指定することで「空オブジェクト」をスポーンさせることも可能です。
この機能は主に後述するオブジェクトの親子関係を設定する際に使用できます。


`pos: mod.Vector`:

オブジェクトをスポーンさせる位置を指定します。
<br>

`offset: mod.Vector`:

スポーンさせるオブジェクトの位置のオフセットを設定します。<br>
この設定は主にオブジェクトの回転を行う際に重要です。<br>
例えば`RuntimeSpawn_Common.FiringRange_Floor_01`は大きさ20.5×20.5の標準的な板型オブジェクトですが、ゲーム内でのオブジェクトの原点は板の角の部分に設定されています。
これは`SpawnObject`などを使用してオブジェクトを回転させた際に角を中心に回転することを意味し、板の中心などを軸とした回転は公式の関数では不可能です。<br>
`RuntimeSpawn_Common.FiringRange_Floor_01`の例では`offset=mod.CreateVector(-10.25,0,-10.25)`と指定すると後述する`QRotation`などによる回転の中心が板の中心として変更されます。

`axis: mod.Vector, angle: nuber`:<br>
2つの引数でオブジェクトの初期姿勢を指定します。<br>
これはデフォルトの姿勢からの任意軸での回転という形で設定します。<br>
`axis`で回転軸、`angle`でラジアンでの回転角の指定を行います。`angle`に正の値を指定した場合は軸に対して左回転、負の値を指定した場合は右回転を行います。<br>
例えば、`axis=mod.CreateVector(0,1,0), angle=Math.PI/3`を指定した場合、オブジェクトはy軸を中心にデフォルトから30度回転した状態でスポーンします。

`scale: mod.Vector`:<br>
オブジェクトのスケールを指定します。<br>
この引数は省略可能であり、省略した場合にはオブジェクトのデフォルトのスケールである`scale=mod.CreateVector(1,1,1)`が指定されます。<br>
ただし、現在のBattlefield 6のバージョンでは公式で用意されている`SetObjectTransform`関数や`MoveObject`関数などを利用してスケールを変更したオブジェクトを移動すると、オブジェクトの当たり判定のスケールはそのままに、見た目のスケールだけがデフォルトに戻ってしまうというバグが存在しています。そのため現状ではこの引数を使用することはおすすめしません。


## クラスメソッド
```typescript
Move(dpos)
```


```typescript
QRotation(axis, angle, rotCenter)
```


```typescript
ApplyTransform()
```


```typescript
NewChild(prefabEnum, pos, offset, axis, angle, scale)
```
オブジェクトの子として新たにオブジェクトをスポーンさせます。
指定できる引数はインスタンスの生成の際と同じです。


```typescript
Remove()
```

## オブジェクトの親子関係
