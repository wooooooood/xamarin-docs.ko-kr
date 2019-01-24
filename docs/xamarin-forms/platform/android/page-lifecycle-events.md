---
title: Android의 페이지 수명 주기 이벤트
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에는 Android를 사용할 플랫폼별은 사용 하지 않도록 설정 된 Disappearing 방법과 Appearing 페이지 이벤트 응용 프로그램에서 일시 중지 및 재개를 각각 설명 합니다.
ms.prod: xamarin
ms.assetid: F6E3759C-D347-407A-91A2-CF9B3B7D4CBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: ab9c404fc9051014fd3a243848290087f43a46d2
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209308"
---
# <a name="page-lifecycle-events-on-android"></a>Android의 페이지 수명 주기 이벤트

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 Android 플랫폼별 사용 하지 않도록 설정 되는 [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) 하 고 [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) 페이지 이벤트 응용 프로그램에서 일시 중지 하 고 각각 AppCompat를 사용 하는 응용 프로그램에 대 한 다시 시작 합니다. 소프트 키보드는 소프트 키보드의 운영 모드를로 일시 중지에 표시 된 경우 다시 시작할 때 표시 되는지 여부를 제어 하는 기능 포함 하는 또한 [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize)합니다.

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

[![](page-lifecycle-events-images/keyboard-on-resume.png "수명 주기 이벤트 플랫폼별")](page-lifecycle-events-images/keyboard-on-resume-large.png#lightbox "수명 주기 이벤트 플랫폼별")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
