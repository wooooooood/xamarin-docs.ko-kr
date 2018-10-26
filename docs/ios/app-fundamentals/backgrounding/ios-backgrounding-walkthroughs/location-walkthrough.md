---
title: 연습-Xamarin.iOS에서 백그라운드 위치
description: 이 문서는 backgrounded Xamarin.iOS 응용 프로그램에서 위치 정보를 사용 하는 방법의 연습을 제공 합니다. 필요한 설정, 사용자 인터페이스 및 응용 프로그램 상태를 설명합니다.
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 0d6dac02ac82b74c9b0e33f5fff0b82223df1f47
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108680"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>연습-Xamarin.iOS에서 백그라운드 위치

이 예제에서는 하겠습니다 iOS 현재 위치에 대 한 정보를 출력 하는 위치 응용 프로그램 빌드: 위도, 경도 및 화면에 다른 매개 변수입니다. 이 응용 프로그램에서는 응용 프로그램은 활성 또는 Backgrounded 위치 업데이트를 제대로 수행 하는 방법을 보여 줍니다.

이 연습에서는 백그라운드 필요한 응용 프로그램으로 앱을 등록 하 고 앱 backgrounded은 경우에 UI 업데이트를 일시 중단 작업을 포함 하 여 개념 backgrounding 일부 키에 설명 합니다 `WillEnterBackground` 하 고 `WillEnterForeground` `AppDelegate` 메서드 .

## <a name="application-set-up"></a>응용 프로그램 설정


1. 먼저 새를 만듭니다 **iOS > 앱 > 단일 뷰 응용 프로그램 (C#)** 합니다. 호출할 _위치_ iPad 및 iPhone 둘 다 선택 되어 있는지 확인 합니다.

1. 위치 응용 프로그램을 iOS에서 백그라운드 필요한 응용 프로그램으로 한정합니다. 편집 하 여 위치 응용 프로그램으로 응용 프로그램을 등록 합니다 **Info.plist** 프로젝트 파일입니다.

    솔루션 탐색기에서 두 번 클릭 합니다 **Info.plist** 파일 열을 목록 아래쪽으로 스크롤합니다. 확인란을 모두 합니다 **Enable Background Modes** 하며 **위치 업데이트** 확인란 합니다.

    Mac 용 Visual Studio에서 코드는 다음과 같이 표시 됩니다 것.

    [![](location-walkthrough-images/image7.png "Enable Background Modes와 위치 업데이트 확인란에서 확인란을 선택")](location-walkthrough-images/image7.png#lightbox)

    Visual Studio에서 **Info.plist** 다음 키/값 쌍을 추가 하 여 수동으로 업데이트 해야 합니다.

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. 응용 프로그램에 등록 했으므로 장치에서 위치 데이터를 가져올 수 것입니다. IOS의 경우에 `CLLocationManager` 클래스 위치 정보에 액세스 하는 데 사용 되 고 위치 업데이트를 제공 하는 이벤트를 발생 시킬 수 있습니다.

1. 코드에서 라는 새 클래스를 만듭니다 `LocationManager` 다양 한 화면과 위치 업데이트를 구독 하는 코드에 대 한 단일 장소를 제공 합니다. 에 `LocationManager` 클래스의 인스턴스를 확인 합니다는 `CLLocationManager` 호출 `LocMgr`:

    ```csharp
    public class LocationManager
    {
        protected CLLocationManager locMgr;

        public LocationManager () {
            this.locMgr = new CLLocationManager();
            this.locMgr.PausesLocationUpdatesAutomatically = false;

            // iOS 8 has additional permissions requirements
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                locMgr.RequestAlwaysAuthorization (); // works in background
                //locMgr.RequestWhenInUseAuthorization (); // only in foreground
            }

            if (UIDevice.CurrentDevice.CheckSystemVersion (9, 0)) {
                locMgr.AllowsBackgroundLocationUpdates = true;
            }
        }

        public CLLocationManager LocMgr {
            get { return this.locMgr; }
        }
    }
    ```

    위의 코드에서 다양 한 속성 및 사용 권한을 설정 합니다 [CLLocationManager](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/) 클래스:

    - `PausesLocationUpdatesAutomatically` -시스템 위치 업데이트를 일시 중지 허용 되는지 여부에 따라 설정할 수 있는 부울입니다. 기본값은 일부 장치에서 `true`, 장치 백그라운드 약 15 분 후 위치 업데이트 수신을 중지를 유발할 수 있습니다.
    - `RequestAlwaysAuthorization` -앱 사용자에 게 백그라운드에서 액세스할 수 위치를 허용 하는 옵션을 제공 하려면이 메서드를 전달 해야 합니다. `RequestWhenInUseAuthorization` 사용자에 게 앱이 포그라운드에서 하는 경우에 액세스할 수 위치를 허용 하는 옵션을 제공 하려는 경우에 전달 수 있습니다.
    - `AllowsBackgroundLocationUpdates` – 앱이 일시 중지 된 경우 위치 업데이트를 받을 수 있도록 설정할 수 있는 iOS 9에에서 도입 된는 부울 속성입니다.

    > [!IMPORTANT]
    > iOS 8 (이상)의 항목을도 필요 합니다 **Info.plist** 권한 부여 요청의 일부로 사용자를 보여 주는 파일입니다.

1. 키를 추가 `NSLocationAlwaysUsageDescription` 또는 `NSLocationWhenInUseUsageDescription` 위치 데이터 액세스를 요청 하는 경고의 사용자에 게 표시 되는 문자열을 사용 합니다.

1. iOS 9를 실행 하려면 사용 하는 경우 `AllowsBackgroundLocationUpdates` 는 **Info.plist** 키를 포함 `UIBackgroundModes` 값을 사용 하 여 `location`입니다. 이 연습의 2 단계,이 이미 완료 한 경우 Info.plist 파일에 있습니다.


1. 내 합니다 `LocationManager` 클래스를 호출 하는 메서드를 만듭니다 `StartLocationUpdates` 다음 코드를 사용 하 여 합니다. 이 코드는 방법을 보여 줍니다에서 위치 업데이트 수신을 시작 하는 `CLLocationManager`:

    ```csharp
    if (CLLocationManager.LocationServicesEnabled) {
        //set the desired accuracy, in meters
        LocMgr.DesiredAccuracy = 1;
        LocMgr.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) =>
        {
            // fire our custom Location Updated event
            LocationUpdated (this, new LocationUpdatedEventArgs (e.Locations [e.Locations.Length - 1]));
        };
        LocMgr.StartUpdatingLocation();
    }
    ```

    이 메서드에서 발생 하는 몇 가지 중요 한 사항이 있습니다. 첫째, 응용 프로그램에 장치에서 위치 데이터에 대 한 액세스 검사를 수행 합니다. 호출 하 여이 확인 했습니다 `LocationServicesEnabled` 에 `CLLocationManager`합니다. 이 메서드는 반환 **false** 사용자가 위치 정보에 대 한 응용 프로그램 액세스를 거부 한 경우.

1. 다음으로 업데이트 하려면 다음과 같이 위치 관리자를 얼마나 자주 알려 줍니다. `CLLocationManager` 필터링 하 고 업데이트의 빈도 포함 하는 위치 데이터를 구성 하기 위한 다양 한 옵션을 제공 합니다. 이 예제에서는 설정 된 `DesiredAccuracy` 측정기 위치 변경 될 때마다 업데이트 합니다. 위치 업데이트 빈도 및 기타 기본 설정을 구성에 대 한 자세한 내용은 참조는 [CLLocationManager 클래스 참조](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) Apple 설명서에 있습니다.

1. 마지막으로 호출 `StartUpdatingLocation` 에 `CLLocationManager` 인스턴스. 이렇게 하면 현재 위치에 따라 초기 픽스를 얻으려면 하 고 업데이트를 보내기 시작 하는 위치 관리자

지금 위치 관리자를 만든 수신 하고자 하는 데이터의 종류를 사용 하 여 구성 했으며 초기 위치를 결정 했습니다. 이제 코드는 사용자 인터페이스에 위치 데이터를 렌더링 해야 합니다. 사용 하는 사용자 지정 이벤트를 사용 하 여 이렇게 할는 `CLLocation` 인수로:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

다음 단계에서 위치 업데이트를 구독 하는 것은 `CLLocationManager`, 시키고 사용자 지정 `LocationUpdated` 이벤트 위치 데이터를 새 위치를 인수로 전달를 사용할 수 있게 하는 경우. 이렇게 하려면 새 클래스를 만듭니다 **LocationUpdateEventArgs.cs**합니다. 이 코드는 주 응용 프로그램 내에서 액세스할 수 있습니다 및 이벤트가 발생 하는 경우에 장치 위치를 반환 합니다.

```csharp
public class LocationUpdatedEventArgs : EventArgs
{
    CLLocation location;

    public LocationUpdatedEventArgs(CLLocation location)
    {
       this.location = location;
    }

    public CLLocation Location
    {
       get { return location; }
    }
}
```

## <a name="user-interface"></a>사용자 인터페이스

1. IOS 디자이너를 사용 하 여 위치 정보를 표시 하는 화면을 만들려고 합니다. 두 번 클릭 합니다 **Main.storyboard** 를 시작 합니다.

    스토리 보드 여러 레이블이 화면에서 위치 정보에 대 한 자리 표시자 역할을 끌어다 놓습니다. 이 예제에서는 위도, 경도, 고도, 강좌 및 속도 대 한 레이블이 있습니다.

    레이아웃은 다음과 유사 합니다.

    ![](location-walkthrough-images/image8.png "IOS 디자이너의에서 UI 레이아웃 예")

1. 솔루션 패드에서 두 번 클릭 합니다 `ViewController.cs` 파일을 편집 하 여 LocationManager 호출의 새 인스턴스를 만듭니다 `StartLocationUpdates`에 있습니다.
  다음과 같이 코드를 변경 합니다.

    ```csharp
    #region Computed Properties
    public static bool UserInterfaceIdiomIsPhone {
                get { return UIDevice.CurrentDevice.UserInterfaceIdiom == UIUserInterfaceIdiom.Phone; }
            }

    public static LocationManager Manager { get; set;}
    #endregion

    #region Constructors
    public ViewController (IntPtr handle) : base (handle)
    {
    // As soon as the app is done launching, begin generating location updates in the location manager
        Manager = new LocationManager();
        Manager.StartLocationUpdates();
    }

    #endregion
    ```

    표시할 데이터가 없습니다 있지만 위치 업데이트를 응용 프로그램 시작 시 시작 됩니다.

1. 위치 업데이트를 수신 했으므로 위치 정보를 사용 하 여 화면을 업데이트 합니다. 다음 메서드는 위치를 가져옵니다는 `LocationUpdated` 이벤트 및 UI에 표시 합니다.

    ```csharp
    #region Public Methods
    public void HandleLocationChanged (object sender, LocationUpdatedEventArgs e)
    {
        // Handle foreground updates
        CLLocation location = e.Location;

        LblAltitude.Text = location.Altitude + " meters";
        LblLongitude.Text = location.Coordinate.Longitude.ToString ();
        LblLatitude.Text = location.Coordinate.Latitude.ToString ();
        LblCourse.Text = location.Course.ToString ();
        LblSpeed.Text = location.Speed.ToString ();

        Console.WriteLine ("foreground updated");
    }
    #endregion
    ```

구독할 필요는 `LocationUpdated` AppDelegate, 및 메서드 호출 하 여 새 UI를 업데이트 하는 이벤트입니다. 다음 코드를 추가 `ViewDidLoad,` 바로 뒤의 `StartLocationUpdates` 호출:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


이제 응용 프로그램을 실행 하는 경우이 같이 표시 됩니다 것:

[![](location-walkthrough-images/image5.png "앱의 예 실행")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>활성 및 백그라운드 상태 처리

1. 응용 프로그램은 포그라운드에 현재 위치 업데이트 출력 됩니다. 앱 백그라운드를 입력 하는 경우를 보여 주기 위해 재정의 `AppDelegate` 응용 프로그램을 추적 하는 메서드는 응용 프로그램이 포그라운드 및 백그라운드 간 전환 시 콘솔에 기록 되도록 변경 상태:

    ```csharp
    public override void DidEnterBackground (UIApplication application)
    {
        Console.WriteLine ("App entering background state.");
    }

    public override void WillEnterForeground (UIApplication application)
    {
        Console.WriteLine ("App will enter foreground");
    }
    ```

    다음 코드를 추가 합니다 `LocationManager` 지속적으로 업데이트 된 위치를 인쇄 위치 정보를 확인 하려면 응용 프로그램 출력에 데이터를 백그라운드에서 계속 사용할 수 있습니다.

    ```csharp
    public class LocationManager
    {
        public LocationManager ()
        {
        ...
        LocationUpdated += PrintLocation;
        }
        ...

        //This will keep going in the background and the foreground
        public void PrintLocation (object sender, LocationUpdatedEventArgs e) {
        CLLocation location = e.Location;
        Console.WriteLine ("Altitude: " + location.Altitude + " meters");
        Console.WriteLine ("Longitude: " + location.Coordinate.Longitude);
        Console.WriteLine ("Latitude: " + location.Coordinate.Latitude);
        Console.WriteLine ("Course: " + location.Course);
        Console.WriteLine ("Speed: " + location.Speed);
        }
    }
    ```

1. 코드를 사용 하 여 한 가지 나머지 문제가: 앱은 backgrounded 때 UI를 업데이트 하는 원인 iOS는 종료 합니다. 앱이 백그라운드에 되 면 코드 위치 업데이트에서 구독을 취소 하 고 UI 업데이트를 중지 해야 합니다.

    iOS 제공 알림을 때 앱에 대 한 다른 응용 프로그램으로 전환 상태입니다. 이 예에서는 구독할 수는 `ObserveDidEnterBackground` 알림.

    다음 코드 조각은 알림을 사용 하 여 뷰의 UI 업데이트를 중지 하는 경우를 파악 하는 방법을 보여 줍니다. 로 이동 `ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    앱을 실행할 때 다음과 같은 출력이 표시 됩니다.

    ![](location-walkthrough-images/image6.png "콘솔에서 위치 출력의 예")

1. 응용 프로그램을 전경에 작동 하는 경우 화면에 위치 업데이트를 인쇄 하 고 백그라운드에서 작동 하는 동안 응용 프로그램 출력 창에 데이터를 인쇄 하도록 계속 됩니다.

하나의 미해결 문제가 여전히: 화면 앱을 처음 로드 되었지만 앱 전경이 다시 입력 하는 경우 알 수 없습니다 하는 경우 UI 업데이트를 시작 합니다. Backgrounded 응용 프로그램은 전경으로 돌아가지, UI 업데이트 다시 시작 하지 않습니다.

이 해결 하려면 중첩 호출 응용 프로그램이 활성 상태가 될 때 발생 하는 다른 알림 내에서 UI 업데이트를 시작 하려면:

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

이제 UI를 사용 하는 응용 프로그램을 처음 시작 하 고 언제 든 지 앱을 업데이트 하는 이력서를 전경으로 반환 됩니다. 업데이트 시작 됩니다.

이 연습에서는 화면 및 응용 프로그램 출력 창에 위치 데이터를 출력 하는 올바르게 동작, 백그라운드 인식 iOS 응용 프로그램을 빌드 했습니다.


## <a name="related-links"></a>관련 링크

- [위치 (4 부) (샘플)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Core 위치 프레임 워크 참조](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
