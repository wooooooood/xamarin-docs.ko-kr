---
title: Automation 속성
description: 이 문서에서는 화면 판독기에 페이지의 요소가 표시될 수 있도록 Xamarin.Forms 애플리케이션에서 AutomationProperties 클래스를 사용하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: 12c6229c1922f0bd4a4d25ca796bcb46141a326c
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "77131134"
---
# <a name="automation-properties-in-xamarinforms"></a>Xamarin.Forms의 Automation 속성

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)

_Xamarin.Forms를 사용하면 AutomationProperties 클래스에서 연결된 속성을 통해 액세스 가능성 값을 사용자 인터페이스 요소에 설정할 수 있으며 이어 기본 액세스 가능성 값을 설정합니다. 이 문서에서는 화면 판독기에 페이지의 요소가 표시될 수 있도록 AutomationProperties 클래스를 사용하는 방법을 설명합니다._

Xamarin.Forms를 사용하면 자동화 속성을 다음과 같은 연결된 속성을 통해 사용자 인터페이스 요소에 설정할 수 있습니다.

- `AutomationProperties.IsInAccessibleTree` – 요소를 액세스 가능한 애플리케이션에 사용할 수 있는지 나타냅니다. 자세한 내용은 [AutomationProperties.IsInAccessibleTree](#isinaccessibletree)를 참조하세요.
- `AutomationProperties.Name` – 요소에 대해 speakable 식별자로 사용되는 요소에 대한 간단한 설명입니다. 자세한 내용은 [AutomationProperties.Name](#name)을 참조하세요.
- `AutomationProperties.HelpText` – 요소와 연결된 도구 설명 텍스트로 간주될 수 있는 요소에 대한 긴 설명입니다. 자세한 내용은 [AutomationProperties.HelpText](#helptext)를 참조하세요.
- `AutomationProperties.LabeledBy` – 다른 요소가 현재 요소에 대한 액세스 가능성 정보를 정의하게 할 수 있습니다. 자세한 내용은 [AutomationProperties.LabeledBy](#labeledby)를 참조하세요.

이러한 연결 속성은 화면 판독기에 요소가 표시될 수 있도록 기본 액세스 가능성 값을 설정합니다. 연결된 속성에 대한 자세한 내용은 [연결된 속성](~/xamarin-forms/xaml/attached-properties.md)를 참조하세요.

> [!IMPORTANT]
> `AutomationProperties` 연결된 속성을 사용하면 Android에서 UI 테스트 실행에 영향을 줄 수 있습니다. [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId), `AutomationProperties.Name` 및 `AutomationProperties.HelpText` 속성은 `AutomationId`값보다 `AutomationProperties.Name` 및 `AutomationProperties.HelpText` 속성 값을 우선 지정하여 기본 `ContentDescription` 속성을 설정합니다(`AutomationProperties.Name` 및 `AutomationProperties.HelpText` 모두가 설정된 경우 값이 연결됩니다). 즉, `AutomationProperties.Name` 또는 `AutomationProperties.HelpText`가 요소에 설정된 경우 `AutomationId`를 검색하는 모든 테스트가 실패합니다. 이 시나리오에서 `AutomationProperties.Name` 또는 `AutomationProperties.HelpText`의 값 또는 둘 다의 연결을 검색하기 위해 UI 테스트를 변경해야 합니다.

각 플랫폼에는 액세스 가능성 값을 설명하는 다른 화면 판독기가 있습니다.

- iOS에는 VoiceOver가 있습니다. 자세한 내용은 developer.apple.com에서 [VoiceOver를 사용하여 디바이스에 대한 엑세스 가능성 테스트](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html)를 참조하세요.
- Android에는 TalkBack이 있습니다. 자세한 내용은 developer.android.com에서 [앱의 액세스 가능성 테스트](https://developer.android.com/training/accessibility/testing.html#talkback)를 참조하세요.
- Windows에는 내레이터가 있습니다. 자세한 내용은 [내레이터를 사용하여 기본 앱 시나리오 확인](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator)을 참조하세요.

그러나 화면 판독기의 정확한 동작은 소프트웨어 및 사용자의 소프트웨어 구성에 따라 달라집니다. 예를 들어 대부분의 화면 판독기는 포커스를 받을 때 컨트롤과 연결된 텍스트를 읽고, 페이지에서 컨트롤 사이를 이동하는 경우 사용자가 스스로 방향을 지정하게 할 수 있습니다. 일부 화면 판독기에는 페이지가 나타나면 사용자가 이동하기 전에 페이지의 사용 가능한 모든 정보 콘텐츠를 받을 수 있도록 설정되는 전체 애플리케이션 사용자 인터페이스가 표시됩니다.

화면 판독기는 다양한 액세스 가능성 값도 읽을 수 있습니다. 샘플 애플리케이션에서

- VoiceOver는 컨트롤을 사용하는 지침에 따라 `Entry`의 `Placeholder` 값을 읽습니다.
- TalkBack은 컨트롤을 사용하는 지침에 따라 `Entry`의 `Placeholder` 값에 이어 `AutomationProperties.HelpText` 값을 읽습니다.
- Narrator는 컨트롤을 사용하는 지침에 따라 `Entry`의 `AutomationProperties.LabeledBy` 값을 읽습니다.

또한 Narrator는 `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, `AutomationProperties.HelpText`의 차례로 우선 순위를 지정합니다. Android에서 TalkBack은 `AutomationProperties.Name` 값 및 `AutomationProperties.HelpText` 값을 결합할 수 있습니다. 따라서 최적의 환경을 제공하도록 각 플랫폼에서 액세스 가능성에 대한 철저한 테스트를 수행하는 것이 좋습니다.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` 연결된 속성은 요소가 화면 판독기에 액세스할 수 있으므로 화면 판독기에 표시되는지 확인하는 `boolean`입니다. 해당 속성은 다른 액세스 가능성 연결 속성을 사용하는 `true`로 설정되어야 합니다. 이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

또는 다음과 같이 C#에서 설정할 수 있습니다.

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 메서드는 `AutomationProperties.IsInAccessibleTree` 연결된 속성을 설정하는 데 사용할 수도 있습니다 - `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` 연결된 속성 값은 화면 판독기가 요소를 발표하는 데 사용하는 설명이 포함된 짧은 텍스트 문자열이어야 합니다. 콘텐츠를 이해하거나 사용자 인터페이스와 상호 작용하는 데 중요한 의미가 있는 요소에 대해 이 속성을 설정해야 합니다. 이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

또는 다음과 같이 C#에서 설정할 수 있습니다.

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 메서드는 `AutomationProperties.Name` 연결된 속성을 설정하는 데 사용할 수도 있습니다 - `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` 연결된 속성을 사용자 인터페이스 요소를 설명하는 텍스트로 설정해야 하며, 요소와 연결된 도구 설명 텍스트로 간주할 수 있습니다. 이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

또는 다음과 같이 C#에서 설정할 수 있습니다.

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 메서드는 `AutomationProperties.HelpText` 연결된 속성을 설정하는 데 사용할 수도 있습니다 - `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

일부 플랫폼에서는 [`Entry`](xref:Xamarin.Forms.Entry)과 같은 편집 컨트롤을 위해 `HelpText` 속성을 경우에 따라 생략하고 자리 표시자 텍스트로 바꿀 수 있습니다. 예를 들어 "여기에 이름 입력"은 사용자가 실제로 입력하기 전에 컨트롤에 텍스트를 배치하는 [`Entry.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) 속성의 좋은 후보입니다.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` 연결된 속성은 다른 요소가 현재 요소에 대한 액세스 가능성 정보를 정의하게 할 수 있습니다. 예를 들어 [`Entry`](xref:Xamarin.Forms.Entry) 옆에 있는 [`Label`](xref:Xamarin.Forms.Label)은 `Entry`가 나타내는 것을 설명하는 데 사용할 수 있습니다. 이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

또는 다음과 같이 C#에서 설정할 수 있습니다.

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) 메서드는 `AutomationProperties.IsInAccessibleTree` 연결된 속성을 설정하는 데 사용할 수도 있습니다 - `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="accessibility-intricacies"></a>내게 필요한 옵션 복잡성

다음 섹션에서는 특정 컨트롤에서 내게 필요한 옵션 값을 설정하는 복잡성을 설명합니다.

### <a name="navigationpage"></a>NavigationPage

Android에서 텍스트를 설정하기 위해 해당 화면 판독기는 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)의 작업 표시줄에서 뒤로 가기 화살표에 대해 읽고, [`Page`](xref:Xamarin.Forms.Page)에서 `AutomationProperties.Name` 및 `AutomationProperties.HelpText` 속성을 설정합니다. 하지만 이 작업은 OS 뒤로 가기 단추에 영향을 주지 않습니다.

### <a name="masterdetailpage"></a>MasterDetailPage

iOS 및 UWP(유니버설 Windows 플랫폼)에서 텍스트를 설정하기 위해 해당 화면 판독기는 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)에서 토글 단추에 대해 읽고, `AutomationProperties.Name` 및 `MasterDetailPage`의 `AutomationProperties.HelpText` 속성 또는 `Master` 페이지의 `IconImageSource` 속성을 설정합니다.

Android에서 텍스트를 설정하기 위해 해당 화면 판독기는 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)에서 토글 단추에 대해 읽고, 문자열 리소스를 Android 프로젝트에 추가합니다.

```xml
<resources>
    <string name="app_name">Xamarin Forms Control Gallery</string>
    <string name="btnMDPAutomationID_open">Open Side Menu message</string>
    <string name="btnMDPAutomationID_close">Close Side Menu message</string>
</resources>
```

그런 다음, `Master` 페이지에서 `IconImageSource` 속성의 `AutomationId` 속성을 적절한 문자열로 설정합니다.

```csharp
var master = new ContentPage { ... };
master.IconImageSource.AutomationId = "btnMDPAutomationID";
```

### <a name="toolbaritem"></a>ToolbarItem

iOS, Android 및 UWP에서 화면 판독기는 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) 인스턴스의 `Text` 속성 값을 읽지만, `AutomationProperties.Name` 또는 `AutomationProperties.HelpText` 값이 정의되지 않습니다.

iOS 및 UWP에서 `AutomationProperties.Name` 속성 값은 화면 판독기에서 읽은 `Text` 속성 값을 대체합니다.

Android에서 `AutomationProperties.Name` 및/또는 `AutomationProperties.HelpText` 속성 값은 화면 판독기에서 표시되고 읽힌 `Text` 속성 값을 대체합니다. API를 26개 미만으로 제한합니다.

## <a name="related-links"></a>관련 링크

- [연결된 속성](~/xamarin-forms/xaml/attached-properties.md)
- [액세스 가능성(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)
