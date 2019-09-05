---
title: Xamarin.ios의 대체 앱 아이콘
description: 이 문서에서는 Xamarin.ios에서 대체 앱 아이콘을 사용 하는 방법을 설명 합니다. 이러한 아이콘을 Xamarin.ios 프로젝트에 추가 하는 방법, info.plist 파일을 수정 하는 방법 및 프로그래밍 방식으로 앱 아이콘을 관리 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: b15b39460b40bc2c9f993b3b0d9bca3275ac7644
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286801"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Xamarin.ios의 대체 앱 아이콘

_이 문서에서는 Xamarin.ios에서 대체 앱 아이콘을 사용 하는 방법을 설명 합니다._

Apple은 앱의 아이콘 관리를 허용 하는 iOS 10.3에 대 한 몇 가지 향상 된 기능을 추가 했습니다.

- `ApplicationIconBadgeNumber`-Springboard에서 앱 아이콘의 배지를 가져오거나 설정 합니다.
- `SupportsAlternateIcons`-앱 `true` 에 대체 아이콘 집합이 있는 경우입니다.
- `AlternateIconName`-현재 선택한 대체 아이콘의 이름을 반환 하거나 `null` 기본 아이콘을 사용 하는 경우을 반환 합니다.
- `SetAlternameIconName`-이 메서드를 사용 하 여 앱의 아이콘을 지정 된 대체 아이콘으로 전환할 수 있습니다.

![](alternate-app-icons-images/icons04.png "앱이 해당 아이콘을 변경 하는 경우의 샘플 경고")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Xamarin.ios 프로젝트에 대체 아이콘 추가

앱이 대체 아이콘으로 전환 될 수 있도록 하려면 Xamarin.ios 앱 프로젝트에 아이콘 이미지 컬렉션을 포함 해야 합니다. 이러한 이미지는 일반적인 `Assets.xcassets` 메서드를 사용 하 여 프로젝트에 추가할 수 없습니다. **리소스** 폴더에 직접 추가 해야 합니다.

다음을 수행합니다.

1. 폴더에서 필수 아이콘 이미지를 선택 하 고 모두를 선택한 다음 **솔루션 탐색기**의 **Resources** 폴더로 끕니다.

    ![](alternate-app-icons-images/icons00.png "폴더에서 아이콘 이미지 선택")

2. 메시지가 표시 되 면 **복사**를 선택 하 고 **선택한 모든 파일에 대해 동일한 작업을 사용** 하 고 **확인** 단추를 클릭 합니다.

    ![](alternate-app-icons-images/icons02.png "폴더에 파일 추가 대화 상자")

3. 완료 되 면 **Resources** 폴더는 다음과 같이 표시 됩니다.

    ![](alternate-app-icons-images/icons01.png "리소스 폴더는 다음과 같습니다.")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Info.plist 파일 수정

**리소스** 폴더에 필요한 이미지가 추가 되 면 [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) 키를 프로젝트의 **info.plist** 파일에 추가 해야 합니다. 이 키는 새 아이콘의 이름과이를 구성 하는 이미지를 정의 합니다.

다음을 수행합니다.

1. **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 편집용으로 엽니다.
2. **원본** 뷰로 전환 합니다.
3. **번들 아이콘** 키를 추가 하 고 해당 **형식을** **사전**으로 설정 된 상태로 둡니다.
4. 키를 `CFBundleAlternateIcons` 추가 하 고 **형식을** **사전**으로 설정 합니다.
5. 키를 `AppIcon2` 추가 하 고 **형식을** **사전**으로 설정 합니다. 새 대체 앱 아이콘 집합의 이름입니다.
6. 키를 추가 하 고 형식을 **배열로** 설정 합니다. `CFBundleIconFiles`
7. 확장 `CFBundleIconFiles` `100_icon`및, 등`@3x`의 접미사를 남기고 각 아이콘 파일의 배열에 새 문자열을 추가 합니다 (예:). `@2x` 대체 아이콘 집합을 구성 하는 모든 파일에 대해이 단계를 반복 합니다.
8. 사전에 키를 추가 하 고, 형식을 부울로 설정 하 고, 값을 아니요로 설정 합니다. `UIPrerenderedIcon` `AppIcon2`
9. 파일의 변경 내용을 저장합니다.

완료 되 면 결과 **info.plist** 파일이 다음과 같이 표시 됩니다.

![](alternate-app-icons-images/icons03.png "완료 된 info.plist 파일입니다.")

또는 텍스트 편집기에서 연 경우 다음과 같이 합니다.

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

## <a name="managing-the-apps-icon"></a>앱의 아이콘 관리 

Xamarin.ios 프로젝트에 포함 된 아이콘 이미지와 **info.plist** 파일이 올바르게 구성 된 상태에서 개발자는 iOS 10.3에 추가 된 많은 새 기능 중 하나를 사용 하 여 앱의 아이콘을 제어할 수 있습니다.

개발자 `SupportsAlternateIcons` 는 `UIApplication` 클래스의 속성을 사용 하 여 응용 프로그램에서 대체 아이콘을 지원 하는지 여부를 확인할 수 있습니다. 예를 들어:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

개발자 `ApplicationIconBadgeNumber` 는 `UIApplication` 클래스의 속성을 사용 하 여 Springboard에서 앱 아이콘의 현재 배지 번호를 가져오거나 설정할 수 있습니다. 기본값은 영(0)입니다. 예를 들어:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

개발자 `AlternateIconName` 는 `UIApplication` 클래스의 속성을 사용 하 여 현재 선택 된 대체 앱 아이콘의 이름을 가져올 수 있으며, 앱 `null` 에서 기본 아이콘을 사용 하는 경우에는를 반환 합니다. 예를 들어:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

개발자는 `UIApplication` 클래스의 속성을사용하여앱아이콘을변경할수있습니다.`SetAlternameIconName` 아이콘의 이름을 전달 하 여 또는 `null` 를 선택 하 여 기본 아이콘으로 돌아갑니다. 예를 들어:

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

앱이 실행 되 고 사용자가 대체 아이콘을 선택 하는 경우 다음과 같은 경고가 표시 됩니다.

![](alternate-app-icons-images/icons04.png "앱이 해당 아이콘을 변경 하는 경우의 샘플 경고")

사용자가 기본 아이콘으로 다시 전환 하는 경우 다음과 같은 경고가 표시 됩니다.

![](alternate-app-icons-images/icons05.png "앱이 기본 아이콘으로 변경 되는 경우의 샘플 경고")

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 프로젝트에 대체 앱 아이콘을 추가 하 고 앱 내에서 사용 하는 방법에 대해 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [iOSTenThree 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
