# bf6-MultiObjectsTransform
このスクリプトはBF6 Portalでのオブジェクトの移動/任意軸での回転や、それらを合成した運動をサポートします。
さらに、他のオブジェクトとの親子関係を設定してオブジェクトを生成することで複合オブジェクトを作成し、複数のオブジェクトを効率的に動かすことが可能です。

これらの機能を用いれば、BF1の飛行船などの巨大兵器の再現や、オブジェクトのアニメーションによるBF4のレボリューションの再現、ある程度の物理演算を実装すればオブジェクトによるサッカーまで様々な機能が実装できると考えています。
この機能を使用していただけた場合は教えていただけると喜んで見に行きます。

## 使い方
[MultiObjectsTransform.ts](https://github.com/yonekuri/bf6-MultiObjectsTransform/blob/main/MultiObjectsTransform.ts "スクリプト")の内容をスクリプトの末尾にコピー&ペーストしてください。

## 機能
このスクリプトではRuntimeObjectクラスを追加します。
以下のように引数を指定してクラスのインスタンスを生成すると、ゲーム内にもオブジェクトがスポーンします。
```typescript
let obj = new RuntimeObject(Enum, pos, offset, axis, angle, scale);
```
