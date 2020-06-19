---
title: Android의 소프트 키보드 입력 모드
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 소프트 키보드 입력 영역에 대 한 운영 모드를 설정 하는 Android 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: AFCDC92F-F61E-42F6-904B-50B5C4949970
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c62d09c7d7848d9f62c018caa1698bb53a2a39a8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128715"
---
# <a name="soft-keyboard-input-mode-on-android"></a>Android의 소프트 키보드 입력 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼별는 소프트 키보드 입력 영역에 대 한 운영 모드를 설정 하는 데 사용 되며 [`Application.WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) 연결 된 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) .

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. [ `Application.UseWindowSoftInputModeAdjust` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. AndroidSpecific. Application. Usewindow소프트 Inputmode조정 ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . 응용 프로그램}, Xamarin.Forms . PlatformConfiguration. AndroidSpecific Windowsoft Inputmode조정) 메서드는 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에서 및의 두 값을 제공 하는 열거형을 사용 하 여 소프트 키보드 입력 영역 운영 모드를 설정 하는 데 사용 됩니다. [`WindowSoftInputModeAdjust`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) [`Pan`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) [`Resize`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize) `Pan`값은 [`AdjustPan`](xref:Android.Views.SoftInput.AdjustPan) 조정 옵션을 사용 합니다 .이 옵션은 입력 컨트롤에 포커스가 있을 때 창의 크기를 조정 하지 않습니다. 대신 창의 내용이 따라 이동 되어 현재 포커스가 소프트 키보드에 의해 가려지지 않습니다. 이 `Resize` 값은 [`AdjustResize`](xref:Android.Views.SoftInput.AdjustResize) 입력 컨트롤에 포커스가 있을 때 창의 크기를 조정 하는 조정 옵션을 사용 하 여 소프트 키보드의 공간을 만듭니다.

그 결과 입력 컨트롤에 포커스가 있을 때 소프트 키보드 입력 영역 운영 모드를 설정할 수 있습니다.

[![](soft-keyboard-input-mode-images/pan-resize.png "Soft Keyboard Operating Mode Platform-Specific")](soft-keyboard-input-mode-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Platform-Specific")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
