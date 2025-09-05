#### ラボ シナリオ (パート 2)

GitHub Copilot は、図書館アプリケーションの新機能の開発に役立ちます。

このラボのパート 2 では、GitHub Copilot を使用して、新機能をより迅速かつ正確に実装するのに役立つコード候補を生成します。

パート 2 には、次の演習が含まれています。

- 新しい "書籍の在庫" 機能を開発する。
- 変更をリポジトリの main ブランチにマージする。

### 演習 5: 新しい "書籍の在庫" 機能を開発する

書籍の在庫の機能には、次のコード更新プログラムが含まれている必要があります。

- 司書が書籍を検索するオプションを選択できるようにする新しい CommonActions.SearchBooks アクションを実装します。
- ConsoleApp.cs に新しい SearchBooks メソッドを作成します。 SearchBooks メソッドは、ユーザーが指定した書籍のタイトルを読み取る必要があります。 書籍が貸出可能かどうかを確認し、"book.title は貸出可能です" または "book.title は別の利用者に貸し出されています" のいずれかのメッセージを表示します。 返却期限は loan.DueDate です。

この演習では、次のタスクを完了します。

- コード リポジトリに "book availability" ブランチを作成します。
- GitHub Copilot の提案を使用すると、司書が選択できる新しい一般的な SearchBooks アクションの実装に役立ちます。
- GitHub Copilot の提案を使用すると、ユーザーが指定した書籍タイトルを読み取り、書籍が貸出可能かどうかを確認し、書籍の在庫状況を示すメッセージを表示する SearchBooks メソッドの開発に役立ちます。

#### タスク 1: リポジトリに新しいブランチを作成する

新しい "書籍の在庫" 機能の開発を開始する前に、リポジトリに新しいブランチを作成する必要があります。 これにより、リポジトリのメイン ブランチに影響を与えずに、新しい機能に取り組むことができます。 新しい機能の準備ができたら、メイン ブランチにマージできます。

以下の手順に従って、演習のこのセクションを完了します。

1. Visual Studio Code に AccelerateDevGitHubCopilot ソリューションが開かれていることを確認します。

1. [ソース管理] ビューを選択し、ローカル リポジトリがリモート リポジトリ (プルまたは同期) と同期していることを確認します。

1. ウィンドウの左下隅にある  **[main]**  を選択します。

1. 新しいブランチを作成するには、「**book availability**」と入力したあと、**[+ 新しいブランチの作成]** を選択します。

1. 新しいブランチをリモート リポジトリにプッシュするには、**[ブランチの発行]** を選択します。

#### タスク 2: 新しい一般的な "SearchBooks" アクションを実装する

演習のこのセクションでは、GitHub Copilot を使用して、SearchBooks という名前のメニュー オプションの実装に役立てます。 司書は SearchBooks アクションを選択して、書籍の在庫状況を確認します。

このタスク中に完了するファイル更新は次のとおりです。

- CommonActions.cs の CommonActions 列挙型の更新: CommonActions 列挙型に新しい SearchBooks アクションを追加します。
- ConsoleApp.cs の WriteInputOptions メソッドの更新: 新しい CommonActions.SearchBooks オプションを表示するためのサポートを追加します。
- ConsoleApp.cs の ReadInputOptions メソッドの更新: 新しい CommonActions.SearchBooks オプションを読み取る (選択する) ためのサポートを追加します。
- ConsoleApp.cs の PatronDetails メソッドの更新: 新しい CommonActions.SearchBooks オプションが選択されたときにそのオプションを処理するためのサポートを追加します。

以下の手順に従って、演習のこのセクションを完了します。

1. [ソリューション エクスプローラー] ビューを開きます。

1. CommonActions.cs ファイルを開いて、**CommonActions** 列挙型を選択します。

    CommonActions に新しい SearchBooks アクションを追加する必要があります。

1. インライン チャットを開き、次のプロンプトを入力します。

    ```plaintext
    Update selection to include a new `SearchBooks` action.
    ```

    GitHub Copilot により、新しい SearchBooks アクションを CommonActions 列挙型に追加するコード更新が提案されるはずです。

1. 推奨された更新を確認し、**[承諾]** を選択します。

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

1. ConsoleApp.cs ファイルを開きます。

1. **WriteInputOptions** メソッドを見つけて選択します。

    新しい CommonActions.SearchBooks オプションのサポートを追加する必要があります。 書籍が貸出可能かどうかを確認するオプションを表示します。

1. インライン チャットを開き、次のプロンプトを入力します。

    ```plaintext
    Update selection to include an option for the `CommonActions.SearchBooks` action. Use the letter "b" and the message "to check for book availability".
    ```

    GitHub Copilot により、SearchBooks アクションの新しい "if" ブロックを追加するコード更新が提案されます。

1. 推奨された更新を確認し、**[承諾]** を選択します。

    提案される更新は、次のコード スニペットのような内容になります。

    ```csharp

    static void WriteInputOptions(CommonActions options)
    {
        Console.WriteLine("Input Options:");
        if (options.HasFlag(CommonActions.ReturnLoanedBook))
        {
            Console.WriteLine(" - \"r\" to mark as returned");
        }
        if (options.HasFlag(CommonActions.ExtendLoanedBook))
        {
            Console.WriteLine(" - \"e\" to extend the book loan");
        }
        if (options.HasFlag(CommonActions.RenewPatronMembership))
        {
            Console.WriteLine(" - \"m\" to extend patron's membership");
        }
        if (options.HasFlag(CommonActions.SearchPatrons))
        {
            Console.WriteLine(" - \"s\" for new search");
        }
        if (options.HasFlag(CommonActions.SearchBooks))
        {
            Console.WriteLine(" - \"b\" to check for book availability");
        }
        if (options.HasFlag(CommonActions.Quit))
        {
            Console.WriteLine(" - \"q\" to quit");
        }
        if (options.HasFlag(CommonActions.Select))
        {
            Console.WriteLine("Or type a number to select a list item.");
        }
    }

    ```

1. 少し上にスクロールして、**ReadInputOptions** メソッドを見つけます。

1. **ReadInputOptions** メソッドを選択します。

    新しい CommonActions.SearchBooks オプションのサポートを追加する必要があります。 SearchBooks アクションを選択するユーザーを処理するケースを入れます。

1. インライン チャットを開き、次のプロンプトを入力します。

    ```plaintext
    Update selection to include an option for the `CommonActions.SearchBooks` action.
    ```

    GitHub Copilot により、SearchBooks アクションを選択するユーザーを処理する新しい "ケース" を追加するコード更新が提案されます。

1. 推奨された更新を確認し、**[承諾]** を選択します。

    提案される更新は、次のコード スニペットのような内容になります。

    ```csharp

    static CommonActions ReadInputOptions(CommonActions options, out int optionNumber)
    {
        CommonActions action;
        optionNumber = 0;
        do
        {
            Console.WriteLine();
            WriteInputOptions(options);
            string? userInput = Console.ReadLine();
            action = userInput switch
            {
                "q" when options.HasFlag(CommonActions.Quit) => CommonActions.Quit,
                "s" when options.HasFlag(CommonActions.SearchPatrons) => CommonActions.SearchPatrons,
                "m" when options.HasFlag(CommonActions.RenewPatronMembership) => CommonActions.RenewPatronMembership,
                "e" when options.HasFlag(CommonActions.ExtendLoanedBook) => CommonActions.ExtendLoanedBook,
                "r" when options.HasFlag(CommonActions.ReturnLoanedBook) => CommonActions.ReturnLoanedBook,
                "b" when options.HasFlag(CommonActions.SearchBooks) => CommonActions.SearchBooks,
                _ when int.TryParse(userInput, out optionNumber) => CommonActions.Select,
                _ => CommonActions.Repeat
            };
            if (action == CommonActions.Repeat)
            {
                Console.WriteLine("Invalid input. Please try again.");
            }
        } while (action == CommonActions.Repeat);
        return action;
    }

    ```

1. 下にスクロールし、**PatronDetails** メソッドを見つけて選択します。

    2 つのことを実行する必要があります。

    - ReadInputOptions を呼び出す前に、CommonActions.SearchBooks をオプションに追加する必要があります。
    - また、SearchBooks アクションを処理する else if も追加する必要があります。 else if ブロックは、SearchBooks という名前の新しいメソッドを呼び出す必要があります。

    これらの両方に一緒に対処できます。

1. インライン チャットを開き、次のプロンプトを入力します。

    ```plaintext
    Update selection to add `CommonActions.SearchBooks` to `options` before calling `ReadInputOptions`. Add an `else if` block to handle the `SearchBooks` action. The `else if` block should call a new method named `SearchBooks`.
    ```

    GitHub Copilot により、ReadInputOptions を呼び出す前に、CommonActions.SearchBooks をオプションに追加するコード更新が提案されるはずです。

1. 推奨された更新を確認し、**[承諾]** を選択します。

    ```csharp

    async Task<ConsoleState> PatronDetails()
    {
        Console.WriteLine($"Name: {selectedPatronDetails.Name}");
        Console.WriteLine($"Membership Expiration: {selectedPatronDetails.MembershipEnd}");
        Console.WriteLine();
        Console.WriteLine("Book Loans:");
        int loanNumber = 1;
        foreach (Loan loan in selectedPatronDetails.Loans)
        {
            Console.WriteLine($"{loanNumber}) {loan.BookItem!.Book!.Title} - Due: {loan.DueDate} - Returned: {(loan.ReturnDate != null).ToString()}");
            loanNumber++;
        }
        CommonActions options = CommonActions.SearchPatrons | CommonActions.Quit | CommonActions.Select | CommonActions.RenewPatronMembership | CommonActions.SearchBooks;
        CommonActions action = ReadInputOptions(options, out int selectedLoanNumber);
        if (action == CommonActions.Select)
        {
            if (selectedLoanNumber >= 1 && selectedLoanNumber <= selectedPatronDetails.Loans.Count())
            {
                var selectedLoan = selectedPatronDetails.Loans.ElementAt(selectedLoanNumber - 1);
                selectedLoanDetails = selectedPatronDetails.Loans.Where(l => l.Id == selectedLoan.Id).Single();
                return ConsoleState.LoanDetails;
            }
            else
            {
                Console.WriteLine("Invalid book loan number. Please try again.");
                return ConsoleState.PatronDetails;
            }
        }
        else if (action == CommonActions.Quit)
        {
            return ConsoleState.Quit;
        }
        else if (action == CommonActions.SearchPatrons)
        {
            return ConsoleState.PatronSearch;
        }
        else if (action == CommonActions.RenewPatronMembership)
        {
            var status = await _patronService.RenewMembership(selectedPatronDetails.Id);
            Console.WriteLine(EnumHelper.GetDescription(status));
            // reloading after renewing membership
            selectedPatronDetails = (await _patronRepository.GetPatron(selectedPatronDetails.Id))!;
            return ConsoleState.PatronDetails;
        }
        else if (action == CommonActions.SearchBooks)
        {
            return await SearchBooks();
        }
        throw new InvalidOperationException("An input option is not handled.");
    }

    ```

    > [!NOTE]
    > インライン チャットの提案により、SearchBooks メソッドのスタブ コードが作成される場合もあります。

#### タスク 3: SearchBooks メソッドを開発する

完了すると、SearchBooks メソッドにより、ユーザーが指定した書籍タイトルが読み取られ、書籍が貸し出し可能かどうかが確認され、その書籍の貸し出し状況を示すメッセージが表示されるはずです。 このメソッドでは、Books.json ファイルと Loans.json ファイルを使用して、一致する書籍タイトルを検索し、その書籍の貸し出し状況を判断する必要があります。

以下の手順に従って、演習のこのセクションを完了します。

1. 少し時間をかけて SearchBooks メソッドのプロセス要件を検討しましょう。

    メソッドは何を行う必要がありますか? 何を返す必要がありますか? パラメーターは必要ですか?

    SearchBooks メソッドは、次のプロセスを実行する必要があります。

    1. ユーザーに書籍のタイトルの入力を求めるメッセージを表示する。
    1. ユーザーが指定した書籍のタイトルを読み取る。
    1. 書籍が貸出可能かどうかを確認する
    1. 次のいずれかのオプションを示すメッセージを表示する。

        - "book.title は貸し出し可能"
        - "book.title は別の利用者に貸し出されています。 返却期限は loan.DueDat です。

    メッセージ オプションをビルドするには、コードで次の JSON ファイルにアクセスする必要があります。

    - 一致する Title と BookId を検索するには、Books.json が必要です。
    - 一致する BookItemId の ReturnDate と DueDate を検索するには、Loans.json が必要です。 BookItemId は、Books.json 内の BookId と同じです。

1. ConsoleApp.cs ファイルに次の **SearchBooks** メソッドが作成されていることを確認してください。

    ```csharp

    async Task<ConsoleState> SearchBooks()
    {
        return ConsoleState.PatronDetails;
    }

    ```

    > [!NOTE]
    > GitHub Copilot によって作成されたコード コメントは必ず削除してください。 不要で不正確なコメントは、GitHub Copilot の提案に悪影響を及ぼすおそれがあります。

1. **SearchBooks** メソッドを選択します。

1. インライン チャットを開き、次のプロンプトを入力します。

    ```plaintext
    Update selection to obtain a book title. Prompt the user to "Enter a book title to search for". Read the user input and ensure the book title isn't null.
    ```

    GitHub Copilot により、ユーザーが指定した書籍のタイトルをキャプチャするコード更新プログラムが提案されます。

1. 推奨された更新を確認し、**[承諾]** を選択します。

    ```csharp

    async Task<ConsoleState> SearchBooks()
    {
        string? bookTitle = null;
        while (String.IsNullOrWhiteSpace(bookTitle))
        {
            Console.Write("Enter a book title to search for: ");
            bookTitle = Console.ReadLine();
        }
        // Perform book search logic here
        return ConsoleState.PatronDetails;
    }

    ```

    > [!NOTE]
    > GitHub Copilot では、書籍タイトルのユーザー入力を処理する別のメソッドを作成できます。 その提案を受け入れ、引き続き SearchBooks メソッドでプライマリ ロジックを開発できます。

1. GitHub Copilot によって生成されたすべてのプレースホルダー コメントを削除して、**SearchBooks** メソッドを選択します。

1. チャット ビューを開きます。

1. チャット コンテキストに次のファイルを追加します: ConsoleApp.cs、JsonData.cs、JsonLoanRepository.cs、Books.json、Loans.json、BookItems.json。

1. 次のプロンプトを入力します。

    ```plaintext
    @workspace  Explain how to update the `SearchBooks` method and ConsoleApps class to determine whether a book title is available for loan. Inside the SearchBooks method, use a LINQ query and the user supplied book title to return a matching book. If a book is found, use a LINQ query and Loans.json to check if the book is available for loan. If the book has been returned, display a message stating "`book.title` is available for loan". If the book is on loan, display a message stating "`book.title` is on loan to another patron. The return due date is `loan.DueDate`".
    ```

1. 少し時間をかけて、提案された更新を確認しましょう。

    GitHub Copilot は、次のような候補に類似した更新プログラムを提案する必要があります。

    SearchBooks メソッドと ConsoleApp クラスを更新して、AE 書籍タイトルが貸出に使用できるかどうかを判断するには、次の手順に従います。

    a. JsonData クラスでタイトル別に書籍を検索するメソッドを追加します。
    b. ConsoleApp クラスの SearchBooks メソッドを更新して、この新しいメソッドを使用します。
    c. 書籍の貸出状態を確認し、適切なメッセージを表示します。

    更新されたコードを次に示します。

    手順 1: JsonData クラスでタイトル別に書籍を検索するメソッドを追加する

    ```csharp

    public Book? SearchBookByTitle(string title)
    {
        return Books?.FirstOrDefault(b => b.Title.Equals(title, StringComparison.OrdinalIgnoreCase));
    }

    ```

    手順 2: ConsoleApp クラスの SearchBooks メソッドを更新する

    ```csharp

    async Task<ConsoleState> SearchBooks()
    {
        string bookTitle = ReadBookTitle();
        // ensure data is loaded
        await _jsonData.LoadData();
        // find the book by title
        Book? book = _jsonData.SearchBookByTitle(bookTitle);
        if (book == null)
        {
            Console.WriteLine($"No book found with title: {bookTitle}");
            return ConsoleState.PatronDetails;
        }
        // find the loan status of the book
        Loan? loan = _jsonData.Loans?.FirstOrDefault(l => l.BookItemId == book.Id && l.ReturnDate == null);
        if (loan == null)
        {
            Console.WriteLine($"{book.Title} is available for loan.");
        }
        else
        {
            Console.WriteLine($"{book.Title} is on loan to another patron. The return due date is {loan.DueDate}.");
        }
        return ConsoleState.PatronDetails;
    }

    ```

    手順 3: JsonData を含むように ConsoleApp クラス コンストラクターを更新する

    ```csharp

    public class ConsoleApp
    {
        // Existing fields...
        JsonData _jsonData;
        public ConsoleApp(ILoanService loanService, IPatronService patronService, IPatronRepository patronRepository, ILoanRepository loanRepository, JsonData jsonData)
        {
            _patronRepository = patronRepository;
            _loanRepository = loanRepository;
            _loanService = loanService;
            _patronService = patronService;
            _jsonData = jsonData;
        }
        // Other methods...

    }

    ```

    手順 4: JsonData を ConsoleApp に渡すProgram.csを更新する

    ```csharp

    using Microsoft.Extensions.DependencyInjection;
    using Library.Infrastructure.Data;
    using Library.ApplicationCore;
    using Microsoft.Extensions.Configuration;
    var services = new ServiceCollection();
    var configuration = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json")
    .Build();
    services.AddSingleton<IConfiguration>(configuration);
    services.AddScoped<IPatronRepository, JsonPatronRepository>();
    services.AddScoped<ILoanRepository, JsonLoanRepository>();
    services.AddScoped<ILoanService, LoanService>();
    services.AddScoped<IPatronService, PatronService>();
    services.AddSingleton<JsonData>();
    services.AddSingleton<ConsoleApp>();
    var servicesProvider = services.BuildServiceProvider();
    var consoleApp = servicesProvider.GetRequiredService<ConsoleApp>();
    consoleApp.Run().Wait();

    ```

    これらの変更により、ConsoleApp クラスの SearchBooks メソッドは、Books.json ファイル内のタイトルで書籍を検索し、Loans.json ファイルでその貸出状態を確認します。 書籍が貸出可能な場合は、書籍が貸出可能であることを示すメッセージが表示されます。 書籍が貸出中の場合は、書籍が貸出中であることを示すメッセージが表示され、返却期限が表示されます。

    > [!NOTE]
    > GitHub Copilot は、JsonData クラスのメソッドを使用するのではなく、SearchBooks メソッドで LINQ クエリを使用してブック オブジェクトを返すように提案する場合があります。 ただし、(SEARCHBooks メソッドに LINQ クエリを使用するのではなく) 書籍のタイトルを返し、貸出状態を確認するためのメソッドを `JsonData` クラスに作成することも推奨されます。 以下の手順は、これらのアプローチの組み合わせを使用する GitHub Copilot の提案に基づいています。

1. 提案された SearchBookByTitle メソッドを **JsonData** クラスの末尾に追加します。

    ```csharp

    public Book? SearchBookByTitle(string title)
    {
        return Books?.FirstOrDefault(b => b.Title.Equals(title, StringComparison.OrdinalIgnoreCase));
    }

    ```

1. _jsonData フィールド宣言をクラス コンストラクターの前の ConsoleAp クラスに追加します。

    ```csharp

    JsonData _jsonData;

    ```

1. JsonData パラメーターを ConsoleApp クラス コンストラクターに追加し、それを使用してコンストラクター内の _jsonData フィールドを初期化します。

    更新されたクラス コンストラクターは、次のコード スニペットのようになります。

    ```csharp

    public ConsoleApp(ILoanService loanService, IPatronService patronService, IPatronRepository patronRepository, ILoanRepository loanRepository, JsonData jsonData)
    {
        _patronRepository = patronRepository;
        _loanRepository = loanRepository;
        _loanService = loanService;
        _patronService = patronService;
        _jsonData = jsonData;
    }

    ```

1. JsonData が ConsoleApp クラスで認識されていないことに注意してください。

    ConsoleApp.cs ファイルの先頭に "using Library.Infrastructure.Data;" を追加する必要があります。

1. ConsoleApp.csファイルの先頭に、次の "using" ステートメントが追加されていることを確認します。

    ```csharp

    using Library.Infrastructure.Data;

    ```

1. コード候補を使用して、**SearchBooks** メソッドを更新します。

    提案されたコードは、SearchBookByTitle メソッド（または LINQ クエリ）を使用してタイトルで書籍を検索する必要があります。 書籍が見つかった場合は、LINQ クエリを使用して、書籍が貸出中かどうかを判断します。 このコードには、検索結果に基づいて書籍の在庫状態を表示するロジックも含まれています。 応答メッセージの表示に使用されるロジックを実装する方法がいくつかあります。 例は以下のコード スニペットに示されています。

    次のコード スニペットは、SearchBooks メソッドの 1 つの実装を示しています。

    ```csharp

    async Task<ConsoleState> SearchBooks()
    {
        string bookTitle = ReadBookTitle();
        Book? book = _jsonData.SearchBookByTitle(bookTitle);
        if (book == null)
        {
            Console.WriteLine($"No book found with title: {bookTitle}");
            return ConsoleState.PatronDetails;
        }
        Loan? loan = _jsonData.Loans?.FirstOrDefault(l => l.BookItemId == book.Id && l.ReturnDate == null);
        if (loan == null)
        {
            Console.WriteLine($"{book.Title} is available for loan.");
        }
        else
        {
            Console.WriteLine($"{book.Title} is on loan to another patron. The return due date is {loan.DueDate}.");
        }
        return ConsoleState.PatronDetails;
    }

    ```

1. 既存の **Program.cs** ファイルと推奨される更新プログラムを比較します。

    ```csharp

    using Microsoft.Extensions.DependencyInjection;
    using Library.Infrastructure.Data;
    using Library.ApplicationCore;
    using Microsoft.Extensions.Configuration;
    var services = new ServiceCollection();
    var configuration = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appSettings.json")
        .Build();
    services.AddSingleton<IConfiguration>(configuration);
    services.AddScoped<IPatronRepository, JsonPatronRepository>();
    services.AddScoped<ILoanRepository, JsonLoanRepository>();
    services.AddScoped<ILoanService, LoanService>();
    services.AddScoped<IPatronService, PatronService>();
    services.AddSingleton<JsonData>();
    services.AddSingleton<ConsoleApp>();
    var servicesProvider = services.BuildServiceProvider();
    var consoleApp = servicesProvider.GetRequiredService<ConsoleApp>();
    consoleApp.Run().Wait();

    ```

    もし Program.cs ファイルに、ASP.NET Core の依存関係挿入システムに対して JsonData クラスの単一のインスタンスを作成し、JsonData が要求された際にその同じインスタンスを使用するよう指示するコードが既に含まれている場合、更新は不要です。

1. **SearchBooks** メソッドを選択します。

1. チャット ビューを開き、次のプロンプトを入力します。

    ```plaintext
    Explain the LINQ queries and the evaluation of the book's availability status.
    ```

1. 少し時間をかけて、GitHub Copilot から提供された説明を確認しましょう。

    書籍が貸出中かどうかを確認するクエリには、次の説明に似た内容が表示されるはずです。

    ```markdown
    **Search for the Loan Status of the Book**
    ```

    ```csharp

    Loan? loan = _jsonData.Loans?.FirstOrDefault(l => l.BookItemId == book.Id && l.ReturnDate == null);

    ```

    ```markdown
    - _jsonData.Loans: Accesses the list of loans from the JsonData class.

    - ?.: The null-conditional operator ensures that if _jsonData.Loans is null, the entire expression will return null without throwing a NullReferenceException.

    - FirstOrDefault: A LINQ method that returns the first element in the collection that matches the specified condition, or null if no such element is found.

    - Lambda Expression: l => l.BookItemId == book.Id && l.ReturnDate == null

        - l.BookItemId == book.Id: Checks if the BookItemId of the loan matches the Id of the book being searched.

        - l.ReturnDate == null: Checks if the loan's ReturnDate is null, indicating that the book is currently on loan and has not been returned.

    - Result: If a matching loan is found, it is assigned to the loan variable. If no matching loan is found, loan is null.

    **Evaluation of the Book's Availability Status**

    After executing the LINQ queries, the method evaluates the availability status of the book:

      - If loan is null, the book has been returned. The method prints that the book is available for loan.
      - If loan is not null, the book is currently on loan (no return date). The method prints that the book is on loan and provides the date when the book should be returned.

    ```

1. **Check if the Book is on Loan** (書籍が貸出中かどうかを確認する) クエリによって実装されたロジックを検証します。

    Null 条件演算子: ”?."  は "null 条件演算子" と呼ばれる演算子です。 これは、_jsonData.Loans が null の場合、NullReferenceException をスローすることなく、式全体が null と評価されることを保証します。

    ラムダ式: ラムダ式 "l => l.BookItemId == book.Id && l.ReturnDate == null" は、FirstOrDefault メソッドの条件を定義します。 各貸出オブジェクト "l" をチェックして、そのオブジェクトの BookItemId が書籍オブジェクトの Id と一致するかどうか、および貸出オブジェクトの ReturnDate が null 値かどうかを確認します。

    このコードは、BookItemId が指定された書籍の Id と一致し ReturnDate が null 値である、_jsonData.Loans コレクション内の最初の貸出オブジェクトを検索しようとしています。 そのような貸出オブジェクトが見つからない場合や、"_jsonData.Loans" が null 値の場合は、貸出オブジェクトが null 値に設定されます。

    > [!NOTE]
    > ほとんどの現代の図書館には、同じ書籍が複数冊所蔵されています。 このコードは、各書籍が 1 冊だけであることを前提としています。 図書館に同じ書籍が複数ある場合は、同じ書籍の複数の貸出を処理するためにコードを更新する必要があります。 このトレーニングの目的上、各書籍は 1 冊だけであり、提供されるロジックで十分であると想定しています。

1. 更新されたファイルを保存します。

1. エラーが発生しないようにソリューションをビルドします。

    **警告**メッセージが表示されますが、エラーは発生しないはずです。

    ソリューション エクスプローラー ビューでソリューションをビルドするには、**AccelerateDevGitHubCopilot** を右クリックし、**[ビルド]** を選択します。

#### まとめ - 演習 5

GitHub Copilot を使用して、図書館アプリケーションの新機能の実装に役立てました。 リポジトリに新しいブランチを作成したあと、GitHub Copilot を使用して、新機能をより迅速かつ正確に実装するのに役立つコード候補を生成しました。 CommonActions に新しい SearchBooks アクションを追加し、ConsoleApp.cs の WriteInputOptions メソッドと ReadInputOptions メソッドを更新し、書籍の在庫状況を判断する新しい SearchBooks メソッドを作成しました。

### 演習 6: 変更をリポジトリのメイン ブランチにマージする

司書が書籍の在庫状況を判断できるようになる新しい機能を作成しました。 次に、機能が期待どおりに動作することを確認し、変更をリポジトリのメイン ブランチにマージする必要があります。

この演習では、次のタスクを完了します。

- 新しい機能が期待どおりに動作することを確認する
- 変更をリモート リポジトリと同期する
- 変更をリポジトリのメイン ブランチにマージする pull request を作成します。

#### タスク 1: 新しい機能が期待どおりに動作することを確認する

自動テストがないため、手動テストを使用して、新しい機能が期待どおりに動作することを確認できます。 検証できるデータ ソースを使用することが重要です。 この場合は、Books.json ファイルと Loans.json ファイルを使用して、新しい機能が書籍の在庫状況を正しく報告することを確認します。

以下の手順に従って、演習のこのセクションを完了します。

1. [ソリューション エクスプローラー] ビューを開きます。

1. アプリケーションを実行するには、**Library.Console** を右クリックし、**[デバッグ]** を選択してから **[新しいインスタンスを開始]** を選択します。

1. 利用者名の入力を求められたら、「**One**」と入力して Enter キーを押します。

    検索クエリに一致する利用者の一覧が表示されます。

1. "Input Options" プロンプトで「**2**」と入力し、Enter キーを押します。

    「**2**」と入力すると、リストにある 2 番目のパトロンが選択されます。

    利用者の名前とメンバーシップの状態が表示され、その後に書籍貸出の詳細が表示されます。

1. "Input Options" プロンプトで「**b**」と入力し、Enter キーを押します。

    「**b**」と入力すると、書籍の在庫状態を検索するオプションが選択されます。

    書籍のタイトルを入力するプロンプトが表示されます。

1. 「**Book One**」と入力し Enter キーを押します。

    ダウンロードした元のデータでは、**Book One** は現在 **Patron Forty-Nine**に貸出中であるため、在庫がありません。

1. 書籍が別の利用者に貸し出されていることを示すメッセージがアプリケーションに表示されることを確認します。

    上にスクロールしないとメッセージが見えない場合があります。

1. **Book Nine** が **Patron One** により返却されたことを確認します。

    前の演習では、図書館アプリケーションのテスト中に、**Patron One** に **Book Nine** を返却してもらいました。 アプリケーションで **Book Nine** が **Patron One** によって返却されていないと示されている場合は、入力オプションを使用して **Book Nine** を選択して返却してから続行してください。

    > [!NOTE]
    > Library.Console.csproj ファイルを使用すると、アプリケーションのビルド時に JSON データ ファイルが出力ディレクトリに確実にコピーされます。 ただし、クリーンまたはリビルド操作を実行すると、本質的にデータがリセットされます。 ビルド操作を繰り返し実行しても、出力ディレクトリ ("/bin/Debug/net8.0/") 内のデータはリセットされません。

1. "Input Options" プロンプトで「**b**」と入力し、Enter キーを押します。

1. 「**Book Nine**」と入力し Enter キーを押します。

1. 書籍が貸出可能であることを示すメッセージがアプリケーションに表示されることを確認します。

1. "Input Options" プロンプトで「**q**」と入力し、Enter キーを押します。

1. デバッグ セッションを停止します。

1. **Loans.json** ファイルを開きます。

    Loans.json ファイルは、各書籍の貸出状況を追跡するために使用されます。 Loans.json ファイルを使用して、Book One と Book Nine の在庫状況が正しいことを確認できます。

    更新された Loans.json ファイルは、"Library.Console\bin\Debug\net8.0\Json" フォルダーまたは "Library.Console\Json" フォルダーにあるはずです。

    - Visual Studio Code デバッガーを使用してアプリを実行している場合は、更新された Loans.json ファイルは "Library.Console\bin\Debug\net8.0\Json" フォルダーにあるはずです。

    - "AccelerateDevGitHubCopilot\src\Library.Console" フォルダーから "dotnet run" コマンドを使用してアプリを実行している場合、更新された Loans.json ファイルは "Library.Console\Json" フォルダーにあるはずです。

1. 貸出 ID 37 と貸出 ID 46 が両方とも Book One ("BookItemId": 1) のものであることを確認します。

    - 貸出 ID 37 には ReturnDate 値 (2024-01-17) があるはずです
    - 貸出 ID 46 には ReturnDate 値がないはずです。 ReturnDate 値は null 値のはずです。

    ReturnDate 値は、書籍が現在貸出中かどうかを判断するために使用されます。 ReturnDate 値が null 値の場合、書籍は貸出中と見なされます。

1. 貸出 ID 22 が Book Nine ("BookItemId": 9) のものであり、ReturnDate 値が当日の日付に設定されていることを確認します。

    デバッガーで実行していて、Loans.json ファイルに変更を加えた後でソリューションをクリーンした場合、それらの変更内容は失われます。 ソリューションをクリーンしたら、次回アプリケーションを実行するときに、Loans.json ファイルが元のデータにリセットされます。 Loans.json ファイルを手動で更新して、Book One と Book Nine の在庫状況をテストできます。

#### タスク 2: 変更をリモート リポジトリと同期する

Visual Studio Code のソース管理ビューを使用して、変更内容をローカル リポジトリにコミットできます。 変更内容をコミットしたら、リモート リポジトリと同期できます。

以下の手順に従って、演習のこのセクションを完了します。

1. [ソース管理] ビューを選択します。

1. **Changes**の下に CommonActions.cs、ConsoleApp.cs、および JsonData.cs ファイルが一覧表示されていることを確認します。

    GitHub Copilot が LINQ クエリを使用して SearchBooks メソッド内で書籍の在庫状況を直接判断する場合は、**Changes** の下に JsonData.cs が表示されない可能性があります。

1. GitHub Copilot を使用して、**[コミット]** のメッセージを生成します。

1. 変更をステージングしてコミットするには、**[コミット]** を選択してから、**[はい]** を選択します。

1. 変更をリモート リポジトリに同期 (またはプッシュ) します。

    要求された場合は、GitHub アカウントを使用して同期を認証および承認します。

#### タスク 3: 変更をリポジトリのメイン ブランチにマージする pull request を作成する

pull request を使用すると、リポジトリのメイン ブランチに対する変更を提案できます。 pull request を作成したら、変更内容をレビューし、チェックに合格したことを確認してから、変更をメイン ブランチにマージできます。 GitHub Copilot を使用すると、レビュー担当者が変更内容を理解するのに役立つ、pull request の概要を生成しやすくなります。

以下の手順に従って、演習のこのセクションを完了します。

1. Web ブラウザーで GitHub リポジトリを開きます。

1. **[pull request]** タブを開きます。

1. **[New pull request](新しい pull request)** を選択します。

1. **[変更の比較]** で、**main** ブランチに **base** が構成されていることを確認します。

1. **[比較]** が **book-availability** ブランチ用に構成されていることを確認します。

    ブランチを構成すると、**main** と **book-availability** の間の変更の比較が表示されます。 CommonActions.cs、ConsoleApp.cs、および JsonDaa.cs ファイルに加えた変更が表示されるはずです。

1. **[pull request の作成]** を選択します。

1. pull request のタイトルを入力します。

    GitHub Copilot Enterprise のサブスクリプションに加入している企業に所属している場合は、GitHub Copilot を使用して pull request の概要を生成できます。 詳細な概要は、変更内容をレビュー担当者が理解するのに役立ちます。

1. GitHub Copilot を使用して、pull request の概要を生成します。

    概要を生成するには、Copilot アクション ボタンを選択し、**[概要]** を選択します。

    [プレビュー] タブを使用して、GitHub Copilot で生成された pull request 概要を確認できます。

1. タイトルと説明にコードの更新が反映されていることを確認し、**[pull request の作成]** を選択します。

1. チェックが完了するのを待ってから、すべてのチェックに合格し、ベース ブランチとの競合がないことを確認します。

    チェックが完了するまでに 1 分かかる場合があります。 変更をメイン ブランチにマージする前に、特定された問題に対処する必要があります。

1. 変更を main ブランチにマージするには、**[pull request をマージする]** を選択し、**[マージの確認]** を選択します。

    変更をマージした後、book-availability ブランチを削除できることに注意してください。

1. Visual Studio Code を開きます。

1. リポジトリの **main** ブランチに切り替えます。

1. リモート リポジトリから変更を**プル**します。

    プロンプトが表示されたら、GitHub アカウントを使用してプルを認証および承認します。

1. **book-availability** ブランチに加えた変更が、**main** ブランチに含まれていることを確認します。

    CommonActions.cs と ConsoleApp.cs ファイルに加えた変更は、main ブランチに存在する必要があります。

#### 概要 – 演習 6

この演習では、コードをテストした後、変更をリポジトリの main ブランチにマージしました。 ローカル リポジトリに変更をコミットし、それらをリモート リポジトリと同期させた後、変更をリポジトリの main ブランチにマージする pull request を作成しました。 GitHub Copilot を使用して pull request の概要を生成し、レビュー担当者が変更内容を理解できるようにしました。


