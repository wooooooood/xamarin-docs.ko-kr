---
제목: "iOS의 탐색 페이지 표시줄 구분 기호" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 NavigationPage의 탐색 모음 아래쪽에 있는 구분선 및 그림자를 숨기는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다. "
assetid: 5A45748A-6779-4441-82F2-415BD68473B9: xamarin-forms author: davidbritch: dabritch:: 10/24/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="navigationpage-bar-separator-on-ios"></a>IOS의 NavigationPage 표시줄 구분 기호

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 전용은의 탐색 모음 아래쪽에 있는 구분선 및 그림자를 숨깁니다 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 됩니다 [`NavigationPage.HideNavigationBarSeparator`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.HideNavigationBarSeparatorProperty) `false` .

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ios:NavigationPage.HideNavigationBarSeparator="true">

</NavigationPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;

public class iOSTitleViewNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public iOSTitleViewNavigationPageCS()
    {
        On<iOS>().SetHideNavigationBarSeparator(true);
    }
}
```

`NavigationPage.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [ `NavigationPage.SetHideNavigationBarSeparator` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . NavigationPage}, system.string) 메서드를 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 사용 하 여 탐색 모음 구분 기호를 숨길지 여부를 제어 합니다. 또한 [ `NavigationPage.HideNavigationBarSeparator` ] (f: Xamarin.Forms 입니다. PlatformConfiguration. iOSSpecific ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. iOS, Xamarin.Forms . NavigationPage}) 메서드를 사용 하 여 탐색 모음 구분 기호를 숨길지 여부를 반환할 수 있습니다.

따라서의 탐색 모음 구분 기호를 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 숨길 수 있습니다.

![](navigation-bar-separator-images/navigationpage-hideseparatorbar.png "NavigationPage navigation bar hidden")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
