---
title: ProGuard
description: ProGuard는 Java 클래스 파일 축소, 최적화, 난독화 및 사전 검증 도구입니다. 사용되지 않는 코드를 검색 및 제거하고, 바이트 코드를 분석 및 최적화한 후 클래스와 클래스 멤버를 난독 처리합니다. 이 가이드에서는 ProGuard가 작동하는 방법, ProGuard를 프로젝트에서 사용하는 방법과 구성하는 방법을 설명합니다. 또한 ProGuard 구성의 몇 가지 예를 제공합니다.
ms.prod: xamarin
ms.assetid: 29C0E850-3A49-4618-9078-D59BE0284D5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: e65c78633ae91318bd8e9cce949bac9cc12675c0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771446"
---
# <a name="proguard"></a>ProGuard

_ProGuard는 Java 클래스 파일 축소, 최적화, 난독화 및 사전 검증 도구입니다. 사용되지 않는 코드를 검색 및 제거하고, 바이트 코드를 분석 및 최적화한 후 클래스와 클래스 멤버를 난독 처리합니다. 이 가이드에서는 ProGuard가 작동하는 방법, ProGuard를 프로젝트에서 사용하는 방법과 구성하는 방법을 설명합니다. 또한 ProGuard 구성의 몇 가지 예를 제공합니다._


## <a name="overview"></a>개요

ProGuard는 패키지 응용 프로그램에서 사용되지 않는 클래스, 필드, 메서드 및 특성을 검색하고 제거합니다. 참조된 라이브러리에 대해서도 동일한 작업을 수행할 수 있습니다(64k 참조 제한에 도달하지 않도록 하는 데 도움이 됨). 또한 Android SDK의 ProGuard 도구는 바이트 코드를 최적화하고, 사용되지 않는 코드 명령을 제거하고, 나머지 클래스, 필드 및 메서드를 더 짧은 이름으로 난독 처리합니다. ProGuard는 **입력 jar**를 읽은 후 축소, 최적화, 난독 처리, 사전 검증한 후 하나 이상의**출력 jar**에 결과를 기록합니다. 

ProGuard는 다음 단계를 통해 입력 APK를 처리합니다. 

1.  **축소 단계** &ndash; ProGuard가 사용할 클래스 및 클래스 멤버를 재귀적으로 결정합니다. 다른 모든 클래스와 클래스 멤버는 삭제됩니다. 

2.  **최적화 단계** &ndash; ProGuard가 코드를 추가로 최적화합니다. 
    여러 가지 최적화 중에서도 진입점이 아닌 클래스 및 메서드를 비공개, 정적 또는 최종으로 만들 수 있고, 사용하지 않는 매개 변수를 제거할 수 있으며, 일부 메서드를 인라인할 수 있습니다. 

3.  **난독 처리 단계** &ndash; ProGuard가 진입점이 아닌 클래스 및 클래스 멤버의 이름을 바꿉니다. 진입점을 그대로 유지하면 원래 이름으로 여전히 액세스할 수 있습니다. 

4.  **사전 검증 단계** &ndash; 런타임 전에 Java 바이트 코드에 대한 검사를 수행하고 Java VM을 위해 클래스 파일에 주석을 추가합니다. 이는 진입점을 알 필요가 없는 유일한 단계입니다. 

이러한 각 단계는 *선택 사항*입니다. 다음 섹션에서 설명하겠지만, Xamarin.Android ProGuard는 이 단계 중 일부만 사용합니다. 



## <a name="proguard-in-xamarinandroid"></a>Xamarin.Android의 ProGuard

Xamarin.Android ProGuard 구성은 APK를 난독 처리하지 않습니다. 사실, ProGuard를 통한 난독 처리를 활성화하는 것은 불가능합니다(사용자 지정 구성 파일을 사용해도 불가능). 따라서 Xamarin.Android의 ProGuard는 **축소** 및 **최적화** 단계만 수행합니다. 

[![축소 및 최적화 단계](proguard-images/01-xa-chain-sml.png)](proguard-images/01-xa-chain.png#lightbox)

ProGuard를 사용하기 전에 미리 알아두어야 할 중요한 항목은 `Xamarin.Android` 빌드 프로세스 내에서의 작동 방식입니다. 이 프로세스는 두 가지 단계를 사용합니다. 

1.  Xamarin Android 링커

2.  ProGuard

이러한 각 단계는 다음에 설명합니다.



### <a name="linker-step"></a>링커 단계

Xamarin.Android 링커는 응용 프로그램의 정적 분석을 적용하여 다음을 확인합니다. 

-   실제로 사용되는 어셈블리.

-   실제로 사용되는 유형.

-   실제로 사용되는 멤버. 

링커는 항상 ProGuard 단계 전에 실행됩니다. 따라서 링커는 ProGuard가 실행되는 어셈블리/유형/멤버를 스트라이프할 수 있습니다. (Xamarin.Android에서 연결하는 방법에 대한 자세한 내용은 [Android에서 연결](~/android/deploy-test/linker.md)을 참조하세요.)



### <a name="proguard-step"></a>ProGuard 단계

링커 단계가 성공적으로 완료되면 ProGuard가 실행되어 사용하지 않는 Java 바이트 코드를 제거합니다. 이는 APK를 최적화하는 단계입니다. 



## <a name="using-proguard"></a>ProGuard 사용

앱 프로젝트에서 ProGuard를 사용하려면 먼저 ProGuard를 활성화해야 합니다. 다음으로, Xamarin.Android 빌드 프로세스가 기본 ProGuard 구성 파일을 사용하도록 하거나, ProGuard에서 사용할 사용자 지정 구성 파일을 직접 만들 수 있습니다. 



### <a name="enabling-proguard"></a>ProGuard 활성화

앱 프로젝트에서 ProGuard를 활성화하려면 다음 단계를 수행합니다.

1.  프로젝트가 **릴리스** 구성으로 설정되어 있는지 확인합니다(ProGuard가 실행되려면 링커가 실행되어야 하므로 중요함). 

    [![릴리스 구성 선택](proguard-images/02-set-release-sml.png)](proguard-images/02-set-release.png#lightbox)
   
2.  **속성 > Android 옵션**의 **패키징** 탭 아래에서 **ProGuard 사용** 옵션을 선택하여 ProGuard를 활성화합니다. 

    [![Proguard 사용 옵션 선택됨](proguard-images/03-enable-proguard-sml.png)](proguard-images/03-enable-proguard.png#lightbox)

대부분의 Xamarin.Android 앱에서는 Xamarin.Android가 제공하는 기본 ProGuard 구성 파일만으로도 사용되지 않는 코드만 모두 제거할 수 있습니다. 기본 ProGuard 구성을 보려면 **obj\\릴리스\\proguard\\proguard_xamarin.cfg**에서 파일을 엽니다. 다음 섹션에서는 사용자 지정 ProGuard 구성 파일을 만드는 방법을 설명합니다. 



### <a name="customizing-proguard"></a>ProGuard 사용자 지정

필요에 따라 사용자 지정 ProGuard 구성 파일을 추가하여 ProGuard 도구를 더 세부적으로 제어할 수 있습니다. 예를 들어 ProGuard에 어떤 클래스를 유지할지 명시적으로 알려줄 수 있습니다. 이렇게 하려면 **솔루션 탐색기**의 **속성** 창에서 새 **.cfg** 파일을 만들고 `ProGuardConfiguration` 빌드 동작을 적용합니다. 

[![선택한 ProguardConfiguration 빌드 작업](proguard-images/04-build-action-sml.png)](proguard-images/04-build-action.png#lightbox)

이 구성 파일은 Xamarin.Android **proguard_xamarin.cfg** 파일을 대체하지 않습니다. 두 파일은 모두 ProGuard에서 사용되기 때문입니다. 

다음 예제는 일반적인 ProGuard 구성 파일을 보여 줍니다.
    

    # This is Xamarin-specific (and enhanced) configuration.

    -dontobfuscate

    -keep class mono.MonoRuntimeProvider { *; <init>(...); }
    -keep class mono.MonoPackageManager { *; <init>(...); }
    -keep class mono.MonoPackageManager_Resources { *; <init>(...); }
    -keep class mono.android.** { *; <init>(...); }
    -keep class mono.java.** { *; <init>(...); }
    -keep class mono.javax.** { *; <init>(...); }
    -keep class opentk.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk.GameViewBase { *; <init>(...); }
    -keep class opentk_1_0.platform.android.AndroidGameView { *; <init>(...); }
    -keep class opentk_1_0.GameViewBase { *; <init>(...); }

    -keep class android.runtime.** { <init>(***); }
    -keep class assembly_mono_android.android.runtime.** { <init>(***); }
    # hash for android.runtime and assembly_mono_android.android.runtime.
    -keep class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }
    -keepclassmembers class md52ce486a14f4bcd95899665e9d932190b.** { *; <init>(...); }

    # Android's template misses fluent setters...
    -keepclassmembers class * extends android.view.View {
       *** set*(***);
    }

    # also misses those inflated custom layout stuff from xml...
    -keepclassmembers class * extends android.view.View {
       <init>(android.content.Context,android.util.AttributeSet);
       <init>(android.content.Context,android.util.AttributeSet,int);
    }
    

ProGuard가 응용 프로그램을 제대로 분석할 수 없는 경우가 있을 수 있습니다. 이 경우 응용 프로그램에 실제로 필요한 코드를 잠재적으로 제거할 수 있습니다. 그러면 사용자 지정 ProGuard 구성 파일에 `-keep` 줄을 추가할 수 있습니다. 

    -keep public class MyClass

이 예제에서는 ProGuard가 건너뛰도록 할 클래스의 실제 이름으로 `MyClass`가 설정됩니다.

또한 `[Register]` 주석을 사용하여 고유한 이름을 등록할 수 있고, 이러한 이름을 사용하여 ProGuard 규칙을 사용자 지정할 수 있습니다. Adapters, Views, BroadcastReceivers, Services, ContentProviders, Activities 및 Fragments의 이름을 등록할 수 있습니다. `[Register]` 사용자 지정 특성을 사용하는 방법에 대한 자세한 내용은 [JNI 사용](~/android/platform/java-integration/working-with-jni.md)을 참조하세요.


### <a name="proguard-options"></a>ProGuard 옵션

ProGuard는 작동을 세부적으로 제어하도록 구성할 수 있는 다양한 옵션을 제공합니다. [ProGuard Manual](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/introduction.html)은 ProGuard 사용에 대한 완전한 참조 설명서를 제공합니다. 

Xamarin.Android는 다음과 같은 ProGuard 옵션을 지원합니다. 


-    [입력/출력 옵션](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#iooptions)

-    [유지 옵션](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptions)

-    [축소 옵션](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#shrinkingoptions)

-    [일반 옵션](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#generaloptions)

-    [클래스 경로](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classpath)

-    [파일 이름](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filename)

-    [파일 필터](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filefilters)

-    [필터](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#filters)

-    [`Keep` 옵션 개요](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoverview)

-    [옵션 한정자 유지](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#keepoptionmodifiers)

-    [클래스 사양](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#classspecification)

다음 옵션은 Xamarin.Android에 의해 *무시*됩니다.

-    [최적화 옵션](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#optimizationoptions)

-    [난독 처리 옵션](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#obfuscationoptions) 

-    [사전 인증 옵션](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/manual/usage.html#preverificationoptions)



## <a name="proguard-and-android-nougat"></a>ProGuard 및 Android Nougat

Android 7.0 이상에서 ProGuard를 사용하려는 경우 최신 버전의 ProGuard를 다운로드해야 합니다. Android SDK는 JDK 1.8과 호환되는 새 버전을 제공하지 않기 때문입니다.

이 [NuGet 패키지](https://www.nuget.org/packages/name.atsushieno.proguard.facebook/5.3.0)를 사용하여 최신 버전의 `proguard.jar`를 설치할 수 있습니다. 기본 Android SDK `proguard.jar`를 업데이트하는 방법에 대한 자세한 내용은 이 [스택 오버플로](http://stackoverflow.com/questions/39514518/xamarin-android-proguard-unsupported-class-version-number-52-0/39514706#39514706) 설명을 참조하세요.

ProGuard의 모든 버전은 [SourceForge 페이지](https://sourceforge.net/projects/proguard/files/)에서 찾을 수 있습니다. 



## <a name="example-proguard-configurations"></a>ProGuard 구성 예제

ProGuard 구성 파일의 두 예제는 다음과 같습니다. 이 경우에는 Xamarin.Android 빌드 프로세스가 **input**, **output** 및 **library** jar를 제공합니다. 따라서 `-keep` 같은 기타 옵션에 집중할 수 있습니다. 

### <a name="a-simple-android-activity"></a>간단한 Android 활동

다음 예제에서는 간단한 Android 활동에 대한 구성을 보여줍니다.

    -injars  bin/classes
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic

    -keep public class mypackage.MyActivity

### <a name="a-complete-android-application"></a>완전한 Android 응용 프로그램

다음 예제에서는 완전한 Android 앱에 대한 구성을 보여줍니다.

    -injars  bin/classes
    -injars  libs
    -outjars bin/classes-processed.jar
    -libraryjars /usr/local/java/android-sdk/platforms/android-9/android.jar

    -dontpreverify
    -repackageclasses ''
    -allowaccessmodification
    -optimizations !code/simplification/arithmetic
    -keepattributes *Annotation*

    -keep public class * extends android.app.Activity
    -keep public class * extends android.app.Application
    -keep public class * extends android.app.Service
    -keep public class * extends android.content.BroadcastReceiver
    -keep public class * extends android.content.ContentProvider

    -keep public class * extends android.view.View {
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
    public void set*(...);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet);
    }

    -keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet, int);
    }

    -keepclassmembers class * implements android.os.Parcelable {
    static android.os.Parcelable$Creator CREATOR;
    }

    -keepclassmembers class **.R$* {
    public static <fields>;
    }


## <a name="proguard-and-the-xamarinandroid-build-process"></a>ProGuard 및 Xamarin.Android 빌드 프로세스

다음 섹션에서는 Xamarin.Android **릴리스** 빌드 중에 ProGuard가 실행되는 방법을 설명합니다.


### <a name="what-command-is-proguard-running"></a>ProGuard가 실행하는 명령은 무엇인가요?

ProGuard는 단순히 Android SDK와 함께 제공되는 `.jar`입니다. 따라서 명령에서 호출됩니다. 

```shell
java -jar proguard.jar options ...
```

### <a name="the-proguard-task"></a>ProGuard 작업

ProGuard 작업은 **Xamarin.Android.Build.Tasks.dll** 어셈블리 내에서 발견됩니다. `_CompileDex` 대상에 포함된 `_CompileToDalvikWithDx` 대상에 포함되어 있습니다. 

다음 목록에는 **파일 > 새 프로젝트**를 통해 새 프로젝트를 만든 후 생성된 기본 매개 변수의 예가 나와 있습니다. 

    ProGuardJarPath = C:\Android\android-sdk\tools\proguard\lib\proguard.jar
    AndroidSdkDirectory = C:\Android\android-sdk\
    JavaToolPath = C:\Program Files (x86)\Java\jdk1.8.0_92\\bin
    ProGuardToolPath = C:\Android\android-sdk\tools\proguard\
    JavaPlatformJarPath = C:\Android\android-sdk\platforms\android-25\android.jar
    ClassesOutputDirectory = obj\Release\android\bin\classes
    AcwMapFile = obj\Release\acw-map.txt
    ProGuardCommonXamarinConfiguration = obj\Release\proguard\proguard_xamarin.cfg
    ProGuardGeneratedReferenceConfiguration = obj\Release\proguard\proguard_project_references.cfg
    ProGuardGeneratedApplicationConfiguration = obj\Release\proguard\proguard_project_primary.cfg
    ProGuardConfigurationFiles

      {sdk.dir}tools\proguard\proguard-android.txt;
      {intermediate.common.xamarin};
      {intermediate.references};
      {intermediate.application};
      ;
     
    JavaLibrariesToEmbed = C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar
    ProGuardJarInput = obj\Release\proguard\__proguard_input__.jar
    ProGuardJarOutput = obj\Release\proguard\__proguard_output__.jar
    DumpOutput = obj\Release\proguard\dump.txt
    PrintSeedsOutput = obj\Release\proguard\seeds.txt
    PrintUsageOutput = obj\Release\proguard\usage.txt
    PrintMappingOutput = obj\Release\proguard\mapping.txt

다음 예제는 IDE에서 실행되는 일반적인 ProGuard 명령을 보여줍니다.

```cmd
C:\Program Files (x86)\Java\jdk1.8.0_92\\bin\java.exe -jar C:\Android\android-sdk\tools\proguard\lib\proguard.jar -include obj\Release\proguard\proguard_xamarin.cfg -include obj\Release\proguard\proguard_project_references.cfg -include obj\Release\proguard\proguard_project_primary.cfg "-injars 'obj\Release\proguard\__proguard_input__.jar';'C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\MonoAndroid\v7.0\mono.android.jar'" "-libraryjars 'C:\Android\android-sdk\platforms\android-25\android.jar'" -outjars "obj\Release\proguard\__proguard_output__.jar" -optimizations !code/allocation/variable
```

## <a name="troubleshooting"></a>문제 해결

### <a name="file-issues"></a>파일 문제

ProGuard가 구성 파일을 읽을 때 다음과 같은 오류 메시지가 표시될 수 있습니다. 

    Unknown option '-keep' in line 1 of file 'proguard.cfg'

이 문제는 일반적으로 Windows에서 `.cfg` 파일에 잘못된 인코딩이 있는 경우에 발생합니다. ProGuard는 텍스트 파일에 있는 _바이트 순서 표시_(BOM)를 처리할 수 없습니다. BOM이 있는 경우 ProGuard는 위의 오류와 함께 종료됩니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

이 문제를 방지하려면 파일을 BOM 없이 저장할 수 있는 텍스트 편집기에서 사용자 지정 구성 파일을 편집합니다. 이 문제를 해결하려면 텍스트 편집기의 인코딩이 `UTF-8`로 설정되어 있는지 확인합니다. 예를 들어 텍스트 편집기 [Notepad++](https://notepad-plus-plus.org/)는 파일을 저장할 때 **Encoding &gt; Encode in UTF-8 Without BOM**을 선택하면 BOM 없이 파일을 저장할 수 있습니다. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

이 문제를 방지하려면 BOM을 생략할 수 있는 텍스트 편집기에서 사용자 지정 구성 파일을 저장합니다. 

-----


### <a name="other-issues"></a>기타 문제

ProGuard [문제 해결](https://stuff.mit.edu/afs/sipb/project/android/sdk/android-sdk-linux/tools/proguard/docs/index.html#manual/troubleshooting.html) 페이지에는 ProGuard 사용 시 발생할 수 있는 일반적인 문제 및 해결책이 설명되어 있습니다.


## <a name="summary"></a>요약

이 가이드에서는 Xamarin.Android에서 ProGuard가 작동하는 방법, ProGuard를 앱 프로젝트에서 사용하는 방법과 구성하는 방법을 설명합니다. 예제 ProGuard 구성을 제공하고 일반적인 문제에 대한 해결책을 설명합니다. ProGuard 도구 및 Android에 대한 자세한 내용은 [코드 축소 및 리소스](http://developer.android.com/tools/help/proguard.html)를 참조하세요. 


## <a name="related-links"></a>관련 링크

- [릴리스할 응용 프로그램 준비](~/android/deploy-test/release-prep/index.md)
