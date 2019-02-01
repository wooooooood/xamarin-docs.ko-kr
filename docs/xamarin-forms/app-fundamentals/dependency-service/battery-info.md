---
title: 배터리 상태 확인
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용하여 각 플랫폼에 대한 배터리 정보에 고유하게 액세스하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 08278c2bc380892706320dbd0e69642257b73005
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233785"
---
# <a name="checking-battery-status"></a>배터리 상태 확인

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/DependencyService)

이 문서는 배터리 상태를 확인하는 애플리케이션의 만들기를 안내합니다. 이 문서는 James Montemagno의 배터리 플러그 인을 기반으로 합니다. 자세한 내용은 [GitHub 리포지토리](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery)를 참조하세요.

Xamarin.Forms에 현재 배터리 상태를 확인하는 기능이 포함되어 있지 않으므로 이 애플리케이션은 [`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용하여 네이티브 API를 활용해야 합니다.  이 문서에서는 `DependencyService`를 사용하기 위한 다음 단계를 설명합니다.

- **[인터페이스 만들기](#Creating_the_Interface)** &ndash; 공유 코드에서 인터페이스를 만드는 방법을 이해합니다.
- **[iOS 구현](#iOS_Implementation)** &ndash; iOS용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[Android 구현](#Android_Implementation)** &ndash; Android용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[유니버설 Windows 플랫폼 구현](#UWPImplementation)** &ndash; UWP(유니버설 Windows 플랫폼)용 네이티브 코드에서 인터페이스를 구현하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)** &ndash; 공유 코드에서 기본 구현을 호출하기 위해 `DependencyService`를 사용하는 방법을 알아봅니다.

완료되면 `DependencyService`를 사용한 애플리케이션에는 다음과 같은 구조가 있습니다.

![](battery-info-images/battery-diagram.png "DependencyService 애플리케이션 구조")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저, 원하는 기능이 표시되는 공유 코드에서 인터페이스를 만듭니다. 배터리 확인 애플리케이션의 경우 관련 정보는 남아 있는 배터리의 백분율, 디바이스가 충전 중인지 여부 및 디바이스에서 전원을 공급 받는 방법입니다.

```csharp
namespace DependencyServiceSample
{
  public enum BatteryStatus
  {
    Charging,
    Discharging,
    Full,
    NotCharging,
    Unknown
  }

  public enum PowerSource
  {
    Battery,
    Ac,
    Usb,
    Wireless,
    Other
  }

  public interface IBattery
  {
    int RemainingChargePercent { get; }
    BatteryStatus Status { get; }
    PowerSource PowerSource { get; }
  }
}
```

공유 코드에서 이 인터페이스에 대한 코딩을 하면 Xamarin.Forms 앱에서 각 플랫폼의 전원 관리 API에 액세스할 수 있습니다.

> [!NOTE]
> 인터페이스를 구현하는 클래스에서 `DependencyService`를 사용하려면 매개 변수가 없는 생성자가 있어야 합니다. 생성자는 인터페이스에 의해 정의될 수 없습니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

각 플랫폼별 애플리케이션 프로젝트에서 `IBattery` 인터페이스를 구현해야 합니다. iOS 구현은 네이티브 [`UIDevice`](xref:UIKit.UIDevice) API를 사용하여 배터리 정보에 액세스합니다. 다음 클래스에는 매개 변수가 없는 생성자가 있으므로 `DependencyService`가 새 인스턴스를 만들 수 있습니다.

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;

namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery
  {
    public BatteryImplementation()
    {
      UIDevice.CurrentDevice.BatteryMonitoringEnabled = true;
    }

    public int RemainingChargePercent
    {
      get
      {
        return (int)(UIDevice.CurrentDevice.BatteryLevel * 100F);
      }
    }

    public BatteryStatus Status
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return BatteryStatus.Charging;
          case UIDeviceBatteryState.Full:
            return BatteryStatus.Full;
          case UIDeviceBatteryState.Unplugged:
            return BatteryStatus.Discharging;
          default:
            return BatteryStatus.Unknown;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Full:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Unplugged:
            return PowerSource.Battery;
          default:
            return PowerSource.Other;
        }
      }
    }
  }
}
```

마지막으로 필요한 모든 `using` 명령문을 포함하여 클래스에 대해(및 정의된 모든 네임스페이스 외부에서) `[assembly]` 특성을 추가합니다.

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;//necessary for registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery {
    ...
```

이 특성은 해당 클래스를 `IBattery` 인터페이스의 구현으로 등록합니다. 즉, 공유 코드에서 `DependencyService.Get<IBattery>`을 사용하여 해당 인스턴스를 만들 수 있습니다.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android 구현

Android 구현은 [`Android.OS.BatteryManager`](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) API를 사용합니다. 이 구현은 iOS 버전보다 더 복잡하며, 배터리 사용 권한 부족을 처리하는 검사가 필요합니다.

```csharp
using System;
using Android;
using Android.Content;
using Android.App;
using Android.OS;
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid;

namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery
  {
    private BatteryBroadcastReceiver batteryReceiver;
    public BatteryImplementation() { }

    public int RemainingChargePercent
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              var level = battery.GetIntExtra(BatteryManager.ExtraLevel, -1);
              var scale = battery.GetIntExtra(BatteryManager.ExtraScale, -1);

              return (int)Math.Floor(level * 100D / scale);
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }

      }
    }

    public DependencyServiceSample.BatteryStatus Status
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;
              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);
              if (isCharging)
                return DependencyServiceSample.BatteryStatus.Charging;

              switch(status)
              {
                case (int)BatteryStatus.Charging:
                  return DependencyServiceSample.BatteryStatus.Charging;
                case (int)BatteryStatus.Discharging:
                  return DependencyServiceSample.BatteryStatus.Discharging;
                case (int)BatteryStatus.Full:
                  return DependencyServiceSample.BatteryStatus.Full;
                case (int)BatteryStatus.NotCharging:
                  return DependencyServiceSample.BatteryStatus.NotCharging;
                default:
                  return DependencyServiceSample.BatteryStatus.Unknown;
              }
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;

              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);

              if (!isCharging)
                return DependencyServiceSample.PowerSource.Battery;
              else if (usbCharge)
                return DependencyServiceSample.PowerSource.Usb;
              else if (acCharge)
                return DependencyServiceSample.PowerSource.Ac;
              else if (wirelessCharge)
                return DependencyServiceSample.PowerSource.Wireless;
              else
                return DependencyServiceSample.PowerSource.Other;
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }
  }
}
```

필요한 모든 `using` 명령문을 포함하여 클래스에 대해(및 정의된 모든 네임스페이스 외부에서) 이 `[assembly]` 특성을 추가합니다.

```csharp
...
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery {
    ...
```

이 특성은 해당 클래스를 `IBattery` 인터페이스의 구현으로 등록합니다. 즉, 공유 코드에서 `DependencyService.Get<IBattery>`을 사용하여 해당 인스턴스를 만들 수 있습니다.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>유니버설 Windows 플랫폼 구현

UWP 구현은 `Windows.Devices.Power` API를 사용하여 배터리 상태 정보를 얻습니다.

```csharp
using DependencyServiceSample.UWP;
using Xamarin.Forms;

[assembly: Dependency(typeof(BatteryImplementation))]
namespace DependencyServiceSample.UWP
{
    public class BatteryImplementation : IBattery
    {
        private BatteryStatus status = BatteryStatus.Unknown;
        Windows.Devices.Power.Battery battery;

        public BatteryImplementation()
        {
        }

        private Windows.Devices.Power.Battery DefaultBattery
        {
            get
            {
                return battery ?? (battery = Windows.Devices.Power.Battery.AggregateBattery);
            }
        }

        public int RemainingChargePercent
        {
            get
            {
                var finalReport = DefaultBattery.GetReport();
                var finalPercent = -1;

                if (finalReport.RemainingCapacityInMilliwattHours.HasValue && finalReport.FullChargeCapacityInMilliwattHours.HasValue)
                {
                    finalPercent = (int)((finalReport.RemainingCapacityInMilliwattHours.Value /
                        (double)finalReport.FullChargeCapacityInMilliwattHours.Value) * 100);
                }
                return finalPercent;
            }
        }

        public BatteryStatus Status
        {
            get
            {
                var report = DefaultBattery.GetReport();
                var percentage = RemainingChargePercent;

                if (percentage >= 1.0)
                {
                    status = BatteryStatus.Full;
                }
                else if (percentage < 0)
                {
                    status = BatteryStatus.Unknown;
                }
                else
                {
                    switch (report.Status)
                    {
                        case Windows.System.Power.BatteryStatus.Charging:
                            status = BatteryStatus.Charging;
                            break;
                        case Windows.System.Power.BatteryStatus.Discharging:
                            status = BatteryStatus.Discharging;
                            break;
                        case Windows.System.Power.BatteryStatus.Idle:
                            status = BatteryStatus.NotCharging;
                            break;
                        case Windows.System.Power.BatteryStatus.NotPresent:
                            status = BatteryStatus.Unknown;
                            break;
                    }
                }
                return status;
            }
        }

        public PowerSource PowerSource
        {
            get
            {
                if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
                {
                    return PowerSource.Ac;
                }
                return PowerSource.Battery;
            }
        }
    }
}
```

네임스페이스 선언에 대한 `[assembly]` 특성은 해당 클래스를 `IBattery` 인터페이스의 구현으로 등록합니다. 즉, 공유 코드에서 `DependencyService.Get<IBattery>`을 사용하여 해당 인스턴스를 만들 수 있습니다.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>공유 코드에서의 구현

각 플랫폼에 대해 인터페이스를 구현했으므로 이제 공유 애플리케이션은 해당 인터페이스를 활용하도록 작성될 수 있습니다. 애플리케이션은 탭하면 해당 텍스트를 현재 배터리 상태로 업데이트하는 단추가 있는 페이지로 구성됩니다. `DependencyService`를 사용하여 `IBattery` 인터페이스의 인스턴스를 가져옵니다. 런타임 시 이 인스턴스는 기본 SDK에 대한 모든 액세스 권한이 있는 플랫폼별 구현이 됩니다.

```csharp
public MainPage ()
{
    var button = new Button {
        Text = "Click for battery info",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    button.Clicked += (sender, e) => {
        var bat = DependencyService.Get<IBattery>();

        switch (bat.PowerSource){
          case PowerSource.Battery:
            button.Text = "Battery - ";
            break;
          case PowerSource.Ac:
            button.Text = "AC - ";
            break;
          case PowerSource.Usb:
            button.Text = "USB - ";
            break;
          case PowerSource.Wireless:
            button.Text = "Wireless - ";
            break;
          case PowerSource.Other:
          default:
            button.Text = "Other - ";
            break;
        }
        switch (bat.Status){
          case BatteryStatus.Charging:
            button.Text += "Charging";
            break;
          case BatteryStatus.Discharging:
            button.Text += "Discharging";
            break;
          case BatteryStatus.NotCharging:
            button.Text += "Not Charging";
            break;
          case BatteryStatus.Full:
            button.Text += "Full";
            break;
          case BatteryStatus.Unknown:
          default:
            button.Text += "Unknown";
            break;
        }
    };
    Content = button;
}
```

iOS, Android 또는 UWP에서 이 애플리케이션을 실행하고 단추를 누르면 단추 텍스트가 디바이스의 현재 전원 상태를 반영하도록 업데이트됩니다.

![](battery-info-images/battery.png "배터리 상태 샘플")


## <a name="related-links"></a>관련 링크

- [DependencyService(샘플)](https://developer.xamarin.com/samples/DependencyService)
- [DependencyService 사용(샘플)](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
