---
제목: "Windows의 VisualElement 레거시 색 모드" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 레거시 색 모드를 사용 하지 않도록 설정 하는 Windows 플랫폼별를 사용 하는 방법을 설명 Xamarin.Forms 합니다. "
assetid: B8759309-07C7-4DCA-A18A-C1A198A7951B: xamarin-forms author: davidbritch: dabritch:: 10/24/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="visualelement-legacy-color-mode-on-windows"></a>Windows의 VisualElement 레거시 색 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

일부 뷰는 Xamarin.Forms 레거시 색 모드를 특징으로 합니다. 이 모드에서 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 뷰의 속성이로 설정 되 면 `false` 뷰에서 사용 하지 않도록 설정 된 기본 색으로 사용자가 설정한 색이 재정의 됩니다. 이전 버전과의 호환성을 위해이 레거시 색 모드는 지원 되는 보기의 기본 동작으로 유지 됩니다.

이 유니버설 Windows 플랫폼 플랫폼별 색 모드를 사용 하지 않도록 설정 하 여 사용자가 보기에 설정 된 색이 뷰를 사용 하지 않도록 설정 된 경우에도 유지 되도록 합니다. 연결 된 속성을로 설정 하 여 XAML에서 사용 됩니다 [`VisualElement.IsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) `false` .

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>`메서드는이 플랫폼별가 Windows 에서만 실행 되도록 지정 합니다. [ `VisualElement.SetIsLegacyColorModeEnabled` ] (F: Xamarin.Forms 입니다. PlatformConfiguration ( Xamarin.Forms .. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . VisualElement}, system.string) 메서드를 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 사용 하 여 레거시 색 모드를 사용 하지 않도록 설정할지 여부를 제어 합니다. 또한 [ `VisualElement.GetIsLegacyColorModeEnabled` ] (f: Xamarin.Forms 입니다. PlatformConfiguration ( Xamarin.Forms .. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . VisualElement}) 메서드를 사용 하 여 레거시 색 모드를 사용 하지 않도록 설정할지 여부를 반환할 수 있습니다.

따라서 레거시 색 모드를 사용 하지 않도록 설정 하 여 보기에 설정 된 색이 보기를 사용 하지 않도록 설정 된 경우에도 유지 되도록 할 수 있습니다.

![](legacy-color-mode-images/legacy-color-mode-disabled.png "Legacy color mode disabled")

> [!NOTE]
> [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup)보기에 대해를 설정 하는 경우 레거시 색 모드가 완전히 무시 됩니다. 시각적 상태에 대 한 자세한 내용은 [ Xamarin.Forms 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
