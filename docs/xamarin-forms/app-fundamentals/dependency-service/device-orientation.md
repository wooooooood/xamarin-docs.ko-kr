---
title: 장치 방향 확인
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용 하 여 공유 코드에서 장치 방향에 액세스 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 620404a217b2e8a31192ae6613dcec023ac366cd
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995643"
---
# <a name="checking-device-orientation"></a>장치 방향 확인

이 문서를 사용 하도록 안내 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) 네이티브 Api를 사용 하 여 각 플랫폼에서 공유 코드에서 장치 방향 확인 합니다. 이 연습은 기존 기반 `DeviceOrientation` Ali Özgür 별로 플러그 인입니다. 참조를 [GitHub 리포지토리에서](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) 자세한 내용은 합니다.

- **[인터페이스를 만드는](#Creating_the_Interface)**  &ndash; 이해 인터페이스에는 공유 코드에서 생성 됩니다.
- **[iOS 구현](#iOS_Implementation)**  &ndash; iOS 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[Android 구현을](#Android_Implementation)**  &ndash; Android 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[UWP 구현을](#WindowsImplementation)**  &ndash; 유니버설 Windows 플랫폼 (UWP)에 대 한 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)**  &ndash; 사용 하는 방법을 알아봅니다 `DependencyService` 공유 코드에서 기본 구현을 호출 합니다.

사용 하 여 응용 프로그램 `DependencyService` 같은 구조를 가집니다.

![](device-orientation-images/orientation-diagram.png "DependencyService 응용 프로그램 구조")

> [!NOTE]
> 설명 된 대로 장치 공유 코드에 세로 또는 가로 방향 인지 여부를 검색할 수 있기에 [장치 Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). 이 문서에서 설명 하는 방법을 장치 거꾸로 인지를 포함 하 여 방향에 대 한 자세한 정보를 보려면 기본 기능을 사용 합니다.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저 구현 하려는 기능을 표현 하는 공유 코드에서 인터페이스를 만듭니다. 예를 들어 인터페이스는 단일 메서드를 포함 합니다.

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

공유 코드에서이 인터페이스에 대 한 코딩 하면 Xamarin.Forms 앱에서 각 플랫폼 장치 방향 Api에 액세스할 수 있습니다.

> [!NOTE]
> 인터페이스를 구현 하는 클래스를 사용 하려면 매개 변수가 없는 생성자가 있어야 합니다 `DependencyService`합니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

각 플랫폼 관련 응용 프로그램 프로젝트에는 인터페이스를 구현 해야 합니다. 클래스는 매개 변수가 없는 생성자에 있도록는 `DependencyService` 새 인스턴스를 만들 수 있습니다.

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

마지막으로,이 추가 `[assembly]` 특성 클래스 위에 (및 정의 된 모든 네임 스페이스 외부)에 필요한 모든 포함 `using` 문:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

이 특성의 구현으로 클래스를 등록 합니다 `IDeviceOrientation` 즉 인터페이스 `DependencyService.Get<IDeviceOrientation>` 의 인스턴스를 만드는 공유 코드에 사용할 수 있습니다.

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

이 추가 `[assembly]` 특성 클래스 위에 (및 정의 된 모든 네임 스페이스 외부)에 필요한 모든 포함 `using` 문:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

이 특성의 구현으로 클래스를 등록 합니다 `IDeviceOrientaiton` 즉 인터페이스 `DependencyService.Get<IDeviceOrientation>` 에서 사용할 수 있습니다 공유 코드를 해당 인스턴스를 만들 수 있습니다.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>유니버설 Windows 플랫폼 구현

다음 구현 코드를 `IDeviceOrientation` 유니버설 Windows 플랫폼에서 인터페이스:

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

추가 된 `[assembly]` 특성 클래스 위에 (및 정의 된 모든 네임 스페이스 외부)에 필요한 모든 포함 `using` 문:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

이 특성의 구현으로 클래스를 등록 합니다 `DeviceOrientationImplementation` 즉 인터페이스 `DependencyService.Get<IDeviceOrientation>` 에서 사용할 수 있습니다 공유 코드를 해당 인스턴스를 만들 수 있습니다.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>공유 코드에서 구현

작성 하 고 공유 코드에 액세스 하는 테스트에서는 이제는 `IDeviceOrientation` 인터페이스입니다. 이 간단한 페이지에는 장치 방향에 따라 고유한 텍스트를 업데이트 하는 단추가 포함 됩니다. 사용 하 여는 `DependencyService` 의 인스턴스를 가져옵니다는 `IDeviceOrientation` 인터페이스 &ndash; 런타임 시이 인스턴스 네이티브 SDK에 대 한 전체 액세스 권한이 있는 플랫폼 전용 구현 됩니다.

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

IOS, Android 또는 Windows 플랫폼에서이 응용 프로그램을 실행 하 고 단추를 누르면는 장치 방향으로 업데이트 하는 단추의 텍스트입니다.

![](device-orientation-images/orientation.png "장치 방향 샘플")


## <a name="related-links"></a>관련 링크

- [DependencyService (샘플)를 사용 하 여](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (샘플)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
