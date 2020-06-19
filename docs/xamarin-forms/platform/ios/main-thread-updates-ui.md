---
title: IOS에서 주 스레드 제어 업데이트
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 기본 스레드에서 컨트롤 레이아웃 및 렌더링 업데이트를 수행할 수 있도록 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 945E711D-9BD2-4BF9-9FB3-CBE0D5B25A49
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 005e8216b887b694b33916179ca276cf8091e006
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
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
