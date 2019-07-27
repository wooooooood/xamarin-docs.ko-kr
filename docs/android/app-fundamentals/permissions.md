---
title: Xamarin Android의 사용 권한
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 1426054b60d182f7f40bf3c4b0bf69b2287ad57e
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509404"
---
# <a name="permissions-in-xamarinandroid"></a>Xamarin Android의 사용 권한


## <a name="overview"></a>개요

Android 응용 프로그램은 자체 샌드박스에서 실행 되며, 보안상의 이유로 장치의 특정 시스템 리소스 또는 하드웨어에 액세스할 수 없습니다. 사용자는 이러한 리소스를 사용 하기 전에 앱에 대 한 권한을 명시적으로 부여 해야 합니다. 예를 들어 응용 프로그램은 사용자가 명시적으로 사용 하지 않고 장치에서 GPS에 액세스할 수 없습니다. 앱이 권한 없이 `Java.Lang.SecurityException` 보호 된 리소스에 액세스 하려고 하면 Android에서을 throw 합니다.

앱을 개발할 때 응용 프로그램 개발자가 사용 권한을 **Androidmanifest** 에서 선언 합니다. Android에는 해당 권한에 대 한 사용자의 동의를 얻기 위한 두 가지 워크플로가 있습니다.
 
* Android 5.1 (API 수준 22)를 대상으로 하는 앱의 경우 앱을 설치할 때 사용 권한 요청이 발생 합니다. 사용자가 권한을 부여 하지 않은 경우 앱이 설치 되지 않습니다. 앱이 설치 되 면 앱을 제거 하는 경우를 제외 하 고는 권한을 취소할 수 없습니다.
* Android 6.0 (API 수준 23)부터 사용자에 게 사용 권한을 보다 세부적으로 제어할 수 있습니다. 앱이 장치에 설치 되어 있는 한 사용 권한을 부여 하거나 취소할 수 있습니다. 이 스크린샷에서는 Google 연락처 앱에 대 한 권한 설정을 보여 줍니다. 다양 한 사용 권한을 나열 하 고 사용자가 사용 권한을 설정 하거나 해제할 수 있습니다.

![샘플 사용 권한 화면](permissions-images/01-permissions-check.png) 

Android 앱은 런타임에 확인 하 여 보호 된 리소스에 액세스할 수 있는 권한이 있는지 확인 해야 합니다. 앱에 권한이 없는 경우 사용자에 게 사용 권한을 부여 하기 위해 Android SDK에서 제공 하는 새 Api를 사용 하 여 요청 해야 합니다. 사용 권한은 다음과 같은 두 가지 범주로 구분 됩니다.

* **일반 권한** &ndash; 사용자의 보안 또는 개인 정보에 대 한 보안 위험을 최소화 하는 사용 권한입니다. Android 6.0는 설치 시 일반 권한을 자동으로 부여 합니다. [일반 사용 권한의 전체 목록은](https://developer.android.com/guide/topics/permissions/normal-permissions.html)Android 설명서를 참조 하세요.
* **위험한 권한** &ndash; 일반적인 사용 권한과는 달리 사용자의 보안 또는 개인 정보를 보호 하는 사용 권한이 위험한 사용 권한입니다. 이러한 명시적으로는 사용자가 부여 해야 합니다. SMS 메시지를 보내거나 받는 작업은 위험한 권한이 필요한 작업의 예입니다.

> [!IMPORTANT]
> 사용 권한이 속하는 범주가 시간이 지남에 따라 변경 될 수 있습니다.  "일반" 권한으로 분류 된 사용 권한은 이후 API 수준에서 위험한 권한으로 승격 될 수 있습니다.

위험한 권한은 추가 [_권한 그룹_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups)으로 추가 됩니다. 권한 그룹은 논리적으로 관련 된 사용 권한을 보유 합니다. 사용자가 권한 그룹의 한 멤버에 게 권한을 부여 하면 Android에서 해당 그룹의 모든 구성원에 게 권한을 자동으로 부여 합니다. 예 [`STORAGE`](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) 를 들어 사용 권한 그룹에는 `WRITE_EXTERNAL_STORAGE` 및 `READ_EXTERNAL_STORAGE` 사용 권한이 모두 포함 됩니다. 사용자가에 권한을 부여 하 `READ_EXTERNAL_STORAGE`는 경우에 `WRITE_EXTERNAL_STORAGE` 는 사용 권한이 자동으로 부여 됩니다.

하나 이상의 권한을 요청 하기 전에 사용 권한을 요청 하기 전에 앱에 권한이 필요한 이유를 설명 하는 것이 가장 좋습니다. 사용자가 이론적으로 이해 하면 앱에서 사용자의 권한을 요청할 수 있습니다. 이론적 이해를 이해 하면 사용자가 권한을 부여 하 고 그렇지 않은 경우 영향을 이해 하려는 경우 적절 한 결정을 내릴 수 있습니다. 

권한을 확인 하 고 요청 하는 전체 워크플로를 _런타임 권한_ 검사 라고 하며, 다음 다이어그램에 요약할 수 있습니다. 

[![런타임 권한 확인 흐름 차트](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Android 지원 라이브러리 백에서 이전 버전의 Android에 대 한 사용 권한에 대 한 몇 가지 새로운 Api를 포트 합니다. 이러한 백 포팅 된 Api는 장치에서 Android 버전을 자동으로 확인 하므로 매번 API 수준 검사를 수행 하지 않아도 됩니다.  

이 문서에서는 Xamarin.ios 응용 프로그램에 사용 권한을 추가 하는 방법과 Android 6.0 (API 수준 23) 이상을 대상으로 하는 앱이 런타임 권한 검사를 수행 하는 방법을 설명 합니다.


> [!NOTE]
> 하드웨어에 대 한 권한이 Google Play으로 앱을 필터링 하는 방법에 영향을 줄 수 있습니다. 예를 들어 앱에 카메라에 대 한 권한이 필요한 경우 Google Play는 카메라를 설치 하지 않은 장치에서 Google Play 스토어 앱을 표시 하지 않습니다.


<a name="requirements" />

## <a name="requirements"></a>요구 사항

Xamarin Android 프로젝트에는 [xamarin.ios](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) 패키지를 포함 하는 것이 좋습니다. 이 패키지는 앱이 실행 되는 Android 버전을 지속적으로 확인할 필요 없이 일반적인 인터페이스 하나를 제공 하는 이전 버전의 Android에 대 한 사용 권한 특정 Api를 제공 합니다.


## <a name="requesting-system-permissions"></a>시스템 사용 권한 요청

Android 권한으로 작업 하는 첫 번째 단계는 Android 매니페스트 파일에서 사용 권한을 선언 하는 것입니다. 앱이 대상으로 하는 API 수준에 관계 없이이 작업을 수행 해야 합니다.

Android 6.0 이상을 대상으로 하는 앱은 사용자에 게 과거의 특정 시점에 사용 권한을 부여 했기 때문에 다음 번에 사용 권한이 유효 하다 고 가정할 수 없습니다. Android 6.0를 대상으로 하는 앱은 항상 런타임 권한 검사를 수행 해야 합니다. Android 5.1이 하를 대상으로 하는 앱은 런타임 권한 검사를 수행할 필요가 없습니다.

> [!NOTE]
> 응용 프로그램은 필요한 권한만 요청 해야 합니다.


### <a name="declaring-permissions-in-the-manifest"></a>매니페스트에서 권한 선언

사용 권한은 `uses-permission` 요소를 사용 하 여 **androidmanifest** 에 추가 됩니다. 예를 들어, 응용 프로그램에서 장치의 위치를 찾으려면 적절 한 강좌 위치 권한이 필요 합니다. 다음 두 요소가 매니페스트에 추가 됩니다. 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에 기본 제공 되는 도구 지원을 사용 하 여 권한을 선언할 수 있습니다.

1. **솔루션 탐색기** 에서 **속성** 을 두 번 클릭 하 고 속성 창 **Android 매니페스트** 탭을 선택 합니다.

    [![Android 매니페스트 탭에서 필요한 권한](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. 응용 프로그램에 androidmanifest이 아직 없는 경우에는 없음을 클릭 **합니다. 아래** 와 같이 클릭 하 여 추가 합니다.

    [![AndroidManifest 메시지 없음](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. **필요한 사용 권한** 목록에서 응용 프로그램에 필요한 사용 권한을 선택 하 고 저장 합니다.

    [![카메라 사용 권한 선택](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac용 Visual Studio에 기본 제공 되는 도구 지원을 사용 하 여 사용 권한을 선언할 수 있습니다.

1. **Solution Pad** 에서 프로젝트를 두 번 클릭 하 고 **옵션 > Android 응용 프로그램 > 빌드**를 선택 합니다.

    [![필요한 권한 섹션 표시](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. 프로젝트에 **Androidmanifest**이 아직 없는 경우 **Android 매니페스트 추가** 단추를 클릭 합니다.

    [![프로젝트의 Android 매니페스트가 없습니다.](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. **필요한 사용 권한** 목록에서 응용 프로그램에 필요한 사용 권한을 선택 하 고 **확인을**클릭 합니다.

    [![카메라 사용 권한 선택](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.ios는 빌드 시 빌드를 디버그 하는 데 일부 권한을 자동으로 추가 합니다. 이렇게 하면 응용 프로그램을 더 쉽게 디버그할 수 있습니다. 특히, 두 가지 주목할 만한 권한은 `INTERNET` 및 `READ_EXTERNAL_STORAGE`입니다. 이러한 자동 설정 권한은 **필요한 사용 권한** 목록에서 사용 하도록 설정 된 것으로 표시 되지 않습니다. 그러나 릴리스 빌드는 **필요한 사용 권한** 목록에 명시적으로 설정 된 권한만 사용 합니다. 

Android 5.1 (API 수준 22)이 하를 대상으로 하는 앱의 경우 수행 해야 할 작업이 없습니다. Android 6.0 (API 23 수준 23) 이상에서 실행 되는 앱은 런타임 권한 검사를 수행 하는 방법에 대 한 다음 섹션을 진행 해야 합니다. 


### <a name="runtime-permission-checks-in-android-60"></a>Android 6.0의 런타임 권한 검사

( `ContextCompat.CheckSelfPermission` Android 지원 라이브러리에서 사용 가능) 메서드는 특정 권한이 부여 되었는지 확인 하는 데 사용 됩니다. 이 메서드는 두 값 [`Android.Content.PM.Permission`](xref:Android.Content.PM.Permission) 중 하나를 포함 하는 열거형을 반환 합니다.

* **`Permission.Granted`** &ndash; 지정 된 사용 권한이 부여 되었습니다.
* **`Permission.Denied`** &ndash; 지정 된 사용 권한이 부여 되지 않았습니다.

이 코드 조각은 활동에서 카메라 사용 권한을 확인 하는 방법의 예입니다. 

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

응용 프로그램에 대 한 사용 권한이 필요한 이유를 사용자에 게 알리는 것이 가장 좋습니다. 따라서 사용 권한을 부여 하는 것에 대 한 의사 결정을 내릴 수 있습니다. 이에 대 한 예로 사진 및 지역 태그를 사용 하는 앱이 있습니다. 사용자가 카메라 사용 권한이 필요 하다는 것을 분명 하 게 알 수 있지만, 앱에도 장치의 위치가 필요한 이유는 명확 하지 않을 수 있습니다. 사용자가 위치 권한이 바람직한 이유와 카메라 권한이 필요한 이유를 이해 하는 데 도움이 되는 메시지를 설명 해야 합니다.

메서드 `ActivityCompat.ShouldShowRequestPermissionRationale` 는 사용자에 게 근거를 표시할지 여부를 결정 하는 데 사용 됩니다. 이 메서드는 지정 `true` 된 사용 권한에 대 한 근거를 표시 해야 하는 경우를 반환 합니다. 이 스크린샷에서는 응용 프로그램에서 장치 위치를 알아야 하는 이유를 설명 하는 Snackbar의 예를 보여 줍니다.

![위치에 대 한 설명](permissions-images/07-rationale-snackbar.png) 

사용자가 권한을 `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` 부여 하는 경우 메서드를 호출 해야 합니다. 이 메서드에는 다음과 같은 매개 변수가 필요 합니다.

* **활동** &ndash; 이는 사용 권한을 요청 하는 작업이 며 Android에서 결과를 받습니다.
* **권한** &ndash; 요청 되는 사용 권한의 목록입니다.
* **Requestcode** 호출에`RequestPermissions` 대 한 권한 요청 결과를 일치 시키는 데 사용 되는 &ndash; 정수 값입니다. 이 값은 0보다 커야 합니다.

이 코드 조각은 설명 된 두 메서드의 예입니다. 먼저 권한 근거를 표시할지 여부를 확인 하는 검사가 수행 됩니다. 이론적으로 표시 되는 경우 Snackbar가 설명으로 표시 됩니다. 사용자가 Snackbar에서 **확인** 을 클릭 하면 앱이 사용 권한을 요청 합니다. 사용자가 설명에 동의 하지 않으면 앱에서 권한 요청을 진행 하지 않아야 합니다. 설명 된 내용이 표시 되지 않으면 활동에서 다음과 같은 사용 권한을 요청 합니다.

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

`RequestPermission`사용자에 게 이미 권한이 부여 된 경우에도을 호출할 수 있습니다. 후속 호출은 필요 하지 않지만 사용자에 게 권한을 확인 하거나 취소할 수 있는 기회를 제공 합니다. 가 `RequestPermission` 호출 되 면 컨트롤은 사용 권한을 허용 하는 UI를 표시 하는 운영 체제로 전달 됩니다.  

![Permssion 대화 상자](permissions-images/08-location-permission-dialog.png)

사용자가 완료 된 후에는 Android에서 콜백 메서드를 `OnRequestPermissionResult`통해 결과를 활동에 반환 합니다. 이 메서드는 활동에 의해 구현 되어야 `ActivityCompat.IOnRequestPermissionsResultCallback` 하는 인터페이스의 일부입니다. 이 인터페이스에는 사용자가 선택한 `OnRequestPermissionsResult`작업을 알리기 위해 Android에서 호출 하는 단일 메서드인가 있습니다. 사용자에 게 권한이 부여 되 면 앱이 계속 진행 되어 보호 된 리소스를 사용할 수 있습니다. 을 구현 `OnRequestPermissionResult` 하는 방법의 예는 다음과 같습니다. 

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

이 가이드에서는 Android 장치에서 사용 권한을 추가 하 고 확인 하는 방법에 대해 설명 했습니다. 이전 Android 앱 (API 수준 < 23)과 새 Android 앱 (API 레벨 > 22) 사이에서 사용 권한이 작동 하는 방식의 차이입니다. Android 6.0에서 런타임 권한 검사를 수행 하는 방법에 대해 설명 했습니다.


## <a name="related-links"></a>관련 링크

- [일반 사용 권한 목록](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [런타임 권한 샘플 앱](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Xamarin Android에서 사용 권한 처리](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
