---
title: Xamarin에서 watchOS 아이콘 사용
description: 이 문서에서는 watchOS 응용 프로그램에 필요한 다양 한 아이콘과 이러한 아이콘을 포함 하는 솔루션을 설정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/26/2018
ms.openlocfilehash: e0bf9ec1553e6638398695157a11242b9885b168
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768105"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Xamarin에서 watchOS 아이콘 사용

Apple Watch 솔루션에는 두 개의 아이콘 집합이 필요 합니다.

- IPhone에 표시 되는 iOS 앱 아이콘입니다.
- 조사식 메뉴와 알림 화면의 원에서 렌더링 되는 아이콘을 Apple Watch 합니다. 앱 보기 아이콘은 [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS 앱에도 표시 됩니다.

## <a name="apple-watch-icons"></a>Apple Watch 아이콘

| | | |
|-|-|-|
|iOS 앱 아이콘|IPhone에 표시 되 고 부모 앱을 시작 합니다.|![iOS 앱 아이콘](icons-images/icon-ios.png)|
|응용 프로그램 보기 아이콘|Apple Watch 홈 화면에 표시 됩니다.|![watchOS 앱 아이콘](icons-images/icon-home.png)|
||보기 알림에 표시|![watchOS 알림 아이콘](icons-images/notification-icon.png)|
||[IOS Apple Watch 앱](~/ios/watchos/app-fundamentals/settings.md) 에 표시 됩니다.|![iOS 조사식 앱 아이콘](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>솔루션 구성

IOS 앱과 시청 앱 모두에 올바른 이름과 아이콘이 표시 되도록 하려면 각 프로젝트에 대 한 다음 지침을 따르세요.

### <a name="ios-app"></a>iOS 앱

Ios [응용 프로그램 아이콘 가이드](~/ios/app-fundamentals/images-icons/app-icons.md) 를 참조 하 여 ios 앱의 아이콘이 올바르게 구성 되었는지 확인 합니다.

#### <a name="infoplist"></a>Info.plist

[Apple Watch settings 앱](~/ios/watchos/app-fundamentals/settings.md) 에서 watch 앱 옆에 표시 되는 문자열은 **iOS 앱의 info.plist**에서 구성 됩니다.

`CFBundleDisplayName` **Info.plist** 에 키와 값이 있는지 확인 합니다 (참고:와는 다르며 둘 다를 가질 수 있음). `CFBundleName`

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch 앱

[부모 앱](~/ios/watchos/app-fundamentals/parent-app.md) 에 아이콘이 구성 된 후에는 응용 프로그램 아이콘 자산 카탈로그를 시청 앱에 추가 해야 합니다.

1. Watch 앱 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **파일 > 새 파일 > 추가 ...를 선택 합니다. iOS > Asset Catalog를 >** 하 여 자산 카탈로그를 프로젝트에 추가 합니다.

    ![](icons-images/newasset.png "프로젝트에 자산 카탈로그 추가")

2. **Appicons.appiconset/Contents** 파일을 두 번 클릭 합니다.

    ![](icons-images/xcassets-iconset-sml.png "AppIcon 내용")

3. 다음 스크린샷에 표시 된 것 처럼 모든 watchOS 이미지를 추가 합니다.

    [![](icons-images/appicons-sml.png "이 스크린샷에 표시 된 것 처럼 모든 watchOS 이미지를 추가 합니다.")](icons-images/appicons.png#lightbox)

    필요한 크기에 대 한 [Apple의 아이콘 지침](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/menu-icons/) (차원이 화면에도 표시 됨)을 참조 하세요. 이러한 아이콘은 원 안에 렌더링 하도록 자동으로 잘립니다.

    아이콘 목록이 다음과 같이 표시 됩니다.

    ![](icons-images/xcassets-complete-sml.png "솔루션 탐색기의 아이콘 목록")

4. 자산 카탈로그가 앱에 포함 되도록 하려면 **Watch 앱의 info.plist**에 다음 키와 값을 추가 합니다.

    ```xml
    <key>XSAppIconAssets</key>
    <string>Images.xcassets/AppIcon.appiconset</string>
    ```

IPhone 시뮬레이터에서 [Apple Watch 설정 앱](~/ios/watchos/app-fundamentals/settings.md) 을 확인 하거나 [알림을](~/ios/watchos/platform/notifications.md) 생성 하 고 알림 화면에 아이콘이 표시 되는지 확인 하 여 아이콘이 올바르게 구성 되었는지 확인할 수 있습니다.

> [!NOTE]
> 아이콘에는 알파 채널이 있을 수 없습니다. 알파 채널이 있으면 앱 스토어를 제출 하는 동안 앱이 거부 됩니다. 알파 채널이 있는지 확인 하 고 [Mac OS X의 미리 보기 앱을 사용 하 여](~/ios/watchos/troubleshooting.md#noalpha)제거할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Apple의 watchOS 아이콘 & 이미지 가이드](https://developer.apple.com/design/human-interface-guidelines/watchos/icons-and-images/)
