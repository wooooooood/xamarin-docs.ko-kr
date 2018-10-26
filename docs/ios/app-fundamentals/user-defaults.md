---
title: Xamarin.iOS에서 사용자 기본값 사용
description: 이 문서에서는 Xamarin iOS 앱 또는 확장에서 기본 설정을 저장 하려면 NSUserDefaults 사용 하 여 작업을 설명 합니다. 높은 수준에서 NSUserDefaults를 설명 하 고 값을 쓰고 읽는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: 688db534d6c99a8fadb7535f0532f9c1e9564707
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120894"
---
# <a name="working-with-user-defaults-in-xamarinios"></a>Xamarin.iOS에서 사용자 기본값 사용

_이 문서에서는 Xamarin.iOS 앱 또는 확장에서 기본 설정을 저장 하려면 NSUserDefault 사용 하 여 작업을 다룹니다._


`NSUserDefaults` 클래스 iOS 앱 및 확장 시스템 차원 기본값은 시스템을 사용 하 여 프로그래밍 방식으로 상호 작용 하는 방법을 제공 합니다. 기본적으로 시스템을 사용 하 여 사용자 앱의 동작 또는 (앱의 디자인에 따라) 해당 기본 설정에 맞게 스타일 지정을 구성할 수 있습니다. 예를 들어 야드/파운드법 측정 메트릭 vs의 데이터를 제공 하거나 특정된 UI 테마를 선택 합니다.

앱 그룹을 사용 하는 경우 `NSUserDefaults` 지정된 된 그룹 내에서 앱 (또는 확장) 간에 통신 하는 방법을 제공 합니다.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>사용자 기본값에 대 한

사용자 기본값은 위에서 설명한 것 처럼 (`NSUserDefaults`) 앱 (또는 확장)를 추가할 수 있고 최종 사용자는 모양이 나 런타임 시 앱의 작업에 맞게 수정할 수 있는 구성 가능한 옵션을 제공 하는 데 사용 합니다.

응용 프로그램 먼저 실행 되 면 `NSUserDefaults` 앱의 사용자 기본값은 데이터베이스에서 키 및 값을 읽고 값이 필요를 열고 될 때마다 데이터베이스를 읽기를 방지 하려면 메모리에 캐시 합니다. 

> [!IMPORTANT]
> Apple 더 이상 권장 하는 개발자 호출을 `Synchronize` 직접 데이터베이스를 사용 하 여 메모리 내 캐시를 동기화 하는 방법입니다. 대신, 자동으로 호출 됩니다 주기적으로 메모리 내 캐시를 사용자의 기본값은 데이터베이스와 동기화 상태로 유지 합니다.

`NSUserDefaults` 읽고와 같은 일반적인 데이터 형식에 대 한 기본 설정 값을 쓰기 위한 몇 가지 편의 메서드를 포함 하는 클래스: 문자열, 정수, float, boolean 및 Url입니다. 사용 하 여 다른 유형의 데이터를 보관할 수 `NSData`에서 읽은 다음, 사용자 기본값은 데이터베이스에 기록 합니다. 자세한 내용은 Apple의를 참조 하세요 [기본 설정 및 설정을 프로그래밍 가이드](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)합니다.

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>공유 NSUserDefaults 인스턴스에 액세스 

공유 사용자 기본값 인스턴스 장치의 현재 사용자에 대 한 사용자 기본값에 대 한 액세스를 제공합니다. 기본적으로 공유 개체가 존재 하지 않으면 처음으로 액세스 되 고 다음 정보를 사용 하 여 초기화 만들어집니다.

- `NSArgumentDomain` 현재 앱에서 구문 분석 된 기본값을 구성 합니다.
- 앱의 번들 식별자 도메인입니다.
- `NSGlobalDomain` 모든 앱에서 공유 하는 기본값을 구성 합니다.
- 각 사용자의 기본 언어에 대해 별도 도메인입니다.
- `NSRegistrationDomain` 검색은 항상 성공 하도록 앱에서 수정할 수 있는 임시 기본값 집합을 사용 하 여 합니다.

공유 사용자 기본값 인스턴스에 액세스 하려면 다음 코드를 사용 합니다.

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>앱 그룹 NSUserDefaults 인스턴스에 액세스

앱 그룹을 사용 하 여 설명한 것 처럼 `NSUserDefaults` 지정된 된 그룹 내 앱 (또는 확장) 간의 통신에 사용할 수 있습니다. 처음에 앱 그룹 및 필수 앱 Id가 올바르게 구성 되도록 해야 합니다 **인증서, 식별자 및 프로필** 섹션에서 [iOS 개발자 센터](https://developer.apple.com/devcenter/ios/) 를 설치 했는지 개발 환경입니다.

앱 및/또는 확장 프로젝트 다음으로, 위에서 만든 유효한 앱 Id 중 하나를 사용할 필요가 및 `Entitlements.plist` 파일이 사용 하도록 설정 하 고 지정 된 앱 그룹을 사용 하 여 앱 번들에 포함 되어야 합니다.

현재 위치에서이 작업은이 모두를 사용 하 여는 공유 앱 그룹 사용자 기본값 액세스할 수 있습니다 다음 코드를 사용 하 여:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

여기서 `group.com.xamarin.todaysharing` 앱 그룹에 만들어집니다 **인증서, 식별자 및 프로필** 액세스 하려는 합니다. 자세한 내용은 참조 하십시오 합니다 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>기본 값 읽기

원하는 사용자 기본 데이터베이스에 액세스 한 후 키/값 쌍을 읽고 있는 데이터의 형식을 기반으로 하는 몇 가지 편의 메서드를 사용 하 여 기본값에서 값을 읽을 수 있습니다.

- `ArrayForKey` -의 배열을 반환 `NSObjects` 지정된 된 키 값에 대 한 합니다.
- `BoolForKey` -지정된 된 키에 대 한 부울 값을 반환 합니다.
- `DataForKey` -반환 된 `NSData` 지정된 된 키에 대 한 개체입니다.
- `DictionaryForKey` -반환 된 `NSDictionary` 지정된 된 키에 대 한 합니다.
- `DoubleForKey` -지정된 된 키에 대 한 double 값을 반환 합니다.
- `FloatForKey` -지정된 된 키에 대 한 부동 소수점 값을 반환 합니다.
- `IntForKey` -지정된 된 키에 대 한 정수 값을 반환 합니다.
- `StringArrayForKey` -의 배열을 반환 `String` 개체에서 지정된 된 키 값입니다.
- `StringForKey` -지정된 된 키에 대 한 문자열 값을 반환 합니다.
- `URLForKey` -반환 된 `NSUrl` 지정된 된 키에 대 한 값입니다.

예를 들어, 다음 코드는은 사용자 기본값에서 부울 값을 다음과 같습니다.

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>기본 값 쓰기

위의 값 읽기와 마찬가지로 원하는 사용자 기본 데이터베이스에 액세스 한 후에 쓸 수 값 키/값 쌍 및 기록 될 데이터 형식을 기반으로 하는 몇 가지 편의 메서드를 사용 하 여 기본값:

- `SetBool` -지정된 된 키에 지정된 된 부울 값을 씁니다.
- `SetDouble` -지정된 된 키에 지정된 된 double 값을 씁니다.
- `SetFloat` -지정된 된 키에 지정 된 float 값을 씁니다.
- `SetString` -지정된 된 키에 지정 된 문자열 값을 씁니다.
- `SetURL` --지정된 된 URL을 작성 하는 중 (`NSUrl`) 값을 지정 된 키입니다.

예를 들어, 다음 코드를 부울 값을 기본적으로 사용자를 작성 합니다.

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> 응용 프로그램 먼저 실행 되 면 `NSUserDefaults` 앱의 사용자 기본값은 데이터베이스에서 키 및 값을 읽고 값이 필요를 열고 될 때마다 데이터베이스를 읽기를 방지 하려면 메모리에 캐시 합니다.



<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 이러한 부분이 `NSUserDefaults` 클래스와이 사용할 수 있는 방법을 최종 사용자는 Xamarin.iOS 앱을 구성 하는 데 사용할 수 있는 옵션의 집합을 제공 합니다. 또한 앱 그룹을 사용 하 여 확장와 해당 부모 앱 또는 그룹의 앱 간에 통신을 다룹니다.


## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [기본 설정 및 설정 프로그래밍 가이드](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
