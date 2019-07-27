---
title: Android 매니페스트 사용
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 4a5b0e7d45878dcaa0f3e97411c2ef83d2e26c5a
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510690"
---
# <a name="working-with-the-android-manifest"></a>Android 매니페스트 사용

**Androidmanifest** 은 android 플랫폼에서 응용 프로그램의 기능 및 요구 사항을 설명 하는 데 사용할 수 있는 강력한 파일입니다. 그러나이 작업을 수행 하는 것은 쉽지 않습니다. Xamarin.ios를 사용 하면 사용자 지정 특성을 클래스에 추가 하 여 사용자가 매니페스트를 자동으로 생성 하는 데 사용할 수 있으므로 이러한 문제를 최소화할 수 있습니다. Microsoft의 목표는 사용자의 99%가 수동으로 **Androidmanifest .xml**을 수정할 필요가 없다는 것입니다. 

**Androidmanifest** 은 빌드 프로세스의 일부로 생성 되며 **Properties/androidmanifest .xml** 내에 있는 xml은 사용자 지정 특성에서 생성 된 xml과 병합 됩니다. 결과 병합 된 **Androidmanifest** 은 **obj** 하위 디렉터리에 상주 합니다. 예를 들어, 디버그 빌드에 대 한 **obj/debug/android/AndroidManifest .xml** 에 상주 합니다. 병합 프로세스는 간단 합니다. 코드 내에서 사용자 지정 특성을 사용 하 여 XML 요소를 생성 하 고 이러한 요소를 **Androidmanifest**에 *삽입* 합니다. 



## <a name="the-basics"></a>기본 사항

컴파일 시간에 어셈블리는 [활동](xref:Android.App.Activity) 에서 파생 되 고`abstract` 특성을 [`[Activity]`](xref:Android.App.ActivityAttribute) 선언 하는 비 클래스에 대해 검색 됩니다. 그런 다음 이러한 클래스 및 특성을 사용 하 여 매니페스트를 빌드합니다. 예를 들어, 다음 코드를 고려하세요. 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

이로 인해 **Androidmanifest**에 생성 되는 항목이 없습니다. `<activity/>` 요소를 생성 하려면 다음을 사용 해야 합니다.[`[Activity]`](xref:Android.App.Activity) 
사용자 지정 특성: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

이 예제에서는 다음 xml 조각이 **Androidmanifest**에 추가 됩니다.

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

특성 `[Activity]` 은 형식에 `abstract` 영향을 주지 않습니다. `abstract` 형식은 무시 됩니다.



### <a name="activity-name"></a>작업 이름

Xamarin.ios 5.1부터 작업의 형식 이름은 내보낼 형식의 어셈블리 정규화 된 이름에 대 한 MD5SUM을 기반으로 합니다. 이렇게 하면 서로 다른 두 어셈블리에서 동일한 정규화 된 이름을 제공할 수 있으며 패키징 오류가 발생 하지 않습니다. (Xamarin Android 5.1 이전에는 활동의 기본 형식 이름이 lowercased 네임 스페이스 및 클래스 이름에서 생성 되었습니다.) 

이 기본값을 재정의 하 고 활동의 이름을 명시적으로 지정 하려면 속성을 사용 합니다 [`Name`](xref:Android.App.ActivityAttribute.Name) . 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

이 예제에서는 다음 xml 조각을 생성 합니다.

```xml
<activity android:name="awesome.demo.activity" />
```

*참고*: 이러한 이름을 바꾸면 런타임에 `Name` 형식 조회가 느려질 수 있으므로 이전 버전과의 호환성을 위해서만 속성을 사용 해야 합니다. Lowercased 네임 스페이스와 클래스 이름을 기반으로 하는 활동의 기본 형식 이름을 필요로 하는 레거시 코드가 있는 경우 호환성 유지 관리에 대 한 팁은 [Android 호출 가능 래퍼 이름 지정](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) 을 참조 하세요. 


### <a name="activity-title-bar"></a>활동 제목 표시줄

기본적으로 Android는 실행 될 때 응용 프로그램에 제목 표시줄을 제공 합니다. 이에 사용 되는 값 [`/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label)은입니다. 대부분의 경우이 값은 클래스 이름과 다릅니다. 제목 표시줄에 앱의 레이블을 지정 하려면 [`Label`](xref:Android.App.ActivityAttribute.Label) 속성을 사용 합니다.
예: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

이 예제에서는 다음 xml 조각을 생성 합니다.

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>응용 프로그램 선택에서 시작 가능한

기본적으로 작업은 Android의 응용 프로그램 시작 관리자 화면에 표시 되지 않습니다. 이는 응용 프로그램에 많은 활동이 있을 가능성이 높기 때문에 모든 활동이 필요 하지 않기 때문입니다. 응용 프로그램 시작 관리자에서 시작 가능한 해야 하는 항목을 지정 하려면 [`MainLauncher`](xref:Android.App.ActivityAttribute.MainLauncher) 속성을 사용 합니다. 예: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

이 예제에서는 다음 xml 조각을 생성 합니다.

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```



### <a name="activity-icon"></a>활동 아이콘

기본적으로 작업에는 시스템에서 제공 하는 기본 시작 관리자 아이콘이 제공 됩니다. 사용자 지정 아이콘을 사용 하려면 먼저 **리소스/그릴**수 있는 **.png에 .png** 를 추가 하 고 빌드 작업을 **androidresource**로 설정한 다음 [`Icon`](xref:Android.App.ActivityAttribute.Icon) 속성을 사용 하 여 사용할 아이콘을 지정 합니다. 예를 들어: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

이 예제에서는 다음 xml 조각을 생성 합니다.

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```


### <a name="permissions"></a>사용 권한

Android 매니페스트에 권한 [추가](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)에 설명 된 대로 android 매니페스트에 권한을 추가 하는 경우 이러한 사용 권한은 **Properties/AndroidManifest .xml**에 기록 됩니다. 예를 들어 `INTERNET` 사용 권한을 설정 하면 다음 요소가 **Properties/androidmanifest**에 추가 됩니다. 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

디버그 빌드는 디버그를 용이 하 게 하는 일부 권한을 자동 `INTERNET` 으로 `READ_EXTERNAL_STORAGE`설정 &ndash; 합니다 (예: 및). 이러한 설정은 생성 된 **obj/debug/android/androidmanifest** 에서만 설정 되며,에서 사용 하도록 설정 된 것으로 표시 되지 않습니다. **필요한 사용 권한** 설정입니다. 

예를 들어, **obj/Debug/android/AndroidManifest**에서 생성 된 매니페스트 파일을 검사 하는 경우 다음과 같은 추가 된 권한 요소가 표시 될 수 있습니다. 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

매니페스트의 릴리스 빌드 버전 ( **obj/Debug/android/AndroidManifest**)에서 이러한 사용 권한은 자동으로 구성 *되지 않습니다* . 릴리스 빌드로 전환 하면 앱이 디버그 빌드에서 사용할 수 있는 권한을 상실 하 게 되 면 앱에 대 한 **필요한 권한** 설정에서이 사용 권한을 명시적으로 설정 했는지 확인 합니다 ( **빌드 > Android 참조 Mac용 Visual Studio의 응용 프로그램** **속성 > Android Manifest** In Visual Studio)를 참조 하세요. 




## <a name="advanced-features"></a>고급 기능


### <a name="intent-actions-and-features"></a>의도 작업 및 기능

Android 매니페스트는 활동의 기능을 설명 하는 방법을 제공 합니다. 이것은 [의도](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) 를 통해 수행 되며[`[IntentFilter]`](xref:Android.App.IntentFilterAttribute)
사용자 지정 특성입니다. 을 사용 하 여 작업에 적절 한 작업을 지정할 수 있습니다.[`IntentFilter`](xref:Android.App.IntentFilterAttribute#ctor*)
생성자 및에 적합 한 범주[`Categories`](xref:Android.App.IntentFilterAttribute.Categories)
속성의 값에 따라 달라집니다. 활동을 생성자에 제공 하는 것과 같은 활동을 하나 이상 제공 해야 합니다. `[IntentFilter]`는 여러 번 제공 될 수 있으며, 각를 사용 하면 `<intent-filter/>` `<activity/>`내에서 별도의 요소가 생성 됩니다. 예를 들어:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

이 예제에서는 다음 xml 조각을 생성 합니다.

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```


### <a name="application-element"></a>Application 요소

또한 Android 매니페스트는 전체 응용 프로그램에 대 한 속성을 선언 하는 방법을 제공 합니다. 이는 `<application>` 요소 및 [응용 프로그램](xref:Android.App.ApplicationAttribute) 사용자 지정 특성을 통해 수행 됩니다. 이러한 설정은 작업 단위 설정 대신 응용 프로그램 전체 (어셈블리 전체) 설정입니다. 일반적으로 전체 응용 `<application>` 프로그램에 대 한 속성을 선언한 다음 작업 별로 이러한 설정을 재정의 합니다 (필요한 경우). 

예를 들어, 응용 `Application` 프로그램을 디버그할 수 있으며, 사용자가 읽을 수 있는 이름이 **My App**이며, 해당 스타일을 `Theme.Light` 모두에 대 한 기본 테마로 사용 한다는 것을 나타내기 위해 **AssemblyInfo.cs** 에 다음 특성이 추가 됩니다. 내용 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

이렇게 선언 하면 다음과 같은 XML 조각이 **obj/Debug/android/AndroidManifest**에 생성 됩니다.

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
이 예제에서는 앱의 모든 활동이 기본적으로 `Theme.Light` 스타일로 바뀝니다. 활동 `Theme.Dialog`의 테마를로 설정 하면 해당 활동에만 `Theme.Dialog` 스타일이 사용 되는 반면, 앱의 다른 모든 활동은 기본적으로 `<application>` 요소에 `Theme.Light` 설정 된 스타일로 설정 됩니다. 

요소 `Application` 는 특성을 구성 `<application>` 하는 유일한 방법이 아닙니다. 또는 `<application>` **속성/androidmanifest**의 요소에 직접 특성을 삽입할 수 있습니다. 이러한 설정은 `<application>` **obj/Debug/android/androidmanifest .xml**에 있는 최종 요소로 병합 됩니다. **Properties/AndroidManifest** 의 내용은 항상 사용자 지정 특성에서 제공 되는 데이터를 재정의 합니다. 

`<application>` 요소에서 구성할 수 있는 응용 프로그램 차원의 많은 특성이 있습니다. 이러한 설정에 대 한 자세한 내용은 [applicationattribute](xref:Android.App.ApplicationAttribute)의 [Public 속성](xref:Android.App.ApplicationAttribute) 섹션을 참조 하세요. 



## <a name="list-of-custom-attributes"></a>사용자 지정 특성 목록

-   [Android.App.ActivityAttribute](xref:Android.App.ActivityAttribute) : [/Manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html) XML 조각을 생성 합니다. 
-   [Android.App.ApplicationAttribute](xref:Android.App.ApplicationAttribute) : [/Manifest/application](https://developer.android.com/guide/topics/manifest/application-element.html) XML 조각을 생성 합니다. 
-   [Android.App.InstrumentationAttribute](xref:Android.App.InstrumentationAttribute) : [/Manifest/instrumentation](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML 조각을 생성 합니다. 
-   [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute) : [//Intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML 조각을 생성 합니다. 
-   [Android.App.MetaDataAttribute](xref:Android.App.MetaDataAttribute) : [//Meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html) XML 조각을 생성 합니다. 
-   [Android.App.PermissionAttribute](xref:Android.App.PermissionAttribute) : / [권한](https://developer.android.com/guide/topics/manifest/permission-element.html) XML 조각을 생성 합니다. 
-   [Android.App.PermissionGroupAttribute](xref:Android.App.PermissionGroupAttribute) : / [그룹](https://developer.android.com/guide/topics/manifest/permission-group-element.html) XML 조각을 생성 합니다. 
-   [Android.App.PermissionTreeAttribute](xref:Android.App.PermissionTreeAttribute) : [트리](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML 조각을 생성 합니다. 
-   [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute) : [/Manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html) XML 조각을 생성 합니다. 
-   [Android.App.UsesLibraryAttribute](xref:Android.App.UsesLibraryAttribute) : [/Manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html) XML 조각을 생성 합니다. 
-   [Android.App.UsesPermissionAttribute](xref:Android.App.UsesPermissionAttribute) : [/Manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML 조각을 생성 합니다. 
-   [Android.Content.BroadcastReceiverAttribute](xref:Android.Content.BroadcastReceiverAttribute) : [/Manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html) XML 조각을 생성 합니다. 
-   [Android.Content.ContentProviderAttribute](xref:Android.Content.ContentProviderAttribute) : [/Manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html) XML 조각을 생성 합니다. 
-   [Android.Content.GrantUriPermissionAttribute](xref:Android.Content.GrantUriPermissionAttribute) : [/Manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML 조각을 생성 합니다.

