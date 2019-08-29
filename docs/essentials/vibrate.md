---
title: 'Xamarin.Essentials: Vibration'
description: 이 문서에서는 원하는 시간 동안 진동 기능을 시작 및 중지할 수 있는 Xamarin.Essentials의 Vibration 클래스를 설명합니다.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 2e4cf713f9ad7478c0d8e288fd3beff4b5015ef5
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120111"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: Vibration

**Vibration** 클래스를 사용하면 원하는 시간 동안 진동 기능을 시작 및 중지할 수 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

**진동** 기능에 액세스하려면 다음 플랫폼 관련 설정이 필요합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Vibrate 권한이 필요하며 Android 프로젝트에서 구성해야 합니다. 이 권한은 다음과 같은 방법으로 추가할 수 있습니다.

**속성** 폴더 아래의 **AssemblyInfo.cs** 파일을 열고 다음을 추가합니다.

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

또는 Android 매니페스트를 업데이트합니다.

**속성** 폴더 아래의 **AndroidManifest.xml** 파일을 열고 **매니페스트** 노드 내부에 다음을 추가합니다.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

또는 Android 프로젝트를 마우스 오른쪽 단추로 클릭하고 프로젝트의 속성을 엽니다. **Android 매니페스트** 아래에서 **필요한 권한:** 영역을 찾아 **VIBRATE** 권한을 확인합니다. 그러면 **AndroidManifest.xml** 파일이 자동으로 업데이트됩니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설정이 필요하지 않습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

플랫폼의 차이점이 없습니다.

-----

## <a name="using-vibration"></a>진동 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

Vibration 기능은 설정된 시간 또는 기본값 500밀리초 동안 요청할 수 있습니다.

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

디바이스 진동의 취소는 `Cancel` 메서드를 사용하여 요청할 수 있습니다.

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="platform-differences"></a>플랫폼의 차이점

# <a name="androidtabandroid"></a>[Android](#tab/android)

플랫폼의 차이점이 없습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

- 디바이스가 “전화벨이 울리면 진동”으로 설정된 경우에만 진동합니다.
- 항상 500밀리초 동안 진동합니다.
- 진동을 취소할 수 없습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

플랫폼의 차이점이 없습니다.

-----

## <a name="api"></a>API

- [진동 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [진동 API 문서](xref:Xamarin.Essentials.Vibration)
