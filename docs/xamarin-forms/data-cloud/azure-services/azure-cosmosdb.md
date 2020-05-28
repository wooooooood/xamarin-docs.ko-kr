---
title: 에서 Azure Cosmos DB 문서 데이터베이스 사용Xamarin.Forms
description: 이 문서에서는 Azure Cosmos DB .NET Standard 클라이언트 라이브러리를 사용 하 여 Azure Cosmos DB 문서 데이터베이스를 응용 프로그램에 통합 하는 방법을 설명 합니다 Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
ms.custom: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 47b35d394eab339a8e9a1f81880e6de4233f29b6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127090"
---
# <a name="consume-an-azure-cosmos-db-document-database-in-xamarinforms"></a>에서 Azure Cosmos DB 문서 데이터베이스 사용Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)

_Azure Cosmos DB 문서 데이터베이스는 JSON 문서에 대 한 짧은 대기 시간 액세스를 제공 하는 NoSQL 데이터베이스로, 원활한 확장 및 전역 복제가 필요한 응용 프로그램에 대해 빠르고 가용성이 뛰어난 확장 가능한 데이터베이스 서비스를 제공 합니다. 이 문서에서는 Azure Cosmos DB .NET Standard 클라이언트 라이브러리를 사용 하 여 Azure Cosmos DB 문서 데이터베이스를 응용 프로그램에 통합 하는 방법을 설명 합니다 Xamarin.Forms ._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB 비디오**

Azure 구독을 사용 하 여 Azure Cosmos DB 문서 데이터베이스 계정을 프로 비전 할 수 있습니다. 각 데이터베이스 계정은 0 개 이상의 데이터베이스를 포함할 수 있습니다. Azure Cosmos DB의 문서 데이터베이스는 문서 컬렉션 및 사용자에 대 한 논리적 컨테이너입니다.

Azure Cosmos DB 문서 데이터베이스에는 0 개 이상의 문서 컬렉션이 포함 될 수 있습니다. 각 문서 컬렉션은 다른 성능 수준을 가질 수 있으므로 자주 액세스 하는 컬렉션에 대해 더 많은 처리량을 지정 하 고 자주 액세스 하지 않는 컬렉션의 처리량은 줄일 수 있습니다.

각 문서 컬렉션은 0 개 이상의 JSON 문서로 구성 됩니다. 컬렉션의 문서는 스키마를 사용 하지 않으므로 동일한 구조 나 필드를 공유할 필요가 없습니다. 문서 컬렉션에 문서를 추가 하면 Cosmos DB 자동으로 인덱싱하고 쿼리를 사용할 수 있게 됩니다.

개발을 위해 에뮬레이터를 통해 문서 데이터베이스를 사용할 수도 있습니다. 에뮬레이터를 사용 하면 Azure 구독을 만들거나 비용을 들이지 않고도 응용 프로그램을 로컬로 개발 하 고 테스트할 수 있습니다. 에뮬레이터에 대 한 자세한 내용은 [Azure Cosmos DB 에뮬레이터를 사용 하 여 로컬로 개발](/azure/cosmos-db/local-emulator/)을 참조 하세요.

이 문서와 함께 제공 되는 샘플 응용 프로그램은 태스크가 Azure Cosmos DB 문서 데이터베이스에 저장 되는 Todo 목록 응용 프로그램을 보여 줍니다. 예제 응용 프로그램에 대 한 자세한 내용은 [샘플 이해](~/xamarin-forms/data-cloud/web-services/introduction.md)를 참조 하세요.

Azure Cosmos DB에 대 한 자세한 내용은 [Azure Cosmos DB 설명서](/azure/cosmos-db/)를 참조 하세요.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

## <a name="setup"></a>설치 프로그램

Azure Cosmos DB 문서 데이터베이스를 응용 프로그램에 통합 하는 프로세스는 다음과 같습니다 Xamarin.Forms .

1. Cosmos DB 계정을 만듭니다. 자세한 내용은 [Azure Cosmos DB 계정 만들기](/azure/cosmos-db/sql-api-dotnetcore-get-started#create-an-azure-cosmos-account)를 참조 하세요.
1. 솔루션의 플랫폼 프로젝트에 [Azure Cosmos DB .NET Standard 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) NuGet 패키지를 추가 Xamarin.Forms 합니다.
1. `using`, 및 네임 스페이스에 대 한 지시문을 `Microsoft.Azure.Documents` `Microsoft.Azure.Documents.Client` `Microsoft.Azure.Documents.Linq` Cosmos DB 계정에 액세스 하는 클래스에 추가 합니다.

이러한 단계를 수행한 후에는 Azure Cosmos DB .NET Standard 클라이언트 라이브러리를 사용 하 여 문서 데이터베이스에 대해 요청을 구성 하 고 실행할 수 있습니다.

> [!NOTE]
> Azure Cosmos DB .NET Standard 클라이언트 라이브러리는 PCL (이식 가능한 클래스 라이브러리) 프로젝트가 아닌 플랫폼 프로젝트에만 설치할 수 있습니다. 따라서 샘플 응용 프로그램은 코드 중복을 방지 하기 위한 SAP (공유 액세스 프로젝트)입니다. 그러나 클래스를 `DependencyService` PCL 프로젝트에서 사용 하 여 플랫폼별 프로젝트에 포함 된 Azure Cosmos DB .NET Standard 클라이언트 라이브러리 코드를 호출할 수 있습니다.

## <a name="consuming-the-azure-cosmos-db-account"></a>Azure Cosmos DB 계정 사용

`DocumentClient`형식은 Azure Cosmos DB 계정에 액세스 하는 데 사용 되는 끝점, 자격 증명 및 연결 정책을 캡슐화 하 고 계정에 대해 요청을 구성 하 고 실행 하는 데 사용 됩니다. 다음 코드 예제에서는이 클래스의 인스턴스를 만드는 방법을 보여 줍니다.

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

생성자에 Cosmos DB Uri 및 기본 키를 제공 해야 합니다 `DocumentClient` . Azure Portal에서 가져올 수 있습니다. 자세한 내용은 [Azure Cosmos DB 계정에 연결](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect)을 참조 하세요.

### <a name="creating-a-database"></a>데이터베이스 만들기

문서 데이터베이스는 문서 컬렉션 및 사용자의 논리적 컨테이너 이며, Azure Portal에서 만들거나 메서드를 사용 하 여 프로그래밍 방식으로 만들 수 있습니다 `DocumentClient.CreateDatabaseIfNotExistsAsync` .

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

`CreateDatabaseIfNotExistsAsync`메서드는 개체를 인수로 지정 하 고 `Database` `Database` 개체는 데이터베이스 이름을 속성으로 지정 합니다 `Id` . `CreateDatabaseIfNotExistsAsync`이 메서드는 데이터베이스가 없는 경우 데이터베이스를 만들고 데이터베이스를 이미 있는 경우이를 반환 합니다. 그러나 샘플 응용 프로그램은 메서드에서 반환 된 모든 데이터를 무시 합니다 `CreateDatabaseIfNotExistsAsync` .

> [!NOTE]
> `CreateDatabaseIfNotExistsAsync`메서드는 개체를 반환 하 고, `Task<ResourceResponse<Database>>` 응답의 상태 코드를 검사 하 여 데이터베이스를 만들었는지 아니면 기존 데이터베이스가 반환 되었는지 확인할 수 있습니다.

### <a name="creating-a-document-collection"></a>문서 컬렉션 만들기

문서 컬렉션은 JSON 문서에 대 한 컨테이너 이며, Azure Portal에서 만들거나 메서드를 사용 하 여 프로그래밍 방식으로 만들 수 있습니다 `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` .

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

`CreateDocumentCollectionIfNotExistsAsync`메서드에는 두 개의 강제 인수, 즉로 지정 된 데이터베이스 이름과 `Uri` 개체가 필요 `DocumentCollection` 합니다. `DocumentCollection`개체는 속성을 사용 하 여 이름이 지정 된 문서 컬렉션을 나타냅니다 `Id` . `CreateDocumentCollectionIfNotExistsAsync`메서드는 문서 컬렉션을 만듭니다 (없는 경우). 또는 문서 컬렉션 (이미 있는 경우)을 반환 합니다. 그러나 샘플 응용 프로그램은 메서드에서 반환 된 모든 데이터를 무시 합니다 `CreateDocumentCollectionIfNotExistsAsync` .

> [!NOTE]
> `CreateDocumentCollectionIfNotExistsAsync`메서드는 개체를 반환 하 `Task<ResourceResponse<DocumentCollection>>` 고 응답의 상태 코드를 검사 하 여 문서 컬렉션을 만들었는지 또는 기존 문서 컬렉션이 반환 되었는지 확인할 수 있습니다.

필요에 따라 `CreateDocumentCollectionIfNotExistsAsync` 메서드는 `RequestOptions` Cosmos DB 계정에 발급 된 요청에 대해 지정할 수 있는 옵션을 캡슐화 하는 개체를 지정할 수도 있습니다. `RequestOptions.OfferThroughput`속성은 문서 컬렉션의 성능 수준을 정의 하는 데 사용 되 고, 샘플 응용 프로그램에서는 초당 400 요청 단위로 설정 됩니다. 컬렉션에 자주 액세스 하거나 자주 액세스 하지 않을 지 여부에 따라이 값을 늘리거나 줄입니다.

> [!IMPORTANT]
> 이 메서드는 `CreateDocumentCollectionIfNotExistsAsync` 가격 책정에 영향을 주는 예약 된 처리량을 사용 하 여 새 컬렉션을 만듭니다.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>문서 컬렉션 문서 검색

문서 컬렉션의 내용은 문서 쿼리를 만들고 실행 하 여 검색할 수 있습니다. 문서 쿼리는 메서드를 사용 하 여 만듭니다 `DocumentClient.CreateDocumentQuery` .

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

이 쿼리는 지정 된 컬렉션에서 모든 문서를 비동기적으로 검색 하 고 문서를 `List<TodoItem>` 표시 하기 위해 컬렉션에 배치 합니다.

`CreateDocumentQuery<T>`메서드는 `Uri` 문서에 대해 쿼리해야 하는 컬렉션을 나타내는 인수를 지정 합니다. 이 예제에서 변수는 `collectionLink` `Uri` 문서를 검색할 문서 컬렉션을 나타내는를 지정 하는 클래스 수준 필드입니다.

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

`CreateDocumentQuery<T>`메서드는 동기적으로 실행 되는 쿼리를 만들고 개체를 반환 `IQueryable<T>` 합니다. 그러나 메서드는 `AsDocumentQuery` `IQueryable<T>` 개체를 `IDocumentQuery<T>` 비동기적으로 실행할 수 있는 개체로 변환 합니다. 비동기 쿼리는 `IDocumentQuery<T>.ExecuteNextAsync` 문서 데이터베이스에서 결과의 다음 페이지를 검색 하는 메서드를 사용 하 여 실행 되며, `IDocumentQuery<T>.HasMoreResults` 쿼리에서 반환 될 추가 결과가 있는지 여부를 나타내는 속성을 사용 하 여 실행 됩니다.

쿼리에 절을 포함 하 여 문서를 필터링 할 수 있습니다 `Where` . 그러면 문서 컬렉션에 대 한 쿼리에 필터링 조건자가 적용 됩니다.

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

이 쿼리는 컬렉션에서 속성이 인 모든 문서를 검색 `Done` `false` 합니다.

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>문서 컬렉션에 문서 삽입

문서는 사용자 정의 JSON 콘텐츠 이며 메서드를 사용 하 여 문서 컬렉션에 삽입할 수 있습니다 `DocumentClient.CreateDocumentAsync` .

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

`CreateDocumentAsync`메서드는 문서를 `Uri` 삽입할 컬렉션을 나타내는 인수와 `object` 삽입할 문서를 나타내는 인수를 지정 합니다.

### <a name="replacing-a-document-in-a-document-collection"></a>문서 컬렉션에서 문서 바꾸기

문서 컬렉션에서 메서드를 사용 하 여 문서를 바꿀 수 있습니다 `DocumentClient.ReplaceDocumentAsync` .

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

`ReplaceDocumentAsync`메서드는 `Uri` 컬렉션에서 바꾸려는 문서를 나타내는 인수와 `object` 업데이트 된 문서 데이터를 나타내는 인수를 지정 합니다.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>문서 컬렉션에서 문서 삭제

문서 컬렉션에서 메서드를 사용 하 여 문서를 삭제할 수 있습니다 `DocumentClient.DeleteDocumentAsync` .

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

`DeleteDocumentAsync`메서드는 `Uri` 컬렉션에서 삭제 해야 하는 문서를 나타내는 인수를 지정 합니다.

### <a name="deleting-a-document-collection"></a>문서 컬렉션 삭제

메서드를 사용 하 여 데이터베이스에서 문서 컬렉션을 삭제할 수 있습니다 `DocumentClient.DeleteDocumentCollectionAsync` .

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

`DeleteDocumentCollectionAsync`메서드는 `Uri` 삭제할 문서 컬렉션을 나타내는 인수를 지정 합니다. 이 메서드를 호출 하면 컬렉션에 저장 된 문서도 삭제 됩니다.

### <a name="deleting-a-database"></a>데이터베이스 삭제

메서드를 사용 하 여 Cosmos DB 데이터베이스 계정에서 데이터베이스를 삭제할 수 있습니다 `DocumentClient.DeleteDatabaesAsync` .

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

`DeleteDatabaseAsync`메서드는 `Uri` 삭제할 데이터베이스를 나타내는 인수를 지정 합니다. 이 메서드를 호출 하면 데이터베이스에 저장 된 문서 컬렉션과 문서 컬렉션에 저장 된 문서도 삭제 됩니다.

## <a name="summary"></a>요약

이 문서에서는 Azure Cosmos DB .NET Standard 클라이언트 라이브러리를 사용 하 여 Azure Cosmos DB 문서 데이터베이스를 응용 프로그램에 통합 하는 방법에 대해 설명 Xamarin.Forms 했습니다. Azure Cosmos DB 문서 데이터베이스는 JSON 문서에 대 한 짧은 대기 시간 액세스를 제공 하는 NoSQL 데이터베이스로, 원활한 확장 및 전역 복제가 필요한 응용 프로그램에 대해 빠르고 가용성이 뛰어난 확장 가능한 데이터베이스 서비스를 제공 합니다.

## <a name="related-links"></a>관련 링크

- [Todo Azure Cosmos DB (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)
- [Azure Cosmos DB 설명서](/azure/cosmos-db/)
- [Azure Cosmos DB .NET Standard 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
