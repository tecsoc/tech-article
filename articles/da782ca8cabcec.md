---
title: "CSSでFizzBuzzを書いてみる"
emoji: "🫠"
type: "tech"
topics:
  - "css"
  - "react"
  - "scss"
  - "js"
  - "fizzbuzz"
published: true
published_at: "2023-09-24 23:27"
---

## 導入
~~元ネタを知らないのですが~~最近Fizz、Buzzの話題で持ちきりですね。
ちょっと一風変わったFizzBuzz何かないかな、とずっと考えていて
CSSでFizzBuzzしよう、と思いついたものの
諸々の忙しさでだいぶ乗り遅れてしまいました。
周回遅れのFizzBuzz in CSS、やっていきます💪

## HTML生成
真心込めてdivを大量に手書きすれば良いので、JavaScriptは必要では無いのですが
~~そんな根性はない~~スマートではないので
JavaScriptでdivを生成してしまいましょう。
どんな手段でも良いのですが、私はReactに慣れているのでReactを使いました。

Props（引数）でFizzBuzzする最大値を受け取り
1~最大値分の数のdivを生成します。
これでHTMLは完成です。

```ts
const FizzBuzz: React.FC<FizzBuzzProps> = ( { maximum } ) => {
  const range = Array.from(
    {
      length: maximum
    },
    (_, i) => i
  );
  
  return (
    <div className='fizzbuzzWrapper'>
      {range.map(() => <div />)}
    </div>
  );
};
```

## CSS
さて、ここからが本題です。CSSと言いながら当たり前のようにSCSSを使っています。
今回は、以下の3つの要素を使ってFizzBuzzを実現します。
1. 疑似クラスの[:nth-child](https://developer.mozilla.org/ja/docs/Web/CSS/:nth-child) - n倍の判定を行う
3. 疑似要素の[:before](https://developer.mozilla.org/ja/docs/Web/CSS/::before), [:after](https://developer.mozilla.org/ja/docs/Web/CSS/::after)と[contentプロパティ](https://developer.mozilla.org/ja/docs/Web/CSS/content) - 文字列の表示
4. [CSSカウンター](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_counter_styles/Using_CSS_counters) - 要素数のカウント

```SCSS
.fizzbuzzWrapper {
  counter-reset: fizzbuzz;
  
  div {
    counter-increment: fizzbuzz;
    
    &:after {
      content: counter(fizzbuzz);
    }
    
    &:nth-child(3n) {
      &:before {
        content: "Fizz";
      }
      
      &:after {
        content: initial;
      }
    }
    
    &:nth-child(5n) {
      &:after {
        content: "Buzz";
      }
    }
  }
}

```

## 工夫ポイント
`:nth-child(3n)`と`:nth-child(5n)`と`:nth-child(15n)`で３つ条件を書くこともできたのですが
個人的にあまり好みではなかったので、beforeとafterを併用し
15の倍数のときには、before要素でFizzが、after要素にでBuzzが表示されるようにしてみました。

## まとめ
FizzBuzzシンプルだけど工夫すると楽しい（小並感）
プレビューがてらCodePenも貼っておきます
@[codepen](https://codepen.io/tecsoc/pen/bGOLZRX)