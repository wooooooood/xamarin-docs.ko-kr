---
title: "설정 작업"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 3ff8800f4e8690069f5394193d11552d917baffe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-settings"></a>설정 작업

Apple Watch 앱 iOS 앱으로 동일한 설정 기능을 사용할 수 있습니다-사용자 인터페이스에 표시 되는 **Apple Watch** iPhone 앱 하지만 값은 모두 iPhone 앱 그리고 조사식 확장에서 액세스할 수 있습니다.

![](settings-images/intro.png "Apple Watch 앱은 iOS 앱으로 동일한 설정 기능을 사용할 수 있습니다.")

설정을 iOS 앱과 조사식 응용 프로그램 확장에 의해 정의에 액세스할 수 있는 공유 파일 위치에 저장 됩니다는 **App 그룹**합니다. 수행 해야 [앱 그룹을 구성](~/ios/watchos/app-fundamentals/app-groups.md) 아래 지침을 사용 하 여 설정을 추가 하기 전에.

## <a name="add-settings-in-a-watch-solution"></a>조사식 솔루션에 설정을 추가합니다

에 **iPhone 앱** 솔루션에 (*하지* watch 앱 또는 확장):

1. 마우스 오른쪽 단추로 클릭 **추가 > 새 파일...**  선택 **Settings.bundle** (에서 이름을 편집할 수 없습니다는 **새 파일** 대화):

   [ ![](settings-images/settings-add-sml.png "새 설정 번들 추가")](settings-images/settings-add.png)

2. 이름을 변경한 **설정 Watch.bundle** (선택 하 고 입력 **명령 + R** 바꾸려면):

   ![](settings-images/settings-rename.png "번들 이름 바꾸기")

3. 새 키를 추가 `ApplicationGroupContainerIdentifier` 에 **Root.plist** 에 구성한 경우 않음 (예: 응용 프로그램 그룹에 설정 된 값과 `group.com.xamarin.WatchSettings` 샘플):

   [ ![](settings-images/settings-appgroup-sml.png "에 Root.plist ApplicationGroupContainerIdentifier 키 추가")](settings-images/settings-appgroup.png)

4. 편집 된 **Settings-Watch.bundle/Root.plist** -를 사용 하려는 옵션을 포함 하도록 템플릿 파일 그룹을 포함 합니다.
  텍스트 필드, 토글 스위치 및 슬라이더 기본적으로 (삭제 하 고 원하는 설정으로 바꿀 수):

  [ ![](settings-images/rootplist-sml.png "Settings-Watch.bundle/Root.plist 편집")](settings-images/rootplist.png)


## <a name="use-settings-in-the-watch-app"></a>조사식 응용 프로그램에서 설정 사용

사용자가 선택한 값에 액세스 하려면 만듭니다는 `NSUserDefaults` 앱 그룹을 사용 하 고 지정 인스턴스 `NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch 앱

[ ![](settings-images/settings-app-sml.png "IPhone에서 새로운 Apple Watch 앱")](settings-images/settings-app.png)

사용자가 새 통해 설정을 상호 작용 **Apple Watch** 응용 프로그램의 iPhone에 액세스 합니다. 이 앱을 사용 하면 표시/숨기기 앱 watch, 및도 편집 설정을 사용 하 여 노출 하는 **설정 Watch.bundle**합니다.

![](settings-images/applewatch-1.png "앱 설정의 예") ![ ] (settings-images/applewatch-2.png "앱 설정의 예")



## <a name="related-links"></a>관련 링크

- [Apple의 설정 문서](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
