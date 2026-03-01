# bf6-MultiObjectsTransform
このスクリプトはBF6 Portalでのオブジェクトの移動/任意軸での回転や、それらを合成した運動をサポートします。
さらに、他のオブジェクトとの親子関係を設定してオブジェクトを生成することで複合オブジェクトを作成し、複数のオブジェクトを効率的に動かすことが可能です。

これらの機能を用いれば、BF1の飛行船などの巨大兵器の再現や、オブジェクトのアニメーションによるBF4のレボリューションの再現、ある程度の物理演算を実装すればオブジェクトによるサッカーまで様々な機能が実装できると考えています。
この機能を使用していただけた場合は教えていただけると喜んで見に行きます。

## 使い方
[MultiObjectsTransform.ts](https://github.com/yonekuri/bf6-MultiObjectsTransform/blob/main/MultiObjectsTransform.ts "スクリプト")の内容をスクリプトの末尾にコピー&ペーストしてください。

## 機能
このスクリプトではRuntimeObjectクラスのみを追加します。

以下のように引数を指定してクラスのインスタンスを生成すると、ゲーム内にもオブジェクトがスポーンします。
```typescript
let obj = new RuntimeObject(prefabEnum, pos, offset, axis, angle, scale);
```
`prefabEnum`

スポーンさせるオブジェクトを指定します。
指定できる内容は現状公式で用意されている`SpawnObject`の引数と同様です。

また、`undefined`を指定することで「空オブジェクト」をスポーンさせることも可能です。
この機能は主に後述するオブジェクトの親子関係を設定する際に使用できます。

`pos: mod.Vector`


`offset: mod.Vector`


`axis: mod.Vector`


`angle: nuber`


`scale: mod.Vector`


## クラスメソッド
```
Move(dpos)
```


```
QRotation(axis, angle, rotCenter)
```


```
ApplyTransform()
```


```
newChild(prefabEnum, pos, offset, axis, angle, scale)
```
オブジェクトの子として新たにオブジェクトをスポーンさせます。
指定できる引数はインスタンスの生成の際と同じです。


```
Remove()
```

## オブジェクトの親子関係
