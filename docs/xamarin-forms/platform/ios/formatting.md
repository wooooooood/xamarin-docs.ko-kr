---
title: 추가 iOS 별 서식 지정
description: 이 문서에서는 Xamarin.Forms 사용자 지정 렌더러를 사용 하지 않고 iOS 별 모양을 설정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 3b8a440617dedfbe23f869e865b3cedae21d6c5b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946433"
---
# <a name="adding-ios-specific-formatting"></a>추가 iOS 별 서식 지정

만들려는 iOS 관련 설정 하는 한 가지 방법은 서식 지정은 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) 컨트롤 집합의 플랫폼별 스타일 및 각 플랫폼에 대 한 색에 대 한 합니다.

Xamarin.Forms iOS 앱의 모양을 포함 하는 방법을 제어할 수 다른 옵션:

* 구성에서 옵션이 표시 됩니다 [ **Info.plist**](#info-plist)
* 통해 컨트롤 스타일을 설정 합니다 [ `UIAppearance` API](#uiappearance)

이러한 대체는 아래 설명 되어 있습니다.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Info.plist를 사용자 지정

합니다 **Info.plist** 파일로 iOS 응용 프로그램의 렌더링, 상태 표시줄 표시 됩니다 하는 방법 (및 여부) 등의 일부 측면을 구성할 수 있습니다.

예를 들어 합니다 [Todo 샘플](https://developer.xamarin.com/samples/xamarin-forms/Todo/) 다음 코드를 사용 하 여 모든 플랫폼에서 탐색 모음의 배경색과 텍스트 색을 설정 합니다.

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

결과 아래 화면 코드 조각에 표시 됩니다. 상태 표시줄의 항목은 검은색 (이렇게 설정할 수 없습니다 Xamarin.Forms 내의 플랫폼별 기능 이므로).

![](theme-images/status-default-sml.png "iOS 테마 설정")

이상적으로 상태 표시줄에도 되는 흰색-iOS 프로젝트에서 직접 수행할 수 있는 것 것입니다. 다음 항목을 추가 합니다 **Info.plist** 흰색 상태 표시줄을 적용할:

![](theme-images/info-plist.png "iOS Info.plist 항목")

또는 해당 편집 **Info.plist** 직접 포함 하도록 파일:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

이제 앱을 실행 하는 경우 탐색 막대는 녹색 텍스트는 흰색 (Xamarin.Forms 서식) 때문 *및* 상태 표시줄 텍스트 흰색 iOS 별 구성 덕분 이기도 합니다.

![](theme-images/status-white-sml.png "iOS 테마 설정")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

합니다 [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) 많은 iOS 컨트롤의 시각적 속성을 설정 하는 데 사용 될 *없이* 만들 필요는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)합니다.

코드를 단일 줄을 추가 합니다 **AppDelegate.cs** `FinishedLaunching` 메서드는 사용 하 여 지정 된 형식의 모든 컨트롤 스타일 지정할 수 있습니다 해당 `Appearance` 속성입니다. 다음 코드에 전역적으로 탭의 스타일을 지정 하는 두 가지 예제-가로 막대형 차트 및 제어를 전환 합니다.

**AppDelegate.cs** iOS 프로젝트에서

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>UITabBar

기본적으로 선택된 된 탭 표시줄 아이콘을 [`TabbedPage`](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
파란색 것입니다.

![](theme-images/tabbar-default.png "기본 iOS TabbedPage에 탭 표시줄 아이콘")

이 동작을 변경 하려면 설정의 `UITabBar.Appearance` 속성:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

이렇게 하면 선택한 탭 녹색 이어야 합니다.

![](theme-images/tabbar-custom.png "녹색 iOS TabbedPage에 탭 표시줄 아이콘")

이 API를 사용 하면 Xamarin.Forms의 모양을 사용자 지정할 수 있습니다 `TabbedPage` 거의 코드를 사용 하 여 iOS에서. 참조 된 [사용자 지정 탭 작성법](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) 사용자 지정 렌더러를 사용 하 여 탭에 대 한 특정 글꼴 설정에 대 한 자세한 내용은 합니다.

### <a name="uiswitch"></a>UISwitch

`Switch` 컨트롤은 쉽게 스타일 지정할 수 있습니다.

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

이러한 두 화면 캡처 기본값을 보여 줍니다 `UISwitch` 왼쪽에 사용자 지정된 된 버전 제어 (설정을 `Appearance`)에서 오른쪽에는 [Todo 샘플](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "기본 색 UISwitch") ![](theme-images/switch-custom.png "UISwitch 색을 사용자 지정")

### <a name="other-controls"></a>다른 컨트롤

여러 iOS 사용자 인터페이스 컨트롤 해당 기본 색 및 기타 특성을 사용 하 여 설정할 수는 [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)합니다.



## <a name="related-links"></a>관련 링크

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [탭을 사용자 지정](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
