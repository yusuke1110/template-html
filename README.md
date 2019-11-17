# BFLOCSSコーディングルール
B: Base  
F: Frame  
L: layout  
O: Object  
CSS

## このコーディングルールが目指すもの
1. 複数人でのコーディングを対応可能にする
1. コードの修正、管理をしやすくする  
Layout部分はディレクターが管理し、Object部分はスタッフが行う想定。  
最後のフィニッシュ作業はコードを整理しながらディレクターがまとめる想定。  
ルールで縛るのではなく、ディレクターにより作業範囲を狭め、スタッフには必要な作業に集中してもらう。  
全体で作業スピードを上げ、フィニッシュでクオリティを上げる。
1. 誰でもルールに則ることができる  
上述のとおり、管理者であるディレクターによって作業範囲を狭め、ルールを単純化する。  
それにより、必要に応じて社内・外注問わず、作業者の入れ替えを可能にする。
1. ルール自体を都度柔軟に変更できる
1. 使用するツールを限定しない  
ツールを使用することにより、効率化を図ることができるが、使用しないことにより作業ができなくなるということは無い様にする。  
分業する場合、ディレクターは後述するSassを使用する必要があるが、それ以外のスタッフにはそれを強要しない。

## コーディングルール概要
- CSSの構成は[FLOCSS](https://github.com/hiloki/flocss)がベース  
FLOCSSとの違い
  - Foundationの呼び名をBaseに変更
  - Frameカテゴリーを追加（FLOCSSで言うLayout）
  - Layoutの捉え方  
FLOCSSでは「ページを構成するプロジェクト共通のコンテナーブロックのスタイルで、ページ単位で唯一の存在である要素」となっているが、ここではコンテナーブロック内の各要素へ適応するものとする。
- CSSの設定にはIDを使用しない
- 多くの要素にクラス名を付与する  
ただし、以下の要素についてはクラス名無しでも良い
  - 装飾用のspanやa、必ず親要素と一緒に使われるtableに対するtr/td、ul/olに対するliなど
  - 本文内でしか使われない要素（mainタグ内のh1〜h6、p、tableなど）
- マルチクラスを採用する
- 設定の記述箇所を明確にする
- Sassを使用する  
変数での色の管理や、パーツごとのファイル管理が行いやすいため、Sassを使用する。  
ただし無理にSassらしい書き方にする必要は無く、従来のCSSの書き方でも良い。  
できるだけ小さな単位のファイルとする事で、記述箇所を明確にしたい為、[パーシャル](https://qiita.com/k_momotani/items/3a728e8256047377f1fa)の機能を使用する。

## 命名規則
- FLOCSSと同様、基本は[MindBEMding](https://github.com/manabuyasuda/styleguide/blob/master/how-to-bem.md)を使用する
- 適切な接頭辞をつける（後述）
- 接頭辞/接尾辞の接続は「_（アンダースコア）」とする。
- 単語の区切りは「-（ハイフン）」とする。  
- 特定のページのみで使用するブロックには、後ろに接尾辞をつける  
  ex.) f_site_home
- Modifierについては、【is_○○】【has_○○】とする（後述）  
- 以下の単語は省略可とする。  
  - nav（ナビゲーション）
  - btn（ボタン）
  - ttl（タイトル）
  - img（画像）
  - bnr（バナー）
  - icn（アイコン）
  - cat（カテゴリー）
  - bg（背景）
  - sec（セクション）
  - glo（グローバル）

## CSSファイルの構成
### 1. Baseカテゴリー
- normalize.css
- base.css

### 2. Frameカテゴリー
- frame.css

### 3. Layoutカテゴリー
- layout.css

### 4. Objectカテゴリー
- component.css
- project.css
- utility.css

## カテゴリーごとの説明
カテゴリーは以下の4つにになります。

### 1. Baseカテゴリー
normalize.cssの読み込みと、htmlやbody、a要素など、タグに直接設定するものを記述します。  
*Things that will be set directly and tags such as HTML, body and a element will be written.*  
このカテゴリーのみ、要素に直接設定する為、接頭辞から始まりません。  
*Only this category will not begin with the prefix.*  
なお、WordPressのエディタから入力する必要があり、クラス名が付与できない場合で、直接要素に対してCSSを指定する必要がある場合は、wordpress.cssなど別のCSSファイルを作成し、そちらに記入してください。
> ex.) html / p / a:hover etc.

### 2. Frameカテゴリー
サイト上の構成の大枠としてページ内に1度しか出てこないものを記述します。  
*We will write about thing the only comes out once on the page as a frame.*  
基本的にこのカテゴリーでは、Gridレイアウトに関する設定のみを行います。  
接頭辞はFrameの頭文字を取って【f_】とします。  
*Prefix will take the "Frame" first letter and use it as "f_".*

モディファイヤを使用する場合は接頭辞【has_】をつけ、各設定の直下に記述します。  
*When using modifier put the prefix "has_" and write it under each layout.*  
[Modifierについて](#Modifierについて)

ここで指定するのは以下の様なものと予想されます。  
*You might use the following.*  
> ex.) .f_site / .f_site\__header / f_site\__nav etc.

### 3. Layoutカテゴリー
再利用可能なコンテンツ内のレイアウトに関するものを記述します。  
フレームはサイト上の構成の大枠として使用するのに対して、レイアウトは各フレーム内で使用するものになります。  
コンテンツ内で使用する場合、sectionには"l_primary"など順序を表す名前、divには"l_large"など大きさを表す名前を使用することを推奨します。  
接頭辞はLayoutの頭文字を取って【l_】とします。  
*Prefix will take the "Layout" first letter and use it as "l_".*

モディファイヤを使用する場合は接頭辞【has_】をつけ、各設定の直下に記述します。  
*When using modifier put the prefix "has_" and write it under each layout.*  
[Modifierについて](#Modifierについて)

ここで指定するのは以下の様なものと予想されます。  
*You might use the following.*  
> ex.) .l_header / .l_nav / l_section etc.

### 4. Objectカテゴリー
#### 4-1. Component
再利用可能な全ての最小の単位のオブジェクトを記述します。  
*All objects that are reusable will be written.*  
『最小の単位』とは、表示の大小では無く、それ以上オブジェクトを小さく分割しても、意味が無い（そこ以外で使用する事が無い）状態を指します。  
コンポーネント同士の幅はProjectで調整するので、外側のマージンはつけません。  
ただし、本文内に使用する要素には、マージンをつけても良いこととします。  
接頭辞はComponentの頭文字を取って【c_】とします。  
*Prefix will take the "Component" first letter and use it as "c_".*  

モディファイヤを使用する場合は接頭辞【is_】をつけ、各コンポーネントの記述の下に記述します。  
*When using modifier put the prefix "is_" and write it under each component.*  
[Modifierについて](#Modifierについて)

ここで指定するのは以下の様なものと予想されます。  
*You might use the following.*  
> .c_btn / .c_breadcrumb / .c_hero / .c_serch / .c_dropdown / .c_thumbnail etc.

```HTML
<!-- HTML -->
<a href="#" class="c_btn">標準ボタン</a>
<a href="#" class="c_btn is_blue">青いボタン</a>
```
```css
/* CSS (component.css) */
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
コンポーネントを組み合わせて一つになるものを記述します。  
言い換えると、コンポーネントが1つでも含まれていればプロジェクトになります。  
プロジェクト同士の幅はLayoutで調整するので、幅の指定や外側のマージンはつけません。

接頭辞はProjectの頭文字を取って【p_】とします。  
*Prefix will take the "Project" first letter and use it as "p_".*

モディファイヤを使用する場合は接頭辞【is_】をつけ、各プロジェクトの記述の下に記述します。  
*When using modifier put the prefix "is_" and write it under each component.*

```HTML
<!-- HTML -->
<div class="p_card">
  <div class="p_card__head"><img src="sample.jpg"></div>
  <div class="p_card__foot">
    <h4 class="c_ttl p_card__ttl">カードタイトル</h4>
    <p class="c_date p_card__date">2019.00.00</p>
  </div>
</div>
```
```CSS
/* CSS (project.css) */
.p_card {
  padding: 10px;
  background-color: white;
}
.p_card__head {
  width: 100%;
  height: 140px;
}
.p_card__date {
  margin-top: 10px;
}
```
```CSS
/* CSS (component.css) */
.c_date {
  font-size: 10px;
}
```

上記の例では、【p_card\__foot】の中にコンポーネントである【c_ttl】と【c_date】が含まれています。  
そのため、このオブジェクトは自動的にプロジェクトとなります。  
ここで注意すべきは、コンポーネントである【c_date】にはマージンの指定をしておらず、プロジェクトのエレメントである【p_card\__date】に対して指定しているということです。  
それにより、【c_date】は他の場所でも使用できるブロックとなります。

プロジェクトなのか、コンポーネントなのかは迷う事が多いと思います。  
その場合はプロジェクトとすることを推奨します。

もし上記の例をコンポーネントと見なした場合は、以下のようになります。

```HTML
<!-- HTML -->
<div class="c_card">
  <div class="c_card__head"><img src="sample.jpg"></div>
  <div class="c_card__foot">
    <h4 class="c_card__ttl">カードタイトル</h4>
    <p class="c_card__date">2019.00.00</p>
  </div>
</div>
```
```CSS
/* CSS */
.c_card {
  padding: 10px;
  background-color: white;
}
.c_card__head {
  width: 100%;
  height: 140px;
}
.c_card__date {
  margin-top: 10px;
  font-size: 10px;
}
```

この場合、【c_card\__date】は【c_card】のエレメントとして指定されており、【c_card\__date】単体を他の場所で再利用することはできませんが、CSSとしては記述箇所がcomponent.cssに集約されるため、見通しが良くなるというメリットがあります。  
最初から最適解を導くことは難しいため、コード全体が見えてきたときに、プロジェクトとコンポーネントかを改めて判断してください。

#### 4-3. Utility
上記以外のページごとの設定や、位置調整、テキスト装飾などで使用する汎用的なものとはUtilityとします。  
ユーティリティには接頭辞【u_】をつけ、style.cssに記述します。  
clearfixもstyle.cssに記述しますが、例外的にclearfixには接頭辞はつけない事とします。  
> ex.) .clearfix、.u_mab10、.u_ALcenterなど  

### Modifierについて
Layoutの幅の違いや、Componentの色のバリエーションなど、状態違いのものはModifierとします。  
Layoutのモディファイアには接頭辞【has_】をつけ、layout.cssの各設定の直下に記述します。  
ComponentとProjectのモディファイアには接頭辞【is_】をつけ、component.cssの各設定の直下に記述します。  
よって、接頭辞【l_】と一緒に指定されるのは【has_】のみ、接頭辞【c_】と一緒に指定されるのは【is_】のみとなります。  
意味合いを含まないデザイン違いのものは「type00」といった連番で区別します。    
記述の際は、【.c_】の後にスペース無しで記入するというルールとします。  
> ex.) .c_btn.is_blue / .c_form.is_large / .c_logo.is_type01 etc.

## よく使うクラス名の例
<table>
  <tr>
    <th>Prefix</th>
    <th>Block</th>
    <th>Element</th>
    <th>Modifier</th>
  </tr>
  <tr>
    <td>f_<br>(Frame)</td>
    <td>site</td>
    <td>\_\_header<br>\_\_hero<br>\_\_nav<br>\_\_container<br>\_\_sidebar<br>\_\_main<br>\_\_footer</td>
    <td>has_○○</td>
  </tr>
  <tr>
    <td>l_<br>(Layout)</td>
    <td>header<br>nav<br>container<br>sidebar<br>main<br>footer<br>section<br>content<br>article</td>
    <td>\_\_left（左）<br>\_\_center（左右中央）<br>\_\_right（右）<br>\_\_top（上）<br>\_\_middle（上下中央）<br>\_\_bottom（下）<br>\_\_head（上部）<br>\_\_body（主要部分）<br>\_\_foot（下部）<br>\_\_inner（内側の）<br>\_\_outer（外側の）<br>\_\_first（1番目の）<br>\_\_second（2番目の）<br>\_\_third（3番目の）<br>\_\_fourth（4番目の）</td>
    <td>has_○○</td>
  </tr>
  <tr>
    <td>c_<br>(Component)</td>
    <td>ttl<br>date<br>time<br>logo<br>nav<br>txt<br>hero<br>hamburger<br>back-to-top<br>search-box<br>btn<br>badge<br>label<br>tag<br>cta<br>dropdown<br>accordion<br>table</td>
    <td>\_\_○○<br>Compoentを最小の単位とするならば、<br>Elementはあまり使われないハズ</td>
    <td>is_ +<br>show（見せる）<br>hide（隠す）<br>open（開く）<br>close（閉じる）<br>current（現在の）<br>active（有効な）<br>disabled（無効）</td>
  </tr>
  <tr>
    <td>p_<br>(Project)<br>作業者によっては<br>Componentと判断する<br>場合も有り</td>
    <td>header<br>footer<br>card<br>profile<br>gellery<br>article</td>
    <td>\_\_01<br>\_\_02<br>以下同…<br>名前を考えるのが大変なので、<br>プロジェクトのエレメントは数字を使用する。</td>
    <td>is_ +<br>vertical（垂直）<br>horizontal（水平）</td>
  </tr>
</table>


参考: [CSSのクラス名を決めるときに使うリストをつくりました](https://qiita.com/manabuyasuda/items/dbb76ed36970bec95470#state)

## 補足
### コンポーネントの粒度について
前述のとおり、最初から全てうまく行くようにつくることは難しいため、どのカテゴリーにするか迷うことがあるかと思います。  
特にコンポーネントとプロジェクトは判断が難しい場合も多いのですが、もし迷った場合はブロックごとプロジェクトにすることを推奨します。  
全体の構成が見えてきて、再利用可能にした方が良いオブジェクトが出てきたときにコンポーネントに変更しましょう。
ルール通りにコーディングしていれば、リファクタリングも容易なはずです。

### エレメントについて
以下の例は、タイトル部分へのクラス名付与の例です。
```HTML
<!-- HTML 例1 -->
<div class="p_card">
  <div class="p_card__head"><img src="sample.jpg"></div>
  <div class="p_card__foot">
    <h4 class="c_ttl-card">カードタイトル</h4> ←（＊）
    <p class="p_card__date">2019.00.00</p>
  </div>
</div>
```
```HTML
<!-- HTML 例2 -->
<div class="c_card">
  <div class="c_card__head"><img src="sample.jpg"></div>
  <div class="c_card__foot">
    <h4 class="c_card__ttl">カードタイトル</h4> ←（＊）
    <p class="c_card__date">2019.00.00</p>
  </div>
</div>
```

＊印をつけた箇所
- c_ttl-card ←OK
- c_card__ttl ←Good

これは考え方によってどちらもあり得るのですが、【c_ttl-card】は、「数あるタイトルのデザインのうちのひとつで、カードブロックの中で使われるもの」という意味で、【c_card__ttl】は、「カードブロックの中で使われているタイトル」という意味になります。  
一見同じ意味にも捉えられますが、【c_ttl-card】はBEMのルール的にはブロック要素であり、他の場所でも使えるということを示唆しているのですが、クラス名として、【card】という名称が使われており、意味合いとしては、カードブロック内でしか使えないものとなります（使えないことは無いのですが、あとから見た時に混乱のもととなると思います）。  
そのため、この例で言えば、カードブロック内のタイトルは【c_card】のエレメントとし、【c_card_ttl】とする方が適切ということになります。  
タイトルをブロックとして扱うのであれば、先に出てきたように、意味的な名前（ここで言うcard）を含まないものにするのが、CSS内の設定の一覧性を高めることに繋がります。

```HTML
<!-- HTML -->
<div class="p_card">
  <div class="p_card__head"><img src="sample.jpg"></div>
  <div class="p_card__foot">
    <h4 class="c_ttl p_card__ttl">カードタイトル</h4>
    <p class="c_date p_card__date">2019.00.00</p>
  </div>
</div>
```
