---
title: 빌드 프로세스
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/14/2018
ms.openlocfilehash: 806ed841ec4db037a063bb458e1eed13226e08bd
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="build-process"></a>빌드 프로세스


## <a name="overview"></a>개요

Xamarin.Android 빌드 프로세스는 [`Resource.designer.cs`를 생성하고](~/android/internals/api-design.md), `AndroidAsset`, `AndroidResource` 및 다른 [빌드 동작](#Build_Actions)을 지원하고, [Android에서 호출 가능한 래퍼](~/android/platform/java-integration/android-callable-wrappers.md)를 생성하고, Android 장치에서 실행할 `.apk`를 생성하는 등 모든 것을 결합하는 역할을 합니다.


## <a name="application-packages"></a>응용 프로그램 패키지

Xamarin.Android 빌드 시스템이 생성할 수 있는 Android 응용 프로그램 패키지(`.apk` 파일)의 종류는 크게 두 가지가 있습니다. 

-   완전히 자체 포함되어 있으며 실행할 추가 패키지가 필요 없는 **릴리스** 빌드. 앱 스토어에 제공되는 패키지입니다. 

-   이와 반대되는 **디버그** 빌드. 

이는 우연치 않게 패키지를 생성하는 MSBuild `Configuration`과 일치합니다.

### <a name="shared-runtime"></a>공유 런타임

*공유 런타임*은 기본 클래스 라이브러리(`mscorlib.dll` 등) 및 Android 바인딩 라이브러리(`Mono.Android.dll` 등)를 제공하는 추가적인 Android 패키지의 쌍입니다. 디버그 빌드는 Android 응용 프로그램 패키지 내 기본 클래스 라이브러리 및 바인딩 어셈블리를 포함하는 대신 공유 런타임에 의존하므로 디버그 패키지 크기가 줄어듭니다.

공유 런타임은 디버그 빌드에서 `$(AndroidUseSharedRuntime)` 속성을 `False`로 설정하여 비활성화할 수 있습니다. 

<a name="Fast_Deployment" />

### <a name="fast-deployment"></a>빠른 배포

*빠른 배포*는 공유 런타임과 함께 작동하여 Android 응용 프로그램의 패키지 크기를 더욱 줄입니다. 이를 위해 패키지 내에 앱의 어셈블리를 묶지 않습니다. 대신 `adb push`를 통해 대상에 복사합니다. 어셈블리가 변경될 *경우에만* 패키지가 다시 설치되지 않으므로 빌드/배포/디버그 주기가 단축됩니다. 대신 업데이트된 어셈블리만 대상 장치에 다시 동기화합니다. 

빠른 배포는 `adb`가 `/data/data/@PACKAGE_NAME@/files/.__override__` 디렉터리에 동기화되지 않게 차단하는 장치에서 실패하는 것으로 알려져 있습니다. 

빠른 배포는 기본적으로 활성화되며, 디버그 빌드에서 `$(EmbedAssembliesIntoApk)` 속성을 `True`로 설정하여 비활성화할 수 있습니다.


## <a name="msbuild-projects"></a>MSBuild 프로젝트

Xamarin.Android 빌드 프로세스는 Mac용 Visual Studio 및 Visual Studio에서 사용하는 프로젝트 파일 형식이기도 한 MSBuild에 기반을 두고 있습니다.
일반적으로 사용자는 MSBuild 파일을 직접 편집할 필요가 없습니다. IDE가 완전히 작동하는 프로젝트를 만들고, 여기에 변경 사항을 업데이트하며, 필요에 따라 빌드 대상을 자동으로 호출하기 때문입니다. 

고급 사용자가 IDE의 GUI에서 지원하지 않는 작업을 수행할 수도 있으므로, 프로젝트 파일을 직접 편집하여 빌드 프로세스를 사용자 지정할 수 있습니다. 이 페이지에서는 Xamarin.Android 관련 기능과 사용자 지정만 설명하지만 일반적인 MSBuild 항목, 속성 및 대상을 사용하여 여러 가지 작업을 수행할 수 있습니다. 

<a name="Build_Targets" />

## <a name="build-targets"></a>빌드 대상

Xamarin.Android 프로젝트에는 다음 빌드 대상이 정의됩니다.

-   **Build** &ndash; 패키지를 빌드합니다.

-   **Clean** &ndash; 빌드 프로세스에서 생성한 모든 파일을 제거합니다.

-   **Install** &ndash; 기본 장치 또는 가상 장치에 패키지를 설치합니다.

-   **Uninstall** &ndash; 기본 장치 또는 가상 장치에서 패키지를 제거합니다.

-   **SignAndroidPackage** &ndash; 패키지를 만들고 서명합니다(`.apk`). 자체 포함 "릴리스" 패키지를 생성하려면 `/p:Configuration=Release`와 함께 사용합니다.

-   **UpdateAndroidResources** &ndash; `Resource.designer.cs` 파일을 업데이트합니다. 이 대상은 일반적으로 프로젝트에 새 리소스가 추가될 때 IDE에 의해 호출됩니다.


## <a name="build-properties"></a>빌드 속성

MSBuild 속성은 대상의 동작을 제어합니다. [MSBuild PropertyGroup 요소](http://msdn.microsoft.com/en-us/library/t4w159bs.aspx) 내 프로젝트 파일(예: **MyApp.csproj**)에 지정됩니다. 

-   **Configuration** &ndash; "디버그" 또는 "릴리스"와 같이 사용할 빌드 구성을 지정합니다. Configuration 속성은 대상 동작을 결정하는 다른 속성에 대한 기본값을 결정하는 데 사용됩니다. IDE 내에 추가 구성을 만들 수 있습니다.

    *기본적으로* `Debug` 구성은 `Install` 및 `SignAndroidPackage` 대상에서 다른 파일과 패키지가 존재해야 작동하는 소형 Android 패키지를 생성하도록 합니다.

    기본 `Release` 구성은 `Install` 및 `SignAndroidPackage` 대상에서 다른 패키지나 파일을 설치하지 않고도 사용할 수 있는 ‘독립 실행형’ Android 패키지를 생성하도록 합니다.

-   **DebugSymbols** &ndash; `$(DebugType)` 속성과 함께 사용되며, Android 패키지가 *디버그 가능*한지 결정하는 부울 값입니다. 디버그 가능한 패키지는 디버그 기호를 포함하고, `//application/@android:debuggable` 특성을 `true`로 설정하며, 디버거가 프로세스에 연결할 수 있도록 `INTERNET` 권한을 자동으로 추가합니다. `DebugSymbols`가 `True`*이고* `DebugType`이 빈 문자열이거나 `Full`일 경우 응용 프로그램을 디버그할 수 있습니다.

-   **DebugType** &ndash; 빌드의 일부로 생성할 [디버그 기호의 유형](http://msdn.microsoft.com/en-us/library/s5c8athz.aspx)을 지정하며, 응용 프로그램의 디버그 가능 여부에도 영향을 줍니다. 가능한 값은 다음과 같습니다.

    - **Full**: 전체 기호가 생성됩니다. `DebugSymbols` MSBuild 속성도 `True`일 경우 응용 프로그램 패키지를 디버그할 수 있습니다.

    - **PdbOnly**: "PDB" 기호가 생성됩니다. 응용 프로그램 패키지를 디버그할 수 *없습니다*.

    `DebugType`이 설정되지 않았거나 빈 문자열이면 `DebugSymbols` 속성에서 응용 프로그램의 디버그 가능 여부를 제어합니다.


### <a name="install-properties"></a>설치 속성

설치 속성은 `Install` 및 `Uninstall` 대상의 동작을 제어합니다.

-   **AdbTarget** &ndash; Android 패키지를 설치하거나 제거할 수 있는 Android 대상 장치를 지정합니다. 이 속성의 값은 [`adb` 대상 장치 옵션](http://developer.android.com/tools/help/adb.html#issuingcommands)과 동일합니다.

    ```bash
    # Install package onto emulator via -e
    # Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
    MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
    ```


### <a name="packaging-properties"></a>패키징 속성

패키징 속성은 Android 패키지의 생성을 제어하며, `Install` 및 `SignAndroidPackage` 대상에서 사용합니다.
릴리스 응용 프로그램을 패키지하는 경우에는 [서명 속성](#Signing_Properties)도 관련이 있습니다.


-   **AndroidApkSigningAlgorithm** &ndash; `jarsigner -sigalg`와 함께 사용하는 서명 알고리즘을 지정하는 문자열 값입니다.

    기본값은 `md5withRSA`입니다.

    Xamarin.Android 8.2에 추가되었습니다.

-   **AndroidApplication** &ndash; 프로젝트가 Android 응용 프로그램용(`True`)인지, Android 라이브러리 프로젝트용(`False` 또는 없음)인지 나타내는 부울 값입니다.

    Android 패키지에는 `<AndroidApplication>True</AndroidApplication>`인 프로젝트가 하나만 존재할 수 있습니다. (하지만 이는 아직 확인되지 않았으며, Android 리소스와 관련하여 미묘하고 기이한 오류를 유발할 수 있습니다.)

-   **AndroidBuildApplicationPackage** &ndash; 패키지(.apk)를 만들고 서명할지 여부를 나타내는 부울 값입니다. 이 값을 `True`로 설정하는 것은 [SignAndroidPackage](#Build_Targets) 빌드 대상을 사용하는 것과 동일합니다.

    이 속성에 대한 지원은 Xamarin.Android 7.1 이후에 추가되었습니다.

    기본적으로 이 속성은 `False`입니다.

-   **AndroidEnableMultiDex** &ndash; 최종 `.apk`에서 Multi-Dex 지원이 사용되는지 여부를 결정하는 부울 속성입니다.

    이 속성에 대한 지원은 Xamarin.Android 5.1에 추가되었습니다.

    기본적으로 이 속성은 `False`입니다.

-   **AndroidEnableSGenConcurrent** &ndash; Mono의 [동시 GC 수집기](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#concurrent-sgen)를 사용할지 여부를 결정하는 부울 속성입니다.

    이 속성에 대한 지원은 Xamarin.Android 7.2에 추가되었습니다.

    기본적으로 이 속성은 `False`입니다.

-   **AndroidErrorOnCustomJavaObject** &ndash; 형식이 `Java.Lang.Object` 또는 `Java.Lang.Throwable`에서도 상속되지 ‘않고’ `Android.Runtime.IJavaObject`
    를 구현할 수 있는지 여부를 확인하는 부울 속성입니다.

    ```csharp
    class BadType : IJavaObject {
        public IntPtr Handle {
            get {return IntPtr.Zero;}
        }

        public void Dispose()
        {
        }
    }
    ```

    True이면 XA4212 오류가 생성되고, 그렇지 않으면 XA4212 경고가 생성됩니다.

    이 속성에 대한 지원은 Xamarin.Android 8.1에 추가되었습니다.

    기본적으로 이 속성은 `True`입니다.

-   **AndroidFastDeploymentType** &ndash; `$(EmbedAssembliesIntoApk)` MSBuild 속성이 `False`일 경우 대상 장치에서 [빠른 배포 디렉터리](#Fast_Deployment)에 배포할 수 있는 형식을 제어하는 값의 콜론(`:`)으로 구분된 목록입니다. 리소스가 빠르게 배포될 경우 생성된 `.apk`에 포함되지 *않아* 배포 시간을 줄일 수 있습니다. (빠르게 배포되는 리소스가 많을수록 `.apk`를 재빌드해야 하는 경우가 줄어들고 설치 프로세스가 빨라질 수 있습니다.) 유효한 값은 다음과 같습니다.

    - `Assemblies`: 응용 프로그램 어셈블리를 배포합니다.

    - `Dexes`: `.dex` 파일, Android 리소스 및 Android 자산을 배포합니다. **이 값은 *오직* Android 4.4 이상(API-19)을 실행 중인 장치에서만 사용할 수 있습니다.**

    기본값은 `Assemblies`입니다.

    **실험적**. Xamarin.Android 6.1에 추가되었습니다.

-   **AndroidApplicationJavaClass** &ndash; 클래스가 [Android.App.Application](https://developer.xamarin.com/api/type/Android.App.Application/)에서 상속되는 경우 `android.app.Application` 대신 사용할 전체 Java 클래스 이름입니다.

    이 속성은 일반적으로 `$(AndroidEnableMultiDex)` MSBuild 속성과 같은 *other* 속성으로 설정됩니다.

    Xamarin.Android 6.1에 추가되었습니다.

-   **AndroidHttpClientHandlerType** &ndash; `System.Net.Http.HttpClient` 기본 생성자에서 사용할 기본 `System.Net.Http.HttpMessageHandler` 구현을 제어합니다. 값은 [`System.Type.GetType(string)`](/dotnet/api/system.type.gettype?view=netcore-2.0#System_Type_GetType_System_String_)과 함께 사용하기에 적합한 `HttpMessageHandler` 서브클래스의 정규화된 어셈블리 형식 이름입니다.

    기본값은 `System.Net.Http.HttpClientHandler, System.Net.Http`입니다.

    이는 네트워크 요청을 수행하기 위해 Android Java API를 사용하는 `Xamarin.Android.Net.AndroidClientHandler`를 포함하도록 재정의될 수 있습니다. 기본 Android 버전이 TLS 1.2를 지원할 때 TLS 1.2 URL 액세스를 허용합니다.  
    Android 5.0 이상에서만 Java를 통해 TLS 1.2 지원을 안정적으로 제공할 수 있습니다.

    참고: Android 버전 5.0 이전에서 TLS 1.2 지원이 필요하거나 TLS 1.2 지원이 `System.Net.WebClient` 및 관련 API에 필요한 경우 `$(AndroidTlsProvider)`를 사용해야 합니다.

    참고: 이 속성에 대한 지원은 [`XA_HTTP_CLIENT_HANDLER_TYPE` 환경 변수](~/android/deploy-test/environment.md)를 설정하여 작동됩니다.
    파일에서 `$XA_HTTP_CLIENT_HANDLER_TYPE` 값을 찾았으며 `@(AndroidEnvironment)`의 빌드 동작이 우선됩니다.

    Xamarin.Android 6.1에 추가되었습니다.

-   **AndroidTlsProvider** &ndash; 응용 프로그램에서 사용할 TLS 공급자를 지정하는 문자열 값입니다. 가능한 값은 다음과 같습니다.

    - `btls`: [HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.aspx)와의 TLS 통신을 위해 [Boring SSL](https://boringssl.googlesource.com/boringssl)을 사용합니다.
      이렇게 하면 모든 Android 버전에서 TLS 1.2를 사용할 수 있습니다.

    - `legacy`: 네트워크 상호 작용을 위해 관리되는 SSL 구현 기록을 사용합니다. 이는 TLS 1.2를 지원하지 *않습니다*.

    - `default`: 기본 TLS 공급자를 선택하도록 *Mono*를 허용합니다.
      이는 Xamarin.Android 7.3에서도 `legacy`에 해당합니다.  
      참고: IDE “기본값”이 `$(AndroidTlsProvider)` 속성에서 ‘제거’되므로 이 값은 `.csproj` 값에 나타날 가능성이 없습니다.

    - 설정되지 않은/빈 문자열: Xamarin.Android 7.1에서 이는 `legacy`에 해당합니다.  
      Xamarin.Android 7.3에서는 `btls`에 해당합니다.

    기본값은 빈 문자열입니다.

    Xamarin.Android 7.1에 추가되었습니다.

-   **AndroidLinkMode** &ndash; Android 패키지 내에 포함된 어셈블리에 대해 수행해야 하는 [연결](~/android/deploy-test/linker.md) 유형을 지정합니다. Android 응용 프로그램 프로젝트 내에서만 사용됩니다. 기본값은 *SdkOnly*입니다. 올바른 값은 다음과 같습니다.

    - **None**: 연결을 시도하지 않습니다.

    - **SdkOnly**: 연결이 기본 클래스 라이브러리에서만 수행되며, 사용자 어셈블리에서는 수행되지 않습니다.

    - **Full**: 연결이 기본 클래스 라이브러리와 사용자 어셈블리에서 수행됩니다. **참고:** `AndroidLinkMode`에 *Full* 값을 사용할 경우 앱이 손상될 수 있습니다(특히 리플렉션을 사용하는 경우). *제대로* 알지 않는 한 이 값을 사용하지 마세요.

    ```xml
    <AndroidLinkMode>SdkOnly</AndroidLinkMode>
    ```

-   **AndroidLinkSkip** &ndash; 연결해서는 안 되는 어셈블리에 대한 세미콜론(`;`)으로 구분된 어셈블리 이름 목록을 파일 확장명 없이 지정합니다. Android 응용 프로그램 프로젝트 내에서만 사용됩니다.

    ```xml
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
    ```

-   **AndroidManagedSymbols** &ndash; `Release` 스택 추적에서 파일 이름과 줄 번호 정보를 추출할 수 있도록 시퀀스 점을 생성할지 여부를 제어하는 부울 속성입니다.

    Xamarin.Android 6.1에 추가되었습니다.

-   **AndroidManifest** &ndash; 앱의 [`AndroidManifest.xml`](~/android/platform/android-manifest.md)에 대한 템플릿으로 사용할 파일 이름을 지정합니다.
    빌드 중에 필요한 다른 모든 값이 병합되어 실제 `AndroidManifest.xml`이 생성됩니다.
    `$(AndroidManifest)`는 `/manifest/@package` 특성에 패키지 이름을 포함해야 합니다.

-   **AndroidSdkBuildToolsVersion** &ndash; Android SDK 빌드 도구 패키지는 **aapt** 및 **zipalign** 등의 도구를 제공합니다. 여러 가지 버전의 빌드-도구 패키지를 동시에 설치할 수 있습니다. 패키징할 빌드-도구 패키지는 “권장” 빌드-도구 버전을 확인하고 사용하는 방식으로 선택됩니다(있는 경우). “권장” 버전이 ‘없을’ 경우 설치된 빌드-도구 패키지 중 가장 높은 버전이 사용됩니다.

    `$(AndroidSdkBuildToolsVersion)` MSBuild 속성에는 기본 설정 빌드 도구 버전이 포함되어 있습니다. Xamarin.Android 빌드 시스템에서 `Xamarin.Android.Common.targets`에 기본값을 제공하고, 최신 aapt가 충돌하고 있지만 이전 버전의 aapt가 작동하는 것으로 알려져 있는 경우와 같이 다른 빌드 도구 버전을 선택하기 위해 프로젝트 파일 내에서 기본값을 재정의할 수 있습니다.

-   **AndroidSupportedAbis** &ndash; `.apk`에 포함되어야 하는 ABI에 대한 세미콜론(`;`)으로 구분된 목록을 포함하는 문자열 속성입니다.

    지원되는 값은 다음과 같습니다.

    -   `armeabi`
    -   `armeabi-v7a`
    -   `x86`
    -   `arm64-v8a`: Xamarin.Android 5.1 이상이 필요합니다.
    -   `x86_64`: Xamarin.Android 5.1 이상이 필요합니다.

-   **AndroidUseSharedRuntime** &ndash; 대상 장치에서 응용 프로그램을 실행하기 위해 *공유 런타임 패키지*가 필요한지 확인하는 부울 속성입니다. 공유 런타임 패키지를 사용하면 응용 프로그램 패키지의 크기를 줄여 패키지 생성 및 배포 프로세스의 속도를 높일 수 있습니다. 그러면 빌드, 배포, 디버그 소요 주기가 빨라집니다.

    이 속성은 디버그 빌드의 경우 `True`, 릴리스 프로젝트의 경우 `False`여야 합니다.

-   **AotAssemblies** &ndash; 어셈블리를 네이티브 코드에 Ahead-of-Time 컴파일하고 `.apk`에 포함할지 여부를 결정하는 부울 속성입니다.

    이 속성에 대한 지원은 Xamarin.Android 5.1에 추가되었습니다.

    기본적으로 이 속성은 `False`입니다.

-   **EmbedAssembliesIntoApk** &ndash; 앱의 어셈블리가 응용 프로그램 패키지에 포함되어야 하는지 여부를 결정하는 부울 속성입니다.

    이 속성은 릴리스 빌드의 경우 `True`, 디버그 빌드의 경우 `False`여야 합니다. 빠른 배포에서 대상 장치를 지원하지 않는 경우 디버그 빌드에서 `True`가 되어야 *할 수 있습니다*.

    이 속성이 `False`이면 `$(AndroidFastDeploymentType)` MSBuild 속성에서 `.apk`에 포함될 항목도 제어하며, 이로 인해 배포 및 다시 빌드 시간에 영향을 줍니다.

-   **EnableLLVM** &ndash; Ahead-of-Time에서 어셈블리를 네이티브 코드로 컴파일할 때 LLVM을 사용할지 여부를 결정하는 부울 속성입니다.

    이 속성에 대한 지원은 Xamarin.Android 5.1에 추가되었습니다.

    기본적으로 이 속성은 `False`입니다.

    `$(AotAssemblies)` MSBuild 속성이 `True`가 아닐 경우 이러한 속성이 무시됩니다.

-   **EnableProguard** &ndash; Java 코드를 연결하기 위해 패키징 프로세스의 일부로 [proguard](http://developer.android.com/tools/help/proguard.html)를 실행할지 여부를 결정하는 부울 속성입니다.

    이 속성에 대한 지원은 Xamarin.Android 5.1에 추가되었습니다.

    기본적으로 이 속성은 `False`입니다.

    `True`일 경우 [ProguardConfiguration](#ProguardConfiguration) 파일을 사용하여 `proguard` 실행을 제어합니다.

-   **JavaMaximumHeapSize** &ndash; 패키징 프로세스의 일부로 `.dex` 파일을 빌드할 때 사용할 **java**
    `-Xmx` 매개 변수 값을 지정합니다. 지정하지 않으면 `-Xmx` 옵션이 **java**에 제공되지 않습니다.

    [`_CompileDex` 대상이 `java.lang.OutOfMemoryError`를 throw](https://bugzilla.xamarin.com/show_bug.cgi?id=18327)할 경우 이 속성을 지정해야 합니다.

    ```xml
    <JavaMaximumHeapSize>1G</JavaMaximumHeapSize>
    ```

-   **JavaOptions** &ndash; `.dex` 파일을 빌드할 때 **java**에 전달할 추가 명령줄 옵션을 지정합니다.

-   **MandroidI18n** &ndash; 데이터 정렬 및 정렬 테이블처럼 응용 프로그램에 포함할 국제화 지원을 지정합니다. 값은 다음과 같은 대/소문자 구분 값이 하나 이상 포함된 쉼표 또는 세미콜론으로 구분된 목록입니다.

    -   **None**: 추가 인코딩을 포함하지 않습니다.

    -   **All**: 사용 가능한 모든 인코딩을 포함합니다.

    -   **CJK**: *일본어(EUC)* \[enc-jp, CP51932\], *일본어(Shift-JIS)* \[iso-2022-jp, shift\_jis, CP932\], *일본어(JIS)* \[CP50220\], *중국어 간체(GB2312)* \[gb2312, CP936\], *한국어(UHC)* \[ks\_c\_5601-1987, CP949\], *한국어(EUC)* \[euc-kr, CP51949\], *중국어 번체(Big5)* \[big5, CP950\] 및 *중국어 간체(GB18030)* \[GB18030, CP54936\] 같은 중국어, 일본어 및 한국어 인코딩을 포함합니다.

    -   **MidEast**: *터키어(Windows)* \[iso-8859-9, CP1254\], *히브리어(Windows)* \[windows-1255, CP1255\], *아랍어(Windows)* \[windows-1256, CP1256\], *아랍어(ISO)* \[iso-8859-6, CP28596\], *히브리어(ISO)* \[iso-8859-8, CP28598\], *라틴어 5(ISO)* \[iso-8859-9, CP28599\] 및 *히브리어(Iso Alternative)* \[iso-8859-8, CP38598\] 같은 중동 인코딩을 포함합니다.

    -   **Other**: *키릴 자모(Windows)* \[CP1251\], *발트어(Windows)* \[iso-8859-4, CP1257\], *베트남어(Windows)* \[CP1258\], *키릴 자모(KOI8-R)* \[koi8-r, CP1251\], *우크라이나어(KOI8-U)* \[koi8-u, CP1251\], *발트어(ISO)* \[iso-8859-4, CP1257\], *키릴 자모(ISO)* \[iso-8859-5, CP1251\], *ISCII 데바나가리 문자* \[x-iscii-de, CP57002\], *ISCII 벵골어* \[x-iscii-be, CP57003\], *ISCII 타밀어* \[x-iscii-ta, CP57004\], *ISCII 텔루구어* \[x-iscii-te, CP57005\], *ISCII 아삼어* \[x-iscii-as, CP57006\], *ISCII 오리야어* \[x-iscii-or, CP57007\], *ISCII 칸나다어* \[x-iscii-ka, CP57008\], *ISCII 말라얄람어* \[x-iscii-ma, CP57009\], *ISCII 구자라트어* \[x-iscii-gu, CP57010\], *ISCII 펀잡어* \[x-iscii-pa, CP57011\] 및 *태국어(Windows)* \[CP874\] 같은 기타 인코딩을 포함합니다.

    -   **Rare**: *IBM EBCDIC(터키어)* \[CP1026\], *IBM EBCDIC(개방형 시스템 라틴어 1)* \[CP1047\], *IBM EBCDIC(미국-캐나다와 유로)* \[CP1140\], *IBM EBCDIC(독일과 유로)* \[CP1141\], *IBM EBCDIC(덴마크/노르웨이와 유로)* \[CP1142\], *IBM EBCDIC(핀란드/스웨덴과 유로)* \[CP1143\], *IBM EBCDIC(이탈리아와 유로)* \[CP1144\], *IBM EBCDIC(라틴 아메리카/스페인과 유로)* \[CP1145\], *IBM EBCDIC(영국과 유로)* \[CP1146\], *IBM EBCDIC(프랑스와 유로)* \[CP1147\], *IBM EBCDIC(국가별 설정과 유로)* \[CP1148\], *IBM EBCDIC(아이슬란드어와 유로)* \[CP1149\], *IBM EBCDIC(독일)* \[CP20273\], *IBM EBCDIC(덴마크/노르웨이)* \[CP20277\], *IBM EBCDIC(핀란드/스웨덴)* \[CP20278\], *IBM EBCDIC(이탈리아)* \[CP20280\], *IBM EBCDIC(라틴 아메리카/스페인)* \[CP20284\], *IBM EBCDIC(영국)* \[CP20285\], *IBM EBCDIC(일본어 가타카나 확장)* \[CP20290\], *IBM EBCDIC(프랑스)* \[CP20297\], *IBM EBCDIC(아랍어)* \[CP20420\], *IBM EBCDIC(히브리어)* \[CP20424\], *IBM EBCDIC(아이슬란드어)* \[CP20871\], *IBM EBCDIC(키릴 자모 - 세르비아어, 불가리아어)* \[CP21025\], *IBM EBCDIC(미국-캐나다)* \[CP37\], *IBM EBCDIC(국가별 설정)* \[CP500\], *아랍어(ASMO 708)* \[CP708\], *중앙 유럽어(DOS)* \[CP852\]*, 키릴 자모(DOS)* \[CP855\], *터키어(DOS)* \[CP857\], *서유럽어(DOS와 유로)* \[CP858\], *히브리어(DOS)* \[CP862\], *아랍어(DOS)* \[CP864\], *러시아어(DOS)* \[CP866\], *그리스어(DOS)* \[CP869\], *IBM EBCDIC(라틴어 2)* \[CP870\] 및 *IBM EBCDIC(그리스어)* \[CP875\] 같은 희귀한 인코딩을 포함합니다.

    -   **West**: *서유럽어(Mac)* \[macintosh, CP10000\], *아이슬란드어(Mac)* \[x-mac-icelandic, CP10079\], *중앙 유럽어(Windows)* \[iso-8859-2, CP1250\], *서유럽어(Windows)* \[iso-8859-1, CP1252\], *그리스어(Windows)* \[iso-8859-7, CP1253\], *중앙 유럽어(ISO)* \[iso-8859-2, CP28592\], *라틴어 3(ISO)* \[iso-8859-3, CP28593\], *그리스어(ISO)* \[iso-8859-7, CP28597\], *라틴어 9 (ISO)* \[iso-8859-15, CP28605\], *OEM 미국* \[CP437\], *서유럽어(DOS)* \[CP850\], *포르투갈어(DOS)* \[CP860\], *아이슬란드어(DOS)* \[CP861\], *프랑스어(캐나다)(DOS)* \[CP863\] 및 *북유럽어(DOS)* \[CP865\] 같은 서부 인코딩을 포함합니다.


    ```xml
    <MandroidI18n>West</MandroidI18n>
    ```

-   **MonoSymbolArchive** &ndash; 릴리스 스택 추적에서 &ldquo;실제&rdquo; 파일 이름 및 줄 번호 정보를 추출하기 위해 나중에 `mono-symbolicate`와 함께 사용할 `.mSYM` 아티팩트를 만들지 여부를 제어하는 부울 속성입니다.

    디버깅 기호가 활성화된 &ldquo;릴리스&rdquo; 앱의 경우 이는 기본적으로 True입니다. `$(EmbedAssembliesIntoApk)`, `$(DebugSymbols)`, `$(Optimize)` 모두 True입니다.

    Xamarin.Android 7.1에 추가되었습니다.

-   **AndroidVersionCodePattern** &ndash; 개발자가 매니페스트의 `versionCode`를 사용자 지정할 수 있게 해주는 문자열 속성입니다.
    `versionCode`를 확인하는 방법에 대한 자세한 내용은 [APK의 버전 코드 만들기](~/android/deploy-test/building-apps/abi-specific-apks.md)를 참조하세요.
    
    예를 들어 `abi`가 `armeabi`이고 매니페스트의 `versionCode`가 `123`일 경우 `{abi}{versionCode}`는 `$(AndroidCreatePackagePerAbi)`가 True일 때 `1123`의 versionCode를 생성하고, 그렇지 않을 경우 값 123을 생성합니다.
    `abi`가 `x86_64`이고 매니페스트의 `versionCode`가 `44`일 경우. `$(AndroidCreatePackagePerAbi)`가 True일 때 `544`를 생성하고, 그렇지 않을 경우 값 `44`를 생성합니다.

    왼쪽 안쪽 여백 문자열 `{abi}{versionCode:0000}`을 포함할 경우 `versionCode`의 왼쪽 안쪽 여백이 `0`이므로 `50044`가 생성됩니다. 또는 이전 예제와 동일한 작업을 수행하는 `{abi}{versionCode:D4}`와 같은 10진수 패딩을 사용할 수 있습니다.

    값이 정수여야 하므로 '0' 및 'Dx' 같은 안쪽 여백 형식만 지원됩니다.
    
    미리 정의된 주요 항목

    -   **abi** &ndash; 앱에 대한 대상 abi를 삽입합니다.
        -   1 &ndash; `armeabi`
        -   2 &ndash; `armeabi-v7a`
        -   3 &ndash; `x86`
        -   4 &ndash; `arm64-v8a`
        -   5 &ndash; `x86_64`

    -   **minSDK** &ndash; 아무것도 정의되지 않은 경우 `AndroidManifest.xml` 또는 `11`에서 지원되는 최소 Sdk 값을 삽입합니다.

    -   **versionCode** &ndash; `Properties\AndroidManifest.xml`에서 직접 버전 코드를 사용합니다. 

    `$(AndroidVersionCodeProperties)` 속성(다음에 정의됨)을 사용하여 사용자 지정 항목을 정의할 수 있습니다.

    기본적으로 값은 `{abi}{versionCode:D6}`으로 설정됩니다. 개발자가 이전 동작을 유지하려는 경우 `$(AndroidUseLegacyVersionCode)` 속성을 `true`로 설정하여 기본값을 재정의할 수 있습니다.

    Xamarin.Android 7.2에 추가되었습니다.

-   **AndroidVersionCodeProperties** &ndash; 개발자가 `AndroidVersionCodePattern`과 함께 사용할 사용자 지정 항목을 정의할 수 있게 해주는 문자열 속성입니다. `key=value` 쌍 형식입니다. `value`의 모든 항목은 정수 값이어야 합니다. 예: `screen=23;target=$(_SupportedApiLevel)` 보시다시피 문자열에 기존 또는 사용자 지정 MSBuild 속성을 활용할 수 있습니다.

    Xamarin.Android 7.2에 추가되었습니다.

-   **AndroidUseLegacyVersionCode** &ndash; 부울 속성을 사용하면 개발자가 versionCode 계산을 이전의 사전 Xamarin.Android 8.2 동작으로 되돌릴 수 있습니다. 이러한 방법은 Google Play 스토어에서 기존 응용 프로그램을 사용하는 개발자만 사용해야 합니다. 새 `$(AndroidVersionCodePattern)` 속성을 사용하는 것이 좋습니다.

    Xamarin.Android 8.2에 추가되었습니다.

-  **AndroidUseManagedDesignTimeResourceGenerator** &ndash; `aapt`가 아니라 관리되는 리소스 파서를 사용하는 디자인 타임 빌드로 전환하는 부울 속성입니다.

    Xamarin.Android 8.1에 추가되었습니다.

-  **AndroidUseApkSigner** &ndash; 개발자가 `jarsigner`가 아니라 `apksigner` 도구를 사용하도록 허용하는 부울 속성입니다.

    Xamarin.Android 8.2에 추가되었습니다.

-  **AndroidApkSignerAdditionalArguments** &ndash; 개발자가 `apksigner` 도구에 추가 인수를 제공할 수 있는 문자열 속성입니다.

    Xamarin.Android 8.2에 추가되었습니다.

### <a name="binding-project-build-properties"></a>프로젝트 빌드 속성 바인딩

다음 MSBuild 속성을 [바인딩 프로젝트](~/android/platform/binding-java-library/index.md)와 함께 사용할 수 있습니다.

-   **AndroidClassParser** &ndash; `.jar` 파일이 구문 분석되는 방법을 제어하는 문자열 속성입니다. 가능한 값은 다음과 같습니다.

    - **class-parse**: JVM의 보조 없이 `class-parse.exe`를 사용하여 Java 바이트코드의 구문 분석을 바로 수행합니다. 이 값은 실험적입니다. 


    - **jar2xml**: `jar2xml.jar`을 통해 Java 리플렉션을 사용하여 `.jar` 파일에서 형식과 멤버를 추출합니다.

    `jar2xml`보다 `class-parse`가 더 나은 점은 다음과 같습니다.

    - `class-parse`는 *디버그* 기호를 포함하는 Java 바이트 코드(예: `javac -g`로 컴파일된 바이트코드)에서 매개 변수 이름을 추출할 수 있습니다.

    - `class-parse`는 확인할 수 없는 형식의 멤버에서 상속하거나 이러한 멤버를 포함하는 클래스를 “건너뛰지” 않습니다.

    **실험적**. Xamarin.Android 6.0에 추가되었습니다.

    기본값은 `jar2xml`입니다.

    이 기본값은 향후 릴리스에서 변경될 예정입니다.

-   **AndroidCodegenTarget** &ndash; 코드 생성 대상 ABI를 제어하는 문자열 속성입니다. 가능한 값은 다음과 같습니다.

    - **XamarinAndroid**: Mono for Android 1.0 이후에 제공되는 JNI 바인딩 API를 사용합니다. Xamarin.Android 5.0 이상을 사용하여 빌드된 바인딩 어셈블리는 Xamarin.Android 5.0 이상(API/ABI 추가)에서만 실행할 수 있지만 *소스*는 이전 제품 버전과 호환됩니다.

    - **XAJavaInterop1**: JNI 호출용 Java.Interop을 사용합니다. `XAJavaInterop1`을 사용하는 바인딩 어셈블리는 Xamarin.Android 6.1 이상을 통해서만 빌드하고 실행할 수 있습니다. Xamarin.Android 6.1 이상은 이 값과 `Mono.Android.dll`을 바인딩합니다.

      `XAJavaInterop1`의 이점은 다음과 같습니다.

      - 더 작은 어셈블리.

      - `base` 메서드 호출을 위한 `jmethodID` 캐싱(상속 계층 구조의 다른 모든 바인딩 형식이 `XAJavaInterop1` 이상으로 빌드된 경우에 한함).

      - 관리되는 서브클래스를 위한 Java Callable Wrapper 생성자 `jmethodID` 캐싱.

    기본값은 `XamarinAndroid`입니다.

    이 기본값은 향후 릴리스에서 변경될 예정입니다.


### <a name="resource-properties"></a>리소스 속성

리소스 속성은 Android 리소스에 대한 액세스를 제공하는 `Resource.designer.cs` 파일의 생성을 제어합니다. 

-   **AndroidResgenExtraArgs** &ndash; Android 자산 및 리소스 처리 시 **aapt** 명령에 전달할 추가 명령줄 옵션을 지정합니다.

-   **AndroidResgenFile** &ndash; 생성할 리소스 파일의 이름을 지정합니다. 기본 템플릿은 이를 `Resource.designer.cs`로 설정합니다.

-   **MonoAndroidResourcePrefix** &ndash; 빌드 동작이 `AndroidResource`인 파일 이름의 앞 부분에서 제거되는 *경로 접두사*를 지정합니다. 리소스가 있는 위치를 변경하도록 하기 위함입니다.

    기본값은 `Resources`입니다. Java 프로젝트 구조체의 경우 이를 `res`로 변경합니다.

-   **AndroidExplicitCrunch** &ndash; 매우 많은 로컬 드로어블을 사용하여 앱을 빌드할 경우 초기 빌드(또는 재빌드)를 완성하는 데 수 분이 걸릴 수 있습니다. 빌드 프로세스 속도를 높이려면 이 속성을 포함하고 `True`로 설정해 보세요. 이 속성이 설정되면 빌드 프로세스가 .png 파일을 사전에 크런치합니다.

    **실험적**. Xamarin.Android 7.0에 추가되었습니다.


<a name="Signing_Properties" />

### <a name="signing-properties"></a>서명 속성

서명 속성은 Android 장치에 설치할 수 있도록 응용 프로그램 패키지에 서명하는 방법을 제어합니다. 더욱 빠른 빌드 반복을 위해 Xamarin.Android 작업은 빌드 프로세스 중에 패키지에 서명되지 않습니다. 서명 속도가 상당히 느리기 때문입니다. 대신, IDE 또는 *설치* 빌드 대상을 통해 설치 전이나 내보내기 중에 서명합니다(필요할 경우). *SignAndroidPackage* 대상을 호출하면 출력 디렉터리에 접미사가 `-Signed.apk`인 패키지가 생성됩니다.

기본적으로 서명 대상은 필요한 경우 새 디버그 서명 키를 생성합니다. 예를 들어 빌드 서버에서 특정 키를 사용하려는 경우 다음 MSBuild 속성을 사용할 수 있습니다.

-   **AndroidKeyStore** &ndash; 사용자 지정 서명 정보를 사용할지 여부를 나타내는 부울 값입니다. 기본값은 `False`이며, 이는 기본 디버그 서명 키를 사용하여 패키지에 서명한다는 의미입니다.

-   **AndroidSigningKeyAlias** &ndash; 키 저장소의 키에 대한 별칭을 지정합니다. 이는 키 저장소를 만들 때 사용되는 **keytool -alias** 값입니다. 

-   **AndroidSigningKeyPass** &ndash; 키 저장소 파일 내에 키의 암호를 지정합니다. 이는 `keytool`이 **$(AndroidSigningKeyAlias)에 대한 키 암호 입력**을 요청하는 경우에 입력하는 값입니다.

-   **AndroidSigningKeyStore** &ndash; `keytool`에서 만든 키 저장소 파일의 파일 이름을 지정합니다. 이는 **keytool -keystore** 옵션에 입력된 값에 해당합니다.

-   **AndroidSigningStorePass** &ndash; `$(AndroidSigningKeyStore)`에 대한 암호를 지정합니다. 이는 키 저장소 파일을 만들 때 `keytool`에 제공되고 **키 저장소 암호 입력:** 을 요청하는 값입니다. 

예를 들어 다음 `keytool` 호출을 살펴봅니다.

```shell
$ keytool -genkey -v -keystore filename.keystore -alias keystore.alias -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password: keystore.filename password
Re-enter new password: keystore.filename password
...
Is CN=... correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: ...
Enter key password for keystore.alias
        (RETURN if same as keystore password): keystore.alias password
[Storing filename.keystore]
```

위에서 생성한 키 저장소를 사용하려면 속성 그룹을 사용합니다.

```xml
<PropertyGroup>
    <AndroidKeyStore>True</AndroidKeyStore>
    <AndroidSigningKeyStore>filename.keystore</AndroidSigningKeyStore>
    <AndroidSigningStorePass>keystore.filename password</AndroidSigningStorePass>
    <AndroidSigningKeyAlias>keystore.alias</AndroidSigningKeyAlias>
    <AndroidSigningKeyPass>keystore.alias password</AndroidSigningKeyPass>
</PropertyGroup>
```

-   **AndroidDebugKeyAlgorithm** &ndash; `debug.keystore`에 사용할 기본 알고리즘을 지정합니다. 기본값은 `RSA`입니다.

-   **AndroidDebugKeyValidity** &ndash; `debug.keystore`에 사용할 기본 유효성을 지정합니다. 기본값은 `10950`, `30 * 365` 또는 `30 years`입니다.

<a name="Build_Actions" />

## <a name="build-actions"></a>빌드 작업

*빌드 동작*은 프로젝트 내 [파일에 적용](http://msdn.microsoft.com/en-us/library/bb629388.aspx)되며, 파일이 처리되는 방식을 제어합니다. 

<a name="AndroidEnvironment" />

### <a name="androidenvironment"></a>AndroidEnvironment

빌드 동작이 `AndroidEnvironment`인 파일은 [프로세스 시작 시 환경 변수 및 시스템 속성을 초기화](~/android/deploy-test/environment.md)하는 데 사용됩니다.
`AndroidEnvironment` 빌드 동작은 여러 파일에 적용될 수 있으며, 특정 순서로 평가되지 않습니다(따라서 여러 파일에 동일한 환경 변수나 시스템 속성을 지정해서는 안 됨).


### <a name="androidjavasource"></a>AndroidJavaSource

빌드 동작이 `AndroidJavaSource`인 파일은 최종 Android 패키지에 포함될 Java 소스 코드입니다.


### <a name="androidjavalibrary"></a>AndroidJavaLibrary

빌드 동작이 `AndroidJavaLibrary`인 파일은 최종 Android 패키지에 포함될 Java 아카이브(`.jar` 파일)입니다.


### <a name="androidresource"></a>AndroidResource

빌드 동작이 *AndroidResource*인 모든 파일은 빌드 프로세스 중에 Android 리소스에 컴파일되며, `$(AndroidResgenFile)`을 통해 액세스할 수 있게 됩니다.

```xml
<ItemGroup>
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
```

고급 사용자는 동일한 유효 경로를 사용하여 여러 구성에서 여러 리소스를 사용하기를 원할 수 있습니다. 이를 위해서는 여러 리소스 디렉터리가 있고 이러한 여러 디렉터리에 동일한 상대 경로를 가진 파일이 있어야 하며, 여러 구성의 여러 파일을 조건부로 포함하는 MSBuild 조건을 사용해야 합니다. 예:

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources\values\strings.xml" />
</ItemGroup>
<ItemGroup  Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug\values\strings.xml"/>
</ItemGroup>
<PropertyGroup>
  <MonoAndroidResourcePrefix>Resources;Resources-Debug<MonoAndroidResourcePrefix>
</PropertyGroup>
```

**LogicalName** &ndash; 리소스 경로를 명시적으로 지정합니다. 여러 개별 리소스 이름으로 사용할 수 있도록 &ldquo;별칭&rdquo; 파일을 허용합니다.

```xml
<ItemGroup Condition="'$(Configuration)'!='Debug'">
  <AndroidResource Include="Resources/values/strings.xml"/>
</ItemGroup>
<ItemGroup Condition="'$(Configuration)'=='Debug'">
  <AndroidResource Include="Resources-Debug/values/strings.xml">
    <LogicalName>values/strings.xml</LogicalName>
  </AndroidResource>
</ItemGroup>
```


### <a name="androidnativelibrary"></a>AndroidNativeLibrary

[네이티브 라이브러리](~/android/platform/native-libraries.md)는 `AndroidNativeLibrary`에 빌드 동작을 설정하여 빌드에 추가됩니다.

Android는 여러 ABI(응용 프로그램 이진 인터페이스)를 지원하므로, 빌드 시스템은 네이티브 라이브러리가 대상으로 하는 ABI를 알아야 합니다. 이는 두 가지 방법으로 수행할 수 있습니다.

1.  경로 "검색".
2.  `Abi` 항목 특성 사용.

경로 검색을 사용하면 네이티브 라이브러리의 부모 디렉터리 이름을 사용하여 라이브러리가 대상으로 하는 ABI를 지정할 수 있습니다. 따라서 빌드에 `lib/armeabi/libfoo.so`를 추가하면 ABI가 `armeabi`로 "검색"됩니다. 


#### <a name="item-attribute-name"></a>항목 특성 이름

**Abi** &ndash; 네이티브 라이브러리의 ABI를 지정합니다.

```xml
<ItemGroup>
  <AndroidNativeLibrary Include="path/to/libfoo.so">
    <Abi>armeabi</Abi>
  </AndroidNativeLibrary>
</ItemGroup>
```


### <a name="androidaarlibrary"></a>AndroidAarLibrary

.aar 파일을 직접 참조하려면 `AndroidAarLibrary`의 빌드 동작을 사용해야 합니다. 이 빌드 동작은 Xamarin 구성 요소에서 가장 일반적으로 사용됩니다. 즉 Google Play 및 기타 서비스를 작동하는 데 필요한 .aar 파일에 대한 참조를 포함합니다.

이 빌드 동작을 사용하는 파일은 라이브러리 프로젝트에 있는 포함 리소스와 비슷한 방식으로 처리됩니다. .aar은 중간 디렉터리로 추출됩니다. 그런 다음, 모든 자산, 자원 및 .jar 파일이 적절한 항목 그룹에 포함됩니다.  

### <a name="content"></a>콘텐츠

일반적인 `Content` 빌드 동작이 지원되지 않습니다(비용이 많이 드는 최초 실행 단계 없이 지원하는 방법을 파악하지 못했기 때문).

Xamarin.Android 5.1부터 `@(Content)` 빌드 작업을 사용하려고 하면 `XA0101` 경고가 표시됩니다.

### <a name="linkdescription"></a>LinkDescription

빌드 동작이 *LinkDescription*인 파일은 [링커 동작을 제어](~/cross-platform/deploy-test/linker.md)하는 데 사용됩니다.


<a name="ProguardConfiguration" />

### <a name="proguardconfiguration"></a>ProguardConfiguration

빌드 동작이 *ProguardConfiguration*인 파일에는 `proguard` 동작을 제어하는 데 사용되는 옵션이 포함됩니다. 이 빌드 동작에 대한 자세한 내용은 [ProGuard](~/android/deploy-test/release-prep/proguard.md)를 참조하세요.

`$(EnableProguard)` MSBuild 속성이 `True`가 아닐 경우 이러한 파일이 무시됩니다.


## <a name="target-definitions"></a>대상 정의

빌드 프로세스의 Xamarin.Android 관련 부분은 `$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets`에 정의되지만 어셈블리를 빌드하기 위해 *Microsoft.CSharp.targets*와 같은 일반적인 언어 관련 대상도 필요합니다.

모든 언어 대상을 가져오기 전에 다음과 같은 빌드 속성을 설정해야 합니다.

```xml
<PropertyGroup>
  <TargetFrameworkIdentifier>MonoDroid</TargetFrameworkIdentifier>
  <MonoDroidVersion>v1.0</MonoDroidVersion>
  <TargetFrameworkVersion>v2.2</TargetFrameworkVersion>
</PropertyGroup>
```

C#에서는 *Xamarin.Android.CSharp.targets*를 가져와서 이러한 대상과 속성을 모두 포함할 수 있습니다. 

```xml
<Import Project="$(MSBuildExtensionsPath)\Xamarin\Android\Xamarin.Android.CSharp.targets" />
```

이 파일은 다른 언어에 맞게 쉽게 조정할 수 있습니다.
