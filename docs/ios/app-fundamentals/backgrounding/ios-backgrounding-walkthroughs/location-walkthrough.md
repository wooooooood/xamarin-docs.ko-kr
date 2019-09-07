---
title: 연습-Xamarin.ios의 백그라운드 위치
description: 이 문서에서는 backgrounded 응용 프로그램에서 위치 정보를 사용 하는 방법에 대 한 연습을 제공 합니다. 필요한 설정, 사용자 인터페이스 및 응용 프로그램 상태를 설명 합니다.
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 9f4708b56b8cf8a243785816440c63b743059cf5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756273"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>연습-Xamarin.ios의 백그라운드 위치

이 예제에서는 현재 위치에 대 한 정보 (예: 위도, 경도 및 기타 매개 변수)를 화면에 출력 하는 iOS 위치 응용 프로그램을 빌드 하겠습니다. 이 응용 프로그램은 응용 프로그램이 활성 또는 Backgrounded 중에 위치 업데이트를 적절 하 게 수행 하는 방법을 보여 줍니다.

이 연습에서는 앱을 백그라운드에서 필요한 응용 프로그램으로 등록 하 고, 앱이 backgrounded 때 UI 업데이트를 일시 중단 하 고, `WillEnterBackground` 및 `WillEnterForeground` `AppDelegate` 메서드를 사용 하는 등의 몇 가지 주요 backgrounding 개념을 설명 합니다. .

## <a name="application-set-up"></a>응용 프로그램 설정

1. 먼저 **단일 뷰 응용 프로그램 > 새 iOS > 앱을 만듭니다 (C#)** . It _위치_ 를 호출 하 고 IPad와 iPhone을 모두 선택 했는지 확인 합니다.

1. 위치 응용 프로그램은 iOS에서 백그라운드에서 필요한 응용 프로그램으로 한정 됩니다. 프로젝트에 대 한 **info.plist** 파일을 편집 하 여 응용 프로그램을 위치 응용 프로그램으로 등록 합니다.

    솔루션 탐색기에서 **info.plist** 파일을 두 번 클릭 하 여 열고 목록의 아래쪽으로 스크롤합니다. **백그라운드 모드 사용** 및 **업데이트 위치** 확인란을 모두 선택 하 여 확인 합니다.

    Mac용 Visual Studio에서 다음과 같이 표시 됩니다.

    [![](location-walkthrough-images/image7.png "백그라운드 모드 및 업데이트 위치 확인란을 모두 사용 하 여 검사를 수행 합니다.")](location-walkthrough-images/image7.png#lightbox)

    Visual Studio에서 info.plist는 다음 키/값 쌍을 추가 하 여 수동으로 업데이트 해야 **합니다** .

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. 이제 응용 프로그램이 등록 되었으므로 장치에서 위치 데이터를 가져올 수 있습니다. IOS `CLLocationManager` 에서 클래스는 위치 정보에 액세스 하는 데 사용 되며 위치 업데이트를 제공 하는 이벤트를 발생 시킬 수 있습니다.

1. 코드에서 다양 한 화면 및 코드에 위치 `LocationManager` 업데이트를 구독할 수 있는 단일 위치를 제공 하는 라는 새 클래스를 만듭니다. 클래스에서 `CLLocationManager` 호출`LocMgr`된의 인스턴스를 만듭니다. `LocationManager`

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

    위의 코드는 [Cllocationmanager](xref:CoreLocation.CLLocationManager) 클래스에서 다양 한 속성 및 사용 권한을 설정 합니다.

    - `PausesLocationUpdatesAutomatically`– 시스템이 위치 업데이트를 일시 중지할 수 있는지 여부에 따라 설정할 수 있는 부울입니다. 일부 장치에서는 기본적으로로 `true`설정 되어 있으며,이로 인해 장치에서 15 분 후에 백그라운드 위치 업데이트 가져오기를 중지할 수 있습니다.
    - `RequestAlwaysAuthorization`-백그라운드에서 위치에 액세스할 수 있도록 허용 하는 옵션을 앱 사용자에 게 제공 하려면이 메서드를 전달 해야 합니다. `RequestWhenInUseAuthorization`앱이 전경에 있는 경우에만 위치에 액세스할 수 있는 옵션을 사용자에 게 제공 하려는 경우에도를 전달할 수 있습니다.
    - `AllowsBackgroundLocationUpdates`– 일시 중단 될 때 앱이 위치 업데이트를 받을 수 있도록 설정할 수 있는 iOS 9에 도입 된 부울 속성입니다.

    > [!IMPORTANT]
    > iOS 8 (및 이상)에는 사용자를 권한 부여 요청의 일부로 표시 하기 위해 **info.plist** 파일에 항목이 있어야 합니다.

1. 위치 데이터 액세스 `NSLocationAlwaysUsageDescription` 를 `NSLocationWhenInUseUsageDescription` 요청 하는 경고에서 사용자에 게 표시 되는 문자열이 나 키를 추가 합니다.

1. iOS 9에서는 info.plist를 사용 `AllowsBackgroundLocationUpdates` 하는 경우 값 `location`이 포함 된 `UIBackgroundModes` 키를 포함 해야 **합니다.** 이 연습의 2 단계를 완료 한 경우 info.plist 파일에 이미 포함 되어 있어야 합니다.

1. 클래스 내에서 다음 코드를 사용 하 `StartLocationUpdates` 여 라는 메서드를 만듭니다. `LocationManager` 이 코드는에서 `CLLocationManager`위치 업데이트 수신을 시작 하는 방법을 보여 줍니다.

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

    이 방법에서는 몇 가지 중요 한 사항이 있습니다. 첫째, 응용 프로그램이 장치의 위치 데이터에 액세스할 수 있는지 확인 하는 검사를 수행 합니다. 에서를 호출 `LocationServicesEnabled` 하 여이를 확인 합니다.`CLLocationManager` 사용자가 위치 정보에 대 한 응용 프로그램 액세스를 거부 한 경우이 메서드는 **false** 를 반환 합니다.

1. 그런 다음 위치 관리자에 업데이트 빈도를 알려 줍니다. `CLLocationManager`에서는 업데이트 빈도를 비롯 하 여 위치 데이터를 필터링 하 고 구성 하는 다양 한 옵션을 제공 합니다. 이 예제에서는 위치가 미터에 `DesiredAccuracy` 의해 변경 될 때마다를 업데이트 하도록 설정 합니다. 위치 업데이트 빈도 및 기타 기본 설정 구성에 대 한 자세한 내용은 Apple 설명서의 [Cllocationmanager 클래스 참조](https://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) 를 참조 하세요.

1. 마지막으로 `CLLocationManager` 인스턴스에서 `StartUpdatingLocation` 를 호출 합니다. 그러면 위치 관리자가 현재 위치에 대 한 초기 수정을 받고 업데이트 보내기를 시작 하도록 지시 합니다.

지금까지 수신 하려는 데이터 종류를 사용 하 여 위치 관리자를 만들고 초기 위치를 결정 했습니다. 이제 코드에서 위치 데이터를 사용자 인터페이스로 렌더링 해야 합니다. 를 `CLLocation` 인수로 사용 하는 사용자 지정 이벤트를 사용 하 여이 작업을 수행할 수 있습니다.

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

다음 단계는에서 위치 업데이트 `CLLocationManager`를 구독 하 고, 새 위치 데이터를 사용할 수 있게 되 면 사용자 지정 `LocationUpdated` 이벤트를 발생 시켜 위치를 인수로 전달 합니다. 이렇게 하려면 **LocationUpdateEventArgs.cs**새 클래스를 만듭니다. 이 코드는 주 응용 프로그램 내에서 액세스할 수 있으며 이벤트가 발생할 때 장치 위치를 반환 합니다.

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

1. IOS 디자이너를 사용 하 여 위치 정보를 표시 하는 화면을 빌드합니다. **주 storyboard** 파일을 두 번 클릭 하 여 시작 합니다.

    스토리 보드에서 여러 레이블을 화면으로 끌어 위치 정보에 대 한 자리 표시자 역할을 합니다. 이 예제에는 위도, 경도, 고도, 과정 및 속도에 대 한 레이블이 있습니다.

    레이아웃은 다음과 유사 합니다.

    ![](location-walkthrough-images/image8.png "IOS 디자이너의 예제 UI 레이아웃")

1. Solution Pad에서 `ViewController.cs` 파일을 두 번 클릭 하 고 편집 하 여 locationmanager의 새 인스턴스를 만들고이를 호출 `StartLocationUpdates`합니다.
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

    그러면 데이터가 표시 되지 않지만 응용 프로그램 시작 시 위치 업데이트가 시작 됩니다.

1. 이제 위치 업데이트가 수신 되었으므로 위치 정보를 사용 하 여 화면을 업데이트 합니다. 다음 메서드는 `LocationUpdated` 이벤트에서 위치를 가져오고 UI에 표시 합니다.

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

여전히 AppDelegate에서 `LocationUpdated` 이벤트를 구독 하 고 새 메서드를 호출 하 여 UI를 업데이트 해야 합니다. 호출`StartLocationUpdates` 바로 뒤에 `ViewDidLoad,` 다음 코드를 추가 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```

이제 응용 프로그램이 실행 될 때 다음과 같이 표시 됩니다.

[![](location-walkthrough-images/image5.png "예제 앱 실행")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>활성 및 백그라운드 상태 처리

1. 응용 프로그램이 포그라운드에서 활성 상태인 동안 위치 업데이트를 출력 합니다. 앱이 백그라운드에 들어갈 때 발생 하는 상황을 보여 주기 `AppDelegate` 위해 응용 프로그램 상태 변경을 추적 하는 메서드를 재정의 하 여 응용 프로그램이 포그라운드에서 백그라운드 사이를 전환할 때 콘솔에 쓰도록 합니다.

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

    에서 다음 코드 `LocationManager` 를 추가 하 여 업데이트 된 위치 데이터를 응용 프로그램 출력으로 지속적으로 인쇄 하 여 백그라운드에서 위치 정보를 계속 사용할 수 있는지 확인 합니다.

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

1. 코드에는 한 가지 남은 문제가 있습니다. 앱이 backgrounded 때 UI를 업데이트 하려고 하면 iOS에서 종료 됩니다. 앱이 백그라운드에 들어가면 코드에서 위치 업데이트의 구독을 취소 하 고 UI 업데이트를 중지 해야 합니다.

    iOS는 앱이 다른 응용 프로그램 상태로 전환 될 때 알림을 제공 합니다. 이 경우 `ObserveDidEnterBackground` 알림을 구독할 수 있습니다.

    다음 코드 조각에서는 알림을 사용 하 여 UI 업데이트를 중단할 시기를 확인 하는 방법을 보여 줍니다. `ViewDidLoad`다음으로 이동 합니다.

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    앱을 실행 하는 경우 출력은 다음과 같이 표시 됩니다.

    ![](location-walkthrough-images/image6.png "콘솔의 위치 출력 예제")

1. 응용 프로그램은 전경에서 작동할 때 화면에 위치 업데이트를 인쇄 하 고 백그라운드에서 작동 하는 동안 계속 해 서 응용 프로그램 출력 창으로 데이터를 인쇄 합니다.

해결 되지 않은 문제는 하나만 남아 있습니다. 즉, 앱이 처음 로드 될 때 화면에서 UI 업데이트를 시작 하지만 앱이 포그라운드로 다시 입력 된 시기를 알 수 있는 방법은 없습니다. Backgrounded 응용 프로그램을 포그라운드로 다시 가져오면 UI 업데이트가 다시 시작 되지 않습니다.

이 문제를 해결 하려면 응용 프로그램이 활성 상태로 전환 될 때 발생 하는 다른 알림 내에서 UI 업데이트를 시작 하는 호출을 중첩 합니다.

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

이제 응용 프로그램이 처음 시작 될 때 UI가 업데이트를 시작 하 고 앱이 포그라운드로 다시 들어올 때마다 업데이트를 다시 시작 합니다.

이 연습에서는 화면 및 응용 프로그램 출력 창에 위치 데이터를 출력 하는 잘 작동 하는 백그라운드 인식 iOS 응용 프로그램을 빌드 했습니다.

## <a name="related-links"></a>관련 링크

- [위치 (4 부) (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/location)
- [핵심 위치 프레임 워크 참조](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
