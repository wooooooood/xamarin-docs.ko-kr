---
title: iOS 9 호환성
description: 응용 프로그램에 곧바로 iOS 9 기능을 추가 하지 않으려는 경우에 최신 버전의 Xamarin 앱을 다시 작성 해야 합니다.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b7cbec3f064e6000f991fb0f9ce256415f6ce5dd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30777553"
---
# <a name="ios-9-compatibility"></a>iOS 9 호환성

_응용 프로그램에 곧바로 iOS 9 기능을 추가 하지 않으려는 경우에 최신 버전의 Xamarin 앱을 다시 작성 해야 합니다._

> [!IMPORTANT]
> 이 페이지의 정보는 고객용 이미 앱 스토어에서에서 앱과 iOS 8을 대상으로 지정 하거나 이전에 게 전송 되지 않은 이미 iOS 9 호환성에 대 한 업데이트 합니다. Xcode 7 및 앱 개발을 위한 Xamarin.iOS 9-하십시오 방문-최신 버전을 이미 사용 중인 경우는 [iOS 9 소개](~/ios/platform/introduction-to-ios9/index.md)합니다.

첫 번째 iOS 9 베타 나타날 때 이전 버전에서 iOS 9 시작할 수 없게 하는 오래 된 앱으로 표시 되는 Xamarin의 두 가지 문제를 확인 했습니다.

이 두 가지 문제 (으로 [당사 포럼에 자세히](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) 되었습니다.

- 8 또는 32 비트 장치에서 시작할 수 없는 이전 버전 iOS 용 앱 빌드 (사용 하 여 빌드한 앱을 포함 하는 [통합 API](~/cross-platform/macios/unified/index.md)).
- P/Invoke의 전체 경로 표시 하며 실패 지정 되지 않았습니다.

이 두 가지 문제를 해결 하는 최신 안정화 채널 릴리스 후 다시 빌드를 응용 프로그램을 다시 배포 Xamarin 설치를 업데이트 합니다.

_최신 버전의 Xamarin 빌드 전 및 앱 스토어에 제출할 다시 권장에 iOS 9 기능으로 앱을 즉시 업데이트를 계획 아닌 경우에_합니다.



이렇게 하면 고객으로 업그레이드 한 후 앱이 iOS 9에서 실행 됩니다.
IOS 8-지원 하도록 계속 수 최신 릴리스를 다시 작성 응용 프로그램의 대상 버전에 영향을 주지 않습니다.

IOS 9에 기존 앱을 테스트 하는 동안 추가 문제를 설정한 경우 읽기는 [향상 호환성](#compat) 아래 섹션.


### <a name="updating-with-visual-studio"></a>Visual Studio와 함께 업데이트

Visual Studio 안정적인 최신 버전으로 업데이트 했는지를 명시적으로 확인 하는 것이 좋습니다.

## <a name="what-about-components-nugets-and-other-libraries"></a>구성 요소, Nugets, 및 기타 라이브러리에 대 한 무엇입니까?

작업을 수행한 **하지** 새로운 버전의 모든 구성 요소 또는 Nugets 위에서 언급 한 두 가지 문제를 해결 하기 위해 사용 하는 때까지 대기 해야 합니다.
안정적인 최신 릴리스의 Xamarin.iOS 앱을 다시 작성 하 여 이러한 문제가 해결 되었습니다.

구성 요소 공급 업체와 Nuget 작성자는 마찬가지로, **하지** 를 위에서 언급 한 두 가지 문제를 해결 하는 새 빌드를 전송 해야 합니다. 그러나 있는 경우 구성 요소 또는 Nuget 사용 하 여 `UICollectionView` 또는 뷰를 로드 **Xib** 파일에 대 한 업데이트 *수 있습니다* 아래에서 설명한 iOS 9 호환성 문제를 해결 해야 합니다.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>코드에서 호환성을 개선합니다.

코드의 몇 가지 사례 하는 패턴는 *사용* iOS 9의에서 주요 iOS의 이전 버전에서 작동 하도록 합니다. 다음은 몇 가지 가능한 문제 (및 솔루션) iOS 9 테스트할 때 발생할 수 있는:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView는 생성자에서 null입니다.

**이유:** iOS 9에에서는 `initWithFrame:` 생성자는 이제 iOS 9와의 동작 변경 내용으로 인해 필요한는 [UICollectionView 설명서에 나와](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)합니다. 셀 호출 하 여 초기화 됩니다 지정된 된 식별자에 대 한 클래스를 등록 하 고 새로운 셀을 만들어야 하는 경우 해당 `initWithFrame:` 메서드.

**Fix:** 추가 `initWithFrame:` 다음과 같은 생성자:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

관련 샘플: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib에서 뷰를 로드할 때 UIView 코드 작성자를 초기화 하지

**이유:** 는 `initWithCoder:` 생성자는 인터페이스 작성기 Xib 파일에서 뷰를 로드할 때 호출 합니다. 이 생성자는 내보내지지 않습니다 비관리 코드의 관리 되는 버전 우리의 호출할 수 없습니다. 이전에 않음 (예: iOS 8)에 `IntPtr` 보기를 초기화할 수 있도록 생성자 호출 되었습니다.

**Fix:** 만들기 및 내보내기는 `initWithCoder:` 다음과 같은 생성자:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

관련된 샘플: [채팅](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld Message: 이름으로 캐시 이미지가 중...

로그에 다음 정보와 함께 충돌이 발생할 수 있습니다.

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**원인:** 개인 프레임 워크 공용 만들 때 발생 합니다. Apple의 네이티브 링커의 버그입니다 (JavaScriptCore 만들어진 iOS 7에서에서 공용 전에 이것은 개인 프레임 워크) 및 앱의 배포 대상 iOS 버전은 때는 프레임 워크 개인 했습니다. 이 경우 Apple의 링커는 공용 버전 대신 프레임 워크의 개인 버전과 연결 됩니다.

**Fix:** iOS 9에 대 한 해결 될 예정 이지만 그 동안에 직접 적용할 수는 쉽게 해결 방법: 방금 (연습할 수 iOS 7이 예제의) 프로젝트에서 나중 iOS 버전을 대상입니다. 다른 프레임 워크는 비슷한 문제를 나타낼 수 있습니다, 예를 들어 WebKit 프레임 워크 iOS 8에서에서 공용으로 설정 되었습니다 (및 없으므로 iOS 7을 대상으로 실행;이 오류가 발생 하면 대상으로 해야 iOS WebKit 응용 프로그램에서 사용 하는 8).



## <a name="related-links"></a>관련 링크

- [iOS 9 호환성 릴리스 정보](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [iOS 9 소개](~/ios/platform/introduction-to-ios9/index.md)
- [Xamarin.iOS 앱 iOS9 (비디오)로 업데이트](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
