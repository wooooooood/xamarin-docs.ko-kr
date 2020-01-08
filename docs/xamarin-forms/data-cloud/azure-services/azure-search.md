---
title: Azure Search 및 Xamarin.ios를 사용 하 여 데이터 검색
description: 이 문서에서는 Microsoft Azure Search 라이브러리를 사용 하 여 Xamarin.Forms 응용 프로그램에 Azure Search를 통합 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: cd5aaac0f41ee6e4afd79397a77635e66abad219
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489793"
---
# <a name="search-data-with-azure-search-and-xamarinforms"></a>Azure Search 및 Xamarin.ios를 사용 하 여 데이터 검색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)

_Azure Search는 업로드 된 데이터에 대 한 인덱싱 및 쿼리 기능을 제공 하는 클라우드 서비스입니다. 이렇게 하면 일반적으로 응용 프로그램에서 검색 기능을 구현 하는 것과 관련 된 인프라 요구 사항 및 검색 알고리즘이 제거 됩니다. 이 문서에서는 Microsoft Azure 검색 라이브러리를 사용 하 여 Azure Search를 Xamarin.ios 응용 프로그램에 통합 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

데이터는 Azure Search에서 인덱스 및 문서 저장 됩니다. *인덱스* 는 Azure Search 서비스에서 검색할 수 있는 데이터의 저장소와 데이터베이스 테이블에 개념적으로 비슷합니다. A *문서* 인덱스에서 검색 가능한 데이터의 단일 단위 이며 데이터베이스 행과 개념적으로 유사 합니다. 경우 문서를 업로드 하 고 Azure Search에 검색 쿼리를 제출을 요청 검색 서비스의 특정 인덱스에 적용 됩니다.

Azure Search에 대 한 각 요청에는 서비스 및 API 키의 이름을 포함 해야 합니다. API 키에는 다음과 같은 두 종류가 있습니다.

- *관리자 키는* 모든 작업에 대 한 전체 권한을 부여 합니다. 여기에 서비스 관리, 만들기 및 인덱스 및 데이터 원본을 삭제 합니다.
- *쿼리 키* 인덱스 및 문서에 대 한 읽기 전용 액세스를 부여 하 고 검색 요청을 발급 하는 응용 프로그램에서 사용 해야 합니다.

Azure Search의 가장 일반적인 요청이 쿼리를 실행 하는 것입니다. 전송할 수 있는 쿼리는 다음과 같은 두 종류가 있습니다.

- A *검색* 쿼리는 인덱스의 모든 검색 가능한 필드에서 하나 이상의 항목에 대 한를 검색 합니다. 검색 쿼리는 간소화 된 구문을 또는 Lucene 쿼리 구문을 사용 하 여 빌드됩니다. 자세한 내용은 [Azure Search의 단순 쿼리 구문](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/), 및 [Azure Search의 Lucene 쿼리 구문](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/)합니다.
- A *필터* 쿼리 인덱스의 모든 필터링 가능한 필드를 통해 부울 식을 계산 합니다. 필터 쿼리는 OData 필터 언어의 하위 집합을 사용 하 여 작성 됩니다. 자세한 내용은 [Azure Search의 OData 식 구문](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/)합니다.

검색 쿼리 및 필터 쿼리는 개별적으로 또는 함께 사용할 수 있습니다. 전체 인덱스를 필터 쿼리에 먼저 적용 되 고 함께 사용 하는 경우 하 고 필터 쿼리 결과에 검색 쿼리를 수행 합니다.

Azure Search는 검색 입력을 기반으로 하는 검색 제안도 지원 합니다. 자세한 내용은 [제안 쿼리](#suggestions)합니다.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

## <a name="setup"></a>설정

Xamarin.Forms 응용 프로그램에 Azure Search를 통합 하기 위한 프로세스는 다음과 같습니다.

1. Azure Search 서비스를 만듭니다. 자세한 내용은 [Azure Portal을 사용 하 여 Azure Search 서비스 만들기](/azure/search/search-create-service-portal/)합니다.
1. 대상 프레임 워크로 Xamarin.Forms 솔루션 이식 가능한 클래스 라이브러리 (PCL)에서 Silverlight를 제거 합니다. 이 플랫폼 간 개발을 지원 하지만 92 프로필 또는 프로필 151 Silverlight를 지원 하지 않습니다 모든 프로필에 PCL 프로필을 변경 하 여 수행할 수 있습니다.
1. 추가 된 [Microsoft Azure Search 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.Search) Xamarin.Forms 솔루션에서 PCL 프로젝트에 NuGet 패키지.

이러한 단계를 수행한 후 Microsoft 검색 라이브러리 API는 검색 인덱스 및 데이터 원본 관리, 업로드 및 문서를 관리 및 쿼리 실행에 사용할 수 있습니다.

## <a name="creating-the-azure-search-index"></a>Azure Search 인덱스 만들기

인덱스 스키마를 검색할 데이터의 구조에 매핑되는 정의 되어야 합니다. 프로그래밍 방식으로 사용 하거나 Azure Portal에서 수행할 수는 `SearchServiceClient` 클래스입니다. 이 클래스는 Azure Search에 대 한 연결을 관리 하 고 인덱스를 만드는 데 사용할 수 있습니다. 다음 코드 예제에는이 클래스의 인스턴스를 만드는 방법을 보여 줍니다.

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient` 생성자 오버 로드는 검색 서비스 이름 및 `SearchCredentials` 인수를 사용 하 여 개체를 `SearchCredentials` 개체 래핑 합니다 *관리자 키* Azure Search 서비스에 대 한 합니다. 합니다 *관리자 키* 인덱스를 만드는 데 필요 합니다.

> [!NOTE]
> 단일 `SearchServiceClient` 인스턴스를 Azure Search에 너무 많은 연결이 열리는 방지 하려면 응용 프로그램에 사용할 수 해야 합니다.

인덱스를 정의한는 `Index` 다음 코드 예제에서 설명한 것 처럼 개체:

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

`Index.Name` 속성을 인덱스의 이름으로 설정 해야 하며 `Index.Fields` 배열의 속성을 설정 해야 `Field` 개체입니다. 각 `Field` 인스턴스 이름, 형식 및 필드가 사용 되는 방법을 지정 하는 모든 속성을 지정 합니다. 이러한 속성은 다음과 같습니다.

- `IsKey` -인덱스의 키 필드 인지를 나타냅니다. 형식의 인덱스에 하나의 필드만 `DataType.String`를 키 필드로 지정 해야 합니다.
- `IsFacetable` –이 필드에서 패싯 탐색을 수행할 수 있는지 여부를 나타냅니다. 기본값은 `false`여야 합니다.
- `IsFilterable` -필드를 필터 쿼리에 사용할 수 있는지 여부를 나타냅니다. 기본값은 `false`여야 합니다.
- `IsRetrievable` -검색 결과에서 필드를 검색할 수 있는지 여부를 나타냅니다. 기본값은 `true`여야 합니다.
- `IsSearchable` – 필드 전체 텍스트 검색에 포함 되는지 여부를 나타냅니다. 기본값은 `false`여야 합니다.
- `IsSortable` – 필드에 사용할 수 있는지 여부를 나타내는 `OrderBy` 식입니다. 기본값은 `false`여야 합니다.

> [!NOTE]
> 배포 된 후 인덱스를 변경 합니다. 다시 작성 하 고 데이터를 다시 로드 해야 합니다.

`Index` 개체를 지정할 수 있습니다는 `Suggesters` 속성 자동 완성을 지원 하거나 제안 쿼리를 검색 하는 데 사용할 인덱스의 필드를 정의 합니다. 합니다 `Suggesters` 배열의 속성을 설정 해야 `Suggester` 검색 제안 결과 작성 하는 데 사용 되는 필드를 정의 하는 개체입니다.

만든 후 합니다 `Index` 개체를 호출 하 여 인덱스를 만들 `Indexes.Create` 에 `SearchServiceClient` 인스턴스.

> [!NOTE]
> 응용 프로그램에서 인덱스를 만들 응답, 보존 하는 경우 사용 된 `Indexes.CreateAsync` 메서드.

자세한 내용은 [.NET SDK를 사용 하 여 Azure Search 인덱스 만들기](/azure/search/search-create-index-dotnet/)합니다.

## <a name="deleting-the-azure-search-index"></a>Azure Search 인덱스를 삭제합니다.

호출 하 여 인덱스를 삭제할 수 있습니다 `Indexes.Delete` 에 `SearchServiceClient` 인스턴스:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Azure Search 인덱스에 데이터 업로드

인덱스를 정의한 후 두 가지 모델 중 하나를 사용 하 여 데이터를 업로드할 수 있습니다.

- **끌어오기 모델** – 데이터는 Azure Cosmos DB, Azure SQL Database, Azure Blob 저장소에서 주기적으로 수집 됩니다. 또는 Azure 가상 컴퓨터에서 호스팅되는 SQL Server입니다.
- **밀어넣기 모델** – 데이터에 프로그래밍 방식으로 인덱스에 보내집니다. 이 문서에 채택 모델입니다.

`SearchIndexClient` 인덱스로 데이터를 가져오려면 인스턴스를 만들어야 합니다. 이 호출 하 여 수행할 수 있습니다는 `SearchServiceClient.Indexes.GetClient` 메서드를 다음 코드 예제에서 설명한 것 처럼:

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

데이터 인덱스를 가져올 수는으로 패키지를 `IndexBatch` 개체의 컬렉션을 캡슐화 하는 `IndexAction` 개체입니다. 각 `IndexAction` 인스턴스 문서 및 Azure Search 문서에 대해 수행할 작업을 지시 하는 속성을 포함 합니다. 위의 코드 예제는 `IndexAction.Upload` 작업 문서의 새로운 이면 인덱스에 삽입 되는 결과 지정 되었거나 이미 있는 경우 대체 합니다. `IndexBatch` 개체는 다음 호출 하 여 인덱스에 전송 됩니다 합니다 `Documents.Index` 메서드를 `SearchIndexClient` 개체. 다른 인덱싱 작업에 대 한 정보를 참조 하세요 [사용할 인덱싱 동작 결정](/azure/search/search-import-data-dotnet#decide-which-indexing-action-to-use)합니다.

> [!NOTE]
> 단일 인덱싱 요청에만 1000 개의 문서를 포함할 수 있습니다.

위의 코드 예제는 `monkeyList` 익명 개체의 컬렉션에서 컬렉션을 만들 `Monkey` 개체입니다. 에 대 한 데이터를 만들고이 `id` 필드 및 파스칼식 대 / 소문자의 매핑을 확인 `Monkey` 카멜식 대 / 소문자 속성 이름을 검색 인덱스 필드 이름입니다. 이 매핑은 추가 하 여 수행할 수도 있습니다 또는 합니다 `[SerializePropertyNamesAsCamelCase]` 특성을 `Monkey` 클래스입니다.

자세한 내용은 [.NET SDK를 사용 하 여 Azure Search에 데이터 업로드](/azure/search/search-import-data-dotnet/)합니다.

## <a name="querying-the-azure-search-index"></a>Azure Search 인덱스 쿼리

`SearchIndexClient` 인스턴스 인덱스 쿼리를 만들어야 합니다. 것이 좋습니다 만들고 최소 권한의 원칙에 따라 응용 프로그램에서 쿼리를 실행 하는 경우는 `SearchIndexClient` 직접 전달 합니다 *쿼리 키* 인수로. 이렇게 하면 사용자가 인덱스 및 문서에 대 한 읽기 전용 액세스 합니다. 다음 코드 예제에서이 방법을 설명 합니다.

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient` 생성자 오버 로드는 검색 서비스 이름, 인덱스 이름 및 `SearchCredentials` 인수를 사용 하 여 개체를 `SearchCredentials` 개체 래핑 합니다 *쿼리 키* Azure Search 서비스에 대 한 합니다.

### <a name="search-queries"></a>검색 쿼리

호출 하 여 인덱스를 쿼리할 수 있습니다는 `Documents.SearchAsync` 메서드는 `SearchIndexClient` 다음 코드 예제에서 설명한 것 처럼 인스턴스:

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

합니다 `SearchAsync` 메서드는 검색 텍스트 인수 및 선택적 `SearchParameters` 쿼리를 구체화 하는 데 사용할 수 있습니다. 설정 하 여 필터 쿼리를 지정할 수 있습니다 하는 동안 검색 텍스트 인수로 검색 쿼리를 지정 합니다 `Filter` 의 속성을 `SearchParameters` 인수. 다음 코드 예제에서는 형식 모두 쿼리 하는 방법을 보여 줍니다.

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

이 필터 쿼리 전체 인덱스에 적용 되 고 결과에서 문서를 제거 합니다. 여기서는 `location` 필드는 중국 같음 및 같지 않음 베트남 합니다. 필터링 후에 검색 쿼리는 필터 쿼리 결과에서 수행 됩니다.

> [!NOTE]
> 를 검색 하지 않고 필터링 하려면 전달 `*` 검색 텍스트 인수입니다.

합니다 `SearchAsync` 메서드가 반환 되는 `DocumentSearchResult` 쿼리 결과 포함 하는 개체입니다. 각를 사용 하 여이 개체를 열거 `Document` 으로 만들어지는 개체는 `Monkey` 에 추가한 개체를 `Monkeys` `ObservableCollection` 표시에 대 한 합니다. 다음 스크린샷에서 표시 검색 쿼리 결과 Azure Search에서 반환 된:

![](azure-search-images/search.png "Search Results")

검색 및 필터링 하는 방법에 대 한 자세한 내용은 참조 하세요. [.NET SDK를 사용 하 여 Azure Search 인덱스 쿼리](/azure/search/search-query-dotnet/)합니다.

<a name="suggestions" />

### <a name="suggestion-queries"></a>제안 쿼리

쿼리를 기반으로 검색을 호출 하 여 azure Search가 제안 요청 될 수 있습니다.는 `Documents.SuggestAsync` 메서드는 `SearchIndexClient` 인스턴스. 다음 코드 예제에서이 확인할 수 있습니다.

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

합니다 `SuggestAsync` 메서드는 검색 텍스트 인수를 사용 하는 확인 기의 이름 (에 정의 된 인덱스), 및 선택적 `SuggestParameters` 쿼리를 구체화 하는 데 사용할 수 있습니다. `SuggestParameters` 인스턴스는 다음 속성을 설정 합니다.

- `UseFuzzyMatching` –로 설정 하면 `true`, Azure Search는 검색 텍스트에 대체 되었거나 누락 된 문자가 있는 경우에 제안을 찾습니다.
- `HighlightPreTag` – 제안 적중 항목 앞에 추가 되는 태그입니다.
- `HighlightPostTag` – 제안 적중에 추가 되는 태그입니다.
- `MinimumCoverage` – 나타내는 제안 쿼리에 쿼리를 처리 해야 하는 인덱스의 백분율을 성공을 보고 합니다. 기본값은 80입니다.
- `Top` -검색할 제안의 수입니다. 이 1과 5 기본값 100 사이의 정수 여야 합니다.

전체 효과 인덱스에서 상위 10 개 결과 강조 표시 된 적중 하 고 결과 철자가 비슷한 검색어 포함 된 문서에 포함 됩니다와 함께 반환 됩니다.

합니다 `SuggestAsync` 메서드가 반환 되는 `DocumentSuggestResult` 쿼리 결과 포함 하는 개체입니다. 각를 사용 하 여이 개체를 열거 `Document` 으로 만들어지는 개체는 `Monkey` 에 추가한 개체를 `Monkeys` `ObservableCollection` 표시에 대 한 합니다. 다음 스크린샷은 Azure Search에서 반환 되는 제안 결과 보여 줍니다.

![](azure-search-images/suggest.png "Suggestion Results")

샘플 응용 프로그램에서의 `SuggestAsync` 메서드는 사용자가 검색 용어 입력을 완료 하는 경우에 호출 됩니다. 그러나이 데도 사용할 수 있습니다 각 keypress에서 실행 하 여 자동 완성 검색 쿼리를 지원 하도록 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램에 Azure Search를 통합 하 여 Microsoft Azure Search 라이브러리를 사용 하는 방법을 보여 줍니다. Azure Search는 인덱싱 및 쿼리 업로드 된 데이터에 대 한 기능을 제공 하는 클라우드 서비스입니다. 인프라 요구 사항 및 응용 프로그램에 검색 기능을 구현 하 여 일반적으로 관련 된 검색 알고리즘 복잡성이 제거 됩니다.

## <a name="related-links"></a>관련 링크

- [Azure Search (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresearch)
- [Azure Search 설명서](/azure/search/)
- [Microsoft Azure Search 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.Search/)
