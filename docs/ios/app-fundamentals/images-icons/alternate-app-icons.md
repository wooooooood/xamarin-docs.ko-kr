---
title: Xamarin.iOS의 대체 앱 아이콘
description: 이 문서에서는 Xamarin.iOS에서 대체 앱 아이콘을 사용 하는 방법에 설명 합니다. 이러한 아이콘 Xamarin.iOS 프로젝트에 추가 하는 방법, Info.plist 파일을 수정 하는 방법 및 응용 프로그램의 아이콘을 프로그래밍 방식으로 관리 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 1d37a29982454367c35bfdfad205abce0eb025af
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784112"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Xamarin.iOS의 대체 앱 아이콘

_이 문서에서는 Xamarin.iOS에 대체 앱 아이콘을 사용 하 여 설명 합니다._

Apple이 iOS 10.3 해당 아이콘을 관리 하는 응용 프로그램을 사용할 수 있는 몇 가지 향상 된 기능 추가.

 - `ApplicationIconBadgeNumber` -응용 프로그램 아이콘의 배지는 Springboard에서 설정 하거나 가져옵니다.
 - `SupportsAlternateIcons` -If `true` 응용 프로그램에 다른 아이콘 집합입니다.
 - `AlternateIconName` -현재 선택 된 대체 아이콘의 이름이 반환 또는 `null` 기본 아이콘을 사용 하는 경우.
 - `SetAlternameIconName` -지정 된 대체 아이콘을 응용 프로그램의 아이콘을 전환 하려면이 메서드를 사용 합니다.

![](alternate-app-icons-images/icons04.png "응용 프로그램 아이콘을 변경 하는 경우 샘플 경고")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>대체 아이콘 Xamarin.iOS 프로젝트에 추가

대체 아이콘으로 전환 하는 응용 프로그램을 허용 하려면 아이콘 이미지의 컬렉션 Xamarin.iOS 앱 프로젝트에 포함 해야 합니다. 일반을 사용 하는 프로젝트에 이러한 이미지를 추가할 수 없습니다 `Assets.xcassets` 메서드를 추가 해야 하는 **리소스** 직접 폴더입니다.

다음을 수행합니다.

1. 폴더에 필요한 아이콘 이미지를 선택 하 고 모두 선택로 끌어 옵니다는 **리소스** 폴더에는 **솔루션 탐색기**:

    ![](alternate-app-icons-images/icons00.png "폴더에서 아이콘 이미지를 선택 합니다.")

2. 메시지가 나타나면 선택 **복사**, **동일한 작업을 사용 하 여 선택한 모든 파일에 대 한** 클릭는 **확인** 단추:

    ![](alternate-app-icons-images/icons02.png "폴더 대화 상자에 파일 추가")

3. **리소스** 폴더 완료 되 면 다음과 같이 표시 됩니다.

    ![](alternate-app-icons-images/icons01.png "Resources 폴더 다음과 같아야 합니다.")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Info.plist 파일 수정

필요한 이미지에 추가 **리소스** 폴더는 [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) 키는 프로젝트에 추가 해야 합니다 **Info.plist** 파일입니다. 이 키는 새 아이콘 및 구성 하는 이미지의 이름을 정의 합니다.

다음을 수행합니다.

1. **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 편집용으로 엽니다.
2. 전환 하는 **소스** 보기.
3. 추가 **아이콘 번들** 두고 키는 **형식** 로 설정 **사전**합니다.
4. 추가 `CFBundleAlternateIcons` 키에 추가한 설정는 **형식** 를 **사전**합니다.
5. 추가 `AppIcons2` 키에 추가한 설정는 **형식** 를 **사전**합니다. 이 새 대체 앱 아이콘 집합의 이름이 됩니다.
6. 추가 `CFBundleIconFiles` 키에 추가한 설정는 **형식** 를 **배열**
7. 새 문자열을 추가할는 `CFBundleIconFiles` 확장을 제외 하는 각 아이콘 파일에 대 한 배열 및 `@2x`, `@3x`, 등 접미사 (예 `100_icon`). 대체 아이콘 집합을 구성 하는 모든 파일에 대해이 단계를 반복 합니다.
8. 추가 `UIPrerenderedIcon` 키를 `AppIcons2` 사전 설정의 **형식** 를 **부울** 와 값을 **아니요**합니다.
9. 파일의 변경 내용을 저장합니다.

그 결과 **Info.plist** 완료 되 면 다음 파일은 같습니다.

![](alternate-app-icons-images/icons03.png "완료 된 Info.plist 파일")

또는이 경우에이 같은 텍스트 편집기에서 열립니다.

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

## <a name="managing-the-apps-icon"></a>관리 응용 프로그램의 아이콘 

Xamarin.iOS 프로젝트에 포함 된 아이콘 이미지와 및 **Info.plist** 올바르게 구성 파일을 개발자 중 하나를 사용 많은 새로운 기능이 추가 10.3 iOS 응용 프로그램의 아이콘을 제어할 수 있습니다.

`SupportsAlternateIcons` 의 속성은 `UIApplication` 클래스를 사용 하면 개발자가 응용 프로그램에서 다른 아이콘을 지원 하는지 확인 합니다. 예를 들어:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber` 속성은 `UIApplication` 클래스를 사용 하면 개발자는 Springboard에서 앱 아이콘의 현재 배지 번호를 설정 합니다. 기본값은 영(0)입니다. 예를 들어:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName` 의 속성은 `UIApplication` 클래스를 사용 하면 개발자가 현재 선택 된 대체 앱 아이콘의 이름을 가져올 수 없거나 반환 `null` 응용 프로그램의 기본 아이콘을 사용 하는 경우. 예를 들어:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName` 의 속성은 `UIApplication` 클래스를 사용 하면 개발자가 응용 프로그램 아이콘을 변경할 수 있습니다. 선택 하려면 아이콘의 이름을 전달 또는 `null` 기본 아이콘으로 돌아갑니다. 예를 들어:

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

앱이 실행 되는 사용자의 대체 아이콘 선택 하는 경우 다음과 같은 경고가 표시 됩니다.

![](alternate-app-icons-images/icons04.png "응용 프로그램 아이콘을 변경 하는 경우 샘플 경고")

사용자가 기본 아이콘으로 다시 전환 하는 경우 다음과 같은 경고가 표시 됩니다.

![](alternate-app-icons-images/icons05.png "응용 프로그램 기본 아이콘으로 변경 하는 경우 샘플 경고")

<a name="Summary" />

## <a name="summary"></a>요약

이 문서 Xamarin.iOS 프로젝트에 다른 응용 프로그램 아이콘을 추가 하 고 응용 프로그램 내부에서 사용 하 여 검사가 수행 됩니다.



## <a name="related-links"></a>관련 링크

- [iOSTenThree 샘플](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
