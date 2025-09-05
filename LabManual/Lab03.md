## パート 3: GitHub Copilot ツールを使用して単体テストを開発する

### 開始する前に

#### プロジェクトの概要

同僚が UnitTests という名前のテスト プロジェクトを作成しました。 既存の単体テストでは、Library.ApplicationCore プロジェクトのサービス クラスが部分的にカバーされています。 Library.ApplicationCore プロジェクトには、ライブラリ管理システムのビジネス ロジックが含まれています。 UnitTests プロジェクトでは、xUnit フレームワークを使用して単体テストを記述して実行します。 また、このプロジェクトでは NSubstitute ライブラリを使用して、テスト用のモック オブジェクトを作成します。

UnitTests プロジェクトを拡張して、Library.Infrastructure プロジェクトのデータ アクセス クラスをカバーする必要があります。


#### ラボ シナリオ (パート 3)

GitHub Copilot は、コードの単体テストを記述するのに役立ちます。 GitHub Copilot を使用して単体テストを生成するには、いくつかの方法があります。

- **テスト ケースの生成**:GitHub Copilot を使用して、コードのテスト ケースを生成できます。 Copilot は、記述したコードに基づいてテスト ケースを提案できます。 その後、これらのテスト ケースを使用して、コードの単体テストを作成できます。
- **テスト メソッドの生成**:Copilot では、コードのテスト メソッドを生成することもできます。 これらのテスト メソッドを使用して、コードの単体テストを作成できます。
- **テスト アサーションの生成**:Copilot は、単体テストで使用できるアサーションを提案できます。 これらのアサーションは、コードの動作を確認するのに役立ちます。
- **テスト モックの生成**:Copilot では、単体テストで使用できるモック オブジェクトを生成できます。 これらのモック オブジェクトは、テストするコードを依存関係から分離するのに役立ちます。
- **テスト データの生成**:Copilot では、単体テストで使用できるテスト データを生成できます。 このテスト データは、さまざまな入力でコードをテストするのに役立ちます。
- **テスト セットアップ コードの生成**:Copilot では、単体テストのセットアップ コードを生成できます。 このコードは、テストを実行する前にテスト環境を設定するのに役立ちます。
- **テスト ティアダウン コードの生成**:Copilot では、単体テストのティアダウン コードを生成できます。 このコードは、テストの実行後にテスト環境をクリーンアップするのに役立ちます。

このラボのパート 3 では、Library.Infrastructure プロジェクトの単体テストを開発します。

パートには、次の演習が含まれます。

- GitHub Copilot を使用して UnitTests プロジェクトを評価および拡張します。

### 演習 7: GitHub Copilot を使用して UnitTests プロジェクトを評価および拡張する

この演習では、GitHub Copilot を使用して次のタスクを完了します。

1. GitHub Copilot を使用して、既存の UnitTests プロジェクトを評価します。

1. UnitTests プロジェクトを拡張してデータ アクセス クラスのテストを開始します。

#### タスク 1: GitHub Copilot を使用して既存の UnitTests プロジェクトを評価する

現在、UnitTests プロジェクトは次のフォルダー構造を実装しています。

- UnitTests\
  - ApplicationCore\
    - LoanService\
      - ExtendLoan.cs
      - ReturnLoan.cs
    - PatronService\
      - RenewMembership.cs
  - LoanFactory.cs
  - PatronFactory.cs

この構造は、ApplicationCore プロジェクトの Services 部分をミラー化し、サポートします。

- ApplicationCore\
  - Services\
    - LoanService.cs: ExtendLoan メソッドと ReturnLoan メソッドが含まれます。
    - PatronService.cs: RenewMembership メソッドが含まれます。

既存の単体テストを評価すると、Library.Infrastructure プロジェクトのデータ アクセス クラスの単体テストを開発するのに役立ちます。

以下の手順に従って、演習のこのセクションを完了します。

1. エディターで開いているすべてのファイルを閉じます。

1. チャット ビューを開きます。

1. チャット コンテキストに次のファイルを追加します: **LoanFactory.cs**、**PatronFactory.cs**、**ExtendLoan.cs**、**ReturnLoan.cs**、**RenewMembership.cs**、**LoanService.cs**、**PatronService.cs**。

    ファイルは、"tests/UnitTests"、"tests/UnitTests/ApplicationCore/LoanService"、"tests/UnitTests/ApplicationCore/PatronService"、および "src/Library.ApplicationCore/Services" フォルダーにあります。

1. 次のプロンプトを入力します。

    ```plaintext
    @workspace Explain the approach to unit testing that's been implemented in this workspace.
    ```

    このプロンプトは、ワークスペースのコンテキストでの単体テストのアプローチについて説明するように GitHub Copilot に求めます。 応答のコンテキストを提供するために、プロンプトにはいくつかのファイルが含まれています。

1. 少し時間を取って、GitHub Copilot からの応答を確認してください。

    次の説明に似た情報が提示されます。

    - モックの依存関係:テストでは、NSubstitute を使用してインターフェイスのモック実装を作成します。
    - テストの編成:テストは、テストされるメソッドに基づいて個別のファイルに編成されます。 たとえば、ExtendLoan メソッドと ReturnLoan メソッドは、LoanService フォルダーの下で個別のファイルでテストされます。
    - アサーション:アサーションは、各テストの予想される結果を検証するために使用されます。
    - テスト シナリオ:テストでは、入力に基づいて異なる状態を返すなど、メソッドごとにさまざまなシナリオがカバーされています。
    - 分離性: 各テストは、副作用を防ぐために他のテストから分離されます。
    - ファクトリ メソッド:ファクトリ メソッドは、テスト データを一貫して作成するために使用されます。

1. 次のプロンプトを入力します。

    ```plaintext
    @workspace What are the benefits of this approach to unit testing?
    ```

1. 少し時間を取って、GitHub Copilot からの応答を確認してください。

    次に挙げるような利点の一覧を示す説明が表示されます。

    - 依存関係の分離:モックを使用すると、作業単位が依存関係から分離されます。
    - 再現性と一貫性:ファクトリ メソッドは、テスト データを作成するための一貫した方法を提供します。
    - 読みやすさと保守容易性:記述的なテスト メソッドとテスト ステップの明確な配置により、テストの読みやすさが向上します。
    - 包括的なカバレッジ:さまざまなシナリオをテストすることで、コードが堅牢になり、さまざまなエッジ ケースを確実に処理できます。
    - 懸念事項の分離:テスト ロジック自体からテスト データの作成を分離すると、テストがクリーンに保たれ、テスト対象の動作に集中できます。
    - 即時のフィードバック:単体テストを実行すると、コードの正確性に関するフィードバックがすぐに得られます。

1. チャット コンテキストに次のファイルを追加します: **ExtendLoan.cs**、**ReturnLoan.cs**、**RenewMembership.cs**、**LoanFactory.cs**、**PatronFactory.cs**、**LoanService.cs**、**PatronService.cs**。

1. 次のプロンプトを入力します。

    ```plaintext
    @workspace How can I extend the `UnitTests` project to begin testing methods in the `Library.Infrastructure` project? Provide a process overview.
    ```

    このプロンプトは、UnitTests プロジェクトを拡張して、Library.Infrastructure プロジェクトでメソッドのテストを開始するためのプロセスの概要を提供するよう GitHub Copilot に求めます。 応答のコンテキストを提供するために、プロンプトにはいくつかのファイルが含まれています。

1. 少し時間を取って、GitHub Copilot からの応答を確認してください。

    次の説明に似た情報が提示されます。

    ```markdown
    1. Add Project Reference:

      - Open the UnitTests.csproj file located at tests/UnitTests/UnitTests.csproj.
      - Add a reference to the Library.Infrastructure project.

    2. Identify methods to test:

      - Identify the methods in the Library.Infrastructure project that need to be tested. For example, if you want to test the LoanService class, identify the methods in the LoanService class that need to be tested.

    3. Create Test Classes:

      - Create new test classes in the tests/UnitTests directory to test the methods in the Library.Infrastructure project. For example, if you want to test LoanService, create a file named LoanServiceTests.cs.

    4. Write Unit Tests:

      - In the newly created test classes, write unit tests for the methods in the Library.Infrastructure project. Use the xUnit framework for writing tests and NSubstitute for mocking dependencies.

    5. Run Tests:

      - Use Visual Studio Code's integrated terminal or the Test Explorer to run the tests.
      - Ensure that the tests are discovered and executed correctly.
    ```

#### タスク 2: UnitTests プロジェクトを拡張してデータ アクセス クラスのテストを開始する

演習のこのセクションでは、GitHub Copilot を使用して、Library.Infrastructure プロジェクトの単体テストを作成します。

Library.Infrastructure プロジェクトには、ファイル システムを操作して、データを読み込んで保存するデータ アクセス クラスが含まれています。 プロジェクトには、次のクラスが含まれています。

- JsonData:JSON データを読み込んで保存するクラス。
- JsonLoanRepository:ILoanRepository インターフェイスを実装し、JsonData クラスを使用して貸し出しデータを読み込んで保存するクラス。
- JsonPatronRepository:IPatronRepository インターフェイスを実装し、JsonData クラスを使用して利用者データを読み込んで保存するクラス。

まず、JsonLoanRepository クラスの単体テストを記述します。

以下の手順に従って、演習のこのセクションを完了します。

1. プロジェクト ファイル **UnitTests.csproj** をチャット コンテキストに追加します。

1. チャット ビューで次のプロンプトを入力します。

    ```plaintext
    @workspace Explain how to add a reference to the Library.Infrastructure project inside `UnitTests.csproj`.
    ```

    このプロンプトは、GitHub Copilot に、UnitTests.csproj ファイル内に Library.Infrastructure プロジェクトへの参照を追加する方法を説明するように求めます。

1. GitHub Copilot の応答を使用して、UnitTests.csproj ファイルを更新します。

    更新された UnitTests.csproj ファイルは、次の XML コードのようになります。

    ```xml

    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <TargetFramework>net8.0</TargetFramework>
        <ImplicitUsings>enable</ImplicitUsings>
        <Nullable>enable</Nullable>
        <IsPackable>false</IsPackable>
        <IsTestProject>true</IsTestProject>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="coverlet.collector" Version="6.0.0" />
        <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.8.0" />
        <PackageReference Include="NSubstitute" Version="5.1.0" />
        <PackageReference Include="xunit" Version="2.5.3" />
        <PackageReference Include="xunit.runner.visualstudio" Version="2.5.3" />
      </ItemGroup>
      <ItemGroup>
        <Using Include="Xunit" />
      </ItemGroup>
      <ItemGroup>
        <ProjectReference Include="..\..\src\Library.ApplicationCore\Library.ApplicationCore.csproj" />
        <ProjectReference Include="..\..\src\Library.Infrastructure\Library.Infrastructure.csproj" />
      </ItemGroup>
    </Project>

    ```

1. **JsonLoanRepository.cs** ファイルを開きます。

    JsonLoanRepository.csは src/Library.Infrastructure/Data/ フォルダーにあります。

1. 少し時間を取って **JsonLoanRepository.cs** ファイルを確認してください。

    ```csharp

    using Library.ApplicationCore;
    using Library.ApplicationCore.Entities;
    namespace Library.Infrastructure.Data;
    public class JsonLoanRepository : ILoanRepository
    {
        private readonly JsonData _jsonData;
        public JsonLoanRepository(JsonData jsonData)
        {
            _jsonData = jsonData;
        }
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
    }

    ```

1. **JsonLoanRepository** クラスに関する次の詳細に注意してください。

    - JsonLoanRepository クラスには、GetLoan と UpdateLoan という 2 つのメソッドが含まれています。
    - JsonLoanRepository クラスは、JsonData オブジェクトを使用して貸し出しデータの読み込みおよび保存を行います。

    まず、GetLoan メソッドの単体テストを記述します。

1. **UnitTests** プロジェクトの下に次のフォルダー構造を作成します。

    - Infrastructure\
        - JsonLoanRepository\

    このフォルダー構造は、Library.ApplicationCore 単体テストに使用されるアプローチを反映しています。

1. JsonLoanRepository フォルダーに、**GetLoan** という名前のクラス ファイルを作成します。

1. 少し時間を取って、GetLoan 単体テストのフィールド コンストラクターとクラス コンストラクターの要件を検討しましょう。

    JsonLoanRepository.GetLoan メソッドは、呼び出されたときに貸し出し ID パラメーターを受け取ります。 このメソッドでは、"_jsonData.EnsureDataLoaded" を使用して最新の JSON データを取得し、"_jsonData.Loans" を使用して一致する貸し出しを検索します。 このメソッドで一致する貸し出し ID が見つかると、事前設定された貸し出しオブジェクト (populated) が返されます。 メソッドで一致する貸し出し ID を見つけることができない場合は、null 値が返されます。

    GetLoan 単体テストの場合:

    - モック貸し出しリポジトリ オブジェクト ("_mockLoanRepository") を使用すると、一致する ID が見つかったケースをテストできます。 検索する ID を使用してモックを読み込みます。 ReturnLoanTest クラスは、ILoanRepository インターフェイスをモックし、モック貸し出しリポジトリ オブジェクトをインスタンス化する方法を示します。

    - 非モック貸し出しリポジトリ オブジェクト ("_jsonLoanRepository") を使用すると、一致する ID が見つからないケースをテストできます。 ファイルに含まれていないことがわかっている貸し出し ID を指定するだけです (100 を超えるものが機能するはずです)。

    - 非モック JsonLoanRepository オブジェクトを作成するには、JsonData オブジェクトが必要です。 UnitTests プロジェクトは ConsoleApp プロジェクトによって作成された JsonData オブジェクトにアクセスできないため、IConfiguration インターフェイスを使用してオブジェクトを作成する必要があります。

1. チャット コンテキストに、**JsonLoanRepository.cs**、**ReturnLoan.cs**、**LoanService.cs**、**LoanFactory.cs**、および **JsonData.cs** ファイルを追加します。

1. 次のプロンプトを入力します。

    ```plaintext
    @workspace Create fields and a class constructor for the `GetLoan.cs` file. The class will be used to create unit tests for the GetLoan method in the `JsonLoanRepository.cs` file. Create the following private readonly fields: `_mockLoanRepository`, `_jsonLoanRepository`, `_configuration`, and `_jsonData`. Instantiate the fields in the `GetLoanTest` constructor. Use `ConfigurationBuilder` to create a `_configuration` object that can be used to instantiate the JsonData object.
    ```

    このプロンプトは、GitHub Copilot にフィールドとクラス コンストラクターを提案するように求めます。

1. 少し時間を取って、GitHub Copilot からの応答を確認してください。

    次のコード スニペットのようなコード候補が表示されます。

    ```csharp

    using NSubstitute;
    using Library.ApplicationCore;
    using Library.ApplicationCore.Entities;
    using Library.ApplicationCore.Interfaces;
    using Library.Infrastructure.Data;
    using Microsoft.Extensions.Configuration;
    namespace UnitTests.Infrastructure.JsonLoanRepository;
    public class GetLoanTest
    {
        private readonly ILoanRepository _mockLoanRepository;
        private readonly JsonLoanRepository _jsonLoanRepository;
        private readonly IConfiguration _configuration;
        private readonly JsonData _jsonData;
        public GetLoanTest()
        {
            _mockLoanRepository = Substitute.For<ILoanRepository>();
            _configuration = new ConfigurationBuilder()
                .AddJsonFile("appsettings.json")
                .Build();
            _jsonData = new JsonData(_configuration);
            _jsonLoanRepository = new JsonLoanRepository(_jsonData);
        }
        // Add test methods here
    }

    ```

1. GitHub Copilot から提供されるコード候補を使用して、**GetLoan.cs** を更新します。

1. 次の問題に注意してください。

    - コード内で、名前空間と JsonLoanRepository の使用との間に競合があります。 ReturnLoan.cs ファイルと RenewMembership.cs ファイルで使用されるパターンに従って、GetLoans.cs の名前空間を更新する必要があります。

    - コードで ILoanRepository が認識されない場合は、Library.ApplicationCore の using ディレクティブをファイルの先頭に追加することが必要になる場合があります。

    - _configuration オブジェクトが正しくインスタンス化されていない場合は、ConfigurationBuilder を含むコード行の更新が必要になる場合があります。 コードを簡略化して "_configuration = new ConfigurationBuilder().Build();" を使用できます。

    - GitHub Copilot によって "using Library.ApplicationCore.Interfaces" が提案されている場合は、その部分をファイルの先頭から削除できます。

1. 次のコード スニペットに一致するように **GetLoan.cs** ファイルを更新します。

    ```csharp

    using NSubstitute;
    using Library.ApplicationCore;
    using Library.ApplicationCore.Entities;
    using Library.Infrastructure.Data;
    using Microsoft.Extensions.Configuration;
    namespace UnitTests.Infrastructure.JsonLoanRepositoryTests;
    public class GetLoanTest
    {
        private readonly ILoanRepository _mockLoanRepository;
        private readonly JsonLoanRepository _jsonLoanRepository;
        private readonly IConfiguration _configuration;
        private readonly JsonData _jsonData;
        public GetLoanTest()
        {
            _mockLoanRepository = Substitute.For<ILoanRepository>();
            _configuration = new ConfigurationBuilder().Build();
            _jsonData = new JsonData(_configuration);
            _jsonLoanRepository = new JsonLoanRepository(_jsonData);
        }
    }

    ```

    コードの更新では、前のステップで特定された問題に対処します。 プロジェクトをビルドすると、エディターで特定された問題が解決されます。

1. チャット コンテキストに、**JsonLoanRepository.cs**、**ReturnLoan.cs**、**LoanService.cs**、**LoanFactory.cs**、**JsonData.cs**、**Loans.json** の各ファイルを追加します。

1. GetLoan.cs ファイルの内容を選択してから、チャット ビューに次のプロンプトを入力します。

    ```plaintext
    @workspace Update the selection to include a unit test for the `JsonLoanRepository.GetLoan` method. The unit test should test the case where a loan ID is found in the data. Use `_mockLoanRepository` to arrange the expected return loan. Use `_jsonLoanRepository` to return an actual loan. Asserts should verify that the return loan ID matches the expected loan ID. Use a loan ID that exists in the `Loans.json` file.
    ```

    このプロンプトは、GitHub Copilot に対して、JsonLoanRepository.GetLoan メソッドの単体テストを提案するように求めます。 単体テストでは、データに貸し出し ID が見つかったケースをテストする必要があります。 このテストでは、"_mockLoanRepository" を使用して、想定される返却貸出、実際の貸出を返却するための "_jsonLoanRepository"、返却貸出 ID と想定される貸出 ID の一致を確認するためのアサートを用意する必要があります。 貸出 ID は、Loans.json ファイルに存在するはずです。

1. 少し時間を取って、GitHub Copilot からの応答を確認してください。

    次のコード スニペットのようなコード候補が表示されます。

    ```csharp

    using NSubstitute;
    using Library.ApplicationCore;
    using Library.ApplicationCore.Entities;
    using Library.Infrastructure.Data;
    using Microsoft.Extensions.Configuration;
    using Xunit;
    namespace UnitTests.Infrastructure.JsonLoanRepositoryTests;
    public class GetLoanTest
    {
        private readonly ILoanRepository _mockLoanRepository;
        private readonly JsonLoanRepository _jsonLoanRepository;
        private readonly IConfiguration _configuration;
        private readonly JsonData _jsonData;
        public GetLoanTest()
        {
            _mockLoanRepository = Substitute.For<ILoanRepository>();
            _configuration = new ConfigurationBuilder().Build();
            _jsonData = new JsonData(_configuration);
            _jsonLoanRepository = new JsonLoanRepository(_jsonData);
        }
        [Fact(DisplayName = "JsonLoanRepository.GetLoan: Returns loan when loan ID is found")]
        public async Task GetLoan_ReturnsLoanWhenLoanIdIsFound()
        {
            // Arrange
            var loanId = 1; // Use a loan ID that exists in the Loans.json file
            var expectedLoan = new Loan { Id = loanId, BookItemId = 101, PatronId = 202, LoanDate = DateTime.Now, DueDate = DateTime.Now.AddDays(14) };
            _mockLoanRepository.GetLoan(loanId).Returns(expectedLoan);
            await _jsonData.EnsureDataLoaded(); // Ensure data is loaded
            // Act
            var actualLoan = await _jsonLoanRepository.GetLoan(loanId);
            // Assert
            Assert.NotNull(actualLoan);
            Assert.Equal(expectedLoan.Id, actualLoan?.Id);
        }
    }

    ```

1. GitHub Copilot から提供されるコード候補を使用して、**GetLoan.cs** を更新します。

    JsonLoanRepository.GetLoan メソッドは、JSON データが確実に読み込まれるように "_jsonData.EnsureDataLoaded" を呼び出します。 "_jsonData.EnsureDataLoaded" を呼び出すコード行がテスト メソッドに含まれている場合は削除できます。

    コードで Loan クラスが認識されない場合は、GetLoan.cs ファイルの先頭に "using Library.ApplicationCore.Entities" ステートメントを必ず含めます。 Loan クラスは Library.ApplicationCore.Entities 名前空間にあります。

1. エラーが発生しないように AccelerateDevGitHubCopilot ソリューションを構築してください。

1. GitHub Copilot のオートコンプリート機能を使用して、貸し出し ID が見つからない場合のテストを作成します。

    GetLoan_ReturnsLoanWhenLoanIdIsFound メソッドの後に空白行を作成します。

    オートコンプリートの提案を受け入れて、新しいテスト メソッドを作成します。

1. 少し時間を取って、GitHub Copilot からの応答を確認してください。

    GitHub Copilot のオートコンプリート機能は、必要がない場合でも予想される貸し出しをモックする可能性があります。 想定される貸出をモックするコードは削除できますが、Loans.json ファイルに存在しない貸出 ID が必要です。

    次のコード スニペットのいずれかに似たコード候補が表示されます。

    ```csharp

    [Fact(DisplayName = "JsonLoanRepository.GetLoan: Returns null when loan ID is not found")]
    public async Task GetLoan_ReturnsNullWhenLoanIdIsNotFound()
    {
        // Arrange
        var loanId = 999; // Use a loan ID that does not exist in the Loans.json file
        var expectedLoan = new Loan { Id = loanId, BookItemId = 101, PatronId = 202, LoanDate = DateTime.Now, DueDate = DateTime.Now.AddDays(14) };
        _mockLoanRepository.GetLoan(loanId).Returns(expectedLoan);
        // Act
        var actualLoan = await _jsonLoanRepository.GetLoan(loanId);
        // Assert
        Assert.Null(actualLoan);
    }

    ```

1. データ セットにない loanId 値を割り当てるオートコンプリート候補を採用します。

    データ セットにない loanId 番号を割り当てる候補がない場合は、**Ctrl + Enter** キーボード ショートカットを使用して追加の候補を一覧表示できます。

1. 単体テストでは JSON データ ファイルへのアクセスが必要であることに注意してください。

    JsonLoanRepository.GetLoan メソッドは、JsonData オブジェクトを使用して貸出データの読み込みおよび保存を行います。

    JSON データ ファイルは、"Library.Console\Json" フォルダーにあります。 これらのファイルをテスト プロジェクトに含めるには、UnitTests.csproj ファイルを更新する必要があります。

1. 次の XML スニペットを **UnitTests.csproj** ファイルに追加します。

    ```xml

    <ItemGroup>
        <None Include="..\..\src\Library.Console\Json\**\*">
            <Link>Json\%(RecursiveDir)%(FileName)%(Extension)</Link>
            <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
        </None>
    </ItemGroup>

    ```

    これにより、テストの実行時に JSON データ ファイルが出力ディレクトリにコピーされます。

#### タスク 3: 作業を確認する

JsonLoanRepository クラスの単体テストを実行するには、いくつかの方法があります。 Visual Studio Code のテスト エクスプローラー、統合ターミナル、または "dotnet test" コマンドを使用できます。

以下の手順に従って、演習のこのセクションを完了します。

1. GetLoans.cs ファイルがエディターで開かれていることを確認します。

1. ソリューションをビルドし、エラーがないことを確認します。

    **AccelerateDevGitHubCopilot** を右クリックし、**[ビルド]** を選択します。

1. テスト メソッドの左側にある "緑色の再生ボタン" に注目してください。

1. Visual Studio Code のテスト エクスプローラー ビューを開きます。

    テスト エクスプローラー ビューを開くには、左側のアクティビティ バーにあるビーカー型のアイコンを選択します。 テスト エクスプローラーには、ユーザー インターフェイスで [テスト] というラベルが付けられています。

    テスト エクスプローラーは、ワークスペース内のすべてのテスト ケースを表示するツリー ビューです。 テスト ケースを実行/デバッグし、テスト エクスプローラーを使用してテスト結果を表示できます。

1. **UnitTests** とその下のノードを展開して、**GetLoanTest** を見つけます。

1. 次を実行します: **JsonLoanRepository.GetLoan:Returns loan when loan ID is found** テスト ケース。

    > [!NOTE]
    > GitHub Copilot では、テスト メソッドの名前が異なる場合があります。 データに存在する貸出 ID を使用するテスト メソッドを実行します。

1. テスト エクスプローラー ビューとエディターのテスト結果に注目してください。

    テストに合格したことを示す緑色のチェックマークが表示されます。

1. エディターを使用して、次を実行します: **JsonLoanRepository.GetLoan:Returns null when loan ID is not found** テスト ケース。

1. テスト エクスプローラー ビューとエディターのテスト結果に注目してください。

    エディターからテストを実行するには、テスト メソッドの左側にある緑色の再生ボタンを選択します。

1. **JsonLoanRepository.GetLoan:Returns null when loan ID is not found** テストに合格していることを確認します。

    両方のテストの左側に緑色のチェックマークが表示されているはずです。

#### まとめ - 演習 7

この演習では、GitHub Copilot を使用して既存の UnitTests プロジェクトを評価し、プロジェクトを拡張して Library.Infrastructure プロジェクトのデータ アクセス クラスのテストを開始しました。 UnitTests.csproj ファイルに Library.Infrastructure プロジェクトへの参照を追加し、JsonLoanRepository クラスの単体テストを作成しました。 GitHub Copilot を使用することで、JsonLoanRepository クラスの GetLoan メソッドの単体テストを記述できるようになりました。 Visual Studio Code のテスト エクスプローラーを使用して単体テストを実行し、テストがパスすることを確認しました。

