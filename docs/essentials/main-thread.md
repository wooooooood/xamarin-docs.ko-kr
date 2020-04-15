---
title: 'Xamarin.Essentials: MainThread'
description: MainThread 클래스를 사용하면 애플리케이션이 주 실행 스레드에서 코드를 실행할 수 있습니다.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: jamesmontemagno
ms.author: jamont
ms.date: 08/20/2019
ms.openlocfilehash: 9109e7bff4cfe60479e711240d290d77b60a9af6
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "70060123"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

**MainThread** 클래스를 사용하면 애플리케이션이 주 실행 스레드에서 코드를 실행할 수 있으며, 특정 코드 블록이 현재 주 스레드에서 실행되고 있는지 확인할 수 있습니다.

## <a name="background"></a>배경

iOS, Android 및 유니버설 Windows 플랫폼을 포함한 대부분의 운영 체제는 사용자 인터페이스와 관련된 코드에 대해 단일 스레딩 모델을 사용합니다. 이 모델은 키 입력 및 터치 입력을 포함한 사용자 인터페이스 이벤트를 올바르게 직렬화하는 데 필요합니다. 이 스레드를 _주 스레드_, _사용자 인터페이스 스레드_ 또는 _UI 스레드_라고 합니다. 이 모델의 단점은 사용자 인터페이스 요소에 액세스하는 모든 코드가 애플리케이션의 주 스레드에서 실행되어야 한다는 것입니다. 

애플리케이션이 보조 실행 스레드의 이벤트 처리기를 호출하는 이벤트를 사용해야 하는 경우도 있습니다. (Xamarin.Essentials 클래스 [`Accelerometer`](accelerometer.md), [`Compass`](compass.md), [`Gyroscope`](gyroscope.md), [`Magnetometer`](magnetometer.md) 및 [`OrientationSensor`](orientation-sensor.md)는 더 빠른 속도로 사용할 경우 모두 보조 스레드에 대한 정보를 반환할 수 있습니다.) 이벤트 처리기가 사용자 인터페이스 요소에 액세스해야 하는 경우, 주 스레드에서 해당 코드를 실행해야 합니다. **MainThread** 클래스를 사용하면 애플리케이션이 주 실행 스레드에서 이 코드를 실행할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="running-code-on-the-main-thread"></a>주 스레드에서 코드 실행

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

주 스레드에서 코드를 실행하려면 static `MainThread.BeginInvokeOnMainThread` 메서드를 호출합니다. 인수는 [`Action`](xref:System.Action) 개체로, 단순히 인수와 반환 값이 없는 메서드입니다.

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

주 스레드에서 실행해야 하는 코드에 대해 별도의 메서드를 정의할 수도 있습니다.

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

그런 다음, `BeginInvokeOnMainThread` 메서드에서 참조하여 주 스레드에서 이 메서드를 실행할 수 있습니다.

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms에는 [`Device.BeginInvokeOnMainThread(Action)`](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread)라는 메서드가 있으며,
> `MainThread.BeginInvokeOnMainThread(Action)`와 동일한 작업을 수행합니다. Xamarin.Forms 앱에서 두 메서드 중 하나를 사용할 수 있지만, 호출 코드에 Xamarin.Forms에 대한 다른 종속성 요구가 있는지 여부를 고려합니다. 다른 종속성 요구가 없으면 `MainThread.BeginInvokeOnMainThread(Action)`가 더 나은 옵션일 가능성이 큽니다.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>코드가 주 스레드에서 실행되고 있는지 확인

`MainThread` 클래스를 사용하면 애플리케이션은 특정 코드 블록이 주 스레드에서 실행되고 있는지도 확인할 수 있습니다. `IsMainThread` 속성은 해당 속성을 호출하는 코드가 주 스레드에서 실행되고 있는 경우 `true`를 반환합니다. 프로그램은 이 속성을 사용하여 주 스레드 또는 보조 스레드에 대해 서로 다른 코드를 실행할 수 있습니다.

```csharp
if (MainThread.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

예를 들어 `BeginInvokeOnMainThread`를 호출하기 전에 다음과 같이 코드가 보조 스레드에서 실행되고 있는지 확인해야 한다고 생각할 수 있습니다.

```csharp
if (MainThread.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

코드 블록이 이미 주 스레드에서 실행되고 있다면 이 검사를 통해 성능을 향상할 수 있지 않을까 하는 의심을 가질 수 있습니다.

_그러나 이 검사는 필요하지 않습니다._ 플랫폼의 `BeginInvokeOnMainThread` 구현 자체에서 호출이 주 스레드에서 수행되었는지 확인합니다. 필요하지 않을 때 `BeginInvokeOnMainThread`를 호출해도 성능에 미치는 영향은 거의 없습니다.

## <a name="additional-methods"></a>추가 방법

`MainThread` 클래스에는 백그라운드 스레드의 사용자 인터페이스 요소와 상호 작용하는 데 사용될 수 있는 다음 추가 `static` 메서드가 포함됩니다.

| 메서드 | 인수 | 반환 값 | 용도 |
|---|---|---|---|
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | 주 스레드에서 `Func<T>`를 호출하고 완료될 때까지 기다립니다. |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | 주 스레드에서 `Action`을 호출하고 완료될 때까지 기다립니다. |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | 주 스레드에서 `Func<Task<T>>`를 호출하고 완료될 때까지 기다립니다. |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | 주 스레드에서 `Func<Task>`를 호출하고 완료될 때까지 기다립니다. |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | 주 스레드의 `SynchronizationContext`를 반환합니다. |

## <a name="api"></a>API

- [MainThread 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread API 문서](xref:Xamarin.Essentials.MainThread)
