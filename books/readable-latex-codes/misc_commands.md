---
title: "他のユーザーコマンドの類型"
---

単コマンドと拡張コマンドは, 自然な対応概念を持つ, いわば1軍コマンドである. ここでは, それらほど重要ではなく, 積極的に定義する必要もないが, 時折必要になるマイナーなコマンド類型を紹介する. それらを一言で表すなら, 対応概念との関連が相対的に薄いコマンドである.

## 記法コマンド

対応概念が存在するなどの理由で, 当該分野の内在的対象と見なすことができるが, その記法のバリエーションが様々あり, コマンド引数が自然には定まらないケースがある. その場合は, 特定の記法を1つ選び, それに特化したコマンドを(場合によっては1軍コマンドから派生させて)作ることになる.

典型的には, 関数列の添字の記法揺れ $\{f_n\}_{n=1}^{\infty}$, $\{f_n\}_{n \in \mathbb{N}}$ や, ダミー変数の省略記法 $\sum_{i} c_i e_i$, $\sum_{i=1}^{k} c_i e_i$ などだ. 1つのコマンドで対応しようとすればできるが, 実際にそれをやると引数が不自然になるだけでなく, 互いに相性の悪い引数を持たせることになったり, 引数に適切なデフォルト値・初期値を設定できなくてかえって使い勝手が悪くなったりする.

このような場合は, エレガントな単一の解決法は諦めて, 2つの類似したコマンドの存在を認めてしまった方がマシな場合がある. 1つのコマンドの多機能化を認めるか, 管理対象の数を増やすかという, どちらにせよ苦々しい選択において後者を取ることに当たる.

たとえば, ダミー変数の省略記法で言えば, `\linerCombAbb{coef=c_i, vec=e_i}`, `\linerComb{coef=c_i, vec=e_i}[end=k]` のように, 省略記法特化版と完全版コマンドの両方の存在を認めてしまう. こうすれば, それぞれの引数に適切なデフォルト値を与えることができ, 直観的なものに仕上げやすい.

このように, 特定の記法に特化したコマンドを**記法コマンド**と呼ぶことにする. 単純なコマンド等に比べて文脈依存性が上がっており, 移植性はやや落ちる. 保守性が落ちるかどうかは, その記法が文脈の中でどれだけ安定的な存在かに依る.

## ダミー記号コマンド

### 言葉の定義: ダミー記号

たとえば, `\tilde{f}` $\tilde{f}$ に対応概念がないときがある. 単に `f` と比較するためにチルダがついているだけで, `f` とは関連のない対象を指している場合である. この記号を使うべき理由はなく, `f_1` でも `f^{\prime}` でも `g` でも別に構わないというケースだ. この `\tilde` のような記号を**ダミー記号**と呼ぶことにする.

ある記号がダミー記号であるかは文脈による. 「ある集合 $V$ の部分集合 $V^{\prime}$」と書いたとき, この `\prime` はダミーだが, 「$f$ の導関数 $f^{\prime}$」と書いたときの `\prime` は導関数を対応概念に持つからダミーではない.

### ダミー記号を出力するコマンド

ダミー記号を出力するコマンドを**ダミー記号コマンド**と呼ぶことにする.
1つのダミー記号を出力するダミー記号コマンドは定義してよい.
たとえば, `\tilde` または `\widetilde` の別名として `\~` というダミー記号コマンドを定義してもよいし, 新たなダミー記号 "$\heartsuit$" を付与するダミー記号コマンド `\dmyHeart{f}=f^{\heartsuit}` $f^{\heartsuit}$ を作ってもよい.

また, ダミー記号が複数の記号・要素から構成されていても, その複数で1単位を成すのであれば, それは1つのダミー記号として扱い, やはりコマンドとして定義してよい. たとえば, $f^{\backprime \prime}$ で1つのダミー記号を意図するのであれば, `\dmyPrimes{f}=f^{\backprime \prime}` などとしてダミー記号コマンドを定義してよい.

注意すべきなのは, これがダミー記号を出力するコマンドを気楽に定義してもよい唯一の類型であることだ. 1つのダミー記号を出力するだけのシンプルなダミー記号コマンドは, 実質的に `\prime` や `\tilde` に準じた存在になるので, 害はない. しかし, 後の章で見るように, ダミー記号を含む合成は推奨されない(!).

## 省略コマンド

単コマンドや合成コマンドのような1軍コマンドだけで全てが足りれば幸せだが, 現実には複雑なコードの見た目をどうしても圧縮する必要があるなどの理由で, しょうもないコマンドをスポットで用いざるを得ない場合もある. そういう状況では, その目的専用と割り切った省略コマンドを定義して使おう. どんなコマンドかと言うと, 次のような, 省略したい表現への純粋なショートカットとして用いるコマンドを指す.

```tex
\newcommand{\xxCommand}[n]{省略したい複雑な表現}
```

省略コマンドはコメントで指す対象を宣言して使おう.

```tex
... &= \some \terms \frac{complex}{terms} \\
% 前の行の最後の分数のマクローリン展開の3次の項までを \xxMacExp{a}{x}{f} とおく
    &= \some \terms + \xxMacExp{a}{x}{f}
```

つまり, 板書とかでやる「この部分をAとおく」をコードでやってしまうわけだ.

省略のコマンドはそうでない通常コマンドと区別できるようにprefixを決めておくとよい. 上の例ではxxとした.

また, 省略のコマンドはその場で使い捨てて再利用しないこと, つまり, 影響範囲をソースコードの一部分に限定することが鉄則. 省略コマンド自体は可読性等に難のある"酷い"コマンドなので, 散在させて得なことなど何もない. 必要悪として瞬間的に利用せざるを得ない場面があるだけだ.

数学における典型的な省略コマンドの使い所としては, 後の議論では必要ないが技術的議論の中で一時的に使う記号が挙げられる. たとえば, Riemann 積分の亜種のXX積分を構成するときにしばしば出会う Riemann 和

$$\sum_k f(t^{(n)}_k) ( t^{(n)}_{k+1} - t^{(n)}_k )$$

```tex
\sum_k f(t^{(n)}_k) ( t^{(n)}_{k+1} - t^{(n)}_k )
```

のようなコードは, `\xxt{k}=`$t^{(n)}_k$ という風に省略コマンド `xxt` を定義すれば,

```tex
\sum_k f(\xxt{k}) ( \xxt{k+1} - \xxt{k} )
```

とだいぶマシな見た目になる. (上のコード例だけなら必要ないが, 多くの場合は `\xxt[m]{k}=`$t^{(m)}_{k}$ とできるように, 上付添字の変更オプションを付けておいた方がよいだろう.)
