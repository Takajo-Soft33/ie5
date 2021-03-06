# オペレーティングシステムⅡ 2019年度 前期中間試験

## 1 語句に関する問題
　プロセスの起動と終了が繰り返されるうちに使用できない小さなメモリの領域がたくさんできる。この領域は<em>メモリフラグメント</em>と呼ばれる。<em>メモリフラグメント</em>を解消するために、実行中のプロセスをメモリ上で移動し詰め合わせを行う。これは<em>メモリコンパクション</em>と呼ばれる。<em>メモリコンパクション</em>を可能にするハードウェア機構であるリロケーションレジスタは、プロセスの<em>ロードアドレス（Base）</em>と<em>大きさ（Limit）</em>を記録するレジスタである。プロセスの実行を開始する際に値が設定される。プロセスの実行中は<em>ロードアドレス（Base)</em>とCPUの出力するアドレスの和がメモリのアドレスになる。リロケーションレジスタは、プロセスの動的再配置機構としても<em>メモリ保護</em>機構としても働く。
　セグメンテーションでは、セグメントサイズを自由に設定できるので<em>内部</em>フラグメントが生じない。しかし、セグメントの間に<em>外部</em>フラグメントが生じる。セグメントを物理メモリに配置する際は、セグメントサイズ<em>以上</em>の<em>連続</em>した領域が必要になる。セグメントが物理メモリより大きくなることが<em>できない</em>。
　ページングでは、使用するメモリ領域の大きさがページサイズの整数倍でない場合に<em>内部</em>フラグメントを生じる。<em>内部</em>フラグメントを小さくするにはページサイズを<em>小さく</em>すればいいが、これによりページテーブルが<em>大きく</em>なる。ページングはメモリ管理のハードウェアである<em>MMU</em>によるp→f変換により実現される。p→f変換はメモリ上に置いた<em>ページテーブル</em>を参照することによってなされる。変換を行うたびにメモリ上の<em>ページテーブル</em>を参照すると時間がかかるので、一度変換した結果は<em>TLB</em>にキャッシュしておく。通常、<em>ページテーブル</em>の大きさは<em>仮想</em>アドレス空間の大きさに比例して大きくなる。<em>ページテーブル</em>が大きくなりすぎないように、<em>多段</em>の<em>ページテーブル</em>を使用するシステムと逆引き<em>ページテーブル</em>を使用するシステムがある。逆引き<em>ページテーブル</em>の大きさは<em>物理</em>アドレス空間の大きさに比例する。
　ページングを用いて仮想空間を実現することができる。フレームが割り付けられていないページをプロセスがアクセスすると、<em>page fault（ページ不在割り込み）</em>が発生し制御がオペレーティングシステムに移る。オペレーティングシステムがフレームを割り付け、ページをフレームに読み込み、プロセスを再開する。このような、ページが必要になった時点でページを読み込む方式を<em>デマンドページング</em>と呼ぶ。
　forkシステムコールを実行する際に、親プロセスの仮想アドレス空間の内容を子プロセスのそれにコピーする必要がある。フレームをコピーするのではなくフレームを親子プロセスで共有することで、コピーによるオーバーヘッドを小さくすることが可能である。内容が変更されないフレームは単に共有すれば良い。内容が変更されるフレームは、どちらかのプロセスが内容を書き換えようとした時点でコピーを作る。この方式を<em>copy on write（COW）</em>と呼ぶ。
　プログラムの実行中、一部のページにアクセスが集中することがある。短い時間に、連続したアドレスに配置されたページにアクセスが集中するのは、プログラムのメモリアクセスに<em>空間</em>的局所性があるためである。また、あるページに着目すると、ある時間にアクセスが集中するのは、プログラムのメモリアクセスに<em>時間</em>的局所性があるためである。ある時間にアクセスされるページの集合は、その時間における<em>ワーキングセット</em>と呼ばれる。<em>ワーキングセット</em>が大きくなりすぎてメモリに入りきらなくなると、swap-in/swap-outを繰り返しシステムの性能が急激に低下する<em>スラッシング</em>が発生する。

## 2 可変区画方式
　110KiBの空き領域を利用可能な、メモリ管理を可変区画方式で行なっているシステムで、すでに40KiB,10KiB,30KiB,20KiBの領域が割り付けられているとします。メモリマップは以下の通りです。
　<small>注意：OSカーネルは0番地から配置されているものとします。また、以下の間で領域を分割してメモリ割り付けする場合は、0番地に近い側の領域を使用するものとします。</small>

<div class="flex">
![semimid-2-a.png]


![semimid-2-b.png]
</div>

以下の操作を順に行った時のメモリマップを、ファーストフィット方式とベストフィット方式を用いた場合について示しなさい。なお、メモリマップには使用中の領域だけ書き込みなさい。

1. 40KiB, 20KiBの領域を解放後、10KiBの領域を割り付けた。
<div class="flex">
ファーストフィット
<em>![semimid-2-1-ff.png]</em>


ベストフィット
<em>![semimid-2-1-bf.png]</em>
</div>

2. 前の操作に続いて、30KiBの領域を解放後、20KiBの領域を割り付けた。
<div class="flex">
ファーストフィット
<em>![semimid-2-2-ff.png]</em>


ベストフィット
<em>![semimid-2-2-bf.png]</em>
</div>

3. 前の操作に続いて、10KiBの領域を割り付けた後、さらに30KiBの領域を割り付けた。
<div class="flex">
ファーストフィット
<em>![semimid-2-3-ff.png]</em>


ベストフィット
<em>![semimid-2-3-bf.png]</em>
</div>

## 3 セグメンテーション
1. 物理メモリ空間のメモリマップが次の図のようになっている時、下のセグメントテーブルを完成しなさい。なお、格納された値に意味がない項目には-を記入しなさい。

![semimid-3.png]

| No | v | ... | B | L |
| --- | --- | --- | --- | --- |
| 0 | <em>0</em> | ... | <em>-</em> | <em>-</em> |
| 1 | <em>1</em> | ... | <em>0x1000</em> | <em>0x1000</em> |
| 2 | <em>0</em> | ... | <em>-</em> | <em>-</em> |
| 3 | <em>1</em> | ... | <em>0x4000</em> | <em>0x3000</em> |

2. 次の仮想アドレスが変換される物理アドレスを答えなさい。なお、変換できない場合は「変換不可」と答えなさい。

a. 0x0:0x1234

  <em>変換不可</em>

b. 0x1:0x1234

  <em>変換不可</em>

c. 0x3:0x1234

  <em>0x5234</em>

## 4 ページング
　バイトごとにアドレス付され、仮想アドレス空間の大きさが2<sup>26</sup>バイト、物理アドレス空間の大きさが2<sup>32</sup>バイト、２段のページテーブルを用いるシステムあるとします。仮想アドレスは次のようにページ番号（p, q）とページ内アドレス（w）に分割されます。

![semimid-4-a.png]

1. このシステムのページサイズを答えなさい。

<em>2<sup>10</sup>バイト</em>

2. フレーム番号が何ビットになるか答えなさい。

<em>22ビット</em>

3. ページテーブルの１エントリが４バイトの時、１段目のページテーブルの大きさをバイト単位で答えなさい。

<em>1024バイト</em>

4. ２段目のページテーブルを格納するために必要なフレーム数は最大で何フレームになるか答えなさい、なお、２段目のページテーブルの１エントリも４バイトとする。

<em>256フレーム</em>
  
5. ページテーブルが次のような内容の時、次の仮想アドレスが変換される物理アドレスを１６進数で答えなさい。なお、２段目のページテーブルは第３フレームに置かれているものとします。また、変換できない場合は「変換不可能」と記しなさい。

  ![semimid-4-5.png]
  
  a. 0x0000412
  
    <em>変換不可能</em>
    
  b. 0x0040412
  
    <em>0x00001412</em>
    
  c. 0x0040812

    <em>変換不可能</em>


## 5 仮想アドレス空間の配置
　次の図は、ページングを用いるUNIXシステムの仮想アドレス空間のメモリマップ例である。
![semimid-5-a.png]
1. 各部の名称を語群の記号で答えなさい。
  <strong>語群</strong>: （あ）初期化データ（data）, （い）スタック, （う）ヒープ, （え）非初期化データ（bss）, （お）プログラム(text)
  
  | a | b | c | d | e |
  | --- | --- | --- | --- | --- |
  | <em>お</em> | <em>あ</em> | <em>え</em> | <em>う</em> | <em>い</em> |

2. 各ページに最適なメモリ保護モード（RWX)を答えなさい。
  
  | ページ0 | ページ1 | ページ2 | ページ3 | ページ4 |
  | --- | --- | --- | --- | --- |
  | <em>R-X</em> | <em>R--</em> | <em>RW-</em> | <em>RW-</em> | <em>RW-</em> |

<style>
em {
  opacity: 0;
}
em:hover {
  opacity: 1;
}
.flex {
  display: flex;
}
</style>
