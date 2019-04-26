---
title: Xamarin.iOS에서 대체 앱 아이콘
description: 이 문서에서는 Xamarin.iOS에서 대체 앱 아이콘을 사용 하는 방법을 설명 합니다. Xamarin.iOS 프로젝트에 이러한 아이콘을 추가 하는 방법, Info.plist 파일을 수정 하는 방법 및 앱의 아이콘을 프로그래밍 방식으로 관리 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: cc5052c8988a27605cf7680a3853f80e7afd38b7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61171170"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Xamarin.iOS에서 대체 앱 아이콘

_이 문서에서는 Xamarin.iOS에서 대체 앱 아이콘을 사용 하 여 설명 합니다._

Apple이 앱을 해당 아이콘을 관리 하는 iOS 10.3 몇 가지 향상 된 기능 추가.

 - `ApplicationIconBadgeNumber` -앱 아이콘 배지를 Springboard에서 설정 하거나 가져옵니다.
 - `SupportsAlternateIcons` - `true` 앱 아이콘 집합을 대체 했습니다.
 - `AlternateIconName` -현재 선택한 대체 아이콘의 이름을 반환 합니다. 또는 `null` 기본 아이콘을 사용 하는 경우.
 - `SetAlternameIconName` -지정 된 대체 아이콘 앱 아이콘을 전환 하려면이 메서드를 사용 합니다.

![](alternate-app-icons-images/icons04.png "샘플 경고를 앱 아이콘을 변경 하는 경우")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>대체 아이콘 Xamarin.iOS 프로젝트에 추가

대체 아이콘으로 전환 하는 앱을 허용 하려면 Xamarin.iOS 앱 프로젝트에 포함 될 아이콘 이미지의 컬렉션 할 수 있습니다. 이러한 이미지를 사용 하 여 일반적인 프로젝트에 추가할 수 없습니다 `Assets.xcassets` 메서드를 추가 해야 하는 **리소스** 직접 폴더입니다.

다음을 수행합니다.

1. 폴더에 필요한 아이콘 이미지를 선택 하 고, 모두 선택,로 합니다 **리소스** 폴더에는 **솔루션 탐색기**:

    ![](alternate-app-icons-images/icons00.png "폴더에서 아이콘 이미지 선택")

2. 메시지가 표시 되 면 선택 **복사**, **동일한 작업을 사용 하 여 선택한 모든 파일에 대 한** 을 클릭 합니다 **확인** 단추:

    ![](alternate-app-icons-images/icons02.png "폴더 대화 상자에 파일 추가")

3. 합니다 **리소스** 폴더 완료 되 면 다음과 같이 표시 됩니다.

    ![](alternate-app-icons-images/icons01.png "리소스 폴더 다음과 같아야 합니다.")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Info.plist 파일 수정

에 추가 하 고 필요한 이미지를 사용 하 여는 **리소스** 폴더를 [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) 키를 프로젝트에 추가 해야 **Info.plist** 파일입니다. 이 키를 새 아이콘 및 구성 하는 이미지의 이름을 정의 합니다.

다음을 수행합니다.

1. **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 편집용으로 엽니다.
2. 으로 전환 합니다 **원본** 보기.
3. 추가 **아이콘을 번들** 두고 키를 **유형** 로 설정 **사전**합니다.
4. 추가 `CFBundleAlternateIcons` 키를 설정 합니다 **형식** 하 **사전**합니다.
5. 추가 `AppIcon2` 키를 설정 합니다 **형식** 하 **사전**합니다. 이 새 대체 앱 아이콘 집합의 이름이 됩니다.
6. 추가 `CFBundleIconFiles` 키를 설정 합니다 **형식** 하려면 **배열**
7. 새 문자열을 추가 합니다 `CFBundleIconFiles` 확장을 제외 하는 각 아이콘 파일에 대 한 배열 및 `@2x`, `@3x`, 등 접미사 (예제 `100_icon`). 대체 아이콘 집합을 구성 하는 모든 파일에 대해이 단계를 반복 합니다.
8. 추가 `UIPrerenderedIcon` 키를 `AppIcon2` 사전 설정를 **형식** 에 **부울** 값을 **아니요**합니다.
9. 파일의 변경 내용을 저장합니다.

결과 **Info.plist** 파일 완료 되 면 다음과 같이 표시 됩니다.

![](alternate-app-icons-images/icons03.png "완료 된 Info.plist 파일")

또는이 경우 같은 텍스트 편집기에서 열립니다.

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon" />

## <a name="managing-the-apps-icon"></a>관리 앱의 아이콘 

Xamarin.iOS 프로젝트에 포함 된 아이콘 이미지를 사용 하 여 하며 **Info.plist** 올바르게 구성 파일인 개발자 중 하나를 사용 iOS 10.3에 추가 하는 여러 가지 새로운 기능이 응용 프로그램의 아이콘을 제어할 수 있습니다.

합니다 `SupportsAlternateIcons` 의 속성을 `UIApplication` 클래스를 사용 하면 앱에서 다른 아이콘을 지원 하는지 확인 하려면 개발자. 예를 들어:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber` 의 속성을 `UIApplication` 클래스를 사용 하면 개발자가 있고 springboard 앱 아이콘 배지 수가 현재 설정할 수 있습니다. 기본값은 영(0)입니다. 예를 들어:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

합니다 `AlternateIconName` 의 속성을 `UIApplication` 클래스를 사용 하면 개발자가 현재 선택 된 대체 앱 아이콘의 이름을 가져올 수 없거나이 반환 `null` 앱에 기본 아이콘을 사용 하는 경우. 예를 들어:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

합니다 `SetAlternameIconName` 의 속성을 `UIApplication` 클래스를 사용 하면 앱 아이콘을 변경 하려면 개발자. 선택 하기 위한 아이콘의 이름을 전달 또는 `null` 기본 아이콘을 반환 합니다. 예를 들어:

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

앱이 실행 되 고 사용자의 대체 아이콘을 선택 하는 경우 다음과 같은 경고가 표시 됩니다.

![](alternate-app-icons-images/icons04.png "샘플 경고를 앱 아이콘을 변경 하는 경우")

기본 아이콘으로 다시 전환 하는 사용자를 경우 다음과 같은 경고가 표시 됩니다.

![](alternate-app-icons-images/icons05.png "샘플 경고는 앱의 기본 아이콘으로 변경 되 면")

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.iOS 프로젝트에 대체 앱 아이콘을 추가 하 고 앱 내에서 사용 검사가 수행 합니다.



## <a name="related-links"></a>관련 링크

- [iOSTenThree 샘플](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
