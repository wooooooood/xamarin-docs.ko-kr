---
title: Xamarin.Essentials 손전등
description: 손전등 클래스에 켜기 / 끄기 장치의 카메라 플래시 손전등으로 만들 수 있습니다.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f187fa404df09e387ed870f524239d3baabfdd3f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials 손전등

![시험판 NuGet](~/media/shared/pre-release.png)

**손전등** 클래스 켜기 / 끄기 장치의 카메라 플래시 손전등으로 만들 수 있습니다.

## <a name="getting-started"></a>시작

액세스는 **손전등** 는 다음과 같은 플랫폼 특정 설치 기능이 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

카메라 권한과 손전등 필수 필드 이며 Android 프로젝트에서 구성 해야 합니다. 다음과 같은 방법으로 추가할 수 있습니다.

열기는 **AssemblyInfo.cs** 에서 파일의 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기는 **AndroidManifest.xml** 아래 파일는 **속성** 폴더 내에 다음 코드를 추가 하 고는 **매니페스트** 노드.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

또는 Anroid 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 **필요한 권한:** 영역 및 검사는 **손전등** 및 **카메라** 사용 권한. 이 자동으로 업데이트는 **AndroidManifest.xml** 파일입니다.

이러한 권한을 추가 하 여 [장치 필터링 하 고 자동으로 Google Play](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) 특정 하드웨어 없이 합니다. Android 프로젝트의 AssemblyInfo.cs 파일에 추가 하 여이 문제를 해결할 수 있습니다.

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설치 하지 않아도 됩니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설치 하지 않아도 됩니다.

-----

## <a name="using-flashlight"></a>손전등을 사용 하 여

클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

손전등 설정할 수 있습니다 및 해제를 통해는 `TurnOnAsync` 및 `TurnOffAsync` 메서드:

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

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 사항

### <a name="androidtabandroid-specifics"></a>[Android](#tab/android-specifics)

손전등 클래스에는 장치의 운영 체제에 따라 optmized 되었습니다.

#### <a name="api-level-23-and-higher"></a>API 수준 23 이상

새 API 수준에서 [다 모드](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) 켜기 / 끄기는 장치의 플래시 단위를 사용 합니다.

#### <a name="api-level-22-and-lower"></a>API 수준 22 및 하 한

카메라 표면 질감 켜기 / 끄기 하기 위해 만들어지는 `FlashMode` 카메라 단위입니다. 

### <a name="iostabios-specifics"></a>[iOS](#tab/ios-specifics)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) 설정 및 다 및 장치의 플래시 모드를 해제 하는 데 사용 됩니다.

### <a name="uwptabuwp-specifics"></a>[UWP](#tab/uwp-specifics)

[램프](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) 설정 하거나 해제 하려면 장치 뒷면에서 첫 번째 램프 검색 하는 데 사용 됩니다.

-----

## <a name="api"></a>API

- [손전등 소스 코드](https://github.com/xamarin/Essentials/tree/master/Essentials/Flashlight)
- [손전등 API 설명서](xref:Xamarin.Essentials.Flashlight)
