---
title: IOS 전용 서식 추가
description: 이 문서에서는 Xamarin.Forms 사용자 지정 렌더러를 사용 하지 않고 iOS 특정 모양을 설정 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 74a3cdc340cb09e8adf15ed0dd09315c985d18b5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243534"
---
# <a name="adding-ios-specific-formatting"></a>IOS 전용 서식 추가

IOS 관련 설정 하는 한 가지 방법은 형식 지정 만드는 것을 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) 컨트롤 집합 플랫폼 특정 스타일 및 각 플랫폼에 대 한 색에 대 한 합니다.

Xamarin.Forms iOS 앱의 모양을 포함 하는 방식을 제어 하는 기타 옵션:

* 옵션 표시 구성 [ **Info.plist**](#info-plist)
* 통해 컨트롤 스타일 설정의 [ `UIAppearance` API](#uiappearance)

이러한 대체 방법 아래 설명 되어 있습니다.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Info.plist 사용자 지정

**Info.plist** 파일로 iOS 응용 프로그램의 렌더링 상태 표시줄이 표시 됩니다 (하는 방법을) 등의 일부 측면을 구성할 수 있습니다.

예를 들어는 [Todo 샘플](https://developer.xamarin.com/samples/xamarin-forms/Todo/) 다음 코드를 사용 하 여 모든 플랫폼에서 탐색 모음의 색과 텍스트 색을 설정 합니다.

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

결과 아래 화면 조각에 표시 됩니다. 상태 표시줄 항목은 검정 (이렇게 설정할 수 없습니다 Xamarin.Forms 내에서 플랫폼 관련 기능은 이기 때문에).

![](theme-images/status-default-sml.png "iOS 테마 설정")

이상적으로 상태 표시줄 수도 흰색-iOS 프로젝트에서 직접 수행할 수 있는 म 문제가 있습니다. 다음 항목을 추가 **Info.plist** 흰색 되도록 상태 표시줄을 강제로:

![](theme-images/info-plist.png "iOS Info.plist 항목")

또는 해당 편집 **Info.plist** 직접 포함 하도록 파일:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

탐색 모음 녹색, Xamarin.Forms 서식) (인해 흰색 텍스트는 응용 프로그램을 실행 하는 경우 이제 *및* 상태 표시줄 텍스트 iOS 관련 구성 수 덕분에 흰색 이기도 합니다.

![](theme-images/status-white-sml.png "iOS 테마 설정")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) 많은 iOS 컨트롤의 시각적 속성을 설정 하는 데 사용 될 *없이* 를 만들 필요는 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)합니다.

코드를 한 줄을 추가 **AppDelegate.cs** `FinishedLaunching` 메서드 스타일을 사용 하 여 지정 된 형식의 모든 컨트롤 수 자신의 `Appearance` 속성입니다. 다음 코드에 전체적으로 탭의 스타일을 지정 하는 두 가지 예-가로 막대형 차트 및 제어를 전환 합니다.

**AppDelegate.cs** iOS 프로젝트에

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

기본적으로 선택 된 탭 표시줄 아이콘에는 [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) 파란색 수:

![](theme-images/tabbar-default.png "기본 iOS TabbedPage 탭 표시줄 아이콘")

이 동작을 변경 하려면 설정는 `UITabBar.Appearance` 속성:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

이 녹색을 선택 된 탭을 생성 합니다.

![](theme-images/tabbar-custom.png "녹색 iOS TabbedPage 탭 표시줄 아이콘")

Xamarin.Forms는 모양을 사용자 지정할 수 있습니다이 API를 사용 하 여 `TabbedPage` 매우 적은 양의 코드로 ios입니다. 참조는 [사용자 지정 탭 레시피](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/) 탭에 대 한 특정 글꼴을 설정 하는 사용자 지정 렌더러를 사용 하 여 대 한 자세한 내용은 합니다.

### <a name="uiswitch"></a>UISwitch

`Switch` 컨트롤은 쉽게 스타일이 지정 될 수 있는 또 다른 예제:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

이러한 두 화면 캡처에는 기본 표시 `UISwitch` 왼쪽 및 사용자 지정 된 버전에 대 한 컨트롤 (설정을 `Appearance`)에서 오른쪽에는 [Todo 샘플](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "기본 UISwitch 색") ![ ] (theme-images/switch-custom.png "UISwitch 색 사용자 지정")

### <a name="other-controls"></a>다른 컨트롤

많은 iOS 사용자 인터페이스 컨트롤의 기본 색 및 기타 특성을 사용 하 여 설정 있을 수 있습니다는 [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)합니다.



## <a name="related-links"></a>관련 링크

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [탭 사용자 지정](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/)
