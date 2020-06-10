---
제목: "iOS의 안전 영역 레이아웃 가이드" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 ios 플랫폼 관련 기능을 사용 하는 방법에 대해 설명 합니다 .이 기능을 사용 하면 iOS 11 이상을 사용 하는 모든 장치에서 안전 하 게 화면 영역에 페이지 콘텐츠를 배치할 수 있습니다. "
assetid: 2B6789C1-39B4-4C16-ADE1-3ED3378EAC63: xamarin-forms author: davidbritch: dabritch:: 10/24/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="safe-area-layout-guide-on-ios"></a>IOS의 안전 영역 레이아웃 가이드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 전용은 iOS 11 이상을 사용 하는 모든 장치에 안전 하 게 사용 되는 화면 영역에 페이지 콘텐츠를 배치 하는 데 사용 됩니다. 특히 iPhone X에서 콘텐츠가 원형 장치 코너, 홈 표시기 또는 센서 하우징에 의해 잘리지 않도록 하는 데 도움이 됩니다. 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 `Page.UseSafeArea` `boolean` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `Page.SetUseSafeArea`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 안전 영역 레이아웃 안내선을 사용할 수 있는지 여부를 제어 합니다.

그 결과, 페이지 콘텐츠를 모든 Iphone에 안전 하 게 화면 영역에 배치할 수 있습니다.

[![](page-safe-area-images/safe-area-layout.png "Safe Area Layout Guide")](page-safe-area-images/safe-area-layout-large.png#lightbox "Safe Area Layout Guide")

> [!NOTE]
> Apple에서 정의한 안전 영역은에서 Xamarin.Forms 속성을 설정 하는 데 사용 되며 [`Page.Padding`](xref:Xamarin.Forms.Page.Padding) 설정 된이 속성의 이전 값을 재정의 합니다.

네임 스페이스의 메서드를 사용 하 여 해당 값을 검색 하 여 안전 영역을 사용자 지정할 수 있습니다 [`Thickness`](xref:Xamarin.Forms.Thickness) `Page.SafeAreaInsets` [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) . 그런 다음 필요에 따라 수정 하 고 `Padding` 페이지 생성자 또는 재정의에서 속성에 다시 할당할 수 있습니다 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) .

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
