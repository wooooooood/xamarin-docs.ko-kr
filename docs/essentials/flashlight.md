---
title: 'Xamarin.Essentials: 손전등'
description: 이 문서에서 Xamarin.Essentials 켜기 / 끄기 장치의 카메라 플래시 손전등으로 변환할 수 있는 손전등 클래스를 설명 합니다.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8c471f64c14a2e41693c450e02f89e7ac845d060
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353362"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials: 손전등

![시험판 버전 NuGet](~/media/shared/pre-release.png)

합니다 **손전등** 클래스 켜기 / 끄기 장치의 카메라 플래시 손전등으로 변환할 수 있습니다.

## <a name="getting-started"></a>시작

액세스 하는 **손전등** 플랫폼 특정 설정 기능은 필요 합니다.

# <a name="androidtabandroid"></a>[Android](#tab/android)

손전등 및 카메라 필요한 사용 권한과 Android 프로젝트에 구성 되어야 합니다. 이 다음과 같은 방법으로 추가할 수 있습니다.

엽니다는 **AssemblyInfo.cs** 파일을 **속성** 폴더 추가:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

또는 Android 매니페스트를 업데이트 합니다.

열기를 **AndroidManifest.xml** 파일을 **속성** 폴더 내부에 다음 줄을 추가 합니다 **매니페스트** 노드.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

또는 Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 프로젝트의 속성을 엽니다. 아래 **Android 매니페스트** 찾을 **필요한 권한:** 영역 및 확인 합니다 **손전등** 및 **카메라** 권한. 이 자동으로 업데이트 합니다 **AndroidManifest.xml** 파일입니다.

이러한 권한을 추가 하 여 [Google Play 장치 자동으로 필터링](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) 특정 하드웨어 없이 합니다. Android 프로젝트의 AssemblyInfo.cs 파일에 다음을 추가 하 여이 문제를 해결할 수 있습니다.

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

추가 설정이 필요 없습니다.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

추가 설정이 필요 없습니다.

-----

## <a name="using-flashlight"></a>손전등을 사용 하 여

클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

```csharp
using Xamarin.Essentials;
```

손전등 설정할 수 있습니다 및 해제를 통해 합니다 `TurnOnAsync` 고 `TurnOffAsync` 메서드:

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

## <a name="platform-implementation-specifics"></a>플랫폼 구현 세부 정보

### <a name="androidtabandroid"></a>[Android](#tab/android)

장치의 운영 체제에 따라 손전등 클래스는 최적화 되었습니다.

#### <a name="api-level-23-and-higher"></a>API 수준 23 이상

최신 API 수준에 [Torch 모드](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) 장치의 플래시 단위를 켜거나 하 게 됩니다.

#### <a name="api-level-22-and-lower"></a>API 레벨 22 및 하 한

카메라 표면 텍스처를 켜거나 끄려면 만들어집니다는 `FlashMode` 카메라 단위입니다. 

### <a name="iostabios"></a>[iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) 켜고 Torch 및 장치의 플래시 모드를 해제 하는 데 사용 됩니다.

### <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[Lamp](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) 켜기 / 끄기 장치의 뒷면에 첫 번째 lamp 검색에 사용 됩니다.

-----

## <a name="api"></a>API

- [손전등 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [손전등 API 설명서](xref:Xamarin.Essentials.Flashlight)
