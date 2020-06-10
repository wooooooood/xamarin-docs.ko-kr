---
제목: "iOS의 TabbedPage 탭 모음" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 TabbedPage에서 탭 모음의 반투명도 모드를 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
assetid: 9581AE81-9557-47AD-8B07-25A1AC5F055B: xamarin-forms author: davidbritch: dabritch:: 01/16/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>IOS의 TabbedPage 반투명 탭 모음

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼에 해당 하는는에서 탭 모음의 반투명도 모드를 설정 하는 데 사용 됩니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . `TabbedPage.TranslucencyMode`바인딩 가능한 속성을 열거형 값으로 설정 하 여 XAML에서 사용 됩니다 `TranslucencyMode` .

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

`TabbedPage.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `TabbedPage.SetTranslucencyMode`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 다음 열거형 값 중 하나를 지정 하 여에서 탭 모음의 반투명도 모드를 설정 하는 데 사용 됩니다 `TranslucencyMode` .

- `Default`-탭 표시줄을 기본 반투명도 모드로 설정 합니다. 이 값은 `TabbedPage.TranslucencyMode` 속성의 기본값입니다.
- `Translucent`탭 표시줄을 반투명 하 게 설정 하는입니다.
- `Opaque`-탭 막대를 불투명 하 게 설정 합니다.

또한 메서드를 사용 하 여에 `GetTranslucencyMode` `TranslucencyMode` 적용 되는 열거형의 현재 값을 검색할 수 있습니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

그러면에서 탭 모음의 반투명도 모드를 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 설정할 수 있습니다.

![IOS의 반투명 및 불투명 탭 막대 스크린샷](tabbedpage-translucent-tabbar-images/translucencymodes.png "반투명 및 불투명 탭 막대")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
