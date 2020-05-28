---
title: Azure Cosmos DB 문서 데이터베이스를 사용 하 여 사용자 인증Xamarin.Forms
description: 이 문서에서는 사용자가 응용 프로그램에서 자신의 문서에만 액세스할 수 있도록 분할 된 컬렉션 Azure Cosmos DB 액세스 제어를 결합 하는 방법을 설명 합니다 Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b0322db5ebcc70347bf35157e3dc7c057e58cf18
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136099"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Azure Cosmos DB 문서 데이터베이스를 사용 하 여 사용자 인증Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Azure Cosmos DB 문서 데이터베이스는 분할 된 컬렉션을 지원 하며,이는 여러 서버 및 파티션에 걸쳐 있을 수 있으며 저장소와 처리량을 무제한으로 지원 합니다. 이 문서에서는 사용자가 응용 프로그램에서 자신의 문서에만 액세스할 수 있도록 분할 된 컬렉션과 액세스 제어를 결합 하는 방법을 설명 합니다 Xamarin.Forms ._

## <a name="overview"></a>개요

분할 된 컬렉션을 만들 때 파티션 키를 지정 해야 하며, 동일한 파티션 키가 있는 문서는 동일한 파티션에 저장 됩니다. 따라서 사용자 id를 파티션 키로 지정 하면 해당 사용자에 대 한 문서만 저장 하는 분할 된 컬렉션이 생성 됩니다. 이렇게 하면 사용자 및 항목 수가 증가 하는 Azure Cosmos DB 문서 데이터베이스도 확장 됩니다.

모든 컬렉션에 대 한 액세스 권한을 부여 해야 하며 SQL API 액세스 제어 모델은 두 가지 유형의 액세스 구문을 정의 합니다.

- **마스터 키** 를 사용 하면 Cosmos DB 계정 내의 모든 리소스에 대 한 모든 관리 권한을 사용할 수 있으며 Cosmos DB 계정을 만들 때 만들어집니다.
- **리소스 토큰** 은 데이터베이스 사용자와 컬렉션 또는 문서와 같은 특정 Cosmos DB 리소스에 대 한 사용자의 권한 간의 관계를 캡처합니다.

마스터 키를 노출 하면 악성 또는 부주의로 사용 가능성에 대 한 Cosmos DB 계정이 열립니다. 그러나 Azure Cosmos DB 리소스 토큰은 부여 된 사용 권한에 따라 클라이언트에서 Azure Cosmos DB 계정의 특정 리소스를 읽고, 쓰고, 삭제할 수 있도록 하는 안전 메커니즘을 제공 합니다.

모바일 응용 프로그램에 대 한 리소스 토큰을 요청, 생성 및 전달 하는 일반적인 방법은 리소스 토큰 브로커를 사용 하는 것입니다. 다음 다이어그램은 샘플 응용 프로그램에서 리소스 토큰 브로커를 사용 하 여 문서 데이터베이스 데이터에 대 한 액세스를 관리 하는 방법에 대 한 개략적인 개요를 보여 줍니다.

![](azure-cosmosdb-auth-images/documentdb-authentication.png "Document Database Authentication Process")

리소스 토큰 브로커는 Cosmos DB 계정의 마스터 키를 소유 하는 Azure App Service에서 호스트 되는 중간 계층 웹 API 서비스입니다. 샘플 응용 프로그램은 리소스 토큰 브로커를 사용 하 여 다음과 같이 문서 데이터베이스 데이터에 대 한 액세스를 관리 합니다.

1. 로그인 할 때 Xamarin.Forms 응용 프로그램은 Azure App Service 연결 하 여 인증 흐름을 시작 합니다.
1. Azure App Service는 Facebook을 사용 하 여 OAuth 인증 흐름을 수행 합니다. 인증 흐름이 완료 되 면 Xamarin.Forms 응용 프로그램은 액세스 토큰을 수신 합니다.
1. Xamarin.Forms응용 프로그램은 액세스 토큰을 사용 하 여 리소스 토큰 브로커에서 리소스 토큰을 요청 합니다.
1. 리소스 토큰 브로커는 액세스 토큰을 사용 하 여 Facebook의 사용자 id를 요청 합니다. 사용자의 id는 인증 된 사용자의 분할 된 컬렉션에 대 한 읽기/쓰기 액세스 권한을 부여 하는 데 사용 되는 Cosmos DB에서 리소스 토큰을 요청 하는 데 사용 됩니다.
1. 응용 프로그램은 리소스 토큰을 사용 하 여 리소스 Xamarin.Forms 토큰으로 정의 된 사용 권한으로 Cosmos DB 리소스에 직접 액세스 합니다.

> [!NOTE]
> 리소스 토큰이 만료 되 면 이후의 문서 데이터베이스 요청에서 401 권한 없는 예외를 수신 합니다. 이 시점에서 Xamarin.Forms 응용 프로그램은 id를 다시 설정 하 고 새 리소스 토큰을 요청 해야 합니다.

Cosmos DB 분할에 대 한 자세한 내용은 [Azure Cosmos DB을 분할 하 고 크기를 조정 하는 방법](/azure/cosmos-db/partition-data/)을 참조 하세요. Cosmos DB access control에 대 한 자세한 내용은 [SQL API에서](/rest/api/documentdb/access-control-on-documentdb-resources/)Cosmos DB 데이터 및 access control에 대 한 [액세스 보안](/azure/cosmos-db/secure-access-to-data/) 을 참조 하세요.

## <a name="setup"></a>설치 프로그램

리소스 토큰 브로커를 응용 프로그램에 통합 하는 프로세스는 다음과 같습니다 Xamarin.Forms .

1. 액세스 제어를 사용할 Cosmos DB 계정을 만듭니다. 자세한 내용은 [Cosmos DB 구성](#cosmosdb_configuration)을 참조 하세요.
1. 리소스 토큰 브로커를 호스트 하는 Azure App Service을 만듭니다. 자세한 내용은 [Azure App Service 구성](#app_service_configuration)을 참조 하세요.
1. 인증을 수행할 Facebook 앱을 만듭니다. 자세한 내용은 [Facebook 앱 구성](#facebook_configuration)을 참조 하세요.
1. Facebook을 사용 하 여 간편한 인증을 수행 하도록 Azure App Service를 구성 합니다. 자세한 내용은 [Azure App Service 인증 구성](#app_service_authentication_configuration)을 참조 하세요.
1. Xamarin.FormsAzure App Service 및 Cosmos DB와 통신 하도록 샘플 응용 프로그램을 구성 합니다. 자세한 내용은 [ Xamarin.Forms 응용 프로그램 구성](#forms_application_configuration)을 참조 하세요.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다.

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB 구성

액세스 제어를 사용 하는 Cosmos DB 계정을 만드는 프로세스는 다음과 같습니다.

1. Cosmos DB 계정을 만듭니다. 자세한 내용은 [Azure Cosmos DB 계정 만들기](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)를 참조 하세요.
1. Cosmos DB 계정에서의 파티션 키를 지정 하 여 라는 새 컬렉션을 만듭니다 `UserItems` `/userid` .

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure App Service 구성

Azure App Service에서 리소스 토큰 브로커를 호스트 하는 프로세스는 다음과 같습니다.

1. Azure Portal에서 새 App Service 웹 앱을 만듭니다. 자세한 내용은 [App Service Environment에서 웹 앱 만들기](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/)를 참조 하세요.
1. Azure Portal에서 웹 앱에 대 한 앱 설정 블레이드를 열고 다음 설정을 추가 합니다.
    - `accountUrl`– 값은 Cosmos DB 계정의 키 블레이드에서 Cosmos DB 계정 URL 이어야 합니다.
    - `accountKey`– 값은 Cosmos DB 계정의 키 블레이드에서 Cosmos DB 마스터 키 (주 또는 보조) 여야 합니다.
    - `databaseId`– 값은 Cosmos DB 데이터베이스의 이름 이어야 합니다.
    - `collectionId`– 값은 Cosmos DB 컬렉션의 이름 (이 경우) 이어야 합니다 `UserItems` .
    - `hostUrl`– 값은 App Service 계정의 개요 블레이드에서 웹 앱의 URL 이어야 합니다.

    다음 스크린샷은이 구성을 보여 줍니다.

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service Web App Settings")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service Web App Settings")

1. 리소스 토큰 브로커 솔루션을 Azure App Service 웹 앱에 게시 합니다.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook 앱 구성

Facebook 앱을 만들어 인증을 수행 하는 프로세스는 다음과 같습니다.

1. Facebook 앱을 만듭니다. 자세한 내용은 Facebook 개발자 센터에서 [앱 등록 및 구성](https://developers.facebook.com/docs/apps/register) 을 참조 하세요.
1. 앱에 Facebook 로그인 제품을 추가 합니다. 자세한 내용은 Facebook 개발자 센터에서 [앱 또는 웹 사이트에 Facebook 로그인 추가](https://developers.facebook.com/docs/facebook-login) 를 참조 하세요.
1. Facebook 로그인을 다음과 같이 구성 합니다.
   - 클라이언트 OAuth 로그인을 사용 하도록 설정 합니다.
   - 웹 OAuth 로그인을 사용 하도록 설정 합니다.
   - 유효한 OAuth 리디렉션 URI를 추가 된 App Service 웹 앱의 URI로 설정 합니다 `/.auth/login/facebook/callback` .

  다음 스크린샷은이 구성을 보여 줍니다.

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook Login OAuth Settings")

자세한 내용은 [Facebook을 사용 하 여 응용 프로그램 등록](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook)을 참조 하세요.

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure App Service 인증 구성

App Service 간편한 인증을 구성 하는 프로세스는 다음과 같습니다.

1. Azure Portal에서 App Service 웹 앱으로 이동 합니다.
1. Azure Portal에서 인증/권한 부여 블레이드를 열고 다음 구성을 수행 합니다.
    - App Service 인증을 설정 해야 합니다.
    - 요청이 인증 되지 않은 경우 수행할 작업은 **Facebook을 사용 하 여 로그인**하도록 설정 되어야 합니다.

    다음 스크린샷은이 구성을 보여 줍니다.

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service Web App Authentication Settings")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service Web App Authentication Settings")

또한 App Service 웹 앱은 Facebook 앱과 통신 하 여 인증 흐름을 사용 하도록 구성 해야 합니다. Facebook id 공급자를 선택 하 고 facebook 개발자 센터의 Facebook 앱 설정에서 **앱 id** 및 **앱 암호** 값을 입력 하 여이를 수행할 수 있습니다. 자세한 내용은 [응용 프로그램에 Facebook 정보 추가](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application)를 참조 하세요.

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.Forms응용 프로그램 구성

예제 응용 프로그램을 구성 하는 프로세스는 Xamarin.Forms 다음과 같습니다.

1. 솔루션을 엽니다 Xamarin.Forms .
1. `Constants.cs`을 열고 다음 상수의 값을 업데이트 합니다.
    - `EndpointUri`– 값은 Cosmos DB 계정의 키 블레이드에서 Cosmos DB 계정 URL 이어야 합니다.
    - `DatabaseName`–이 값은 문서 데이터베이스의 이름 이어야 합니다.
    - `CollectionName`–이 값은 문서 데이터베이스 컬렉션의 이름 (이 경우) 이어야 합니다 `UserItems` .
    - `ResourceTokenBrokerUrl`– 값은 App Service 계정의 개요 블레이드에서 리소스 토큰 브로커 웹 앱의 URL 이어야 합니다.

## <a name="initiating-login"></a>로그인 시작 중

예제 응용 프로그램은 다음 예제 코드에 표시 된 것 처럼 브라우저를 id 공급자 URL로 리디렉션하여 로그인 프로세스를 시작 합니다.

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

이로 인해 Azure App Service와 Facebook 사이에서 OAuth 인증 흐름이 시작 되어 Facebook 로그인 페이지가 표시 됩니다.

![](azure-cosmosdb-auth-images/login.png "Facebook Login")

IOS에서 **취소** 단추를 누르거나 Android에서 **뒤로** 단추를 눌러 로그인을 취소할 수 있습니다 .이 경우 사용자가 인증 되지 않은 상태로 유지 되 고 id 공급자 사용자 인터페이스가 화면에서 제거 됩니다.

## <a name="obtaining-a-resource-token"></a>리소스 토큰 가져오기

성공적인 인증 후에 `WebRedirectAuthenticator.Completed` 이벤트가 발생 합니다. 다음 코드 예제에서는이 이벤트를 처리 하는 방법을 보여 줍니다.

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

성공한 인증의 결과는 사용 가능한 속성인 액세스 토큰입니다 `AuthenticatorCompletedEventArgs.Account` . 액세스 토큰은 추출 되어 리소스 토큰 브로커 API에 대 한 GET 요청에 사용 됩니다 `resourcetoken` .

`resourcetoken`API는 액세스 토큰을 사용 하 여 Facebook의 사용자 id를 요청 합니다. 그러면이 토큰은 Cosmos DB에서 리소스 토큰을 요청 하는 데 사용 됩니다. 문서 데이터베이스에 사용자에 대 한 올바른 사용 권한 문서가 이미 있는 경우 검색 되 고 리소스 토큰이 포함 된 JSON 문서가 응용 프로그램에 반환 됩니다 Xamarin.Forms . 사용자에 대 한 올바른 사용 권한 문서가 없는 경우 문서 데이터베이스에서 사용자 및 권한이 만들어지고 리소스 토큰은 사용 권한 문서에서 추출 되어 Xamarin.Forms JSON 문서의 응용 프로그램으로 반환 됩니다.

> [!NOTE]
> 문서 데이터베이스 사용자는 문서 데이터베이스와 연결 된 리소스 이며, 각 데이터베이스에는 0 개 이상의 사용자가 포함 될 수 있습니다. 문서 데이터베이스 권한은 문서 데이터베이스 사용자와 연결 된 리소스 이며, 각 사용자는 0 개 이상의 사용 권한을 포함할 수 있습니다. 권한 리소스는 문서와 같은 리소스에 액세스 하려고 할 때 사용자가 요구 하는 보안 토큰에 대 한 액세스를 제공 합니다.

API가 `resourcetoken` 성공적으로 완료 되 면 리소스 토큰을 포함 하는 JSON 문서와 함께 HTTP 상태 코드 200 (OK)이 응답에 전송 됩니다. 다음 JSON 데이터는 일반적인 성공적인 응답 메시지를 표시 합니다.

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed`이벤트 처리기는 API에서 응답을 읽고 `resourcetoken` 리소스 토큰 및 사용자 id를 추출 합니다. 그런 다음 리소스 토큰은 `DocumentClient` Cosmos DB 액세스 하는 데 사용 되는 끝점, 자격 증명 및 연결 정책을 캡슐화 하는 생성자에 인수로 전달 되 고 Cosmos DB에 대해 요청을 구성 하 고 실행 하는 데 사용 됩니다. 리소스 토큰은 리소스에 직접 액세스 하는 각 요청과 함께 전송 되며, 인증 된 사용자의 분할 된 컬렉션에 대 한 읽기/쓰기 액세스 권한이 부여 됨을 나타냅니다.

## <a name="retrieving-documents"></a>문서 검색

사용자 id를 파티션 키로 포함 하는 문서 쿼리를 작성 하 여 인증 된 사용자 에게만 속하는 문서를 검색할 수 있으며, 다음 코드 예제에서이에 대해 보여 줍니다.

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

이 쿼리는 지정 된 컬렉션에서 인증 된 사용자에 속하는 모든 문서를 비동기적으로 검색 하 여 `List<TodoItem>` 표시를 위해 컬렉션에 배치 합니다.

`CreateDocumentQuery<T>`메서드는 `Uri` 문서 및 개체에 대해 쿼리해야 하는 컬렉션을 나타내는 인수를 지정 합니다 `FeedOptions` . `FeedOptions`개체는 쿼리에 의해 무제한 개수의 항목을 반환할 수 있으며, 사용자의 id를 파티션 키로 지정 합니다. 이렇게 하면 사용자의 분할 된 컬렉션에 있는 문서만 결과에 반환 됩니다.

> [!NOTE]
> 리소스 토큰 브로커에 의해 생성 된 사용 권한 문서는 응용 프로그램에서 만든 문서와 동일한 문서 컬렉션에 저장 됩니다 Xamarin.Forms . 따라서 문서 쿼리에는 `Where` 문서 컬렉션에 대해 쿼리에 필터링 조건자를 적용 하는 절이 포함 되어 있습니다. 이 절을 사용 하면 문서 컬렉션에서 사용 권한 문서가 반환 되지 않습니다.

문서 컬렉션에서 문서를 검색 하는 방법에 대 한 자세한 내용은 [문서 컬렉션 문서 검색](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#document_query)을 참조 하세요.

## <a name="inserting-documents"></a>문서 삽입

문서 컬렉션에 문서를 삽입 하기 전에 `TodoItem.UserId` 다음 코드 예제에서 보여 주는 것 처럼 파티션 키로 사용 되는 값으로 속성을 업데이트 해야 합니다.

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

이렇게 하면 문서가 사용자의 분할 된 컬렉션에 삽입 됩니다.

문서 컬렉션에 문서를 삽입 하는 방법에 대 한 자세한 내용은 문서 [컬렉션에 문서 삽입](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting_document)을 참조 하세요.

## <a name="deleting-documents"></a>문서 삭제

분할 된 컬렉션에서 문서를 삭제할 때는 다음 코드 예제에서 설명한 것 처럼 파티션 키 값을 지정 해야 합니다.

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

이렇게 하면 Cosmos DB에서 문서를 삭제할 분할 된 컬렉션을 알 수 있습니다.

문서 컬렉션에서 문서를 삭제 하는 방법에 대 한 자세한 내용은 [문서 컬렉션에서 문서 삭제](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting_document)를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 사용자가 응용 프로그램에서 자체 문서 데이터베이스 문서에만 액세스할 수 있도록 액세스 제어를 분할 된 컬렉션과 결합 하는 방법을 설명 Xamarin.Forms 했습니다. 사용자 id를 파티션 키로 지정 하면 분할 된 컬렉션이 해당 사용자에 대 한 문서만 저장할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Todo Azure Cosmos DB Auth (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Azure Cosmos DB 문서 데이터베이스 사용](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Azure Cosmos DB 데이터에 대한 액세스 보호](/azure/cosmos-db/secure-access-to-data/)
- [SQL API의 액세스 제어](/rest/api/documentdb/access-control-on-documentdb-resources/)
- [Azure Cosmos DB에서 분할 및 확장하는 방법](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
