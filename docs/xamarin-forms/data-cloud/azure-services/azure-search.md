---
title: Azure Search 및를 사용 하 여 데이터 검색Xamarin.Forms
description: 이 문서에서는 Microsoft Azure 검색 라이브러리를 사용 하 여 Azure Search을 응용 프로그램에 통합 하는 방법을 보여 줍니다 Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 69962bbb51a493ba2bcaed5d3c9407c5aafe471c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84133291"
---
# <a name="search-data-with-azure-search-and-xamarinforms"></a>Azure Search 및를 사용 하 여 데이터 검색Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)

_Azure Search는 업로드 된 데이터에 대 한 인덱싱 및 쿼리 기능을 제공 하는 클라우드 서비스입니다. 이렇게 하면 일반적으로 응용 프로그램에서 검색 기능을 구현 하는 것과 관련 된 인프라 요구 사항 및 검색 알고리즘이 제거 됩니다. 이 문서에서는 Microsoft Azure 검색 라이브러리를 사용 하 여 Azure Search을 응용 프로그램에 통합 하는 방법을 보여 줍니다 Xamarin.Forms ._

## <a name="overview"></a>개요

데이터는 Azure Search에 인덱스 및 문서로 저장 됩니다. *인덱스* 는 Azure Search 서비스에서 검색할 수 있는 데이터의 저장소 이며 개념적으로 데이터베이스 테이블과 유사 합니다. *문서* 는 인덱스에서 검색 가능한 데이터의 단일 단위 이며 개념적으로 데이터베이스 행과 유사 합니다. 문서를 업로드 하 고 Azure Search에 검색 쿼리를 제출 하는 경우 검색 서비스의 특정 인덱스에 대 한 요청이 수행 됩니다.

Azure Search에 대 한 각 요청은 서비스 이름 및 API 키를 포함 해야 합니다. API 키에는 다음과 같은 두 가지 유형이 있습니다.

- *관리 키는* 모든 작업에 모든 권한을 부여 합니다. 여기에는 서비스 관리, 인덱스 만들기 및 삭제, 데이터 원본 등이 포함 됩니다.
- *쿼리 키는* 인덱스와 문서에 대 한 읽기 전용 액세스 권한을 부여 하며, 검색 요청을 실행 하는 응용 프로그램에서 사용 해야 합니다.

가장 일반적인 Azure Search 요청은 쿼리를 실행 하는 것입니다. 제출할 수 있는 쿼리는 다음과 같은 두 가지 유형이 있습니다.

- *검색* 쿼리는 인덱스의 모든 검색 가능 필드에서 하나 이상의 항목을 검색 합니다. 검색 쿼리는 단순화 된 구문이 나 Lucene 쿼리 구문을 사용 하 여 빌드됩니다. 자세한 내용은 [Azure Search의 단순 쿼리 구문](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/)및 [Azure Search에서 Lucene 쿼리 구문](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/)을 참조 하세요.
- *필터* 쿼리는 인덱스의 필터링 가능한 모든 필드에 대해 부울 식을 계산 합니다. 필터 쿼리는 OData 필터 언어의 하위 집합을 사용 하 여 작성 됩니다. 자세한 내용은 [Azure Search의 OData 식 구문](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/)을 참조 하세요.

검색 쿼리 및 필터 쿼리를 개별적으로 또는 함께 사용할 수 있습니다. 필터 쿼리를 함께 사용 하는 경우에는 먼저 전체 인덱스에 필터 쿼리를 적용 한 다음 필터 쿼리 결과에 대해 검색 쿼리를 수행 합니다.

Azure Search는 검색 입력을 기준으로 제안 검색도 지원 합니다. 자세한 내용은 [제안 쿼리](#suggestions)를 참조 하세요.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

## <a name="setup"></a>설치 프로그램

Azure Search를 응용 프로그램에 통합 하는 프로세스는 Xamarin.Forms 다음과 같습니다.

1. Azure Search 서비스를 만듭니다. 자세한 내용은 [Azure Portal을 사용 하 여 Azure Search 서비스 만들기](/azure/search/search-create-service-portal/)를 참조 하세요.
1. Xamarin.Forms솔루션 PCL (이식 가능한 클래스 라이브러리)에서 대상 프레임 워크로 Silverlight를 제거 합니다. 이렇게 하려면 PCL 프로필을 플랫폼 간 개발을 지 원하는 프로필로 변경 하지만 프로필 151 또는 프로필 92과 같은 Silverlight는 지원 하지 않습니다.
1. [Microsoft Azure 검색 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.Search) NuGet 패키지를 솔루션의 PCL 프로젝트에 추가 Xamarin.Forms 합니다.

이러한 단계를 수행한 후 Microsoft Search Library API를 사용 하 여 검색 인덱스 및 데이터 원본을 관리 하 고, 문서를 업로드 및 관리 하 고, 쿼리를 실행할 수 있습니다.

## <a name="creating-the-azure-search-index"></a>Azure Search 인덱스 만들기

검색할 데이터의 구조에 매핑되는 인덱스 스키마를 정의 해야 합니다. 이는 Azure Portal에서 또는 클래스를 사용 하 여 프로그래밍 방식으로 수행할 수 있습니다 `SearchServiceClient` . 이 클래스는 Azure Search에 대 한 연결을 관리 하 고 인덱스를 만드는 데 사용할 수 있습니다. 다음 코드 예제에서는이 클래스의 인스턴스를 만드는 방법을 보여 줍니다.

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient`생성자 오버 로드는 `SearchCredentials` `SearchCredentials` Azure Search 서비스에 대 한 *관리 키* 를 래핑하는 개체를 사용 하 여 검색 서비스 이름과 개체를 인수로 사용 합니다. 인덱스를 만들려면 *관리자 키* 가 필요 합니다.

> [!NOTE]
> `SearchServiceClient`응용 프로그램에서 단일 인스턴스를 사용 하 여 Azure Search에 대 한 연결을 너무 많이 열지 않도록 해야 합니다.

인덱스는 `Index` 다음 코드 예제에서 보여 주는 것 처럼 개체에 의해 정의 됩니다.

```csharp
static void CreateSearchIndex()
{
  var index = new Index()
  {
    Name = Constants.Index,
    Fields = new[]
    {
      new Field("id", DataType.String) { IsKey = true, IsRetrievable = true },
      new Field("name", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("location", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("details", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSearchable = true },
      new Field("imageUrl", DataType.String) { IsRetrievable = true }
    },
    Suggesters = new[]
    {
      new Suggester("nameSuggester", SuggesterSearchMode.AnalyzingInfixMatching, new[] { "name" })
    }
  };

  searchClient.Indexes.Create(index);
}
```

`Index.Name`속성은 인덱스의 이름으로 설정 해야 하며 `Index.Fields` 속성은 개체의 배열로 설정 되어야 합니다 `Field` . 각 `Field` 인스턴스는 필드를 사용 하는 방법을 지정 하는 이름, 형식 및 속성을 지정 합니다. 이러한 속성은 다음과 같습니다.

- `IsKey`– 필드가 인덱스의 키인지 여부를 나타냅니다. 형식의 인덱스에 있는 필드 중 하나만 `DataType.String` 키 필드로 지정 해야 합니다.
- `IsFacetable`–이 필드에서 패싯 탐색을 수행할 수 있는지 여부를 나타냅니다. 기본값은 `false`입니다.
- `IsFilterable`– 필터 쿼리에서 필드를 사용할 수 있는지 여부를 나타냅니다. 기본값은 `false`입니다.
- `IsRetrievable`– 검색 결과에서 필드를 검색할 수 있는지 여부를 나타냅니다. 기본값은 `true`입니다.
- `IsSearchable`– 필드가 전체 텍스트 검색에 포함 되는지 여부를 나타냅니다. 기본값은 `false`입니다.
- `IsSortable`– 식에 필드를 사용할 수 있는지 여부를 나타냅니다 `OrderBy` . 기본값은 `false`입니다.

> [!NOTE]
> 배포 후 인덱스를 변경 하려면 데이터를 다시 작성 하 고 다시 로드 해야 합니다.

`Index`개체는 필요에 따라 `Suggesters` 자동 완성 또는 검색 제안 쿼리를 지 원하는 데 사용 되는 인덱스의 필드를 정의 하는 속성을 지정할 수 있습니다. `Suggesters`속성은 `Suggester` 검색 제안 결과를 작성 하는 데 사용 되는 필드를 정의 하는 개체의 배열로 설정 되어야 합니다.

개체를 만든 후 `Index` 에는 인스턴스에서를 호출 하 여 인덱스를 만듭니다 `Indexes.Create` `SearchServiceClient` .

> [!NOTE]
> 응답을 유지 해야 하는 응용 프로그램에서 인덱스를 만드는 경우 메서드를 사용 `Indexes.CreateAsync` 합니다.

자세한 내용은 [.NET SDK를 사용 하 여 Azure Search 인덱스 만들기](/azure/search/search-create-index-dotnet/)를 참조 하세요.

## <a name="deleting-the-azure-search-index"></a>Azure Search 인덱스를 삭제 하는 중

인스턴스에서를 호출 하 여 인덱스를 삭제할 수 있습니다 `Indexes.Delete` `SearchServiceClient` .

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Azure Search 인덱스에 데이터 업로드

인덱스를 정의한 후에는 다음 두 가지 모델 중 하나를 사용 하 여 데이터를 업로드할 수 있습니다.

- **끌어오기 모델** – 데이터가 Azure 가상 머신에서 호스트 되는 Azure Cosmos DB, Azure SQL Database, Azure Blob Storage 또는 SQL Server에서 정기적으로 수집 됩니다.
- **푸시 모델** – 데이터를 프로그래밍 방식으로 인덱스로 보냅니다. 이는이 문서에서 적용 되는 모델입니다.

`SearchIndexClient`인덱스에 데이터를 가져오려면 인스턴스를 만들어야 합니다. 이 `SearchServiceClient.Indexes.GetClient` 작업은 다음 코드 예제에서 보여 주는 것 처럼 메서드를 호출 하 여 수행할 수 있습니다.

```csharp
static void UploadDataToSearchIndex()
{
  var indexClient = searchClient.Indexes.GetClient(Constants.Index);

  var monkeyList = MonkeyData.Monkeys.Select(m => new
  {
    id = Guid.NewGuid().ToString(),
    name = m.Name,
    location = m.Location,
    details = m.Details,
    imageUrl = m.ImageUrl
  });

  var batch = IndexBatch.New(monkeyList.Select(IndexAction.Upload));
  try
  {
    indexClient.Documents.Index(batch);
  }
  catch (IndexBatchException ex)
  {
    // Sometimes when the Search service is under load, indexing will fail for some
    // documents in the batch. Compensating actions like delaying and retrying should be taken.
    // Here, the failed document keys are logged.
    Console.WriteLine("Failed to index some documents: {0}",
      string.Join(", ", ex.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
  }
}
```

인덱스로 가져올 데이터는 개체 `IndexBatch` 의 컬렉션을 캡슐화 하는 개체로 패키지 됩니다 `IndexAction` . 각 `IndexAction` 인스턴스에는 문서와 문서에 대해 수행할 작업을 Azure Search 알려주는 속성이 포함 되어 있습니다. 위의 코드 예제에서는 `IndexAction.Upload` 작업이 지정 되어 새 항목이 있는 경우 인덱스에 삽입 되거나 이미 있는 경우 교체 됩니다. `IndexBatch`그런 다음 개체에 대해 메서드를 호출 하 여 개체를 인덱스로 보냅니다 `Documents.Index` `SearchIndexClient` . 다른 인덱싱 동작에 대 한 자세한 내용은 [사용할 인덱싱 동작 결정](/azure/search/search-import-data-dotnet#decide-which-indexing-action-to-use)을 참조 하세요.

> [!NOTE]
> 1000 문서만 단일 인덱싱 요청에 포함할 수 있습니다.

위의 코드 예제에서 `monkeyList` 컬렉션은 개체의 컬렉션에서 익명 개체로 생성 됩니다 `Monkey` . 이렇게 하면 필드에 대 한 데이터가 만들어지고 `id` 파스칼식 대/소문자 `Monkey` 속성 이름에 카멜식 대/소문자 검색 인덱스 필드 이름이 매핑됩니다. 또는 클래스에 특성을 추가 하 여이 매핑을 수행할 수도 있습니다 `[SerializePropertyNamesAsCamelCase]` `Monkey` .

자세한 내용은 [.NET SDK를 사용 하 여 Azure Search에 데이터 업로드](/azure/search/search-import-data-dotnet/)를 참조 하세요.

## <a name="querying-the-azure-search-index"></a>Azure Search 인덱스 쿼리

`SearchIndexClient`인덱스를 쿼리하려면 인스턴스를 만들어야 합니다. 응용 프로그램에서 쿼리를 실행 하는 경우 최소 권한 원칙을 따르고 `SearchIndexClient` *쿼리 키* 를 인수로 전달 하 여를 직접 만드는 것이 좋습니다. 이렇게 하면 사용자에 게 인덱스 및 문서에 대 한 읽기 전용 액세스 권한이 있습니다. 이 방법은 다음 코드 예제에서 보여 줍니다.

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient`생성자 오버 로드는 `SearchCredentials` `SearchCredentials` Azure Search 서비스에 대 한 *쿼리 키* 를 래핑하는 개체를 사용 하 여 검색 서비스 이름, 인덱스 이름 및 개체를 인수로 사용 합니다.

### <a name="search-queries"></a>검색 쿼리

`Documents.SearchAsync` `SearchIndexClient` 다음 코드 예제에서 보여 주는 것 처럼 인스턴스에서 메서드를 호출 하 여 인덱스를 쿼리할 수 있습니다.

```csharp
async Task AzureSearch(string text)
{
  Monkeys.Clear();

  var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text);
  foreach (SearchResult<Monkey> result in searchResults.Results)
  {
    Monkeys.Add(new Monkey
    {
      Name = result.Document.Name,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SearchAsync`메서드는 검색 텍스트 인수와 `SearchParameters` 쿼리를 추가로 구체화 하는 데 사용할 수 있는 선택적 개체를 사용 합니다. 검색 쿼리는 검색 텍스트 인수로 지정 되지만 필터 쿼리는 인수의 속성을 설정 하 여 지정할 수 있습니다 `Filter` `SearchParameters` . 다음 코드 예제에서는 두 쿼리 유형을 모두 보여 줍니다.

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

이 필터 쿼리는 전체 인덱스에 적용 되 고 `location` 필드가 중국어와 같지 않고 베트남와 같지 않은 결과에서 문서를 제거 합니다. 필터링 후 필터 쿼리 결과에 대해 검색 쿼리가 수행 됩니다.

> [!NOTE]
> 검색 하지 않고 필터링 하려면를 `*` 검색 텍스트 인수로 전달 합니다.

`SearchAsync`메서드는 `DocumentSearchResult` 쿼리 결과를 포함 하는 개체를 반환 합니다. 이 개체는 열거 되며, 각 `Document` 개체는 `Monkey` 개체로 만들어지고 표시를 위해에 추가 됩니다 `Monkeys` `ObservableCollection` . 다음 스크린샷에는 Azure Search에서 반환 된 검색 쿼리 결과가 나와 있습니다.

![](azure-search-images/search.png "Search Results")

검색 및 필터링에 대 한 자세한 내용은 [.NET SDK를 사용 하 여 Azure Search 인덱스 쿼리](/azure/search/search-query-dotnet/)를 참조 하세요.

<a name="suggestions" />

### <a name="suggestion-queries"></a>제안 쿼리

Azure Search를 사용 하면 인스턴스에서 메서드를 호출 하 여 검색 쿼리를 기반으로 제안을 요청할 수 있습니다 `Documents.SuggestAsync` `SearchIndexClient` . 이는 다음 코드 예제에서 보여 줍니다.

```csharp
async Task AzureSuggestions(string text)
{
  Suggestions.Clear();

  var parameters = new SuggestParameters()
  {
    UseFuzzyMatching = true,
    HighlightPreTag = "[",
    HighlightPostTag = "]",
    MinimumCoverage = 100,
    Top = 10
  };

  var suggestionResults =
    await indexClient.Documents.SuggestAsync<Monkey>(text, "nameSuggester", parameters);

  foreach (var result in suggestionResults.Results)
  {
    Suggestions.Add(new Monkey
    {
      Name = result.Text,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SuggestAsync`메서드는 검색 텍스트 인수, 사용할 확인 기 이름 (인덱스에 정의 됨) 및 `SuggestParameters` 쿼리를 추가로 구체화 하는 데 사용할 수 있는 선택적 개체를 사용 합니다. `SuggestParameters`인스턴스는 다음 속성을 설정 합니다.

- `UseFuzzyMatching`–로 설정 하면 `true` 검색 텍스트에 대체 문자 또는 누락 된 문자가 있는 경우에도 Azure Search에서 제안을 찾을 수 있습니다.
- `HighlightPreTag`– 제안 적중 항목 앞에 나오는 태그입니다.
- `HighlightPostTag`– 제안 적중에 추가 되는 태그입니다.
- `MinimumCoverage`– 쿼리가 성공 여부를 보고 하기 위해 제안 쿼리가 적용 해야 하는 인덱스의 백분율을 나타냅니다. 기본값은 80입니다.
- `Top`– 검색할 제안 수입니다. 이 값은 1에서 100 사이의 정수 여야 하며 기본값은 5입니다.

전반적인 효과는 인덱스의 상위 10 개 결과가 적중 항목 강조 표시와 함께 반환 되 고 결과는 유사한 철자의 검색어를 포함 하는 문서를 포함 하는 것입니다.

`SuggestAsync`메서드는 `DocumentSuggestResult` 쿼리 결과를 포함 하는 개체를 반환 합니다. 이 개체는 열거 되며, 각 `Document` 개체는 `Monkey` 개체로 만들어지고 표시를 위해에 추가 됩니다 `Monkeys` `ObservableCollection` . 다음 스크린샷에는 Azure Search에서 반환 된 제안 결과가 나와 있습니다.

![](azure-search-images/suggest.png "Suggestion Results")

예제 응용 프로그램에서 `SuggestAsync` 메서드는 사용자가 검색 단어 입력을 완료 한 경우에만 호출 됩니다. 그러나 각 keypress에서를 실행 하 여 자동 완성 검색 쿼리를 지 원하는 데 사용 될 수도 있습니다.

## <a name="summary"></a>요약

이 문서에서는 Microsoft Azure 검색 라이브러리를 사용 하 여 Azure Search을 응용 프로그램에 통합 하는 방법을 보여 주었습니다 Xamarin.Forms . Azure Search는 업로드 된 데이터에 대 한 인덱싱 및 쿼리 기능을 제공 하는 클라우드 서비스입니다. 이렇게 하면 일반적으로 응용 프로그램에서 검색 기능을 구현 하는 것과 관련 된 인프라 요구 사항 및 검색 알고리즘이 제거 됩니다.

## <a name="related-links"></a>관련 링크

- [Azure Search (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)
- [Azure Search 설명서](/azure/search/)
- [Microsoft Azure 검색 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.Search/)
