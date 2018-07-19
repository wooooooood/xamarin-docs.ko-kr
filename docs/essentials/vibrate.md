---
title: 'Xamarin.Essentials: 진동'
description: 이 문서에서 시작 하 고 원하는 기간에 대 한 진동 기능을 중지할 수 있는 Xamarin.Essentials 진동 클래스를 설명 합니다.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 530273543c6cb71038613c22fa4a6bfbde4928d7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947259"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: 진동

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **진동** 클래스를 사용 하면 시작 하 고 원하는 기간에 대 한 진동 기능을 중지 합니다.

## <a name="getting-started"></a>시작

액세스 하는 **진동** 플랫폼 특정 설정 기능은 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

진동 권한이 필요 하며 Android 프로젝트에 구성 되어야 합니다. 이 다음과 같은 방법으로 추가할 수 있습니다.

엽니다는 **AssemblyInfo.cs** 파일을 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기를 **AndroidManifest.xml** 파일을 **속성** 폴더 내부에 다음 줄을 추가 합니다 **매니페스트** 노드.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

또는 Anroid 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 합니다 **필요한 권한:** 영역과 확인을 **진동** 권한. 이 자동으로 업데이트 합니다 **AndroidManifest.xml** 파일입니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설정이 필요 없습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

플랫폼 차이점이 없습니다.

-----

## <a name="using-vibration"></a>진동을 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

일정 시간 동안에 기본값인 500 밀리초 진동 기능을 요청할 수 있습니다.

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

사용 하 여 장치 진동의 취소를 요청할 수 있습니다는 `Cancel` 메서드:

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

플랫폼 차이점이 없습니다.

# <a name="iostabios"></a>[iOS](#tab/ios)

* 장치 "링에서 진동"로 설정 된 경우 진동만 합니다.
* 500 밀리초 동안 진동 항상 있습니다.
* 진동을 취소 하는 것이 불가능 합니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

플랫폼 차이점이 없습니다.

-----

## <a name="api"></a>API

- [진동 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [진동 API 설명서](xref:Xamarin.Essentials.Vibration)
