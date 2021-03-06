# 第７回　統計学
## 検定
* __帰無仮説__ _H<sub>0</sub>_: 否定したい仮説
* __対立仮説__ _H<sub>1</sub>_: 主張した仮説

帰無仮説 _H<sub>0</sub>_ が棄却される → _H<sub>1</sub>_ が採択される

帰無仮説 _H<sub>0</sub>_ が棄却されない → _H<sub>1</sub>_ は採択されない ( _H<sub>1</sub>_ についてはなんとも言えない )

| 結果＼真実 | 帰無仮説が正しい | 対立仮説が正しい |
| --- | --- | --- |
| 帰無仮説を棄却しない<br>対立仮説が正しいとは言えない | 正しい | 第二種の過誤<br>β |
| 帰無仮説を棄却する<br>対立仮説が正しい | 第一種の過誤<br>α | 正しい<br>1-β |

* __第一種の過誤__ : 本当は帰無仮説が正しいのに帰無仮説を棄却してしまう場合
* __第二種の過誤__ : 本当は対立仮説が正しいのに帰無仮説を棄却しない場合

### 検定の手順
1. 仮説の設定
  * 母集団のパラメータまたは分布に関する帰無仮説 _H<sub>0</sub>_ と対立仮説 _H<sub>1</sub>_ を設定する
2. 統計量の決定
  * 適当な統計量 T = T(X<sub>1</sub>, X<sub>2</sub>, ..., X<sub>n</sub>) を選択する
  * 帰無仮説 _H<sub>0</sub>_ の下で統計量 T の分布を決定する
3. 有意基準と棄却域の設定
  * 有意基準（危険率） α の決定
  * 棄却域 W (帰無仮説が棄却される範囲) を決定
  * 右側検定 : _H<sub>0</sub>_ : θ = θ<sub>0</sub>, _H<sub>1</sub>_ : θ ＞ θ<sub>0</sub>　→ W = [b,∞)
  * 左側検定 : _H<sub>0</sub>_ : θ = θ<sub>0</sub>, _H<sub>1</sub>_ : θ ＜ θ<sub>0</sub>　→ W = (-∞,a]
  * 両側検定 : _H<sub>0</sub>_ : θ = θ<sub>0</sub>, _H<sub>1</sub>_ : θ ≠ θ<sub>0</sub>　→ W = (-∞,a]∪[b,∞)
4. 帰無仮説の棄却・採択
  * 有意基準 α で _H<sub>0</sub>_ を棄却(_H<sub>1</sub>_ を採択)or採択(_H<sub>1</sub>_ を棄却しない)
