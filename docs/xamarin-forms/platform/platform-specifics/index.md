---
title: 플랫폼 특성
description: 플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: eb0933aeda7438e0d1f81a9b9d0ef35ceb543a1b
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2018
---
# <a name="platform-specifics"></a>플랫폼 특성

_플랫폼 비슷하므로 허용 사용자 지정 렌더러 또는 효과 구현 하지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다._

다음 플랫폼 특정 기능 Xamarin.Forms에 기본 제공 됩니다.

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[MasterDetailPage.CollapsedPaneWidth 및 MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[VisualElement.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[Application.SendDisappearingEventOnPause, Application.SendAppearingEventOnResume, and Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|
|[Page.PrefersStatusBarHidden 및 Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|

플랫폼별 코드를 fluent API 또는 XAML을 통해 사용 하기 위한 프로세스는 다음과 같습니다.

1. 추가 `xmlns` 선언 또는 `using` 에 대 한 지시문은 [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) 네임 스페이스입니다.
1. 추가 `xmlns` 선언 또는 `using` 플랫폼 특정 기능이 포함 된 네임 스페이스에 대 한 지시문:
    1. 이것은 ios,는 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) 네임 스페이스입니다.
    1. 이 android에서는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) 네임 스페이스입니다. 이 Android AppCompat에 대 한는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) 네임 스페이스입니다.
    1. 이 유니버설 Windows 플랫폼에는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) 네임 스페이스입니다.
1. 사용 하 여 코드 또는 XAML에서 플랫폼별으로 적용는 `On<T>` fluent API입니다. 값 `T` 수는 [ `iOS` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOS/), [ `Android` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Android/), 또는 [ `Windows` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Windows/) 에서 형식을 [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) 네임 스페이스입니다.

> [!NOTE]
> 제공 되지 않는 플랫폼에서 플랫폼별 사용 하려고 시도 발생 하지 않을 것임 오류가 참고 합니다. 대신, 코드 적용 되는 플랫폼 특정 없이 실행 됩니다.

플랫폼 특성을 통해 사용는 `On<T>` fluent API 반환 코드 [ `IPlatformElementConfiguration` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IPlatformElementConfiguration%3CTPlatform,TElement%3E/) 개체입니다. 따라서 여러 플랫폼-비슷하므로 메서드 연계 된 동일한 개체에서 호출할 수 있습니다.

플랫폼 세부 사항에 대 한 자세한 내용은 참조 하십시오. [소비 플랫폼-세부 사항](~/xamarin-forms/platform/platform-specifics/consuming/index.md) 및 [만드는 플랫폼 비슷하므로](~/xamarin-forms/platform/platform-specifics/creating.md)합니다.


## <a name="related-links"></a>관련 링크

- [플랫폼별 사용](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)
