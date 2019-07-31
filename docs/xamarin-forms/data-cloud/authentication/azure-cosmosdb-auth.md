---
title: Azure Cosmos DB 문서 데이터베이스 및 Xamarin.ios를 사용 하 여 사용자 인증
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 자신의 문서에만 액세스할 수 있도록 분할 하는 Azure Cosmos DB 컬렉션을 사용 하 여 액세스 제어를 결합 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 0067a9e576e695a308e4326955b540be2ff46f61
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657223"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Azure Cosmos DB 문서 데이터베이스 및 Xamarin.ios를 사용 하 여 사용자 인증

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Azure Cosmos DB 문서 데이터베이스 무제한 저장소 및 처리량을 지원 하면서 여러 서버 및 파티션에 걸칠 수 있는 분할 된 컬렉션을 지원 합니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 자신의 문서에만 액세스할 수 있도록 분할 된 컬렉션을 사용 하 여 액세스 제어를 결합 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

분할 된 컬렉션을 만들 때 파티션 키를 지정 해야 하 고 동일한 파티션에 동일한 파티션 키를 사용 하 여 문서를 저장할 합니다. 따라서 파티션 키로 사용자의 id를 지정 하는 사용자에 대 한 문서 저장만 하는 분할 된 컬렉션을 발생 합니다. 또한 이렇게 하면 Azure Cosmos DB 문서 데이터베이스는 사용자의 수로 확장 하 고 항목 증가 합니다.

모든 컬렉션에 대 한 액세스를 부여 합니다 및 SQL API 액세스 제어 모델 두 가지 유형의 액세스 생성을 정의 합니다.

- **마스터 키** Cosmos DB 계정 내 모든 리소스에 대 한 완전 한 관리 액세스를 사용 하도록 설정 하 고 Cosmos DB 계정을 만들 때 만들어집니다.
- **리소스 토큰** 컬렉션 또는 문서와 같은 특정 Cosmos DB 리소스에 대 한 사용자에 게 권한과 데이터베이스의 사용자 간의 관계를 포착 합니다.

마스터 키를 노출 악성 또는 부주의로 인해 사용 가능성을 Cosmos DB 계정을 엽니다. 그러나 Azure Cosmos DB 리소스 토큰 클라이언트가 읽기, 쓰기 및 부여 된 권한에 따라 Azure Cosmos DB 계정에 특정 리소스를 삭제 하도록 허용 하기 위한 안전 메커니즘을 제공 합니다.

요청 하는 일반적인 방법, 생성 및 모바일 응용 프로그램에 리소스 토큰을 제공 리소스 토큰 broker를 사용 하는 것입니다. 다음 다이어그램은 샘플 응용 프로그램 문서 데이터베이스 데이터에 대 한 액세스를 관리 하는 리소스 토큰 broker를 사용 하는 방법에 대 한 개략적인 개요를 보여줍니다.

![](azure-cosmosdb-auth-images/documentdb-authentication.png "문서 데이터베이스 인증 프로세스")

리소스 토큰 broker는 Cosmos DB 계정의 마스터 키를 소유 하는 Azure App Service에서 호스팅되는 중간 계층 웹 API 서비스를 합니다. 리소스 토큰 broker를 사용 하 여 다음과 같은 문서 데이터베이스 데이터에 대 한 액세스를 관리 하는 샘플 응용 프로그램:

1. Xamarin.Forms 응용 프로그램 로그인 인증 흐름을 시작 하려면 Azure App Service에 연결 합니다.
1. Azure App Service는 Facebook 사용 하 여 OAuth 인증 흐름을 수행합니다. 인증 흐름이 완료 된 후 Xamarin.Forms 응용 프로그램 액세스 토큰을 수신 합니다.
1. Xamarin.Forms 응용 프로그램 리소스 토큰 broker에서 리소스 토큰을 요청 하는 액세스 토큰을 사용 합니다.
1. 리소스 토큰 broker 액세스 토큰을 사용 하 여 Facebook에서 사용자의 id를 요청 합니다. 사용자의 id는 인증된 된 사용자의 분할 된 컬렉션에 대 한 읽기/쓰기 액세스 권한을 부여 하는 데 사용 되는 Cosmos db 리소스 토큰을 요청에 사용 됩니다.
1. Xamarin.Forms 응용 프로그램 리소스의 토큰으로 정의 된 권한을 사용 하 여 Cosmos DB 리소스에 직접 액세스할 리소스 토큰을 사용 합니다.

> [!NOTE]
> 리소스 토큰이 만료 되 면 후속 문서 데이터베이스 요청 수는 401 권한이 없음된 예외가 표시 됩니다. 이 시점에서 Xamarin.Forms 응용 프로그램 id를 다시 설정 하 고 새 리소스 토큰을 요청 해야 합니다.

Cosmos DB에서 분할 하는 방법에 대 한 자세한 내용은 참조 하십시오 [Azure Cosmos DB에서 분할 및 확장 하는 방법을](/azure/cosmos-db/partition-data/)합니다. Cosmos DB 액세스 제어에 대 한 자세한 내용은 참조 하세요. [Cosmos DB 데이터에 대 한 액세스 보호](/azure/cosmos-db/secure-access-to-data/) 하 고 [SQL API에서 액세스 제어](/rest/api/documentdb/access-control-on-documentdb-resources/)입니다.

## <a name="setup"></a>설정

Xamarin.Forms 응용 프로그램에 리소스 토큰 broker를 통합 하기 위한 프로세스는 다음과 같습니다.

1. 액세스 제어를 사용 하는 Cosmos DB 계정을 만듭니다. 자세한 내용은 [Cosmos DB 구성](#cosmosdb_configuration)합니다.
1. Azure App Service를 사용 하 여 리소스 토큰 broker 호스트를 만듭니다. 자세한 내용은 [Azure App Service 구성](#app_service_configuration)합니다.
1. 인증을 수행 하려면 Facebook 앱을 만듭니다. 자세한 내용은 [Facebook 앱 구성을](#facebook_configuration)합니다.
1. Facebook 사용 하 여 쉽게 인증 하는 데 Azure App Service를 구성 합니다. 자세한 내용은 [Azure App Service 인증 구성](#app_service_authentication_configuration)합니다.
1. Azure App Service 및 Cosmos DB와 통신 하는 Xamarin.Forms 샘플 응용 프로그램을 구성 합니다. 자세한 내용은 [Xamarin.Forms 응용 프로그램 구성](#forms_application_configuration)합니다.

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB 구성

액세스 제어를 사용 하는 Cosmos DB 계정을 만드는 프로세스는 다음과 같습니다.

1. Cosmos DB 계정을 만듭니다. 자세한 내용은 [Azure Cosmos DB 계정을 만들고](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)합니다.
1. Cosmos DB 계정에서 명명 된 새 컬렉션을 만듭니다 `UserItems`를 파티션 키를 지정 `/userid`합니다.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure App Service 구성

Azure App Service에서 리소스 토큰 broker를 호스트 하기 위한 프로세스는 다음과 같습니다.

1. Azure portal에서 새 App Service 웹 앱을 만듭니다. 자세한 내용은 [App Service Environment에서 웹 앱 만들기](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/)합니다.
1. Azure portal에서 웹 앱에 대 한 앱 설정 블레이드에서 열고 다음 설정을 추가 합니다.
    - `accountUrl` – 값은 Cosmos DB 계정의 키 블레이드에서 Cosmos DB 계정 URL 이어야 합니다.
    - `accountKey` – 값은 Cosmos DB 계정의 키 블레이드에서 Cosmos DB 마스터 키 (기본 또는 보조) 여야 합니다.
    - `databaseId` – 값에는 Cosmos DB 데이터베이스의 이름 이어야 합니다.
    - `collectionId` – 값에는 Cosmos DB 컬렉션의 이름 이어야 합니다 (이 예제의 경우 `UserItems`).
    - `hostUrl` – 값은 App Service 계정의 [개요] 블레이드에서 웹 앱의 URL 이어야 합니다.

    다음 스크린샷은 이러한 구성을 보여 줍니다.

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service 웹 앱 설정을")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service 웹 앱 설정")

1. Azure App Service 웹 앱에 리소스 토큰 broker 솔루션을 게시 합니다.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook 앱 구성

인증을 수행 하려면 Facebook 응용 프로그램을 만드는 프로세스는 다음과 같습니다.

1. Facebook 앱을 만듭니다. 자세한 내용은 [등록 및 앱 구성](https://developers.facebook.com/docs/apps/register) Facebook 개발자 센터에서.
1. 앱에 Facebook 로그인 제품을 추가 합니다. 자세한 내용은 [응용 프로그램 또는 웹 사이트를 Facebook 로그인 추가](https://developers.facebook.com/docs/facebook-login) Facebook 개발자 센터에서.
1. Facebook 로그인을 다음과 같이 구성 합니다.
   - 클라이언트 OAuth 로그인을 사용 하도록 설정 합니다.
   - 웹 OAuth 로그인을 사용 하도록 설정 합니다.
   - 유효한 OAuth 리디렉션 URI는 App Service 웹 앱의 URI 사용 하 여 설정 `/.auth/login/facebook/callback` 추가 합니다.

  다음 스크린샷은 이러한 구성을 보여 줍니다.

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook 로그인 OAuth 설정")

자세한 내용은 [Facebook 응용 프로그램 등록](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook)합니다.

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure App Service 인증 구성

App Service 간편한 인증을 구성 하기 위한 프로세스는 다음과 같습니다.

1. Azure Portal의 App Service 웹 앱으로 이동 합니다.
1. Azure Portal을 열고 인증 / 권한 부여 블레이드 다음 구성을 수행 합니다.
    - App Service 인증 켜야 합니다.
    - 요청이 인증 되지 않으면 수행할 동작을 설정 해야 **Facebook 로그인**합니다.

    다음 스크린샷은 이러한 구성을 보여 줍니다.

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service 웹 앱 인증 설정")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service 웹 앱 인증 설정")

App Service 웹 앱 인증 흐름을 사용 하도록 설정 하려면 Facebook 응용 프로그램을 사용 하 여 통신에 구성 되어야 합니다. Facebook id 공급자를 선택 하 고 입력 하 여이 작업을 수행할 수 있습니다는 **앱 ID** 하 고 **App Secret** Facebook 개발자 센터에서 Facebook 앱 설정 값입니다. 자세한 내용은 [응용 프로그램에 추가 Facebook 정보](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application)합니다.

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.Forms 응용 프로그램 구성

Xamarin.Forms 샘플 응용 프로그램을 구성 하기 위한 프로세스는 다음과 같습니다.

1. Xamarin.Forms 솔루션을 엽니다.
1. 열기 `Constants.cs` 다음 상수 중 값을 업데이트 합니다.
    - `EndpointUri` – 값은 Cosmos DB 계정의 키 블레이드에서 Cosmos DB 계정 URL 이어야 합니다.
    - `DatabaseName` – 값에는 문서 데이터베이스의 이름 이어야 합니다.
    - `CollectionName` – 값에는 문서 데이터베이스 컬렉션의 이름 이어야 합니다 (이 예제의 경우 `UserItems`).
    - `ResourceTokenBrokerUrl` – 값은 App Service 계정의 [개요] 블레이드에서 리소스 토큰 broker 웹 앱의 URL 이어야 합니다.

## <a name="initiating-login"></a>시작 로그인

다음 예제 코드 에서처럼는 id 공급자 URL을 브라우저를 리디렉션할 Xamarin.Auth를 사용 하 여 로그인 프로세스를 시작 하는 샘플 응용 프로그램:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

이렇게 하면 Azure App Service 및 Facebook 로그인 페이지를 표시 하는 Facebook 간에 시작 하는 OAuth 인증 흐름:

![](azure-cosmosdb-auth-images/login.png "Facebook 로그인")

키를 눌러 로그인을 취소할 수 있습니다 합니다 **취소** 키를 눌러 iOS에서 단추를 **다시** android에서 사용자 인증 되지 않은 남아 있는 경우에 id 공급자 사용자 인터페이스 이며 단추 화면에서 제거 합니다.

Xamarin.Auth에 대 한 자세한 내용은 참조 하세요. [Id 공급자를 사용 하 여 사용자 인증](~/xamarin-forms/data-cloud/authentication/oauth.md)합니다.

## <a name="obtaining-a-resource-token"></a>리소스 토큰 가져오기

인증이 성공 하면 다음을 `WebRedirectAuthenticator.Completed` 이벤트가 발생 합니다. 다음 코드 예제에서는이 이벤트를 처리 하는 방법을 보여 줍니다.

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

인증에 성공 하면 결과 사용할 수 있는 액세스 토큰을 `AuthenticatorCompletedEventArgs.Account` 속성입니다. 액세스 토큰을 추출 하 고 리소스 토큰 broker는 GET 요청에서 사용 되는 `resourcetoken` API.

`resourcetoken` API에서 Cosmos db에서 리소스 토큰을 요청 하는 데 사용 되는 Facebook의 사용자 id를 요청 하려면 액세스 토큰을 사용 합니다. 문서 데이터베이스에서 사용자에 대 한 유효한 사용 권한 문서 이미 있는 경우 검색 하 고 리소스 토큰을 포함 하는 JSON 문서는 Xamarin.Forms 응용 프로그램에 반환 됩니다. 사용자에 대 한 유효한 사용 권한 문서 존재 하지 않으면 권한과 사용자 문서 데이터베이스에서 만들어지고 리소스 토큰은 사용 권한 문서에서 추출 하 고 JSON 문서에서는 Xamarin.Forms 응용 프로그램에 반환 합니다.

> [!NOTE]
> 문서 데이터베이스 사용자는 문서 데이터베이스와 연결 된 리소스 및 각 데이터베이스는 0 개 이상의 사용자를 포함할 수 있습니다. 문서 데이터베이스 사용 권한 문서 데이터베이스 사용자와 연결 된 리소스 이며 각 사용자는 0 개 이상의 권한을 포함할 수 있습니다. 권한 리소스에 사용자는 문서와 같은 리소스에 액세스 하려고 할 때 필요한 보안 토큰에 대 한 액세스를 제공 합니다.

경우는 `resourcetoken` API가 성공적으로 완료, HTTP 상태 코드 200 (정상) 응답에서 리소스 토큰을 포함 하는 JSON 문서와 함께 전송 됩니다. 다음 JSON 데이터는 일반적으로 성공적인 응답 메시지가 표시 됩니다.

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed` 이벤트 처리기에서 응답을 읽는 `resourcetoken` API 리소스 토큰 및 사용자 id를 추출 합니다. 리소스 토큰은 다음에 인수로 전달 된 `DocumentClient` 끝점, 자격 증명 및 연결 정책 Cosmos DB에 액세스 하는 데 사용을 캡슐화 하 고 구성 및 Cosmos DB에 대 한 요청을 실행 하는 데 사용 되는 생성자입니다. 리소스 토큰을 직접 리소스에 액세스 하려면 각 요청과 함께 전송 되 고 인증 된 사용자의 분할 된 컬렉션에 대 한 읽기/쓰기 액세스 부여 되어 있음을 나타냅니다.

## <a name="retrieving-documents"></a>문서 검색

인증된 된 사용자에만 속하는 문서를 검색 하는 다음 코드 예제에서 파티션 키로 사용자의 id를 포함 하 고 설명 하는 문서 쿼리를 만들어 수행할 수 있습니다.

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

쿼리에서 비동기적으로 모든 지정된 된 컬렉션에서 인증된 된 사용자에 속하는 문서를 검색 하 고 배치에 `List<TodoItem>` 표시에 대 한 컬렉션입니다.

`CreateDocumentQuery<T>` 메서드를 `Uri` 문서에 대해 쿼리해야 하는 컬렉션을 나타내는 인수 및 `FeedOptions` 개체. `FeedOptions` 개체 항목 개수에 제한 없이 쿼리 및 파티션 키로 사용자의 id에서 반환할 수 있는지 지정 합니다. 이렇게 하면 사용자의 분할 된 컬렉션의 문서에만 결과에 반환 되도록 합니다.

> [!NOTE]
> 권한 문서, 리소스 토큰 broker에 의해 만들어진 Xamarin.Forms 응용 프로그램에서 만든 문서 동일한 문서 컬렉션에 저장 됩니다 하는 참고 합니다. 따라서 문서 쿼리를 포함 한 `Where` 문서 컬렉션에 대해 쿼리를 필터링 하는 조건자를 적용 하는 절. 이 절은 사용 권한 문서는 문서 컬렉션에서 반환 되지 않습니다 확인 합니다.

문서 컬렉션에서 문서를 검색 하는 방법에 대 한 자세한 내용은 참조 하세요. [검색 문서 컬렉션 문서](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#document_query)합니다.

## <a name="inserting-documents"></a>문서를 삽입합니다.

문서 컬렉션에 문서를 삽입 하기 전에 `TodoItem.UserId` 다음 코드 예제에 설명 된 대로 속성을 파티션 키로 사용 하는 값을 사용 하 여 업데이트 해야 합니다.

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

이렇게 하면 문서는 사용자의 분할 된 컬렉션에 삽입 됩니다.

문서 컬렉션에 문서를 삽입 하는 방법에 대 한 자세한 내용은 참조 하세요. [문서 컬렉션에 문서를 삽입](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting_document)합니다.

## <a name="deleting-documents"></a>문서 삭제

다음 코드 예제에서 설명한로 분할 된 컬렉션에서 문서를 삭제 하는 경우 파티션 키 값을 지정 해야 합니다.

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

이렇게 하면 Cosmos DB는 분할을 알고 있는 컬렉션에서 문서를 삭제 합니다.

문서 컬렉션에서 문서를 삭제 하는 방법에 대 한 자세한 내용은 참조 하세요. [문서 컬렉션에서 문서를 삭제 하면](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting_document)합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms 응용 프로그램에서 해당 문서 데이터베이스 문서에만 액세스할 수 있도록 분할 된 컬렉션을 사용 하 여 액세스 제어를 결합 하는 방법을 설명 합니다. 사용자의 id를 파티션 키로 지정 하면 분할 된 컬렉션을 해당 사용자에 대 한 문서에만 저장할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [Todo Azure Cosmos DB 인증 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Azure Cosmos DB 문서 데이터베이스 사용](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Azure Cosmos DB 데이터에 대 한 액세스 보호](/azure/cosmos-db/secure-access-to-data/)
- [SQL API에서 액세스 제어](/rest/api/documentdb/access-control-on-documentdb-resources/)입니다.
- [Azure Cosmos DB에서 분할 및 확장 하는 방법](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
