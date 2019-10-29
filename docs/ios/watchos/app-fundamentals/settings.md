---
title: Xamarin에서 watchOS 설정 작업
description: 이 문서에서는 Xamarin에서 watchOS 설정으로 작업 하는 방법을 설명 합니다. 응용 프로그램의 설정 및 iPhone의 Apple Watch 앱을 사용 하 여 watch 앱 솔루션에 설정을 추가 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 1a59979b164c5200a96343caa1a44e05992763d0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028382"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>Xamarin에서 watchOS 설정 작업

Apple Watch 앱은 iOS 앱과 동일한 설정 기능을 사용할 수 있습니다. 설정 사용자 인터페이스는 **Apple Watch** iphone 앱에 표시 되지만 iphone 앱과 조사식 확장 모두에서 값에 액세스할 수 있습니다.

![](settings-images/intro.png "Apple Watch apps can use the same Settings functionality as iOS apps")

설정은 **앱 그룹**에서 정의한 iOS 앱 및 watch 앱 확장 모두에서 액세스할 수 있는 공유 파일 위치에 저장 됩니다. 아래 지침을 사용 하 여 설정을 추가 하기 전에 [앱 그룹을 구성](~/ios/watchos/app-fundamentals/app-groups.md) 해야 합니다.

## <a name="add-settings-in-a-watch-solution"></a>조사식 솔루션에 설정 추가

솔루션의 **iPhone 앱** (시청 앱 또는 확장이*아님* ):

1. **> 새 파일 추가** ...를 마우스 오른쪽 단추로 클릭 하 고 **설정. 번들** 을 선택 합니다 ( **새 파일** 대화 상자에서 이름을 편집할 수 없음).

   [![](settings-images/settings-add-sml.png "Add a new Settings Bundle")](settings-images/settings-add.png#lightbox)

2. 이름을 **설정-Watch. 번들** 로 변경 합니다 (이름을 바꾸려면 **명령 + R** 을 입력 합니다.).

   ![](settings-images/settings-rename.png "Rename the bundle")

3. 구성 된 앱 그룹으로 설정 된 값을 사용 하 여 **info.plist** 에 새 키 `ApplicationGroupContainerIdentifier`를 추가 합니다 (예: 샘플의 `group.com.xamarin.WatchSettings`):

   [![](settings-images/settings-appgroup-sml.png "Add a ApplicationGroupContainerIdentifier key to the Root.plist")](settings-images/settings-appgroup.png#lightbox)

4. 사용 하려는 옵션을 포함 하도록 **Settings-Watch/info.plist** 를 편집 합니다. 템플릿 파일에 그룹이 포함 되어 있습니다.
  textfield, 기본적으로 스위치와 슬라이더를 설정/해제 합니다. 사용자 고유의 설정으로 삭제 하 고 바꿀 수 있습니다.

  [![](settings-images/rootplist-sml.png "Edit the Settings-Watch.bundle/Root.plist")](settings-images/rootplist.png#lightbox)

## <a name="use-settings-in-the-watch-app"></a>시청 앱에서 설정 사용

사용자가 선택한 값에 액세스 하려면 앱 그룹을 사용 하 여 `NSUserDefaults` 인스턴스를 만들고 `NSUserDefaultsType.SuiteName`를 지정 합니다.

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch 앱

[![](settings-images/settings-app-sml.png "The new Apple Watch app on the iPhone")](settings-images/settings-app.png#lightbox)

사용자는 iPhone에서 새로운 **Apple Watch** 앱을 통해 설정과 상호 작용 합니다. 이 앱을 사용 하면 사용자가 조사식에 앱을 표시 하거나 숨길 수 있으며, **설정-watch. 번들**을 사용 하 여 노출 된 설정을 편집할 수도 있습니다.

![](settings-images/applewatch-1.png "앱 설정의 예") ![](settings-images/applewatch-2.png "앱 설정의 예")

## <a name="related-links"></a>관련 링크

- [Apple의 설정 문서](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
