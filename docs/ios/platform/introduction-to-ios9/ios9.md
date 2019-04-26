---
title: iOS 9 호환성
description: IOS 9 기능을 바로 앱을 추가 하지 않으려는 경우에 최신 버전의 Xamarin 앱을 다시 작성 해야 합니다.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 6ade1c05c8e1cc64a4d24df1284d86175083ab80
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293654"
---
# <a name="ios-9-compatibility"></a>iOS 9 호환성

_IOS 9 기능을 바로 앱을 추가 하지 않으려는 경우에 최신 버전의 Xamarin 앱을 다시 작성 해야 합니다._

> [!IMPORTANT]
> 이 페이지의 정보는 iOS 8을 대상으로 하는 앱 스토어에서 이미 앱을 사용 하 여 고객을 위해 또는 이전에 전송 되지 않은 이미 iOS 9 호환성에 대 한 업데이트 합니다. 최신 버전은 이미 사용 중인 경우 Xcode 7 응용 프로그램 개발에 대 한 Xamarin.iOS 9-방문 하십시오 합니다 [iOS 9 소개](~/ios/platform/introduction-to-ios9/index.md)합니다.

첫 번째 iOS 9 베타 표시 하는 경우 이전 앱 ios 9 시작 하는 것으로 표시 되는 Xamarin의 이전 버전을 사용 하 여 두 가지 문제를 확인 했습니다.

이러한 두 가지 문제 (으로 [포럼에서 자세한](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) 되었습니다.

- 8 또는 32 비트 장치에서 시작할 수 없는 이전 버전의 iOS 용 앱 빌드 (로 빌드된 앱을 포함 하는 [Unified API](~/cross-platform/macios/unified/index.md)).
- P/Invoke의 전체 경로 사용 하 여 실패를 지정 하지 않았습니다.

이러한 두 가지 문제를 해결 Xamarin 설치를 최신 안정 채널 릴리스 및 다음 다시 빌드하고 앱을 업데이트 합니다.

_최신 버전의 Xamarin 사용 하 여 다시 빌드 및 다시 앱 스토어에 제출 권장 iOS 9 기능을 사용 하 여 앱을 즉시 업데이트 계획이 아니라면, 경우에_합니다.



이렇게 하면 고객에 게 업그레이드 한 후 앱 iOS 9에서 실행 됩니다.
IOS 8-지원 하도록 할 수 있습니다 최신 릴리스를 사용 하 여 다시 작성 응용 프로그램의 대상 버전에 영향을 주지 않습니다.

IOS 9에서 기존 앱을 테스트 하는 동안 추가 문제가 있는 경우 읽기를 [호환성 향상](#compat) 아래의 섹션입니다.


### <a name="updating-with-visual-studio"></a>Visual Studio를 사용 하 여 업데이트

Visual Studio 안정적인 최신 버전으로 업데이트 되도록를 명시적으로 확인 하는 것이 좋습니다.

## <a name="what-about-components-nugets-and-other-libraries"></a>구성 요소, Nuget, 및 다른 라이브러리에 대 한 무엇입니까?

할 **되지** 새 버전의 모든 구성 요소 또는 Nuget 위에서 언급 한 두 가지 문제를 해결 하는 데 사용할 때까지 기다려야 합니다.
Xamarin.iOS의 안정적인 최신 릴리스를 사용 하 여 앱을 다시 작성 하면 이러한 문제가 해결 되었습니다.

구성 요소 공급 업체 및 Nuget 작성자는 마찬가지로 **되지** 위에서 언급 한 두 가지 문제를 해결 하는 새 빌드를 전송 해야 합니다. 그러나 있으면 구성 요소 또는 Nuget을 사용 하 여 `UICollectionView` 에서 뷰를 로드 하거나 **Xib** 파일을 업데이트 *수 있습니다* 아래 언급 된 iOS 9 호환성 문제를 해결 해야 합니다.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>코드의 호환성을 향상

코드의 경우 몇 가지 패턴에 *사용* iOS 9의에서 주요 iOS의 이전 버전에서 작동 하도록 합니다. 다음은 몇 가지 가능한 문제 (및 해당 솔루션) iOS 9에서 테스트할 때 발생할 수 있는:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView는 생성자에서 null입니다.

**원인:** Ios 9 `initWithFrame:` 생성자는 이제 iOS 9로의 동작 변경 내용으로 인해 필요한 합니다 [UICollectionView 설명서에 나와](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)합니다. 셀을 호출 하 여 초기화 됩니다 지정된 된 식별자에 대 한 클래스를 등록 하 고 새 셀을 만들어야 하는 경우 해당 `initWithFrame:` 메서드.

**해결 방법:** 추가 된 `initWithFrame:` 다음과 같은 생성자:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

관련된 샘플: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView는 Xib/Nib에서 뷰를 로드할 때 코드 작성자를 사용 하 여 초기화 하지 못함

**원인:** `initWithCoder:` 생성자 인터페이스 작성기 Xib 파일에서 뷰를 로드할 때 호출 됩니다. 이 생성자는 내보내지지 않음 경우 비관리 코드는 관리 되는 버전을 호출할 수 없습니다. 이전에 (예: iOS 8)에 `IntPtr` 뷰를 초기화 하려면 생성자 호출 되었습니다.

**해결 방법:** 만들기 및 내보내기는 `initWithCoder:` 다음과 같은 생성자:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

관련된 샘플: [Chat](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld 메시지: 캐시 이미지가 없는 이름...

로그에서 다음 정보를 사용 하 여 충돌을 발생할 수 있습니다.

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**원인:** 개인 프레임 워크를 활용 하며 공개 하는 경우 발생 하는 Apple 네이티브 링커가의 버그입니다 (JavaScriptCore 수행 된 iOS 7에서에서 공개 하기 전에 개인 프레임 워크 였습니다) 및 프레임 워크 개인 때 앱의 배포 대상은 iOS 버전입니다. 이 경우 Apple의 링커는 공용 버전 대신 프레임 워크의 전용 버전을 사용 하 여 연결 됩니다.

**해결 방법:** IOS 9에 대 한 해결 될 것 이지만 그 동안에 직접 적용할 수 있습니다 문제를 쉽게 해결: 방금 이상 (보십시오 iOS 7이 예제의) 프로젝트에서 iOS 버전을 대상입니다. 다른 프레임 워크와 비슷한 문제가 발생할 수 있습니다, 예를 들어 WebKit framework iOS 8에서에서 공용으로 설정 되었습니다 (및 iOS 앱에서 WebKit를 사용 하는 8을 두어야 이므로이 오류를 발생 하면 iOS 7을 대상으로).



## <a name="related-links"></a>관련 링크

- [iOS 9 호환성 릴리스 정보](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [iOS 9 소개](~/ios/platform/introduction-to-ios9/index.md)
- [IOS9 (비디오)를 Xamarin.iOS 앱을 업데이트 하는 중입니다.](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
