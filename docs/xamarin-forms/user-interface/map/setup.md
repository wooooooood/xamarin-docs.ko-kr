---
title: Xamarin.ios 맵 초기화 및 구성
description: 응용 프로그램에서 maps 기능을 사용 하려면 Xamarin.ios NuGet 패키지가 필요 합니다. 또한 사용자의 위치에 액세스 하려면 응용 프로그램에 대 한 위치 권한이 있어야 합니다.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/07/2020
ms.openlocfilehash: 177359dfe081cba3cc43031d807f669f93a31ee9
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516529"
---
# <a name="xamarinforms-map-initialization-and-configuration"></a>Xamarin.ios 맵 초기화 및 구성

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

컨트롤 [`Map`](xref:Xamarin.Forms.Maps.Map) 은 각 플랫폼에서 네이티브 맵 컨트롤을 사용 합니다. 이를 통해 사용자는 빠르고 친숙 한 지도를 제공할 수 있지만 각 플랫폼 API 요구 사항을 준수 하려면 몇 가지 구성 단계가 필요 합니다.

## <a name="map-initialization"></a>맵 초기화

컨트롤 [`Map`](xref:Xamarin.Forms.Maps.Map) 은 솔루션의 모든 프로젝트에 추가 해야 하는 [xamarin.ios](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet 패키지에서 제공 됩니다.

[Xamarin.ios](https://www.nuget.org/packages/Xamarin.Forms.Maps/) NuGet 패키지를 설치한 후에는 각 플랫폼 프로젝트에서 초기화 해야 합니다.

IOS에서 메서드를 호출한 *후* `Xamarin.Forms.Forms.Init` 메서드 **AppDelegate.cs** 를 `Xamarin.FormsMaps.Init` 호출 하 여 AppDelegate.cs에서 발생 해야 합니다.

```csharp
Xamarin.FormsMaps.Init();
```

Android에서 메서드를 호출한 *후* `Xamarin.Forms.Forms.Init` 메서드 **MainActivity.cs** 를 `Xamarin.FormsMaps.Init` 호출 하 여 MainActivity.cs에서 발생 해야 합니다.

```csharp
Xamarin.FormsMaps.Init(this, savedInstanceState);
```

UWP (유니버설 Windows 플랫폼)에서이는 `Xamarin.FormsMaps.Init` `MainPage` 생성자에서 메서드를 호출 하 여 **MainPage.xaml.cs** 에서 발생 해야 합니다.

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

UWP에 필요한 인증 토큰에 대 한 자세한 내용은 [유니버설 Windows 플랫폼](#universal-windows-platform)를 참조 하세요.

NuGet 패키지를 추가 하 고 각 응용 프로그램 내에서 초기화 메서드를 호출 하면 `Xamarin.Forms.Maps` 공유 코드 프로젝트에서 api를 사용할 수 있습니다.

## <a name="platform-configuration"></a>플랫폼 구성

지도를 표시 하려면 Android 및 유니버설 Windows 플랫폼 (UWP)에서 추가 구성이 필요 합니다. 또한 iOS, Android 및 UWP에서 사용자의 위치에 액세스 하려면 응용 프로그램에 대 한 위치 권한이 있어야 합니다.

### <a name="ios"></a>iOS

IOS에서 지도를 표시 하 고 상호 작용 하려면 추가 구성이 필요 하지 않습니다. 그러나 위치 서비스에 액세스 하려면 info.plist에서 다음 키를 설정 해야 합니다 **.**

- iOS 11 이상
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26)– 응용 프로그램이 사용 중일 때 위치 서비스를 사용 하는 경우
  - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/bundleresources/information_property_list/nslocationalwaysandwheninuseusagedescription)– 항상 위치 서비스를 사용 하는 경우
- iOS 10 및 이전 버전
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26)– 응용 프로그램이 사용 중일 때 위치 서비스를 사용 하는 경우
  - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18)– 항상 위치 서비스를 사용 하는 경우    

IOS 11 및 이전 버전을 지원 하기 위해 `NSLocationWhenInUseUsageDescription`, 및 `NSLocationAlwaysAndWhenInUseUsageDescription` `NSLocationAlwaysUsageDescription`의 세 가지 키를 모두 포함할 수 있습니다.

**Info.plist** 에서 이러한 키에 대 한 XML 표현은 다음과 같습니다. 응용 프로그램에서 위치 `string` 정보를 사용 하는 방법을 반영 하도록 값을 업데이트 해야 합니다.

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your application is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist** 파일을 편집 하는 동안 **Info.plist** 항목을 **소스** 뷰에 추가할 수도 있습니다.

![IOS 8 용 info.plist](setup-images/ios8-map-permissions.png "iOS 8 필수 정보. info.plist 항목")

그러면 응용 프로그램에서 액세스를 요청 하는 사용자의 위치에 액세스 하려고 할 때 프롬프트가 표시 됩니다.

[![IOS의 위치 권한 요청 스크린샷](setup-images/permission-ios.png "iOS 권한 요청")](setup-images/permission-ios-large.png#lightbox "iOS 권한 요청")

### <a name="android"></a>Android

Android에서 지도를 표시 하 고 상호 작용 하는 구성 프로세스는 다음과 같습니다.

1. Google Maps API 키를 가져와서 매니페스트에 추가 합니다.
1. 매니페스트의 Google Play services 버전 번호를 지정 합니다.
1. 매니페스트에서 Apache HTTP 레거시 라이브러리의 요구 사항을 지정 합니다.
1. 필드 매니페스트에서 WRITE_EXTERNAL_STORAGE 권한을 지정 합니다.
1. 필드 매니페스트에서 위치 사용 권한을 지정 합니다.
1. 필드 `MainActivity` 클래스의 요청 런타임 위치 권한입니다.

올바르게 구성 된 매니페스트 파일의 예는 샘플 응용 프로그램의 [Androidmanifest .xml](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/WorkingWithMaps.Android/Properties/AndroidManifest.xml) 을 참조 하십시오.

#### <a name="get-a-google-maps-api-key"></a>Google Maps API 키 가져오기

Android에서 [Google MAPS api](https://developers.google.com/maps/documentation/android/) 를 사용 하려면 api 키를 생성 해야 합니다. 이렇게 하려면 [Google MAPS API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)의 지침을 따르세요.

API 키를 가져온 후에는 `<application>` **Properties/AndroidManifest .xml** 파일의 요소 내에 추가 해야 합니다.

```xml
<application ...>
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="PASTE-YOUR-API-KEY-HERE" />
</application>
```

이렇게 하면 API 키가 매니페스트에 포함 됩니다. 유효한 API 키가 없으면 컨트롤 [`Map`](xref:Xamarin.Forms.Maps.Map) 은 빈 그리드를 표시 합니다.

> [!NOTE]
> `com.google.android.geo.API_KEY`는 API 키에 대해 권장 되는 메타 데이터 이름입니다. 이전 버전과의 호환성을 `com.google.android.maps.v2.API_KEY` 위해 메타 데이터 이름을 사용할 수 있지만 ANDROID Maps API v 2에 대 한 인증만 허용 합니다.

APK가 Google Maps에 액세스 하려면 APK에 서명 하는 데 사용 하는 모든 키 저장소 (디버그 및 릴리스)에 대해 SHA-1 지문 및 패키지 이름을 포함 해야 합니다. 예를 들어 디버그에 하나의 컴퓨터를 사용 하 고 다른 컴퓨터에서 릴리스 APK를 생성 하는 경우 첫 번째 컴퓨터의 디버그 키 저장소의 SHA-1 인증서 지문을 포함 하 고 두 번째 컴퓨터의 릴리스 키 저장소 SHA-1 인증서 지문을 포함 해야 합니다. 또한 앱의 **패키지 이름이** 변경 되는 경우 키 자격 증명을 편집 해야 합니다. [Google MAPS API 키 가져오기를](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)참조 하세요.

#### <a name="specify-the-google-play-services-version-number"></a>Google Play services 버전 번호를 지정 합니다.

`<application>` **Androidmanifest**의 요소 내에 다음 선언을 추가 합니다.

```xml
<meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
```

여기에는 응용 프로그램이 컴파일되는 Google Play services 버전이 매니페스트에 포함 됩니다.

#### <a name="specify-the-requirement-for-the-apache-http-legacy-library"></a>Apache HTTP 레거시 라이브러리의 요구 사항 지정

Xamarin Forms 응용 프로그램이 API 28 이상을 대상으로 하는 경우 `<application>` **Androidmanifest**의 요소 내에 다음 선언을 추가 해야 합니다.

```xml
<uses-library android:name="org.apache.http.legacy" android:required="false" />    
```

그러면 응용 프로그램에 Android 9 `bootclasspath` 의에서 제거 된 Apache Http 클라이언트 라이브러리를 사용 하 라는 메시지가 나타납니다.

#### <a name="specify-the-write_external_storage-permission"></a>WRITE_EXTERNAL_STORAGE 사용 권한 지정

응용 프로그램에서 API 22이 하를 대상으로 하는 경우 `WRITE_EXTERNAL_STORAGE` `<manifest>` 요소의 자식으로 매니페스트에 권한을 추가 해야 할 수 있습니다.

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

응용 프로그램이 API 23 이상을 대상으로 하는 경우에는이 기능이 필요 하지 않습니다.

#### <a name="specify-location-permissions"></a>위치 권한 지정

응용 프로그램에서 사용자의 위치에 액세스 해야 하는 경우에는 `ACCESS_COARSE_LOCATION` 또는 `ACCESS_FINE_LOCATION` 권한을 `<manifest>` 요소의 자식으로 매니페스트 (또는 둘 다)에 추가 하 여 권한을 요청 해야 합니다.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="com.companyname.myapp">
  ...
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
</manifest>
```

이 `ACCESS_COARSE_LOCATION` 사용 권한을 통해 API는 WiFi 또는 모바일 데이터 또는 둘 다를 사용 하 여 장치 위치를 확인할 수 있습니다. 이 `ACCESS_FINE_LOCATION` 사용 권한을 통해 API는 GPS (전역 위치 시스템), WiFi 또는 모바일 데이터를 사용 하 여 가능한 한 정확한 위치를 결정할 수 있습니다.

또는 매니페스트 편집기를 사용 하 여 다음 권한을 추가 하 여 이러한 권한을 설정할 수 있습니다.

- `AccessCoarseLocation`
- `AccessFineLocation`

이러한 작업은 아래 스크린샷에 표시 됩니다.

![Android에 필요한 권한](setup-images/android-map-permissions.png "Android에 필요한 권한")

#### <a name="request-runtime-location-permissions"></a>런타임 위치 요청 권한

응용 프로그램이 API 23 이상을 대상으로 하 고 사용자의 위치에 액세스 해야 하는 경우 런타임에 필요한 권한이 있는지 확인 하 고, 그렇지 않은 경우 요청 해야 합니다. 이렇게 하려면 다음을 수행합니다.

1. `MainActivity` 클래스에 다음 필드를 추가 합니다.

    ```csharp
    const int RequestLocationId = 0;

    readonly string[] LocationPermissions =
    {
        Manifest.Permission.AccessCoarseLocation,
        Manifest.Permission.AccessFineLocation
    };
    ```

1. `MainActivity` 클래스에서 다음 `OnStart` 재정의를 추가 합니다.

    ```csharp
    protected override void OnStart()
    {
        base.OnStart();

        if ((int)Build.VERSION.SdkInt >= 23)
        {
            if (CheckSelfPermission(Manifest.Permission.AccessFineLocation) != Permission.Granted)
            {
                RequestPermissions(LocationPermissions, RequestLocationId);
            }
            else
            {
                // Permissions already granted - display a message.
            }
        }
    }
    ```

    응용 프로그램이 API 23 이상을 대상으로 하는 경우이 코드는 `AccessFineLocation` 권한에 대 한 런타임 권한 검사를 수행 합니다. 사용 권한이 부여 되지 않은 경우 `RequestPermissions` 메서드를 호출 하 여 권한 요청을 수행 합니다.

1. `MainActivity` 클래스에서 다음 `OnRequestPermissionsResult` 재정의를 추가 합니다.

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Permission[] grantResults)
    {
        if (requestCode == RequestLocationId)
        {
            if ((grantResults.Length == 1) && (grantResults[0] == (int)Permission.Granted))
                // Permissions granted - display a message.
            else
                // Permissions denied - display a message.
        }
        else
        {
            base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
        }
    }
    ```

    이 재정의는 권한 요청의 결과를 처리 합니다.

이 코드의 전반적인 효과는 응용 프로그램이 사용자의 위치를 요청할 때 권한을 요청 하는 다음과 같은 대화 상자가 표시 된다는 것입니다.

[![Android의 위치 권한 요청 스크린샷](setup-images/permission-android.png "Android 권한 요청")](setup-images/permission-android-large.png#lightbox "Android 권한 요청")

### <a name="universal-windows-platform"></a>UWP

UWP에서는 응용 프로그램이 인증 되어야 맵을 표시 하 고 map service를 사용할 수 있습니다. 응용 프로그램을 인증 하려면 맵 인증 키를 지정 해야 합니다. 자세한 내용은 [지도 인증 키 요청](/windows/uwp/maps-and-location/authentication-key)을 참조 하세요. 그런 다음 `FormsMaps.Init("AUTHORIZATION_TOKEN")` 메서드 호출에서 인증 토큰을 지정 하 여 Bing Maps를 사용 하 여 응용 프로그램을 인증 합니다.

> [!NOTE]
> UWP에서 지 오 코딩와 같은 map services를 사용 하려면 `MapService.ServiceToken` 속성을 인증 키 값으로 설정 해야 합니다. 다음 코드 줄을 사용 하 여이 작업 `Windows.Services.Maps.MapService.ServiceToken = "INSERT_AUTH_TOKEN_HERE";`을 수행할 수 있습니다.

또한 응용 프로그램에서 사용자의 위치에 액세스 해야 하는 경우 패키지 매니페스트에서 위치 기능을 사용 하도록 설정 해야 합니다. 이렇게 하려면 다음을 수행합니다.

1. **솔루션 탐색기**에서 **appxmanifest.xml** 를 두 번 클릭 하 고 **기능** 탭을 선택 합니다.
1. **기능** 목록에서 **위치**확인란을 선택 합니다. 이렇게 하면 `location` 장치 기능이 패키지 매니페스트 파일에 추가 됩니다.

    ```xml
    <Capabilities>
      <!-- DeviceCapability elements must follow Capability elements (if present) -->
      <DeviceCapability Name="location"/>
    </Capabilities>
    ```

#### <a name="release-builds"></a>릴리스 빌드

UWP 릴리스 빌드는 .NET 네이티브 컴파일을 사용 하 여 응용 프로그램을 네이티브 코드로 직접 컴파일합니다. 그러나이로 인해 UWP의 [`Map`](xref:Xamarin.Forms.Maps.Map) 컨트롤에 대 한 렌더러는 실행 파일에서 연결 되지 않을 수 있습니다. `Forms.Init` **APP.XAML.CS**에서 메서드의 UWP 관련 오버 로드를 사용 하 여이 문제를 해결할 수 있습니다.

```csharp
var assembliesToInclude = new [] { typeof(Xamarin.Forms.Maps.UWP.MapRenderer).GetTypeInfo().Assembly };
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
```

이 코드는 `Xamarin.Forms.Maps.UWP.MapRenderer` 클래스가 있는 어셈블리를 `Forms.Init` 메서드에 전달 합니다. 이렇게 하면 어셈블리가 .NET 네이티브 컴파일 프로세스에 의해 실행 파일에 연결 되지 않습니다.

> [!IMPORTANT]
> 이 작업을 수행 하지 못하면 릴리스 빌드 [`Map`](xref:Xamarin.Forms.Maps.Map) 를 실행할 때 컨트롤이 표시 되지 않습니다.

## <a name="related-links"></a>관련 링크

- [Maps 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.ios 고정](~/xamarin-forms/user-interface/map/pins.md).
- [맵 API](xref:Xamarin.Forms.Maps)
- [사용자 지정 렌더러 매핑](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
