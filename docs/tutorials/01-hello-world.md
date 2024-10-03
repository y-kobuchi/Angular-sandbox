# Angular Hello World チュートリアル

このチュートリアルは、Angularアプリケーションの基本を学ぶための最初のステップです。このレッスンでは、有名な「Hello World」のテキストを表示するアプリケーションを作成します。

## 学習内容

- 開発環境のセットアップ確認
- Angularアプリケーションの基本構造理解

## 手順

1. デフォルトアプリのダウンロード:
   - 右上の「Download」アイコンをクリックして、ソースコードを含む.zipファイルをダウンロード。
   - ローカルのターミナルとIDEで開く。  

2. 依存関係のインストール:
   - プロジェクトディレクトリで以下のコマンドを実行。

     ```sh
     npm install
     ```

     実行失敗。package.jsonが該当ディレクトリに存在しないためエラー。  
     仕方ないので、新規プロジェクト作成時にできたpackage.jsonを持ってきてそれを利用。

3. アプリのビルドとサーブ:
   - 以下のコマンドを実行してアプリをビルドし、サーブ。

     ```sh
     ng serve
     ```

     実行失敗。`Error: This command is not available when running the Angular CLI outside a workspace.`  
     解決策が不明なので、いったん`ng new 01-hello-world`で新規プロジェクトを作成して、そこをベースに修正していく。

   - ブラウザで `http://localhost:4200` を開き、デフォルトのウェブサイトが表示されることを確認。
