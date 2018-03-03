---
title: "사용자 인터페이스 요소에 내게 필요한 옵션 값을 설정합니다."
description: "Xamarin.Forms 내게 필요한 옵션 값을을 네이티브 내게 필요한 옵션 값을 설정은 AutomationProperties 클래스에서 연결 된 속성을 사용 하 여 사용자 인터페이스 요소에서 설정할 수 있습니다. 이 문서를 페이지에는 화면 판독기의 요소에 대 한 서로 통신할 수 있습니다 AutomationProperties 클래스를 사용 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 4aeeea7f946a121b12741d2da217daf531935849
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>사용자 인터페이스 요소에 내게 필요한 옵션 값을 설정합니다.

_Xamarin.Forms 내게 필요한 옵션 값을을 네이티브 내게 필요한 옵션 값을 설정은 AutomationProperties 클래스에서 연결 된 속성을 사용 하 여 사용자 인터페이스 요소에서 설정할 수 있습니다. 이 문서를 페이지에는 화면 판독기의 요소에 대 한 서로 통신할 수 있습니다 AutomationProperties 클래스를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Forms에 내게 필요한 옵션 값을을 다음 연결 된 속성을 통해 사용자 인터페이스 요소에서 설정할 수 있습니다.

- `AutomationProperties.IsInAccessibleTree` – 요소를 액세스할 수 있는 응용 프로그램에 사용할 수 있는지를 나타냅니다. 자세한 내용은 참조 [AutomationProperties.IsInAccessibleTree](#isinaccessibletree)합니다.
- `AutomationProperties.Name` – 요소에 대 한 speakable 식별자로 사용 하는 요소에 대 한 간단한 설명을 합니다. 자세한 내용은 참조 [AutomationProperties.Name](#name)합니다.
- `AutomationProperties.HelpText` – 요소와 연결 된 도구 설명 텍스트로 간주 될 수 있는 요소에 대 한 긴 설명을 합니다. 자세한 내용은 참조 [AutomationProperties.HelpText](#helptext)합니다.
- `AutomationProperties.LabeledBy` -다른 요소가 현재 요소에 대 한 내게 필요한 옵션 정보를 정의할 수 있습니다. 자세한 내용은 참조 [AutomationProperties.LabeledBy](#labeledby)합니다.

이러한 첨부 속성 집합 네이티브 내게 필요한 옵션 값 되므로 화면 판독기가 요소에 대 한 서로 통신할 수 있습니다. 연결 된 속성에 대 한 자세한 내용은 참조 [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md)합니다.

> [!IMPORTANT]
> 사용 하 여 `AutomationProperties` 연결 된 속성 Android에서 UI 테스트 실행에 영향을 줄 수 있습니다. [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/), `AutomationProperties.Name` 및 `AutomationProperties.HelpText` 속성 모두 네이티브 설정 `ContentDescription` 속성으로는 `AutomationProperties.Name` 및 `AutomationProperties.HelpText` 우선 순위는 차지하는속성값`AutomationId`값 (모두 `AutomationProperties.Name` 및 `AutomationProperties.HelpText` 설정 값은 연결 된). 즉, 모든 테스트를 찾고 `AutomationId` 경우 실패 합니다 `AutomationProperties.Name` 또는 `AutomationProperties.HelpText` 요소에 설정 됩니다. 이 시나리오에서는 값을 찾기 위해 UI 테스트를 변경할 `AutomationProperties.Name` 또는 `AutomationProperties.HelpText`, 또는 둘 다 연결 합니다.

각 플랫폼에 다양 한 화면 판독기가 내게 필요한 옵션 값을 내레이션:

- iOS 음성을 있습니다. 자세한 내용은 참조 [음성 사용 중인 장치에서 테스트 접근성](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) developer.apple.com에 있습니다.
- Android TalkBack에 있습니다. 자세한 내용은 참조 [테스트 응용 프로그램의 내게 필요한 옵션](https://developer.android.com/training/accessibility/testing.html#talkback) developer.android.com에 있습니다.
- Windows 내레이터를 있습니다. 자세한 내용은 참조 [내레이터를 사용 하 여 기본 응용 프로그램 시나리오를 확인할](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/)합니다.

그러나 정확한 동작은 화면 판독기의 소프트웨어 및 사용자의 구성 하는 것에 따라 다릅니다. 예를 들어 대부분의 화면 판독기 텍스트를 읽을 포커스를 받을 때 컨트롤과 연결 된 사용자를 페이지의 컨트롤 간에 이동 하는 동안 자체를 배치할 수 있도록 합니다. 화면 판독기에 전체 응용 프로그램 사용자 인터페이스를 모두 읽을 수도 페이지가 표시 되 면 사용자가 탐색 하기 전에 모든 페이지의 사용 가능한 정보 콘텐츠를 받을 수 있습니다.

화면 판독기 다른 내게 필요한 옵션 값을 읽을 수도 있습니다. 샘플 응용 프로그램:

- 음성 읽힙니다는 `Placeholder` 의 값은 `Entry`, 컨트롤을 사용 하 여에 대 한 지침입니다.
- TalkBack 읽습니다는 `Placeholder` 의 값은 `Entry`옵니다는 `AutomationProperties.HelpText` 값, 컨트롤을 사용 하 여에 대 한 지침.
- 내레이터는 읽기는 `AutomationProperties.LabeledBy` 의 값은 `Entry`, 컨트롤을 사용 하 여에 대 한 지침입니다.

내레이터는 우선 순위를 또한 `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, 차례로 `AutomationProperties.HelpText`합니다. Android에서는 TalkBack 결합할 수는 `AutomationProperties.Name` 및 `AutomationProperties.HelpText` 값입니다. 따라서는 철저 한 내게 필요한 옵션 한 테스트가 수행 최적의 환경을 유지 하기 위해 각 플랫폼에는 것이 좋습니다.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` 연결 된 속성은 한 `boolean` 결정 하는 요소에 액세스할 수 있고 화면 판독기에 따라서 표시 합니다. 로 설정 해야 `true` 연결 된 속성을 다른 내게 필요한 옵션을 사용 합니다. 이 위해서는 XAML에서 다음과 같이 합니다.

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

또는 설정할 수 있습니다 C#에서 다음과 같이 합니다.

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) 메서드 설정에 사용할 수도 있습니다는 `AutomationProperties.IsInAccessibleTree` 연결 된 속성 – `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` 연결 된 속성 값에는 화면 판독기를 사용 하 여 요소를 발표 짧은 이지만 설명이 포함 된 텍스트 문자열 이어야 합니다. 이 속성은 콘텐츠를 이해 하거나 사용자 인터페이스와 상호 작용에 대 한 중요 한 의미를 갖는 요소에 대 한 설정 되어야 합니다. 이 위해서는 XAML에서 다음과 같이 합니다.

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

또는 설정할 수 있습니다 C#에서 다음과 같이 합니다.

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) 메서드 설정에 사용할 수도 있습니다는 `AutomationProperties.Name` 연결 된 속성 – `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` 속성 사용자 인터페이스 요소를 설명 하는 텍스트를 설정 하 고 도구 설명 텍스트도 간주와 연결 될 수 있는 요소에 연결 합니다. 이 위해서는 XAML에서 다음과 같이 합니다.

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

또는 설정할 수 있습니다 C#에서 다음과 같이 합니다.

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) 메서드 설정에 사용할 수도 있습니다는 `AutomationProperties.HelpText` 연결 된 속성 – `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

일부 플랫폼에서는 편집에 대 한 제어와 같은 프로그램 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), `HelpText` 속성 수 경우가 생략 되 고 개체 틀 텍스트도 바뀝니다. 적합 한 필드에 "여기에 이름을 입력 합니다" 예를 들어는 [ `Entry.Placeholder` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) 사용자의 실제 입력 앞의 컨트롤에 텍스트를 삽입 하는 속성입니다.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` 연결 된 속성을 사용 하면 다른 요소가 현재 요소에 대 한 내게 필요한 옵션 정보를 정의할 수 있습니다. 예를 들어 한 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 옆에 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 는 대상을 설명 하는 데 사용할 수는 `Entry` 나타냅니다. 이 위해서는 XAML에서 다음과 같이 합니다.

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

또는 설정할 수 있습니다 C#에서 다음과 같이 합니다.

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) 메서드 설정에 사용할 수도 있습니다는 `AutomationProperties.IsInAccessibleTree` 연결 된 속성 – `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>요약

이 문서에서는 내게 필요한 옵션에 값을 설정 사용자 인터페이스 요소 Xamarin.Forms 응용 프로그램에서에서 연결 된 속성을 사용 하 여는 방법의 `AutomationProperties` 클래스입니다. 이러한 첨부 속성은 네이티브 내게 필요한 옵션 값을 설정 되므로 페이지 화면 판독기가 요소에 대 한 서로 통신할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [연결 된 속성](~/xamarin-forms/xaml/attached-properties.md)
- [내게 필요한 옵션 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
