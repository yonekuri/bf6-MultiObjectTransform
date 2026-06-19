# bf6-MultiObjectTransform
[*Here is the English description of this repository.](https://github.com/yonekuri/bf6-MultiObjectsTransform/blob/main/README.md)

このスクリプトは主にBF6 Portalにおける以下の機能をサポートします。
・複数のオブジェクトに対する親子関係の設定
・オブジェクトの移動/任意軸での回転、それらを合成した運動
これによって複数のオブジェクトを効率的に動かすことが可能です。<br>
![movie1](https://github.com/user-attachments/assets/547a08e6-6d3e-495f-9512-9b3a43ade3f4)
![movie2](https://github.com/user-attachments/assets/746ad1b1-8aa3-4610-9405-345af69e7aeb)

これらの機能を用いれば、BF1の飛行船などの巨大兵器の再現や、オブジェクトのアニメーションによるBF4のレボリューションの再現、ある程度の物理演算を実装すればオブジェクトによるサッカーまで様々な機能が実装できると考えています。

## 使い方
[MultiObjectsTransform.ts](https://github.com/yonekuri/bf6-MultiObjectsTransform/blob/main/MultiObjectsTransform.ts "スクリプト")の内容をスクリプトの末尾にコピー&ペーストしてください。

## サンプルコード
[MOT_Sample.ts](https://github.com/yonekuri/bf6-MultiObjectTransform/blob/main/MOT_Sample.ts "スクリプト")を使用することで機能を試すことが可能です。

このサンプルコードではエイムを行いながらジャンプすることで視線の先に6つの板状オブジェクトを組み合わせた立方体が出現します。<br>
しゃがむことで立方体は以下の運動を同時に行います。
* エイムで指定した方向への移動（コード86行目で指定）
* エイムで指定した軸を中心にその場で回転（コード87行目で指定）
* 呼び出したプレイヤーを中心に回転（コード88行目で指定）

## 機能
このライブラリは、複数オブジェクトの変形を管理するTransformableObjectクラス、クォータニオン計算を提供するQuaternions名前空間、およびTransformableObjectクラスに関連する型を定義する同名の名前空間を追加します。
ここでは主にTransformableObjectクラスに含まれるメソッドとプロパティについて説明します。

### メソッド
#### オブジェクトの生成
ライブラリで扱うTransformableObjectオブジェクトは3種類あり、それぞれに生成関数が存在します。
* **createRuntimeObject**
```typescript
createRuntimeObject(prefabEnum, position, rotation, offset, scale)
createRuntimeObject(prefabEnum, position, angle, axis, offset, scale)
```
新たにTransformableObjectオブジェクトを生成します。_
_prefabEnum_

* **createEmptyObject**
```typescript
createEmptyObject(potision, rotation, scale)
createEmptyObject(position, angle, axis, scale)
```
新たに空のTransformableObjectオブジェクトを生成します。
これはGodotにおけるNode3Dオブジェクトに対応します。
主に複数のオブジェクトをまとめて動かす際の親オブジェクトとして使用します。

* **createExistingObject**
```typescript
createExistingObject(object, offset, scale)
```
既にゲーム内に存在するオブジェクトをTransformableObjectオブジェクトとして管理できるようにします。



#### 子オブジェクトの生成
対象のオブジェクトの子オブジェクトとしてTransformableObjectオブジェクトを生成します。
子オブジェクトの仕様は基本的にGodotを参考にしています。

#### オブジェクトの管理

#### オブジェクトの移動・回転

#### ベクトルの変換


### プロパティ

