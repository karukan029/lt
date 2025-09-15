---
theme: default
background: linear-gradient(135deg, #1e3a8a 0%, #3730a3 50%, #581c87 100%)
class: text-center text-white
highlighter: shiki
lineNumbers: false
info: |
  ## tsgoを触ってみて得た学び
drawings:
  persist: false
transition: slide-left
title: tsgoを触ってみて得た学び
mdc: true
---

# tsgoを触ってみて得た学び

<div class="pt-8">
  <div class="text-xl opacity-90">
    かるカン
  </div>
</div>

---
transition: slide-left
---

# 自己紹介

<div class="grid grid-cols-2 gap-8 mt-24">

<div class="flex justify-center items-center">
<img src="./twitter_icon_400x400.jpeg" class="w-40 h-40 rounded-full border-4 border-white shadow-lg" alt="プロフィール画像" />
</div>

<div>

<h3>👋 かるカン</h3>

- **X (Twitter)**: @karukan013L23
- フロントエンドエンジニア
- 最近はTypeScript/Reactを書いてます
- コーヒーが好き

</div>

</div>

---
transition: slide-left
layout: center
class: text-center
---

<div class="text-center">
<h1 class="text-6xl mb-8">
tsgo、触ってみましたか？
</h1>

<div v-click class="text-2xl mt-12">
<div class="flex items-center justify-center mb-4">
<span>はい！触ってみました！</span>
</div>

</div>
</div>

---
transition: slide-left
---

# typescript-go とは？
- TypeScriptをGoで実装し直したバージョン
  - **Strada**: v6まで（従来のTS）
  - **Corsa**: v7以降（Go版のTS）
- 現在はプレビュー版として一部機能が提供されている
  - LSP
    - VSCode向けに拡張機能が提供されている
      - importの補完などまだ一部動作しない
  - コンパイラの型チェック
    - ↑これを試しに触ってみた

---
transition: slide-left
---

# なぜts-goが必要なのか？

TypeScriptエコシステムの現状とボトルネック

<div class="grid grid-cols-3 gap-6 mt-8">

<div>
<h3>型チェック</h3>

- **tsc** のみ
- 代替手段なし
- ボトルネックになりがち

</div>

<div>
<h3>コンパイル</h3>

- **tsc**
- **esbuild** 🚀
- **SWC** 🚀
- 高速化の選択肢あり

</div>

<div>
<h3>Language Server</h3>

- **ts-server**
- エディタ支援
- LLMコード生成時の遅延

</div>

</div>

<div class="mt-8 p-4 bg-red-900 text-white border-l-4 border-red-400 rounded">
💡 <strong>課題:</strong> 型チェックの高速化が唯一の未解決領域
</div>

---
transition: slide-left
---

# 実際に試してみた結果

## 小規模プロジェクト (ポモドーロタイマー)

<div class="grid grid-cols-2 gap-8 mt-6">

<div>
<h4>tsc</h4>

```
Files: 789
Lines: 190,287
Total time: 2.23s
```

</div>

<div>
<h4>ts-go</h4>

```
Files: 789
Lines: 190,287
Total time: 0.710s
```

</div>

</div>

<div class="text-center mt-8">
<div class="text-4xl font-bold text-green-600">3.14x faster!</div>
</div>

---
transition: slide-left
---

# 大規模プロジェクト (VSCode)

<div class="grid grid-cols-2 gap-8 mt-6">

<div>
<h4>tsc</h4>

```
Files: 5,109
Lines: 1,604,791
Check time: 40.36s
Total time: 47.22s
```

</div>

<div>
<h4>ts-go</h4>

```
Files: 5,109
Lines: 1,604,791
Check time: 4.416s
Total time: 5.571s
```

</div>

</div>

<div class="text-center mt-8">
<div class="text-4xl font-bold text-green-600">8.47x faster!</div>
</div>

<div class="mt-4 p-3 bg-blue-900 text-white border-l-4 border-blue-400 rounded">
💡 大規模プロジェクトほどパフォーマンス改善が顕著
</div>

---
transition: slide-left
---

# 開発体験への影響

<div class="grid grid-cols-2 gap-8 mt-8">

<div>
<h3>🤖 LLMとの組み合わせ</h3>

- コード生成後の型チェックが高速化
- エディタの応答性向上
- 開発フローの改善

</div>

<div>
<h3>🔧 CI/CDパイプライン</h3>

- ビルド時間の大幅短縮
- フィードバックループの高速化
- 開発効率の向上

</div>

</div>

<div class="mt-8">
<h3>🎯 特に効果が期待される場面</h3>

- 大規模なモノレポ
- 複雑な型定義を持つプロジェクト
- 頻繁な型チェックが必要な開発環境
</div>

---
transition: slide-left
layout: center
class: text-center
---

# まとめ

<div class="mt-8 space-y-6">

<div class="text-xl">
🚀 <strong>ts-go</strong> は型チェックの大幅な高速化を実現
</div>

<div class="text-xl">
📊 大規模プロジェクトほど効果が顕著 <span class="text-green-600 font-bold">(8.47x faster)</span>
</div>

<div class="text-xl">
🤖 LLM時代の開発体験向上に貢献
</div>

<div class="text-xl">
🔮 今後のTypeScript開発における重要な選択肢
</div>

</div>

<div class="mt-12">
<div class="text-sm opacity-75">
参考: https://github.com/microsoft/typescript-go
</div>
</div>

---
transition: slide-left
layout: center
class: text-center
---

# ありがとうございました

<div class="mt-8">
<div class="text-lg opacity-75">ぜひtsgoを触ってみてください！</div>
</div>