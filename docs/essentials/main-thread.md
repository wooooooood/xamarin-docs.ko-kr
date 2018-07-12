---
title: 'Xamarin.Essentials: MainThread'
description: MainThread 클래스를 사용 하면 응용 프로그램의 주 실행 스레드에서 코드 실행 합니다.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831427"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **MainThread** 클래스를 사용 하면 응용 프로그램의 주 스레드에서 실행의 코드를 실행 하 고 특정 코드 블록을 확인 하려면 현재 주 스레드에서 실행 됩니다.

## <a name="background"></a>배경

대부분의 운영 체제-iOS, Android 및 유니버설 Windows 플랫폼을 포함 하 여, 사용자 인터페이스와 관련 된 코드에 대 한 단일 스레딩 모델을 사용 합니다. 이 모델은 제대로 키 입력을 포함 하 여 사용자 인터페이스 이벤트를 직렬화 하 고 터치식 입력 해야 합니다. 이 스레드가 라고 합니다 _주 스레드에서_ 또는 _사용자 인터페이스 스레드_ 또는 _UI 스레드_합니다. 이 모델의 단점은 사용자 인터페이스 요소에 액세스 하는 모든 코드는 응용 프로그램의 주 스레드에서 실행 해야 합니다. 

경우에 따라 응용 프로그램 실행의 보조 스레드에서 이벤트 처리기를 호출 하는 이벤트를 사용 해야 합니다. (Xamarin.Essentials 클래스 [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md)하십시오 [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), 및 [ `OrientationSensor` ](orientation-sensor.md) 모든 빠른 속도 사용 하는 경우 보조 스레드에서 정보를 반환할 수 있습니다.) 이벤트 처리기를 사용자 인터페이스 요소에 액세스 해야 하는 경우 주 스레드에서 코드를 실행 해야 합니다. 합니다 **MainThread** 클래스를 사용 하면 응용 프로그램이 주 스레드에서이 코드를 실행 합니다.

## <a name="running-code-on-the-main-thread"></a>주 스레드에서 코드 실행

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

주 스레드에서 코드를 실행 하려면 정적 호출 `MainThread.BeginInvokeOnMainThread` 메서드. 인수가 [ `Action` ](xref:System.Action) 개체 인수 및 반환 값이 없는 메서드 일 뿐입니다.

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

주 스레드에서 실행 해야 하는 코드에 대 한 별도 메서드를 정의할 수 이기도 합니다.

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

그러면이 메서드는 주 스레드에서 참조 하 여 실행할 수 있습니다는 `BeginInvokeOnMainThread` 메서드:

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms 라는 메서드가 [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) 와 동일한 작업을 수행 하는 `MainThread.BeginInvokeOnMainThread(Action)`합니다. 두 방법 중 하나를 사용 하 여 Xamarin.Forms 앱에서 호출 코드에서 Xamarin.Forms 종속성에 대 한 다른 필요가 있는지 여부를 고려해 야 합니다. 그러지 않으면 `MainThread.BeginInvokeOnMainThread(Action)` 가능성이 더 나은 옵션입니다.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>주 스레드에서 코드 실행 되 고 있는지 확인 합니다.

`MainThread` 클래스에서는 응용 프로그램을 특정 코드 블록을 주 스레드에서 실행 되 고 있는지 확인 합니다. 합니다 `IsMainThread` 속성이 반환 `true` 속성을 호출 하는 코드는 주 스레드에서 실행 중인 경우. 프로그램을이 속성을 사용 하 여 주 스레드가 보조 스레드에 다른 코드를 실행할 수 있습니다.

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

코드 호출 하기 전에 보조 스레드에서 실행 되 고 있는지 확인 해야 하는 경우 궁금할 `BeginInvokeOnMainThread`, 예를 들어, 다음과 같은:

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

아실 수도 있지만 코드 블록의 주 스레드에서 이미 실행 중인 경우이 검사 성능을 향상 시킬 수 있습니다.

_그러나이 검사 필요는 없습니다._ 플랫폼 구현 `BeginInvokeOnMainThread` 경우 호출 됩니다. 주 스레드에서 자체 확인 합니다. 호출 하는 경우 거의 성능 저하가 발생 `BeginInvokeOnMainThread` 반드시 필요한 경우.

## <a name="api"></a>API

- [MainThread 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread API 설명서](xref:Xamarin.Essentials.MainThread)
