---
title: Xamarin.Android에서 사용 권한
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 204dd903586164691d068a956e741c406df10b36
ms.sourcegitcommit: 9492e417f739772bf264f5944d6bae056e130480
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53746871"
---
# <a name="permissions-in-xamarinandroid"></a>Xamarin.Android에서 사용 권한


## <a name="overview"></a>개요

Android 응용 프로그램 자체 샌드박스에서 실행 하 고 보안에 대 한 이유 권한이 없는 특정 시스템 리소스 또는 하드웨어 장치에서 키를 누릅니다. 명시적으로 사용자는 이러한 리소스를 사용할 수는 앱에 대 한을 부여 해야 합니다. 예를 들어, 응용 프로그램 사용자의 명시적인 허가 없이 장치에 GPS 액세스할 수 없습니다. Android 시킵니다는 `Java.Lang.SecurityException` 앱 권한 없이 보호 된 리소스에 액세스 하려고 하는 경우.

사용 권한에서 선언 되는 **AndroidManifest.xml** 은 앱 개발 하는 경우 응용 프로그램 개발자. Android는 이러한 사용 권한에 대 한 사용자의 동의 가져오기 위한 두 가지 워크플로:
 
* Android 5.1 (API 수준 22) 또는 lower를 대상으로 하는 앱에 대 한 권한 요청에는 앱을 설치할 때 발생 했습니다. 사용자 권한을 부여 하지 않은 경우 앱은 설치 되지 않습니다. 앱이 설치 되 면 앱을 제거 하 여 제외 하 고 사용 권한을 취소 하지 방법이 있습니다.
* Android 6.0 (API 레벨 23)부터 사용자 에게도 더 많이 제어할 권한이 제거 됩니다. 권한을 부여 또는 revoke 사용 권한으로 앱을 장치에 설치 되어 있습니다. 이 스크린샷은 Google 연락처 앱에 대 한 사용 권한 설정을 보여 줍니다. 다양 한 사용 권한을 나열 하 고 사용 또는 사용 권한을 사용 하지 않도록 설정할 수 있습니다.

![사용 권한 화면 샘플](permissions-images/01-permissions-check.png) 

Android 앱 보호 된 리소스에 액세스할 수 있는 권한을 갖고 있는지 확인에 런타임 시 확인 해야 합니다. 앱에 권한이 없는 경우 권한을 부여 하려면 사용자에 대 한 Android SDK에서 제공 하는 새 Api를 사용 하 여 요청을 확인 해야 합니다. 사용 권한은 두 범주로 나뉩니다.

* **일반 사용 권한** &ndash; 이러한 권한은 사용자의 보안 또는 개인 정보를 거의 보안 위험입니다. Android 6.0 설치 시 일반적인 사용 권한을 자동으로 부여 됩니다. 에 대 한 Android 설명서를 참조 하십시오는 [정상적인 사용 권한의 전체 목록은](https://developer.android.com/guide/topics/permissions/normal-permissions.html)합니다.
* **위험한 권한에** &ndash; 일반 권한 달리 위험한 권한은 사용자의 보안 또는 개인 정보를 보호 하는 것입니다. 이러한 이어야 합니다 명시적으로 부여 된 사용자가. SMS 메시지를 받거나 보낼는 위험한 권한 요청 작업의 예시입니다.

> [!IMPORTANT]
> 범주에 속하는 사용 권한을 시간이 지남에 따라 변경 될 수 있습니다.  "normal" 권한이 될 수 있으므로 분류 하는 권한이 상승 된 나중에 API 수준을 위험한 권한 가능성이 있습니다.

위험한 권한에 세분화할 [ _권한 그룹_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups)합니다. 권한 그룹에 논리적으로 관련 된 사용 권한을 유지 됩니다. 사용자 권한 그룹의 구성원에 권한을 부여할 경우 Android 권한을 해당 그룹의 모든 멤버에 자동으로 부여 합니다. 예를 들어, 합니다 [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) 권한 그룹 둘 다 포함 합니다 `WRITE_EXTERNAL_STORAGE` 및 `READ_EXTERNAL_STORAGE` 권한. 사용자에 게 권한을 부여 하는 경우 `READ_EXTERNAL_STORAGE`, 그런 다음 `WRITE_EXTERNAL_STORAGE` 권한을 동시에 자동으로 부여 됩니다.

하나 이상의 권한을 요청 하기 전에 앱 사용 권한을 요청 하기 전에 권한이 필요 하는 이유는 무엇에 대 한 근거 제공이 가장 좋습니다 것. 사용자 근거 인식 되 면 앱 사용자 로부터 권한을 요청할 수 있습니다. 이유를 이해 하면 사용자는 권한을 부여 하 고 그렇지 않으면 관련 된 영향을 이해 하려는 경우 합리적인된 결정을 내릴 수 있습니다. 

전체 워크플로를 확인 하 고 사용 권한을 요청 하 라고는 _런타임 권한이_ 확인 하 고 다음 다이어그램과에서 요약할 수 있습니다. 

[![런타임 권한 검사 흐름 차트](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Android 지원 라이브러리 backports 이전 버전의 Android 권한 위한 새로운 Api의 일부입니다. 자동으로 이러한 백 포팅 Api는 API 수준 검사 될 때마다 수행 하는 데 필요한 이므로 장치의 Android 버전이 확인 해야 합니다.  

이 문서에서는 Xamarin.Android 응용 프로그램에 권한을 추가 하는 방법을 설명 하는 방법 및 Android 6.0 (API 레벨 23)를 대상으로 하는 앱 또는 더 높은 런타임 권한 검사를 수행 해야 합니다.


> [!NOTE]
> 있기 하드웨어에 대 한 사용 권한 앱이 Google Play에서 필터링 되는 방식 저하 될 수 있습니다. 예를 들어, 앱에 카메라에 대 한 권한이 필요한 경우 다음 Google Play 표시 되지 않습니다 앱 카메라가 설치 되지 않은 장치에서 Google Play 스토어에서.


<a name="requirements" />

## <a name="requirements"></a>요구 사항

Xamarin.Android 프로젝트에 포함 되어 있는 것이 좋습니다 합니다 [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) NuGet 패키지. 이전 버전의 Android 하나의 공통을 제공 하는 특정 Api 인터페이스를 필요 없이 지속적으로이 패키지는 백 포팅 권한 앱에서 실행 되는 Android의 버전을 확인 합니다.


## <a name="requesting-system-permissions"></a>시스템 사용 권한 요청

Android 사용 권한으로 작업 하는 첫 번째 단계는 Android에서 사용 권한을 매니페스트 파일을 선언 하는 것입니다. 앱이 대상으로 하는 API 수준에 관계 없이 수행 해야 합니다.

Android 6.0 대상 또는 더 높은 없습니다 없다고 해 서 사용자 권한이 특정 시점 이전에는 사용 권한을 더 유효 다음에 앱입니다. Android 6.0을 대상으로 하는 앱 런타임 사용 권한 검사가 항상 수행 해야 합니다. 5.1 이하로 Android를 대상으로 하는 앱 런타임 권한 확인을 수행할 필요가 없습니다.

> [!NOTE]
> 응용 프로그램에 필요한 권한을 요청만 해야 합니다.


### <a name="declaring-permissions-in-the-manifest"></a>권한을 매니페스트에서 선언

에 추가 됩니다는 **AndroidManifest.xml** 사용 하 여는 `uses-permission` 요소입니다. 예를 들어, 응용 프로그램 장치의 위치를 찾는 경우 정상적으로 필요 하며 위치 권한이 과정입니다. 다음 두 요소를 매니페스트에 추가 됩니다. 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에 기본 제공 도구 지원을 사용 하 여 사용 권한을 선언 하는 것이 가능 합니다.

1. 두 번 클릭 **속성** 에 **솔루션 탐색기** 선택 합니다 **Android 매니페스트** 속성 창에서 탭:

    [![Android 매니페스트 탭에서 필요한 권한](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. 응용 프로그램에 아직 없는 경우는 AndroidManifest.xml, 클릭 **아니요 AndroidManifest.xml을 찾을 수 있습니다. 하나를 추가 하려면 클릭** 아래와 같이:

    [![AndroidManifest.xml 메시지 없음](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. 응용 프로그램에서 필요한 모든 사용 권한을 선택 합니다 **필요한 권한** 나열 하 고 저장 합니다.

    [![예제 카메라 권한 선택](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac 용 Visual Studio에 기본 제공 도구 지원을 사용 하 여 사용 권한을 선언 하는 것이 가능 합니다.

1. 프로젝트를 두 번 클릭 합니다 **Solution Pad** 선택한 **옵션 > 빌드 > Android 응용 프로그램**:

    [![표시 된 필요한 사용 권한 섹션](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. 클릭 합니다 **Android 매니페스트 추가** 프로젝트에 아직 없는 경우 단추를 **AndroidManifest.xml**:

    [![프로젝트의 Android 매니페스트가 없습니다.](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. 응용 프로그램에서 필요한 모든 사용 권한을 선택 합니다 **필요한 권한** 나열 하 고 클릭 **확인**:

    [![예제 카메라 권한 선택](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android 빌드 시 일부 권한을 디버그 빌드에 추가 자동으로 합니다. 이렇게 하면 쉽게 응용 프로그램을 디버깅 합니다. 특히, 두 가지 주목할 만한 권한은 `INTERNET` 고 `READ_EXTERNAL_STORAGE`입니다. 이러한 자동으로 집합 권한을 사용 하도록 설정 표시 되지 것입니다 합니다 **필요한 권한** 목록입니다. 하지만 릴리스 빌드에 명시적으로 설정 된 권한만 사용 합니다 **필요한 권한** 목록입니다. 

Android 5.1 (API 수준 22) 이하인 대상으로 하는 앱에 대 한 없는 더 많은 작업을 수행 해야 하는 됩니다. 앱 (API 23 레벨 23) Android 6.0 이상 실행 될 런타임 권한 검사를 수행 하는 방법은 다음 섹션에 진행 해야 합니다. 


### <a name="runtime-permission-checks-in-android-60"></a>Android 6.0에 대 한 런타임 권한을 확인합니다

`ContextCompat.CheckSelfPermission` 메서드 (Android 지원 라이브러리를 사용 하 여 사용 가능)는 특정 권한이 부여 된 경우 확인을 위해 사용 합니다. 이 메서드는 반환 된 [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) 열거형에 두 값 중 하나:

* **`Permission.Granted`** &ndash; 지정한 사용 권한의 부여 되었습니다.
* **`Permission.Denied`** &ndash; 지정한 사용 권한의 허가 받지 않았습니다.

이 코드 조각은 다음과 같습니다. 활동에서 카메라 사용 권한을 확인 하는 방법의 예 

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

사용 권한을 필요한 이유는 응용 프로그램에 대 한 권한을 부여 하는 합리적인된 결정을 내릴 수 있도록 하는 것에 대 한 사용자에 게는 것이 좋습니다. 이러한 예로 사진 및 지역 태그를 사용 하는 앱 수 합니다. 카메라 사용 권한을 이지만 필요한 것 지울 수 없는 앱을 장치의 위치 해야 하는 이유는 사용자에 게 명확있지 않습니다. 근거는 사용자 위치 권한도 올바른지 및 카메라 권한이 필요 하다 이유는 이해 하는 데 메시지가 표시 됩니다.

`ActivityCompat.ShouldShowRequestPermissionRationale` 메서드는 ' 설계 원리 '를 사용자에 게 표시될지 여부를 결정 하는 합니다. 이 메서드는 반환 `true` 지정 된 사용 권한에 대해 ' 설계 원리 '를 표시 합니다. 이 스크린샷에서 앱 장치의 위치를 알고 있어야 하는 이유를 설명 하는 응용 프로그램에서 표시 하는 Snackbar의 예를 보여줍니다.

![위치에 대 한 설명](permissions-images/07-rationale-snackbar.png) 

사용자에 권한을 부여 하는 경우는 `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` 메서드를 호출 해야 합니다. 이 메서드는 다음 매개 변수가 필요합니다.

* **활동** &ndash; 결과의 Android에서 정보를 얻는 이며 사용 권한을 요청 하는 작업입니다.
* **사용 권한** &ndash; 요청 되는 사용 권한 목록입니다.
* **requestCode** &ndash; 권한 요청 결과 일치 하는 데 사용 되는 정수 값을 `RequestPermissions` 호출 합니다. 이 값은 0보다 커야 합니다.

이 코드는 설명한 두 가지 방법의 예시입니다. 먼저 권한 ' 설계 원리 '를 표시 하는 경우를 결정 하는 검사가 수행 됩니다. 이유를 표시할 경우를 Snackbar는 ' 설계 원리 '를 사용 하 여 표시 됩니다. 사용자가 클릭 하면 **확인** Snackbar에서 다음 앱은 사용 권한을 요청 합니다. 사용자 근거에 동의 하지 않는 경우 앱 권한을 요청 하려면 진행 되지 됩니다. 이유 표시 되지 않으면 활동은 권한을 요청 합니다.

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

`RequestPermission` 이미 사용자 권한을 부여 하는 경우에 호출할 수 있습니다. 후속 호출은 필요 하지 않습니다. 하지만 사용자 사용 권한을 확인 (또는 취소) 하는 기회와 함께 제공 합니다. 때 `RequestPermission` 는 호출 제어는 넘겨 권한을 허용 하는 것에 대 한 UI를 표시 하는 운영 체제:  

![Permssion 대화 상자](permissions-images/08-location-permission-dialog.png)

사용자가 완료 되 면 Android 돌아갑니다 결과 작업 콜백 메서드를 통해 `OnRequestPermissionResult`합니다. 이 메서드는 인터페이스의 일부가 `ActivityCompat.IOnRequestPermissionsResultCallback` 활동에 의해 구현 되어야 합니다. 이 인터페이스에는 단일 메서드 `OnRequestPermissionsResult`, 사용자의 선택 항목의 활동에 알림을 보내야 하는 Android에서 호출 됩니다. 사용자가 권한이, 다음 앱 진행 하 고 보호 된 리소스를 사용할 수 있습니다. 구현 하는 방법의 예로 `OnRequestPermissionResult` 아래에 표시 됩니다. 

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
            Snackbar.Make(layout, Resource.String.permission_available_camera, Snackbar.LengthShort).Show();            
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

이 가이드에는 추가 하 여 Android 장치에 대 한 권한을 확인 하는 방법을 설명 합니다. 사용 권한을 이전 Android 앱 (API 레벨 < 23) 및 새 Android 앱 간에 작동 하는 방법의 차이점 (API 레벨 > 22). 이 Android 6.0에서 런타임 권한 검사를 수행 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [일반 권한의 목록](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [런타임 권한 샘플 앱](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Xamarin.Android에서 사용 권한을 처리](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
