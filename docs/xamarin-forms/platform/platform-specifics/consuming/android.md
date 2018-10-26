---
title: Android 플랫폼별
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에는 Android 플랫폼별 Xamarin.Forms에 기본 제공 되는 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: 50c7b05261cf3f07ea37373cdcdcc8f250243647
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108979"
---
# <a name="android-platform-specifics"></a>Android 플랫폼별

_플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에는 Android 플랫폼별 Xamarin.Forms에 기본 제공 되는 사용 하는 방법을 보여 줍니다._

## <a name="visualelements"></a>VisualElements

Android에서 Xamarin.Forms 뷰, 페이지 및 레이아웃에 대해 다음 플랫폼 특정 기능을 제공 됩니다.

- Z-순서 시각적 요소의 그리기 순서를 결정을 제어 합니다. 자세한 내용은 [권한 상승의 시각적 요소를 제어](#elevation)입니다.
- 지원 되는 레거시 색 모드 사용 안 함 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다. 자세한 내용은 [레거시 색 모드 사용 안 함](#legacy-color-mode)합니다.

<a name="elevation" />

### <a name="controlling-the-elevation-of-visual-elements"></a>시각적 요소의 높이 제어합니다.

이 플랫폼별 API 21를 대상으로 하는 권한 상승 또는 응용 프로그램에 대 한 시각적 요소의 Z 순서를 제어 하는 데가 큰 경우 시각적 요소 상승 Z 값이 높을수록 occluding Z 값이 낮은 시각적 요소를 사용 하 여 시각적 요소를 사용 하 여 해당 그리기 순서를 결정 합니다. 설정 하 여 XAML에서 사용 되는 `VisualElement.Elevation` 연결 된 속성을 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. `VisualElement.SetElevation` 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에 null을 허용 하는 상승 시각적 요소를 설정 하는 `float`합니다. 또한는 `VisualElement.GetElevation` 메서드를 사용 하 여 시각적 요소 높이 값을 검색할 수 있습니다.

결과 더 높은 Z 값이 포함 된 시각적 요소 채워집니다 Z 값이 낮은 시각적 요소 수 있도록 시각적 요소의 높이 제어할 수 있습니다. 따라서이 예제의 두 번째 [ `Button` ](xref:Xamarin.Forms.Button) 위에 렌더링 되는 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 권한 상승 값이 높을수록 있기 때문에:

![](android-images/elevation.png)

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>레거시 색 모드를 사용 하지 않도록 설정

Xamarin.Forms 뷰의 일부 레거시 색 모드를 기능입니다. 이 모드에서는 때 합니다 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 뷰의 속성 `false`, 보기에는 색 사용 안 함된 상태에 대 한 기본 네이티브 색을 사용 하 여 사용자 설정 보다 우선 합니다. 이전 버전과 호환성을 위해이 레거시 색 모드는 지원 되는 보기에 대 한 기본 동작을 유지 합니다.

이 플랫폼별 뷰를 사용 하지 않도록 설정 하는 경우에 사용자가 설정한 뷰에 색 유지 되도록이 레거시 색 모드를 해제 합니다. 설정 하 여 XAML에서 사용 되는 [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) 연결 된 속성을 `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. 합니다 [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 레거시 색 모드가 비활성화 되어 있는지 여부를 제어 하려면 네임 스페이스는 합니다. 또한 합니다 [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) 메서드를 사용 하 여 레거시 색 모드를 비활성화 되었는지 여부를 반환할 수 있습니다.

결과 보기를 비활성화 하는 경우 사용자가 설정한 뷰에 색도 계속 되도록 레거시 색 모드를 비활성화할 수 있습니다.

![](android-images/legacy-color-mode-disabled.png "레거시 색 모드 사용 안 함")

> [!NOTE]
> 설정 하는 경우는 [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) 레거시 색 모드는 완전히 뷰에서 무시 됩니다. 시각적 상태에 대 한 자세한 내용은 참조 하세요. [은 Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

## <a name="views"></a>보기

Android에서 Xamarin.Forms 보기에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다.

- 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용합니다. 자세한 내용은 [Android 단추를 사용 하 여](#button-padding-shadow)입니다.
- 입력된 방법에 대 한 소프트 키보드에 대 한 편집기 옵션 설정 된 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 자세한 내용은 [설정은 항목 입력 방법 편집기 옵션](#entry-imeoptions)합니다.
- 빠른 스크롤을 사용 하도록 설정 된 [ `ListView` ](xref:Xamarin.Forms.ListView) 자세한 내용은 참조 하세요. [는 ListView의 빠른 스크롤 사용 하도록 설정](#fastscroll).
- 제어 여부는 [ `WebView` ](xref:Xamarin.Forms.WebView) 혼합 된 콘텐츠를 표시할 수 있습니다. 자세한 내용은 [혼합 콘텐츠는 WebView에서 사용 하도록 설정 하면](#webview-mixed-content)합니다.

<a name="button-padding-shadow" />

### <a name="using-android-buttons"></a>Android 단추를 사용 하 여

이 플랫폼별 기본 안쪽 여백 및 Android 단추의 섀도 값 Xamarin.Forms 단추를 사용 하는지 여부를 제어 합니다. 설정 하 여 XAML에서 사용 되는 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) 하 고 [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) 속성을 연결 `boolean` 값:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) 및[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스 Xamarin.Forms 단추 기본값을 사용 하는지 여부를 제어 하는 데 사용 됩니다 안쪽 여백 및 Android 단추의 섀도 값입니다. 또한 합니다 [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) 및 [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) 단추 기본 각각 값 및 기본값 그림자를 안쪽 여백 사용 하는지 여부를 반환 하려면 메서드를 사용할 수 있습니다.

결과 Xamarin.Forms 단추는 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용할 수 있는:

![](android-images/button-padding-and-shadow.png "레거시 색 모드 사용 안 함")

위의 각 스크린 샷 [ `Button` ](xref:Xamarin.Forms.Button) 점을 제외 하면 동일한 정의가 오른쪽 `Button` 기본 안쪽 여백 및 Android 단추의 섀도 값을 사용 하 여 합니다.

<a name="entry-imeoptions" />

### <a name="setting-entry-input-method-editor-options"></a>항목 입력 방법 편집기 옵션 설정

이 플랫폼별 입력된 방법에 대 한 소프트 키보드 (입력기) 옵션을 설정 된 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 소프트 키보드와의 상호 작용의 아래쪽 모서리에서 사용자 작업 단추를 설정 하는 것이 여기는 `Entry`합니다. 설정 하 여 XAML에서 사용 되는 [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) 연결 된 속성의 값에는 [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 열거형:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) 메서드를를 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에 대 한 소프트 키보드 입력된 메서드가 작업 옵션을 설정 하는 합니다 [ `Entry` ](xref:Xamarin.Forms.Entry), 사용 하 여 합니다 [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 다음 값을 제공 하는 열거형:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – 나타내며 특정 작업 키는 필요 없습니다 수 내부 컨트롤 자체를 생성 합니다. 중 하나 `Next` 또는 `Done`합니다.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – 없습니다 동작 키를 사용할 수 있도록 나타냅니다.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – 동작 키 "이동" 작업을 수행할지에 입력 텍스트의 대상 사용자를 나타냅니다.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) -는 "검색" 연산을 수행 하는 동작 키, 텍스트 검색의 결과가 사용자 입력을 나타냅니다.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – 동작 키가 대상에 텍스트를 제공 "보내기" 작업을 수행 되는 것을 나타냅니다.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – 동작 키 텍스트를 허용 하는 다음 필드에 사용자를 수행 하는 "다음" 작업을 수행 되는 것을 나타냅니다.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – 동작 키 소프트 키보드 닫기를 "완료" 작업을 수행할지를 나타냅니다.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – 동작 키 텍스트를 허용 하는 이전 필드에 사용자를 이동 "이전" 작업을 수행 되는 것을 나타냅니다.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) -작업 옵션을 선택 하는 마스크입니다.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – 나타냅니다는 맞춤법 검사기는 사용자에 대해 알아봅니다 아니고 기반으로 새로운 사용자가 이전에 입력 한 수정을 제안 합니다.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – UI 화면을 이동 하지 해야 나타냅니다.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) -추출 된 텍스트에 대 한 UI가 표시를 나타냅니다.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – 사용자 지정 작업에 대 한 UI를 표시할지 나타냅니다.

결과 지정 된 [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 값에 대 한 소프트 키보드에 적용 됩니다는 [ `Entry` ](xref:Xamarin.Forms.Entry)를 설정 하는 입력된 방법 편집기 옵션:

[![항목 입력 방법 편집기 플랫폼별](android-images/entry-imeoptions.png "항목 입력 방법 편집기 플랫폼별")](android-images/entry-imeoptions-large.png#lightbox "항목 입력 방법 편집기 플랫폼별")

<a name="fastscroll" />

### <a name="enabling-fast-scrolling-in-a-listview"></a>빠른 스크롤 하는 ListView를 사용 하도록 설정

데이터를 통해 빠른 스크롤을 사용 하려면이 플랫폼별 되는 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 설정 하 여 XAML에서 사용 되는 `ListView.IsFastScrollEnabled` 연결 된 속성을 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. `ListView.SetIsFastScrollEnabled` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에서 데이터를 통해 빠른 스크롤을 사용 하는 [ `ListView` ](xref:Xamarin.Forms.ListView)합니다. 또한 합니다 `SetIsFastScrollEnabled` 메서드를 호출 하 여 빠른 스크롤 설정/해제를 사용할 수는 `IsFastScrollEnabled` 빠른 스크롤 사용 되는지 여부를 반환 하는 방법.

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

결과에서 데이터를 통해 빠른 스크롤 하는 한 [ `ListView` ](xref:Xamarin.Forms.ListView) 스크롤의 크기를 변경 하는 사용할 수 있습니다.

[![](android-images/fastscroll.png "ListView FastScroll 플랫폼별")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="webview-mixed-content" />

### <a name="enabling-mixed-content-in-a-webview"></a>WebView에서 혼합 된 콘텐츠를 사용 하도록 설정

이 플랫폼별 제어 여부를 [ `WebView` ](xref:Xamarin.Forms.WebView) 수 표시 혼합 된 콘텐츠가 응용 프로그램에서 API 21 이상를 대상으로 하는 합니다. 혼합 된 콘텐츠는 HTTPS 연결을 통해 처음 로드 되는 하지만 HTTP 연결을 통해 리소스 (예: 이미지, 오디오, 비디오, 스타일 시트, 스크립트)를 로드 하는 콘텐츠입니다. 설정 하 여 XAML에서 사용 되는 [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) 연결 된 속성의 값에는 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 열거형:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 하려면 네임 스페이스는 사용 하 여는 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 세 가지 가능한 값을 제공 하는 열거형:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – 나타내는 합니다 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTP 원본에서 콘텐츠를 로드 하는 HTTPS 원본 허용 됩니다.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – 나타내는 합니다 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTPS origin HTTP 원본에서 콘텐츠를 로드할 수 없습니다.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – 나타내는 합니다 [ `WebView` ](xref:Xamarin.Forms.WebView) 방식의 최신 장치 웹 브라우저와 호환 되도록 하려고 합니다. 일부 HTTP 콘텐츠를 HTTPS origin에서 로드할 수 있습니다 하 고 다른 형식의 콘텐츠 차단 됩니다. 각 운영 체제 릴리스를 사용 하 여 허용 되거나 차단 되는 콘텐츠 유형을 변경할 수 있습니다.

결과 지정 된 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 값에 적용 됩니다는 [ `WebView` ](xref:Xamarin.Forms.WebView), 혼합된 콘텐츠를 표시할 수 있는지 여부를 제어 합니다.

[![WebView 혼합 콘텐츠 처리 플랫폼별](android-images/webview-mixedcontent.png "WebView 혼합 콘텐츠 처리 플랫폼별")](android-images/webview-mixedcontent-large.png#lightbox "WebView 혼합 콘텐츠 처리 플랫폼 전용")

## <a name="pages"></a>인쇄할 페이지

Android에서 Xamarin.Forms 페이지에 대 한 다음과 같은 플랫폼별 기능 제공 됩니다.

- 탐색 막대의 높이에 설정 된 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)합니다. 자세한 내용은 [탐색 막대 높이 NavigationPage 설정](#navigationpage-barheight)합니다.
- 페이지 사이 탐색할 때 전환 애니메이션을 사용 하지 않도록 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 자세한 내용은 [는 TabbedPage 페이지 전환 애니메이션 사용 하지 않도록 설정](#tabbedpage-transition-animations)합니다.
- 내 페이지 간 살짝 사용 하도록 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 자세한 내용은 [활성화 살짝 페이지 사이 TabbedPage에서](#enable_swipe_paging)합니다.
- 도구 모음 위치와 색에 설정 된 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 자세한 내용은 [설정은 TabbedPage 도구 모음 배치 및 색](#tabbedpage-toolbar)합니다.

<a name="navigationpage-barheight" />

### <a name="setting-the-navigation-bar-height-on-a-navigationpage"></a>탐색 모음 설정에 NavigationPage 높이

탐색 모음의 높이 설정 하는이 플랫폼별을 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)합니다. 설정 하 여 XAML에서 사용 되는 [ `NavigationPage.BarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) 정수 값으로 바인딩할 수 있는 속성:

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

`NavigationPage.On<Android>` 메서드 지정이 플랫폼별 앱 호환성 Android에만 실행 됩니다. [ `NavigationPage.SetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.SetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage},System.Int32)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) 네임 스페이스는 탐색 막대의 높이 설정 하는 데 사용 되는 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)합니다. 또한 합니다 [ `NavigationPage.GetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.GetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage})) 메서드를 사용 하 여에서 탐색 막대의 높이를 반환할 수는 `NavigationPage`합니다.

결과에 있는 탐색 막대의 높이 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 설정할 수 있습니다.

![](android-images/navigationpage-barheight.png "NavigationPage 탐색 막대 높이")

<a name="tabbedpage-transition-animations" />

### <a name="disabling-page-transition-animations-in-a-tabbedpage"></a>TabbedPage의 페이지 전환 애니메이션을 사용 하지 않도록 설정

이 플랫폼별에 탭 표시줄을 사용 하는 경우 탐색할 때 페이지를 통해 하거나 프로그래밍 방식으로 또는 전환 애니메이션을 사용 하지 않도록 설정 되는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 설정 하 여 XAML에서 사용 되는 `TabbedPage.IsSmoothScrollEnabled` 바인딩 가능한 속성을 `false`:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

`TabbedPage.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. `TabbedPage.SetIsSmoothScrollEnabled` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 전환 애니메이션의 페이지 간을 탐색할 때 표시 될 수 있는지 여부를 제어 하려면 네임 스페이스는를 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). 또한 합니다 `TabbedPage` 클래스는 `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` 네임 스페이스는 다음 메서드도 들어:

- `IsSmoothScrollEnabled`에 전환 애니메이션의 페이지 간을 탐색할 때 표시 될 수 있는지 여부를 검색 하는 데 사용 됩니다는 `TabbedPage`합니다.
- `EnableSmoothScroll`에 페이지 사이 탐색할 때 전환 애니메이션을 사용 하도록 설정 하는 데 사용 됩니다는 `TabbedPage`합니다.
- `DisableSmoothScroll`에 페이지 사이 탐색할 때 전환 애니메이션을 사용 하지 않도록 설정 하는 데 사용 됩니다는 `TabbedPage`합니다.

<a name="enable_swipe_paging" />

### <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>TabbedPage 내에서 페이지 간 살짝 사용 하도록 설정

이 플랫폼별 살짝는 페이지 사이의 가로 손가락 제스처를 사용 하 여 사용 하도록 설정 되는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 설정 하 여 XAML에서 사용 되는 [ `TabbedPage.IsSwipePagingEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) 연결 된 속성을 `boolean` 값:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. 합니다 [ `TabbedPage.SetIsSwipePagingEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled(Xamarin.Forms.BindableObject,System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스를 사용 하는 페이지 사이의 살짝 활성화를 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 또한 합니다 `TabbedPage` 클래스를 `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` 네임 스페이스 역시를 [ `EnableSwipePaging` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) 이 플랫폼별 수 있도록 하는 메서드 및 [ `DisableSwipePaging` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) 사용 하지 않도록 설정 하는 메서드 이 플랫폼별입니다. 합니다 [ `TabbedPage.OffscreenPageLimit` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty) 연결 된 속성 및 [ `SetOffscreenPageLimit` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit(Xamarin.Forms.BindableObject,System.Int32)) 메서드를 현재 페이지의 어느 쪽에서 유휴 상태에서 유지 되어야 하는 페이지 수를 설정 하는 데 사용 됩니다.

결과를 사용 하 여 표시 페이지 안쪽으로 살짝 밀어 페이징 하는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 사용 가능:

![](android-images/tabbedpage-swipe.png)

<a name="tabbedpage-toolbar" />

### <a name="setting-tabbedpage-toolbar-placement-and-color"></a>TabbedPage 도구 모음 배치 및 색 설정

이러한 플랫폼별 되에서 배치 및 도구 모음 색을 설정 하는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)합니다. 설정 하 여 XAML에서 사용 되는 [ `TabbedPage.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) 연결 된 속성의 값을는 [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) 열거형 및 [ `TabbedPage.BarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) 및 [ `TabbedPage.BarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) 연결 된 속성에는 [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

또는 fluent API를 사용 하 여 C#에서 사용 될 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

`TabbedPage.On<Android>` 메서드 이러한 플랫폼별 Android에만 실행 되도록 지정 합니다. [ `TabbedPage.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에 도구 모음 배치를 설정 하는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)를 사용 하 여는 [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) 열거형은 다음 값을 제공 합니다.

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default) – 도구 모음 페이지에서 기본 위치에 배치 됩니다 함을 나타냅니다. 휴대폰에서는 페이지의 맨 위에 다른 장치 관용구의 경우 페이지의 맨 아래입니다.
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top) – 페이지의 맨 위에 있는 도구 모음 배치는 함을 나타냅니다.
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom) – 도구 모음 페이지의 맨 아래에 배치 됩니다 함을 나타냅니다.

또한 합니다 [ `TabbedPage.SetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) 하 고 [ `TabbedPage.SetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) 메서드 각각 도구 모음 항목 및 선택한 도구 모음 항목의 색을 설정 하는 데 사용 됩니다.

> [!NOTE]
> 합니다 [ `GetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage}))를 [ `GetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})), 및 [ `GetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) 메서드를 사용 하 여 배치 및 색을 검색할 수는 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 도구 모음입니다.

결과 도구 모음 배치, 도구 모음 항목의 색 및 선택한 도구 모음 항목의 색으로 설정할 수 있도록를 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

![](android-images/tabbedpage-toolbar-placement.png)

## <a name="application"></a>응용 프로그램

다음 플랫폼 특정 기능을 android에서 Xamarin.Forms에 대 한 제공 됩니다 [ `Application` ](xref:Xamarin.Forms.Application) 클래스:

- 소프트 키보드의 운영 모드를 설정 합니다. 자세한 내용은 [소프트 키보드 입력 모드 설정](#soft_input_mode)합니다.
- 사용 하지 않도록 설정 합니다 [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) 하 고 [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) 페이지 수명 주기 이벤트를 일시 중지 하 고 각각 AppCompat를 사용 하는 응용 프로그램에 대 한 다시 시작 합니다. 자세한 내용은 [Disappearing 및 페이지 수명 주기 이벤트 표시 해제](#disable_lifecycle_events)합니다.

<a name="soft_input_mode" />

### <a name="setting-the-soft-keyboard-input-mode"></a>소프트 키보드 입력된 모드를 설정합니다.

이 플랫폼별 소프트 키보드 입력된 영역에 대 한 운영 모드를 설정 하는 데 사용 되 고 설정 하 여 XAML에서 사용 되는 [ `Application.WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) 연결 된 속성의 값에는 [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) 열거형:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. 합니다 [ `Application.UseWindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스를 사용 하 여 소프트 키보드 입력된 영역 운영 모드를 설정 하는 합니다 [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) 두 값을 제공 하는 열거형: [ `Pan` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) 하 고 [ `Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize)합니다. 합니다 `Pan` 사용 하 여 값을 [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) 입력된 컨트롤에 포커스가 있을 때 창 크기가 조정 되지 않는 조정 옵션을 합니다. 대신 창의 내용은 이동 하는 현재 포커스 소프트 키보드 가려져 되지 않도록 합니다. 합니다 `Resize` 사용 하 여 값을 [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) 입력된 컨트롤에 포커스를 확보 하기 위해 소프트 키보드 창의 크기를 조정 하는 조정 옵션입니다.

결과 소프트 키보드 입력 영역 입력된 컨트롤에 포커스가 있을 때 운영 모드를 설정할 수 있습니다는.

[![](android-images/pan-resize.png "운영 모드 플랫폼별 소프트 키보드")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="disable_lifecycle_events" />

### <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>사라지는 / 페이지 수명 주기 이벤트를 사용 하지 않도록 설정

이 플랫폼별은 사용 하지 않도록 설정 하는 데 사용 되는 [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) 하 고 [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) 페이지 이벤트 응용 프로그램에서 일시 중지 하 고 각각 AppCompat를 사용 하는 응용 프로그램에 대 한 다시 시작 합니다. 소프트 키보드는 소프트 키보드의 운영 모드를로 일시 중지에 표시 된 경우 다시 시작할 때 표시 되는지 여부를 제어 하는 기능 포함 하는 또한 [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize)합니다.

> [!NOTE]
> 이러한 이벤트는 이벤트를 사용 하는 응용 프로그램에 대 한 기존 동작을 유지 하기 위해 기본적으로 설정 되어 있는지 note 합니다. 사전 AppCompat 이벤트 주기를 일치 하는 AppCompat 이벤트 주기를 통해 이러한 이벤트를 사용 하지 않도록 설정 합니다.

이 플랫폼별으로 설정 하 여 XAML에서 사용 될 수는 [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty)합니다 [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty), 및 [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty) 에연결된속성`boolean` 값:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

`Application.Current.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) 네임 스페이스를 사용 하도록 설정 하거나 발생 하지 않도록 설정 되는 [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) 페이지 이벤트, 응용 프로그램 백그라운드를 입력합니다. 합니다 [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) 메서드를 사용 하도록 설정 하거나 발생 하지 않도록 설정 되는 [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) 백그라운드에서 응용 프로그램 다시 시작 될 때 페이지 이벤트입니다. 합니다 [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) 메서드는 컨트롤 일시 중지에 표시 된 경우 다시 시작할 때 소프트 키보드 표시 되는지 여부를 제공 소프트 키보드의 운영 모드 설정 되어 있는지 [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

결과 [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) 하 고 [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) 페이지 이벤트 응용 프로그램 일시 중지 시 발생 하지 않습니다 각각 다시 시작 하 고 있는지 소프트 키보드를 선택할 때 표시 된 응용 프로그램 가 일시 중지 된 것도 표시 됩니다 응용 프로그램을 다시 시작 하는 경우:

[![](android-images/keyboard-on-resume.png "수명 주기 이벤트 플랫폼별")](android-images/keyboard-on-resume-large.png#lightbox "수명 주기 이벤트 플랫폼별")

## <a name="summary"></a>요약

이 문서에는 Android 플랫폼별 Xamarin.Forms에 기본 제공 되는 사용 하는 방법을 보여 줍니다. 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
