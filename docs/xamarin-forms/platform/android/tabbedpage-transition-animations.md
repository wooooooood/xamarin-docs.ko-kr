---
제목: "Android의 TabbedPage 페이지 전환 애니메이션" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 TabbedPage에서 페이지를 탐색할 때 전환 애니메이션을 사용 하지 않도록 설정 하는 Android 플랫폼별를 사용 하는 방법을 설명 합니다. "
assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3: xamarin-forms author: dabritch:: ms. date: 07/10/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="tabbedpage-page-transition-animations-on-android"></a>Android의 TabbedPage 페이지 전환 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

에서 프로그래밍 방식으로 또는 탭 모음을 사용 하는 경우에서 페이지를 탐색 하는 경우이 Android 플랫폼별를 사용 하 여 전환 애니메이션을 사용 하지 않도록 설정 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 합니다. 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 됩니다 `TabbedPage.IsSmoothScrollEnabled` `false` .

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

`TabbedPage.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. `TabbedPage.SetIsSmoothScrollEnabled`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 의 페이지를 탐색할 때 전환 애니메이션을 표시할지 여부를 제어 하는 데 사용 됩니다 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . 또한 `TabbedPage` 네임 스페이스의 클래스에는 `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` 다음 메서드도 있습니다.

- `IsSmoothScrollEnabled`는의 페이지를 탐색할 때 전환 애니메이션을 표시할지 여부를 검색 하는 데 사용 됩니다 `TabbedPage` .
- `EnableSmoothScroll`는의 페이지 간을 탐색할 때 전환 애니메이션을 사용 하도록 설정 하는 데 사용 됩니다 `TabbedPage` .
- `DisableSmoothScroll`는의 페이지 간을 탐색할 때 전환 애니메이션을 사용 하지 않도록 설정 하는 데 사용 됩니다 `TabbedPage` .

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
