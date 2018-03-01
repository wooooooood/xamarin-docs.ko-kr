---
title: "연습-배경 위치를 사용 하 여"
ms.topic: article
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: ba460bee067162f8e42f84f230f93cb1cf98ba98
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-background-location"></a>연습-배경 위치를 사용 하 여

이 예제에서는 하겠습니다 iOS 우리의 현재 위치에 대 한 정보를 인쇄 하는 위치 응용 프로그램을 빌드: 위도, 경도 및 화면에 다른 매개 변수입니다. 이 응용 프로그램에서는 응용 프로그램이 있는 동안 액티브 또는 Backgrounded 위치 업데이트를 제대로 수행 하는 방법을 보여 줍니다.

이 연습에서는 일부 주요 개념에 배경 필요한 응용 프로그램으로 앱을 등록, 앱 backgrounded은 때 UI 업데이트를 일시 중단 된 작업 등 backgrounding 설명는 `WillEnterBackground` 및 `WillEnterForeground` `AppDelegate` 메서드 .

## <a name="application-set-up"></a>응용 프로그램 설정


1. 먼저 새 만듭니다 **iOS > 앱 > 단일 보기 응용 프로그램 (C#)**합니다. 호출 _위치_ iPad 및 iPhone을 모두 선택 되어 있는지 확인 합니다.

1. 위치 응용 프로그램에서 iOS 배경 필요한 응용 프로그램으로 한정합니다. 응용 프로그램을 편집 하 여 위치 응용 프로그램으로 등록는 **Info.plist** 프로젝트 파일입니다.

    솔루션 탐색기에서 두 번 클릭은 **Info.plist** 파일을 열고 목록 아래쪽으로 스크롤합니다. 확인란에 둘 다로 **백그라운드 모드를 사용 하도록 설정** 및 **위치 업데이트** 확인란을 선택 합니다.


    Mac 용 Visual Studio에서 코드는 다음과 같이 보입니다.

    [![](location-walkthrough-images/image7.png "사용 하도록 설정 하는 백그라운드 모드와 위치 업데이트 확인란에서 확인란을 선택합니다")](location-walkthrough-images/image7.png)

    Visual Studio에서 **Info.plist** 다음 키/값 쌍을 추가 하 여 수동으로 업데이트 해야 합니다.

        ```csharp
        <key>UIBackgroundModes</key>
        <array>
            <string>location</string>
        </array>
        ```

1. 응용 프로그램 등록은 장치에서 위치 데이터를 가져올 수 있습니다. IOS의 경우에 `CLLocationManager` 클래스 위치 정보에 액세스 하는 데 사용 되 고 위치 업데이트를 제공 하는 이벤트를 발생 시킬 수 있습니다.

1. 코드에서 라는 새 클래스를 만들고 `LocationManager` 다양 한 화면 및 위치 업데이트를 구독 하는 코드에 대 한 단일 장소를 제공 합니다. 에 `LocationManager` 클래스의 인스턴스를 확인 합니다는 `CLLocationManager` 호출 `LocMgr`:

```csharp
        public class LocationManager
        {
          protected CLLocationManager locMgr;

          public LocationManager (){
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

          public CLLocationManager LocMgr{
            get { return this.locMgr; }
          }
        }
```

    The code above sets a number of properties and permissions on the [CLLocationManager](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/) class:

    - `PausesLocationUpdatesAutomatically` -위치 업데이트를 일시 중지 하는 시스템 수 있는지 여부에 따라 설정할 수 있는 부울 값입니다. 기본적으로 일부 장치에서 `true`, 업데이트를 중단 배경 위치 약 15 분 후에 장치가 될 수 있습니다.
    - `RequestAlwaysAuthorization` -응용 프로그램 사용자에 게 위치에 백그라운드에서 액세스를 허용 하는 옵션을 제공 하려면이 메서드를 전달 해야 합니다. `RequestWhenInUseAuthorization` 위치에 액세스 하 여 앱이 포그라운드에서 하는 경우에 허용 하는 옵션을 사용자에 게 제공 하려는 경우에 전달 수 있습니다.
    - `AllowsBackgroundLocationUpdates` – 앱이 일시 중단 되었을 때 위치 업데이트를 받을 수 있도록 설정할 수 있는 iOS 9에에서 도입 하는 부울 속성입니다.

    > [!IMPORTANT]
> **경고**: iOS 8 (및 큰)에 있는 항목 필요는 **Info.plist** 파일을 사용자에 게 권한 부여 요청의 일부로 표시 합니다.

1. 키를 추가 `NSLocationAlwaysUsageDescription` 또는 `NSLocationWhenInUseUsageDescription` 요청 위치 데이터에 액세스 하는 경고의 사용자에 게 표시 되는 문자열을 사용 합니다.

1. iOS 9를 실행 하려면 사용 하는 경우 `AllowsBackgroundLocationUpdates` 는 **Info.plist** 에 키를 포함 `UIBackgroundModes` 값과 `location`합니다. 이 연습의 2 단계를 완료 한 경우,이 이미 해야 Info.plist 파일에 있습니다.


1. 내에서 `LocationManager` 클래스, 이라는 메서드를 만들어 `StartLocationUpdates` 를 다음 코드로 합니다. 이 코드에서는에서 위치 업데이트 수신을 시작 하는 `CLLocationManager`:

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

    There are several important things happening in this method. First, we perform a check to see if the application has access to location data on the device. We verify this by calling `LocationServicesEnabled` on the `CLLocationManager`. This method will return **false** if the user has denied the application access to location information.

1. Next, tell the location manager how often to update. `CLLocationManager` provides many options for filtering and configuring location data, including the frequency of updates. In this example, set the `DesiredAccuracy` to update whenever the location changes by a meter. For more information on configuring location update frequency and other preferences, refer to the [CLLocationManager Class Reference](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) in the Apple documentation.

1. Finally, call `StartUpdatingLocation` on the `CLLocationManager` instance. This tells the location manager to get an initial fix on the current location, and to start sending updates

So far, the location manager has been created, configured with the kinds of data we want to receive, and has determined the initial location. Now the code needs to render the location data to the user interface. We can do this with a custom event that takes a `CLLocation` as an argument:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

다음 단계에서 위치 업데이트를 구독 하는 것은 `CLLocationManager`, 시키고 사용자 지정 `LocationUpdated` 위치 데이터를 새 위치를 인수로 전달를 사용할 수 있게 하는 경우 이벤트입니다. 이렇게 하려면 새 클래스를 만듭니다 **LocationUpdateEventArgs.cs**합니다. 이 코드는 주 응용 프로그램 내에서 액세스할 수 하 고 이벤트 발생 하는 경우 장치 위치를 반환 합니다.

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

1. IOS 디자이너를 사용 하는 위치 정보를 표시 하는 화면을 작성 합니다. 두 번 클릭 하 고 **Main.storyboard** 을 시작 합니다.

    스토리 보드 여러 레이블이 화면에서 위치 정보에 대 한 자리 표시자 역할을 끌어다 놓습니다. 이 예제에서는 위도, 경도, 고도, 강좌, 및 속도 대 한 레이블이 있습니다.

    레이아웃은 다음과 유사 합니다.

    ![](location-walkthrough-images/image8.png "IOS 디자이너의에서 레이아웃 예 UI")

1. 솔루션 패드에서 두 번 클릭은 `ViewController.cs` 파일을 편집 하는 LocationManager와 호출의 새 인스턴스를 만들고 `StartLocationUpdates`에 있습니다.
  다음과 같이 표시 하는 코드를 변경 합니다.

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

    데이터가 없는 표시 되지만 동적 디스크인 위치 업데이트 응용 프로그램 시작 시에 시작 됩니다.

1. 위치 업데이트를 수신 했으므로 화면 위치 정보로 업데이트 합니다. 다음 메서드에서 위치를 가져옵니다 우리의 `LocationUpdated` 이벤트 UI에 표시 합니다.

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


구독 해야는 `LocationUpdated` AppDelegate, UI를 업데이트 하 고 새 메서드 호출에는 이벤트입니다. 에 다음 코드를 추가 `ViewDidLoad,` 바로 뒤의 `StartLocationUpdates` 호출:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


이제 응용 프로그램이 실행 될 때 같아야 합니다 다음과 같습니다.

[![](location-walkthrough-images/image5.png "실행 하는 예제 응용 프로그램")](location-walkthrough-images/image5.png)

## <a name="handling-active-and-background-states"></a>활성 및 배경 상태 처리

1. 응용 프로그램은 포그라운드 및 활성 상태일 위치 업데이트를 출력 합니다. 응용 프로그램이 백그라운드 들어가며 때 수행 되는 작업을 보여 주기 위해 재정의 `AppDelegate` 전경과 배경 간에 전환할 때 응용 프로그램을 콘솔에 작성 되도록 응용 프로그램을 추적 하는 메서드 상태 변경:

        public override void DidEnterBackground (UIApplication application)
        {
          Console.WriteLine ("App entering background state.");
        }

        public override void WillEnterForeground (UIApplication application)
        {
          Console.WriteLine ("App will enter foreground");
        }

    에 다음 코드를 추가 `LocationManager` 지속적으로 업데이트 된 위치를 인쇄 하려면 데이터를 응용 프로그램 출력 위치 정보를 확인 하는 백그라운드에서 계속 사용할 수 있습니다.

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

1. 코드에 나머지 하나의 문제가: 응용 프로그램은 backgrounded 때 UI를 업데이트 하는 동안 됩니다 원인 iOS 됩니다 종료 합니다. 응용 프로그램을 백그라운드 화할 때는 코드 위치 업데이트에서 등록을 취소 하 고 UI 업데이트를 중지 해야 합니다.

    iOS 우리 될 때 알림을 제공 하는 방법에 대 한 다른 응용 프로그램으로 전환 상태입니다. 이 경우에 가입할 수 우리는 `ObserveDidEnterBackground` 알림입니다.

    다음 코드 조각 알림을 사용 하 여 UI 업데이트를 중단 하는 경우 확인 보기를 하는 방법을 보여 줍니다. 이동이 `ViewDidLoad`:

        UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
          Manager.LocationUpdated -= HandleLocationChanged;
        });

    앱을 실행 하는 경우 출력 다음과 같이 표시 됩니다.

    ![](location-walkthrough-images/image6.png "콘솔에서 위치 출력의 예")

1. 응용 프로그램을 전경에 작동할 때 화면에 대 한 위치 업데이트를 인쇄 하 고 백그라운드에서 작동 하는 동안 응용 프로그램 출력 창에 데이터를 인쇄 하도록 계속 됩니다.

하나의 해결 되지 않은 문제가 여전히 발생: 처음으로 앱 로드 하지만 응용 프로그램은 전경을 다시 입력에 알 수 있는 방법이 화면 UI 업데이트를 시작 합니다. Backgrounded 응용 프로그램은 전경으로 돌아가지, UI 업데이트 다시 시작 되지 않습니다.

이 해결 하려면 응용 프로그램이 활성 상태가 될 때 발생 하는 다른 알림 내부 UI 업데이트를 시작 하는 호출의 중첩 시킵니다.

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

이제 UI를 사용 하는 응용 프로그램을 처음 시작 하 고 언제 든 지 앱을 업데이트 하는 이력서를 전경으로 반환 됩니다. 업데이트 시작 됩니다.

이 연습에서는 화면와 응용 프로그램 출력 창에 위치 데이터를 출력 하는 올바르게 동작, 배경 인식 iOS 응용 프로그램을 빌드 했습니다.


## <a name="related-links"></a>관련 링크

- [위치 (파트 4) (샘플)](https://developer.xamarin.com/samples/monotouch/Location/)
- [핵심 위치 프레임 워크 참조](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
