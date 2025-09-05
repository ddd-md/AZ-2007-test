# ラボ02: GitHub Copilot を使用して新機能を開発する

## 概要

このラボでは、GitHub Copilotを活用して図書館アプリケーションに新しい「書籍の在庫確認」機能を追加します。実際の開発現場で使われる手法（ブランチ作成、機能開発、プルリクエスト）を体験しながら、AI支援開発の効果を実感していただきます。

---

## 演習5: 新しい「書籍の在庫確認」機能を開発する

### 🎯 演習の目標
司書が書籍の貸出状況を素早く確認できる機能を追加し、図書館業務の効率化を図ります。

### 📋 実装する機能
- **新機能名**: SearchBooks（書籍検索）
- **機能概要**: 書籍タイトルで検索し、貸出可能か貸出中かを表示
- **表示メッセージ例**:
  - 「Book Oneは貸出可能です」
  - 「Book Oneは別の利用者に貸し出されています。返却期限は2024-01-15です」

---

## タスク1: 開発用ブランチの作成

### ステップ1: 現在の状態を確認

1. **Visual Studio Codeで AccelerateDevGitHubCopilot が開かれていることを確認**

2. **ソース管理ビューを開く**
   - 左サイドバーの**分岐アイコン**（ソース管理）をクリック
   - または **Ctrl + Shift + G** を押下

3. **リモートリポジトリとの同期確認**
   - 画面下部に「同期」または「プル」ボタンが表示されている場合はクリック
   - すべての変更が最新状態になったことを確認

### ステップ2: 新しいブランチを作成

1. **現在のブランチを確認**
   - 画面左下に **「main」** と表示されていることを確認
   
2. **新しいブランチを作成**
   - 左下の **「main」** をクリック
   - 表示されたメニューで **「book availability」** と入力
   - **「+ 新しいブランチの作成」** を選択

3. **ブランチをリモートに公開**
   - **「ブランチの発行」** をクリック
   - GitHub認証が求められた場合は承認してください

> **✅ 完了確認**: 左下の表示が **「book availability」** に変わったことを確認

---

## タスク2: 新しいSearchBooksアクションの実装

### 📁 作業対象ファイル
以下のファイルを順番に更新していきます：
1. `CommonActions.cs` - 新しいアクション定義
2. `ConsoleApp.cs` - ユーザーインターフェース部分

### ステップ1: CommonActions列挙型を更新

1. **ソリューションエクスプローラービューを開く**
   - 左サイドバーの**ファイルアイコン**をクリック

2. **CommonActions.csファイルを開く**
   - `src/Library.Console/CommonActions.cs` を開く

3. **CommonActions列挙型を選択**
   ```csharp
   public enum CommonActions
   {
       Repeat = 0,
       Select = 1,
       Quit = 2,
       SearchPatrons = 4,
       RenewPatronMembership = 8,
       ReturnLoanedBook = 16,
       ExtendLoanedBook = 32
   }
   ```

4. **GitHub Copilotで新しいアクションを追加**
   - **Ctrl + I** でインラインチャットを開く
   - 以下のプロンプトを入力：
   ```plaintext
   選択範囲を更新して、新しい SearchBooks アクションを追加してください
   ```

5. **生成されたコードを確認して承諾**
   - 以下のような結果が表示されることを確認：
   ```csharp
   public enum CommonActions
   {
       Repeat = 0,
       Select = 1,
       Quit = 2,
       SearchPatrons = 4,
       RenewPatronMembership = 8,
       ReturnLoanedBook = 16,
       ExtendLoanedBook = 32,
       SearchBooks = 64
   }
   ```

### ステップ2: メニューオプション表示機能を更新

1. **ConsoleApp.csファイルを開く**
   - `src/Library.Console/ConsoleApp.cs` を開く

2. **WriteInputOptionsメソッドを検索**
   - **Ctrl + F** で「WriteInputOptions」を検索

3. **WriteInputOptionsメソッドを選択**
   ```csharp
   static void WriteInputOptions(CommonActions options)
   {
       // メソッド全体を選択
   }
   ```

4. **GitHub Copilotで機能を追加**
   - **Ctrl + I** でインラインチャットを開く
   - 以下のプロンプトを入力：
   ```plaintext
   選択範囲を更新して、CommonActions.SearchBooks アクションのオプションを追加してください。文字「b」と「書籍の在庫確認」メッセージを使用してください
   ```

5. **生成されたコードを確認**
   - 以下のようなif文が追加されることを確認：
   ```csharp
   if (options.HasFlag(CommonActions.SearchBooks))
   {
       Console.WriteLine(" - \"b\" to check for book availability");
   }
   ```

### ステップ3: 入力処理機能を更新

1. **ReadInputOptionsメソッドを検索**
   - 同じファイル内で **Ctrl + F** で「ReadInputOptions」を検索

2. **ReadInputOptionsメソッドを選択**
   - メソッド全体を選択

3. **GitHub Copilotで入力処理を追加**
   - **Ctrl + I** でインラインチャットを開く
   - 以下のプロンプトを入力：
   ```plaintext
   選択範囲を更新して、CommonActions.SearchBooks アクションのオプションを追加してください
   ```

4. **生成されたコードを確認**
   - switch文に以下のようなケースが追加されることを確認：
   ```csharp
   "b" when options.HasFlag(CommonActions.SearchBooks) => CommonActions.SearchBooks,
   ```

### ステップ4: PatronDetailsメソッドを更新

1. **PatronDetailsメソッドを検索**
   - **Ctrl + F** で「PatronDetails」を検索

2. **PatronDetailsメソッドを選択**
   - メソッド全体を選択

3. **GitHub Copilotで機能を統合**
   - **Ctrl + I** でインラインチャットを開く
   - 以下のプロンプトを入力：
   ```plaintext
   選択範囲を更新して、ReadInputOptions呼び出し前にCommonActions.SearchBooksをoptionsに追加してください。また、SearchBooksアクションを処理するelse ifブロックを追加し、SearchBooksという名前の新しいメソッドを呼び出してください
   ```

4. **生成されたコードを確認**
   - optionsの初期化部分が更新されることを確認
   - 新しいelse ifブロックが追加されることを確認

---

## タスク3: SearchBooksメソッドの実装

### ステップ1: メソッドの設計を理解

SearchBooksメソッドの処理フロー：
1. ユーザーから書籍タイトルを入力
2. Books.jsonから一致する書籍を検索
3. Loans.jsonで貸出状況を確認
4. 結果メッセージを表示

### ステップ2: 基本的なSearchBooksメソッドを作成

1. **ConsoleApp.csの末尾に移動**

2. **基本的なメソッドの骨組みを確認**
   ```csharp
   async Task<ConsoleState> SearchBooks()
   {
       return ConsoleState.PatronDetails;
   }
   ```

3. **ユーザー入力処理を追加**
   - メソッドを選択して **Ctrl + I**
   - 以下のプロンプトを入力：
   ```plaintext
   選択範囲を更新して、書籍タイトルを取得してください。「検索する書籍タイトルを入力してください」とプロンプト表示し、ユーザー入力を読み取り、書籍タイトルがnullでないことを確認してください
   ```

### ステップ3: データアクセス機能の実装

1. **必要なファイルをチャットコンテキストに追加**
   - チャットビューを開く（**Ctrl + Alt + I**）
   - 以下のファイルをドラッグ&ドロップで追加：
     - `ConsoleApp.cs`
     - `JsonData.cs`（Library.Infrastructure/Data/）
     - `JsonLoanRepository.cs`（Library.Infrastructure/Data/）
     - `Books.json`（Library.Console/Json/）
     - `Loans.json`（Library.Console/Json/）
     - `BookItems.json`（Library.Console/Json/）

2. **包括的な実装指示を送信**
   ```plaintext
   @workspace SearchBooksメソッドとConsoleAppクラスを更新して、書籍タイトルが貸出可能かどうかを判断する方法を説明してください。SearchBooksメソッド内で、LINQクエリとユーザー提供の書籍タイトルを使用して一致する書籍を返してください。書籍が見つかった場合、LINQクエリとLoans.jsonを使用して書籍が貸出可能かどうかを確認してください。書籍が返却されている場合、「{book.title}は貸出可能です」というメッセージを表示してください。書籍が貸出中の場合、「{book.title}は別の利用者に貸し出されています。返却期限は{loan.DueDate}です」というメッセージを表示してください
   ```

### ステップ4: 生成されたコードの実装

GitHub Copilotからの提案に基づいて、以下のファイルを更新してください：

1. **JsonData.csにSearchBookByTitleメソッドを追加**
   ```csharp
   public Book? SearchBookByTitle(string title)
   {
       return Books?.FirstOrDefault(b => b.Title.Equals(title, StringComparison.OrdinalIgnoreCase));
   }
   ```

2. **ConsoleApp.csのクラス宣言部分を更新**
   - `_jsonData`フィールドを追加
   - コンストラクターにJsonDataパラメーターを追加
   - 必要なusing文を追加

3. **SearchBooksメソッドを完成**
   - 書籍検索ロジック
   - 貸出状況確認ロジック
   - 適切なメッセージ表示

4. **Program.csの依存関係注入を確認**
   - JsonDataのサービス登録が含まれていることを確認

### ステップ5: エラーの修正

1. **ビルドエラーを確認**
   - **Ctrl + Shift + P** → 「Tasks: Run Build Task」
   - または右クリック → 「ビルド」

2. **using文の追加**
   - `using Library.Infrastructure.Data;` をConsoleApp.csに追加

3. **最終的なビルド確認**
   - エラーがないことを確認（警告は無視して構いません）

---

## タスク4: 機能テストの実施

### ステップ1: アプリケーションの起動

1. **ソリューションをクリーン**
   - ソリューションエクスプローラーで **AccelerateDevGitHubCopilot** を右クリック
   - **「クリーン」** を選択

2. **アプリケーションを実行**
   - **Library.Console** プロジェクトを右クリック
   - **「デバッグ」** → **「新しいインスタンスを開始」**

### ステップ2: 新機能のテスト

1. **基本的な操作フローを実行**
   - パトロン名: **「One」** と入力
   - 利用者選択: **「2」** と入力

2. **書籍在庫確認機能をテスト**
   - "Input Options"で **「b」** と入力
   - 書籍タイトル: **「Book One」** と入力

3. **期待される結果**
   - Book Oneが貸出中の場合のメッセージを確認

4. **利用可能な書籍もテスト**
   - 再度 **「b」** を入力
   - 書籍タイトル: **「Book Nine」** と入力
   - 貸出可能メッセージを確認

5. **アプリケーション終了**
   - **「q」** を入力してアプリケーションを終了

> **📝 注意**: JSONデータファイルの場所により、更新内容の反映場所が異なります
> - デバッガー実行: `Library.Console\bin\Debug\net8.0\Json\`
> - dotnet run実行: `Library.Console\Json\`

---

## 演習6: 変更をメインブランチにマージする

### タスク1: 変更のコミットとプッシュ

1. **ソース管理ビューを開く**

2. **変更ファイルの確認**
   - 「変更」セクションに更新されたファイルが表示されることを確認

3. **AIコミットメッセージの生成**
   - コミットメッセージボックス右側の**✨アイコン**をクリック
   - GitHub Copilotが適切なコミットメッセージを生成

4. **変更をコミット**
   - **「コミット」** をクリック
   - 確認ダイアログで **「はい」** を選択

5. **リモートリポジトリに同期**
   - **「同期」** または **「プッシュ」** をクリック

### タスク2: プルリクエストの作成

1. **GitHubリポジトリを開く**
   - ブラウザーでGitHubアカウントにアクセス
   - AccelerateDevGitHubCopilotリポジトリを開く

2. **プルリクエストの開始**
   - **「Pull requests」** タブをクリック
   - **「New pull request」** をクリック

3. **ブランチの設定**
   - **base**: `main` ブランチ
   - **compare**: `book-availability` ブランチ

4. **プルリクエストの詳細入力**
   - タイトル: 「書籍在庫確認機能の追加」
   - 説明: GitHub Copilotで生成（利用可能な場合）

5. **プルリクエストの作成**
   - **「Create pull request」** をクリック

### タスク3: マージの実行

1. **チェックの完了を待機**
   - すべてのチェックが緑色になることを確認

2. **プルリクエストのマージ**
   - **「Merge pull request」** をクリック
   - **「Confirm merge」** をクリック

3. **ローカルでの更新**
   - Visual Studio Codeで **main** ブランチに切り替え
   - **「プル」** で最新の変更を取得

---

## 💡 学習のポイント

1. **ブランチワークフロー**: 機能開発用のブランチを作成し、完成後にマージする実践的な開発フロー
2. **AI支援開発**: GitHub Copilotを活用した効率的なコード生成
3. **段階的な実装**: 大きな機能を小さなタスクに分割して実装
4. **テスト駆動**: 実装後の動作確認の重要性

## ❓ トラブルシューティング

**Q: ビルドエラーが発生する**
A: using文の追加、プロジェクト参照の確認を行ってください

**Q: GitHub認証エラーが発生する**
A: Visual Studio Codeの設定でGitHubアカウントを再認証してください

**Q: 書籍在庫確認が正しく動作しない**
A: JSONファイルの読み込み場所とデータの整合性を確認してください

---

> **次のステップ**: パート3では、作成した機能の単体テストを開発します。品質保証の観点から重要な作業となります。