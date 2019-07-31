---
title: Xamarin.ios의 사용자 기본값 작업
description: 이 문서에서는 NSUserDefaults를 사용 하 여 Xamarin iOS 앱 또는 확장에서 기본 설정을 저장 하는 방법을 설명 합니다. 상위 수준에서 NSUserDefaults를 설명 하 고 값을 읽고 쓰는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: a1bc00d69f5b00787ba0e16b7e3846d5f18a4bed
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655129"
---
# <a name="working-with-user-defaults-in-xamarinios"></a>Xamarin.ios의 사용자 기본값 작업

_이 문서에서는 NSUserDefault를 사용 하 여 Xamarin.ios 앱 또는 확장의 기본 설정을 저장 하는 방법을 설명 합니다._


클래스 `NSUserDefaults` 를 사용 하면 iOS 앱 및 확장이 시스템 차원의 기본값 시스템과 프로그래밍 방식으로 상호 작용 하는 방법을 제공 합니다. 사용자는 기본값 시스템을 사용 하 여 앱의 동작 또는 스타일을 구성 하 여 앱의 디자인에 따라 기본 설정을 충족 시킬 수 있습니다. 예를 들어 메트릭 및 왕정 측정의 데이터를 표시 하거나 지정 된 UI 테마를 선택 합니다.

앱 그룹과 함께 사용 하 `NSUserDefaults` 는 경우 지정 된 그룹 내에서 앱 (또는 확장) 간에 통신 하는 방법도 제공 합니다.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>사용자 기본값 정보

위에서 설명한 것 처럼 사용자 기본값 (`NSUserDefaults`)을 앱 또는 확장에 추가 하 고 최종 사용자가 런타임에 응용 프로그램의 모양이 나 작업을 조정 하기 위해 수정할 수 있는 구성 가능한 옵션을 제공 하는 데 사용할 수 있습니다.

앱이 처음으로 실행 될 `NSUserDefaults` 때는 앱의 사용자 기본값 데이터베이스에서 키와 값을 읽어 메모리에 캐시 하 여 값이 필요할 때마다 데이터베이스를 열고 읽지 못하게 합니다. 

> [!IMPORTANT]
> Apple에서는 개발자가 메서드를 `Synchronize` 호출 하 여 메모리 내 캐시를 데이터베이스와 직접 동기화 하도록 권장 하지 않습니다. 대신, 메모리 내 캐시를 사용자의 기본값 데이터베이스와 동기화 된 상태로 유지 하기 위해 정기적으로 정기적으로 호출 됩니다.

클래스 `NSUserDefaults` 에는 문자열, 정수, 부동 소수점, 부울 및 url과 같은 공통 데이터 형식에 대 한 기본 설정 값을 읽고 쓰기 위한 몇 가지 편리한 메서드가 포함 되어 있습니다. 다른 유형의 데이터는를 사용 하 여 `NSData`보관 한 다음 사용자 기본값 데이터베이스에서 읽거나 쓸 수 있습니다. 자세한 내용은 Apple의 [기본 설정 및 설정 프로그래밍 가이드](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)를 참조 하세요.

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>공유 NSUserDefaults 인스턴스에 액세스 

공유 사용자 기본값 인스턴스는 장치의 현재 사용자에 대 한 사용자 기본값에 대 한 액세스를 제공 합니다. 공유 기본값 개체가 없으면 다음 정보를 사용 하 여 처음으로 액세스 하 고 초기화할 때 생성 됩니다.

- 현재 응용 프로그램에서 구문 분석 되는 기본값으로 구성된입니다.`NSArgumentDomain`
- 앱의 번들 식별자 도메인입니다.
- 모든 응용 프로그램에서 공유 하는 기본값으로 구성된입니다.`NSGlobalDomain`
- 각 사용자의 기본 설정 언어에 대 한 별도의 도메인입니다.
- 검색을 항상 성공적으로 수행 하기 위해 앱에서 수정할 수 있는 임시 기본값 집합이 포함된입니다.`NSRegistrationDomain`

공유 사용자 기본값 인스턴스에 액세스 하려면 다음 코드를 사용 합니다.

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>앱 그룹 NSUserDefaults 인스턴스에 액세스

위에서 설명한 것 처럼 앱 그룹을 `NSUserDefaults` 사용 하 여 지정 된 그룹 내의 앱 (또는 확장) 간에 통신 하는 데 사용할 수 있습니다. 먼저 [IOS 개발자 센터](https://developer.apple.com/devcenter/ios/) 의 **인증서, 식별자 & 프로필** 섹션에서 앱 그룹 및 필수 앱 id가 올바르게 구성 되어 있고 개발 환경에 설치 되어 있는지 확인 해야 합니다.

다음으로, 앱 및/또는 확장 프로젝트에는 위에서 만든 유효한 앱 id 중 하나가 있어야 하며,이 파일 `Entitlements.plist` 은 앱 그룹을 사용 하도록 설정 하 고 지정 하 여 앱 번들에 포함 해야 합니다.

이 모두를 사용 하는 경우 다음 코드를 사용 하 여 공유 앱 그룹 사용자 기본값에 액세스할 수 있습니다.

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

여기서 `group.com.xamarin.todaysharing` 는 인증서에서 만든 앱 그룹이 고 **, 식별자 &** 는 액세스 하려는 프로필입니다. 자세한 내용은 참조 하십시오 합니다 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>기본값 읽기

원하는 사용자 기본 데이터베이스에 액세스 한 후에는 키/값 쌍을 사용 하 여 기본값에서 값을 읽을 수 있으며 읽고 있는 데이터의 형식에 따라 여러 가지 편리한 메서드를 사용할 수 있습니다.

- `ArrayForKey`-지정 된 키 값 `NSObjects` 에 대 한의 배열을 반환 합니다.
- `BoolForKey`-지정 된 키에 대 한 부울 값을 반환 합니다.
- `DataForKey`-지정 된 `NSData` 키에 대 한 개체를 반환 합니다.
- `DictionaryForKey`-지정 된 `NSDictionary` 키에 대 한를 반환 합니다.
- `DoubleForKey`-지정 된 키에 대 한 double 값을 반환 합니다.
- `FloatForKey`-지정 된 키에 대 한 부동 소수점 값을 반환 합니다.
- `IntForKey`-지정 된 키에 대 한 정수 값을 반환 합니다.
- `StringArrayForKey`-지정 된 키 값 `String` 에서 개체의 배열을 반환 합니다.
- `StringForKey`-지정 된 키에 대 한 문자열 값을 반환 합니다.
- `URLForKey`-지정 된 `NSUrl` 키에 대 한 값을 반환 합니다.

예를 들어 다음 코드는 사용자 기본값에서 부울 값을 읽습니다.

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>기본값 작성

위의 값을 읽는 것과 마찬가지로 원하는 사용자 기본 데이터베이스에 액세스 한 후에는 키/값 쌍을 사용 하 여 기본값에 값을 쓰고 작성 되는 데이터의 형식에 따라 몇 가지 편리한 메서드를 만들 수 있습니다.

- `SetBool`-지정 된 키에 지정 된 부울 값을 씁니다.
- `SetDouble`-지정 된 키에 지정 된 double 값을 씁니다.
- `SetFloat`-지정 된 키에 지정 된 float 값을 씁니다.
- `SetString`-지정 된 키에 지정 된 문자열 값을 씁니다.
- `SetURL`-지정 된 URL (`NSUrl`) 값을 지정 된 키에 씁니다.

예를 들어 다음 코드는 사용자 기본값에 부울 값을 기록 합니다.

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> 앱이 처음으로 실행 될 `NSUserDefaults` 때는 앱의 사용자 기본값 데이터베이스에서 키와 값을 읽어 메모리에 캐시 하 여 값이 필요할 때마다 데이터베이스를 열고 읽지 못하게 합니다.



<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 `NSUserDefaults` 클래스와 클래스를 사용 하 여 최종 사용자가 xamarin.ios 앱을 구성 하는 데 사용할 수 있는 옵션 집합을 제공 하는 방법에 대해 설명 했습니다. 또한 앱 그룹을 사용 하 여 확장과 부모 앱 간에 통신 하거나 그룹의 앱 간에 통신 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [기본 설정 및 설정 프로그래밍 가이드](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
