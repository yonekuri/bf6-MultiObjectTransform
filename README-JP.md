# bf6-MultiObjectTransform
[*Here is the English description of this repository.](https://github.com/yonekuri/bf6-MultiObjectsTransform/blob/main/README.md)

このスクリプトは主にBF6 Portalにおける以下の機能をサポートします。  
・複数のオブジェクトに対する親子関係の設定  
・オブジェクトの移動/任意軸での回転、それらを合成した運動  


これによって複数のオブジェクトを効率的に動かすことが可能です。  
![movie1](https://github.com/user-attachments/assets/547a08e6-6d3e-495f-9512-9b3a43ade3f4)
![movie2](https://github.com/user-attachments/assets/746ad1b1-8aa3-4610-9405-345af69e7aeb)

これらの機能を用いれば、BF1の飛行船などの巨大兵器の再現や、オブジェクトのアニメーションによるBF4のレボリューションの再現、ある程度の物理演算を実装すればオブジェクトによるサッカーまで様々な機能が実装できると考えています。

## 使い方
[MultiObjectsTransform.ts](https://github.com/yonekuri/bf6-MultiObjectsTransform/blob/main/MultiObjectsTransform.ts "スクリプト")の内容をスクリプトの末尾にコピー&ペーストしてください。

## サンプルコード
[MOT_Sample.ts](https://github.com/yonekuri/bf6-MultiObjectTransform/blob/main/MOT_Sample.ts "スクリプト")を使用することで機能を試すことが可能です。

このサンプルコードではエイムを行いながらジャンプすることで視線の先に6つの板状オブジェクトを組み合わせた立方体が出現します。  
しゃがむことで立方体は以下の運動を同時に行います。
* エイムで指定した方向への移動（コード86行目で指定）
* エイムで指定した軸を中心にその場で回転（コード87行目で指定）
* 呼び出したプレイヤーを中心に回転（コード88行目で指定）

## 機能
このライブラリは、複数オブジェクトの変形を管理するTransformableObjectクラス、クォータニオン計算を提供するQuaternions名前空間、およびTransformableObjectクラスに関連する型を定義する同名の名前空間を追加します。  
ここでは主にTransformableObjectクラスに含まれるメソッドとプロパティについて説明します。

### オブジェクトの生成
ライブラリで扱うTransformableObjectオブジェクトは3種類あり、それぞれに生成関数が存在します。
#### **createRuntimeObject**
```typescript
createRuntimeObject(prefabEnum, position, rotation, offset, scale): TransformableObject
createRuntimeObject(prefabEnum, position, angle, axis, offset, scale): TransformableObject
```
新たにTransformableObjectオブジェクトを生成します。  
**_prefabEnum_**  
`RuntimeSpawn_Common.FiringRange_Floor_01`などオブジェクトのprefabEnumを入力します。


**_potision_**  
オブジェクトの生成位置を`mod.Vector`で指定します。  


オブジェクトの姿勢には2通りの指定方法があります。  
1. **`mod.SpawnObject`と同様にオイラー角で指定する方法**  
**_rotation_**  
オブジェクトの初期姿勢を`mod.Vector`で指定します。  
`mod.SpawnObject`と同様の感覚で使用できる、Godotの値をコピーして使用できるなどのメリットがあります。  
Godotの値をコピーする際は事前にRotation OrderをZYXに設定する必要があります。
<p align="center">
<img width="352" height="410" alt="image" src="https://github.com/user-attachments/assets/4c514fbe-2082-45d7-87c5-65d1c9307f3c" />
</p>

3. **回転角と回転軸を指定してオブジェクトの初期姿勢からの回転で指定する方法**  
**_angle_**  
ラジアンでの回転角を`number`で指定します。  
**_axis_**  
回転軸を`mod.Vector`で指定します。  
初期姿勢はオブジェクトのデフォルトの姿勢から_axis_を中心に_angle_だけ回転させた姿勢として決定します。  
この方法は仕組みを理解していればオイラー角よりも直感的に扱うことができます。  


**_offset_**  
スポーンさせるオブジェクトの位置のオフセットを`mod.Vector`で指定します（省略可能）。  
この設定は主にオブジェクトの回転を行う際に重要です。   
例えば`RuntimeSpawn_Common.FiringRange_Floor_01`は大きさ20.5×20.5の標準的な板型オブジェクトですが、ゲーム内でのオブジェクトの原点は板の角の部分に設定されています。
これは`RotateObject`などを使用してオブジェクトを回転させた際に角を中心に回転することを意味し、板の中心などを軸とした回転は公式の関数では不可能です。  
`RuntimeSpawn_Common.FiringRange_Floor_01`の例では`offset=mod.CreateVector(-10.25,0,-10.25)`と指定すると後述する`rotate`などによる回転の中心が板の中心として変更されます。  
省略された場合は`offset=mod.CreateVector(0,0,0)`として扱われます。
<p align="center">
<img width="547" height="322" alt="figure1" src="https://github.com/user-attachments/assets/a44f80ea-03b6-4430-9b3b-5825e87e1698" />
</p>


**_scale_**  
オブジェクトのスケールを`number`で指定します（省略可能）。  
省略された場合は`scale=1`として扱われます。
> ⚠️現在のBattlefield 6のバージョンでは公式で用意されている`SetObjectTransform`関数や`MoveObject`関数などを利用してスケールを変更したオブジェクトを回転させると、見た目の回転がズレるというバグが存在しています。<br>
> そのため現状ではこの引数を使用することはおすすめしません。


#### **createEmptyObject**
```typescript
createEmptyObject(potision, rotation, scale): TransformableObject
createEmptyObject(position, angle, axis, scale): TransformableObject
```
新たに空のTransformableObjectオブジェクトを生成します。
これはGodotにおけるNode3Dオブジェクトに対応します。  
主に複数のオブジェクトをまとめて動かす際の親オブジェクトとして使用します。


####  **createExistingObject**
```typescript
createExistingObject(object, offset, scale): TransformableObject
```
既にゲーム内に存在するオブジェクトをTransformableObjectオブジェクトとして管理できるようにします。  
**_object_**  
対象のオブジェクトを`mod.Object`で指定します。  


### 子オブジェクトの生成
対象オブジェクトの子オブジェクトとしてTransformableObjectオブジェクトを生成します。  
子オブジェクトの仕様は基本的にGodotを参考にしています。  
各引数の意味ははオブジェクト生成関数と同様です。  
ただし、Godotにおける子オブジェクトの仕様に基づいて、**入力された_potision_, _rotation_, _angle_, _axis_は親のローカル座標系における値として解釈されることに注意してください**。  
####  **createRuntimeChild**
```typescript
createRuntimeChild(prefabEnum, position, rotation, offset, scale): TransformableObject
createRuntimeChild(prefabEnum, position, angle, axis, offset, scale): TransformableObject
```
新たに対象の子オブジェクトとしてRuntimeオブジェクトを生成します。  


####  **createEmptyChild**
```typescript
createEmptyChild(potision, rotation, scale): TransformableObject
createEmptyChild(position, angle, axis, scale): TransformableObject
```
新たに対象の子オブジェクトとしてEmptyオブジェクトを生成します。


####  **createExistingChild**
```typescript
createExistingChild(object, offset, scale): TransformableObject
```
既にゲーム内に存在するオブジェクトを対象の子オブジェクトとして管理できるようにします。   


### オブジェクトの管理

### オブジェクトの移動・回転

### ベクトルの変換

### プロパティ

