---
title: IOS에서 주 스레드 제어 업데이트
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 기본 스레드에서 컨트롤 레이아웃 및 렌더링 업데이트를 수행할 수 있도록 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 945E711D-9BD2-4BF9-9FB3-CBE0D5B25A49
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 9603cccc1f08be057bc66012cdde75e1b7391f1a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655273"
---
# <a name="main-thread-control-updates-on-ios"></a>IOS에서 주 스레드 제어 업데이트

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼에 따라 백그라운드 스레드에서 수행 되는 것이 아니라 컨트롤 레이아웃 및 렌더링 업데이트를 주 스레드에서 수행할 수 있습니다. 거의 필요 하지, 있지만 일부의 경우에서 충돌 하지 못할 수 있습니다. 설정 하 여 해당 사용된에서 XAML 합니다 `Application.HandleControlUpdatesOnMainThread` 바인딩 가능한 속성을 `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

`Application.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Application.SetHandleControlUpdatesOnMainThread` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스는 사용 제어 되는지 여부를 제어 레이아웃 및 렌더링 업데이트 백그라운드 스레드에서 수행 되는 대신 주 스레드에서 수행 됩니다. 또한는 `Application.GetHandleControlUpdatesOnMainThread` 메서드를 사용 하 여 컨트롤 레이아웃 및 렌더링 업데이트 주 스레드에서 수행 되는 여부를 반환할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
