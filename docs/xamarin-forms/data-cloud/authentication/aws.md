---
title: Amazon SimpleDB 서비스와 사용자 인증
description: Amazon SimpleDB 자체 사용 권한 리소스 기반의 시스템을 제공 하지 않습니다. 대신, id 공급자에 대 한 인증 갖도록 하는 데 사용자만 자신의 데이터에 대 한 액세스 SimpleDB 도메인에 사용할 수 있습니다. 이 문서에서는 자신의 SimpleDB 데이터에 대 한 사용자 액세스를 제한 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 797C91A5-9720-4DAC-89D8-5C85996584C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 592e957e0c64e7189d6f01f1ba0f23da074c4bec
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/12/2018
---
# <a name="authenticating-users-with-an-amazon-simpledb-service"></a>Amazon SimpleDB 서비스와 사용자 인증

_Amazon SimpleDB 자체 사용 권한 리소스 기반의 시스템을 제공 하지 않습니다. 대신, id 공급자에 대 한 인증 갖도록 하는 데 사용자만 자신의 데이터에 대 한 액세스 SimpleDB 도메인에 사용할 수 있습니다. 이 문서에서는 자신의 SimpleDB 데이터에 대 한 사용자 액세스를 제한 하는 방법에 설명 합니다._

[Xamarin.Auth](https://github.com/xamarin/Xamarin.Auth) 샘플 응용 프로그램에서 사용자 인증 프로세스를 관리 하 고 장치에 사용자의 계정 세부 정보를 안전 하 게 저장 하는 데 사용 됩니다. 자세한 내용은 참조 [Id 공급자를 통해 사용자 인증](~/xamarin-forms/data-cloud/authentication/oauth.md)합니다.

## <a name="allowing-an-authenticated-user-access-to-simpledb-domain-data"></a>SimpleDB 도메인 데이터에 대 한 인증 된 사용자 액세스를 허용합니다.

예제 응용 프로그램 사용은 `TodoItem` 모델 데이터에는 클래스입니다. 저장 하는 `TodoItem` 으로 먼저 변환 해야 SimpleDB 서비스의 인스턴스는 `List` 의 `ReplaceableAttribute` 개체입니다. 자세한 내용은 참조 [SimpleDB 만들기 개체](~/xamarin-forms/data-cloud/consuming/aws.md)합니다.

> [!NOTE]
> IOS 9 이상, 앱 전송 보안 (AT)는 인터넷 리소스 (예: 응용 프로그램의 백 엔드 서버)와 응용 프로그램 간에 보안 연결 중요 한 정보가 노출 되거나 실수로 인 한 것을 방지 적용 합니다. IOS 9에 대해 빌드된 앱에서 기본적으로 AT를 사용 하므로 모든 연결 AT 보안 요구 사항이 적용 됩니다. 연결에는 이러한 요구 사항을 충족 하지 않는 경우 예외와 함께 실패 합니다.
> 사용할 수 없는 경우의 AT 옵트인 수 있습니다는 `HTTPS` 및 프로토콜을 인터넷 리소스에 대 한 통신을 보호 합니다. 응용 프로그램의 업데이트 하 여 이렇게 할 **Info.plist** 파일입니다. 자세한 내용은 참조 [앱 전송 보안](~/ios/app-fundamentals/ats.md)합니다.

사용자 SimpleDB 도메인에만 자신의 데이터에 액세스할 수 있어야 하는 `ToSimpleDBReplaceableAttributes` 메서드 저장에 대 한 추가 특성이 `TodoItem` 다음 코드 예제와 같이 인스턴스:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    ...
    new ReplaceableAttribute () {
      Name = "User",
      Value = App.User.Email,
      Replace = true
    },
  };
}
```

이 특성 SimpleDB 도메인에 저장 된 각 항목 데이터 속한 사용자를 고유 하 게 식별 하는 데 사용 되는 사용자에 대 한 연결 된 전자 메일 주소 인지 확인 합니다. 호출 하 여 도메인의 콘텐츠는 검색 되는 경우는 `AmazonSimpleDBClient.SelectAsync` 메서드, 쿼리 식을 사용 하면 인증된 된 사용자에 대 한 항목만 검색 되었는지 다음 코드 예제에 나와 있는 것 처럼:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
    SelectExpression = string.Format ("SELECT * from {0} WHERE User = '{1}'", tableName, App.User.Email)
  };
  var response = await client.SelectAsync (request);
  ...
}
```

`SelectAsync` 메서드 항목 및 쿼리 식과 일치 하는 관련된 특성의 컬렉션을 포함 하는 응답을 반환 합니다. 쿼리 식만 사용자의 전자 메일 주소와 일치 하는 항목을 검색할 보장 합니다. 쿼리 식에 대 한 자세한 내용은 참조 [만들 Amazon SimpleDB 쿼리를 사용 하 여 선택](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) Amazon의 웹 사이트에서.

> [!NOTE]
> 규칙을 따르도록 따옴표 쿼리 식을 생성할 때는 주의 해야 합니다. 자세한 내용은 참조 [인용 규칙 선택](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) Amazon의 웹 사이트에서.

## <a name="summary"></a>요약

이 문서에는 자신의 SimpleDB 데이터에 대 한 사용자 액세스를 제한 하는 방법을 설명 합니다. Amazon SimpleDB 자체 사용 권한 리소스 기반의 시스템을 제공 하지 않습니다. 대신, id 공급자에 대 한 인증 SimpleDB 도메인에만 자신의 데이터에 액세스할 수 있어야 사용자 데 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [TodoAWSAuth (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWSAuth/)
- [Amazon SimpleDB 서비스 사용](~/xamarin-forms/data-cloud/consuming/aws.md)
- [Id 공급자를 통해 사용자 인증](~/xamarin-forms/data-cloud/authentication/oauth.md)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB 개발자 설명서](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [.NET 용 Amazon 웹 서비스 SDK](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
