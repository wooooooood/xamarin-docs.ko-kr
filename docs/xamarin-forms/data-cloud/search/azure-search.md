---
title: Azure 검색 데이터를 검색합니다.
description: Azure 검색은 인덱싱 및 쿼리 업로드 된 데이터에 대 한 기능을 제공 하는 클라우드 서비스입니다. 인프라 요구 사항 및 응용 프로그램에서 검색 기능을 구현 하는과 관련 된 검색 알고리즘 복잡성 제거 합니다. 이 문서에서는 Azure 검색 Xamarin.Forms 응용 프로그램에 통합 하는 Microsoft Azure 검색 라이브러리를 사용 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: b0542b330e54a41a0cbe6ffe364def78ab6386b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="searching-data-with-azure-search"></a>Azure 검색 데이터를 검색합니다.

_Azure 검색은 인덱싱 및 쿼리 업로드 된 데이터에 대 한 기능을 제공 하는 클라우드 서비스입니다. 인프라 요구 사항 및 응용 프로그램에서 검색 기능을 구현 하는과 관련 된 검색 알고리즘 복잡성 제거 합니다. 이 문서에서는 Azure 검색 Xamarin.Forms 응용 프로그램에 통합 하는 Microsoft Azure 검색 라이브러리를 사용 하는 방법을 보여줍니다._

## <a name="overview"></a>개요

데이터는 Azure 검색에서 인덱스와 문서도 저장 됩니다. *인덱스* 는 Azure 검색 서비스에서 검색할 수 있는 데이터의 저장소와 데이터베이스 테이블에 개념적으로 비슷합니다. A *문서* , 인덱스의 검색 가능한 데이터의 단일 단위 이며 데이터베이스 행 개념적으로 비슷합니다. 때 문서를 업로드 하 고 Azure 검색에 대 한 검색 쿼리를 제출, 요청 검색 서비스에 있는 특정 인덱스에 적용 됩니다.

Azure 검색에 대 한 각 요청에는 서비스 및 API 키의 이름을 포함 해야 합니다. API 키에는 다음과 같은 두 종류가 있습니다.

- *관리자 키는* 모든 작업에 대 한 전체 권한을 부여 합니다. 여기에 서비스를 관리, 만들기 및 삭제, 인덱스 및 데이터 원본.
- *쿼리 키* 인덱스와 문서에 대 한 읽기 전용 액세스 권한을 부여 하 고 검색 요청을 실행 하는 응용 프로그램에서 사용 되어야 합니다.

Azure 검색에 가장 일반적으로 요청 쿼리를 실행 하는 것입니다. 전송할 수 있는 쿼리는 다음과 같은 두 종류가 있습니다.

- A *검색* 쿼리는 인덱스의 모든 검색 가능한 필드에서 하나 이상의 항목에 대 한를 검색 합니다. 검색 쿼리는 간소화 된 구문 또는 Lucene 쿼리 구문을 사용 하 여 작성 됩니다. 자세한 내용은 참조 [Azure 검색의 단순 쿼리 구문을](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/), 및 [Azure 검색의 쿼리 구문을 Lucene](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/)합니다.
- A *필터* 쿼리는 인덱스의 모든 필터링 가능한 필드에 따라 부울 식을 평가 합니다. 필터 쿼리 OData 필터 언어의 하위 집합을 사용 하 여 작성 됩니다. 자세한 내용은 참조 [Azure 검색의 OData 식 구문](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/)합니다.

검색 쿼리 및 쿼리 필터링 기준 개별적으로 또는 함께 사용할 수 있습니다. 필터 쿼리 전체 인덱스를 먼저 적용을 조합 하면 하 한 필터 쿼리의 결과에 검색 쿼리가 수행 되는 다음 합니다.

Azure 검색도 검색 입력을 기반으로 하는 검색 제안을 지원 합니다. 자세한 내용은 참조 [제안 쿼리](#suggestions)합니다.

## <a name="setup"></a>설정

Azure 검색 Xamarin.Forms 응용 프로그램으로 통합 하기 위한 프로세스는 다음과 같습니다.

1. Azure 검색 서비스를 만듭니다. 자세한 내용은 참조 [Azure 포털을 사용 하 여 Azure 검색 서비스 만들기](/azure/search/search-create-service-portal/)합니다.
1. Xamarin.Forms 솔루션 PCL 이식 가능한 클래스 라이브러리 ()에서 대상 프레임 워크로 Silverlight를 제거 합니다. 플랫폼 간 개발을 지원 하지만 151 프로필 또는 프로필 92 Silverlight를 지원 하지 않습니다 프로필에 PCL 프로필을 변경 하 여이 수행할 수 있습니다.
1. 추가 [Microsoft Azure 검색 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.Search) Xamarin.Forms 솔루션에서 PCL 프로젝트에 NuGet 패키지 합니다.

다음이 단계를 수행한 후 Microsoft 검색 라이브러리 API 검색 인덱스 및 데이터 원본 관리, 업로드 하 고 문서를 관리 및 쿼리 실행에 사용할 수 있습니다.

## <a name="creating-the-azure-search-index"></a>Azure 검색 인덱스 만들기

인덱스 스키마를 검색할 데이터의 구조에 매핑되는 정의 되어야 합니다. Azure 포털에서 수행 하거나 프로그래밍 방식으로 사용 하 여이 수는 `SearchServiceClient` 클래스입니다. 이 클래스는 Azure 검색에 대 한 연결을 관리 하 고 인덱스를 만드는 데 사용할 수 있습니다. 다음 코드 예제에서는이 클래스의 인스턴스를 만드는 방법을 보여 줍니다.

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient` 생성자 오버 로드는 검색 서비스 이름 및 `SearchCredentials` 인수로 개체는 `SearchCredentials` 개체 배치는 *관리자 키* Azure 검색 서비스에 대 한 합니다. *관리자 키* 인덱스를 만드는 데 필요 합니다.

> [!NOTE]
>  단일 `SearchServiceClient` Azure 검색에 너무 많은 연결을 방지 하기 위해 인스턴스를 응용 프로그램에서 사용 해야 합니다.

인덱스에서 정의 된는 `Index` 다음 코드 예제에서와 같이 개체:

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

`Index.Name` 속성을 인덱스의 이름으로 설정 해야 및 `Index.Fields` 배열에 속성을 설정 해야 `Field` 개체입니다. 각 `Field` 인스턴스 이름을, 형식과 필드를 사용 하는 방법을 지정 하는 모든 속성을 지정 합니다. 이러한 속성은 다음과 같습니다.

- `IsKey` -인덱스의 키 필드 인지를 나타냅니다. 형식의 인덱스에 하나의 필드만 `DataType.String`, 키 필드로 지정 되어야 합니다.
- `IsFacetable` –이 필드에서 패싯 탐색을 수행할 수 인지를 나타냅니다. 기본값은 `false`입니다.
- `IsFilterable` – 필터 쿼리에서 필드를 사용할 수 있는지 여부를 나타냅니다. 기본값은 `false`입니다.
- `IsRetrievable` – 검색 결과에서 필드를 검색할 수 있는지 여부를 나타냅니다. 기본값은 `true`입니다.
- `IsSearchable` – 필드 전체 텍스트 검색에 포함 되는지 여부를 나타냅니다. 기본값은 `false`입니다.
- `IsSortable` – 필드에 사용할 수 있는지 여부를 나타내는 `OrderBy` 식입니다. 기본값은 `false`입니다.

> [!NOTE]
> 다시 작성 하 고 데이터를 다시 로드 해야 배포 후에 인덱스를 변경 합니다.

`Index` 개체 선택적으로 지정할 수는 `Suggesters` 속성 자동 완성 지원 또는 검색 제안 쿼리 하는 데 사용할 인덱스의 필드를 정의 합니다. `Suggesters` 배열에 속성을 설정 해야 `Suggester` 검색 제안 결과 만드는 데 사용 되는 필드를 정의 하는 개체입니다.

만든 후의 `Index` 개체를 호출 하 여 인덱스를 만들 `Indexes.Create` 에 `SearchServiceClient` 인스턴스.

> [!NOTE]
> 응용 프로그램에서 인덱스를 만들 응답성이 뛰어난, 보존 하는 경우 사용 된 `Indexes.CreateAsync` 메서드.

자세한 내용은 참조 [.NET SDK를 사용 하 여 Azure 검색 인덱스를 만들](/azure/search/search-create-index-dotnet/)합니다.

## <a name="deleting-the-azure-search-index"></a>Azure 검색 인덱스를 삭제합니다.

호출 하 여 인덱스를 삭제할 수 있습니다 `Indexes.Delete` 에 `SearchServiceClient` 인스턴스:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Azure 검색 인덱스에 데이터를 업로드 하는 중

인덱스를 정의한 후 데이터 두 모델 중 하나를 사용 하 여 업로드할 수 있습니다.

- **끌어오기 모델** – 데이터는 주기적으로 수집 된 Azure Cosmos DB, Azure SQL 데이터베이스, Azure Blob 저장소에서 또는 Azure 가상 컴퓨터에서 호스팅되는 SQL Server.
- **밀어넣기 모델** – 데이터에 프로그래밍 방식으로 인덱스에 보내집니다. 이 문서에 적용 하는 모델입니다.

A `SearchIndexClient` 인덱스에 데이터를 가져오려면 인스턴스를 만들어야 합니다. 이 호출 하 여 수행할 수 있습니다는 `SearchServiceClient.Indexes.GetClient` 메서드를 다음 코드 예제에서와 같이:

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

인덱스에 가져올 데이터를로 패키지 됩니다는 `IndexBatch` 개체의 컬렉션을 캡슐화 하는 `IndexAction` 개체입니다. 각 `IndexAction` 인스턴스에 Azure 검색 문서에서 수행할 수 있는 작업을 지시 하는 속성 및 문서를 포함 합니다. 위의 코드 예제는 `IndexAction.Upload` 동작을 지정 하 고 새로 만들었거나, 이면 인덱스에 삽입 되 고 문서에서 발생 하는 되었거나 이미 있는 경우 대체 합니다. `IndexBatch` 인덱스에 다음 개체가 모두 보내질 호출 하 여는 `Documents.Index` 에서 메서드는 `SearchIndexClient` 개체입니다. 다른 인덱싱 작업에 대 한 정보를 참조 하십시오. [인덱싱 작업을 사용 하 여 결정](/azure/search/search-import-data-dotnet#ii-decide-which-indexing-action-to-use)합니다.

> [!NOTE]
> 1000 개만 문서를 인덱싱 단일 요청에 포함할 수 있습니다.

위의 코드 예제는 `monkeyList` 익명 개체의 컬렉션에서 컬렉션을 만든 `Monkey` 개체입니다. 에 대 한 데이터를 만들고이 `id` 파스칼식 대/소문자 매핑 필드 했다가 `Monkey` 카멜식 대/소문자 속성 이름을 인덱스 필드 이름을 검색 합니다. 이 매핑은 추가 하 여 수행할 수도 있습니다 또는 `[SerializePropertyNamesAsCamelCase]` 특성을 `Monkey` 클래스입니다.

자세한 내용은 참조 [Azure 검색.NET SDK를 사용 하 여 데이터를 업로드](/azure/search/search-import-data-dotnet/)합니다.

## <a name="querying-the-azure-search-index"></a>Azure 검색 인덱스 쿼리

A `SearchIndexClient` 인스턴스 인덱스 쿼리를 만들어야 합니다. 응용 프로그램 쿼리를 실행 하는 경우는 것이 좋습니다를 최소 권한의 원칙에 따라 만들는 `SearchIndexClient` 를 직접 전달는 *쿼리 키* 인수로 합니다. 이렇게 하면 사용자가 인덱스와 문서에 대 한 읽기 전용 액세스. 이 방법은 다음 코드 예제에 설명 된:

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient` 생성자 오버 로드는 검색 서비스 이름, 인덱스 이름, 사용 및 `SearchCredentials` 인수로 개체는 `SearchCredentials` 개체 배치는 *쿼리 키* Azure 검색 서비스에 대 한 합니다.

### <a name="search-queries"></a>검색 쿼리

호출 하 여 인덱스를 쿼리할 수 있습니다는 `Documents.SearchAsync` 에서 메서드는 `SearchIndexClient` 다음 코드 예제에서와 같이 인스턴스:

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

`SearchAsync` 메서드는 검색 텍스트 인수 및 선택적 `SearchParameters` 쿼리를 구체화 하는 데 사용할 수 있습니다. 검색 쿼리를 설정 하 여 필터 쿼리를 지정할 수 있습니다 하는 동안 검색 텍스트 인수로 지정는 `Filter` 의 속성은 `SearchParameters` 인수입니다. 다음 코드 예제에서는 형식을 쿼리 하는 둘 다 보여 줍니다.

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

이 필터 쿼리의 전체 인덱스에 적용 되 고 결과에서 문서를 제거 합니다. 여기서는 `location` 필드는 중국에 같고 베트남 같지 않습니다. 필터링 후 검색 쿼리 필터 쿼리 결과에서 수행 됩니다.

> [!NOTE]
> 를 검색 하지 않고 필터링 하려면 전달 `*` 검색 텍스트 인수로 합니다.

`SearchAsync` 메서드가 반환 되는 `DocumentSearchResult` 쿼리 결과가 포함 된 개체입니다. 이 개체를 열거 하면 각 `Document` 으로 만들어지는 개체는 `Monkey` 개체를에 추가 `Monkeys` `ObservableCollection` 디스플레이 대 한 합니다. 다음 스크린 샷 쇼 검색 쿼리 결과 Azure 검색에서 반환 된:

![](azure-search-images/search.png "검색 결과")

검색 및 필터링 하는 방법에 대 한 자세한 내용은 참조 [.NET SDK를 사용 하 여 Azure 검색 인덱스를 쿼리하여](/azure/search/search-query-dotnet/)합니다.

<a name="suggestions" />

### <a name="suggestion-queries"></a>제안 쿼리

Azure 검색을 사용 하면 제안을 요청할 수 기반 검색 쿼리를 호출 하 여는 `Documents.SuggestAsync` 에서 메서드는 `SearchIndexClient` 인스턴스. 다음 코드 예제에서이 확인할 수 있습니다.

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

`SuggestAsync` 메서드 인수를 검색 텍스트를 사용 하는 확인 기의 이름 (정의 된 인덱스에), 선택적 `SuggestParameters` 쿼리를 구체화 하는 데 사용할 수 있습니다. `SuggestParameters` 인스턴스는 다음 속성을 설정 합니다.

- `UseFuzzyMatching` –로 설정 된 경우 `true`, Azure 검색은 검색 텍스트에 대체 되었거나 누락 된 문자는 경우에 제안을 찾습니다.
- `HighlightPreTag` – 앞 제안 적중 항목에 추가 되는 태그입니다.
- `HighlightPostTag` – 제안 적중 항목에 추가 되는 태그입니다.
- `MinimumCoverage` – 나타냅니다 되도록 쿼리에 대 한 추천 쿼리에서 포함 되어야 하는 인덱스의 백분율을 성공을 보고 합니다. 기본값은 80입니다.
- `Top` – 검색할 제안의 수 있습니다. 것 1에서 5의 기본값은 100 사이의 정수 여야 합니다.

이 따라 적중 강조 표시 하 고 결과 검색 용어를 철자가 비슷한 포함 된 문서에 포함 됩니다는 인덱스에서 상위 10 개의 결과 함께 반환 될 경우

`SuggestAsync` 메서드가 반환 되는 `DocumentSuggestResult` 쿼리 결과가 포함 된 개체입니다. 이 개체를 열거 하면 각 `Document` 으로 만들어지는 개체는 `Monkey` 개체를에 추가 `Monkeys` `ObservableCollection` 디스플레이 대 한 합니다. 다음 스크린샷에서 Azure 검색에서 반환 된 제안 결과 보여 줍니다.

![](azure-search-images/suggest.png "제안 결과")

샘플 응용 프로그램에서의 `SuggestAsync` 메서드는 사용자는 검색 단어를 입력 합니다. 완료 될 때만 호출 됩니다. 그러나도 사용할 수 있습니다 쿼리를 지 원하는 자동 완성 검색 키를 누를 때마다에서 실행 하 여 합니다.

## <a name="summary"></a>요약

이 문서에 Azure 검색 Xamarin.Forms 응용 프로그램에 통합 하는 Microsoft Azure 검색 라이브러리를 사용 하는 방법을 보여 줍니다. Azure 검색은 인덱싱 및 쿼리 업로드 된 데이터에 대 한 기능을 제공 하는 클라우드 서비스입니다. 인프라 요구 사항 및 응용 프로그램에서 검색 기능을 구현 하는과 관련 된 검색 알고리즘 복잡성 제거 합니다.


## <a name="related-links"></a>관련 링크

- [Azure 검색 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureSearch/)
- [Azure 검색 설명서](/azure/search/)
- [Microsoft Azure 검색 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.Search/)
