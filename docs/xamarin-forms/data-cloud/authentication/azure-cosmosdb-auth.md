---
title: Azure 코스모스 DB 문서 데이터베이스 및 Xamarin.Forms를 사용하여 사용자를 인증합니다.
description: 이 문서에서는 사용자가 Xamarin.Forms 응용 프로그램에서만 자신의 문서에 액세스할 수 있도록 액세스 제어를 Azure Cosmos DB 분할된 컬렉션과 결합하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6e79e647d64103b6d257de7233f488899bcaff40
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388605"
---
# <a name="authenticate-users-with-an-azure-cosmos-db-document-database-and-xamarinforms"></a>Azure 코스모스 DB 문서 데이터베이스 및 Xamarin.Forms를 사용하여 사용자를 인증합니다.

[![샘플](~/media/shared/download.png) 다운로드 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)

_Azure Cosmos DB 문서 데이터베이스는 여러 서버와 파티션에 걸쳐 무제한 저장소 및 처리량을 지원하면서 분할된 컬렉션을 지원합니다. 이 문서에서는 사용자가 Xamarin.Forms 응용 프로그램에서만 자신의 문서에 액세스할 수 있도록 액세스 제어를 분할된 컬렉션과 결합하는 방법을 설명합니다._

## <a name="overview"></a>개요

파티션 컬렉션을 만들 때 파티션 키를 지정해야 하며 동일한 파티션 키가 있는 문서는 동일한 파티션에 저장됩니다. 따라서 사용자의 ID를 파티션 키로 지정하면 해당 사용자에 대한 문서만 저장하는 분할된 컬렉션이 생성됩니다. 또한 사용자 및 항목 수가 증가함에 따라 Azure Cosmos DB 문서 데이터베이스가 확장됩니다.

모든 컬렉션에 액세스 권한을 부여해야 하며 SQL API 액세스 제어 모델은 두 가지 유형의 액세스 구문정의를 정의합니다.

- **마스터 키를** 사용하면 Cosmos DB 계정 내의 모든 리소스에 대한 전체 관리 액세스가 가능하며 Cosmos DB 계정이 생성될 때 만들어집니다.
- **리소스 토큰은** 데이터베이스 사용자와 컬렉션 또는 문서와 같은 특정 Cosmos DB 리소스에 대한 사용자 권한 간의 관계를 캡처합니다.

마스터 키를 노출하면 Cosmos DB 계정이 악의적이거나 부주의한 사용 가능성을 볼 수 있습니다. 그러나 Azure Cosmos DB 리소스 토큰은 클라이언트가 부여된 사용 권한에 따라 Azure Cosmos DB 계정에서 특정 리소스를 읽고 쓰고 삭제할 수 있도록 하는 안전한 메커니즘을 제공합니다.

모바일 응용 프로그램에 리소스 토큰을 요청, 생성 및 제공하는 일반적인 방법은 리소스 토큰 브로커를 사용하는 것입니다. 다음 다이어그램에서는 샘플 응용 프로그램이 리소스 토큰 브로커를 사용하여 문서 데이터베이스 데이터에 대한 액세스를 관리하는 방법에 대한 상위 개요를 보여 주며 있습니다.

![](azure-cosmosdb-auth-images/documentdb-authentication.png "Document Database Authentication Process")

리소스 토큰 브로커는 Cosmos DB 계정의 마스터 키를 보유한 Azure App Service에서 호스팅되는 중간 계층 웹 API 서비스입니다. 샘플 응용 프로그램은 리소스 토큰 브로커를 사용하여 다음과 같이 문서 데이터베이스 데이터에 대한 액세스를 관리합니다.

1. 로그인시 Xamarin.Forms 응용 프로그램은 Azure 앱 서비스에 문의하여 인증 흐름을 시작합니다.
1. Azure 앱 서비스는 Facebook에서 OAuth 인증 흐름을 수행합니다. 인증 흐름이 완료되면 Xamarin.Forms 응용 프로그램은 액세스 토큰을 받습니다.
1. Xamarin.Forms 응용 프로그램은 액세스 토큰을 사용하여 리소스 토큰 브로커에서 리소스 토큰을 요청합니다.
1. 리소스 토큰 브로커는 액세스 토큰을 사용하여 Facebook에서 사용자의 ID를 요청합니다. 그런 다음 사용자의 ID를 사용하여 Cosmos DB에서 리소스 토큰을 요청하는데 사용되며, 이 토큰은 인증된 사용자의 분할된 컬렉션에 대한 읽기/쓰기 액세스 권한을 부여하는 데 사용됩니다.
1. Xamarin.Forms 응용 프로그램은 리소스 토큰을 사용하여 리소스 토큰에 정의된 권한을 사용하여 Cosmos DB 리소스에 직접 액세스합니다.

> [!NOTE]
> 리소스 토큰이 만료되면 후속 문서 데이터베이스 요청은 401개의 무단 예외를 받게 됩니다. 이 시점에서 Xamarin.Forms 응용 프로그램은 ID를 다시 설정하고 새 리소스 토큰을 요청해야 합니다.

코스모스 DB 분할에 대한 자세한 내용은 [Azure Cosmos DB에서 분할 및 확장 하는 방법을](/azure/cosmos-db/partition-data/)참조하십시오. 코스모스 DB 액세스 제어에 대한 자세한 내용은 [SQL API에서](/rest/api/documentdb/access-control-on-documentdb-resources/) [Cosmos DB 데이터에 대한 액세스 및](/azure/cosmos-db/secure-access-to-data/) 액세스 제어 보안을 참조하십시오.

## <a name="setup"></a>설치 프로그램

리소스 토큰 브로커를 Xamarin.Forms 응용 프로그램에 통합하는 프로세스는 다음과 같습니다.

1. 액세스 제어를 사용하는 Cosmos DB 계정을 만듭니다. 자세한 내용은 [코스모스 DB 구성을](#cosmosdb_configuration)참조하십시오.
1. 리소스 토큰 브로커를 호스트하는 Azure 앱 서비스를 만듭니다. 자세한 내용은 [Azure 앱 서비스 구성](#app_service_configuration)을 참조하십시오.
1. 인증을 수행하기 위해 Facebook 앱을 만듭니다. 자세한 내용은 [Facebook 앱 구성을](#facebook_configuration)참조하십시오.
1. Facebook에서 간편한 인증을 수행하도록 Azure 앱 서비스를 구성합니다. 자세한 내용은 [Azure 앱 서비스 인증 구성을](#app_service_authentication_configuration)참조하십시오.
1. Azure 앱 서비스 및 Cosmos DB와 통신하도록 Xamarin.Forms 샘플 응용 프로그램을 구성합니다. 자세한 내용은 [Xamarin.Forms 응용 프로그램 구성](#forms_application_configuration)을 참조하십시오.

> [!NOTE]
> [Azure 구독이](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)없는 경우 시작하기 전에 [무료 계정을](https://aka.ms/azfree-docs-mobileapps) 만듭니다.

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure 코스모스 DB 구성

액세스 제어를 사용하는 Cosmos DB 계정을 만드는 프로세스는 다음과 같습니다.

1. 코스모스 DB 계정을 만듭니다. 자세한 내용은 [Azure Cosmos DB 계정 만들기를](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account)참조하십시오.
1. Cosmos DB 계정에서 `UserItems` `/userid`의 파티션 키를 지정하는 라는 새 컬렉션을 만듭니다.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure 앱 서비스 구성

Azure 앱 서비스에서 리소스 토큰 브로커를 호스팅하는 프로세스는 다음과 같습니다.

1. Azure 포털에서 새 앱 서비스 웹 앱을 만듭니다. 자세한 내용은 [앱 서비스 환경에서 웹 앱 만들기를](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/)참조하십시오.
1. Azure 포털에서 웹 앱에 대한 앱 설정 블레이드를 열고 다음 설정을 추가합니다.
    - `accountUrl`– 값은 코스모스 DB 계정의 키 블레이드에서 코스모스 DB 계정 URL이어야한다.
    - `accountKey`– 값은 코스모스 DB 계정의 키 블레이드에서 코스모스 DB 마스터 키 (기본 또는 보조)이어야한다.
    - `databaseId`– 값은 코스모스 DB 데이터베이스의 이름이어야 합니다.
    - `collectionId`– 값은 코스모스 DB 컬렉션의 이름이어야 합니다(이 `UserItems`경우).
    - `hostUrl`– 값은 앱 서비스 계정의 개요 블레이드에서 웹 앱의 URL이어야 한다.

    다음 스크린샷은 이 구성을 보여 줍니다.

    [![](azure-cosmosdb-auth-images/azure-web-app-settings.png "App Service Web App Settings")](azure-cosmosdb-auth-images/azure-web-app-settings-large.png#lightbox "App Service Web App Settings")

1. 리소스 토큰 브로커 솔루션을 Azure 앱 서비스 웹 앱에 게시합니다.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>페이스 북 앱 구성

인증을 수행하기 위해 Facebook 앱을 만드는 프로세스는 다음과 같습니다.

1. Facebook 앱을 만듭니다. 자세한 내용은 Facebook 개발자 센터에서 [앱 등록 및 구성을](https://developers.facebook.com/docs/apps/register) 참조하세요.
1. 앱에 Facebook 로그인 제품을 추가합니다. 자세한 내용은 Facebook 개발자 센터의 [앱 또는 웹사이트에 Facebook 로그인 추가를 참조하세요.](https://developers.facebook.com/docs/facebook-login)
1. 다음과 같이 페이스 북 로그인을 구성 :
   - 클라이언트 OAuth 로그인을 활성화합니다.
   - 웹 OAuth 로그인을 활성화합니다.
   - 유효한 OAuth 리디렉션URI를 `/.auth/login/facebook/callback` 추가된 앱 서비스 웹 앱의 URI로 설정합니다.

  다음 스크린샷은 이 구성을 보여 줍니다.

  ![](azure-cosmosdb-auth-images/facebook-oauth-settings.png "Facebook Login OAuth Settings")

자세한 내용은 [Facebook에 응용 프로그램 등록을](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook)참조하십시오.

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure 앱 서비스 인증 구성

앱 서비스 간편 인증을 구성하는 프로세스는 다음과 같습니다.

1. Azure 포털에서 앱 서비스 웹 앱으로 이동합니다.
1. Azure 포털에서 인증/권한 부여 블레이드를 열고 다음 구성을 수행합니다.
    - 앱 서비스 인증을 켜야 합니다.
    - 요청이 인증되지 않은 경우 취할 작업은 **Facebook에서 로그인으로**설정해야 합니다.

    다음 스크린샷은 이 구성을 보여 줍니다.

    [![](azure-cosmosdb-auth-images/app-service-authentication-settings.png "App Service Web App Authentication Settings")](azure-cosmosdb-auth-images/app-service-authentication-settings-large.png#lightbox "App Service Web App Authentication Settings")

인증 흐름을 활성화하도록 Facebook 앱과 통신하도록 앱 서비스 웹 앱도 구성해야 합니다. 이 작업은 Facebook ID 공급자를 선택하고 Facebook 개발자 센터의 Facebook 앱 설정에서 **앱 ID** 및 **앱 비밀** 값을 입력하면 수행할 수 있습니다. 자세한 내용은 [응용 프로그램에 Facebook 정보 추가를](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application)참조하십시오.

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>자마린.양식 응용 프로그램 구성

Xamarin.Forms 샘플 응용 프로그램을 구성하는 프로세스는 다음과 같습니다.

1. Xamarin.Forms 솔루션을 엽니다.
1. 다음 `Constants.cs` 상수의 값을 열고 업데이트합니다.
    - `EndpointUri`– 값은 코스모스 DB 계정의 키 블레이드에서 코스모스 DB 계정 URL이어야한다.
    - `DatabaseName`- 값은 문서 데이터베이스의 이름이어야 합니다.
    - `CollectionName`– 값은 문서 데이터베이스 컬렉션의 이름이어야 합니다(이 `UserItems`경우).
    - `ResourceTokenBrokerUrl`– 값은 앱 서비스 계정의 개요 블레이드에서 리소스 토큰 브로커 웹 앱의 URL이어야 합니다.

## <a name="initiating-login"></a>로그인 을 다시

샘플 응용 프로그램은 다음 예제 코드에서 설명한 대로 브라우저를 ID 공급자 URL로 리디렉션하여 로그인 프로세스를 시작합니다.

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

이로 인해 Azure 앱 서비스와 Facebook 간에 OAuth 인증 흐름이 시작되어 Facebook 로그인 페이지가 표시됩니다.

![](azure-cosmosdb-auth-images/login.png "Facebook Login")

로그인은 iOS의 **취소** 버튼을 누르거나 Android의 **뒤로** 버튼을 눌러 취소할 수 있으며, 이 경우 사용자는 인증되지 않은 상태로 유지되고 ID 공급자 사용자 인터페이스가 화면에서 제거됩니다.

## <a name="obtaining-a-resource-token"></a>리소스 토큰 얻기

인증에 `WebRedirectAuthenticator.Completed` 성공하면 이벤트가 발생합니다. 다음 코드 예제에서는 이 이벤트를 처리하는 것을 보여 줍니다.

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

성공적인 인증의 결과는 사용 가능한 `AuthenticatorCompletedEventArgs.Account` 속성인 액세스 토큰입니다. 액세스 토큰이 추출되어 리소스 토큰 브로커의 `resourcetoken` API에 대한 GET 요청에서 사용됩니다.

API는 `resourcetoken` 액세스 토큰을 사용하여 Facebook에서 사용자의 ID를 요청하며, 이 토큰은 Cosmos DB에서 리소스 토큰을 요청하는 데 사용됩니다. 문서 데이터베이스에 있는 사용자에 대한 유효한 사용 권한 문서가 이미 있는 경우 해당 문서가 검색되고 리소스 토큰이 포함된 JSON 문서가 Xamarin.Forms 응용 프로그램에 반환됩니다. 사용자에 대해 유효한 사용 권한 문서가 없으면 사용자 및 사용 권한이 문서 데이터베이스에 만들어지고 리소스 토큰이 사용 권한 문서에서 추출되어 JSON 문서의 Xamarin.Forms 응용 프로그램으로 반환됩니다.

> [!NOTE]
> 문서 데이터베이스 사용자는 문서 데이터베이스와 연결된 리소스이며 각 데이터베이스에는 0명 이상의 사용자가 포함될 수 있습니다. 문서 데이터베이스 권한은 문서 데이터베이스 사용자와 연결된 리소스이며 각 사용자에게 0개 이상의 사용 권한이 포함될 수 있습니다. 권한 리소스는 문서와 같은 리소스에 액세스하려고 할 때 사용자에게 필요한 보안 토큰에 대한 액세스를 제공합니다.

API가 `resourcetoken` 성공적으로 완료되면 리소스 토큰이 포함된 JSON 문서와 함께 응답에서 HTTP 상태 코드 200(확인)을 보냅니다. 다음 JSON 데이터는 일반적인 성공적인 응답 메시지를 보여 주며 다음과 같은

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

이벤트 `WebRedirectAuthenticator.Completed` 처리기는 `resourcetoken` API에서 응답을 읽고 리소스 토큰 및 사용자 ID를 추출합니다. 그런 다음 리소스 토큰은 `DocumentClient` Cosmos DB에 액세스하는 데 사용되는 끝점, 자격 증명 및 연결 정책을 캡슐화하고 Cosmos DB에 대한 요청을 구성하고 실행하는 데 사용되는 생성자에 대한 인수로 전달됩니다. 리소스 토큰은 리소스에 직접 액세스하기 위해 각 요청과 함께 전송되며 인증된 사용자의 분할된 컬렉션에 대한 읽기/쓰기 액세스가 허용된다는 것을 나타냅니다.

## <a name="retrieving-documents"></a>문서 검색

인증된 사용자만 속하는 문서를 검색하는 것은 사용자의 ID를 파티션 키로 포함하는 문서 쿼리를 만들어 수행할 수 있으며 다음 코드 예제에서 보여 줍니다.

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

쿼리는 지정된 컬렉션에서 인증된 사용자에 속한 모든 문서를 비동기적으로 검색하고 표시할 컬렉션에 `List<TodoItem>` 배치합니다.

메서드는 `CreateDocumentQuery<T>` 문서에 대해 `Uri` 쿼리해야 하는 컬렉션과 개체를 `FeedOptions` 나타내는 인수를 지정합니다. 개체는 `FeedOptions` 쿼리에서 무제한의 항목을 반환할 수 있으며 사용자의 ID를 파티션 키로 지정합니다. 이렇게 하면 사용자의 분할된 컬렉션에 있는 문서만 결과에서 반환됩니다.

> [!NOTE]
> 리소스 토큰 브로커가 만든 권한 문서는 Xamarin.Forms 응용 프로그램에서 만든 문서와 동일한 문서 컬렉션에 저장됩니다. 따라서 문서 쿼리에는 `Where` 문서 컬렉션에 대해 쿼리에 필터링 조건자 적용 하는 절이 포함 됩니다. 이 절은 권한 문서가 문서 컬렉션에서 반환되지 않도록 합니다.

문서 컬렉션에서 문서를 검색하는 데 대한 자세한 내용은 [문서 컬렉션 문서 검색을](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#document_query)참조하십시오.

## <a name="inserting-documents"></a>문서 삽입

문서를 문서 컬렉션에 삽입하기 전에 `TodoItem.UserId` 다음 코드 예제에서 설명한 대로 파티션 키로 사용되는 값으로 속성을 업데이트해야 합니다.

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

이렇게 하면 문서가 사용자의 분할된 컬렉션에 삽입됩니다.

문서를 문서 컬렉션에 삽입하는 데 대한 자세한 내용은 [문서 컬렉션에 문서 삽입](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#inserting_document)을 참조하십시오.

## <a name="deleting-documents"></a>문서 삭제

다음 코드 예제에서 설명한 것처럼 파티션 된 컬렉션에서 문서를 삭제할 때 파티션 키 값을 지정해야 합니다.

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

이렇게 하면 Cosmos DB에서 문서를 삭제할 분할된 컬렉션을 알 수 있습니다.

문서 컬렉션에서 문서를 삭제하는 것에 대한 자세한 내용은 [문서 컬렉션에서 문서 삭제를](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md#deleting_document)참조하십시오.

## <a name="summary"></a>요약

이 문서에서는 사용자가 Xamarin.Forms 응용 프로그램에서 자신의 문서 데이터베이스 문서에만 액세스할 수 있도록 액세스 제어를 분할된 컬렉션과 결합하는 방법을 설명했습니다. 파티션 키로 사용자의 ID를 지정하면 분할된 컬렉션에서 해당 사용자에 대한 문서만 저장할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [토도 Azure 코스모스 DB Auth (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdbauth)
- [Azure Cosmos DB 문서 데이터베이스 사용](~/xamarin-forms/data-cloud/azure-services/azure-cosmosdb.md)
- [Azure Cosmos DB 데이터에 대한 액세스 보호](/azure/cosmos-db/secure-access-to-data/)
- [SQL API의 액세스 제어.](/rest/api/documentdb/access-control-on-documentdb-resources/)
- [Azure Cosmos DB에서 분할 및 확장하는 방법](/azure/cosmos-db/partition-data/)
- [Azure 코스모스 DB 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
