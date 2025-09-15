# ts-goを触ってみて得た学び

tsgo触ってみた

とりあえず、remix-cloudflareの自分のプロジェクトで比較

↑普段使っているフレームワークのサンプルコードとか持ってきてみるか

- テックブログネタだが旬が過ぎてしまった感
    - プレビューとれたタイミングでやりたい

check timeが特に改善してそう

# 何を話そうか？

- ts-goに触ってみて得た学び
- ポモドーロタイマーアプリをサンプルにやる
    - コード量多いパターンのサンプルでNext.js or 他のOSSをサンプルにする
- 話したいこと
    - ts-goによるパフォーマンス改善
    - TypeScriptってどういう構成なんだっけ
        - ts-goはコンパイラ
        - 全体で見てどこが改善したのか
    - 開発している時は意識しづらいけど、実際に触ってみると裏側のこと意識して学びがあったってことを話したい
    - LLMと絡めて
        - 言語機能も高速化すると嬉しい
        - LLMでコード生成すると、エディタでTypeScriptの言語機能が適用されるのに時間がかかる
            - ファイルが生成される
            - 大量のコードが生成されると、言語サーバーが解析するのに時間がかかるから

## Note

- https://github.com/microsoft/typescript-go
    - Strada
        - v6まで
        - 従来のTS
    - Corsa
        - v7以降
        - go版のTS
- **Anders Hejlsberg（アンダース・ヘルスバーグ）さんの話**
    - https://devblogs.microsoft.com/typescript/typescript-native-port/
    - singleスレッドのオプション実行して時間の話してる
        - これが並列化セクションの話ね
        - 並列化*性能改善=10x faster
    - TypeScriptのLSP導入
        - 今までは逆にどういう仕組み？
            - ts-server周りの話
                - LSPの代わり
                - ts-morphが無くなる云々はここ関連
    - https://notebooklm.google.com/notebook/ce325754-b340-4bc7-9990-28f90140fd64?original_referer=https:%2F%2Fwww.google.com%23&pli=1
        - JavaScript、JS Doc、JSXのサポートが完了していない…？
        - GithubのREADMEを見ると完了してそう
            - 今回のPreviewリリースでJSXをサポート
- TypeScript API
    1. **Compiler API**: Allows programmatic compilation, used by build tools
    2. **Language Service API**: Provides IDE features, used by editor extensions
    3. **Server API**: Implements the TypeScript server protocol
- 今回のスコープは主に1と2
- TypeScript Compiler
    - 型チェック
        - tscしかない
            - 高速化したいぞ
            - ニーズここが高いぞ
                - ts-go(10x faster!!!!)
    - コンパイル（トランスパイル）
        - tsc以外にもある
        - esbuild、SWCなど、高速なコンパイラーが登場
            - Viteと組み合わせて使用
            - Vite
                - ビルドツール
                - 開発モードではバンドルせず、ESMをそのまま実行
                - ビルド時はRollupを使用してバンドル
- バンドラー：Vite（Rollup）
    - TypeCheck：tsc
        - tsc --noEmitで型チェックのみ実行
    - Compiler
        - tsc
        - esbuild
        - SWC

## Verification

## ポモドーロ（コード量：小）

https://github.com/karukan029/focus-flare-time

3.14x faster!!

## tsc

```bash
Files:                         789
Lines of Library:            39992
Lines of Definitions:       143650
Lines of TypeScript:          6645
Lines of JavaScript:             0
Lines of JSON:                   0
Lines of Other:                  0
Identifiers:                208631
Symbols:                    259601
Types:                       35079
Instantiations:             582984
Memory used:               325243K
Assignability cache size:    20056
Identity cache size:          1224
Subtype cache size:            554
Strict subtype cache size:     161
I/O Read time:               0.10s
Parse time:                  0.39s
ResolveModule time:          0.09s
ResolveTypeReference time:   0.00s
ResolveLibrary time:         0.00s
Program time:                0.63s
Bind time:                   0.20s
Check time:                  1.39s
printTime time:              0.00s
Emit time:                   0.00s
Total time:                  2.23s
```

### tsgo

```bash
Files:              789
Lines:           190287
Identifiers:     208631
Symbols:         464169
Types:           223932
Instantiations: 4229536
Memory used:    392090K
Memory allocs:  4451913
Config time:     0.004s
Parse time:      0.085s
Bind time:       0.020s
Check time:      0.596s
Emit time:       0.000s
Total time:      0.710s
```

## VSCode（コード量：大）

https://github.com/microsoft/vscode

8.47x faseter!!

### tsc

```bash
Files:                         5109
Lines of Library:             52632
Lines of Definitions:        144578
Lines of TypeScript:        1409510
Lines of JavaScript:              0
Lines of JSON:                    0
Lines of Other:                   0
Identifiers:                2660806
Symbols:                    3478738
Types:                      1170077
Instantiations:             1669475
Memory used:               3609324K
Assignability cache size:    393854
Identity cache size:          16308
Subtype cache size:           72377
Strict subtype cache size:    65748
I/O Read time:                0.82s
Parse time:                   2.92s
ResolveModule time:           0.51s
ResolveTypeReference time:    0.00s
ResolveLibrary time:          0.01s
Program time:                 4.93s
Bind time:                    1.91s
Check time:                  40.36s
printTime time:               0.01s
Emit time:                    0.01s
Total time:                  47.22s
```

### tsgo

```bash
Files:              5109
Lines:           1604791
Identifiers:     2660190
Symbols:         4358761
Types:           1767503
Instantiations:  2951197
Memory used:    3235616K
Memory allocs:  26464352
Config time:      0.181s
Parse time:       0.719s
Bind time:        0.121s
Check time:       4.416s
Emit time:        0.016s
Total time:       5.571s
```

## LT

### 流れ

- 今回のPreview版のスコープ
    - 型チェック
        - 図で示す
            - 型チェック、コンパイル（トランスパイル）、言語サーバー
            - 今回何が嬉しいか
                - 型チェックの高速化
                - tscによる型チェックは代替手段がなく、ボトルネック
                - コンパイラはSWC、esbuildなど高速化するための代替手段がある
    - LSP
        - importの補完は動かない
        - LLMでコード生成すると、TypeScriptの言語機能が再度読み込まれる
            - 高速で読み込めるようになると、ここが改善してくるかも
- 手元で試してみた
    - 型チェック
        - 小規模：190287line
            - 3.14x faster!!
        - 大規模：1604791line
            - 8.47x faseter!!
        - ベンチマークを見ても、大規模なプロジェクトほどパフォーマンスが改善している
- 普段何気なく使っているTypeScript云々〜

- part
    - https://www.figma.com/design/HLMKSdgL0ZBqgPIMRMYcNX/LT-drawing?node-id=0-1&t=KcWYbaIWZANb0Asu-1
- slidev
    - https://sli.dev/

## **References**

- https://github.com/microsoft/typescript-go
- https://devblogs.microsoft.com/typescript/typescript-native-port/
- https://devblogs.microsoft.com/typescript/announcing-typescript-native-previews/
- https://deepwiki.com/microsoft/TypeScript