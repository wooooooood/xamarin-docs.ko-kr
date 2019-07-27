---
title: Android의 소프트 키보드 입력 모드
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 소프트 키보드 입력 영역에 대 한 운영 모드를 설정 하는 Android 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: AFCDC92F-F61E-42F6-904B-50B5C4949970
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 1a2e770049eac215ac4fdd59bd67451cb7b8eacf
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511782"
---
# <a name="soft-keyboard-input-mode-on-android"></a>Android의 소프트 키보드 입력 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

이 Android 플랫폼별는 소프트 키보드 입력 영역에 대 한 운영 모드를 설정 하는 데 사용 되며 [`Application.WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) 연결 된 속성을 [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다.

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

`Application.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. 합니다 [ `Application.UseWindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스를 사용 하 여 소프트 키보드 입력된 영역 운영 모드를 설정 하는 합니다 [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) 두 값을 제공 하는 열거형: [ `Pan` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) 하 고 [ `Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize)합니다. 합니다 `Pan` 사용 하 여 값을 [ `AdjustPan` ](xref:Android.Views.SoftInput.AdjustPan) 입력된 컨트롤에 포커스가 있을 때 창 크기가 조정 되지 않는 조정 옵션을 합니다. 대신 창의 내용은 이동 하는 현재 포커스 소프트 키보드 가려져 되지 않도록 합니다. 합니다 `Resize` 사용 하 여 값을 [ `AdjustResize` ](xref:Android.Views.SoftInput.AdjustResize) 입력된 컨트롤에 포커스를 확보 하기 위해 소프트 키보드 창의 크기를 조정 하는 조정 옵션입니다.

결과 소프트 키보드 입력 영역 입력된 컨트롤에 포커스가 있을 때 운영 모드를 설정할 수 있습니다는.

[![](soft-keyboard-input-mode-images/pan-resize.png "운영 모드 플랫폼별 소프트 키보드")](soft-keyboard-input-mode-images/pan-resize-large.png#lightbox "운영 모드 플랫폼별 소프트 키보드")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
