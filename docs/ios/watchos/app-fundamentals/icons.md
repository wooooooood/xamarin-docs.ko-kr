---
title: WatchOS에서 Xamarin 아이콘 사용
description: 이 문서는 watchOS 응용 프로그램 및 이러한 아이콘을 포함 하는 솔루션을 설정 하는 방법에 필요한 다양 한 아이콘을 설명 합니다.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/26/2018
ms.openlocfilehash: e46ecc9d78ccc5dcfbe571c9ec5350fe6c391b7e
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275926"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>WatchOS에서 Xamarin 아이콘 사용

Apple Watch 솔루션 아이콘을 두 가지 집합이 필요합니다.

* IPhone에서 표시 되는 iOS 앱 아이콘입니다.
* 보기 메뉴의 원에 알림 화면에 렌더링 되는 Apple Watch 아이콘입니다. Watch 앱 아이콘에도 표시 됩니다는 [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS 앱입니다.

## <a name="apple-watch-icons"></a>Apple Watch 아이콘

| | | |
|-|-|-|
|iOS 앱 아이콘|IPhone에서 표시 되 고 부모 앱 시작|![iOS 앱 아이콘](icons-images/icon-ios.png)|
|Watch 앱 아이콘|Apple Watch 홈 화면에 표시 됩니다.|![watchOS 앱 아이콘](icons-images/icon-home.png)|
||조사식 알림에 표시 됩니다.|![watchOS 알림 아이콘](icons-images/notification-icon.png)|
||에 표시 되는 [iOS Apple Watch 앱](~/ios/watchos/app-fundamentals/settings.md)|![iOS Watch 앱 아이콘](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>솔루션 구성

IOS 앱 및 watch 앱 모두 올바른 이름 및 아이콘을 표시 하도록 각 프로젝트에 대 한 다음이 지침을 따릅니다.

### <a name="ios-app"></a>iOS 앱

참조 된 [iOS 응용 프로그램 아이콘 가이드](~/ios/app-fundamentals/images-icons/app-icons.md) iOS 앱의 아이콘은 올바르게 구성 되도록 합니다.

#### <a name="infoplist"></a>Info.plist

Watch 앱 옆에 표시 되는 문자열을 [Apple Watch 설정 앱](~/ios/watchos/app-fundamentals/settings.md) 에서 구성 됩니다 합니다 **iOS 앱의 Info.plist**.

있는지 확인 하 **Info.plist** 에 `CFBundleName` 키와 값 (참고:이 다릅니다는 `CFBundleDisplayName`, 모두 포함할 수):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch 앱

한 번에 [부모 앱](~/ios/watchos/app-fundamentals/parent-app.md) watch 앱에 응용 프로그램 아이콘 자산 카탈로그를 추가 해야 해당 아이콘에 구성 합니다.

1. Watch 앱 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **파일 > 추가 > 새 파일... > iOS > 자산 카탈로그** 프로젝트에 자산 카탈로그를 추가 합니다.

 ![](icons-images/newasset.png "프로젝트에 자산 카탈로그 추가")

2. 두 번 클릭 합니다 **AppIcon.appiconset/Contents.json** 파일

  ![](icons-images/xcassets-iconset-sml.png "AppIcon 콘텐츠")

3. 이 스크린샷에 표시 된 대로 모든 watchOS 이미지를 추가 합니다.

  [![](icons-images/appicons-sml.png "이 스크린샷에 표시 된 대로 모든 watchOS 이미지 추가")](icons-images/appicons.png#lightbox)

  가리킵니다 [Apple 아이콘 지침](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/menu-icons/) 필요한 크기 (화면의 크기 같습니다). 이러한 아이콘 원으로 렌더링에 자동으로 클리핑됩니다를 기억 합니다.

  아이콘 목록에는 다음과 같이 표시 됩니다.

  ![](icons-images/xcassets-complete-sml.png "솔루션 탐색기에서 아이콘 목록")

4. 자산 카탈로그 앱에 포함 된을 보장 하려면 다음 키를 추가 하 고 값을 **Watch 앱의 Info.plist**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcon.appiconset</string>
```

아이콘을 선택 하 여 올바른 구성 된 것을 확인할 수 있습니다 합니다 [Apple Watch 설정 앱](~/ios/watchos/app-fundamentals/settings.md) iPhone 시뮬레이터에서에서 또는 생성을 [알림](~/ios/watchos/platform/notifications.md) 알림을 표시 아이콘을 확인 하 고 화면입니다.

> [!NOTE]
> 아이콘에 알파 채널이 (앱 거부 됩니다 앱 스토어 제출 하는 동안에 알파 채널이 있는 경우)를 사용할 수 없습니다. 알파 채널이 있는 및 제거 하는 경우 확인할 수 있습니다 [미리 보기 앱을 사용 하 여 Mac OS X에서](~/ios/watchos/troubleshooting.md#noalpha)합니다.


## <a name="related-links"></a>관련 링크

- [Apple의 watchOS 아이콘 및 이미지 가이드](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/)
