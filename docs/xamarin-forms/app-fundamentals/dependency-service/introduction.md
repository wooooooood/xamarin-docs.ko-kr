---
title: Xamarin.Forms DependencyService 소개
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용하여 네이티브 플랫폼 기능을 호출하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/12/2019
ms.openlocfilehash: b27b4b0c3c5662c6cc1c2c151dd9ebe1523da3a4
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "71198523"
---
# <a name="xamarinforms-dependencyservice-introduction"></a>Xamarin.Forms DependencyService 소개

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)

[`DependencyService`](xref:Xamarin.Forms.DependencyService) 클래스는 Xamarin.Forms 애플리케이션이 공유 코드에서 네이티브 플랫폼 기능을 호출할 수 있도록 하는 서비스 로케이터입니다.

[`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용하여 네이티브 플랫폼 함수를 호출하는 프로세스는 다음과 같습니다.

1. 공유 코드에서 네이티브 플랫폼 기능용 인터페이스를 만듭니다. 자세한 내용은 [인터페이스 만들기](#create-an-interface)를 참조하세요.
1. 필요한 플랫폼 프로젝트에서 인터페이스를 구현합니다. 자세한 내용은 [각 플랫폼에서 인터페이스 구현](#implement-the-interface-on-each-platform)을 참조하세요.
1. [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 플랫폼 구현을 등록합니다. 이렇게 하면 Xamarin.Forms가 런타임에 플랫폼 구현을 찾을 수 있습니다. 자세한 내용은 [플랫폼 구현 등록](#register-the-platform-implementations)을 참조하세요.
1. 공유 코드에서 플랫폼 구현을 확인하고 호출합니다. 자세한 내용은 [플랫폼 구현 확인](#resolve-the-platform-implementations)을 참조하세요.

다음 다이어그램은 Xamarin.Forms 애플리케이션에서 네이티브 플랫폼 기능이 어떻게 호출되는지 보여 줍니다.

![Xamarin.Forms DependencyService 클래스를 사용하는 서비스 위치 개요](introduction-images/dependency-service.png "DependencyService 서비스 위치")

## <a name="create-an-interface"></a>인터페이스 만들기

공유 코드에서 네이티브 플랫폼 기능을 호출할 수 있는 첫 번째 단계는 네이티브 플랫폼 기능을 조작하기 위한 API를 정의하는 인터페이스를 만드는 것입니다. 이 인터페이스는 공유 코드 프로젝트에 배치되어야 합니다.

다음 예제에서는 디바이스의 방향을 검색하는 데 사용할 수 있는 API의 인터페이스를 보여 줍니다.

```csharp
public interface IDeviceOrientationService
{
    DeviceOrientation GetOrientation();
}
```

## <a name="implement-the-interface-on-each-platform"></a>각 플랫폼에서 인터페이스 구현

네이티브 플랫폼 기능을 조작하기 위한 API를 정의하는 인터페이스를 만든 후 각 플랫폼 프로젝트에서 인터페이스를 구현해야 합니다.

### <a name="ios"></a>iOS

다음 코드 예제에서는 iOS에서 `IDeviceOrientationService` 인터페이스의 구현을 보여 줍니다.

```csharp
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            UIInterfaceOrientation orientation = UIApplication.SharedApplication.StatusBarOrientation;

            bool isPortrait = orientation == UIInterfaceOrientation.Portrait ||
                orientation == UIInterfaceOrientation.PortraitUpsideDown;
            return isPortrait ? DeviceOrientation.Portrait : DeviceOrientation.Landscape;
        }
    }
}
```

### <a name="android"></a>Android

다음 코드 예제에서는 Android에서 `IDeviceOrientationService` 인터페이스의 구현을 보여 줍니다.

```csharp
namespace DependencyServiceDemos.Droid
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            SurfaceOrientation orientation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = orientation == SurfaceOrientation.Rotation90 ||
                orientation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientation.Landscape : DeviceOrientation.Portrait;
        }
    }
}
```

### <a name="universal-windows-platform"></a>UWP

다음 코드 예제는 UWP(유니버설 Windows 플랫폼)에서 `IDeviceOrientationService` 인터페이스의 구현을 보여 줍니다.

```csharp
namespace DependencyServiceDemos.UWP
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ApplicationViewOrientation orientation = ApplicationView.GetForCurrentView().Orientation;
            return orientation == ApplicationViewOrientation.Landscape ? DeviceOrientation.Landscape : DeviceOrientation.Portrait;
        }
    }
}
```

## <a name="register-the-platform-implementations"></a>플랫폼 구현 등록

각 플랫폼 프로젝트에서 인터페이스를 구현한 후 [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 플랫폼 구현을 등록해야 Xamarin.Forms가 런타임에 해당 구현을 찾을 수 있습니다. 이 작업은 일반적으로 지정된 형식이 인터페이스의 구현을 제공함을 나타내는 [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)를 사용하여 수행합니다.

다음 예제에서는 [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)를 사용하여 `IDeviceOrientationService` 인터페이스의 iOS 구현을 등록하는 방법을 보여 줍니다.

```csharp
using Xamarin.Forms;

[assembly: Dependency(typeof(DependencyServiceDemos.iOS.DeviceOrientationService))]
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ...
        }
    }
}
```

이 예제에서 [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)는 [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 `DeviceOrientationService`를 등록합니다. 마찬가지로 다른 플랫폼의 `IDeviceOrientationService` 인터페이스 구현은 [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)를 사용하여 등록해야 합니다.

[`DependencyService`](xref:Xamarin.Forms.DependencyService)에 플랫폼 구현을 등록하는 방법에 대한 자세한 내용은 [Xamarin.Forms DependencyService 등록 및 확인](registration-and-resolution.md)을 참조하세요.

## <a name="resolve-the-platform-implementations"></a>플랫폼 구현 확인

[`DependencyService`](xref:Xamarin.Forms.DependencyService)에 플랫폼 구현을 등록한 후 호출하기 전에 구현을 확인해야 합니다. 이 작업은 일반적으로 공유 코드에서 [`DependencyService.Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 메서드를 사용하여 수행합니다.

다음 코드는 [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 메서드를 호출하여 `IDeviceOrientationService` 인터페이스를 확인한 후 해당 `GetOrientation` 메서드를 호출하는 예제를 보여 줍니다.

```csharp
IDeviceOrientationService service = DependencyService.Get<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

또는 이 코드를 다음 한 줄로 요약할 수 있습니다.

```csharp
DeviceOrientation orientation = DependencyService.Get<IDeviceOrientationService>().GetOrientation();
```

[`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용하여 플랫폼 구현을 확인하는 방법에 대한 자세한 내용은 [Xamarin.Forms DependencyService 등록 및 확인](registration-and-resolution.md)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [DependencyService 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [Xamarin.Forms DependencyService 등록 및 확인](registration-and-resolution.md)
