---
title: "Android 매니페스트에서 사용"
ms.topic: article
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: aa2d2ce6cabe9c394b9807ca3d6328da5b4ba311
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-the-android-manifest"></a>Android 매니페스트에서 사용


## <a name="overview"></a>개요

**AndroidManifest.xml** 는 Android 응용 프로그램의 요구 사항과 기능을 설명 하기 위해 수 있는 Android 플랫폼에 있는 강력한 파일입니다. 그러나 사용 하 여 작업 쉽지 않습니다. Xamarin.Android 있습니다에 대 한 매니페스트를 자동으로 생성 하는 데 사용할 수는 클래스에 사용자 지정 특성을 추가할 수 있으므로 이러한 문제를 최소화 하는 데 도움이 됩니다. 목표는 99%의 사용자를 수동으로 수정 필요 하지는 **AndroidManifest.xml**합니다. 

**AndroidManifest.xml** 복 XML 및 빌드 프로세스의 일부로 생성 **Properties/AndroidManifest.xml** 사용자 지정 특성에서 생성 되는 XML과 병합 됩니다. 병합 된 **AndroidManifest.xml** 에 **obj** ; 하위 디렉터리에 있는 예를 들어 **obj/Debug/android/AndroidManifest.xml** 디버그 빌드에 대 한 . 병합 프로세스는 trivial: 코드 내에서 사용자 지정 특성을 사용 하 여 XML 요소를 생성 하 고 *삽입* 이러한 요소를 **AndroidManifest.xml**합니다. 



## <a name="the-basics"></a>기본 사항

어셈블리에 대 한 검사는 컴파일 타임에 비-`abstract` 에서 파생 된 클래스 [활동](https://developer.xamarin.com/api/type/Android.App.Activity/) 있고는 [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) 특성에 선언 되었습니다. 다음에 매니페스트를 빌드합니다 이러한 클래스와 특성을 사용 합니다. 예를 들어, 다음 코드를 고려하세요. 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

이 인해 생성 되 고 아무 **AndroidManifest.xml**합니다. 원하는 경우는 `<activity/>` 생성 되는 요소를 사용 해야는 [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) 사용자 지정 특성: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

이 예제에서는 다음 xml 조각에 추가할 **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` 특성 영향을 주지 않습니다에 `abstract` 형식; `abstract` 형식은 무시 됩니다.



### <a name="activity-name"></a>작업 이름

Xamarin.Android 5.1부터, 활동의 형식 이름을 기반으로 내보내는 형식의 어셈블리 정규화 된 이름 MD5SUM 합니다. 따라서 동일한 정규화 된 이름을 두 명의 다른 어셈블리에서 제공 하는 패키징 오류가 발생 하지을 수 있습니다. (Xamarin.Android 5.1 하기 전에 활동의 기본 형식 이름에서 만들었는지 바뀌거나 네임 스페이스 및 클래스 이름입니다.) 

이 기본값을 재정의 하 고 사용 하 여 활동의 이름을 명시적으로 지정 하려는 경우는 [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) 속성: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

이 예에서는 다음 xml 조각은 생성합니다.

```xml
<activity android:name="awesome.demo.activity" />
```

*참고*: 사용할지는 `Name` 이전 버전과 호환성 이유로으로 이름 바꾸기에 대해서만 속성 런타임 시 형식 조회 속도가 느려질 수 있습니다. 바뀌거나 네임 스페이스를 기반으로 할 활동의 기본 형식 이름과 클래스 이름 참조 필요로 하는 기존 코드가 있는 경우 [Android 호출 가능 래퍼 명명](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) 호환성을 유지 관리에 대 한 팁입니다. 


### <a name="activity-title-bar"></a>작업 제목 표시줄

기본적으로 Android 응용 프로그램에 부여 제목 표시줄을 실행 하는 경우. 이 사용 되는 값은 [ `/manifest/application/activity/@android:label` ](http://developer.android.com/guide/topics/manifest/activity-element.html#label)합니다. 대부분의 경우에서 클래스 이름에서이 값은 달라 집니다. 제목 표시줄에을 앱의 레이블을 지정 하려면 사용 된 [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) 속성입니다.
예: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

이 예에서는 다음 xml 조각은 생성합니다.

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>응용 프로그램 선택에서 컴파일되

기본적으로 활동 Android의 응용 프로그램 시작 관리자 화면에 표시 되지 않습니다. 이 되기 마련 작업은 많은 응용 프로그램에서 모든 항목에 대 한 아이콘 않으려면 때문입니다. 지정 하는 응용 프로그램 실행 프로그램에서 시작 가능한를 사용 하 여는 [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) 속성입니다. 예: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

이 예에서는 다음 xml 조각은 생성합니다.

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

기본적으로 작업은 시스템에서 제공 하는 기본 시작 관리자 아이콘을 제공 됩니다. 사용자 지정 아이콘을 사용 하려면 먼저 추가 프로그램 **.png** 를 **리소스/그릴**, 빌드 작업 설정 **AndroidResource**를 사용 하 여는 [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) 속성을 통해 사용할 아이콘을 지정 합니다. 예: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

이 예에서는 다음 xml 조각은 생성합니다.

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

추가 하면 사용 권한을 Android 매니페스트 (에 설명 된 대로 [Android 매니페스트에 권한 추가](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/)), 이러한 사용 권한은에 기록 됩니다 **Properties/AndroidManifest.xml**합니다. 예를 들어, 설정 하는 경우는 `INTERNET` 권한, 다음 요소에 추가 됩니다 **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

디버그 빌드에 보다 쉽게 디버깅을 수행할 수 있는 일부 권한이 자동으로 설정 (같은 `INTERNET` 및 `READ_EXTERNAL_STORAGE`) &ndash; 이 설정이 활성화 됩니다으로 생성 된만 **obj/Debug/android/AndroidManifest.xml** 되며 없습니다 사용 하도록 설정 되었다고 표시는 **필요한 권한** 설정 합니다. 

예를 들어에서 생성 된 매니페스트 파일을 검토 하면 **obj/Debug/android/AndroidManifest.xml**, 다음 권한을 요소를 추가 했습니다. 표시 될 수 있습니다. 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

릴리스에서 빌드 매니페스트 버전 (에서 **obj/Debug/android/AndroidManifest.xml**), 이러한 사용 권한은 *하지* 자동으로 구성 된 합니다. 앱이 디버그 빌드에 사용할 수 있는 사용 권한을 손실 하면 릴리스 빌드로 전환를 찾을 경우 확인에서이 사용 권한을 명시적으로 설정 하는 **필요한 권한** (참조 응용프로그램에대한설정 **빌드 > Android 응용 프로그램** Mac;에 대 한 Visual Studio에서 참조 **속성 > Android 매니페스트** Visual Studio에서). 




## <a name="advanced-features"></a>고급 기능


### <a name="intent-actions-and-features"></a>의도 된 동작 및 기능

Android 매니페스트에서 작업의 기능을 설명 하는 방법을 제공 합니다. 통해 이렇게 [의도](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) 및 [ `[IntentFilter]` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) 사용자 지정 특성입니다. 와 작업에 대 한 적절 한 작업을 지정할 수는 [ `IntentFilter` ](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) 생성자 및 있는 범주와 적절 한는 [ `Categories` ](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) 속성입니다. 하나 이상의 활동 (이 활동의 생성자에 제공 되는 이유)를 제공 합니다. `[IntentFilter]` 별도 각 사용으로 인해 여러 번 제공 될 수 있습니다 `<intent-filter/>` 내의 요소는 `<activity/>`합니다. 예:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

이 예에서는 다음 xml 조각은 생성합니다.

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

Android 매니페스트에서 또한 전체 응용 프로그램에 대 한 속성을 선언할 수 있는 방법을 제공 합니다. 통해 이렇게는 `<application>` 요소 및 해당 정수는 [응용 프로그램](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) 사용자 지정 특성입니다. 이 활동 당 설정 하지 않고 응용 프로그램 수준의 (어셈블리 수준) 설정을 note 합니다. 일반적으로, 선언 `<application>` 전체 응용 프로그램에 대 한 속성 한 후 재정의 이러한 설정이 필요에 따라 활동 당 기준입니다. 

예를 들어, 다음 `Application` 특성이 추가 **AssemblyInfo.cs** 응용 프로그램 디버깅이 될 수 있음, 해당 사용자가 읽을 수 있는 이름이 인지 나타내기 위해 **My App**는 사용`Theme.Light` 모든 활동에 대 한 기본 테마로 스타일: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

이 선언 하면 다음 XML 조각에서 생성 될 **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
이 예제에서는 앱에서 모든 작업은 기본적으로 `Theme.Light` 스타일입니다. 활동의 테마를 설정 하면 `Theme.Dialog`, 활동은 사용 하는 `Theme.Dialog` 스타일 앱의 다른 모든 작업은 기본적으로 하는 동안는 `Theme.Light` 스타일에 설정 된 대로 `<application>` 요소입니다. 

`Application` 요소를 구성 하는 유일한 방법은 않습니다 `<application>` 특성입니다. 또는 특성에 직접 삽입할 수 있습니다는 `<application>` 요소의 **Properties/AndroidManifest.xml**합니다. 이러한 설정은 최종에 병합 됩니다 `<application>` 에 상주 하는 요소 **obj/Debug/android/AndroidManifest.xml**합니다. 내용을 **Properties/AndroidManifest.xml** 항상 사용자 지정 특성에서 제공 하는 데이터를 무시 합니다. 

구성할 수 있는 많은 응용 프로그램 수준 특성이 없는 `<application>` 요소로 이러한 설정에 대 한 자세한 내용은 참조는 [공용 속성](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) 섹션 [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>사용자 지정 특성의 목록

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : 생성 된 [/manifest/application/activity](http://developer.android.com/guide/topics/manifest/activity-element.html) XML 조각 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : 생성 된 [/매니페스트/응용 프로그램](http://developer.android.com/guide/topics/manifest/application-element.html) XML 조각 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : 생성 된 [/매니페스트/계측](http://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML 조각 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : 생성 된 [//intent-filter](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML 조각 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : 생성 된 [//meta-data](http://developer.android.com/guide/topics/manifest/meta-data-element.html) XML 조각 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : 생성 된 [//permission](http://developer.android.com/guide/topics/manifest/permission-element.html) XML 조각 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : 생성 된 [//permission-group](http://developer.android.com/guide/topics/manifest/permission-group-element.html) XML 조각 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : 생성 된 [//permission-tree](http://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML 조각 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : 생성 된 [/manifest/application/service](http://developer.android.com/guide/topics/manifest/service-element.html) XML 조각 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : 생성 된 [/manifest/application/uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) XML 조각 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : 생성 된 [/manifest/uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML 조각 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : 생성 된 [/manifest/application/receiver](http://developer.android.com/guide/topics/manifest/receiver-element.html) XML 조각 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : 생성 된 [/manifest/application/provider](http://developer.android.com/guide/topics/manifest/provider-element.html) XML 조각 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : 생성 된 [/manifest/application/provider/grant-uri-permission](http://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML 조각

