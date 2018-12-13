---
title: 디바이스 방향 확인
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용하여 공유 코드에서 디바이스 방향에 액세스하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 52b82033cbd6fe0e1a44f5729c815074852230bf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115421"
---
# <a name="checking-device-orientation"></a>디바이스 방향 확인

이 문서에서는 각 플랫폼에서 기본 API를 사용하여 공유 코드에서 디바이스 방향을 확인하기 위해 [`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용하는 방법을 안내합니다. 이 연습은 Ali Özgür별 기존 `DeviceOrientation` 플러그 인을 따릅니다. 자세한 내용은 [GitHub 리포지토리](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation)를 참조하세요.

- **[인터페이스 만들기](#Creating_the_Interface)** &ndash; 공유 코드에서 인터페이스를 만드는 방법을 이해합니다.
- **[iOS 구현](#iOS_Implementation)**  &ndash; iOS용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[Android 구현](#Android_Implementation)**  &ndash; Android용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[UWP 구현](#WindowsImplementation)** &ndash; UWP(유니버설 Windows 플랫폼)용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)** &ndash; 공유 코드에서 기본 구현을 호출하기 위해 `DependencyService`를 사용하는 방법을 알아봅니다.

`DependencyService`를 사용한 애플리케이션에는 다음과 같은 구조가 있습니다.

![](device-orientation-images/orientation-diagram.png "DependencyService 애플리케이션 구조")

> [!NOTE]
> [디바이스 방향](~/xamarin-forms/user-interface/layouts/device-orientation.md#Reacting_to_Changes_in_Orientation)에 표시된 대로 디바이스가 공유 코드에서 세로 방향 또는 가로 방향인지 여부를 검색할 수 있습니다. 이 문서에서 설명한 메서드는 기본 기능을 사용하여 디바이스가 뒤집혔는지 여부를 포함한 방향에 대한 자세한 정보를 얻습니다.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저 구현하려는 기능이 표시되는 공유 코드에서 인터페이스를 만듭니다. 예를 들어 인터페이스에는 단일 메서드가 포함됩니다.

```csharp
namespace DependencyServiceSample.Abstractions
{
    public enum DeviceOrientations
    {
        Undefined,
        Landscape,
        Portrait
    }

    public interface IDeviceOrientation
    {
        DeviceOrientations GetOrientation();
    }
}
```

공유 코드에서 이 인터페이스에 대한 코딩을 하면 Xamarin.Forms 앱에서 각 플랫폼의 디바이스 방향 API에 액세스할 수 있습니다.

> [!NOTE]
> 인터페이스를 구현하는 클래스에는 `DependencyService`를 사용하려면 매개 변수가 없는 생성자가 있어야 합니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

각 플랫폼별 애플리케이션 프로젝트에서 인터페이스를 구현해야 합니다. 클래스에는 매개 변수가 없는 생성자가 있으므로 `DependencyService`가 새 인스턴스를 만들 수 있습니다.

```csharp
using UIKit;
using Foundation;

namespace DependencyServiceSample.iOS
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation(){ }

        public DeviceOrientations GetOrientation()
        {
            var currentOrientation = UIApplication.SharedApplication.StatusBarOrientation;
            bool isPortrait = currentOrientation == UIInterfaceOrientation.Portrait
                || currentOrientation == UIInterfaceOrientation.PortraitUpsideDown;

            return isPortrait ? DeviceOrientations.Portrait: DeviceOrientations.Landscape;
        }
    }
}
```

마지막으로 필요한 모든 `using` 문을 포함하여 클래스에 대해(및 정의된 모든 네임스페이스 외부에서) `[assembly]` 특성을 추가합니다.

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

이 특성은 해당 클래스를 `IDeviceOrientation` 인터페이스의 구현으로 등록합니다. 즉, 공유 코드에서 `DependencyService.Get<IDeviceOrientation>`을 사용하여 해당 인스턴스를 만들 수 있습니다.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android 구현

다음 코드는 Android에서 `IDeviceOrientation`을 구현합니다.

```csharp
using DependencyServiceSample.Droid;
using Android.Hardware;

namespace DependencyServiceSample.Droid
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public static void Init() { }

        public DeviceOrientations GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            var rotation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = rotation == SurfaceOrientation.Rotation90 || rotation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientations.Landscape : DeviceOrientations.Portrait;
        }
    }
}
```

필요한 모든 `using` 문을 포함하여 클래스에 대해(및 정의된 모든 네이스페이스 외부에서) 이 `[assembly]` 특성을 추가합니다.

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

이 특성은 해당 클래스를 `IDeviceOrientaiton` 인터페이스의 구현으로 등록합니다. 즉, 공유 코드에서 `DependencyService.Get<IDeviceOrientation>`을 사용하여 해당 인스턴스를 만들 수 있습니다.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>유니버설 Windows 플랫폼 구현

다음 코드는 유니버설 Windows 플랫폼에서 `IDeviceOrientation` 인터페이스를 구현합니다.

```csharp
namespace DependencyServiceSample.WindowsPhone
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public DeviceOrientations GetOrientation()
        {
            var orientation = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            if (orientation == Windows.UI.ViewManagement.ApplicationViewOrientation.Landscape) {
                return DeviceOrientations.Landscape;
            }
            else {
                return DeviceOrientations.Portrait;
            }
        }
    }
}
```

필요한 모든 `using` 문을 포함하여 클래스에 대해(및 정의된 모든 네임스페이스 외부에서) `[assembly]` 특성을 추가합니다.

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

이 특성은 해당 클래스를 `DeviceOrientationImplementation` 인터페이스의 구현으로 등록합니다. 즉, 공유 코드에서 `DependencyService.Get<IDeviceOrientation>`을 사용하여 해당 인스턴스를 만들 수 있습니다.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>공유 코드에서의 구현

이제 `IDeviceOrientation` 인터페이스에 액세스하는 공유 코드를 작성하고 테스트할 수 있습니다. 이 간단한 페이지에는 디바이스 방향에 따라 고유의 텍스트를 업데이트하는 단추가 포함됩니다. 이 페이지는 `DependencyService`를 사용하여 `IDeviceOrientation` 인터페이스의 인스턴스를 가져옵니다 &ndash; 런타임 시 이 인스턴스는 기본 SDK에 대한 모든 액세스 권한이 있는 플랫폼별 구현이 됩니다.

```csharp
public MainPage ()
{
    var orient = new Button {
        Text = "Get Orientation",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    orient.Clicked += (sender, e) => {
       var orientation = DependencyService.Get<IDeviceOrientation>().GetOrientation();
       switch(orientation){
           case DeviceOrientations.Undefined:
                orient.Text = "Undefined";
                break;
           case DeviceOrientations.Landscape:
                orient.Text = "Landscape";
                break;
           case DeviceOrientations.Portrait:
                orient.Text = "Portrait";
                break;
       }
    };
    Content = orient;
}
```

iOS, Android 또는 Windows 플랫폼에서 이 애플리케이션을 실행하고 단추를 누르면 단추의 텍스트가 디바이스 방향으로 업데이트됩니다.

![](device-orientation-images/orientation.png "디바이스 방향 샘플")


## <a name="related-links"></a>관련 링크

- [DependencyService(샘플) 사용](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService(샘플)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
