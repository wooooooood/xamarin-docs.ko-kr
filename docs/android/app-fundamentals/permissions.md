---
title: Permissions In Xamarin.Android
ms.topic: article
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 39ee7f826d4c775ead679a09ce56a7c0f92b60ed
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="permissions-in-xamarinandroid"></a>Permissions In Xamarin.Android


## <a name="overview"></a>개요

자신의 샌드박스에서 android 응용 프로그램 실행 및 보안에 대 한 이유로 특정 시스템 리소스 또는 하드웨어에 대 한 액세스에 없는 장치입니다. 사용자는 명시적으로에 권한을 부여 해야 앱이이 리소스를 사용할 수 있습니다. 예를 들어 응용 프로그램 사용자의 명시적 사용 권한 없이 장치에서 GPS 액세스할 수 없습니다. Android를 throw 합니다는 `Java.Lang.SecurityException` 응용 프로그램의 허가 없이 보호 된 리소스에 액세스 하려고 합니다.

사용 권한에서 선언 되는 **AndroidManifest.xml** 때 응용 프로그램으로 개발 된 응용 프로그램 개발자가 합니다. Android에는 해당 사용 권한에 대 한 사용자의 동의 가져오기 위한 두 가지 워크플로 있습니다.
 
* Android 5.1 (API 수준 22) 또는 lower를 대상으로 하는 응용 프로그램의 경우 권한 요청에는 앱을 설치할 때 발생 했습니다. 사용자 권한을 부여 하지 못한, 하는 경우 앱은 설치 되지 않습니다. 앱이 설치 되 면 응용 프로그램을 제거 하 여 제외 하 고 사용 권한을 취소할 수 없는 방법이 있습니다.
* Android 6.0 (API 수준 23) 부터는 사용자 에게도 더 많이 제어할 권한이 제거 됩니다. 권한을 부여 또는 앱이 장치에 설치 된 상태로 사용 권한을 취소할 수 있습니다. 이 스크린샷은 Google 연락처 앱에 대 한 권한 설정을 보여 줍니다. 다양 한 권한을 나열 하 고 사용 권한을 사용 하지 않도록 설정 하거나 설정 하려면 사용자 수 있습니다.

![샘플 사용 권한 화면](permissions-images/01-permissions-check.png) 

Android 앱은 보호 된 리소스를 액세스할 수 있는 권한이 있는지 확인 하기 위해 실행 시 확인 해야 합니다. 응용 프로그램에 권한이 없는 경우 권한을 부여 하려면 사용자에 대 한 Android SDK에서 제공 된 새 Api를 사용 하 여 요청을 확인 해야 합니다. 사용 권한은 두 가지 범주로 나뉩니다.

* **기본 사용 권한을** &ndash; 하는 권한 있는 사용자의 보안 또는 개인 정보를 거의 보안 위험 합니다. Android 6.0 설치 시 기본 사용 권한을 자동으로 허용 합니다. Android에 대 한 설명서를 참조 하십시오는 [일반 권한의 전체 목록은](https://developer.android.com/guide/topics/permissions/normal-permissions.html)합니다.
* **위험한 권한** &ndash; 일반 권한 달리 위험한 권한은 사용자의 보안 또는 개인 정보를 보호 하는 것입니다. 해야 명시적으로 부여 된 사용자가 있습니다. SMS 메시지를 받거나 보낼는 위험한 권한이 필요한 동작의 예시입니다.

> [!IMPORTANT]
> 범주에 속하는 사용 권한을 시간이 지남에 따라 변경 될 수 있습니다.  "일반" 권한 수 있으므로 범주로 분류 된 있는 사용 권한을 승격할 나중에 API 수준은 위험한 사용 권한 것 같습니다.

위험한 권한은 세분화할 [ _사용 권한 그룹_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups)합니다. 권한 그룹에 논리적으로 관련 된 사용 권한을 포함 됩니다. 사용자 권한 그룹의 구성원에 권한을 부여할 경우 Android 권한을 해당 그룹의 모든 구성원에 자동으로 부여 합니다. 예를 들어는 [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) 권한 그룹을 모두 보유는 `WRITE_EXTERNAL_STORAGE` 및 `READ_EXTERNAL_STORAGE` 사용 권한. 사용자에 게 권한을 부여 하는 경우 `READ_EXTERNAL_STORAGE`, 하면 `WRITE_EXTERNAL_STORAGE` 권한을 동시에 자동으로 부여 됩니다.

하나 이상의 사용 권한을 요청을 하기 전에 것은 이유는 앱에 필요한 사용 권한을 요청 하기 전에 권한에 대 한 근거를 제공 하는 것이 좋습니다. 사용자의 논리적 근거를 인식 되 면 응용 프로그램 사용자 로부터 사용 권한을 요청할 수 있습니다. 도입한 이유를 이해 하면 사용자는 권한을 부여 하 고, 그렇지 않으면 된 영향을 이해를 만들려는 경우 결정을 내릴된 가능 합니다. 

확인 하 고 사용 권한을 요청의 전체 워크플로가 라고는 _런타임 권한_ 확인 하 고 다음 다이어그램에 요약할 수 있습니다. 

[![런타임에 사용 권한 검사 순서도](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Android 지원 라이브러리 backports 일부 이전 버전의 Android에 대 한 사용 권한에 대 한 새 Api. 이러한 backported Api API 수준 검사 될 때마다 수행 되지 않도록 장치에서 android 버전을 자동으로 확인 됩니다.  

이 문서는 Xamarin.Android 응용 프로그램에 권한을 추가 하는 방법에 설명 합니다 방법과 Android 6.0 (API 수준 23)을 대상으로 하는 응용 프로그램 또는 이상이 실행 시 사용 권한 검사를 수행 해야 합니다.


> [!NOTE]
> 있기 하드웨어에 대 한 사용 권한 앱으로 Google Play 필터링 되는 방식 저하 될 수 있습니다. 예를 들어 카메라에 대 한 사용 권한을 요구 하는 응용 프로그램, 하는 경우 다음 Google Play 표시 되지 않습니다 앱 카메라를 설치 하지 않은 장치에서 Google Play 스토어에서.


<a name="requirements" />

## <a name="requirements"></a>요구 사항

Xamarin.Android 프로젝트에 포함 되어이 가장 좋습니다는 [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) NuGet 패키지 합니다. 이전 버전의 Android, 하나의 공통 제공 위한 특정 Api 인터페이스를 필요 없이 지속적으로이 패키지는 backport 권한을 응용 프로그램에서 실행 되는 Android 버전을 확인 합니다.


## <a name="requesting-system-permissions"></a>시스템 사용 권한 요청

Android 사용 권한을 사용 하는 첫 번째 단계는 Android에서 사용 권한을 매니페스트 파일을 선언 하는 것입니다. 응용 프로그램을 대상으로 하는 API 수준에 관계 없이 수행 해야 합니다.

앱 대상으로 Android 6.0 이상 없습니다 없다고 해 서 사용자 권한을 유효 하도록 다음에 이전에 특정 시점에 권한이 부여입니다. Android 6.0을 대상으로 하는 응용 프로그램 런타임 사용 권한 검사가 항상 수행 해야 합니다. Android 5.1을 대상으로 하는 응용 프로그램 실행 시 사용 권한 검사를 수행 하도록 필요가 없습니다.

> [!NOTE]
> 응용 프로그램에 필요한 권한을 요청만 해야 합니다.


### <a name="declaring-permissions-in-the-manifest"></a>권한을 매니페스트에서 선언

사용 권한을에 추가 되는 **AndroidManifest.xml** 와 `uses-permission` 요소입니다. 예를 들어 응용 프로그램 장치 위치를 찾는 경우 제대로 필요로 하며 과정 위치 사용 권한. 다음과 같은 두 가지 요소가 매니페스트에 추가 됩니다. 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에 기본 제공 도구 지원을 사용 하 여 사용 권한을 선언 하는 것이 불가능:

1. 두 번 클릭 **속성** 에 **솔루션 탐색기** 선택 하 고는 **Android 매니페스트** 속성 창에서 탭:

    [![Android 매니페스트 탭에서 필요한 권한](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. 응용 프로그램에 아직 없는 경우는 AndroidManifest.xml, 클릭 **아니요 AndroidManifest.xml 찾을 수 있습니다. 하나를 추가 하려면 클릭** 아래와 같이:

    [![No AndroidManifest.xml message](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. 응용 프로그램에서 필요한 모든 사용 권한을 선택은 **필요한 권한** 나열 하 고 저장 합니다.

    [![예제 카메라 권한 선택](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 용 Visual Studio에 기본 제공 도구 지원을 사용 하 여 사용 권한을 선언 하는 것이 불가능:

1. 프로젝트를 두 번 클릭은 **솔루션 패드** 선택 **옵션 > 빌드 > Android 응용 프로그램**:

    [![표시 된 필요한 사용 권한 섹션](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. 클릭는 **Android 매니페스트 추가** 프로젝트에 아직 없는 경우 단추는 **AndroidManifest.xml**:

    [![프로젝트의 Android 매니페스트가](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. 응용 프로그램에서 필요한 모든 사용 권한을 선택은 **필요한 권한** 나열 하 고 클릭 **확인**:

    [![예제 카메라 권한 선택](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android는 자동으로 추가 일부 사용 권한이 빌드 시 디버그 빌드. 이렇게 하면 보다 쉽게 응용 프로그램을 디버깅 있게 됩니다. 특히, 두 가지 주목할 만한 권한은 `INTERNET` 및 `READ_EXTERNAL_STORAGE`합니다. 사용 권한을 이러한 자동으로 설정에서 활성화 하도록 표시 되지 것입니다는 **필요한 권한** 목록입니다. 명시적으로 설정 하는 권한만 사용, 릴리스 빌드에 **필요한 권한** 목록입니다. 

Android 5.1 (API 수준 22) 또는 lower를 대상으로 하는 응용 프로그램의 경우 작업을 수행 해야 하는 자세한 일은 없습니다. 이상 Android 6.0 (API 23 수준 23)에서 실행 되는 앱 런타임에 권한 확인이 수행 하는 방법에는 다음 섹션 진행 해야 합니다. 


### <a name="runtime-permission-checks-in-android-60"></a>Android 6.0에서 런타임 권한 확인

`ContextCompat.CheckSelfPermission` 메서드 (Android 지원 라이브러리에서 사용 가능) 특정 권한이 부여 된 경우 확인을 위해 사용 합니다. 이 메서드는 반환 된 [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) 두 값 중 하나가 열거형.

* **`Permission.Granted`** &ndash; 지정 된 사용 권한 부여 되었습니다.
* **`Permission.Denied`** &ndash; 지정 된 권한이 부여 되지 않았습니다.

이 코드 조각은 활동에서 카메라 권한을 확인 하는 방법의 예입니다. 

```csharp
if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.Camera) == (int)Permission.Granted) 
{
    // We have permission, go ahead and use the camera.
} 
else 
{
    // Camera permission is not granted. If necessary display rationale & request.
}
```

에 속하는지에 대 한 이유는 권한을 응용 프로그램에 필요한 사용 권한을 부여할 결정을 내릴된 수 있도록 사용자에 게 알리는 것이 좋습니다. 이러한 예로 사진 및 지리적 태그를 사용 하는 응용 프로그램 수에 있습니다. 카메라 사용 권한 작업은 필요 하지만 그 지울 수 없는 응용 프로그램이 또한 장치의 위치 해야 하는 이유는 사용자에 게 명확있지 않습니다. 근거 이유 위치 사용 권한은 것이 카메라 권한이 필요 하 고 이해 하는 사용자가 메시지를 표시 해야 합니다.

`ActivityCompat.ShouldShowRequestPermissionRational` 메서드를 근거는 사용자에 게 표시할지 여부를 확인 하려면 사용 합니다. 이 메서드는 반환 `true` 지정된 된 사용 권한 대 한 이유를 표시 합니다. 이 스크린샷에서 앱을 장치의 위치를 알고 있어야 하는 이유를 설명 하는 응용 프로그램에서 표시할 Snackbar의 예를 보여 줍니다.

![위치에 대 한 설명](permissions-images/07-rationale-snackbar.png) 

사용자는 권한을 부여 하는 경우는 `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` 메서드를 호출 해야 합니다. 이 메서드는 다음 매개 변수가 필요합니다.

* **활동** &ndash; 하는 권한을 요청 하 고 결과의 Android에서 알림을 받을 작업입니다.
* **사용 권한** &ndash; 요청 되는 사용 권한 목록입니다.
* **requestCode** &ndash; 권한 요청을 결과 일치 하는 데 사용 되는 정수 값을 `RequestPermissions` 호출 합니다. 이 값은 0보다 커야 합니다.

이 코드 조각은 설명한 두 가지 방법의 예시입니다. 첫째, 권한 근거를 표시할지 여부를 결정 하는 검사는 수행 됩니다. 도입한 이유를 표시할 경우 Snackbar는 근거 함께 표시 됩니다. 사용자가 클릭할 경우 **확인** Snackbar에서 다음 응용 프로그램은 사용 권한을 요청 합니다. 사용자가 근거를 받아들이지 않습니다, 응용 프로그램 권한을 요청 하려면 진행 되지 해야 합니다. 근거 표시 되지 않으면 활동은 권한을 요청 합니다.

```csharp
if (ActivityCompat.ShouldShowRequestPermissionRationale(this, Manifest.Permission.AccessFineLocation)) 
{
    // Provide an additional rationale to the user if the permission was not granted
    // and the user would benefit from additional context for the use of the permission.
    // For example if the user has previously denied the permission.
    Log.Info(TAG, "Displaying camera permission rationale to provide additional context.");

    var requiredPermissions = new String[] { Manifest.Permission.AccessFineLocation };
    Snackbar.Make(layout, 
                   Resource.String.permission_location_rationale,
                   Snackbar.LengthIndefinite)
            .SetAction(Resource.String.ok, 
                       new Action<View>(delegate(View obj) {
                           ActivityCompat.RequestPermissions(this, requiredPermissions, REQUEST_LOCATION);
                       }    
            )
    ).Show();
}
else 
{
    ActivityCompat.RequestPermissions(this, new String[] { Manifest.Permission.Camera }, REQUEST_LOCATION);
}
```

`RequestPermission` 이미 사용자 권한을 부여 하는 경우에 호출할 수 있습니다. 후속 호출은 필요 하지 않습니다. 하지만 사용 권한을 확인 (또는 취소) 하는 사용자 제공. 때 `RequestPermission` 은 호출 제어로 이루어집니다 사용 권한을 적용 하기 위한 UI를 표시 하는 운영 체제:  

![Permssion 대화 상자](permissions-images/08-location-permission-dialog.png)

사용자가 완료 되 면 Android 결과 반환 합니다 활동에 콜백 메서드를 통해 `OnRequestPermissionResult`합니다. 이 메서드는 인터페이스의 일부 `ActivityCompat.IOnRequestPermissionsResultCallback` 활동에 의해 구현 되어야 합니다. 이 인터페이스에는 단일 메서드 `OnRequestPermissionsResult`, 알릴 사용자의 선택 항목의 활동에는 Android에서 호출 됩니다. 사용자가 사용 권한을 허가 하 고, 그런 다음 응용 프로그램 계속 진행 하 고 보호 된 리소스를 사용 하 여 수 있습니다. 구현 하는 방법의 예로 `OnRequestPermissionResult` 아래에 표시 됩니다. 

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Permission[] grantResults)
{
    if (requestCode == REQUEST_LOCATION) 
    {
        // Received permission result for camera permission.
        Log.Info(TAG, "Received response for Location permission request.");

        // Check if the only required permission has been granted
        if ((grantResults.Length == 1) && (grantResults[0] == Permission.Granted)) {
            // Location permission has been granted, okay to retrieve the location of the device.
            Log.Info(TAG, "Location permission has now been granted.");
            Snackbar.Make(layout, Resource.String.permision_available_camera, Snackbar.LengthShort).Show();            
        } 
        else 
        {
            Log.Info(TAG, "Location permission was NOT granted.");
            Snackbar.Make(layout, Resource.String.permissions_not_granted, Snackbar.LengthShort).Show();
        }
    } 
    else 
    {
        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```  


## <a name="summary"></a>요약

이 가이드에는 추가 하 고 Android 장치에서 사용 권한을 확인 하는 방법을 설명 합니다. 사용 권한 (API 23 <를 수준) 이전 Android 앱과 새 Android 앱 간에 작동 하는 방법의 차이점 (API는 > 22 수준). Android 6.0에서 런타임 실행 권한 검사를 수행 하는 방법에 설명 합니다.


## <a name="related-links"></a>관련 링크

- [일반 사용 권한 목록](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [런타임 권한 샘플 앱](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Xamarin.Android에서 처리 권한](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest)
