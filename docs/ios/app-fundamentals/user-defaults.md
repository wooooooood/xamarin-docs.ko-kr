---
title: "사용자 기본값 사용"
description: "이 문서에서는 NSUserDefault Xamarin iOS 응용 프로그램 또는 확장에서에서 기본 설정을 저장 하려면 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: DAE7FFC4-B8C9-4D9E-886A-9B2388452EEB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 73f3beb87fffcb37ef3e36d54f634c3bc62da538
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-user-defaults"></a>사용자 기본값 사용

_이 문서에서는 NSUserDefault Xamarin.iOS 앱 또는 확장에서 기본 설정을 저장 하려면 작업을 설명 합니다._


`NSUserDefaults` ios 앱 및 확장 프로그래밍 방식으로 시스템 차원 기본값 시스템과 상호 작용 하는 방법을 제공 합니다. 기본적으로 시스템을 사용 하 여 사용자는 앱의 동작이 나 (응용 프로그램의 디자인에 기반)의 기본 설정을 충족 하기 위해 스타일을 지정 구성할 수 있습니다. 예를 들어, 제국 측정 단위가 미터법 vs에서 데이터를 표시 하거나 특정된 UI 테마를 선택 합니다.

앱 그룹와 함께 사용할 경우 `NSUserDefaults` 또한 지정된 된 그룹 내에서 응용 프로그램 (또는 확장) 사이 통신 하는 방법을 제공 합니다.

<a name="About-User-Defaults" />

## <a name="about-user-defaults"></a>사용자 기본값에 대 한

사용자 기본값 위에서 설명한 것 처럼 (`NSUserDefaults`) 응용 프로그램 (또는 확장명)에 추가할 수 있으며 최종 사용자는 모양이 나 런타임에 응용 프로그램의 작업을 조정 하기 위해 수정할 수 있는 구성 가능한 옵션을 제공 하는 데 사용 합니다.

응용 프로그램 먼저 실행 되 면 `NSUserDefaults` 응용 프로그램의 사용자 기본 데이터베이스에서 키와 값은 읽고 값이 필요 열기 및 될 때마다 데이터베이스 읽기를 방지 하기 위해 메모리에 캐시 합니다. 

> [!IMPORTANT]
> Apple 더 이상 권장 하는 개발자 호출은 `Synchronize` 메서드를 직접 데이터베이스와 메모리 내 캐시를 동기화 합니다. 대신, 자동으로 호출 됩니다 주기적으로 메모리에 캐시를 사용자의 기본 데이터베이스와 동기화 상태로 유지 합니다.

`NSUserDefaults` 읽고와 같은 일반적인 데이터 형식에 대 한 기본 설정 값을 쓰기 위한 몇 가지 편의 메서드를 포함 하는 클래스: 문자열, 정수, float, boolean 및 Url입니다. 사용 하 여 다른 종류의 데이터를 보관할 수 `NSData`, 다음에서 읽거나 사용자 기본 데이터베이스에 기록 합니다. 자세한 내용은 Apple의를 참조 하십시오 [기본 설정 및 설정 프로그래밍 가이드](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)합니다.

<a name="Accessing-the-Shared-NSUserDefaults-Instance" />

## <a name="accessing-the-shared-nsuserdefaults-instance"></a>공유 NSUserDefaults 인스턴스에 액세스 

공유 사용자 기본값 인스턴스 장치의 현재 사용자에 대 한 사용자 기본값에 대 한 액세스를 제공합니다. 기본적으로 공유 개체가 존재 하지 않으면 처음으로 액세스 되 고 다음 정보를 사용 하 여 초기화 만들어집니다.

- `NSArgumentDomain` 로 구성 된 현재 응용 프로그램에서 구문 분석 하는 기본값입니다.
- 앱의 번들 식별자 도메인입니다.
- `NSGlobalDomain` 로 구성 된 모든 앱에서 공유 하는 기본값입니다.
- 각 사용자의 기본 언어에 대해 별도 도메인입니다.
- `NSRegistrationDomain` 검색은 항상 성공 하도록 응용 프로그램에서 수정할 수 있는 임시 기본값 집합이 있는 합니다.

공유 사용자 기본값 인스턴스에 액세스 하려면 다음 코드를 사용 합니다.

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
```

<a name="Accessing-an-App-Group-NSUserDefaults-Instance" />

## <a name="accessing-an-app-group-nsuserdefaults-instance"></a>앱 그룹 NSUserDefaults 인스턴스 액세스

앱 그룹을 사용 하 여 설명한 것 처럼 `NSUserDefaults` 지정된 된 그룹 내에서 응용 프로그램 (또는 확장) 간의 통신에 사용할 수 있습니다. 첫째, 있는지 App 그룹 및 필수 응용 프로그램 Id가 올바르게 구성 되도록 해야 합니다는 **인증서, 식별자 및 프로필** 섹션에서 [iOS Dev Center](https://developer.apple.com/devcenter/ios/) 및 설치 된 개발 환경입니다.

응용 프로그램 및/또는 확장 프로젝트 다음으로, 위에서 만든 유효한 응용 프로그램 Id 중 하나를 가질 필요가 및 `Entitlements.plist` 파일의 앱 그룹을 사용 하 고 지정한 앱 번들에 포함 되도록 합니다.

위치에이 작업은이 모두와 공유 응용 프로그램 그룹 사용자 기본값 액세스할 수 있습니다 다음 코드를 사용 하 여:

```csharp
// Get App Group User Defaults
var plist = new NSUserDefaults ("group.com.xamarin.todaysharing", NSUserDefaultsType.SuiteName);
```

여기서 `group.com.xamarin.todaysharing` 에서 응용 프로그램 그룹이 생성은 **인증서, 식별자 및 프로필** 액세스 하려는 합니다. 자세한 내용은 참조 하십시오는 [앱 그룹 기능](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) 설명서입니다.

<a name="Reading-Default-Values" />

## <a name="reading-default-values"></a>기본 값 읽기

원하는 사용자 기본 데이터베이스에 액세스 한 후 키/값 쌍을 읽고 있는 데이터의 형식을 기반으로 여러 개의 편리한 메서드를 사용 하 여 기본값에서 값을 읽을 수 있습니다.

- `ArrayForKey` -의 배열을 반환 `NSObjects` 주어진된 키 값에 대 한 합니다.
- `BoolForKey` -지정된 된 키에 대 한 부울 값을 반환합니다.
- `DataForKey` -반환는 `NSData` 지정된 된 키에 대 한 개체입니다.
- `DictionaryForKey` -반환는 `NSDictionary` 지정된 된 키에 대 한 합니다.
- `DoubleForKey` -지정된 된 키에 대 한 double 값을 반환 합니다.
- `FloatForKey` -지정된 된 키에 대 한 부동 소수점 값을 반환합니다.
- `IntForKey` -지정된 된 키에 대 한 정수 값을 반환 합니다.
- `StringArrayForKey` -의 배열을 반환 `String` 지정된 된 키 값에서 개체입니다.
- `StringForKey` -지정된 된 키에 대 한 문자열 값을 반환 합니다.
- `URLForKey` -반환는 `NSUrl` 지정된 된 키에 대 한 값입니다.

예를 들어 다음 코드는 사용자 기본값에서 부울 값을 다음과 같습니다.

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Get value
var useHeader = plist.BoolForKey("UseHeader");

```

<a name="Writing-Default-Values" />

## <a name="writing-default-values"></a>기본 값을 쓰는 중

위의 값을 읽는 것 처럼 원하는 사용자 기본 데이터베이스에 액세스 한 후에 쓸 수 있습니다 값 키/값 쌍과 기록 될 데이터 형식에 따라 여러 개의 편리한 메서드를 사용 하 여 기본값:

- `SetBool` -지정된 된 키에 지정 된 부울 값을 씁니다.
- `SetDouble` -지정된 된 키에 지정된 하는 double 값을 씁니다.
- `SetFloat` -지정된 된 키에 지정 된 float 값을 씁니다.
- `SetString` -지정된 된 키에 지정 된 문자열 값을 씁니다.
- `SetURL` -지정된 된 URL을 기록 합니다. (`NSUrl`) 값을 지정 된 키입니다.

예를 들어 다음 코드는 사용자 기본값에 부울 값을 쓸 것:

```csharp
// Get Shared User Defaults
var plist = NSUserDefaults.StandardUserDefaults;
...

// Save value
plist.SetBool(useHeader, "UseHeader");
...

```

> [!IMPORTANT]
> 응용 프로그램 먼저 실행 되 면 `NSUserDefaults` 응용 프로그램의 사용자 기본 데이터베이스에서 키와 값은 읽고 값이 필요 열기 및 될 때마다 데이터베이스 읽기를 방지 하기 위해 메모리에 캐시 합니다.



<a name="Summary" />

## <a name="summary"></a>요약

이 문서 검사가 수행 된 `NSUserDefaults` 클래스와 어떻게 최종 사용자 Xamarin.iOS 앱을 구성 하는 데 사용할 수 있는 옵션 집합 제공 하기 위해 사용할 수 있습니다. 또한 앱 그룹을 사용 하 여 확장와 해당 부모 응용 프로그램 간에 또는 그룹에는 앱 간에 통신을 다룹니다.


## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [기본 설정 및 설정 프로그래밍 가이드](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)
- [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/#//apple_ref/doc/constant_group/NSUserDefaults_Domains)
