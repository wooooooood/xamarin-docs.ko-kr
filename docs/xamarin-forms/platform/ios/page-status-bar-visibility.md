---
제목: "iOS의 페이지 상태 표시줄 표시 유형" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 페이지에서 상태 표시줄의 표시 여부를 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다. "
assetid: D8BB7C24-A27F-4758-8557-6A81F909ABD9: xamarin-forms author: davidbritch: dabritch:: 10/24/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="page-status-bar-visibility-on-ios"></a>IOS의 페이지 상태 표시줄 표시 유형

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별는 상태 표시줄의 표시 여부를 설정 하는 데 사용 되며, 상태 표시줄의 표시 여부를 제어 하는 [`Page`](xref:Xamarin.Forms.Page) 기능을 포함 합니다 `Page` . `Page.PrefersStatusBarHidden`연결 된 속성을 열거형의 값으로 설정 하 `StatusBarHiddenMode` 고 선택적으로 `Page.PreferredStatusBarUpdateAnimation` 연결 된 속성을 열거형 값으로 설정 하 여 XAML에서 사용 됩니다 `UIStatusBarAnimation` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `Page.SetPrefersStatusBarHidden`네임 스페이스의 메서드는 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` [`Page`](xref:Xamarin.Forms.Page) 열거형 값 중 하나 (, `StatusBarHiddenMode` `Default` `True` 또는 `False` )를 지정 하 여에서 상태 표시줄의 표시 여부를 설정 하는 데 사용 됩니다. `StatusBarHiddenMode.True`및 `StatusBarHiddenMode.False` 값은 장치 방향에 관계 없이 상태 표시줄의 표시 여부를 설정 하 고, `StatusBarHiddenMode.Default` 값은 수직 컴팩트 환경에서 상태 표시줄을 숨깁니다.

따라서에 대 한 상태 표시줄의 표시 여부를 [`Page`](xref:Xamarin.Forms.Page) 설정할 수 있습니다.

![](page-status-bar-visibility-images/hide-status-bar.png "Status Bar Visibility Platform-Specific")

> [!NOTE]
> 에서 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 지정 된 `StatusBarHiddenMode` 열거형 값은 모든 자식 페이지의 상태 표시줄도 업데이트 합니다. 다른 모든 [`Page`](xref:Xamarin.Forms.Page) 파생 형식에서 지정 된 `StatusBarHiddenMode` 열거형 값은 현재 페이지의 상태 표시줄만 업데이트 합니다.

메서드는 열거형 값 중 하나 (, 또는)를 `Page.SetPreferredStatusBarUpdateAnimation` 지정 하 여 상태 표시줄에서를 입력 하거나 그 뒤에 두는 방법을 설정 하는 데 사용 됩니다. [`Page`](xref:Xamarin.Forms.Page) `UIStatusBarAnimation` `None` `Fade` `Slide` `Fade`또는 `Slide` 열거형 값이 지정 된 경우 상태 표시줄에서를 입력 하거나 벗어나면 0.25 번째 애니메이션이 실행 됩니다 `Page` .

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
