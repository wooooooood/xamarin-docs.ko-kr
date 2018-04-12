---
title: Amazon SimpleDB 서비스 사용
description: Amazon SimpleDB 저장 하 고 Amazon의 클라우드의 데이터를에서 쿼리 하는 기능을 제공 하는 웹 서비스입니다. 이 문서에서는 쿼리를 만들고, 바꾸기 SimpleDB 서비스에 저장 된 데이터를 삭제 하려면 AWS SDK for.NET을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 823819AA-15F9-4144-B355-78A10AD37513
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 1602319dbf5a5d00ac5de75f2d438b9aea692699
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-an-amazon-simpledb-service"></a>Amazon SimpleDB 서비스 사용

_Amazon SimpleDB 저장 하 고 Amazon의 클라우드의 데이터를에서 쿼리 하는 기능을 제공 하는 웹 서비스입니다. 이 문서에서는 쿼리를 만들고, 바꾸기 SimpleDB 서비스에 저장 된 데이터를 삭제 하려면 AWS SDK for.NET을 사용 하는 방법을 설명 합니다._

SimpleDB 서비스는 REST 서비스의 소비자에 게 친숙 한 요청 및 응답 모델을 사용합니다. 작업은 데이터를 포함할 수 있는 요청을 보내 SimpleDB 서비스에서 호출 됩니다. 요청을 처리 한 후 SimpleDB 서비스는 모든 결과 포함 하는 응답을 반환 합니다. SimpleDB 서비스 프로그래밍 방식으로 만들어야 하며 만들 수는 [AWS 콘솔](https://aws.amazon.com)합니다. 그러나 AWS 계정이 만들고 모든 Amazon 웹 서비스에 액세스 하는 데 필요한 됩니다.

SimpleDB 서비스에서 데이터 도메인에 있는 데이터를 배치 될 수 있으며 데이터에 대해 쿼리가 실행으로 구성 됩니다. 도메인 이름-값 쌍 특성을 기준으로 설명 하는 항목으로 구성 됩니다. 도메인 특성 열 및 행과 비슷한 되 고 항목 유사한 것으로 테이블의 경우 유사한 것으로 생각할 수 있습니다. SimpleDB 데이터 모델에 대 한 자세한 내용은 참조 [데이터 모델](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/DataModel.html) Amazon의 웹 사이트에서.

샘플 응용 프로그램을 함께 제공 되는 추가 정보 파일에 필요한 Amazon 서비스 설정에 대 한 지침을 찾을 수 있습니다. 샘플 응용 프로그램을 실행 다음 스크린샷에 표시 된 것 처럼 SimpleDB 서비스에 대 한 액세스 권한을 부여 하는 Amazon Cognito identity 풀에 연결할 합니다.

![](aws-images/portal.png "샘플 응용 프로그램")

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 적용 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 AT 옵트인 수 있습니다는 `HTTPS` 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

## <a name="consuming-a-simpledb-service"></a>SimpleDB 서비스 사용

Amazon Cognito Identity 응용 프로그램에 하드 코딩 AWS 자격 증명 없이 응용 프로그램에서 호출할 SimpleDB 같은 AWS 서비스 수 있습니다. 대신에 고유한 id 풀을 만듭니다는 [Amazon Cognito 콘솔](https://console.aws.amazon.com/cognito/home)합니다. Identity 풀 id에 액세스할 수 있는 SimpleDB 등의 리소스를 지정 하려면 역할을 사용 하는 id를 포함 합니다.

[AWS SDK for.NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22) 제공는 `CognitoAWSCredentials` 및 `AmazonSimpleDBClient` 다음 코드 예제에 나와 있는 것 처럼 SimpleDB 서비스에 액세스 하는 Xamarin.Forms 응용 프로그램에서 사용 되는 클래스:

```csharp
AmazonSimpleDBClient client;
...

public SimpleDBStorage ()
{
  var credentials = new CognitoAWSCredentials (
                      Constants.CognitoIdentityPoolId,
                      RegionEndpoint.USEast1);
  var config = new AmazonSimpleDBConfig ();
  config.RegionEndpoint = RegionEndpoint.USWest2;
  client = new AmazonSimpleDBClient (credentials, config);
  ...
}
```

새 인스턴스는 `CognitoAWSCredentials` 클래스의 고유한 id 풀 id Cognito Id 계정의 영역을 제공 하 여 만들어집니다. 작성 시점에 Cognito Id는만 사용할 수 있는 USEast1 및 EUWest1 영역. 그러나 해당 영역 외부 Amazon 서비스와 통신할 수 있습니다.

때는 `AmazonSimpleDBClient` 인스턴스를 만들는 `CognitoAWSCredentials` 인스턴스에 제공 해야 합니다., 함께 `AmazonSimpleDBConfig` SimpleDB 서비스가 있는 지리적 위치 영역을 지정 하는 인스턴스가 있습니다. `CognitoAWSCredentials` 인스턴스를 사용 하면 액세스는 SimpleDB 서비스 identity 풀 생성 된, 응용 프로그램에 AWS 액세스 키 및 비밀 키를 포함할 필요가 없도록 하는 동안 AWS 계정과 연결 된 것입니다.

호출 하 여 만든 SimpleDB 서비스 도메인에서 `AmazonSimpleDBClient.CreateDomainAsync` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

```csharp
string tableName = "Todo";
...

async Task CreateDomain ()
{
  ...
  await client.CreateDomainAsync (new CreateDomainRequest { DomainName = tableName });
  ...
}
```

`CreateDomainAsync` 메서드를 사용 하려면 한 `CreateDomainRequest` 인스턴스를 매개 변수로 합니다. `CreateDomainRequest` 인스턴스를 초기화 하는 `DomainName` 속성 도메인을 식별 하는 데 사용할 값입니다. 도메인을 만들려면이 값은 AWS 계정과 연결 된 도메인에서 고유 해야 합니다. 그렇지 않으면 도메인 생성 되지 않습니다 및 오류 응답이 있음을 보내집니다. 도메인 이름에 대 한 모든 작업 새로 만든된 도메인 아니라 기존 도메인에 대해 수행 됩니다.

### <a name="creating-simpledb-objects"></a>SimpleDB 개체 만들기

예제 응용 프로그램 사용은 `TodoItem` 모델 데이터에는 클래스입니다. 저장 하는 `TodoItem` 으로 먼저 변환 해야 SimpleDB 서비스의 인스턴스는 `List` 의 `ReplaceableAttribute` 개체입니다. 이렇게는 `ToSimpleDBReplaceableAttributes` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    new ReplaceableAttribute () {
      Name = "Name",
      Value = item.Name,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Notes",
      Value = item.Notes,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Done",
      Value = item.Done.ToString (),
      Replace = true
    }
  };
}
```

이 메서드가 만드는 `List` 의 새 `ReplaceableAttribute` 인스턴스와는 `List` 단일 나타내는 `TodoItem` 인스턴스. 각 `ReplaceableAttribute` 인스턴스가 단일 속성을 나타내는 `TodoItem` 인스턴스. 에 대 한 자세한 내용은 `ReplaceableAttribute` 클래스를 참조 하십시오. [ReplaceableAttribute 클래스](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_ReplaceableAttribute.htm) Amazon의 웹 사이트에서.

마찬가지로, SimpleDB 서비스에서 데이터를 검색 하는 경우 변환 해야에서 `List` 의 `Attribute` 인스턴스는 `TodoItem` 인스턴스. 이 사용 하 여 수행 되는 `FromSimpleDBAttributes` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

```csharp
TodoItem FromSimpleDBAttributes (List<Amazon.SimpleDB.Model.Attribute> attributeList, string id)
{
  var todoItem = new TodoItem ();
  todoItem.ID = id;
  todoItem.Name = attributeList.Where (attr => attr.Name == "Name").FirstOrDefault ().Value;
  todoItem.Notes = attributeList.Where (attr => attr.Name == "Notes").FirstOrDefault ().Value;
  todoItem.Done = Convert.ToBoolean (attributeList.Where (attr => attr.Name == "Done").FirstOrDefault ().Value);
  return todoItem;
}
```

이 메서드는 단순히 각 검색 `Attribute` 에서 인스턴스는 `List` 새로 만들어진 설정 `TodoItem` 인스턴스.

에 대 한 자세한 내용은 `Attribute` 클래스를 참조 하십시오. [특성 클래스](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_Attribute.htm) Amazon의 웹 사이트에서.

### <a name="querying-data"></a>데이터 쿼리

호출 하 여 도메인의 콘텐츠를 검색할 수는 `AmazonSimpleDBClient.SelectAsync` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
      SelectExpression = string.Format ("SELECT * from {0}", tableName)
  };
  var response = await client.SelectAsync (request);
  foreach (var item in response.Items) {
    Items.Add (FromSimpleDBAttributes (item.Attributes, item.Name));
  }
  ...
}
```

`SelectAsync` 메서드에 `SelectRequest` 인스턴스를 지정 하는 매개 변수로 `Select` 쿼리 식에서의 `SelectExpression` 속성입니다. 쿼리 식의 형식은 표준 SQL 형식에 비슷합니다. `SELECT` 문. 쿼리 식에 대 한 자세한 내용은 참조 [만들 Amazon SimpleDB 쿼리를 사용 하 여 선택](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) Amazon의 웹 사이트에서.

> [!NOTE]
> 규칙을 따르도록 따옴표 쿼리 식을 생성할 때는 주의 해야 합니다. 자세한 내용은 참조 [인용 규칙 선택](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) Amazon의 웹 사이트에서.

`SelectAsync` 메서드 항목 및 쿼리 식과 일치 하는 관련된 특성의 컬렉션을 포함 하는 응답을 반환 합니다. 이 컬렉션은 다음 변환 된 `List` 의 `TodoItem` 디스플레이 대 한 인스턴스.

### <a name="creating-and-replacing-data"></a>만들기 및 데이터를 교체

`AmazonSimpleDBClient.PutAttributesAsync` 다음 코드 예제와 같이 메서드를 만들고 SimpleDB 서비스 도메인에서 데이터를 사용 합니다.

```csharp
public async Task SaveTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBReplaceableAttributes (todoItem);
  var request = new PutAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.PutAttributesAsync (request);
  ...
}
```

`PutAttributesAsync` 메서드에 `PutAttributesRequest` 인스턴스를 매개 변수로 합니다. `PutAttributesRequest` 인스턴스에 새 항목으로 만들거나 기존 항목에서 대체 된 특성 이름-값 쌍을 지정 합니다. `List` 의 `ReplaceableAttribute` 인스턴스 여 빌드되는 `ToSimpleDBReplaceableAttributes` 메서드. 이 메서드는 또한 설정는 `Replace` 각 속성 `ReplaceableAttribute` 를 `true`합니다. 이렇게 하면 데이터를 대체 하는 경우 기존 특성 값을 바꿀 새 특성 값입니다. 그러나 존재 하지 않는 특성 값을 대체 하는 오류 응답에서 발생 하지 합니다.

값은 `PutAttributesRequest.ItemName` 속성 도메인에 새 항목은 추가 여부 또는 기존 항목은 대체 하는 여부를 제어 합니다. 응용 프로그램을 새 항목을 만들 때 설정 된 `TodoItem.ID` 속성을 새 `Guid`합니다. 이렇게 하면 각 `TodoItem` 인스턴스에 고유 식별자입니다. 따라서 경우는 `PutAttributesRequest.ItemName` 속성이 도메인에 존재 하지 않는 값으로 설정 되 면 SimpleDB 서비스는 지정 된 특성 이름-값 쌍이 들어 있는 새 항목을 만듭니다. 경우는 `PutAttributesRequest.ItemName` 속성이 도메인에 이미 있는 값으로 설정 되어 있으면 SimpleDB 서비스 지정 된 특성 이름-값 쌍이 있는 항목을 업데이트 합니다.

### <a name="deleting-data"></a>데이터 삭제

`AmazonSimpleDBClient.DeleteAttributesAsync` 메서드는 다음 코드 예제에 나와 있는 것 처럼 SimpleDB 서비스 도메인에서 데이터를 삭제 사용 됩니다.

```csharp
public async Task DeleteTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBAttributes (todoItem);
  var request = new DeleteAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.DeleteAttributesAsync (request);
  ...
}
```

`DeleteAttributesAsync` 메서드에 `DeleteAttributesRequest` 인스턴스를 매개 변수로 합니다.  `DeleteAttributesRequest` 와 해당 항목을 삭제 하는 특성을 지정 하는 인스턴스는 `List` 의 `Attribute` 하 여 작성 중인 인스턴스를 삭제할 수는 `ToSimpleDBAttributes` 메서드. 항목의 속성을 모두 삭제 하는 항목이 삭제 됩니다.

## <a name="summary"></a>요약

이 문서는 쿼리, 만들기 및 바꾸기, AWS SDK for.NET을 사용 하며 SimpleDB 서비스에 저장 된 데이터를 삭제 하는 방법을 설명 합니다. 이 SDK에서 제공 된 `CognitoAWSCredentials` 및 `AmazonSimpleDBClient` SimpleDB 서비스에 액세스 하는 Xamarin.Forms 응용 프로그램에서 사용 되는 클래스입니다.


## <a name="related-links"></a>관련 링크

- [TodoAWS (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS/)
- [Amazon 웹 서비스 SDK Xamarin 개발자 가이드](http://docs.aws.amazon.com/mobile/sdkforxamarin/developerguide/)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB 개발자 설명서](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [AmazonSimpleDBClient 클래스](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_AmazonSimpleDBClient.htm)
- [.NET 용 Amazon 웹 서비스 SDK](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
