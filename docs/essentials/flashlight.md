---
title: 'Xamarin.Essentials: 손전등'
description: 이 문서에서는 디바이스의 카메라 플래시를 켜거나 꺼서 손전등으로 전환하는 기능이 있는 Xamarin.Essentials의 Flashlight 클래스를 설명합니다.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: c2caf1583e3099903cb0b05628ed6b2984a954d9
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671418"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials: 손전등

**Flashlight** 클래스에는 디바이스의 카메라 플래시를 켜거나 꺼서 손전등으로 전환하는 기능이 있습니다.

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

**손전등** 기능에 액세스하려면 다음 플랫폼 관련 설정이 필요합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Flashlight 및 Camera 권한이 필요하며 Android 프로젝트에서 구성해야 합니다. 이 권한은 다음과 같은 방법으로 추가할 수 있습니다.

**속성** 폴더 아래의 **AssemblyInfo.cs** 파일을 열고 다음을 추가합니다.

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

또는 Android 매니페스트를 업데이트합니다.

**속성** 폴더 아래의 **AndroidManifest.xml** 파일을 열고 **매니페스트** 노드 내부에 다음을 추가합니다.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

또는 Android 프로젝트를 마우스 오른쪽 단추로 클릭하고 프로젝트의 속성을 엽니다. **Android 매니페스트** 아래에서 **필요한 권한:** 영역을 찾아 **FLASHLIGHT** 및 **CAMERA** 권한을 확인합니다. 그러면 **AndroidManifest.xml** 파일이 자동으로 업데이트됩니다.

이러한 권한을 추가하면 특정 하드웨어 없이 [Google Play에서 자동으로 디바이스를 필터링](https://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features)합니다. Android 프로젝트에서 AssemblyInfo.cs 파일에 다음을 추가하여 이를 처리할 수 있습니다.

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설정이 필요하지 않습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설정이 필요하지 않습니다.

-----

## <a name="using-flashlight"></a>손전등 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

`TurnOnAsync` 및 `TurnOffAsync` 메서드를 통해 손전등을 켜고 끌 수 있습니다.

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## <a name="platform-implementation-specifics"></a>플랫폼 구현 관련 정보

### <a name="androidtabandroid"></a>[Android](#tab/android)

Flashlight 클래스는 디바이스의 운영 체제에 따라 최적화되었습니다.

#### <a name="api-level-23-and-higher"></a>API 레벨 23 이상

최신 API 레벨에서 [손전등 모드](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode)는 디바이스의 플래시 디바이스를 켜거나 끄는 데 사용됩니다.

#### <a name="api-level-22-and-lower"></a>API 레벨 22 이하

카메라 표면 텍스처는 카메라 장치의 `FlashMode`을 켜거나 끄기 위해 만들어졌습니다. 

### <a name="iostabios"></a>[iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/)는 디바이스의 손전등 및 플래시 모드를 켜고 끄는 데 사용됩니다.

### <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[램프](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp)는 켜거나 끌 디바이스 뒷면에 있는 첫 번째 램프를 검색하는 데 사용됩니다.

-----

## <a name="api"></a>API

- [손전등 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [손전등 API 문서](xref:Xamarin.Essentials.Flashlight)
