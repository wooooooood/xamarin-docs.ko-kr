---
title: Android 매니페스트 작업
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
ms.openlocfilehash: 1438c012608b367c21ebcc401c058b186b917f53
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027797"
---
# <a name="working-with-the-android-manifest"></a>Android 매니페스트 작업

**AndroidManifest.xml**은 Android 플랫폼에서 Android에 대한 애플리케이션의 기능 및 요구 사항을 기술할 수 있게 해주는 강력한 파일입니다. 하지만 이를 사용한 작업은 쉽지 않습니다. Xamarin.Android는 매니페스트 자동 생성을 위해 사용되는 사용자 지정 특성을 클래스에 추가하여 이러한 어려움을 최소화하는 데 도움을 줍니다. 목표는 사용자의 99%가 **AndroidManifest.xml**을 수동으로 수정할 필요가 없도록 하는 것입니다. 

**AndroidManifest.xml**은 빌드 프로세스 중에 생성되며, **Properties/AndroidManifest.xml** 내에서 발견된 XML은 사용자 지정 특성으로부터 생성된 XML과 병합됩니다. 그 결과로 병합된 **AndroidManifest.xml**은 **obj** 하위 디렉터리에 저장됩니다. 예를 들어 디버그 빌드의 경우에는 **obj/Debug/android/AndroidManifest.xml**에 저장됩니다. 병합 프로세스는 간단합니다. 코드 내에서 사용자 지정 특성을 사용하여 XML 요소를 생성하고 이 요소를 **AndroidManifest.xml**에 *삽입*합니다. 

## <a name="the-basics"></a>기본 사항

컴파일 시간에 어셈블리에서 [작업](xref:Android.App.Activity)으로부터 파생되고 여기에 [`[Activity]`](xref:Android.App.ActivityAttribute) 특성이 선언된 비`abstract` 클래스를 스캔합니다. 그런 후 이러한 클래스 및 특성을 사용해서 매니페스트를 빌드합니다. 예를 들어, 다음 코드를 고려하세요. 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

그러면 **AndroidManifest.xml**에 아무 것도 생성되지 않습니다. `<activity/>` 요소를 생성하려면 [`[Activity]`](xref:Android.App.Activity) 
사용자 지정 특성을 사용해야 합니다. 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

이 예에서는 다음 xml 조각이 **AndroidManifest.xml**에 추가됩니다.

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` 특성은 `abstract` 형식에 영향을 주지 않으며, `abstract` 형식은 무시됩니다.

### <a name="activity-name"></a>작업 이름

Xamarin.Android 5.1부터 작업의 형식 이름은 내보내려는 형식의 정규화된 어셈블리 이름인 MD5SUM을 기준으로 합니다. 이렇게 하면 서로 다른 두 개의 어셈블리에서 동일한 정규화된 이름을 제공할 수 있으며, 패키징 오류가 발생하지 않습니다. (Xamarin.Android 5.1 이전에는 작업의 기본 형식 이름이 소문자 네임스페이스 및 클래스 이름으로부터 생성되었습니다.) 

이 기본값을 재정의하고 작업 이름을 명시적으로 지정하려면 [`Name`](xref:Android.App.ActivityAttribute.Name) 속성을 사용하십시오. 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

이 예제는 다음 xml 조각을 생성합니다.

```xml
<activity android:name="awesome.demo.activity" />
```

> [!NOTE]
> 이렇게 이름을 바꾸면 런타임에 형식 조회 속도가 느려질 수 있기 때문에 이전 버전과의 호환성을 위해서만 `Name` 속성을 사용해야 합니다. 작업의 기본 형식 이름이 소문자 네임스페이스 및 클래스 이름 기반인 것으로 예상되는 레거시 코드가 있을 때, 호환성 유지 관리에 대한 팁은 [Android 호출 가능 래퍼 이름 지정](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming)을 참조하십시오. 

### <a name="activity-title-bar"></a>작업 제목 표시줄

기본적으로 Android는 애플리케이션이 실행될 때 애플리케이션에 제목 표시줄을 제공합니다. 여기에 사용되는 값은 [`/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label)입니다. 대부분의 경우 이 값은 클래스 이름과 다릅니다. 제목 표시줄에 앱 레이블을 지정하려면 [`Label`](xref:Android.App.ActivityAttribute.Label) 속성을 사용하십시오.
예를 들어: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

이 예제는 다음 xml 조각을 생성합니다.

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

### <a name="launchable-from-application-chooser"></a>애플리케이션 선택기에서 시작 가능

기본적으로 작업은 Android의 애플리케이션 시작 프로그램 화면에 표시되지 않습니다. 왜냐하면 애플리케이션에서 수행되는 작업이 많을 가능성이 높은데 이것들에 대해 모두 하나씩 아이콘을 만들지는 않기 때문입니다. 애플리케이션 시작 프로그램에서 시작할 수 있는 항목을 지정하려면 [`MainLauncher`](xref:Android.App.ActivityAttribute.MainLauncher) 속성을 사용하십시오. 예를 들어: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

이 예제는 다음 xml 조각을 생성합니다.

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

### <a name="activity-icon"></a>작업 아이콘

기본적으로 작업에는 시스템에서 제공된 기본 시작 프로그램 아이콘이 부여됩니다. 사용자 지정 아이콘을 사용하려면 먼저 **.png**를 **리소스/드로어블**에 추가하고 빌드 작업을 **AndroidResource**로 설정한 후 [`Icon`](xref:Android.App.ActivityAttribute.Icon) 속성을 사용하여 사용할 아이콘을 지정합니다. 예를 들어: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

이 예제는 다음 xml 조각을 생성합니다.

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

Android 매니페스트에 권한을 추가할 때([Android 매니페스트에 권한 추가](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest) 참조), 이러한 권한은 **Properties/AndroidManifest.xml**에 기록됩니다. 예를 들어 `INTERNET` 권한을 설정하면 다음 요소가 **Properties/AndroidManifest.xml**에 추가됩니다. 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

디버그 빌드는 디버그를 더 쉽게 수행하기 위해 일부 권한을 자동으로 설정합니다(예: `INTERNET` 및 `READ_EXTERNAL_STORAGE`). &ndash; 이러한 설정은 생성된 **obj/Debug/android/AndroidManifest.xml**에만 설정되고, **필요한 권한** 설정에 사용하도록 표시되지 않습니다. 

예를 들어 **obj/Debug/android/AndroidManifest.xml**에서 생성된 매니페스트 파일을 검사할 경우 다음과 같이 추가된 권한 요소가 표시될 수 있습니다. 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

매니페스트의 릴리스 빌드 버전(**obj/Debug/android/AndroidManifest.xml**)에서 이러한 권한은 자동으로 구성되지 *않습니다*. 릴리스 빌드로 전환하여 디버그 빌드에서 사용 가능했던 권한이 앱에서 손실되는 것으로 확인되면, 앱의 **필요한 권한** 설정에서 이 권한을 명시적으로 설정했는지 확인합니다(Mac용 Visual Studio의 **빌드 > Android 애플리케이션** 또는 Visual Studio의 **속성 > Android 매니페스트** 참조). 

## <a name="advanced-features"></a>고급 기능

### <a name="intent-actions-and-features"></a>의도된 작업 및 기능

Android 매니페스트는 작업 기능을 설명할 수 있는 방법을 제공합니다. 이 작업은 [의도](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) 및 [`[IntentFilter]`](xref:Android.App.IntentFilterAttribute)
사용자 지정 특성을 통해 수행됩니다. [`IntentFilter`](xref:Android.App.IntentFilterAttribute#ctor*)
생성자로 작업에 적합한 조치와 [`Categories`](xref:Android.App.IntentFilterAttribute.Categories)
속성. 최소한 하나 이상의 작업을 제공해야 합니다(작업이 생성자에 제공되는 이유). `[IntentFilter]`는 여러 번 제공될 수 있으며, 사용할 때마다 `<activity/>` 내에 `<intent-filter/>` 요소가 개별적으로 생성됩니다. 예를 들어:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

이 예제는 다음 xml 조각을 생성합니다.

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

Android 매니페스트는 또한 전체 애플리케이션에 대해 속성을 선언할 수 있는 방법을 제공합니다. 이 작업은 `<application>` 요소 및 상대 요소인 [Application](xref:Android.App.ApplicationAttribute) 사용자 지정 특성을 통해 수행됩니다. 이러한 설정은 작업별 설정이 아닌 애플리케이션 전체(어셈블리 전체)에 적용되는 설정입니다. 일반적으로 전체 애플리케이션에 대해 `<application>` 속성을 선언하고 작업별 기준으로 이러한 설정을 (필요에 따라) 재정의합니다. 

예를 들어 다음 `Application` 특성이 **AssemblyInfo.cs**에 추가되어 애플리케이션을 디버그할 수 있고, 사용자가 읽을 수 있는 이름이 **My App**이며, 모든 작업의 기본 테마로 `Theme.Light` 스타일이 사용됨을 나타냅니다. 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

이 선언은 다음 XML 조각을 **obj/Debug/android/AndroidManifest.xml**에 생성합니다.

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```

이 예에서 앱의 모든 작업은 기본적으로 `Theme.Light` 스타일로 지정됩니다. 작업 테마를 `Theme.Dialog`로 설정하면 해당 작업에만 `Theme.Dialog` 스타일이 사용되고, 앱에 있는 다른 모든 작업에는 기본적으로 `<application>` 요소에 설정된 대로 `Theme.Light` 스타일이 적용됩니다. 

`Application` 요소는 `<application>` 특성을 구성하기 위한 유일한 방법이 아닙니다. 또한 특성을 **Properties/AndroidManifest.xml**의 `<application>` 요소에 직접 삽입할 수 있습니다. 이러한 설정은 **obj/Debug/android/AndroidManifest.xml**에 있는 최종 `<application>` 요소에 병합됩니다. **Properties/AndroidManifest.xml**의 내용은 항상 사용자 지정 특성으로 제공되는 데이터를 재정의합니다. 

`<application>` 요소에는 여러 가지 애플리케이션 전체 특성을 구성할 수 있습니다. 이러한 설정들에 대한 자세한 내용은 [ApplicationAttribute](xref:Android.App.ApplicationAttribute)의 [공용 속성](xref:Android.App.ApplicationAttribute)을 참조하십시오. 

## <a name="list-of-custom-attributes"></a>사용자 지정 특성 목록

- [Android.App.ActivityAttribute](xref:Android.App.ActivityAttribute): [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html) XML 조각을 생성합니다. 
- [Android.App.ApplicationAttribute](xref:Android.App.ApplicationAttribute): [/manifest/application](https://developer.android.com/guide/topics/manifest/application-element.html) XML 조각을 생성합니다. 
- [Android.App.InstrumentationAttribute](xref:Android.App.InstrumentationAttribute): [/manifest/instrumentation](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML 조각을 생성합니다. 
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute): [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML 조각을 생성합니다. 
- [Android.App.MetaDataAttribute](xref:Android.App.MetaDataAttribute): [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html) XML 조각을 생성합니다. 
- [Android.App.PermissionAttribute](xref:Android.App.PermissionAttribute): [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html) XML 조각을 생성합니다. 
- [Android.App.PermissionGroupAttribute](xref:Android.App.PermissionGroupAttribute): [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html) XML 조각을 생성합니다. 
- [Android.App.PermissionTreeAttribute](xref:Android.App.PermissionTreeAttribute): [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML 조각을 생성합니다. 
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute): [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html) XML 조각을 생성합니다. 
- [Android.App.UsesLibraryAttribute](xref:Android.App.UsesLibraryAttribute): [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html) XML 조각을 생성합니다. 
- [Android.App.UsesPermissionAttribute](xref:Android.App.UsesPermissionAttribute): [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML 조각을 생성합니다. 
- [Android.Content.BroadcastReceiverAttribute](xref:Android.Content.BroadcastReceiverAttribute): [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html) XML 조각을 생성합니다. 
- [Android.Content.ContentProviderAttribute](xref:Android.Content.ContentProviderAttribute): [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html) XML 조각을 생성합니다. 
- [Android.Content.GrantUriPermissionAttribute](xref:Android.Content.GrantUriPermissionAttribute): [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML 조각을 생성합니다.
