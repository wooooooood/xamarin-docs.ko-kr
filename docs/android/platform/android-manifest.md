---
title: Android 매니페스트 사용
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 5e354f8271257ab21a855bdf5d576ce3062fadc7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60957395"
---
# <a name="working-with-the-android-manifest"></a>Android 매니페스트 사용


## <a name="overview"></a>개요

**AndroidManifest.xml** Android 플랫폼의 기능 및 Android 응용 프로그램의 요구 사항을 설명 하는 데 허용 하는 강력한 파일입니다. 그러나 사용 하 여 작업은 쉽지 않습니다. Xamarin.Android는 사용자 지정 특성 수에 대 한 매니페스트를 자동으로 생성 한 다음 사용할 클래스를 추가할 수 있도록 하 여이 문제를 최소화할 수 있습니다. 우리의 목표는 사용자의 99%를 수동으로 수정 해야 하지 **AndroidManifest.xml**합니다. 

**AndroidManifest.xml** 내에 있는 XML 및 빌드 프로세스의 일부로 생성 됩니다 **Properties/AndroidManifest.xml** 사용자 지정 특성에서 생성 되는 XML과 병합 됩니다. 병합 결과 **AndroidManifest.xml** 에 상주 합니다 **obj** ; 하위 디렉터리에 상주 하는 예를 들어 **obj/Debug/android/AndroidManifest.xml** 디버그 빌드에 대 한 . 병합 프로세스는 간단: 코드 내에서 사용자 지정 특성을 사용 하 여 XML 요소를 생성 하 고 *삽입* 에 해당 요소 **AndroidManifest.xml**합니다. 



## <a name="the-basics"></a>기본 사항

컴파일 타임에 어셈블리에 대 한 검색 이외`abstract` 에서 파생 된 클래스 [활동](https://developer.xamarin.com/api/type/Android.App.Activity/) 있고를 [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) 에 선언 된 특성입니다. 이를 사용 하 여 이러한 클래스 및 특성 매니페스트를 빌드합니다. 예를 들어, 다음 코드를 고려하세요. 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

이 인해에서 생성 되 고 아무 **AndroidManifest.xml**합니다. 하려는 경우는 `<activity/>` 생성 되는 요소를 사용 해야 합니다 [`[Activity]`](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) 
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

이 예제에서는 다음 xml 조각에 추가할 **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

합니다 `[Activity]` 특성을 주지는 `abstract` ; 형식 `abstract` 형식은 무시 됩니다.



### <a name="activity-name"></a>작업 이름

Xamarin.Android 5.1부터 활동의 형식 이름을 더해서 내보내는 형식의 어셈블리 정규화 된 이름의 MD5SUM입니다. 따라서 동일한 정규화 된 이름을 두 명의 다른 어셈블리에서 제공 하는 패키징 오류가 발생 하지을 수 있습니다. (Xamarin.Android 5.1 하기 전에 작업의 기본 형식 이름에서 만들었는지 소문자 네임 스페이스 및 클래스 이름입니다.) 

이 기본값을 재정의 하 고 사용 하 여 작업의 이름을 명시적으로 지정 하려는 경우는 [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) 속성: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

이 예제의 다음 xml 조각은 다음과 같습니다.

```xml
<activity android:name="awesome.demo.activity" />
```

*참고*: 기능을 사용할지는 `Name` 속성 이름 바꾸기와 같은 이전 버전과 호환성 이유로 대해서만 런타임 시 형식 조회 속도가 느려질 수 있습니다. 소문자 네임 스페이스를 기반으로 하는 작업의 기본 형식 이름 및 클래스 이름에 참조 필요로 하는 기존 코드가 있다면 [Android 호출 가능 래퍼 명명](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) 호환성을 유지 관리에 대 한 팁입니다. 


### <a name="activity-title-bar"></a>작업 제목 표시줄

기본적으로 Android 응용 프로그램에 부여 제목 표시줄을 실행 하는 경우. 이 사용 되는 값이 [ `/manifest/application/activity/@android:label` ](https://developer.android.com/guide/topics/manifest/activity-element.html#label)합니다. 대부분의 경우에서이 값은 클래스 이름에서 달라 집니다. 제목 표시줄에서 앱의 레이블을 지정 하려면 사용 합니다 [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) 속성입니다.
예를 들어: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

이 예제의 다음 xml 조각은 다음과 같습니다.

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>응용 프로그램 선택에서 시작할

기본적으로 작업 Android의 응용 프로그램 시작 관리자 화면에 표시 되지 않습니다. 이 해야 하는 경우가 많은 활동이 응용 프로그램에서 모든 항목에 대 한 아이콘을 원하지 때문입니다. 어느 응용 프로그램 시작 관리자에서 시작할 해야를 지정 하려면 사용 합니다 [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) 속성입니다. 예를 들어: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

이 예제의 다음 xml 조각은 다음과 같습니다.

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

기본적으로 작업 시스템에서 제공 된 기본 시작 관리자 아이콘을 제공 됩니다. 사용자 지정 아이콘을 사용 하려면 먼저 추가 프로그램 **.png** 하 **리소스/drawable**, 해당 빌드 작업을 설정 **AndroidResource**를 사용 하 여는 [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) 속성을 통해 사용할 아이콘을 지정 합니다. 예를 들어: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

이 예제의 다음 xml 조각은 다음과 같습니다.

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

Android 매니페스트 사용 권한을 추가 하면 (에 설명 된 대로 [Android 매니페스트 권한 추가](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)), 이러한 사용 권한을 기록 됩니다 **Properties/AndroidManifest.xml**합니다. 예를 들어, 설정 하는 경우는 `INTERNET` 권한, 다음 요소에 추가 됩니다 **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

디버그 빌드에 자동으로 쉽게 디버깅할 수 있도록 일부 권한을 설정 (같은 `INTERNET` 하 고 `READ_EXTERNAL_STORAGE`) &ndash; 이 설정이 활성화 됩니다 생성 된 에서만 **obj/Debug/android/AndroidManifest.xml** 없는 사용 하도록 설정 하는 것으로 표시 합니다 **필요한 권한** 설정 합니다. 

예를 들어에서 생성된 된 매니페스트 파일을 살펴보면 **obj/Debug/android/AndroidManifest.xml**, 다음 추가 권한 요소 표시 될 수 있습니다. 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

릴리스에서 매니페스트 버전을 빌드 (에서 **obj/Debug/android/AndroidManifest.xml**), 이러한 권한은 *하지* 자동으로 구성 합니다. 릴리스 빌드로 전환 이면 디버그 빌드에 사용할 수 있는 권한을 잃게 앱을 발견할 경우이 사용 권한을 명시적으로 설정 했는지 확인 합니다 **필요한 권한** (참조 앱에대한설정 **빌드 > Android 응용 프로그램** ; Mac 용 Visual Studio에 표시 **속성 > Android 매니페스트** Visual Studio에서). 




## <a name="advanced-features"></a>고급 기능


### <a name="intent-actions-and-features"></a>의도 한 동작 및 기능

Android 매니페스트에서 작업의 기능을 설명 하는 방법을 제공 합니다. 이 통해 이루어집니다 [의도](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) 및 [`[IntentFilter]`](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) 
사용자 지정 특성입니다. 작업을 사용 하 여 작업에 대 한 적절 한를 지정할 수는 [`IntentFilter`](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) 
생성자 및 범주와 적합 합니다 [`Categories`](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) 
속성. 하나 이상의 활동 (이 활동의 생성자에서 제공 되는 이유)를 제공 합니다. `[IntentFilter]` 여러 번 사용 하 여 각 별도의 결과 제공 될 수 있습니다 `<intent-filter/>` 내의 요소는 `<activity/>`합니다. 예를 들어:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

이 예제의 다음 xml 조각은 다음과 같습니다.

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


### <a name="application-element"></a>응용 프로그램 요소

Android 매니페스트는 또한 전체 응용 프로그램에 대 한 속성을 선언할 수 있는 방법을 제공 합니다. 통해 수행 됩니다는 `<application>` 요소와이 대응 합니다 [응용 프로그램](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) 사용자 지정 특성입니다. 작업별 설정 하지 않고 응용 프로그램 수준 (어셈블리 수준) 설정을 이들은 note 합니다. 선언 하는 일반적으로 `<application>` 전체 응용 프로그램에 대 한 속성 및 그런 다음 이러한 설정을 재정의할 필요에 따라 작업별 기준입니다. 

예를 들어, 다음 `Application` 특성이 추가 되었습니다 **AssemblyInfo.cs** 응용 프로그램 디버깅 수, 해당 사용자를 읽을 수 있는 이름 인지를 나타내는 **My App**, 합니다 를사용하여`Theme.Light` 모든 작업의 기본 테마 스타일: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

이 선언 하면 생성 되 게 하는 다음 XML 조각은 **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
이 예제에서는 앱에서 모든 활동은 기본적으로 `Theme.Light` 스타일입니다. 활동의 테마를 설정 하면 `Theme.Dialog`는 활동에 사용 됩니다 합니다 `Theme.Dialog` 스타일 앱에서 다른 작업이 모두 기본적으로 하는 동안를 `Theme.Light` 스타일에 설정 된 대로 `<application>` 요소. 

합니다 `Application` 요소를 구성 하는 유일한 방법은 아닙니다. `<application>` 특성입니다. 또는 특성에 직접 삽입할 수 있습니다 합니다 `<application>` 요소의 **Properties/AndroidManifest.xml**합니다. 마지막으로 이러한 설정은 병합 됩니다 `<application>` 요소에 있는 **obj/Debug/android/AndroidManifest.xml**합니다. 내용의 **Properties/AndroidManifest.xml** 항상 사용자 지정 특성에 의해 제공 되는 데이터를 재정의 합니다. 

구성할 수 있는 많은 응용 프로그램 수준 특성을 가지는 `<application>` 요소로 이러한 설정에 대 한 자세한 내용은 참조 하세요.를 [공용 속성](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) 섹션 [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>사용자 지정 특성의 목록

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : 생성 된 [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html) XML 조각 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : 생성 된 [/매니페스트 및 응용 프로그램](https://developer.android.com/guide/topics/manifest/application-element.html) XML 조각 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : 생성 된 [/매니페스트/계측](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML 조각 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : 생성 된 [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML 조각 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : 생성 된 [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html) XML 조각 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : 생성 된 [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html) XML 조각 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : 생성 된 [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html) XML 조각 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : 생성 된 [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML 조각 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : 생성 된 [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html) XML 조각 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : 생성 된 [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html) XML 조각 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : 생성 된 [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML 조각 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : 생성 된 [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html) XML 조각 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : 생성 된 [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html) XML 조각 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : 생성 된 [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML 조각

