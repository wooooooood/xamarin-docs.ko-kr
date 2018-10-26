---
title: Watchos에서 Xamarin 설정 작업
description: 이 문서에서는 Xamarin에서 watchOS 설정을 사용 하 여 작업 하는 방법을 설명 합니다. IPhone 앱과 Apple Watch 앱에서 이러한 설정을 사용 하 여 watch 앱 솔루션에 추가 설정을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 36164e1e9f92b5a5520d10f769f3953cfa2ceb85
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106353"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>Watchos에서 Xamarin 설정 작업

Apple Watch 앱은 iOS 앱으로 동일한 설정 기능을 사용할 수-사용자 인터페이스에 표시 되는 **Apple Watch** iPhone 앱 이지만 값은 모두 iPhone 앱 및 watch 확장을 액세스할 수 있습니다.

![](settings-images/intro.png "Apple Watch 앱은 iOS 앱으로 동일한 설정 기능을 사용할 수 있습니다.")

설정이 iOS 앱 및 정의한 watch 앱 확장을 모두에 액세스할 수 있는 공유 파일 위치에 저장 됩니다는 **앱 그룹**합니다. 수행 해야 합니다 [앱 그룹을 구성할](~/ios/watchos/app-fundamentals/app-groups.md) 아래 지침을 사용 하 여 설정을 추가 하기 전에 합니다.

## <a name="add-settings-in-a-watch-solution"></a>조사식 솔루션에서 설정 추가

에 **iPhone 앱** 솔루션에서 (*하지* watch 앱 또는 확장):

1. 마우스 오른쪽 단추로 클릭 **추가 > 새 파일...**  선택한 **Settings.bundle** (이름을 편집할 수 없습니다는 **새 파일** 대화):

   [![](settings-images/settings-add-sml.png "새 설정 번들을 추가 합니다.")](settings-images/settings-add.png#lightbox)

2. 이름을 변경할 **설정을 Watch.bundle** (선택 하 고 입력 **명령 + R** 바꾸려면):

   ![](settings-images/settings-rename.png "번들 이름 바꾸기")

3. 새 키를 추가 `ApplicationGroupContainerIdentifier` 에 **Root.plist** 에 구성한 경우 (예: 앱 그룹에 설정 된 값을 사용 하 여 `group.com.xamarin.WatchSettings` 예제의):

   [ ![](settings-images/settings-appgroup-sml.png "root.plist ApplicationGroupContainerIdentifier 키를 추가 합니다.")](settings-images/settings-appgroup.png#lightbox)

4. 편집 된 **Settings-Watch.bundle/Root.plist** -사용 하려는 옵션을 포함 하도록 템플릿 파일 그룹을 포함 합니다.
  텍스트 필드, 토글 스위치 및 슬라이더 기본적으로 (삭제 하 고 사용자 고유의 설정으로 대체):

  [![](settings-images/rootplist-sml.png "Settings-Watch.bundle/Root.plist 편집")](settings-images/rootplist.png#lightbox)


## <a name="use-settings-in-the-watch-app"></a>Watch 앱의 설정 사용

사용자가 선택한 값에 액세스 하려면 만듭니다는 `NSUserDefaults` 앱 그룹을 사용 하 고 지정 인스턴스 `NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch 앱

[![](settings-images/settings-app-sml.png "IPhone에 새 Apple Watch 앱")](settings-images/settings-app.png#lightbox)

사용자가 새 통해 설정을 사용 하 여 상호 작용 **Apple Watch** 자신의 iPhone 앱. 이 앱 숨기기를 시청 하 고, 또한 설정을 사용 하 여 노출 하는 편집에서 앱을 사용 하면 합니다 **설정을 Watch.bundle**합니다.

![](settings-images/applewatch-1.png "앱 설정의 예") ![](settings-images/applewatch-2.png "앱 설정의 예")



## <a name="related-links"></a>관련 링크

- [Apple의 설정 문서](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
