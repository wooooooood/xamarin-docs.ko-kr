---
title: "작업 아이콘"
ms.topic: article
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 98cd780a29abdbeaab02483e4b6ed01a218f88e5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-icons"></a>작업 아이콘

Apple Watch 솔루션 아이콘을 두 개의 집합이 필요합니다.

* IPhone에 표시 되는 iOS 앱 아이콘
* 조사식 메뉴에 원이 및 알림 화면 렌더링할 수 있는 Apple Watch 아이콘입니다. 조사식 앱 아이콘에도 나타납니다.는 [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) iOS 앱.

## <a name="apple-watch-icons"></a>Apple Watch 아이콘

<table align="center" border="1" cellpadding="1" cellspacing="1">
    <tr>
      <td valign="top">
        <b>iOS 응용 프로그램 아이콘</b>
      </td>
      <td valign="top">
IPhone에 나타나고 부모 응용 프로그램이 시작 </td>
      <td>
        <img src="icons-images/icon-ios.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top" rowspan="3">
        <b>조사식 앱 아이콘</b>
      </td>
      <td valign="top">
Apple Watch 홈 화면에 표시 </td>
      <td>
        <img src="icons-images/icon-home.png" class="tableimg" />
      </td>
    </tr>
    <tr>
      <td valign="top">
조사식 알림만 표시 됩니다. </td>
      <td>
        <img src="icons-images/notification-icon.png" class="tableimg" />
      </td>
    </tr>
    <tr>
      <td valign="top">
에 표시 된 <a href="~/ios/watchos/app-fundamentals/settings.md">iOS Apple Watch 앱</a>
      </td>
      <td>
        <a href="icons-images/watch-app.png">
          <img src="icons-images/watch-app-sml.png" class="tableimg">
        </a>
      </td>
    </tr>
    <tbody>
</table>



## <a name="configuring-your-solution"></a>솔루션 구성

IOS 앱 및 watch 앱 올바른 이름 및 아이콘을 표시 하려면 각 프로젝트에 대 한 다음이 지침을 따르십시오.

### <a name="ios-app"></a>iOS 앱

참조는 [iOS 응용 프로그램 아이콘 안내](~/ios/app-fundamentals/images-icons/app-icons.md) iOS 앱의 아이콘은 올바르게 구성 합니다.

#### <a name="infoplist"></a>Info.plist

조사식 응용 프로그램 옆에 표시 되는 문자열의 [Apple Watch 설정 앱](~/ios/watchos/app-fundamentals/settings.md) 에 구성 된는 **iOS 앱 Info.plist**합니다.

있는지 확인 프로그램 **Info.plist** 에 `CFBundleName` 키와 값 (참고:이 다른 받는 `CFBundleDisplayName`, 둘 다 가질 수):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Apple Watch 앱

한 번에 [부모 앱](~/ios/watchos/app-fundamentals/parent-app.md) watch 앱에 응용 프로그램 아이콘 자산 카탈로그를 추가 해야이 아이콘을 구성 합니다.

1. 조사식 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **파일 > 추가 > 새 파일... > iOS > 자산 카탈로그** 자산 카탈로그의 프로젝트에 추가 하려면.

 ![](icons-images/newasset.png "자산 카탈로그의 프로젝트에 추가")

2. 두 번 클릭 하 고 **AppIcons.appiconset/Contents.json** 파일

  ![](icons-images/xcassets-iconset-sml.png "AppIcons 내용")

3. 이 스크린 샷에 표시 된 대로 모든 watchOS 이미지를 추가 합니다.

  [![](icons-images/appicons-sml.png "이 스크린 샷에 표시 된 것 처럼 모든 watchOS 이미지 추가")](icons-images/appicons.png#lightbox)

  참조 [Apple의 아이콘 지침](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html) (크기는도 화면에 표시 됨) 필요한 크기에 대 한 합니다. 이러한 아이콘은 원 안에 렌더링 자동으로 잘릴 수 기억 합니다.

  아이콘 목록 코드는 다음과 같아야 합니다.

  ![](icons-images/xcassets-complete-sml.png "솔루션 탐색기에서 아이콘 목록")

4. 자산 카탈로그의 응용 프로그램에 포함 된을 보장 하려면 다음 키를 추가 하 고 값을 **Watch 앱 Info.plist**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcons.appiconset</string>
```

아이콘은 올바른 검사 하 여 구성 작업을 확인할 수 있습니다는 [Apple Watch 설정 앱](~/ios/watchos/app-fundamentals/settings.md) iPhone 시뮬레이터에서에서 또는 생성 한 [알림](~/ios/watchos/platform/notifications.md) 알림 표시 아이콘을 확인 하 고 화면입니다.

> [!NOTE]
> **참고**: 아이콘 (앱 거부 되며 앱 스토어 제출 하는 동안 알파 채널이 있는 경우) 알파 채널이 있을 수 없습니다. 알파 채널 존재 하 고 제거 하는 경우를 확인할 수 있습니다 [미리 보기 앱을 사용 하 여 Mac OS x](~/ios/watchos/troubleshooting.md#noalpha)합니다.


## <a name="related-links"></a>관련 링크

- [Apple의 아이콘 및 이미지 안내](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)
