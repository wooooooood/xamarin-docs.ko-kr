---
title: 배터리 상태를 확인 하는 중
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용 하 여 각 플랫폼에 대해 고유 하 게 배터리 정보에 액세스 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: cbb4a01ac2c6d933fe40a0b3c2571d1fe3ce75c0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998400"
---
# <a name="checking-battery-status"></a>배터리 상태를 확인 하는 중

이 문서는 배터리 상태를 확인 하는 응용 프로그램 만들기를 안내 합니다. 이 문서는 James montemagno 배터리 플러그 인을 기반으로 합니다. 자세한 내용은 참조는 [GitHub 리포지토리에서](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery)합니다.

Xamarin.Forms에 현재 배터리 상태를 확인 하기 위한 기능이 포함 되어 있지 않으므로,이 응용 프로그램 사용 해야 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) 네이티브 Api를 활용할 수 있습니다.  이 문서에서는 사용 하는 다음 단계를 설명 하는 `DependencyService`:

- **[인터페이스를 만드는](#Creating_the_Interface)**  &ndash; 공유 코드에서 인터페이스를 만드는 방법을 이해 합니다.
- **[iOS 구현](#iOS_Implementation)**  &ndash; iOS 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[Android 구현을](#Android_Implementation)**  &ndash; Android 용 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[유니버설 Windows 플랫폼 구현](#UWPImplementation)**  &ndash; 유니버설 Windows 플랫폼 (UWP)에 대 한 네이티브 코드에서 인터페이스를 구현 하는 방법을 알아봅니다.
- **[공유 코드에서 구현](#Implementing_in_Shared_Code)**  &ndash; 사용 하는 방법을 알아봅니다 `DependencyService` 공유 코드에서 기본 구현을 호출 합니다.

완료 되 면 응용 프로그램 `DependencyService` 같은 구조를 가집니다.

![](battery-info-images/battery-diagram.png "DependencyService 응용 프로그램 구조")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>인터페이스 만들기

먼저, 원하는 기능을 표현 하는 공유 코드에서 인터페이스를 만듭니다. 응용 프로그램을 확인 하는 배터리의 경우 관련 정보는 장치, 청구 및 어떻게 장치 전원이 인지 남은 배터리 백분율:

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

공유 코드에서이 인터페이스에 대 한 코딩 하면 Xamarin.Forms 앱 각 플랫폼에서 전원 관리 Api에 액세스할 수 있습니다.

> [!NOTE]
> 인터페이스를 구현 하는 클래스를 사용 하려면 매개 변수가 없는 생성자가 있어야 합니다 `DependencyService`합니다. 인터페이스에서 생성자를 정의할 수 없습니다.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS 구현

`IBattery` 각 플랫폼별으로 응용 프로그램 프로젝트에서 인터페이스를 구현 해야 합니다. IOS 구현이 네이티브 사용할지 [ `UIDevice` ](https://developer.xamarin.com/api/type/UIKit.UIDevice/) 배터리 정보에 액세스할 수 있는 Api입니다. 다음 클래스는 매개 변수가 없는 생성자에 있도록는 `DependencyService` 새 인스턴스를 만들 수 있습니다.

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

마지막으로,이 추가 `[assembly]` 특성 클래스 위에 (및 정의 된 모든 네임 스페이스 외부)에 필요한 모든 포함 `using` 문:

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

이 특성의 구현으로 클래스를 등록 합니다 `IBattery` 즉 인터페이스 `DependencyService.Get<IBattery>` 의 인스턴스를 만드는 공유 코드에서 사용할 수 있습니다:

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android 구현

Android 구현을 사용 하 여 [ `Android.OS.BatteryManager` ](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) API. 이 구현은 배터리 사용 권한 부족을 처리 하는 검사를 요구에 iOS 버전 보다 더 복잡 한 것입니다.

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

이 추가 `[assembly]` 특성 클래스 위에 (및 정의 된 모든 네임 스페이스 외부)에 필요한 모든 포함 `using` 문:

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

이 특성의 구현으로 클래스를 등록 합니다 `IBattery` 즉 인터페이스 `DependencyService.Get<IBattery>` 에서 사용할 수 있습니다 공유 코드를 해당 인스턴스를 만들 수 있습니다.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>유니버설 Windows 플랫폼 구현

UWP 구현을 사용 하는 `Windows.Devices.Power` Api 배터리 상태 정보를 얻으려면:

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

`[assembly]` 네임 스페이스 선언 위에 특성의 구현으로 클래스를 등록 합니다 `IBattery` 인터페이스, 즉 `DependencyService.Get<IBattery>` 의 인스턴스를 만드는 공유 코드에서 사용할 수 있습니다.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>공유 코드에서 구현

각 플랫폼에 대 한 인터페이스를 구현한 다음에 이제 해당 활용 하기 위해 공유 응용 프로그램을 작성할 수 있습니다. 응용 프로그램으로 구성 됩니다 업데이트를 탭 할 때 단추를 사용 하 여 페이지의 현재 배터리 상태를 사용 하 여 해당 텍스트입니다. 사용 된 `DependencyService` 의 인스턴스를 가져옵니다는 `IBattery` 인터페이스. 런타임 시이 인스턴스는 네이티브 SDK에 대 한 전체 액세스 권한이 있는 플랫폼 전용 구현 됩니다.

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

IOS에서이 응용 프로그램을 실행 중인, Android 또는 UWP 및 단추를 누르면 장치의 현재 전원 상태를 반영 하도록 업데이트 단추 텍스트에 발생 합니다.

![](battery-info-images/battery.png "배터리 상태 샘플")


## <a name="related-links"></a>관련 링크

- [DependencyService (샘플)](https://developer.xamarin.com/samples/DependencyService)
- [DependencyService (샘플)를 사용 하 여](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
