---
title: 장치 방향을 확인 하는 중
description: 공유 코드에서 액세스 장치 방향을를 DependencyService를 사용 하 여
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: b8392dad578f94380e90da24cbf44120d38f754d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="checking-device-orientation"></a>장치 방향을 확인 하는 중

사용 하는이 문서에서는 안내 [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) 에서 공유 코드는 각 플랫폼에서 네이티브 Api를 사용 하 여 장치 방향을 확인 합니다. 이 연습에서는 기존 기반 `DeviceOrientation` 알리 Özgür 별로 플러그 인 합니다. 참조는 [GitHub 리포지토리](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) 자세한 정보에 대 한 합니다.

- **[인터페이스를 만드는 방법](#Creating_the_Interface)**  &ndash; 이해 인터페이스 하는 방법을 공유 코드에 만들어집니다.
- **[iOS 구현](#iOS_Implementation)**  &ndash; iOS에 대 한 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[Android 구현](#Android_Implementation)**  &ndash; Android 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[Windows 구현](#WindowsImplementation)**  &ndash; 네이티브 코드에 Windows Phone 및 유니버설 Windows 플랫폼 (UWP)에 대 한 인터페이스를 구현 하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)**  &ndash; 사용 방법을 배울 `DependencyService` 공유 코드에서 네이티브 구현으로 호출할 수 있습니다.

사용 하 여 응용 프로그램 `DependencyService` 다음과 같은 구조를 갖습니다.

![](device-orientation-images/orientation-diagram.png "DependencyService 응용 프로그램 구조")

> [!NOTE]
> 와 같이 공유 코드에는 장치가 세로 또는 가로 방향 인지 여부를 검색할 수 있기에 [장치 Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). 이 문서에서 설명 하는 방법을 장치 거꾸로 인지를 포함 하 여 방향에 대 한 자세한 정보를 보려면 기본 기능을 사용 합니다.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저 구현 하려는 기능에 따라 표시 되는 공유 코드에서 인터페이스를 만듭니다. 이 예제에서는 인터페이스 메서드 하나가 포함 됩니다.

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

공유 코드에서이 인터페이스에 대 한 코딩 하면 Xamarin.Forms 앱 각 플랫폼에서 장치 방향을 Api에 액세스할 수 있습니다.

> [!NOTE]
> 인터페이스를 구현 하는 클래스를 작성 하려면 매개 변수가 없는 생성자가 있어야는 `DependencyService`합니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

각 플랫폼 관련 응용 프로그램 프로젝트에서 인터페이스를 구현 해야 합니다. 매개 변수가 없는 생성자는 클래스에 있도록는 `DependencyService` 새 인스턴스를 만들 수:

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

마지막으로,이 추가 `[assembly]` 클래스 (위와 정의 된 모든 네임 스페이스 외부), 모든 필수 비롯 하 여 특성 `using` 문:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

이 특성의 구현으로 클래스를 등록는 `IDeviceOrientation` 인터페이스 즉 `DependencyService.Get<IDeviceOrientation>` 해당 형식의 인스턴스를 만들려고 공유 코드에서 사용할 수 있습니다.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android 구현

다음 코드 구현 `IDeviceOrientation` Android에서:

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

이 추가 `[assembly]` 클래스 (위와 정의 된 모든 네임 스페이스 외부), 모든 필수 비롯 하 여 특성 `using` 문:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

이 특성의 구현으로 클래스를 등록는 `IDeviceOrientaiton` 않음을 의미 하는 인터페이스 `DependencyService.Get<IDeviceOrientation>` 에서 사용할 수는 공유 코드는 해당 형식의 인스턴스를 만들 수 있습니다.

<a name="WindowsImplementation" />

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone 및 유니버설 Windows 플랫폼 구현

다음 코드 구현 하는 `IDeviceOrientation` Windows Phone 및 유니버설 Windows 플랫폼 인터페이스:

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

추가 `[assembly]` 클래스 (위와 정의 된 모든 네임 스페이스 외부), 모든 필수 비롯 하 여 특성 `using` 문:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

이 특성의 구현으로 클래스를 등록는 `DeviceOrientationImplementation` 않음을 의미 하는 인터페이스 `DependencyService.Get<IDeviceOrientation>` 에서 사용할 수는 공유 코드는 해당 형식의 인스턴스를 만들 수 있습니다.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>공유 코드에서 구현

작성 하 고 공유 코드에 액세스 하는 테스트에서는 이제는 `IDeviceOrientation` 인터페이스입니다. 이 간단한 페이지 장치 방향을 기준으로 자체 텍스트를 업데이트 하는 단추를 포함 합니다. 사용 하 여는 `DependencyService` 의 인스턴스를 가져오려면는 `IDeviceOrientation` 인터페이스 &ndash; 런타임 시이 인스턴스가 네이티브 SDK에 대 한 모든 권한이 있는 플랫폼별으로 구현 됩니다.

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

IOS, Android 또는 Windows 플랫폼에서이 응용 프로그램을 실행 하 고 단추를 누르면 발생 합니다는 장치 방향을 사용 하는 업데이트 하는 단추의 텍스트입니다.

![](device-orientation-images/orientation.png "장치 방향 샘플")


## <a name="related-links"></a>관련 링크

- [DependencyService (샘플)를 사용 하 여](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (샘플)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
