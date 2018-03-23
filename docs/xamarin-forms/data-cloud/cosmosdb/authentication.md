---
title: Azure DB Cosmos 문서 데이터베이스와 사용자 인증
description: Azure DB Cosmos 문서 데이터베이스에는 여러 서버 및 파티션 저장소 및 처리량을 지원 하면서 확장할 수 있는 분할 된 컬렉션을 지원 합니다. 이 문서에서는 사용자가 자신의 문서 Xamarin.Forms 응용 프로그램에만 액세스할 수 있도록 분할 된 컬렉션을 사용한 액세스 제어를 결합 하는 방법을 설명 합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 8de64d6489b4022e43bcf694f3b13d6f7eaaecbd
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2018
---
# <a name="authenticating-users-with-an-azure-cosmos-db-document-database"></a>Azure DB Cosmos 문서 데이터베이스와 사용자 인증

_Azure DB Cosmos 문서 데이터베이스에는 여러 서버 및 파티션 저장소 및 처리량을 지원 하면서 확장할 수 있는 분할 된 컬렉션을 지원 합니다. 이 문서에서는 사용자가 자신의 문서 Xamarin.Forms 응용 프로그램에만 액세스할 수 있도록 분할 된 컬렉션을 사용한 액세스 제어를 결합 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

분할 된 컬렉션을 만들 때 파티션 키를 지정 해야 하 고 동일한 파티션 키를 사용 하 여 문서를 동일한 파티션에 저장 됩니다. 따라서 파티션 키로 사용자의 id를 지정 하가 해당 사용자에 대 한 문서를 저장할만 분할 된 컬렉션을 발생 합니다. 이 또한 그래야 Azure Cosmos DB 문서 데이터베이스 사용자의 수로 확장 하 고 항목 증가 합니다.

모든 컬렉션에 대 한 액세스를 부여 해야 하 고 SQL API 액세스 제어 모델 두 가지 유형의 액세스 구문 정의 합니다.

- **마스터 키** Cosmos DB 계정 내에서 모든 리소스에 대 한 완전 한 관리 액세스를 사용 하도록 설정 하 고 Cosmos DB 계정이 만들어질 때 만들어집니다.
- **리소스 토큰이** 데이터베이스의 사용자와 사용자에 게 컬렉션 또는 문서와 같은 특정 Cosmos DB 리소스에 대 한 사용 권한을 간의 관계를 캡처합니다.

마스터 키가 노출 악성 또는 부주의로 사용 가능성을 Cosmos DB 계정을 열립니다. 그러나 Azure Cosmos DB 리소스 토큰 읽기, 쓰기 및 부여 된 권한에 따라 Azure Cosmos DB 계정에 특정 리소스를 삭제 하는 클라이언트 수에 대 한 안전한 메커니즘을 제공 합니다.

요청에 일반적인 방법은, 생성 및 모바일 응용 프로그램에 리소스 토큰을 전달 하 리소스 토큰 broker를 사용 하는 것입니다. 다음 다이어그램에는 상위 수준 개요를 예제 응용 프로그램 리소스 토큰 broker을 사용 하 여 문서 데이터베이스 데이터에 대 한 액세스를 관리 하는 방법을 보여 줍니다.

![](authentication-images/documentdb-authentication.png "문서 데이터베이스 인증 프로세스")

리소스 토큰 브로커는 Cosmos DB 계정의 마스터 키를 소유 하는 Azure 앱 서비스에서 호스트 하는 중간 계층 웹 API 서비스. 리소스 토큰 broker를 사용 하 여 다음과 같은 문서 데이터베이스 데이터에 대 한 액세스를 관리 하는 샘플 응용 프로그램:

1. Xamarin.Forms 응용 프로그램에 로그인을 인증 흐름을 시작 하려면 Azure 앱 서비스에 연결 합니다.
1. Azure 앱 서비스 Facebook과는 OAuth 인증 흐름을 수행합니다. 인증 흐름 완료 되 면 Xamarin.Forms 응용 프로그램 액세스 토큰을 수신 합니다.
1. Xamarin.Forms 응용 프로그램 리소스 토큰 브로커에서 리소스 토큰을 요청 하 고 액세스 토큰을 사용 합니다.
1. 리소스 토큰 broker Facebook에서 사용자의 id를 요청 하려면 액세스 토큰을 사용 합니다. 인증된 된 사용자의 분할 된 컬렉션에 대 한 읽기/쓰기 액세스 권한을 부여 하는 데 사용 되는 Cosmos DB에서 리소스 토큰을 요청 하는 사용자의 id 사용 됩니다.
1. Xamarin.Forms 응용 프로그램 리소스 토큰에 의해 정의 된 사용 권한을 가진 Cosmos DB 리소스에 직접 액세스할 리소스 토큰을 사용 합니다.

> [!NOTE]
> 리소스 토큰이 만료 되 면 후속 문서 데이터베이스 요청 401 권한이 없음된 예외가 표시 됩니다. 이 시점에서 Xamarin.Forms 응용 프로그램 id 다시 설정 하 고 새 리소스 토큰을 요청 해야 합니다.

Cosmos DB 분할에 대 한 자세한 내용은 참조 [파티션과 Azure Cosmos DB에 크기 조정 하는 방법을](/azure/cosmos-db/partition-data/)합니다. Cosmos DB 액세스 제어에 대 한 자세한 내용은 참조 [Cosmos DB 데이터에 대 한 액세스 보호](/azure/cosmos-db/secure-access-to-data/) 및 [SQL API의 액세스 제어](/rest/api/documentdb/access-control-on-documentdb-resources/)합니다.

## <a name="setup"></a>설정

리소스 토큰 브로커 Xamarin.Forms 응용 프로그램으로 통합 하기 위한 프로세스는 다음과 같습니다.

1. 액세스 제어를 사용 하는 Cosmos DB 계정을 만듭니다. 자세한 내용은 참조 [Cosmos DB 구성](#cosmosdb_configuration)합니다.
1. Azure 앱 서비스를 사용 하 여 리소스 토큰 브로커 호스트를 만듭니다. 자세한 내용은 참조 [Azure 응용 프로그램 서비스 구성](#app_service_configuration)합니다.
1. 인증을 수행 하는 Facebook 응용 프로그램을 만듭니다. 자세한 내용은 참조 [Facebook 응용 프로그램 구성](#facebook_configuration)합니다.
1. Facebook과 쉬운 인증을 수행 하는 Azure 앱 서비스를 구성 합니다. 자세한 내용은 참조 [Azure 앱 서비스 인증 구성](#app_service_authentication_configuration)합니다.
1. Cosmos DB 및 Azure 앱 서비스와 통신 하는 Xamarin.Forms 샘플 응용 프로그램을 구성 합니다. 자세한 내용은 참조 [Xamarin.Forms 응용 프로그램 구성](#forms_application_configuration)합니다.

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB 구성

액세스 제어를 사용 하는 Cosmos DB 계정을 만들기 위한 프로세스는 다음과 같습니다.

1. Cosmos DB 계정을 만듭니다. 자세한 내용은 참조 [Azure Cosmos DB 계정 만들기](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)합니다.
1. Cosmos DB 계정에서 명명 된 새 컬렉션을 만들 `UserItems`의 파티션 키를 지정 하 `/userid`합니다.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure 앱 서비스 구성

Azure 앱 서비스에서 리소스 토큰 브로커를 호스팅하기 위한 프로세스는 다음과 같습니다.

1. Azure 포털에서 새 앱 서비스 웹 앱을 만듭니다. 자세한 내용은 참조 [앱 서비스 환경에서 웹 앱을 만들](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/)합니다.
1. Azure 포털에서 웹 앱에 대 한 앱 설정 블레이드에서 열고 다음 설정을 추가 합니다.
    - `accountUrl` – 값 Cosmos DB 계정의 키 블레이드에서 Cosmos DB 계정 URL 이어야 합니다.
    - `accountKey` – 값 Cosmos DB 계정의 키 블레이드에서 Cosmos DB 마스터 키 (기본 또는 보조) 이어야 합니다.
    - `databaseId` – 값 Cosmos DB 데이터베이스의 이름 이어야 합니다.
    - `collectionId` – 값 Cosmos DB 컬렉션의 이름 이어야 합니다 (이 경우 `UserItems`).
    - `hostUrl` – 값의 응용 프로그램 서비스 계정 개요 블레이드에에서 웹 앱의 URL 이어야 합니다.

    다음 스크린샷은이 구성을 보여 줍니다.

    [![](authentication-images/azure-web-app-settings.png "앱 서비스 웹 앱 설정을")](authentication-images/azure-web-app-settings-large.png#lightbox "앱 서비스 웹 앱 설정")

1. Azure 앱 서비스 웹 앱으로 리소스 토큰 브로커 솔루션을 게시 합니다.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook 응용 프로그램 구성

인증을 수행 하는 Facebook 응용 프로그램을 만들기 위한 프로세스는 다음과 같습니다.

1. Facebook 응용 프로그램을 만듭니다. 자세한 내용은 참조 [등록 하 고 응용 프로그램 구성](https://developers.facebook.com/docs/apps/register) Facebook 개발자 센터에 있습니다.
1. Facebook 로그인 제품 응용 프로그램을 추가 합니다. 자세한 내용은 참조 [응용 프로그램 또는 웹 사이트를 Facebook 로그인 추가](https://developers.facebook.com/docs/facebook-login) Facebook 개발자 센터에 있습니다.
1. Facebook 로그인을 다음과 같이 구성 합니다.
  - 클라이언트 OAuth 로그인을 사용 하도록 설정 합니다.
  - 웹 OAuth 로그인 할 수 있도록 합니다.
  - 유효한 OAuth 리디렉션 URI는 앱 서비스 웹 응용 프로그램의 URI 사용 하 여 설정 `/.auth/login/facebook/callback` 추가 됩니다.

  다음 스크린샷은이 구성을 보여 줍니다.

  ![](authentication-images/facebook-oauth-settings.png "Facebook 로그인 OAuth 설정")

자세한 내용은 참조 [Facebook과 응용 프로그램 등록](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook)합니다.

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure 앱 서비스 인증 구성

앱 서비스 쉬운 인증 구성에 대 한 프로세스는 다음과 같습니다.

1. Azure 포털에서 앱 서비스 웹 앱으로 이동 합니다.
1. Azure 포털에서 열고 인증 / 권한 부여 블레이드 다음 구성을 수행:
  - 앱 서비스 인증을 설정 해야 합니다.
  - 요청이 인증 되지 않은 경우 수행할 동작을 설정 해야 **Facebook으로 로그인**합니다.

  다음 스크린샷은이 구성을 보여 줍니다.

  [![](authentication-images/app-service-authentication-settings.png "앱 서비스 웹 앱 인증 설정을")](authentication-images/app-service-authentication-settings-large.png#lightbox "앱 서비스 웹 앱 인증 설정")

앱 서비스 웹 앱 인증 흐름을 사용 하는 Facebook 응용 프로그램와 통신 하도록 구성도 해야 합니다. Facebook id 공급자를 선택 하 고를 입력 하 여이 작업을 수행할 수 있습니다는 **앱 ID** 및 **응용 프로그램 암호** Facebook 개발자 센터에서 Facebook 응용 프로그램 설정 값입니다. 자세한 내용은 참조 [응용 프로그램에 추가 Facebook 정보](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application)합니다.

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.Forms 응용 프로그램 구성

Xamarin.Forms 샘플 응용 프로그램을 구성 하기 위한 프로세스는 다음과 같습니다.

1. Xamarin.Forms 솔루션을 엽니다.
1. 열기 `Constants.cs` 다음과 같은 상수 값을 업데이트 합니다.
  - `EndpointUri` – 값 Cosmos DB 계정의 키 블레이드에서 Cosmos DB 계정 URL 이어야 합니다.
  - `DatabaseName` – 값 문서 데이터베이스의 이름 이어야 합니다.
  - `CollectionName` – 값 문서 데이터베이스 컬렉션의 이름 이어야 합니다 (이 경우 `UserItems`).
  - `ResourceTokenBrokerUrl` – 값의 응용 프로그램 서비스 계정 개요 블레이드에 리소스 토큰 브로커 웹 응용 프로그램의 URL 이어야 합니다.

## <a name="initiating-login"></a>로그인을 시작합니다.

다음 예제 코드에서와 같이 브라우저는 id 공급자 URL을 리디렉션하도록 Xamarin.Auth를 사용 하 여 로그인 프로세스를 시작 하는 샘플 응용 프로그램:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Azure 앱 서비스와 Facebook 로그인 페이지를 표시 하는 Facebook 간의 시작 되는 OAuth 인증 흐름을 프로그램이 합니다.

![](authentication-images/login.png "Facebook 로그인")

키를 눌러 로그인이 취소 될 수는 **취소** iOS 하거나 눌러 단추는 **다시** 동안 사용자 인증 되지 않은 경우 및 id 공급자 사용자 인터페이스는 Android에서 단추 화면에서 제거 합니다.

Xamarin.Auth에 대 한 자세한 내용은 참조 [Id 공급자를 통해 사용자 인증](~/xamarin-forms/data-cloud/authentication/oauth.md)합니다.

## <a name="obtaining-a-resource-token"></a>리소스 토큰을 가져오려면

성공적으로 인증 된 `WebRedirectAuthenticator.Completed` 이벤트 발생 합니다. 다음 코드 예제에서는이 이벤트를 처리 하는 방법을 보여 줍니다.

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

성공적인 인증의 결과 사용할 수 있는 액세스 토큰을 `AuthenticatorCompletedEventArgs.Account` 속성입니다. 액세스 토큰에서 추출 하 고 리소스 토큰 브로커에 GET 요청에 사용 되는 `resourcetoken` API입니다.

`resourcetoken` API Cosmos DB에서 리소스 토큰을 요청에 사용 되는 facebook 사용자 id를 요청 하 고 액세스 토큰을 사용 합니다. 문서 데이터베이스의 사용자에 대 한 유효한 사용 권한 문서가 이미 있는 경우를 검색 하 고 리소스 토큰을 포함 하는 JSON 문서 Xamarin.Forms 응용 프로그램에 반환 됩니다. 사용자에 대 한 유효한 사용 권한 문서가 존재 하지 않는 경우 사용자 및 권한 문서 데이터베이스에서 만들어지고 리소스 토큰은 권한 문서에서 추출 되 고 JSON 문서에서는 Xamarin.Forms 응용 프로그램에 반환 합니다.

> [!NOTE]
> 문서 데이터베이스 사용자는 문서 데이터베이스와 관련 된 리소스와 각 데이터베이스에는 0 개 이상의 사용자가 포함 될 수 있습니다. 문서 데이터베이스 사용 권한 문서 데이터베이스 사용자와 연결 된 리소스 이며 각 사용자는 0 개 이상의 사용 권한을 포함 될 수 있습니다. 사용 권한 리소스에 사용자 문서와 같은 리소스에 액세스 하려고 할 때 필요한 보안 토큰에 대 한 액세스를 제공 합니다.

경우는 `resourcetoken` API가 성공적으로 완료 된, HTTP 상태 코드 200 (정상) 응답으로 리소스 토큰을 포함 하는 JSON 문서와 함께 전송 됩니다. 다음 JSON 데이터에는 일반적인 성공 응답 메시지를 보여 줍니다.

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed` 이벤트 처리기에서 응답을 읽고는 `resourcetoken` API에서 리소스 토큰 및 사용자 id를 추출 합니다. 리소스 토큰은 다음에 대 한 인수로 전달 되는 `DocumentClient` 생성자 끝점, 자격 증명 및 연결 정책을 Cosmos DB에 액세스 하는 데 사용을 캡슐화 하 고 구성 하 고 Cosmos DB에 대 한 요청을 실행 하는 데 사용 됩니다. 리소스 토큰에 리소스에 직접 액세스 각 요청과 함께 전송 되 고 인증 된 사용자의 분할 된 컬렉션에 대 한 읽기/쓰기 액세스 부여 되어 있음을 나타냅니다.

## <a name="retrieving-documents"></a>문서를 검색합니다.

인증된 된 사용자에만 속해 있는 문서를 검색에 다음 코드 예제에서는 파티션 키로 사용자의 id를 포함 하며 나와 있는 문서 쿼리를 만들어 구현할 수 있습니다.

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

쿼리 비동기적으로 지정된 된 컬렉션에서 인증된 된 사용자에 속하는 문서를 모두 검색 하 고에 배치는 `List<TodoItem>` 디스플레이 대 한 컬렉션입니다.

`CreateDocumentQuery<T>` 메서드 지정는 `Uri` 문서에 대해 쿼리해야 하는 컬렉션을 나타내는 인수 및 `FeedOptions` 개체입니다. `FeedOptions` 개체 쿼리와 파티션 키로 사용자의 id에서 항목 개수에 제한 없이 반환할 수 있는지를 지정 합니다. 이렇게 하면 사용자의 분할 된 컬렉션의 문서에만 결과에 반환 됩니다.

> [!NOTE]
> 참고 리소스 토큰 브로커에 의해 생성 되는 권한 문서 Xamarin.Forms 응용 프로그램에서 만든 문서 동일한 문서 컬렉션에 저장 됩니다. 따라서 문서 쿼리를 포함 한 `Where` 문서 컬렉션에 대해 쿼리를 필터링 조건자를 적용 하는 절. 이 절을 사용 하면 권한 문서는 문서 컬렉션에서 반환 되지 않습니다.

문서 컬렉션에서 문서를 검색 하는 방법에 대 한 자세한 내용은 참조 [검색 문서 컬렉션 문서](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#document_query)합니다.

## <a name="inserting-documents"></a>문서를 삽입합니다.

문서 문서 컬렉션에 삽입 하기 전에 `TodoItem.UserId` 다음 코드 예제에서와 같이 속성이 파티션 키로 사용 되 고 값으로 업데이트 해야 합니다.

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

이렇게 하면 사용자의 분할 된 컬렉션에 문서를 삽입할 수 됩니다.

문서 문서 컬렉션에 삽입 하는 방법에 대 한 자세한 내용은 참조 [문서 문서 컬렉션에 삽입](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#inserting_document)합니다.

## <a name="deleting-documents"></a>문서 삭제

다음 코드 예제에서 보여 준로 분할 된 컬렉션에서 문서를 삭제 하는 경우 파티션 키 값을 지정 해야 합니다.

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

이렇게 하면 Cosmos DB가 있는 분할을 알고 있는 컬렉션에서 문서를 삭제 합니다.

문서 컬렉션에서 문서를 삭제 하는 방법에 대 한 자세한 내용은 참조 [문서 컬렉션에서 문서를 삭제](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#deleting_document)합니다.

## <a name="summary"></a>요약

이 문서는 사용자가 자신의 문서 데이터베이스 문서 Xamarin.Forms 응용 프로그램에만 액세스할 수 있도록 분할 된 컬렉션을 사용한 액세스 제어를 결합 하는 방법을 설명 합니다. 파티션 키로 사용자의 id를 지정 하면 분할 된 컬렉션을 해당 사용자에 대 한 문서에만 저장할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [Todo Azure Cosmos DB Auth (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)
- [Azure Cosmos DB 문서 데이터베이스 사용](~/xamarin-forms/data-cloud/cosmosdb/consuming.md)
- [Azure Cosmos DB 데이터에 대 한 액세스 보호](/azure/cosmos-db/secure-access-to-data/)
- [SQL API의 액세스 제어](/rest/api/documentdb/access-control-on-documentdb-resources/)합니다.
- [파티션 및 Azure Cosmos DB에 크기 조정 하는 방법](/azure/cosmos-db/partition-data/)
- [Azure DB Cosmos 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
