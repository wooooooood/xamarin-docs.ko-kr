---
title: 릴리스용 애플리케이션 준비
ms.prod: xamarin
ms.assetid: 9C8145B3-FCF1-4649-8C6A-49672DDA4159
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/21/2018
---

# <a name="preparing-an-application-for-release"></a>릴리스용 애플리케이션 준비

애플리케이션을 코딩하고 테스트한 후에 배포할 패키지를 준비해야 합니다. 이 패키지를 준비하는 첫 번째 작업은 릴리스할 애플리케이션을 빌드하는 것입니다. 여기에는 주로 일부 애플리케이션 특성을 설정하는 작업이 포함됩니다.

다음 단계를 통해 릴리스용 앱을 빌드합니다.

-   **[애플리케이션 아이콘 지정](#Specify_the_Application_Icon)**&ndash; 각각의 Xamarin.Android 애플리케이션에는 지정된 애플리케이션 아이콘이 있어야 합니다. 기술적으로 필요하지는 않지만 Google Play와 같은 일부 마켓에서 필요합니다.

-   **[애플리케이션 버전 지정](#Versioning)**&ndash; 이 단계에서는 버전 정보를 초기화하거나 업데이트합니다. 이것은 향후 애플리케이션 업데이트와, 사용자가 설치한 애플리케이션 버전을 인지하도록 하기 위해 중요합니다.

-   **[APK 축소](#shrink_apk)** &ndash; 관리 코드에서 Xamarin.Android 링커나 Java 바이트코드에서 ProGuard를 사용하여 최종 APK의 크기를 상당히 줄일 수 있습니다.

-   **[애플리케이션 보호](#protect_app)**&ndash; 디버깅을 사용하지 못하게 하고, 관리 코드를 난독 처리하고, 디버그 방지 및 변조 방지를 추가하며 네이티브 컴파일을 사용하여 사용자나 공격자가 애플리케이션을 디버깅, 변조 또는 리버스 엔지니어링하지 못하게 합니다. 

-   **[패키지 속성 설정](#Set_Packaging_Properties)**&ndash; 패키지 속성은 Android 애플리케이션 패키지(APK)의 생성을 제어합니다. 이 단계에서는 APK를 최적화하고 그 자산을 보호하며 필요에 맞게 패키지를 모듈화합니다.

-   **[컴파일](#Compile)** &ndash; 이 단계에서는 코드와 자산을 컴파일하여 릴리스 모드에서의 빌드를 확인합니다.

-   **[게시를 위한 보관](#archive)** &ndash; 이 단게에서는 앱을 빌드하여 서명 및 게시를 위해 보관합니다.

이러한 각 단계는 아래에서 자세히 설명합니다.

<a name="Specify_the_Application_Icon" />

## <a name="specify-the-application-icon"></a>애플리케이션 아이콘 지정

각각의 Xamarin.Android 애플리케이션마다 애플리케이션 아이콘을 지정하는 것이 좋습니다. 일부 애플리케이션 마켓플레이스에서는 아이콘 없이 Android 애플리케이션을 게시하지 못합니다. `Application` 특성의 `Icon` 속성은 Xamarin.Android 프로젝트의 애플리케이션 아이콘을 지정하는 데 사용됩니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio 2017 이상에서는 다음 스크린샷과 같이 프로젝트 **속성**의 **Android 매니페스트** 섹션을 통해 애플리케이션 아이콘을 지정합니다.

[![애플리케이션 아이콘 설정](images/vs/01-application-icon-sml.png)](images/vs/01-application-icon.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac에서도 다음 스크린 샷에서처럼 **프로젝트 옵션**의 **Android 애플리케이션** 섹션을 통해 애플리케이션 아이콘을 지정할 수 있습니다.

[![애플리케이션 아이콘 설정](images/xs/01-application-icon-sml.png)](images/xs/01-application-icon.png#lightbox)

-----

이러한 예제에서 `@drawable/icon`은 **Resources/drawable/icon.png**(**.png** 확장명이 리소스 이름에 포함되지 않음)에 있는 아이콘 파일을 참조합니다. 이 특성은 이 샘플 코드 조각에서처럼 **Properties\AssemblyInfo.cs** 파일에서 선언할 수도 있습니다.

```csharp
[assembly: Application(Icon = "@drawable/icon")]
```

일반적으로 `using Android.App`은 **AssemblyInfo.cs**(`Application` 특성의 네임스페이스는 `Android.App`임)의 맨 위에 선언되지만, 아직 없는 경우 이 `using` 문을 추가해야 합니다.

<a name="Versioning" />

## <a name="version-the-application"></a>애플리케이션 버전 지정

버전 작업은 Android 애플리케이션 유지 관리와 배포를 위해 중요합니다. 일종의 버전 작업이 없으면 애플리케이션이 업데이트되어야 하는지 여부를 판단하기가 어렵습니다. 버전 작업을 지원하기 위해 Android는 두 가지 유형의 정보를 인식합니다. 

-   **버전 번호**&ndash; 애플리케이션의 버전을 나타내는 정수값(Android 및 애플리케이션에서 내부적으로 사용)입니다. 대부분의 애플리케이션은 이 값을 1로 설정한 다음 빌드마다 커집니다. 이 값은 버전 이름 특성과의 관련이나 선호 관계가 없습니다(아래 참조). 애플리케이션 및 게시 서비스는 이 값을 사용자에게 표시해서는 안 됩니다. 이 값은 **AndroidManifest.xml** 파일에 `android:versionCode`로 저장됩니다. 

-   **버전 이름**&ndash; 애플리케이션 버전에 대한 정보를 사용자에게 알리는 데만 사용되는 문자열입니다(특정 장치에 설치된 대로). 버전 이름은 사용자 또는 Google Play에 표시하기 위한 것입니다. 이 문자열은 Android에서 내부적으로 사용되지 않습니다. 버전 이름은 디바이스에 설치된 빌드를 사용자가 식별하는 데 도움이 되는 모든 문자열이 될 수 있습니다. 이 값은 **AndroidManifest.xml** 파일에 `android:versionName`으로 저장됩니다. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio에서는 다음 스크린 샷에서처럼 프로젝트 **속성**의 **Android 매니페스트** 섹션에서 이 값을 설정할 수 있습니다.

[![버전 번호 설정](images/vs/02-versioning-sml.png)](images/vs/02-versioning.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이러한 값은 다음 스크린 샷에서처럼 **프로젝트 옵션**의 **빌드 &gt; Android 애플리케이션** 섹션을 통해 설정할 수 있습니다.

[![버전 번호 설정](images/xs/02-versioning-sml.png)](images/xs/02-versioning.png#lightbox)

-----

<a name="shrink_apk" />

## <a name="shrink-the-apk"></a>APK 축소

불필요한 *관리* 코드를 제거하는 Xamarin.Android 링커와, 사용하지 않는 *Java 바이트코드*를 제거하는 Android SDK *ProGuard* 도구 조합을 통해 Xamarin.Android APK를 더 작게 만들 수 있습니다. 빌드 프로세스에서는 먼저 Xamarin.Android 링커를 사용하여 관리 코드(C#) 수준에서 앱을 최적화한 다음 ProGuard(사용하도록 설정된 경우)를 사용하여 Java 바이트코드 수준에서 APK를 최적화합니다.


### <a name="configure-the-linker"></a>링커 구성

릴리스 모드는 공유 런타임을 해제하고 연결을 실행하여 애플리케이션이 런타임 시 필요한 Xamarin.Android 부분만 탑재할 수 있게 합니다. Xamarin.Android의 *링커*는 정적 분석을 사용하여 Xamarin.Android 애플리케이션에서 사용하거나 참조하는 어셈블리, 형식 및 형식 번호를 결정합니다. 그런 다음 링커는 사용되지 않는(또는 참조되지 않는) 모든 어셈블리, 형식 및 멤버를 버립니다. 이렇게 해서 패키지 크기를 상당히 줄일 수 있습니다. 예를 들어 [HelloWorld](~/android/deploy-test/linker.md) 샘플에서는 최종 APK 크기가 83% 줄었습니다. 

-   구성: None &ndash; Xamarin.Android 4.2.5 Size = 17.4 MB.

-   구성: SDK Assemblies Only &ndash; Xamarin.Android 4.2.5 Size = 3.0 MB.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

프로젝트 **속성**의 **Android 옵션** 섹션을 통해 링커 옵션을 설정합니다.

[![링커 옵션](images/vs/03-linking-sml.png)](images/vs/03-linking.png#lightbox)

**연결** 풀다운 메뉴는 링커 제어를 위한 다음 옵션을 제공합니다.

-   **없음** &ndash; 링커가 해제되고 연결이 수행되지 않습니다.

-   **SDK 어셈블리만** &ndash; [Xamarin.Android에서 필요한](~/cross-platform/internals/available-assemblies.md) 어셈블리만 연결합니다. 
    다른 어셈블리는 연결되지 않습니다.

-   **Sdk 및 사용자 어셈블리**&ndash; Xamarin.Android뿐 아니라 애플리케이션에서 필요한 모든 어셈블리를 연결합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

링커 옵션은 다음 스크린 샷에서처럼 **프로젝트 옵션**의 **Android 빌드** 섹션에 있는 **링커** 탭을 통해 설정합니다.

[![링커 옵션](images/xs/03-linking-sml.png)](images/xs/03-linking.png#lightbox)

링커 제어를 위한 옵션은 다음과 같습니다.

-   **연결 안 함** &ndash; 링커가 해제되고 연결이 수행되지 않습니다.

-   **SDK 어셈블리만 연결** &ndash; [Xamarin.Android에서 필요한](~/cross-platform/internals/available-assemblies.md) 어셈블리만 연결합니다. 다른 어셈블리는 연결되지 않습니다.

-   **모든 어셈블리**&ndash; Xamarin.Android뿐 아니라 애플리케이션에서 필요한 모든 어셈블리를 연결합니다.

-----

연결에는 의도치 않은 부작용이 있을 수 있으므로 물리적 장치의 릴리스 모드에서 애플리케이션을 다시 테스트하는 것이 중요합니다.

### <a name="proguard"></a>ProGuard

*ProGuard*는 Java 코드를 연결하고 난독 처리하는 Android SDK 도구입니다. ProGuard는 일반적으로 APK에 포함된 대형 라이브러리(예: Google Play Services)의 공간을 축소하여 애플리케이션을 더 작게 만드는 데 사용됩니다. ProGuard는 사용되지 않는 Java 바이트 코드를 제거하기 때문에 앱이 더 작아집니다. 예를 들어, 소형 Xamarin.Android 앱에 ProGuard를 사용하면 크기가 약 24% 감소합니다. &ndash; 여러 라이브러리 종속성이 있는 대형 앱에 ProGuard를 사용하면 보통 더 큰 축소 효과가 있습니다. 

ProGuard는 Xamarin.Android 링커를 대체하지 않습니다. Xamarin.Android 링커는 *관리* 코드를 연결하고 ProGuard는 Java 바이트 코드를 연결합니다. 빌드 프로세스에서는 먼저 Xamarin.Android 링커를 사용하여 관리 코드(C#) 수준에서 앱을 최적화한 다음 ProGuard(사용하도록 설정된 경우)를 사용하여 Java 바이트 코드 수준에서 APK를 최적화합니다. 

**ProGuard 사용**을 선택하면 Xamarin.Android이 나타나는 APK에서 ProGuard 도구를 실행합니다. ProGuard 구성 파일이 생성되며 빌드 시점에 ProGuard에서 사용됩니다. Xamarin.Android는 사용자 지정*ProguardConfiguration* 빌드 작업도 지원합니다. 사용자 지정 ProGuard 구성 파일을 프로젝트에 추가하고 아래 예제에서처럼 마우스 오른쪽 단추로 클릭하여 빌드 작업으로 선택할 수 있습니다.  

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Proguard 빌드 작업](images/vs/05-proguard-build-action-sml.png)](images/vs/05-proguard-build-action.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Proguard 빌드 작업](images/xs/05-proguard-build-action-sml.png)](images/xs/05-proguard-build-action.png#lightbox)

-----

ProGuard는 기본적으로 사용하지 않게 설정되어 있습니다. **ProGuard 사용** 옵션은 프로젝트가 **릴리스** 모드로 설정된 경우에만 사용할 수 있습니다. **ProGuard 사용**을 선택하지 않았다면 모든 ProGuard 빌드 작업이 무시됩니다. Xamarin.Android ProGuard 구성은 APK를 난독 처리하지 않으며 사용자 지정 구성 파일을 통해서도 난독 처리를 사용할 수 없습니다. 난독 처리를 사용려면 [Dotfuscator를 통한 애플리케이션 보호](~/android/deploy-test/release-prep/index.md#dotfuscator)를 참조하세요. 

ProGuard 도구 사용에 대한 자세한 정보는 [ProGuard](~/android/deploy-test/release-prep/proguard.md)를 참조하세요.

<a name="protect_app" />

## <a name="protect-the-application"></a>애플리케이션 보호

<a name="Disable_Debugging" />

### <a name="disable-debugging"></a>디버깅 사용 안 함

Android 애플리케이션 개발 중에는 *JDWP(Java Debug Wire Protocol)* 를 사용하여 디버그를 수행합니다. 이것은 **adb** 같은 도구가 디버그를 위해 JVM과 통신할 수 있게 하는 기술입니다. JDWP는 Xamarin.Android 애플리케이션의 디버그 빌드에 대해 기본적으로 켜져 있습니다.  JDWP는 개발 중에 중요하지만 릴리스된 애플리케이션에는 보안 문제를 야기할 수 있습니다. 

> [!IMPORTANT]
> 디버그 상태를 사용하지 않게 설정하지 않은 경우 Java 프로세스에 완전히 액세스할 수 있고 애플리케이션의 컨텍스트에서 임의 코드를 실행할 수 있으므로(JDWP를 통해 가능) 릴리스된 애플리케이션에서는 디버그 상태를 항상 사용하지 않게 설정합니다.

Android 매니페스트에는 애플리케이션의 디버그 가능 여부를 제어하는 `android:debuggable` 특성이 포함됩니다. `android:debuggable` 특성은 `false`로 설정하는 것이 좋습니다. 이것은 조건부 컴파일 문을 **AssemblyInfo.cs**에 추가하면 간단하게 설정할 수 있습니다. 

```csharp
#if DEBUG
[assembly: Application(Debuggable=true)]
#else
[assembly: Application(Debuggable=false)]
#endif
```

더비그 빌드에서는 디버그 편의를 위해 자동으로 일부 권한을 설정합니다(예: **인터넷** 및 **ReadExternalStorage**). 하지만 릴리스 빌드는 명시적으로 구성된 권한만 사용합니다. 릴리스 빌드로 전환하면 앱이 디버그 빌드에서 사용 가능했던 권한을 잃게 되는 경우 **권한**에서 설명한 것처럼 [필수 권한](~/android/app-fundamentals/permissions.md)에서 이 권한을 명시적으로 사용하도록 설정했는지 확인합니다. 

<a name="dotfuscator" id="dotfuscator" />

### <a name="application-protection-with-dotfuscator"></a>Dotfuscator를 통한 애플리케이션 보호

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[디버깅을 사용하지 않는](#Disable_Debugging) 경우에도 공격자가 애플리케이션을 다시 패키지하고, 구성 옵션이나 권한을 추가 또는 제거할 가능성은 여전히 남아 있습니다. 이를 통해 애플리케이션의 리버스 엔지니어링, 디버그 또는 변조가 가능해집니다.
[Dotfuscator CE(Community Edition)](https://www.preemptive.com/products/dotfuscator/overview)를 사용하여 관리 코드를 난독 처리하고, Xamarin.Android 앱이 루트 디바이스에서 실행되고 있는지 검색하고 응답하기 위해 빌드 시간에 런타임 보안 상태 검색 코드를 이 앱에 삽입할 수 있습니다.

Dotfuscator CE는 Visual Studio 2017에 포함되어 있습니다.
Dotfuscator를 사용하려면 **도구 > PreEmptive Protection - Dotfuscator**를 클릭합니다.

Dotfuscator CE를 구성하려면 [Xamarin에서 Dotfuscator Community Edition 사용](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator)을 참조하세요.
구성된 후에는 Dotfuscator CE가 만들어진 각 빌드를 자동으로 보호합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[디버깅을 사용하지 않는](#Disable_Debugging) 경우에도 공격자가 애플리케이션을 다시 패키지하고, 구성 옵션이나 권한을 추가 또는 제거할 가능성은 여전히 남아 있습니다. 이를 통해 애플리케이션의 리버스 엔지니어링, 디버그 또는 변조가 가능해집니다.
Mac용 Visual Studio는 지원하지 않지만 Visual Studio에서 [Dotfuscator CE(Community Edition)](https://www.preemptive.com/products/dotfuscator/overview)를 사용하여 관리 코드를 난독 처리하고, Xamarin.Android 앱이 루트 디바이스에서 실행되고 있는지 검색하고 응답하기 위해 빌드 시간에 런타임 보안 상태 검색 코드를 이 앱에 삽입할 수 있습니다.

Dotfuscator CE를 구성하려면 [Xamarin에서 Dotfuscator Community Edition 사용](https://www.preemptive.com/obfuscating-xamarin-with-dotfuscator)을 참조하세요.
구성된 후에는 Dotfuscator CE가 만들어진 각 빌드를 자동으로 보호합니다.

-----

<a name="bundle" />

### <a name="bundle-assemblies-into-native-code"></a>어셈블리를 네이티브 코드에 번들

이 옵션을 사용할 경우 어셈블리가 네이티브 공유 라이브러리에 번들로 포함됩니다. 이 옵션은 코드를 안전하게 유지합니다. 즉 네이티브 바이너리에 관리 어셈블리를 포함하여 보호합니다.

이 옵션을 사용하려면 엔터프라이즈 라이선스가 필요하며 **빠른 배포 사용**을 사용하지 않도록 설정한 경우에만 사용할 수 있습니다. **어셈블리를 네이티브 코드에 번들**은 기본적으로 사용하지 않게 설정되어 있습니다.

**네이티브 코드에 번들** 옵션은 어셈블리가 네이티브 코드로 컴파일 되는 것이 *아닙니다*. [**AOT 컴파일**](#aot)을 사용하여 어셈블리를 네이티브 코드로 컴파일할 수 없습니다(현재 테스트 기능이며 프로덕션에 사용할 수 없음).

<a name="aot" />

### <a name="aot-compilation"></a>AOT 컴파일

**AOT 컴파일** 옵션([패키지 속성](#Set_Packaging_Properties) 페이지)을 사용하면 어셈블리의 AOT(Ahead-of-Time) 컴파일이 가능합니다. 이 옵션을 사용하면 런타임 이전에 어셈블리를 미리 컴파일하여 JIT(Just In Time) 시작 과부하가 최소화됩니다. 나타나는 네이티브 코드는 컴파일되지 않은 어셈블리와 함께 APK에 포함됩니다. 이로 인해 애플리케이션 시작 시간이 짧아지며 APK 크기는 약간 커집니다.

**AOT 컴파일** 옵션에는 엔터프라이즈 이상의 라이선스가 필요합니다. **AOT 컴파일**은 프로젝트가 릴리스 모드로 구성된 경우에만 사용할 수 있고 기본적으로 사용하지 않게 설정되어 있습니다. AOT에 대한 자세한 내용은 [AOT](https://www.mono-project.com/docs/advanced/aot/)를 참조하세요.

#### <a name="llvm-optimizing-compiler"></a>LLVM 최적화 컴파일러

_LLVM 최적화 컴파일러_는 더 작고 빠른 컴파일 코드를 만들며 AOT 컴파일 어셈블리를 네이티브 코드로 변환하지만 빌드 시간이 느려집니다. LLVM 컴파일러는 기본적으로 사용하지 않게 설정되어 있습니다. LLVM 컴파일러를 사용하려면 먼저 **AOT 컴파일** 옵션을 사용하도록 설정해야 합니다([패키지 속성](#Set_Packaging_Properties) 페이지).


> [!NOTE]
> **LLVM 최적화 컴파일러** 옵션에는 엔터프라이즈 라이선스가 필요합니다.  

<a name="Set_Packaging_Properties" />

## <a name="set-packaging-properties"></a>패키지 속성 설정

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

다음 스크린 샷에서처럼 프로젝트 **속성**의 **Android 옵션** 섹션에서 패키지 속성을 설정할 수 있습니다.

[![패키징 속성](images/vs/04-packaging-sml.png)](images/vs/04-packaging.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

다음 스크린 샷에서처럼 **프로젝트 옵션**에서 패키지 속성을 설정할 수 있습니다.

[![패키징 속성](images/xs/04-packaging-sml.png)](images/xs/04-packaging.png#lightbox)

-----

**공유 런타임 사용**, **빠른 배포 사용** 등과 같이 이러한 여러 속성은 디버그 모드용입니다. 그러나 애플리케이션이 릴리스 모드용으로 구성되었을 때 앱이 [크기 및 실행 속도를 위해 최적화되고](#shrink_apk), [변조를 방지하며](#protect_app), 다양한 아키텍처 및 크기 제한을 지원하도록 패키징할 수 있는 방법을 결정하는 다른 설정이 있습니다.

### <a name="specify-supported-architectures"></a>지원되는 아키텍처 지정

Xamarin.Android 앱의 릴리스를 준비할 때는 지원되는 CPU 아키텍처를 지정해야 합니다. 단일 APK가 여러 서로 다른 아키텍처를 지원하는 머신 코드를 포함할 수 잇습니다. 여러 CPU 아키텍처 지원에 대한 세부 정보는 [CPU 아키텍처](~/android/app-fundamentals/cpu-architectures.md)를 참조하세요.

### <a name="generate-one-package-apk-per-selected-abi"></a>선택한 ABI마다 한 패키지(.APK) 생성

이 옵션을 사용하는 경우 모든 지원되는 ABI에 대해 대형 단일 APK를 만드는 것이 아니라, 지원되는 ABI 각각에 대해 하나의 APK가 만들어집니다([CPU 아키텍처](~/android/app-fundamentals/cpu-architectures.md)에서 설명한 것처럼 **고급** 탭). 이 옵션은 프로젝트가 릴리스 모드로 구성되었고 기본적으로 사용하지 않게 설정된 경우에만 사용할 수 있습니다.

### <a name="multi-dex"></a>Multi-Dex

**Multi-Dex 사용** 옵션을 사용하도록 선택한 경우 Android SDK 도구를 사용하여 **.dex** 파일 형식의 65K 메서드 제한을 무시하게 됩니다. 65k로 메서드 제한은 앱이 _참조_하는 Java 메서드 수(앱이 사용하는 모든 라이브러리의 항목 포함)&ndash;를 기준으로 하며, _원본 코드에 작성된_ 메서드 수는 기준이 아닙니다. 애플리케이션이 일부 메서드만 정의하고 여러 메서드(또는 대규모 라이브러리)를 사용하는 경우 65K 제한이 초과될 수 있습니다.

앱이 참조되는 모든 라이브러리의 모든 메서드를 사용하는 것은 아니므로 ProGuard 같은 도구(위 참조)를 사용하면 사용되지 않는 메서드를 코드에서 제거할 수 있습니다. **Multi-Dex 사용**은 절대적으로 필요한 경우에만 사용하도록 설정합니다. 즉 ProGuard를 사용한 후에도 앱이 65K 이상의 Java 메서드를 참조하는 경우입니다.

Multi-Dex에 대한 자세한 내용은 [64K가 넘는 메서드의 앱 구성](https://developer.android.com/tools/building/multidex.html)을 참조하세요.

<a name="Compile" />

## <a name="compile"></a>Compile

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

위의 단계가 모두 완료되면 앱을 컴파일할 수 있습니다. **빌드 > 솔루션 다시 빌드**를 선택하여 릴리스 모드에서 성공적으로 빌드되는지 확인합니다. 이 단계에서는 아직 APK가 생성되지 않았습니다.

[앱 패키지 서명](~/android/deploy-test/signing/index.md)에서 패키지와 서명을 더 상세하게 설명합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

위의 단계가 모두 완료되면 애플리케이션을 빌드하여(**빌드 &gt; 모두 빌드**) 릴리스 모드에서 성공적으로 빌드되는지 확인합니다. 이 단계에서는 아직 APK가 생성되지 않았습니다.

-----

<a name="archive" />

## <a name="archive-for-publishing"></a>게시를 위해 보관

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

게시 프로세스를 시작하려면 **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고 **보관...**  바로 가기 메뉴 항목을 선택합니다.

[![앱 보관](images/vs/07-archive-for-publishing-sml.png)](images/vs/07-archive-for-publishing.png#lightbox)

**보관...** 은 **Archive Manager**를 실행하고 이 스크린 샷에서처럼 앱 번들을 보관하는 프로세스를 시작합니다.

[![보관 관리자](images/vs/08-archive-manager-sml.png)](images/vs/08-archive-manager.png#lightbox)

보관 파일을 만드는 또 다른 방법은 **솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭하고 **모두 보관...** 을 선택하는 것입니다. 그러면 솔루션을 빌드하고 보관 파일을 생성할 수 있는 모든 Xamarin 프로젝트를 보관합니다.

[![모두 보관](images/vs/09-archive-all-sml.png)](images/vs/09-archive-all.png#lightbox)

**보관** 및 **모두 보관**에서는 모두 **Archive Manager**가 자동으로 시작됩니다. **Archive Manager**를 직접 시작하려면 **도구 > Archive Manager...** 메뉴 항목을 클릭합니다.

[![보관 관리자 시작](images/vs/10-launch-archive-manager-sml.png)](images/vs/10-launch-archive-manager.png#lightbox)

**솔루션** 노드를 마우스 오른쪽 단추로 클릭하고 **보관 보기**를 선택하면 언제든 솔루션의 보관 파일을 볼 수 있습니다.

[![보관 파일 보기](images/vs/11-view-archives-sml.png)](images/vs/11-view-archives.png#lightbox)

### <a name="the-archive-manager"></a>Archive Manager

**Archive Manager**는 **솔루션 목록** 창, **보관 목록**, **세부 정보 패널**로 구성됩니다.

[![보관 관리자 창](images/vs/12-archive-manager-detail-sml.png)](images/vs/12-archive-manager-detail.png#lightbox)

**솔루션 목록**은 보관된 프로젝트가 하나 이상 있는 모든 솔루션을 표시합니다. **솔루션 목록**에는 다음 섹션이 포함됩니다.

* **현재 솔루션** &ndash; 현재 솔루션을 표시합니다. 현재 솔루션에 기존 보관 파일이 없으면 이 영역이 비었을 수 있습니다.
* **모든 보관** &ndash; 보관 파일이 있는 모든 솔루션을 표시합니다.
* **검색** 텍스트 상자(위) &ndash; 텍스트 상자에 입력한 검색 문자열에 따라 **모든 보관** 목록에 나열된 솔루션을 필터링합니다.

**보관 목록** 선택한 솔루션에 대한 모든 보관 파일 목록을 표시합니다. **보관 목록**에는 다음 섹션이 포함됩니다.

* **솔루션 이름 선택** &ndash; **솔루션 목록**에서 선택한 솔루션의 이름을 표시합니다. **보관 목록**에 표시된 모든 정보는 이 선택한 솔루션에 대한 것입니다.
* **플랫폼 필터** &ndash; 이 필드를 통해 플랫폼 종류(예: iOS 또는 Android)에 따라 보관 파일을 필터링할 수 있습니다.
* **항목 보관** &ndash; 선택한 솔루션의 보관 파일의 목록입니다. 이 목록의 각 항목에는 프로젝트 이름, 만든 날짜 및 플랫폼이 포함되어 있습니다. 항목이 보관 또는 게시 중인 동안 진행률 같은 추가 정보를 표시할 수도 있습니다.

**세부 정보 패널** 각 보관 파일에 대한 추가 정보를 표시합니다. 이를 통해 사용자가 배포 워크플로를 시작하거나 배포가 만들어진 폴더를 열 수 있습니다. **빌드 설명** 섹션을 사용하면 보관 파일에 빌드 설명을 포함할 수 있습니다.

### <a name="distribution"></a>분포

애플리케이션의 보관 버전을 게시할 준비가 되면 **Archive Manager**에서 보관 파일을 선택하고 **배포...** 단추를 클릭합니다.

[![배포 단추](images/vs/13-distribute-sml.png)](images/vs/13-distribute.png#lightbox)

**배포 채널** 대화 상자에는 앱 관련 정보, 배포 워크플로 진행률 표시, 배포 채널 선택이 나타납니다. 첫 번째 실행에서는 두 가지 선택 항목이 표시됩니다.

[![배포 채널 선택](images/vs/14-distribution-channel-sml.png)](images/vs/14-distribution-channel.png#lightbox)

다음 배포 채널 중 하나를 선택할 수 있습니다.

* **임시**&ndash; 서명된 APK를 Android 디바이스에 사이드로드할 수 있는 디스크에 저장합니다. 계속하여 [앱 패키지 서명](~/android/deploy-test/signing/index.md)에서 Android 서명 ID를 만들고, Android 애플리케이션용 새 서명 인증서를 만들며, _임시_ 앱 버전을 디스크에 게시하는 방법을 알아봅니다. 테스트를 위한 APK를 만드는 좋은 방법입니다.

* **Google Play** &ndash; 서명된 APK를 Google Play에 게시합니다. 계속하여 [Google Play에 게시](~/android/deploy-test/publishing/publishing-to-google-play/index.md)에서 APK를 서명하여 Google Play 스토어에 게시하는 방법을 알아봅니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

게시 프로세스를 시작하려면 **빌드 > 게시를 위해 보관**을 선택합니다.

[![게시를 위해 보관](images/xs/07-archive-for-publishing-sml.png)](images/xs/07-archive-for-publishing.png#lightbox)

**게시를 위해 보관**은 프로젝트를 빌드하고 보관 파일에 번들로 묶습니다. **모두 보관** 메뉴 선택은 솔루션에서 보관 가능한 모든 프로젝트를 보관하게 됩니다. 두 경우 모두 빌드 및 번들 작업이 완료되면 자동으로 **Archive Manager**가 열립니다.

[![보관 파일 보기](images/xs/08-archives-view-sml.png)](images/xs/08-archives-view.png#lightbox)

이 예제에서는 **Archive Manager**가 하나의 보관된 애플리케이션인 **MyApp**만 나열합니다. 설명 필드를 통해 짧은 설명을 보관파일과 함께 저장할 수 있습니다. Xamarin.Android 애플리케이션의 보관된 버전을 게시하려면 **Archive Manager**에서 앱을 선택하고 다음과 같이 **서명 및 배포...** 를 클릭합니다. 나타나는 **서명 및 배포** 대화 상자에는 두 가지 선택 항목이 있습니다.

[![서명 및 배포](images/xs/09-sign-and-distribute-sml.png)](images/xs/09-sign-and-distribute.png#lightbox)

여기에서는 배포 채널을 선택할 수 있습니다.

-   **임시**&ndash; 서명된 APK를 Android 디바이스에 사이드로드할 수 있게 디스크에 저장합니다. 계속하여 [앱 패키지 서명](~/android/deploy-test/signing/index.md)에서 Android 서명 ID를 만들고, Android 애플리케이션용 새 서명 인증서를 만들며, &ldquo;임시&rdquo; 앱 버전을 디스크에 게시하는 방법을 알아봅니다. 테스트를 위한 APK를 만드는 좋은 방법입니다.


-   **Google Play** &ndash; 서명된 APK를 Google Play에 게시합니다.
    계속하여 [Google Play에 게시](~/android/deploy-test/publishing/publishing-to-google-play/index.md)에서 APK를 서명하여 Google Play 스토어에 게시하는 방법을 알아봅니다.

-----

## <a name="related-links"></a>관련 링크

- [다중 코어 디바이스 및 Xamarin.Android](~/android/deploy-test/multicore-devices.md)
- [CPU 아키텍처](~/android/app-fundamentals/cpu-architectures.md)
- [AOT](https://www.mono-project.com/docs/advanced/aot/)
- [코드 및 리소스 축소 ](https://developer.android.com/tools/help/proguard.html)
- [64K가 넘는 메서드의 앱 구성](https://developer.android.com/tools/building/multidex.html)
