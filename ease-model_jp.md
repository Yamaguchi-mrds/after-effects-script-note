# After Effects キーフレーム イージングの数学モデル

## 1. 基本概念：Speed と Influence

Adobe After Effects では、**スピードグラフ**はプロパティの値の時間変化率（速度）を表す。
各キーフレームには以下の2つのパラメータがある：

* **speed（速度）**

  * グラフの傾き（単位はプロパティによって異なる値/秒、例：px/sec、°/sec）
  * 値変化の急さを決定する
* **influence（影響度）**

  * 時間方向のハンドル長さ（%）
  * 高いほど、隣接キーフレーム間隔に対するハンドル長（時間方向の比率）が長くなり、加速・減速の範囲が広がる

**例：**
デフォルトの「イージーイーズ」では、入出速度0・影響度33.33%で、ほぼ正弦波に近いS字カーブが得られる。

---

## 2. Speed と Influence がベジェハンドルに与える影響

```
時間 →
  [Key1]●───╮
             ╰─────●[Key2]

     ↑ 横方向距離 ＝ 区間時間 × (influence / 100)
     ↑ 縦方向距離 ＝ speed × 横方向距離
```

* **横方向距離（時間軸）** = 区間長 × (influence ÷ 100)
* **縦方向距離（値軸）** = speed × 横方向距離

この2つでハンドルの角度が決まり、イーズカーブ形状が決定する。

---

## 3. 数学モデル（エルミート補間）

区間を 0〜1 に正規化したパラメータ **u** を使った補間式：

```jsx
p(u) = (2u^3 - 3u^2 + 1) * p0 
     + (u^3 - 2u^2 + u)   * m0
     + (-2u^3 + 3u^2)     * p1
     + (u^3 - u^2)        * m1
```

* p0, p1 = キーフレームの値
* m0, m1 = 接線（tangent）
  - m0 = speed_out × 区間時間（秒単位の速度を、値単位の接線長に変換）
  - m1 = speed_in  × 区間時間（同上）

この式で、与えられた速度と影響度から滑らかなS字カーブを生成できる。

---

## 4. ExtendScript 補間関数例（安全版）

```jsx
function interpolateEase(val1, val2, t1, t2, vOut1, vIn2) {
    // time: 現在の評価時刻（秒）
    var dt = t2 - t1;
    var u = (time - t1) / dt; 
    var m0 = vOut1 * dt;
    var m1 = vIn2 * dt;
    var h00 =  2*u*u*u - 3*u*u + 1;
    var h10 =      u*u*u - 2*u*u + u;
    var h01 = -2*u*u*u + 3*u*u;
    var h11 =      u*u*u -   u*u;
    return h00*val1 + h10*m0 + h01*val2 + h11*m1;
}
```

---

## 5. まとめ図

```
値軸 ↑
     |
p1   |                     ●[Key2]
     |                  ／
     |               ／
     |            ／
p0   |●[Key1]───╯
     +--------------------------------→ 時間軸
          ← influence → 
     speed：縦方向の立ち上がり
     influence：横方向の伸び
```

---

## 参考リンク

* [Adobe After Effects Scripting Guide – KeyframeEase](https://ae-scripting.docsforadobe.dev/other/keyframeease.html)
* [Adobe公式 ヘルプ – キーフレーム補間](https://helpx.adobe.com/jp/after-effects/using/keyframe-interpolation.html)



