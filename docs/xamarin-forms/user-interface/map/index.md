---
title: Xamarin.ios 맵 초기화 및 구성
description: 이 문서에서는 Xamarin.ios Map 클래스를 사용 하 여 각 플랫폼에서 네이티브 맵 Api를 사용 하 여 사용자에 게 친숙 한 맵 환경을 제공 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/07/2019
ms.openlocfilehash: d9b1b93b0667415281bb04e2c4264f03be19bd83
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697712"
---
# <a name="xamarinforms-map-initialization-and-configuration"></a>Xamarin.ios 맵 초기화 및 구성

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

_Xamarin.ios는 각 플랫폼에서 네이티브 맵 Api를 사용 합니다._

Xamarin.ios 맵은 각 플랫폼에서 네이티브 맵 Api를 사용 합니다. 이를 통해 사용자는 빠르고 친숙 한 지도를 제공할 수 있지만 각 플랫폼 API 요구 사항을 준수 하려면 몇 가지 구성 단계가 필요 합니다.
구성 된 `Map` 컨트롤은 공통 코드의 다른 Xamarin.ios 요소와 똑같이 작동 합니다.

지도 컨트롤은 아래에 표시 된 [Mapssample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps) 샘플에서 사용 되었습니다.

 [![MobileCRM 샘플의 Maps](map-images/maps-zoom-sml.png "맵 컨트롤 예제")](map-images/maps-zoom.png#lightbox "맵 컨트롤 예제")

지도 [사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)를 만들어 지도 기능을 더욱 향상 시킬 수 있습니다.

<a name="Maps_Initialization" />

## <a name="map-initialization"></a>맵 초기화

Xamarin Forms 응용 프로그램에 맵을 추가 하는 경우 **에는 솔루션** 의 모든 프로젝트에 추가 해야 하는 별도의 NuGet 패키지입니다.
Android에서는 Xamarin.googleplayservices.base를 추가할 때 자동으로 다운로드 되는 다른 NuGet ()에 대 한 종속성도 있습니다.

NuGet 패키지를 설치한 후에는 `Xamarin.Forms.Forms.Init` 메서드 호출 *후* 에 각 응용 프로그램 프로젝트에 일부 초기화 코드가 필요 합니다. IOS의 경우 다음 코드를 사용 합니다.

```csharp
Xamarin.FormsMaps.Init();
```

Android에서 `Forms.Init`와 동일한 매개 변수를 전달 해야 합니다.

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

UWP (유니버설 Windows 플랫폼)의 경우 다음 코드를 사용 합니다.

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

각 플랫폼에 대해 다음 파일에이 호출을 추가 합니다.

- `FinishedLaunching` 메서드의 **iOS** -AppDelegate.cs 파일입니다.
- @No__t_1 메서드의 **Android** -MainActivity.cs 파일입니다.
- @No__t_1 생성자의 **UWP** MainPage.xaml.cs 파일입니다.

NuGet 패키지를 추가 하 고 각 응용 프로그램 내에서 초기화 메서드를 호출 하면 공용 .NET Standard 라이브러리 프로젝트 또는 공유 프로젝트 코드에서 `Xamarin.Forms.Maps` Api를 사용할 수 있습니다.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>플랫폼 구성

지도를 표시 하려면 일부 플랫폼에서 추가 구성 단계가 필요 합니다.

### <a name="ios"></a>iOS

IOS의 위치 서비스에 액세스 하려면 info.plist에서 다음 키를 설정 해야 합니다 **.**

- iOS 11
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – 응용 프로그램이 사용 중일 때 위치 서비스를 사용 하는 경우
  - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – 항상 위치 서비스를 사용 하는 경우
- iOS 10 및 이전 버전
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – 응용 프로그램이 사용 중일 때 위치 서비스를 사용 하는 경우
  - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – 항상 위치 서비스를 사용 하는 경우    

IOS 11 및 이전 버전을 지원 하기 위해 `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription` 및 `NSLocationAlwaysUsageDescription`의 세 가지 키를 모두 포함할 수 있습니다.

**Info.plist** 에서 이러한 키에 대 한 XML 표현은 다음과 같습니다. 응용 프로그램에서 위치 정보를 사용 하는 방법을 반영 하 여 `string` 값을 업데이트 해야 합니다.

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist** 파일을 편집 하는 동안 **Info.plist** 항목을 **소스** 뷰에 추가할 수도 있습니다.

![IOS 8 용 info.plist](map-images/ios8-map-permissions.png "iOS 8 필수 정보. info.plist 항목")

### <a name="android"></a>Android

Android에서 [Google MAPS api](https://developers.google.com/maps/documentation/android/) v 2를 사용 하려면 api 키를 생성 하 여 android 프로젝트에 추가 해야 합니다.
[Google MAPS API v2 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)에 대 한 Xamarin doc의 지침을 따릅니다.
이러한 지침을 수행한 후에는 속성/a s **d. x m l** 파일 (보기 원본 및 다음 요소 찾기/업데이트)에 API 키를 붙여넣습니다.

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

유효한 API 키를 사용 하지 않으면 지도 컨트롤이 Android에서 회색 상자로 표시 됩니다.

> [!NOTE]
> APK가 Google Maps에 액세스 하도록 하려면 APK에 서명 하는 데 사용 하는 모든 키 저장소 (디버그 및 릴리스)에 대해 SHA-1 지문 및 패키지 이름을 포함 해야 합니다. 예를 들어 디버그에 하나의 컴퓨터를 사용 하 고 다른 컴퓨터에서 릴리스 APK를 생성 하는 경우 첫 번째 컴퓨터의 디버그 키 저장소에서 SHA-1 인증서 지문을 포함 하 고 키 저장소의 릴리스를 사용 하 여 SHA-1 인증서 지문을 포함 해야 합니다. 두 번째 컴퓨터입니다. 또한 앱의 **패키지 이름이** 변경 되는 경우 키 자격 증명을 편집 해야 합니다. [Google MAPS API v2 키 가져오기를](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)참조 하세요.

Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 > 옵션을 선택 하 **> Android 응용 프로그램 빌드** 및 다음을 선택 하 여 적절 한 사용 권한을 설정 해야 합니다.

- `AccessCoarseLocation`
- `AccessFineLocation`
- `AccessLocationExtraCommands`
- `AccessMockLocation`
- `AccessNetworkState`
- `AccessWifiState`
- `Internet`

이 중 일부는 아래 스크린샷에 나와 있습니다.

![Android에 필요한 권한](map-images/android-map-permissions.png "Android에 필요한 권한")

응용 프로그램에서 맵 데이터를 다운로드 하려면 네트워크 연결이 필요 하기 때문에 마지막 두 개가 필요 합니다. 자세한 내용은 Android [사용 권한](https://developer.android.com/reference/android/Manifest.permission.html) 을 참조 하세요.

또한 Android 9는 bootclasspath에서 Apache HTTP 클라이언트 라이브러리를 제거 했으므로 API 28 이상을 대상으로 하는 응용 프로그램에서는 사용할 수 없습니다. API 28 이상을 대상으로 하는 응용 프로그램에서 Apache HTTP 클라이언트를 계속 사용 하려면 다음 줄을 **Androidmanifest .xml** 파일의 `application` 노드에 추가 해야 합니다.

```xml
<application ...>
    ...
    <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

### <a name="universal-windows-platform"></a>UWP

유니버설 Windows 플랫폼에서 맵을 사용 하려면 인증 토큰을 생성 해야 합니다. 자세한 내용은 MSDN에서 [지도 인증 키 요청](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) 을 참조 하세요.

그런 다음 `FormsMaps.Init("AUTHORIZATION_TOKEN")` 메서드 호출에서 인증 토큰을 지정 하 여 Bing Maps를 사용 하 여 앱을 인증 해야 합니다.

<a name="Using_Maps" />

## <a name="map-configuration"></a>맵 구성

코드에서 지도 컨트롤을 사용 하는 방법에 대 한 예제는 MobileCRM 샘플에서 [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) 를 참조 하세요. 간단한 `MapPage` 클래스가 다음과 같이 표시 될 수 있습니다. 지도 보기를 배치 하기 위해 새 `MapSpan` 생성 된 것을 확인할 수 있습니다.

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>지도 유형

@No__t_0 속성을 설정 하 여 일반적인 거리 (기본값), 위성 이미지 또는 둘의 조합을 표시 하도록 지도 콘텐츠를 변경할 수도 있습니다.

```csharp
map.MapType = MapType.Street;
```

유효한 `MapType` 값은 다음과 같습니다.

- `Hybrid`
- `Satellite`
- `Street` (기본값)

### <a name="map-region-and-mapspan"></a>지도 영역 및 MapSpan

위의 코드 조각에 나와 있는 것 처럼 map 생성자에 `MapSpan` 인스턴스를 제공 하면 맵의 초기 보기 (중심점 및 확대/축소 수준)가 로드 될 때이를 설정 합니다. 새 `MapSpan` 인스턴스를 만드는 방법에는 두 가지가 있습니다.

- **Mapspan.** 로 `Position`에서 범위를 만들고 `Distance`를 지정 하는 정적 메서드입니다.
- **새 MapSpan ()** -`Position`를 사용 하는 생성자와 위도 및 경도의 각도를 표시 합니다.

그러면 map 클래스의 `MoveToRegion` 메서드를 사용 하 여 지도의 위치 또는 확대/축소 수준을 변경할 수 있습니다. 위치를 변경 하지 않고 지도의 확대/축소 수준을 변경 하려면 맵 컨트롤의 `VisibleRegion.Center` 속성에서 현재 위치를 사용 하 여 새 `MapSpan`을 만듭니다. @No__t_0를 사용 하 여 다음과 같이 지도 확대/축소를 제어할 수 있습니다. 그러나 지도 컨트롤에서 직접 확대/축소 하는 작업은 현재 슬라이더의 값을 업데이트할 수 없습니다.

```csharp
Slider slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) =>
{
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

[![확대/축소 맵](map-images/maps-zoom-sml.png "맵 컨트롤 확대/축소")](map-images/maps-zoom.png#lightbox "맵 컨트롤 확대/축소")

또한 [`Map`](xref:Xamarin.Forms.Maps.Map) 클래스에는 바인딩 가능한 속성에 의해 지원 되는 `bool` 형식의 `MoveToLastRegionOnLayoutChange` 속성이 있습니다. 기본적으로이 속성은 `true`입니다 .이는 장치 회전과 같이 레이아웃 변경이 발생할 때 표시 되는 맵 지역이 현재 영역에서 이전 집합 영역으로 이동 함을 나타냅니다. 이 속성을 `false`로 설정 하면 레이아웃 변경이 발생할 때 표시 되는 맵 지역이 가운데에 그대로 유지 됩니다. 다음 예제에서는이 속성을 설정 하는 방법을 보여 줍니다.

```csharp
map.MoveToLastRegionOnLayoutChange = false;
```

### <a name="map-clicks"></a>지도 클릭

`Map`는 맵을 누를 때 발생 하는 `MapClicked` 이벤트를 정의 합니다. @No__t_1 이벤트와 함께 제공 되는 `MapClickedEventArgs` 개체에는 `Position` 형식의 `Position` 이라는 단일 속성이 있습니다. 이벤트가 발생 하면 `Position` 속성의 값이 탭 된 지도 위치로 설정 됩니다.

다음 코드 예제에서는 `MapClicked` 이벤트에 대 한 이벤트 처리기를 보여 줍니다.

```csharp
map.MapClicked += OnMapClicked;

void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

이 예제에서 `OnMapClicked` 이벤트 처리기는 탭 맵 위치를 나타내는 위도 및 경도를 출력 합니다.

<a name="Using_Xaml" />

### <a name="create-a-map-in-xaml"></a>XAML에서 맵 만들기

다음 예제와 같이 맵을 XAML로 만들 수도 있습니다.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map x:Name="MyMap"
                  Clicked="OnMapClicked"
                  WidthRequest="320"
                  HeightRequest="200"                  
                  IsShowingUser="true"
                  MapType="Hybrid" />
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> Xamarin.ios 컨트롤을 참조 하려면 추가 `xmlns` 네임 스페이스 정의가 필요 합니다. 이전 예제에서 `Xamarin.Forms.Maps` 네임 스페이스는 `maps` 키워드를 통해 참조 됩니다.

@No__t_0는 `Map`에 대 한 명명 된 참조를 사용 하 여 코드에서 설정할 수 있습니다.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.ios 고정](~/xamarin-forms/user-interface/map/pins.md).
- [지도 API](xref:Xamarin.Forms.Maps)
- [사용자 지정 렌더러 매핑](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
