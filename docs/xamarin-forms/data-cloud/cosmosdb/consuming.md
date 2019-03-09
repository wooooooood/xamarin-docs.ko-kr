---
title: Azure Cosmos DB 문서 데이터베이스를 사용합니다.
description: 이 문서에서는 Azure Cosmos DB.NET Standard 클라이언트 라이브러리를 사용 하 여 Xamarin.Forms 응용 프로그램에 Azure Cosmos DB 문서 데이터베이스를 통합 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: bc50f3567135d5b1dc805fa691cdd95acadf34f1
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667554"
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Azure Cosmos DB 문서 데이터베이스를 사용합니다.

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)

_Azure Cosmos DB 문서 데이터베이스는 원활한 확장 및 전역 복제가 필요한 응용 프로그램에 대 한 빠른, 고가용성, 확장성 있는 데이터베이스 서비스를 제공 하는 JSON 문서에 대 한 대기 시간이 짧은 액세스를 제공 하는 NoSQL 데이터베이스. 이 문서에서는 Azure Cosmos DB.NET Standard 클라이언트 라이브러리를 사용 하 여 Xamarin.Forms 응용 프로그램에 Azure Cosmos DB 문서 데이터베이스를 통합 하는 방법에 설명 합니다._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB에서 [Xamarin University](https://university.xamarin.com/)**

Azure Cosmos DB 문서 데이터베이스 계정은 Azure 구독을 사용 하 여 프로 비전 할 수 있습니다. 각 데이터베이스 계정에는 0 개 이상의 데이터베이스가 있을 수 있습니다. Azure Cosmos db에서 문서 데이터베이스는 문서 컬렉션 및 사용자에 대 한 논리적 컨테이너입니다.

Azure Cosmos DB 문서 데이터베이스는 0 개 이상의 문서 컬렉션을 포함할 수 있습니다. 각 문서 컬렉션에는 더 많은 처리량 자주 액세스 되는 컬렉션에 지정 되어야 하 고 더 적은 처리량 자주 액세스 되는 컬렉션에 대 한 다양 한 성능 수준을 포함할 수 있습니다.

0 개 이상의 JSON 문서의 각 문서 컬렉션 구성 됩니다. 컬렉션의 문서 스키마 제약이 없는 되며 따라서 동일한 구조 또는 필드를 공유할 필요가 없습니다. 문서는 문서 컬렉션에 추가 되 면 Cosmos DB 및 쿼리할 수에 제공 되 면 자동으로 인덱싱합니다.

문서 데이터베이스 개발을 위해 에뮬레이터를 통해 사용 될 수도 있습니다. 에뮬레이터를 사용 하 여 응용 프로그램 개발 하 고 Azure 구독을 만들거나 모든 비용이 발생 하지 않고도 로컬에서 테스트 합니다. 에뮬레이터에 대 한 자세한 내용은 참조 하세요. [Azure Cosmos DB 에뮬레이터를 사용 하 여 로컬로 개발](/azure/cosmos-db/local-emulator/)합니다.

이 문서 및 샘플 응용 프로그램을 함께 제공 되는 작업을 Azure Cosmos DB 문서 데이터베이스에 저장 되어 있는 Todo 목록 응용 프로그램을 보여 줍니다. 샘플 응용 프로그램에 대 한 자세한 내용은 참조 하세요. [샘플 이해](~/xamarin-forms/data-cloud/walkthrough.md)합니다.

Azure Cosmos DB에 대 한 자세한 내용은 참조는 [Azure Cosmos DB 설명서](/azure/cosmos-db/)합니다.

## <a name="setup"></a>설정

Xamarin.Forms 응용 프로그램에 Azure Cosmos DB 문서 데이터베이스를 통합 하기 위한 프로세스는 다음과 같습니다.

1. Cosmos DB 계정을 만듭니다. 자세한 내용은 [Azure Cosmos DB 계정을 만들고](/azure/cosmos-db/sql-api-dotnetcore-get-started#create-an-azure-cosmos-account)합니다.
1. 추가 된 [Azure Cosmos DB.NET Standard 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) Xamarin.Forms 솔루션의 플랫폼 프로젝트에 NuGet 패키지.
1. 추가 `using` 에 대 한 지시문의 `Microsoft.Azure.Documents`, `Microsoft.Azure.Documents.Client`, 및 `Microsoft.Azure.Documents.Linq` Cosmos DB 계정에 액세스 하는 클래스에 네임 스페이스입니다.

이러한 단계를 수행한 후 Azure Cosmos DB.NET Standard 클라이언트 라이브러리를 구성 하 고 문서 데이터베이스에 대 한 요청을 실행 하려면 사용할 수 있습니다.

> [!NOTE]
> 이식 가능한 클래스 라이브러리 (PCL) 프로젝트로 하지 플랫폼 프로젝트에 Azure Cosmos DB.NET Standard 클라이언트 라이브러리만 설치할 수 있습니다. 따라서 샘플 응용 프로그램 코드 중복을 피하기 위해 액세스 프로젝트 공유 (SAP)입니다. 그러나는 `DependencyService` 클래스 수를 PCL 프로젝트에 플랫폼별 프로젝트에 포함 된 Azure Cosmos DB.NET Standard 클라이언트 라이브러리 코드를 호출 합니다.

## <a name="consuming-the-azure-cosmos-db-account"></a>Azure Cosmos DB 계정 사용

`DocumentClient` 유형 끝점, 자격 증명 및 Azure Cosmos DB 계정에 액세스 하는 데 사용 되는 연결 정책을 캡슐화 및 구성 하 고 계정에 대해 요청을 실행 하는 데 사용 됩니다. 다음 코드 예제에는이 클래스의 인스턴스를 만드는 방법을 보여 줍니다.

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Cosmos DB Uri 및 기본 키를 제공 해야 합니다 `DocumentClient` 생성자입니다. 이러한 Azure Portal에서 가져올 수 있습니다. 자세한 내용은 [Azure Cosmos DB 계정에 연결](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect)합니다.

### <a name="creating-a-database"></a>데이터베이스 만들기

문서 데이터베이스는 문서 컬렉션 및 사용자에 대 한 논리적 컨테이너 및 Azure Portal에서 만들거나 프로그래밍 방식으로 사용 하 여 수를 `DocumentClient.CreateDatabaseIfNotExistsAsync` 메서드:

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

`CreateDatabaseIfNotExistsAsync` 메서드는 지정을 `Database` 인수로 사용 하 여 개체를 `Database` 데이터베이스 이름을 지정 하는 개체 해당 `Id` 속성입니다. `CreateDatabaseIfNotExistsAsync` 메서드는 존재 하지 않는 또는 이미 있는 경우 데이터베이스를 반환 하는 경우 데이터베이스를 만듭니다. 샘플 응용 프로그램에서 반환 된 모든 데이터를 무시 하는 반면는 `CreateDatabaseIfNotExistsAsync` 메서드.

> [!NOTE]
> 합니다 `CreateDatabaseIfNotExistsAsync` 메서드가 반환 되는 `Task<ResourceResponse<Database>>` 있는지 여부를 확인 데이터베이스가 생성 된 기존 데이터베이스에서 반환 된 개체 및 응답의 상태 코드를 확인할 수 있습니다.

### <a name="creating-a-document-collection"></a>문서 컬렉션 만들기

문서 컬렉션은 JSON 문서용 컨테이너 및 Azure Portal에서 만들거나 프로그래밍 방식으로 사용 하 여 수를 `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` 메서드:

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

합니다 `CreateDocumentCollectionIfNotExistsAsync` 메서드 두 필수 인수가 – 필요로 지정 된 데이터베이스 이름을 `Uri`, 및 `DocumentCollection` 개체입니다. 합니다 `DocumentCollection` 개체와 이름이 지정 된 문서 컬렉션을 나타냅니다는 `Id` 속성입니다. `CreateDocumentCollectionIfNotExistsAsync` 이미 있는 경우 문서 컬렉션을 반환 하거나 존재 하지 않는 경우 메서드는 문서 컬렉션을 만듭니다. 샘플 응용 프로그램에서 반환 된 모든 데이터를 무시 하는 반면는 `CreateDocumentCollectionIfNotExistsAsync` 메서드.

> [!NOTE]
> 합니다 `CreateDocumentCollectionIfNotExistsAsync` 메서드가 반환 되는 `Task<ResourceResponse<DocumentCollection>>` 있는지 여부를 확인 문서 컬렉션이 생성 된 기존 문서 컬렉션을 반환 된 개체 및 응답의 상태 코드를 확인할 수 있습니다.

필요에 따라 합니다 `CreateDocumentCollectionIfNotExistsAsync` 메서드를 지정할 수도 있습니다는 `RequestOptions` Cosmos DB 계정에 발급 요청에 대해 지정할 수 있는 옵션을 캡슐화 하는 개체입니다. `RequestOptions.OfferThroughput` 속성 문서 컬렉션의 성능 수준을 정의 하는 데 사용 됩니다 하 고 샘플 응용 프로그램에서 초당 요청 단위 400으로 설정 됩니다. 이 값을 증가 또는 컬렉션은 액세스할 수 있는지 여부 자주 또는 드물게에 따라 감소 시켜야 합니다.

> [!IMPORTANT]
> `CreateDocumentCollectionIfNotExistsAsync` 메서드는 가격 책정 의미가 있는 예약 된 처리량을 사용 하 여 새 컬렉션을 만듭니다.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>문서 컬렉션 문서 검색

문서 컬렉션의 콘텐츠를 만들고 문서 쿼리를 실행 하 여 검색할 수 있습니다. 문서 쿼리를 사용 하 여 만들어집니다는 `DocumentClient.CreateDocumentQuery` 메서드:

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

이 쿼리는 비동기적으로 지정된 된 컬렉션에서 모든 문서를 검색 및 배치에서 문서를 `List<TodoItem>` 표시에 대 한 컬렉션입니다.

합니다 `CreateDocumentQuery<T>` 메서드는 지정을 `Uri` 문서용 쿼리해야 하는 컬렉션을 나타내는 인수입니다. 이 예제는 `collectionLink` 변수가 지정 하는 클래스 수준 필드를 `Uri` 에서 문서를 검색 문서의 컬렉션을 나타내는:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

합니다 `CreateDocumentQuery<T>` 메서드는 동기적으로 실행 되 고 반환 하는 쿼리를 만듭니다는 `IQueryable<T>` 개체입니다. 그러나를 `AsDocumentQuery` 메서드 변환 합니다 `IQueryable<T>` 개체는 `IDocumentQuery<T>` 비동기적으로 실행 될 수 있는 개체입니다. 비동기 쿼리를 사용 하 여 실행할 합니다 `IDocumentQuery<T>.ExecuteNextAsync` 메서드를 사용 하 여 문서 데이터베이스에서 결과의 다음 페이지를 검색 하는 `IDocumentQuery<T>.HasMoreResults` 쿼리에서 반환할 추가 결과가 있는지 여부를 나타내는 속성입니다.

문서 수를 포함 하 여 서버 쪽 필터링을 `Where` 문서 컬렉션에 대해 쿼리를 필터링 하는 조건자를 적용 되는 쿼리 절:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

이 쿼리는 모든 검색 된 컬렉션에서 문서 `Done` 속성이 `false`합니다.

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>문서 컬렉션에 문서를 삽입합니다.

문서는 사용자 정의 JSON 콘텐츠 및 사용 하 여 문서 컬렉션에 삽입할 수는 `DocumentClient.CreateDocumentAsync` 메서드:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

`CreateDocumentAsync` 메서드는 지정을 `Uri` 문서를 삽입 해야 하는 컬렉션을 나타내는 인수 및 `object` 삽입할 문서를 나타내는 인수입니다.

### <a name="replacing-a-document-in-a-document-collection"></a>문서 컬렉션에서 문서 바꾸기

사용 하 여 문서 컬렉션에서 문서를 대체할 수 있습니다는 `DocumentClient.ReplaceDocumentAsync` 메서드:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

`ReplaceDocumentAsync` 메서드는 지정을 `Uri` 대체 해야 하는 컬렉션의 문서를 나타내는 인수 및 `object` 업데이트 된 문서 데이터를 나타내는 인수입니다.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>문서 컬렉션에서 문서를 삭제 하는 중

문서를 사용 하 여 문서 컬렉션에서 삭제할 수는 `DocumentClient.DeleteDocumentAsync` 메서드:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

`DeleteDocumentAsync` 메서드는 지정을 `Uri` 삭제 해야 하는 컬렉션의 문서를 나타내는 인수입니다.

### <a name="deleting-a-document-collection"></a>문서 컬렉션 삭제

문서 컬렉션을 사용 하 여 데이터베이스에서 삭제할 수는 `DocumentClient.DeleteDocumentCollectionAsync` 메서드:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

`DeleteDocumentCollectionAsync` 메서드를 `Uri` 삭제할 문서 컬렉션을 나타내는 인수입니다. 이 메서드를 호출도 삭제 됩니다 컬렉션에 저장 된 문서를 참고 합니다.

### <a name="deleting-a-database"></a>데이터베이스를 삭제합니다.

데이터베이스를 사용 하 여 Cosmos DB 데이터베이스 계정에서 삭제할 수는 `DocumentClient.DeleteDatabaesAsync` 메서드:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

`DeleteDatabaseAsync` 메서드를 `Uri` 삭제할 데이터베이스를 나타내는 인수입니다. 이 메서드를 호출도 삭제 됩니다 데이터베이스에 저장 된 문서 컬렉션 및 문서 컬렉션에 저장 된 문서를 참고 합니다.

## <a name="summary"></a>요약

이 문서에서는 Azure Cosmos DB.NET Standard 클라이언트 라이브러리를 사용 하 여 Xamarin.Forms 응용 프로그램에 Azure Cosmos DB 문서 데이터베이스를 통합 하는 방법을 설명 합니다. Azure Cosmos DB 문서 데이터베이스는 원활한 확장 및 전역 복제가 필요한 응용 프로그램에 대 한 빠른, 고가용성, 확장성 있는 데이터베이스 서비스를 제공 하는 JSON 문서에 대 한 대기 시간이 짧은 액세스를 제공 하는 NoSQL 데이터베이스.


## <a name="related-links"></a>관련 링크

- [Todo Azure Cosmos DB (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Azure Cosmos DB 설명서](/azure/cosmos-db/)
- [Azure Cosmos DB.NET Standard 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
