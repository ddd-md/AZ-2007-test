#### ラボ シナリオ (パート 4)

このラボのパート 4 では、GitHub Copilot を使用して、次のコード セクションをリファクターします。

- リフレクションではなくディクショナリを使用するように EnumHelper クラスをリファクターします。 ディクショナリを使用すると、リフレクションのオーバーヘッドが軽減され、パフォーマンスが向上します。 また、リフレクションを排除すると、コードの可読性、保守性、セキュリティも向上します。

- foreach ループではなく LINQ (統合言語クエリ) を使用するようにデータ アクセス メソッドをリファクターします。 LINQ を使用すると、コレクション、データベース、XML ドキュメントのクエリを実行する、より簡潔で読みやすい方法が提供されます。 LINQ を使用すると、コードの可読性、保守性、パフォーマンスを向上させることができます。

パートには次の演習が含まれています。

- GitHub Copilot を使用して EnumHelper クラスをリファクターします。
- GitHub Copilot を使用して、LINQ によるデータ アクセス クラスを改善します。

### 演習 8: GitHub Copilot を使用して EnumHelper クラスをリファクターする

リフレクションは、実行時にオブジェクトを検査および操作できる強力な機能です。 ただし、リフレクションには時間がかかる可能性があり、リフレクションに関連する潜在的なセキュリティ リスクを考慮する必要があります。

既存の EnumHelper クラスは、リフレクションを使用して列挙値の description 属性を取得します。 リフレクションではなくディクショナリを使用するように EnumHelper クラスをリファクターできます。 ディクショナリを使用すると、パフォーマンスが向上し、リフレクションの使用に関連するセキュリティ上の懸念が解消されます。

この演習では、GitHub Copilot を使用して、EnumHelper クラスのリファクターを支援します。

以下の手順に従ってこの演習を完了します。

1. ソリューション エクスプローラー ビューを使用して、EnumHelper.cs ファイルを開きます。

    EnumHelper.cs は src/Library.ApplicationCore/Enums フォルダーにあります。

    ```csharp

    using System.ComponentModel;
    using System.Reflection;
    namespace Library.ApplicationCore.Enums;
    public static class EnumHelper
    {
        public static string GetDescription(Enum value)
        {
            if (value == null)
                return string.Empty;
            FieldInfo fieldInfo = value.GetType().GetField(value.ToString())!;
            DescriptionAttribute[] attributes =
                (DescriptionAttribute[])fieldInfo.GetCustomAttributes(typeof(DescriptionAttribute), false);
            if (attributes != null && attributes.Length > 0)
            {
                return attributes[0].Description;
            }
            else
            {
                return value.ToString();
            }
        }
    }

    ```

1. チャット ビューを開き、**EnumHelper.cs**、**LoanExtensionStatus.cs**、**LoanReturnStatus.cs**、および **MembershipRenewalStatus.cs** ファイルをチャット コンテキストに追加します。

1. 次のプロンプトを入力します。

    ```plaintext
    @workspace I want to refactor the `EnumHelper` class using dictionaries rather than reflection to get enum description attributes. I want a separate dictionary for each enum. The enum values are in the `LoanExtensionStatus.cs`, `LoanReturnStatus.cs`, and `MembershipRenewalStatus.cs` files. Explain how to update the EnumHelper class to use dictionaries and provide the updated code.
    ```

    このプロンプトは、GitHub Copilot に対し、リフレクションではなくディクショナリを使用するように EnumHelper クラスを更新する方法の説明を求めます。 また、列挙値ごとにディクショナリを使用するように更新されたコードも要求します。

1. 少し時間をとって、GitHub Copilot から提供された説明を確認します。

    説明は次のようなものになります。

    ```markdown
    To refactor the EnumHelper class to use dictionaries for each enum, you need to create a dictionary for each enum type that maps the enum values to their descriptions. This will eliminate the need for reflection at runtime.

    Here's how you can update the EnumHelper class:

    1. Create dictionaries for each enum type.
    2. Populate these dictionaries with the enum values and their corresponding descriptions.
    3. Update the GetDescription method to use these dictionaries.

    Below is the updated code for the EnumHelper class:

    ```

    ```csharp

    using System.ComponentModel;
    using System.Collections.Generic;
    namespace Library.ApplicationCore.Enums
    {
        public static class EnumHelper
        {
            private static readonly Dictionary<MembershipRenewalStatus, string> MembershipRenewalStatusDescriptions = new()
            {
                { MembershipRenewalStatus.Success, "Membership renewal was successful." },
                { MembershipRenewalStatus.PatronNotFound, "Patron not found." },
                { MembershipRenewalStatus.TooEarlyToRenew, "It is too early to renew the membership." },
                { MembershipRenewalStatus.LoanNotReturned, "Cannot renew membership due to an outstanding loan." },
                { MembershipRenewalStatus.Error, "Cannot renew membership due to an error." }
            };
            private static readonly Dictionary<LoanReturnStatus, string> LoanReturnStatusDescriptions = new()
            {
                { LoanReturnStatus.Success, "Book was successfully returned." },
                { LoanReturnStatus.LoanNotFound, "Loan not found." },
                { LoanReturnStatus.AlreadyReturned, "Cannot return book as the book is already returned." },
                { LoanReturnStatus.Error, "Cannot return book due to an error." }
            };
            private static readonly Dictionary<LoanExtensionStatus, string> LoanExtensionStatusDescriptions = new()
            {
                { LoanExtensionStatus.Success, "Book loan extension was successful." },
                { LoanExtensionStatus.LoanNotFound, "Loan not found." },
                { LoanExtensionStatus.LoanExpired, "Cannot extend book loan as it already has expired. Return the book instead." },
                { LoanExtensionStatus.MembershipExpired, "Cannot extend book loan due to expired patron's membership." },
                { LoanExtensionStatus.LoanReturned, "Cannot extend book loan as the book is already returned." },
                { LoanExtensionStatus.Error, "Cannot extend book loan due to an error." }
            };
            public static string GetDescription(Enum value)
            {
                if (value == null)
                    return string.Empty;
                return value switch
                {
                    MembershipRenewalStatus status => MembershipRenewalStatusDescriptions[status],
                    LoanReturnStatus status => LoanReturnStatusDescriptions[status],
                    LoanExtensionStatus status => LoanExtensionStatusDescriptions[status],
                    _ => value.ToString()
                };
            }
        }
    }

    ```

    ```markdown

    This refactored EnumHelper class uses dictionaries to store the descriptions for each enum type. The GetDescription method now uses pattern matching to determine the type of the enum and retrieves the description from the appropriate dictionary. This approach avoids the use of reflection and improves performance.

    ```

    > [!NOTE]
    > GitHub Copilot がディクショナリに正しい説明を事前設定しない場合は、GitHub Copilot 拡張機能が再起動を要求していないことを確認します。 している場合は、拡張機能を再起動し、プロンプトをもう一度試してください。

1. GitHub Copilot が提案する EnumHelper クラスを使用して、EnumHelper.cs ファイルを更新します。

1. 更新された EnumHelper クラスが、LoanExtensionStatus.cs、LoanReturnStatus.cs、および MembershipRenewalStatus.cs ファイルの列挙値を使用していることを確認します。

    各列挙ファイルを開き、ディクショナリ内の列挙値が正しいことを確認します。 不整合がある場合は、GitHub Copilot で各列挙型のディクショナリを個別に更新します。 たとえば、LoanExtensionStatus 列挙型に対して次のプロンプトを使用できます。

    ```plaintext
    @workspace Use the description values in LoanExtensionStatus.cs to update the LoanExtensionStatus dictionary in the EnumHelper class. Provide the updated code for the LoanExtensionStatus dictionary in the EnumHelper class.
    ```

    必要に応じて、LoanReturnStatus 列挙型と MembershipRenewalStatus 列挙型に対して同じアプローチを使用します。

1. 少し時間を取って **GetDescription** メソッドを確認してください。

    ```csharp

    public static string GetDescription(Enum value)
    {
        if (value == null)
            return string.Empty;
        return value switch
        {
            MembershipRenewalStatus status => MembershipRenewalStatusDescriptions[status],
            LoanReturnStatus status => LoanReturnStatusDescriptions[status],
            LoanExtensionStatus status => LoanExtensionStatusDescriptions[status],
            _ => value.ToString()
        };
    }

    ```

    このコードは、パターン マッチングを使用して列挙型の型を判断し、適切なディクショナリから説明を取得します。 switch ステートメントは列挙値の型をチェックし、ディクショナリから対応する説明を返します。 列挙値がディクショナリ内に見つからない場合、このメソッドは列挙値を文字列として返します。

    GitHub Copilot にこのコードをリファクターしてラムダ式を削除するように依頼すると、読みやすくなります。

    ```csharp

    public static string GetDescription(Enum value)
    {
        if (value == null)
            return string.Empty;
        switch (value)
        {
            case MembershipRenewalStatus status:
                return MembershipRenewalStatusDescriptions[status];
            case LoanReturnStatus status:
                return LoanReturnStatusDescriptions[status];
            case LoanExtensionStatus status:
                return LoanExtensionStatusDescriptions[status];
            default:
                return value.ToString();
        }
    }

    ```

1. 変更内容を EnumHelper.cs ファイルに保存します。

1. ソリューションを構築してエラーがないことを確認します。

    警告が表示されます。 今のところは無視しても構いません。

#### まとめ - 演習 8

この演習では、GitHub Copilot を使用して、リフレクションではなくディクショナリを使用するように EnumHelper クラスをリファクターしました。 ディクショナリを使用すると、リフレクションのオーバーヘッドが軽減され、パフォーマンスが向上します。 また、リフレクションを排除すると、コードの可読性、保守性、セキュリティも向上します。

### 演習 9: GitHub Copilot を使用して LINQ によるデータ アクセス クラスを改善する

LINQ (言語統合クエリ) は、コレクション、データベース、XML ドキュメントに対し統一した方法でクエリを実行できるようにする C# の強力な機能です。 LINQ は、従来の foreach ループと比較して、データのクエリを実行するためのより簡潔で読みやすい方法を提供します。

この演習には、次のタスクが含まれています。

- LINQ を使用するように JsonData.cs のメソッドをリファクターします。
- LINQ を使用するように JsonLoanRepository.cs のメソッドをリファクターします。
- LINQ を使用するように JsonPatronRepository.cs のメソッドをリファクターします。

#### タスク 1: LINQ を使用するように JsonData.cs のメソッドをリファクターする

JsonData クラスには、次のデータ アクセス メソッドが含まれています。GetPopulatedPatron、GetPopulatedLoan、GetPopulatedBookItem、GetPopulatedBook。 これらのメソッドは、foreach ループを使用してコレクションを反復処理し、オブジェクトを設定します。 これらのメソッドをリファクターして LINQ を使用すると、コードの可読性と保守性が向上します。

以下の手順に従って、演習のこのセクションを完了します。

1. JsonData.cs ファイルを開きます。

1. **GetPopulatedPatron** メソッドを選択します。

    GetPopulatedPatron メソッドは、完全に事前設定された Patron オブジェクトを作成するように設計されています。 これは、Patron の基本プロパティをコピーし、その Loans コレクションを詳細な Loans オブジェクトを使用して事前設定します。

    ```csharp

    public Patron GetPopulatedPatron(Patron p)
    {
        Patron populated = new Patron
        {
            Id = p.Id,
            Name = p.Name,
            ImageName = p.ImageName,
            MembershipStart = p.MembershipStart,
            MembershipEnd = p.MembershipEnd,
            Loans = new List<Loan>()
        };
        foreach (Loan loan in Loans!)
        {
            if (loan.PatronId == p.Id)
            {
                populated.Loans.Add(GetPopulatedLoan(loan));
            }
        }
        return populated;
    }

    ```

1. インライン チャットを開き、LINQ を使用してメソッドをリファクターするプロンプトを入力します。

    ```plaintext
    #selection refactor selection to `return new Patron` using LINQ
    ```

1. 少し時間を取って、おすすめの更新を確認します。

    おすすめの更新は、次のコードのようになります。

    ```csharp

    public Patron GetPopulatedPatron(Patron p)
    {
        return new Patron
        {
            Id = p.Id,
            Name = p.Name,
            ImageName = p.ImageName,
            MembershipStart = p.MembershipStart,
            MembershipEnd = p.MembershipEnd,
            Loans = Loans!
                .Where(loan => loan.PatronId == p.Id)
                .Select(GetPopulatedLoan)
                .ToList()
        };
    }

    ```

    最初に、LINQ クエリを使用して"foreach (Loan loan in Loans!)" ループを置き換えます。 LINQ コードを配置すると、オブジェクト初期化子を使用して、オブジェクト プロパティを新しい Patron オブジェクトに割り当てることができます。 こうすることで、Patron オブジェクトの個別の事前設定インスタンスの必要性が軽減されます。 全体的に、更新されたコードは短くなり、読みやすくなっています。

    このコードは、入力された利用者 "p" を使用して、いくつかの基本プロパティを新しい Patron オブジェクトに割り当てます。 次に、入力された利用者に関連付けられた貸し出しを使用して Loans コレクションを事前設定し、GetPopulatedLoan メソッドを使用して各貸し出しを変換します。

    Loans コレクションを事前設定する LINQ コード行は次のように分解できます。

    - `Loans!`: "Loans!" 式は Loans コレクションにアクセスします。 "!" 演算子は null 免除演算子であり、Loans が null ではないと開発者が確信していることを示します。 GetPopulatedPatron メソッドを呼び出す前に、Loans を適切に初期化する必要があります。
    - `.Where(loan => loan.PatronId == p.Id)`: このコードは、入力された利用者 "p" に属するもののみを含むように、貸し出しをフィルター処理します。
    - `.Select(GetPopulatedLoan)`: このコードは、GetPopulatedLoan メソッドを使用して、フィルター処理された各貸し出しを変換します。
    - `.ToList()`: 結果を Loan オブジェクトのリストに変換します。

1. おすすめの更新を受け入れるには、**[同意する]** を選択します。

    GetPopulatedPatron メソッドをリファクターする方法を見てきましたが、LINQ を使用するように GetPopulatedLoan、GetPopulatedBookItem、および GetPopulatedBook メソッドをリファクターすることもできます。

1. 同じ方法で、**GetPopulatedLoan**、**GetPopulatedBookItem**、および **GetPopulatedBook** メソッドをリファクターします。

    たとえば、次のプロンプトを使用して 3 つのメソッドをリファクターします。

    **GetPopulatedLoan** メソッドの場合:

    ```plaintext
    #selection refactor selection to `return new Loan` using LINQ. Use `GetPopulatedBookItem` for the `BookItem` property. Use `Single` for BookItem and Patron properties.
    ```

    **GetPopulatedBookItem** メソッドの場合:

    ```plaintext
    #selection refactor selection to `return new BookItem` using LINQ. Use `GetPopulatedBook` and `Single` for the `BookItem` property.
    ```

    **GetPopulatedBook** メソッドの場合:

    ```plaintext
    #selection refactor selection to `return new Book` using LINQ. Use `Where` and `Select` for `Author` property. Use `First` author.
    ```

1. おすすめの更新を受け入れたら、少し時間を取って、コードの変更を確認します。

    更新されたコードは、次のコードのようになります。

    ```csharp

    public Loan GetPopulatedLoan(Loan l)
    {
        return new Loan
        {
            Id = l.Id,
            BookItemId = l.BookItemId,
            PatronId = l.PatronId,
            LoanDate = l.LoanDate,
            DueDate = l.DueDate,
            ReturnDate = l.ReturnDate,
            BookItem = GetPopulatedBookItem(BookItems!.Single(bi => bi.Id == l.BookItemId)),
            Patron = Patrons!.Single(p => p.Id == l.PatronId)
        };
    }
    public BookItem GetPopulatedBookItem(BookItem bi)
    {
        return new BookItem
        {
            Id = bi.Id,
            BookId = bi.BookId,
            AcquisitionDate = bi.AcquisitionDate,
            Condition = bi.Condition,
            Book = GetPopulatedBook(Books!.Single(b => b.Id == bi.BookId))
        };
    }
    public Book GetPopulatedBook(Book b)
    {
        return new Book
        {
            Id = b.Id,
            Title = b.Title,
            AuthorId = b.AuthorId,
            Genre = b.Genre,
            ISBN = b.ISBN,
            ImageName = b.ImageName,
            Author = Authors!.Where(a => a.Id == b.AuthorId).Select(a => new Author {
                Id = a.Id,
                Name = a.Name
            }).First()
        };
    }

    ```

1. LINQ クエリの説明を表示するには、**Explain** スマート アクションを使用します。

    **Explain** スマート アクションは、コードで使用されている LINQ クエリの詳細な説明を提供します。

    たとえば、GetPopulatedBook メソッドで **Explain** スマート アクションを使用すると、Book オブジェクトの Author プロパティを事前設定するのに使用される LINQ クエリの説明を表示できます。

    ```csharp

    Author = Authors!.Where(a => a.Id == b.AuthorId).Select(a => new Author {
        Id = a.Id,
        Name = a.Name
    }).First()

    ```

    **Explain** スマート アクションは、Book オブジェクトの Author プロパティを事前設定するのに使用される LINQ クエリの詳細な説明を提供します。

    たとえば、説明は次のようになります。

    ```plaintext
    The active selection is a C# code snippet that assigns a value to the Author property. This value is derived from a collection of Author objects named Authors. The code uses LINQ (Language Integrated Query) to filter and transform the data within this collection.
    
    First, the Authors! expression uses the null-forgiving operator (!) to indicate that Authors is not null, even if the compiler might think otherwise. This is a way to suppress nullable warnings. The Where method is then called on the Authors collection to filter the elements. The lambda expression a => a.Id == b.AuthorId is used to find all Author objects where the Id matches the AuthorId property of another object b.
    
    After filtering, the Select method is used to project each filtered Author object into a new Author object. This is done by creating a new instance of the Author class and copying the Id and Name properties from the original Author object.
    
    Finally, the First method is called to retrieve the first element from the resulting sequence. This means that the Author property will be assigned the first Author object that matches the filter criteria and has been projected into a new Author instance.
    
    This approach ensures that the Author property is set to a new Author object with the same Id and Name as the first matching Author in the Authors collection.
    ```

1. 変更内容を JsonData.cs ファイルに保存します。

1. ソリューションを構築してエラーがないことを確認します。

    警告が表示されます。 今のところは無視しても構いません。

#### タスク 2: LINQ を使用するように JsonLoanRepository.cs のメソッドをリファクターする

JsonLoanRepository クラスには、GetLoan および UpdateLoan データ アクセス メソッドが含まれています。 これら 2 つのメソッドをリファクターし、foreach ループを LINQ に置き換えて、コードの可読性と保守性を向上させます。

以下の手順に従って、演習のこのセクションを完了します。

1. **JsonLoanRepository.cs** ファイルを開きます。

1. **GetLoan** メソッドを選択します。

    GetLoan メソッドは、ID によって貸し出しを取得するように設計されています。

    ```csharp

    public async Task<Loan?> GetLoan(int id)
    {
        await _jsonData.EnsureDataLoaded();
        foreach (Loan loan in _jsonData.Loans!)
        {
            if (loan.Id == id)
            {
                Loan populated = _jsonData.GetPopulatedLoan(loan);
                return populated;
            }
        }
        return null;
    }

    ```

1. インライン チャットを開き、LINQ を使用してメソッドをリファクターするプロンプトを入力します。

    たとえば、次のプロンプトを入力します。

    ```plaintext
    #selection refactor selection using LINQ with `_jsonData.Loans!. Use `Where`, `Select` and `GetPopulatedLoan` to return `FirstOrDefault`.
    ```

1. 少し時間を取って、おすすめの更新を確認します。

    おすすめの更新は、次のコードのようになります。

    ```csharp

    public async Task<Loan?> GetLoan(int id)
    {
        await _jsonData.EnsureDataLoaded();
        Loan? loan = _jsonData.Loans!
            .Where(l => l.Id == id)
            .Select(l => _jsonData.GetPopulatedLoan(l))
            .FirstOrDefault();
        return loan;
    }

    ```

    更新されたコードでは、LINQ を使用してローンのコレクションをフィルター処理し、指定された ID を持つローンのみを含めます。 貸し出しは Null 許容 (`Loan? loan`) として宣言する必要があることに注意してください。 次に、GetPopulatedLoan メソッドを使用して貸し出しを変換し、最初の結果を返します。 一致する貸し出しが見つからない場合、FirstOrDefault は null 値を返します。 このメソッドは、指定された "id" の貸し出しが存在しない場合は null 値となる可能性がある、この貸し出しオブジェクトを返します。 このアプローチにより、返されたローンには必要なすべての関連データが完全に設定され、ローンのレコードの包括的なビューが提供されます。

    このコードはさらに簡略化できます。

    ```csharp

    public async Task<Loan?> GetLoan(int id)
    {
        await _jsonData.EnsureDataLoaded();
        return _jsonData.Loans!
            .Where(l => l.Id == id)
            .Select(l => _jsonData.GetPopulatedLoan(l))
            .FirstOrDefault();
    }

    ```

1. 更新された GetLoan メソッドを受け入れるには、**[Accept]** を選択します。

1. **UpdateLoan** メソッドを選択します。

    ```csharp

    public async Task UpdateLoan(Loan loan)
    {
        Loan? existingLoan = null;
        foreach (Loan l in _jsonData.Loans!)
        {
            if (l.Id == loan.Id)
            {
                existingLoan = l;
                break;
            }
        }
        if (existingLoan != null)
        {
            existingLoan.BookItemId = loan.BookItemId;
            existingLoan.PatronId = loan.PatronId;
            existingLoan.LoanDate = loan.LoanDate;
            existingLoan.DueDate = loan.DueDate;
            existingLoan.ReturnDate = loan.ReturnDate;
            await _jsonData.SaveLoans(_jsonData.Loans!);
            await _jsonData.LoadData();
        }
    }

    ```

1. インライン チャットを開き、LINQ を使用してメソッドをリファクターするプロンプトを入力します。

    たとえば、次のプロンプトを入力します。

    ```plaintext
    #selection refactor selection using LINQ find an existing loan `_jsonData.Loans!. Replace existing loan.
    ```

1. 少し時間を取って、おすすめの更新を確認します。

    おすすめの更新は、次のコードのようになります。

    ```csharp

    public async Task UpdateLoan(Loan loan)
    {
        Loan? existingLoan = _jsonData.Loans!.FirstOrDefault(l => l.Id == loan.Id);
        if (existingLoan != null)
        {
            existingLoan.BookItemId = loan.BookItemId;
            existingLoan.PatronId = loan.PatronId;
            existingLoan.LoanDate = loan.LoanDate;
            existingLoan.DueDate = loan.DueDate;
            existingLoan.ReturnDate = loan.ReturnDate;
            await _jsonData.SaveLoans(_jsonData.Loans!);
            await _jsonData.LoadData();
        }
    }

    ```

    更新されたコードでは、LINQ を使用してローンのコレクション内の既存のローンを検索します。 次に、既存のローンを新しいローン データで更新します。 次に、このメソッドは更新されたローンのコレクションを保存し、データを再読み込みします。 このアプローチにより、ローン データが正しく更新され、変更がデータ ストアに保持されることが保証されます。

    メソッドが実行される前にデータが確実に読み込まれるようにするコードを追加することもできます。

    ```csharp

    public async Task UpdateLoan(Loan loan)
    {
        await _jsonData.EnsureDataLoaded();
        Loan? existingLoan = _jsonData.Loans!.FirstOrDefault(l => l.Id == loan.Id);
        if (existingLoan != null)
        {
            existingLoan.BookItemId = loan.BookItemId;
            existingLoan.PatronId = loan.PatronId;
            existingLoan.LoanDate = loan.LoanDate;
            existingLoan.DueDate = loan.DueDate;
            existingLoan.ReturnDate = loan.ReturnDate;
            await _jsonData.SaveLoans(_jsonData.Loans!);
            await _jsonData.LoadData();
        }
    }

    ```

1. 更新された UpdateLoan メソッドを受け入れるには、**[Accept]** を選択します。

1. 変更内容を JsonLoanRepository.cs ファイルに保存します。

1. ソリューションを構築してエラーがないことを確認します。

    警告が表示されます。 今のところは無視しても構いません。

#### タスク 3: LINQ を使用するように JsonPatronRepository のメソッドをリファクターする

JsonPatronRepository クラスには、SearchPatrons、GetPatron、および UpdatePatron メソッドが含まれており、最適化の有力な候補です。 これらのメソッドをリファクターし、foreach ループを LINQ に置き換えて、コードの可読性と保守性を向上させます。

以下の手順に従って、演習のこのセクションを完了します。

1. **JsonPatronRepository.cs** ファイルを開きます。

1. **SearchPatrons** メソッドを選択します。

    SearchPatrons メソッドは、利用者を名前で検索するように設計されています。

    ```csharp

    public async Task<List<Patron>> SearchPatrons(string searchInput)
    {
        await _jsonData.EnsureDataLoaded();
        List<Patron> searchResults = new List<Patron>();
        foreach (Patron patron in _jsonData.Patrons)
        {
            if (patron.Name.Contains(searchInput))
            {
                searchResults.Add(patron);
            }
        }
        searchResults.Sort((p1, p2) => String.Compare(p1.Name, p2.Name));
        searchResults = _jsonData.GetPopulatedPatrons(searchResults);
        return searchResults;
    }

    ```

1. インライン チャットを開き、LINQ を使用してメソッドをリファクターするプロンプトを入力します。

    たとえば、次のプロンプトを入力します。

    ```plaintext
    #selection refactor selection using LINQ with `_jsonData.Patrons!. Replace the loop with `Where`, `OrderBy`, and `GetPopulatedPatrons`.
    ```

1. 少し時間を取って、おすすめの更新を確認します。

    おすすめの更新は、次のコードのようになります。

    ```csharp

    public async Task<List<Patron>> SearchPatrons(string searchInput)
    {
        await _jsonData.EnsureDataLoaded();
        List<Patron> searchResults = _jsonData.Patrons!
            .Where(patron => patron.Name.Contains(searchInput))
            .OrderBy(patron => patron.Name)
            .ToList();
        searchResults = _jsonData.GetPopulatedPatrons(searchResults);
        return searchResults;
    }

    ```

1. 更新された SearchPatrons メソッドを受け入れるには、**[Accept]** を選択します。

1. **GetPatron** メソッドを選択します。

    GetPatron メソッドは、指定した "id" に一致する利用者を返すように設計されています。

    ```csharp

    public async Task<Patron?> GetPatron(int id)
    {
        await _jsonData.EnsureDataLoaded();
        foreach (Patron patron in _jsonData.Patrons!)
        {
            if (patron.Id == id)
            {
                Patron populated = _jsonData.GetPopulatedPatron(patron);
                return populated;
            }
        }
        return null;
    }

    ```

1. インライン チャットを開き、LINQ を使用してメソッドをリファクターするプロンプトを入力します。

    たとえば、次のプロンプトを入力します。

    ```plaintext
    #selection refactor selection using LINQ with `_jsonData.Patrons!. Use `Where`, `Select` and `GetPopulatedPatron` to return `FirstOrDefault`
    ```

1. 少し時間を取って、おすすめの更新を確認します。

    おすすめの更新は、次のコードのようになります。

    ```csharp

    public async Task<Patron?> GetPatron(int id)
    {
        await _jsonData.EnsureDataLoaded();
        var patron = _jsonData.Patrons!
            .Where(p => p.Id == id)
            .Select(p => _jsonData.GetPopulatedPatron(p))
            .FirstOrDefault();
        return patron;
    }

    ```

    このコードはさらに簡略化できます。

    ```C#

    public async Task<Patron?> GetPatron(int id)
    {
        await _jsonData.EnsureDataLoaded();
        return _jsonData.Patrons!
            .Where(p => p.Id == id)
            .Select(p => _jsonData.GetPopulatedPatron(p))
            .FirstOrDefault();
    }

    ```

1. 更新された GetPatron メソッドを受け入れるには、**[Accept]** を選択します。

1. **UpdatePatron** メソッドを選択します。

    UpdatePatron メソッドは、指定した "id" の利用者を更新するように設計されています。

    ```csharp

    public async Task UpdatePatron(Patron patron)
    {
        await _jsonData.EnsureDataLoaded();
        var patrons = _jsonData.Patrons!;
        Patron existingPatron = null;
        foreach (var p in patrons)
        {
            if (p.Id == patron.Id)
            {
                existingPatron = p;
                break;
            }
        }
        if (existingPatron != null)
        {
            existingPatron.Name = patron.Name;
            existingPatron.ImageName = patron.ImageName;
            existingPatron.MembershipStart = patron.MembershipStart;
            existingPatron.MembershipEnd = patron.MembershipEnd;
            existingPatron.Loans = patron.Loans;
            await _jsonData.SavePatrons(patrons);
            await _jsonData.LoadData();
        }
    }

    ```

1. インライン チャットを開き、LINQ を使用してメソッドをリファクターするプロンプトを入力します。

    たとえば、次のプロンプトを入力します。

    ```plaintext
    #selection refactor selection using LINQ to find `patron` in `_jsonData.Patrons!. Replace existing patron with `patron`.
    ```

1. 少し時間を取って、おすすめの更新を確認します。

    おすすめの更新は、次のコードのようになります。

    ```csharp

    public async Task UpdatePatron(Patron patron)
    {
        await _jsonData.EnsureDataLoaded();
        var patrons = _jsonData.Patrons!;
        var existingPatron = patrons.FirstOrDefault(p => p.Id == patron.Id);
        if (existingPatron != null)
        {
            existingPatron.Name = patron.Name;
            existingPatron.ImageName = patron.ImageName;
            existingPatron.MembershipStart = patron.MembershipStart;
            existingPatron.MembershipEnd = patron.MembershipEnd;
            existingPatron.Loans = patron.Loans;
            await _jsonData.SavePatrons(patrons);
            await _jsonData.LoadData();
        }
    }

    ```

1. 更新された UpdatePatron メソッドを受け入れるには、**[Accept]** を選択します。

#### タスク 4:作業を確認する

作業内容を確認するには、以下の手順を実行します。

1. ソリューションをクリーンアップするには、**AccelerateAppDevGitHubCopilot** を右クリックし、**[Clean]** を選択します。

    このアクションにより、以前のビルドからビルド成果物が削除されます。 ソリューションをクリーンすると、JSON データ ファイルが (出力ディレクトリ内で) 元の値に効率的にリセットされます。

1. アプリケーションをビルドして、エラーがないことを確認します。

1. アプリケーションを実行します。

    **Library.Console** プロジェクトを右クリックし、**[デバッグ]** を選択して、**[新しいインスタンスの開始]** を選択することで、ソリューション エクスプローラー ビューからアプリケーションを実行できます。

1. パトロン名の入力を求められたら、「**One**」と入力して Enter キーを押します。

1. "Matching Patrons" プロンプトで「**2**」と入力し、Enter キーを押します。

1. "Book Loans" プロンプトで「**1**」と入力し、Enter キーを押します。

1. "Input Options" プロンプトで「**r**」と入力し、Enter キーを押します。

1. **"Book was successfully returned."** (書籍は問題なく返却されました) というメッセージが表示されることを 確認します。

1. 新しい検索を開始するには、「**s**」と入力して Enter キーを押します。

1. パトロン名の入力を求められたら、「**One**」と入力して Enter キーを押します。

1. "Matching Patrons" プロンプトで「**2**」と入力し、Enter キーを押します。

1. 最初の書籍貸出が **Returned: True** とマークされていることを確認します。

1. "Input Options" プロンプトで「**q**」と入力し、Enter キーを押します。

1. アプリケーションを停止します。

#### まとめ - 演習 9

この演習では、LINQ を使用するように JSONData、JsonLoanRepository、および JsonPatronRepository クラスのデータ アクセス メソッドをリファクターしました。 これらの変更により、コードの可読性、保守性、パフォーマンスが向上します。
