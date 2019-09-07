---
title: iOS 9 호환성
description: IOS 9 기능을 앱에 바로 추가할 계획이 없더라도 최신 버전의 Xamarin을 사용 하 여 앱을 다시 빌드해야 합니다.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 246653cee7917141ddd0f911a7c4d1b21f945360
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70751973"
---
# <a name="ios-9-compatibility"></a>iOS 9 호환성

_IOS 9 기능을 앱에 바로 추가할 계획이 없더라도 최신 버전의 Xamarin을 사용 하 여 앱을 다시 빌드해야 합니다._

> [!IMPORTANT]
> 이 페이지의 정보는 ios 9 호환성에 대 한 업데이트를 아직 제출 하지 않은 iOS 8 또는 이전 버전을 대상으로 하는 앱 스토어에 이미 앱이 있는 고객을 위한 것입니다. 최신 버전의 Xcode 7 및 Xamarin. iOS 9를 이미 사용 중인 경우에는 [ios 9 소개](~/ios/platform/introduction-to-ios9/index.md)를 참조 하세요.

첫 번째 iOS 9 베타 버전이 표시 되 면 iOS 9에서 오래 된 앱을 시작할 수 없는 것으로 확인 된 이전 버전의 Xamarin에 대 한 두 가지 문제를 확인 했습니다.

이러한 두 가지 문제 ( [포럼에 자세히 설명](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529))는 다음과 같습니다.

- IOS 8 또는 이전 버전의 앱 빌드는 32 비트 장치 ( [Unified API](~/cross-platform/macios/unified/index.md)로 빌드된 앱 포함)에서 시작할 수 없습니다.
- 전체 경로를 지정 하지 않고 P/Invoke가 실패 합니다.

Xamarin 설치를 안정적인 최신 채널 릴리스로 업데이트 한 다음 앱을 다시 빌드하고 다시 배포 하면 이러한 두 가지 문제가 해결 됩니다.

_IOS 9 기능으로 앱을 즉시 업데이트 하지 않으려는 경우에도 최신 버전의 Xamarin을 사용 하 여 다시 빌드하고 앱 스토어에 다시 제출 하는 것이 좋습니다_.

이렇게 하면 고객이 업그레이드 된 후 iOS 9에서 앱이 실행 됩니다.
최신 릴리스를 사용 하 여 다시 작성 해도 응용 프로그램 대상 버전에는 영향을 주지 않고 iOS 8을 계속 지원할 수 있습니다.

IOS 9에서 기존 앱을 테스트 하는 동안 추가 문제가 발생 하는 경우 아래의 [호환성 개선](#compat) 섹션을 참조 하세요.

### <a name="updating-with-visual-studio"></a>Visual Studio를 사용 하 여 업데이트

Visual Studio가 안정적인 최신 버전으로 업데이트 되었는지 여부를 명시적으로 확인 하는 것이 좋습니다.

## <a name="what-about-components-nugets-and-other-libraries"></a>구성 요소, Nuget 및 기타 라이브러리는 어떻게 되나요?

위에서 설명한 두 가지 문제를 해결 하기 위해 사용 하는 구성 요소 또는 Nuget의 새 버전을 기다릴 필요가 **없습니다** .
이러한 문제는 매우 안정적인 Xamarin.ios의 최신 릴리스로 앱을 다시 빌드하여 간단히 수정할 수 있습니다.

마찬가지로, 구성 요소 공급 업체와 Nuget 작성자는 위에서 언급 한 두 가지 문제를 해결 하기 위해 새 빌드를 제출 하는 데 필요 **하지 않습니다** . 그러나 구성 요소나 Nuget에서 **Xib** 파일의 뷰 `UICollectionView` 를 사용 하거나 로드 하는 경우 아래에 설명 된 iOS 9 호환성 문제를 해결 하기 위해 업데이트가 필요할 수 *있습니다* .

<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>코드의 호환성 향상

IOS 9에서 이전 버전의 iOS에서 작업 하는 *데 사용* 되는 몇 가지 코드 패턴이 있습니다. IOS 9에서 테스트 하는 경우 발생할 수 있는 몇 가지 문제 (및 해결 방법)는 다음과 같습니다.

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell는 생성자에서 null입니다.

**문서화** Ios 9 `initWithFrame:` 에서는 [UICollectionView 설명서 상태로](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath)ios 9의 동작 변경으로 인해 생성자가 필요 합니다. 지정 된 식별자에 대 한 클래스를 등록 하 고 새 셀을 만들어야 하는 경우 이제 해당 `initWithFrame:` 메서드를 호출 하 여 셀을 초기화 합니다.

**방법을** 다음과 같이 `initWithFrame:` 생성자를 추가 합니다.

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

관련 샘플: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Xib/Nib에서 뷰를 로드 하는 경우 UIView가 코드 작성자 있는지를 사용 하 여 초기화 되지 않습니다.

**문서화** 생성자 `initWithCoder:` 는 Interface Builder Xib 파일에서 뷰를 로드할 때 호출 되는 생성자입니다. 이 생성자가 내보내지 않는 경우 관리 되지 않는 코드는 관리 되는 버전을 호출할 수 없습니다. 이전 (예: iOS 8) `IntPtr` 에서 뷰를 초기화 하기 위해 생성자가 호출 되었습니다.

**방법을** 다음과 같이 생성자를 `initWithCoder:` 만들고 내보냅니다.

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

관련 샘플: [채팅](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

### <a name="dyld-message-no-cache-image-with-name"></a>Dyld 메시지: 이름이 있는 캐시 이미지가 없습니다 ...

로그에서 다음 정보를 사용 하 여 충돌이 발생할 수 있습니다.

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**문서화** 이는 개인 프레임 워크를 공용으로 만들 때 (JavaScriptCore가 전용 프레임 워크 였던 이전에는 iOS 7에서 공용으로 설정 됨), 프레임 워크가 전용 인 경우 앱의 배포 대상이 iOS 버전용입니다. 이 경우 Apple의 링커는 공용 버전 대신 프레임 워크의 전용 버전과 연결 됩니다.

**방법을** 이는 iOS 9에 대 한 것 이지만 사용자에 게 직접 적용할 수 있는 해결 방법이 있습니다. 프로젝트에서 이후 iOS 버전을 대상으로 합니다 .이 경우 iOS 7을 사용해 볼 수 있습니다. 다른 프레임 워크는 비슷한 문제를 나타낼 수 있습니다. 예를 들어 ios 8에서 WebKit framework가 공용으로 설정 되었으므로 iOS 7을 대상으로 지정 하면이 오류가 발생 합니다. 앱에서 WebKit를 사용 하려면 iOS 8을 대상으로 해야 합니다.

## <a name="related-links"></a>관련 링크

- [iOS 9 호환성 릴리스 정보](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [iOS 9 소개](~/ios/platform/introduction-to-ios9/index.md)
- [IOS9에 Xamarin.ios 앱 업데이트 (비디오)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
