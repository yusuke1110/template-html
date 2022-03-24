# HTML/CSSテンプレートとコーディングルール

## このテンプレートについて
- HTMLテンプレートとして[Pug](https://pugjs.org/api/getting-started.html)、CSSプリプロセッサとして[Sass](https://sass-lang.com/)（SCSS記法）を使用しています。
- PugやSassを展開するコンパイラとして、[Prepros](https://prepros.io/)を使用しています。
（ファイル内にある『prepros.config』はPreprosの設定ファイルです。）

### テンプレートに含まれるファイルの構成
```
template-html
├── README.md - このファイル
├── prepros.config - Preprosの設定ファイル
├── src - 展開前のファイル
│   ├── _frame.pug - Pugのテンプレートのベースファイル
│   ├── index.pug - Homeページサンプル
│   ├── inner.pug - 中ページサンプル
│   ├── _include - Pugからincludeするための共通ファイル
│   │   ├── _head.pug
│   │   ├── _header.pug
│   │   ├── _footer.pug
│   │   └── ...
│   └── common - サイト全体に関わる共通ファイル
│       └── sass - Sassファイル
│           ├── _Foundationbase.scss
│           ├── _component.scss
│           └── _frame.scss
└── dist - 展開後のファイル
    ├── index.html - index.pugを展開したもの
    ├── inner.html - inner.pugを展開したもの
    └── common
        ├── css - Sassを展開したもの
        ├── img - 画像
        └── js - javascript
```

### このコーディングルールが目指すもの
1. 複数人でのコーディングを対応可能にする
1. コードの修正、管理をしやすくする
1. 誰でもルールに則ることができる
  管理者であるディレクターによって作業範囲を狭め、ルールを単純化する。
  それにより、必要に応じて社内・外注問わず、作業者の入れ替えを可能にする。
1. ルール自体を都度柔軟に変更できる

## 1. コーディングルール
- CSSの構成は[FLOCSS](https://github.com/hiloki/flocss)がベース  
  FLOCSSとの差異
  - FoundationとLayoutの間にFrameカテゴリーを追加（FLOCSSで言うLayout）
  - Layoutの捉え方の変更
FLOCSSでは「ページを構成するプロジェクト共通のコンテナーブロックのスタイルで、ページ単位で唯一の存在である要素」となっているが、FLOCSSでのLayoutは、コンテナーブロック内の各要素へ適応する枠のことを指す。  
CSSフレームワークで使用されるカラムなどもここに含まれる。
- CSSの設定にはIDを使用しない
- （ほぼ）すべての要素にクラス名を付与する  
ただし、例外的に以下の要素についてはクラス名無しでも良い
  - 装飾用の`span`や`a`、必ず親要素と一緒に使われる`table`に対する`tr` `td`、`ul` `ol`に対する`li`など
  - 本文内でしか使われない要素（`main`タグ内の`h1`〜`h6`、`p`、`table`
  など）
    ※WordPressなどCMSを使用している場合にも有効
- マルチクラスを採用する
- クラス名に接頭辞を付与する事で、記述箇所を明確にする  
  ex.) 接頭辞が`.c_`の場合→『component.css』に記述する

このコーディングルールは、[FLOCSS](https://github.com/hiloki/flocss)の考え方をベースに、Frameレイヤーを追加したCSS構成案です。
Frameを追加する事で、外側のレイヤーと中身のレイヤーがより明確になり、作業分担が行いやすくなります。

1. F: Foundationレイヤー - 要素への直接指定や、normalize.cssなどサイト全体に対する設定
2. F: Frameレイヤー - `header`や`main`、`footer`など全体の枠の設定
3. L: layoutレイヤー - Frame内のカラムなど、部分的な枠の設定
4. O: Objectレイヤー
   1. Component - `button`、`title`、`table`など
   2. Project - `card`、`breadcrumb`、`hamburger`など
   3. Utility - `.clearfix`、`margin`など

![img_css-category](https://user-images.githubusercontent.com/47527392/109655526-b49e6880-7ba6-11eb-9f3a-10bdfe8b20a0.png)

### 1. Foundationレイヤー
normalize.cssの読み込みと、`html`や`body`、`a`要素など、タグに直接設定するものを記述します。  
*Things that will be set directly and tags such as HTML, body and a element will be written.*
このカテゴリーについては要素に直接設定する為、接頭辞から始まりません。  
*Only this category will not begin with the prefix.*
> ex.) `html` / `p` / `a:hover` etc.

なお、WordPressなどのCMSを使用している場合で、管理画面からのクラス名付与がしづらい（クライアント側でのクラス名付与を前提とした運用が難しい）といった理由で、直接要素に対してCSSを指定する必要がある場合は、wordpress.cssなど別のCSSファイルを作成し、そちらに記入する事を推奨します。

```HTML
<!-- WordPressから吐き出されたHTMLの例 -->
<div class="entry-content ">
  <h1>タイトル</h1>
  <p>本文</p>
</div>
```
```Sass
/* wordpress.scss (Sass展開前) */
.entry-content {
  h1 { ... }
  p { ... }
}
```
```css
/* wordpress.css （Sass展開後) */
.entry-content h1 { ... }
.entry-content p { ... }
```

### 2. Frameレイヤー
サイト上の構成の大枠として、ページ内に1度しか出てこないものを記述します。
*We will write about thing the only comes out once on the page as a frame.*

接頭辞はFrameの頭文字を取って`f_`とします。
*Prefix will take the "Frame" first letter and use it as `f_`.*

モディファイヤを使用する場合は接頭辞`has_`をつけ、各設定の直下に記述します。
*When using modifier put the prefix `has_` and write it under each layout.*
[Modifierについて](#Modifierについて)

> ex.) `.f_site` / `.f_site__header` / `.f_site__nav` etc.

基本的にFrameは、`display: grid;`で設定します。

```HTML
<!-- HTMLの例 -->
<div class="f_site">
  <header class="f_site__header"></header>
  <div class="f_site__main"></div>
  <footer class="f_site__footer"></footer>
</div>
```
```css
/* CSSの例 */
.f_site {
  display: grid;
  grid-template:
    "header header header" auto
    ".      main   .     " auto
    "footer footer footer" auto
    /1fr    745px  1fr;
}
.f_site__header { grid-area: header; }
.f_site__main { grid-area: main; }
.f_site__footer { grid-area: footer; }
```

### 3. Layoutレイヤー
再利用可能なコンテンツ内のレイアウトに関するものを記述します。  
フレームはサイト上の構成の大枠として使用するのに対して、レイアウトは各フレーム内で使用するものになります。  
[Bulma](https://bulma.io/)や[Bootstrap](https://getbootstrap.com/)といったCSSフレームワークで使用されるカラムなどもここに含まれます。

接頭辞はLayoutの頭文字を取って`l_`とします。  
*Prefix will take the "Layout" first letter and use it as `l_`.*

モディファイヤを使用する場合は接頭辞`has_`をつけ、各設定の直下に記述します。  
*When using modifier put the prefix `has_` and write it under each layout.*
[Modifierについて](#Modifierについて)

> ex.) `.l_header` / `.l_nav` / `l_section` etc.

このLayoutについては、場合によっては後述するProjectに内包してしまった方がわかり易いかもしれません。  
**Layoutカテゴリーに含まれるのがCSSフレームワークのみという事もありえます。**

### 4. Objectレイヤー
OOCSSのコンセプトを元に、繰り返されるビジュアルパターンをすべてObjectと定義します。  
FLOCSSのObjectレイヤーには以下の3つが含まれます。

- 4-1. Component
- 4-2. Project
- 4-3. Utility

#### 4-1. Component
再利用可能な全ての最小の単位のブロックを記述します。
*All objects that are reusable will be written.*
『最小の単位』とは表示の大小では無く、それ以上ブロックを小さく分割しても意味が無い状態を指します。  
言い換えると、使い回しの可能性があるギリギリの単位の事であり、それ以上分割しても他の場所で使い回す事がない（=意味が無い）という状態になります。  
コンポーネントはサイト内のどこでも使用できるようにするために、幅やマージンはプロジェクトのエレメント（子要素）で調整します。  
コンポーネント自体に幅や一番外側のマージンの設定をしないようにしましょう。（`width: 100%;`は除く）

接頭辞はComponentの頭文字を取って`c_`とします。
*Prefix will take the "Component" first letter and use it as `c_`.*

モディファイヤを使用する場合は接頭辞`is_`をつけ、各コンポーネントの記述の下に記述します。  
*When using modifier put the prefix `is_` and write it under each component.*
[Modifierについて](#Modifierについて)

> `.c_btn` / `.c_breadcrumb` / `.c_hero` / `.c_dropdown` / `.c_thumbnail` etc.

```HTML
<!-- HTMLの例 -->
<a href="#" class="c_btn">標準ボタン</a>
<a href="#" class="c_btn is_blue">青いボタン</a>
```
```css
/* CSSの例 (component.css) */
.c_btn {
  display: inline-block;
  padding: 15px 20px;
  border-radius: 5px;
  background-color: gray;
}
.c_btn.is_blue {
  color: white;
  background-color: blue;
}
```

#### 4-2. Project
コンポーネントやプロジェクトを組み合わせて一つになるものを記述します。  
言い換えると、コンポーネントが1つでも含まれていれば自動的にプロジェクトになります。  
また、プロジェクト内に別のプロジェクトが内包される事も許容します。  
プロジェクトはサイト内のどこでも使用できるようにするために、幅やマージンは親プロジェクトのエレメント（子要素）で調整します。  
プロジェクト自体に幅や一番外側のマージンの設定をしないようにしましょう。（`width: 100%;`は除く）

接頭辞はProjectの頭文字を取って`p_`とします。  
*Prefix will take the "Project" first letter and use it as `p_`.*

モディファイヤを使用する場合は接頭辞`is_`をつけ、各プロジェクトの記述の下に記述します。  
*When using modifier put the prefix `is_` and write it under each component.*

```HTML
<!-- HTMLの例 -->
<div class="p_card">
  <div class="p_card__media"><img src="sample.jpg"></div>
  <div class="p_card__contents">
    <h4 class="c_ttl p_card__ttl">カードタイトル</h4>
    <p class="c_date p_card__date">2019.00.00</p>
  </div>
</div>
```
```CSS
/* CSSの例 (project.css) */
.p_card {
  padding: 10px;
  background-color: white;
}
.p_card__date {
  margin-top: 10px;
}
```
```CSS
/* CSSの例 (component.css) */
.c_date {
  font-size: 10px;
}
```

上記の例では、`.p_card`>`.p_card__contents`の中にコンポーネントである`.c_ttl`と`.c_date`が含まれています。  
そのため、このオブジェクトは自動的にプロジェクトとなります。  
ここで注目すべきは、コンポーネントである`.c_date`にはマージンの指定をしておらず、プロジェクトのエレメントである`.p_card__date`に対して指定しているということです。  
それにより、`.c_date`はマージンの指定されていないピュアなコンポーネントとなり、他の場所でも使用できるブロックとなります。  

プロジェクトなのか、コンポーネントなのかは迷う事が多いと思います。  
その場合は深く考えずにとりあえずプロジェクトとしてしまうことを推奨します。

もし上記の例をコンポーネントと見なした場合は、以下のようになります。

```HTML
<!-- HTMLの例 -->
<div class="c_card">
  <div class="c_card__media"><img src="sample.jpg"></div>
  <div class="c_card__contents">
    <h4 class="c_card__ttl">カードタイトル</h4>
    <p class="c_card__date">2019.00.00</p>
  </div>
</div>
```
```CSS
/* CSSの例 (component.css) */
.c_card {
  padding: 10px;
  background-color: white;
}
.c_card__date {
  margin-top: 10px;
  font-size: 10px;
}
```

この場合、`.c_card__date`は`.c_card`のエレメントとして指定されており、`.c_card__date`単体を他の場所で再利用することはできませんが、CSSとしては記述箇所がcomponent.cssに集約されるため、見通しが良くなるというメリットがあります。  
最初から最適解を導くことは難しいため、コード全体が見えてきたときに改めてプロジェクトとコンポーネントかを判断してください。

#### 4-3. Utility
上記以外のページごとの設定や、位置調整、テキスト装飾などで使用する汎用的なものはUtilityとします。  
ユーティリティには接頭辞`u_`をつけ、『_utility.css』に記述します。  
なお、clearfixもユーティリティに該当しますが、例外的に接頭辞はつけない事とします。
> ex.) .clearfix、.u_mab10、.u_ALcenterなど

### Modifierについて
Layoutの幅の違いや、Componentの色のバリエーションなど、状態違いのものはModifierとします。  
FrameとLayoutのモディファイアには接頭辞`has_`をつけ、layout.cssの各設定の直下に記述します。  
ComponentとProjectのモディファイアには接頭辞`is_`をつけ、component.cssの各設定の直下に記述します。  
よって、接頭辞`l_`と一緒に指定されるのは`has_`のみ、接頭辞`c_`と一緒に指定されるのは`is_`のみとなります。  
意味合いを含まないデザイン違いのものは「type00」といった連番で区別します。  
記述の際は、`.c_`の後にスペース無しで記入するというルールとします。
> ex.) .c_btn.is_blue / .c_form.is_large / .c_logo.is_type01 etc.

### 命名規則
- FLOCSSと同様、[MindBEMding](https://github.com/manabuyasuda/styleguide/blob/master/how-to-bem.md)+[SMACSS](http://smacss.com/ja)の融合ルールを使用する
- クラス名には適切な接頭辞をつけ、接続は`_（アンダースコア）`とする
  - `.f_◯◯`（Frame）
  - `.l_◯◯`（Layout）
  - `.c_◯◯`（Component）
  - `.p_◯◯`（Project）
  - `.u_◯◯`（Utility）
  - `#js_◯◯`（Javascriptで制御する場合）
- 特定のページや特定の幅のみで使用するブロックには、後ろに接尾辞をつけ、接続は`_（アンダースコア）`とする
  - ex.1) `.◯_sm`（Small幅でのみ使用する場合の例）
  - ex.2) `.◯_home`（ホームでのみ使用する場合の例）
  - ex.3) `.◯_sm_home`（Small幅且つホームでのみ使用する場合の例）
- 単語の区切りは`-（ハイフン）`とする。
  - ex.) `.u_stretched-link`
- Modifierについては、`is_○○`、`has_○○`とする（Modifierについては後述）
  - ex.1) `.c_btn.is_narrow`（`.is_narrow`は`.c_btn`のModifier）
  - ex.2) `.l_columns.has_12`（`.has_12`は`.l_columns`のModifier）
- 以下の単語は省略可とする。
  - `nav`（ナビゲーション）
  - `btn`（ボタン）
  - `ttl`（タイトル）
  - `txt`（テキスト）
  - `tbl`（テーブル）
  - `img`（画像）
  - `bnr`（バナー）
  - `ico`（アイコン）
  - `cat`（カテゴリー）
  - `bg`（背景）
  - `sec`（セクション）
  - `glo`（グローバル）
- 例外として、`-wrapper`はブロック要素を囲む必要が出た際の親として使用可能
  - ex.) `.p_○○`を囲みたい場合、外側に`.p_○○-wrapper`を設定する（この場合のwrapperは`.p_○○`ブロックとみなし、CSSは`.p_○○`に記述する）

#### MindBEMdingについて
BEMシステムのシンタックスである、Block、Element、Modifierに分類して命名する方法です。

- Block: コンポーネントの親要素
- Element: Blockの子孫要素
- Modifier: BlockやElementのバリエーション

BlockとElementはアンダースコア2つ、Block/ElementとModifierはハイフン2つで繋ぎます。

```HTML
<!-- MindBEMdingでのHTMLコーディング例 -->
<div class="block">
  <div class="block__element"></div>
  <div class="block__element--modifier"></div>
</div>
```

ただし、FLOCSSのModifierについては、SMACSSのStateパターンの命名規則を利用し、`is_◯◯`というようにします。

```HTML
<!-- FLOCSSでのHTMLコーディング例 -->
<div class="block">
  <div class="block__element"></div>
  <div class="block__element is_modifier"></div>
</div>
```

また、Elementの入れ子は避け、HTML構造上の入れ子関係があっても常にブロック直下のElementであるように書いてください。（[参考](http://getbem.com/faq/#css-nested-elements)）
```css
.block__element1__element2 { ... } /* NG */
.block__element2 { ... } /* OK */
```

### よく使うクラス名の例
以下はあくまでも一例であり、作業者によってはLayoutをProjectで担ったり、ProjectをComponentと判断する場合も十分ありえます。  
重要なのは1つのプロジェクトに対してルールを一貫させる事です。
<table>
  <tr>
    <th>Layer</th>
    <th>Prefix</th>
    <th>Block</th>
    <th>Element</th>
    <th>Modifier</th>
  </tr>
  <tr>
    <td>Frame</td>
    <td>f_</td>
    <td>site</td>
    <td>__header<br>__hero<br>
    __nav<br>
    __container<br>__sidebar<br>__main<br>__footer</td>
    <td>has_○○</td>
  </tr>
  <tr>
    <td rowspan="3">Layout</td>
    <td rowspan="3">l_</td>
    <td>container</td>
    <td>__main（main要素に対して付与）<br>      
      __side（aside要素に対して付与）</td>
    <td rowspan="3">has_○○</td>
  </tr>
  <tr>
    <td>main</td>
    <td>__body（主要部分）<br>__side（付帯部分）</td>
  </tr>
  <tr>
    <td>header<br>nav<br>sidebar<br>footer<br>section<br>content<br>article</td>
    <td>__left（左）<br>__center（左右中央）<br>__right（右）<br>__top（上）<br>__middle（上下中央）<br>__bottom（下）<br>__head（上部）<br>__foot（下部）<br>__inner（内側の）<br>__outer（外側の）<br>__first（1番目の）<br>__second（2番目の）<br>__third（3番目の）<br>__fourth（4番目の）</td>
  </tr>
  <tr>
    <td>Component</td>
    <td>c_</td>
    <td>ttl<br>date<br>time<br>logo<br>nav<br>txt<br>hero<br>hamburger<br>back-to-top<br>search-box<br>btn<br>badge<br>label<br>tag<br>cta<br>dropdown<br>accordion<br>table</td>
    <td>__○○</td>
    <td>is_ +<br>show（見せる）<br>hide（隠す）<br>open（開く）<br>close（閉じる）<br>current（現在の）<br>active（有効な）<br>disabled（無効）</td>
  </tr>
  <tr>
    <td>Project<br></td>
    <td>p_</td>
    <td>header<br>footer<br>card<br>profile<br>gellery<br>article<br>sidebar</td>
    <td>__ttl<br>__media<br>__contents<br>__[number]</td>
    <td>is_ +<br>vertical（垂直）<br>horizontal（水平）</td>
  </tr>
</table>

参考: [CSSのクラス名を決めるときに使うリストをつくりました](https://qiita.com/manabuyasuda/items/dbb76ed36970bec95470#state)

## 補足
### コンポーネントの粒度について
前述のとおり、最初から最適な分類にする事は難しいです。  
特にコンポーネントとプロジェクトは判断が難しい場合も多いのですが、もし迷った場合はブロックごとプロジェクトにすることを推奨します。  
全体の構成が見えてきて、再利用可能にした方が良いオブジェクトが出てきたときにコンポーネントに変更しましょう。

### ブロックにするか、エレメントにするか
以下はカードにおけるタイトル部分へのクラス名付与の例です。  
※ここでのタイトルは、カード内でのみ使われるデザインとします。
```HTML
<!-- HTML 例1 -->
<div class="p_card">
  <div class="p_card__media"><img src="sample.jpg"></div>
  <div class="p_card__contents">
    <h4 class="c_ttl-card">カードタイトル</h4> ←（＊）
    <p class="p_card__date">2019.00.00</p>
  </div>
</div>
```
```HTML
<!-- HTML 例2 -->
<div class="p_card">
  <div class="p_card__media"><img src="sample.jpg"></div>
  <div class="p_card__contents">
    <h4 class="p_card__ttl">カードタイトル</h4> ←（＊）
    <p class="p_card__date">2019.00.00</p>
  </div>
</div>
```

＊印をつけた箇所に着目して見てください。
- `c_ttl-card` ←OK
- `p_card__ttl` ←Good

これは考え方によってどちらもありうるのですが、例1のタイトル（`c_ttl-card`）はブロック、例2のタイトル（`c_card__ttl`）はエレメントとして捉えられています。

ブロック要素である`c_ttl-card`は他の場所でも使えるということを示唆しているのですが、クラス名として`card`という名称が使われており、意味合いとしてはカードブロック内でしか使えないものという事になります。（ルールには合致するが、名前から考えると使えない。）

そのため、この例のようにカード内でしか使われないタイトルという事であれば、カードブロック内のタイトルは`p_card`のエレメントとし、`p_card__ttl`とする方が適切ということになります。

カード内のタイトルが他の箇所に出てくるタイトルと同じもので、一つのブロックとして扱うのであれば、自然と意味的な名前（ここで言うcard）が含まれないものになるハズです。

```HTML
<!-- HTML -->
<div class="p_card">
  <div class="p_card__media"><img src="sample.jpg"></div>
  <div class="p_card__contents">
    <h4 class="c_ttl p_card__ttl">カードタイトル</h4> ←（c_ttl）
    <p class="c_date p_card__date">2019.00.00</p> ←（c_date）
  </div>
</div>
```
### 命名における意味合いを考える
もう一つ、カードを例にしてクラス名付与の例を見てみます。
```HTML
<!-- HTML 例1 -->
<div class="p_card-price">
〜中略〜
</div>
```
```HTML
<!-- HTML 例2 -->
<div class="p_price-card">
〜中略〜
</div>
```
`p_card-price`か`p_price-card`かの違いで、この例も考え方によってはどちらもありえます。

一見同じ意味にも捉えられますが、`p_card-price`は、「カードというカテゴリーの中の一つ」という意味で、`p_price-card`は、「料金表というカテゴリーの中のカード」という意味になります。

どちらをカテゴリーの親として捉えるかという事ですが、以下の特徴があると考えられます。（SASSのパーシャル機能でブロックごとにファイルを分けるという前提です。）

#### 例1『カードというパーツ』を主体ととらえた場合
見た目からパーツ名を連想できるものについてはクラス名が探しやすい。
```
- p_card-a.scss
- p_card-b.scss
- p_card-price.scss
- p_card-z.scss
```
#### 例2『特定の箇所』を主体ととらえた場合
特定の箇所のファイルが並ぶ事になり、特定の箇所ごとにグループ化される。
```
- p_price-abcde.scss
- p_price-card.scss
- p_price-list.scss
- p_price-table.scss
```
これらはどちらの方が良いというのは無いので、プロジェクトごとに決めてください。（但しどちらかに統一するのが適切。）
## まとめ
これまで何度か出てきたように、コンポーネントの粒度（どのレイヤーに属するか）は作業者の判断に委ねられます。  
あまり深く考えすぎずに、少し落ち着いたタイミングで都度リファクタリングをする、くらいの軽い気持ちで進めてください。