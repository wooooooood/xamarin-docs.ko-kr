---
제목: "Android의 페이지 수명 주기 이벤트" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 응용 프로그램 일시 중지 및 다시 시작 시 각각 사라짐 및 표시 페이지 이벤트를 사용 하지 않도록 설정 하는 Android 플랫폼별를 사용 하는 방법을 설명 합니다. "
assetid: F6E3759C-D347-407A-91A2-CF9B3B7D4CBD: xamarin-forms author: davidbritch: dabritch:: 07/10/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="page-lifecycle-events-on-android"></a>Android의 페이지 수명 주기 이벤트

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼 전용은 [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) [`Appearing`](xref:Xamarin.Forms.Page.Appearing) AppCompat을 사용 하는 응용 프로그램에 대해 각각 응용 프로그램 일시 중지 및 다시 시작에서 및 페이지 이벤트를 사용 하지 않도록 설정 하는 데 사용 됩니다. 또한 소프트 키보드의 운영 모드가로 설정 된 경우 일시 중지 시 소프트 키보드가 표시 되는지 여부를 제어 하는 기능을 포함 합니다 [`WindowSoftInputModeAdjust.Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) .

> [!NOTE]
> 이러한 이벤트는 이벤트를 사용 하는 응용 프로그램에 대 한 기존 동작을 유지 하기 위해 기본적으로 사용 하도록 설정 됩니다. 이러한 이벤트를 사용 하지 않도록 설정 하면 AppCompat 이벤트 주기가 이전 AppCompat 이벤트 주기와 일치 합니다.

이 플랫폼 관련 [`Application.SendDisappearingEventOnPause`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty) 속성은, [`Application.SendAppearingEventOnResume`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty) 및 [`Application.ShouldPreserveKeyboardOnResume`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty) 연결 된 속성을 값으로 설정 하 여 XAML에서 사용 될 수 있습니다 `boolean` .

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

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

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

`Application.Current.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. [ `Application.SendDisappearingEventOnPause` ] (F: Xamarin.Forms 입니다. 플랫폼 구성. Xamarin.Forms SendDisappearingEventOnPause ()를 지정 합니다. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . Application}, system.string) 메서드를 사용 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) 하 여 [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) 응용 프로그램이 배경을 시작할 때 페이지 이벤트의 발생을 활성화 또는 비활성화 합니다. [ `Application.SendAppearingEventOnResume` ] (F: Xamarin.Forms 입니다. 플랫폼 구성. Xamarin.Forms SendAppearingEventOnResume ()를 지정 합니다. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . Application}, system.string) 메서드는 [`Appearing`](xref:Xamarin.Forms.Page.Appearing) 응용 프로그램이 백그라운드에서 다시 시작 될 때 페이지 이벤트의 발생을 활성화 하거나 비활성화 하는 데 사용 됩니다. [ `Application.ShouldPreserveKeyboardOnResume` ] (F: Xamarin.Forms 입니다. 플랫폼 구성. Xamarin.Forms ShouldPreserveKeyboardOnResume ()를 지정 합니다. IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . Application}, system.string) 메서드를 사용 하 여 소프트 키보드의 운영 모드가로 설정 된 경우 일시 중지 시 소프트 키보드가 표시 되는지 여부를 제어할 수 [`WindowSoftInputModeAdjust.Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) 있습니다.

결과적으로 응용 프로그램을 [`Disappearing`](xref:Xamarin.Forms.Page.Appearing) [`Appearing`](xref:Xamarin.Forms.Page.Appearing) 일시 중지 하 고 다시 시작 하는 동안 및 페이지 이벤트가 발생 하지 않으며 응용 프로그램이 일시 중지 될 때 소프트 키보드가 표시 되 면 응용 프로그램이 다시 시작 될 때에도 표시 됩니다.

[![](page-lifecycle-events-images/keyboard-on-resume.png "Lifecycle Events Platform-Specific")](page-lifecycle-events-images/keyboard-on-resume-large.png#lightbox "Lifecycle Events Platform-Specific")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
