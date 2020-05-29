---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 005e8216b887b694b33916179ca276cf8091e006
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135982"
---
# <a name="main-thread-control-updates-on-ios"></a>IOS에서 주 스레드 제어 업데이트

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼에 따라 백그라운드 스레드에서 수행 되는 것이 아니라 컨트롤 레이아웃 및 렌더링 업데이트를 주 스레드에서 수행할 수 있습니다. 거의 필요 하지 않지만 경우에 따라 충돌을 방지할 수 있습니다. 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 되는 `Application.HandleControlUpdatesOnMainThread` 입니다 `true` .

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

`Application.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `Application.SetHandleControlUpdatesOnMainThread`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 백그라운드 스레드에서 수행 되는 대신 컨트롤 레이아웃 및 렌더링 업데이트가 주 스레드에서 수행 되는지 여부를 제어 하는 데 사용 됩니다. 또한 `Application.GetHandleControlUpdatesOnMainThread` 메서드를 사용 하 여 기본 스레드에서 컨트롤 레이아웃 및 렌더링 업데이트를 수행할지 여부를 반환할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
