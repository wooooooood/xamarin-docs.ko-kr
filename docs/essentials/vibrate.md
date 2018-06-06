---
title: 'Xamarin.Essentials: 진동'
description: 이 문서에서 시작 하 고 원하는 기간 동안 vibrate 기능을 중지할 수 있는 Xamarin.Essentials 진동 클래스를 설명 합니다.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2a2902ca4eac8b889f6875580d7cb4ea352803a8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782923"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: 진동

![시험판 NuGet](~/media/shared/pre-release.png)

**진동** 클래스 사용 시작 하 고 원하는 기간 동안 vibrate 기능을 중지할 수 있습니다.

## <a name="getting-started"></a>시작

액세스는 **진동** 는 다음과 같은 플랫폼 특정 설치 기능이 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Vibrate 권한이 필요 하며 Android 프로젝트에서 구성 해야 합니다. 다음과 같은 방법으로 추가할 수 있습니다.

열기는 **AssemblyInfo.cs** 에서 파일의 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기는 **AndroidManifest.xml** 아래 파일는 **속성** 폴더 내에 다음 코드를 추가 하 고는 **매니페스트** 노드.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

또는 Anroid 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 **필요한 권한:** 영역 및 검사는 **VIBRATE** 권한. 이 자동으로 업데이트는 **AndroidManifest.xml** 파일입니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설치 하지 않아도 됩니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설치 하지 않아도 됩니다.

-----

## <a name="using-vibration"></a>진동을 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

진동 기능 일정 시간 동안 또는 500 밀리초의 기본값을 요청할 수 있습니다.

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

장치가 진동의 취소를 사용 하 여 요청 수는 `Cancel` 메서드:

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

| 플랫폼 | 차이 |
| --- | --- |
| iOS | 500 밀리초 동안 진동 항상 합니다. |
| iOS | 진동을 취소 하는 것이 불가능 합니다. |

## <a name="api"></a>API

- [진동 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [진동 API 설명서](xref:Xamarin.Essentials.Vibration)
