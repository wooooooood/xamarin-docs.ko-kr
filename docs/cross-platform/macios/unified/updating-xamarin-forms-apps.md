---
title: "Xamarin.Forms는 기존 앱을 업데이트합니다."
description: "및 버전 1.3.1 업데이트 통합 API를 사용 하는 기존 Xamarin.Forms 응용 프로그램을 업데이트 하려면 다음이 단계를 수행 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 52b53618e23a47884bee6cb821d85b15d759968c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="updating-existing-xamarinforms-apps"></a>Xamarin.Forms는 기존 앱을 업데이트합니다.

_및 버전 1.3.1 업데이트 통합 API를 사용 하는 기존 Xamarin.Forms 응용 프로그램을 업데이트 하려면 다음이 단계를 수행 합니다._


> [!IMPORTANT]
> Xamarin.Forms 1.3.1 첫 번째 릴리스를 지 원하는 통합 API 이기 때문에 전체 솔루션을 통합 하는 iOS 앱을 마이그레이션할 때와 동시에 최신 버전을 사용 하도록 업데이트 되어야 합니다. 즉, 통합 지원에 대 한 iOS 프로젝트를 업데이트 하는 것 외에도 것도 할 경우에서 코드를 편집할 _모든_ 솔루션의 프로젝트입니다.



이 업데이트는 두 단계로 수행 됩니다.

1. 마이그레이션 도구에서 Mac의 빌드에 대 한 Visual Studio를 사용 하는 통합 API에는 iOS 앱을 마이그레이션하십시오.

    - 마이그레이션 도구를 사용 하 여 프로젝트를 자동으로 업데이트 해야 합니다.

    - 업데이트 iOS 네이티브 Api에 대 한 지침에 설명 된 대로 [iOS 앱 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md) 특히 사용자 지정 렌더러 또는 종속성 서비스 코드에서.

2. 버전 1.3 xamarin.forms 전체 솔루션을 업데이트 합니다.

    1. 1.3.1 Xamarin.Forms NuGet 패키지를 설치 합니다.

    2. 업데이트는 `App` 공유 코드의 클래스.

    3. 업데이트는 `AppDelegate` iOS 프로젝트에 있습니다.

    4. 업데이트는 `MainActivity` Android 프로젝트에서입니다.

    5. 업데이트는 `MainPage` Windows Phone 프로젝트에 있습니다.


# <a name="1-ios-app-unified-migration"></a>1입니다. iOS 앱 (Unified 마이그레이션)

마이그레이션 중 Xamarin.Forms 1.3 지 원하는 버전을 통합 API로 업그레이드 해야 합니다. 을 올바른 어셈블리 참조를 만들 수 있도록 먼저 통합 API를 사용 하 여 iOS 프로젝트를 업데이트 해야 합니다.

## <a name="migration-tool"></a>마이그레이션 도구

IOS 프로젝트를 클릭 하 여 선택 되어 선택 **프로젝트 > Xamarin.iOS 통합 API로 마이그레이션 중...**  표시 되는 경고 메시지에 동의 해야 합니다.

![](updating-xamarin-forms-apps-images/beta-tool1.png "프로젝트를 선택 > Xamarin.iOS 통합 API... 마이그레이션하고 표시 되는 경고 메시지에 동의")

이 자동으로:

 - 통합 64 비트 API를 지원 하기 위해 프로젝트 형식을 변경 합니다.
 - 프레임 워크 참조를 변경 **Xamarin.iOS** (이전 교체 **monotouch** 참조).
 - 네임 스페이스 참조를 제거 하려면 코드에서 변경 된 `MonoTouch` 접두사입니다.
 - 업데이트는 **csproj** 통합 API에 대 한 올바른 빌드 대상을 사용 하는 파일입니다.

**클린** 및 **빌드** 프로젝트 문제를 해결 하려면 다른 오류가 있는지 확인 하십시오. 추가 작업이 없으므로 필요 하지 않습니다. 다음이 단계에서 보다 자세히 설명 된 [통합 API 문서](~/cross-platform/macios/unified/updating-ios-apps.md)합니다.

## <a name="update-native-ios-apis-if-required"></a>(필요한 경우) 네이티브 iOS Api 업데이트

추가 iOS 네이티브 코드 (예: 사용자 지정 렌더러 또는 종속성 서비스)를 추가한 경우 추가 수동 코드 문제를 해결 해야 합니다. 다시 앱을 컴파일 및 참조는 [업데이트 기존 iOS 앱 지침](~/cross-platform/macios/unified/updating-ios-apps.md) 필요할 수 있는 변경 내용에 대 한 추가 정보에 대 한 합니다. [이러한 팁](~/cross-platform/macios/unified/updating-tips.md) 필요한 변경 내용을 식별도 도움이 됩니다.


# <a name="2-xamarinforms-131-update"></a>2. Xamarin.Forms 1.3.1 Update

IOS 앱 통합 API를 업데이트 한 후 솔루션의 나머지 버전 1.3.1 xamarin.forms 업데이트 해야 합니다. 여기에는 다음이 포함됩니다.

 - 각 프로젝트의 Xamarin.Forms NuGet 패키지를 업데이트합니다.
 - 새 Xamarin.Forms를 사용 하도록 코드를 변경 `Application`, `FormsApplicationDelegate` (iOS) `FormsApplicationActivity` (Android) 및 `FormsApplicationPage` (Windows Phone) 클래스입니다.

이러한 단계는 아래 설명 되어 있습니다.


## <a name="21-update-nuget-in-all-projects"></a>2.1 모든 프로젝트에서 NuGet 업데이트

Xamarin.Forms 1.3.1 업데이트 솔루션의 모든 프로젝트에 대 한 NuGet 패키지 관리자를 사용 하 여 시험판: PCL (있는 경우), iOS, Android 및 Windows Phone 합니다. 것이 좋습니다 했는지 **삭제 하 고 다시 추가** Xamarin.Forms NuGet 패키지 버전 1.3 업데이트 합니다.

**참고:** Xamarin.Forms 버전 1.3.1 문서가 현재 *시험판*합니다. 즉, 선택 해야 합니다는 **시험판** 최신 시험판 버전을 보려면 (눈금-상자 Mac 용 Visual Studio에서) 또는 드롭 목록에서 Visual Studio를 통해 NuGet 옵션입니다.


> [!IMPORTANT]
> Visual Studio를 사용 하는 경우 최신 버전의 NuGet 패키지 관리자가 설치 되어 있는지 확인 합니다. 이전 버전의 Visual Studio에서 NuGet 통합 버전 1.3.1 Xamarin.Forms의 올바르게 설치 되지 않습니다. 로 이동 **도구 > 확장 및 업데이트 중...**  에서 클릭 하 고는 **설치 됨** 있는지 여부를 확인 하려면 목록에서 **Visual Studio 용 NuGet 패키지 관리자** 적어도 2.8.5 버전. 오래 된 것 이면 클릭는 **업데이트** 최신 버전을 다운로드 하는 목록입니다.



1.3.1 xamarin.forms NuGet 패키지를 업데이트 한 후 다음과 같이 변경 새로 업그레이드 하는 각 프로젝트에 `Xamarin.Forms.Application` 클래스입니다.

## <a name="22-portable-class-library-or-shared-project"></a>2.2 이식 가능한 클래스 라이브러리 (또는 공유 프로젝트)

변경 된 **App.cs** 파일 있도록:

 - `App` 에서 이제 클래스를 상속 `Application`합니다.
 - `MainPage` 속성을 표시 하려면 첫 번째 콘텐츠 페이지로 설정 합니다.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

완전히 제거 했는지는 `GetMainPage` 메서드를 대신 설정 하 고는 `MainPage` *속성* 에 `Application` 하위 클래스입니다.

이 새로운 `Application` 기본 클래스도 지원 합니다.는 `OnStart`, `OnSleep`, 및 `OnResume` 재정의 응용 프로그램의 수명 주기를 관리할 수 있도록 합니다.

`App` 클래스 전달 되어 새 `LoadApplication` 아래 설명 된 대로 각 응용 프로그램 프로젝트에서 메서드:


## <a name="23-ios-app"></a>2.3 iOS 앱


변경 된 **AppDelegate.cs** 파일 있도록:

 - 클래스에서 상속 `FormsApplicationDelegate` (대신 `UIApplicationDelegate` 이전에).
 - `LoadApplication` 새 인스턴스를 사용 하 여 호출은 `App`합니다.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```


## <a name="23-android-app"></a>2.3 android 앱

변경 된 **MainActivity.cs** 파일 있도록:

 - 클래스에서 상속 `FormsApplicationActivity` (대신 `FormsActivity` 이전에).
 - `LoadApplication` 새 인스턴스를 사용 하 여 호출 됩니다. `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```


## <a name="24-windows-phone-app"></a>2.4 Windows Phone 앱

업데이트 해야는 **MainPage** -XAML 및 코드 숨김 파일 모두.

변경 된 **MainPage.xaml** 파일 있도록:

 - 루트 XAML 요소 이어야 합니다. `winPhone:FormsApplicationPage`합니다.
 - `xmlns:phone` 특성 해야 *변경* 를 `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

업데이트 된 예제는 다음과 같습니다-편집 (나머지 특성은 동일 함) 이러한 것 들만 있어야 합니다.

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```


변경 된 **MainPage.xaml.cs** 파일 있도록:

 - 클래스에서 상속 `FormsApplicationPage` (대신 `PhoneApplicationPage` 이전에).
 - `LoadApplication` Xamarin.Forms는의 새 인스턴스를 사용 하 여 호출은 `App` 클래스입니다. Windows Phone 있으므로이 참조를 정규화 해야 할 수 자신의 `App` 이미 정의 된 클래스입니다.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

## <a name="troubleshooting"></a>문제 해결

경우에 따라 Xamarin.Forms NuGet 패키지를 업데이트 한 후 다음과 비슷한 오류가 표시 됩니다. NuGet 업데이트 프로그램에서 이전 버전에 대 한 참조를 완전히 제거 하지 않습니다 경우 프로그램 **csproj** 파일입니다.

>프로그램\_PROJECT.csproj: 오류:이 프로젝트는이 컴퓨터에서 누락 된 NuGet 패키지를 참조 합니다. 다운로드 하려면 NuGet 패키지 복원을 사용 하도록 설정 합니다.  자세한 내용은 http://go.microsoft.com/fwlink/?LinkID=322105를 참조 하십시오. 누락 된 파일은... /.. /packages/Xamarin.Forms.1.2.3.6257/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets 합니다. (프로그램\_프로젝트)

이러한 오류를 해결 하려면 엽니다는 **csproj** 텍스트 편집기에서 파일을 찾아보십시오 `<Target` 아래에 표시 된 요소와 같은 이전 버전의 Xamarin.Forms 참조 하는 요소입니다. 이 전체 요소를 수동으로 삭제 해야는 **csproj** 파일 및 변경 내용을 저장 합니다.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

이러한 이전 참조가 제거 되 면 프로젝트가 성공적으로 빌드되어야 합니다.

# <a name="considerations"></a>고려 사항

해당 응용 프로그램 구성 요소 또는 NuGet 패키지를 하나 이상에 의존 하는 경우 새 통합 API 클래식 API에서 기존 Xamarin.Forms 프로젝트 변환할 때 다음 사항을 고려 고려해 취해야 합니다.

## <a name="components"></a>구성 요소

응용 프로그램에 포함 된 모든 구성 요소는 통합 API를 업데이트 해야 합니다 또는 컴파일하려고 할 때 충돌이 발생 합니다. 포함 된 모든 구성 요소에 대 한 현재 버전에서 지 원하는 통합 API Xamarin 구성 요소 저장소에서 새 버전으로 교체 하 고 깨끗 한 빌드를 수행 합니다. 작성자가 아직 변환 하지 않은 모든 구성 요소는 32 비트 구성 요소 저장소에만 경고를 표시 합니다.


## <a name="nuget-support"></a>NuGet 지원

NuGet 통합 API 지원을 사용 하도록 변경을 제공한 것 동안 없었습니다 NuGet의 새로운 릴리스가 하므로 새 Api를 인식 하는 NuGet을 가져오는 방법을 평가 하는 것입니다.

그 전 까지는 구성 요소와 마찬가지로 통합 Api를 지 원하는 버전으로 프로젝트에 포함 한 모든 NuGet 패키지를 전환 하 고 깨끗 한 빌드를 나중에 실행 해야 합니다.

> [!IMPORTANT]
> **참고:** 형태로 오류가 있는 경우 _"오류 3 동일한 Xamarin.iOS 프로젝트에 'monotouch.dll'와 'Xamarin.iOS.dll'를 포함할 수 없습니다-'monotouch.dll'에서 참조 하는 동안 'Xamarin.iOS.dll' 명시적으로 참조 되 ' xxx, 버전 = 0.0.000, Culture = neutral, PublicKeyToken = null'"_ 후 통합 Api에 응용 프로그램으로 변환에 일반적으로 인해 구성 요소 또는 NuGet 패키지는 통합 API에 업데이트 되지 않은 프로젝트에서. 제거 하려면 기존 구성 요소/NuGet 통합 Api를 지 원하는 버전으로 업데이트 고 깨끗 한 빌드를 수행 해야 합니다.




# <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS 앱의 빌드를 64 비트를 사용 하도록 설정

Xamarin.iOS 모바일 응용 프로그램의 통합 API로 변환 된 경우 개발자는 여전히 응용 프로그램의 옵션 중에서 64 비트 컴퓨터에 대 한 응용 프로그램의 빌드를 사용 하도록 설정 해야 합니다. 참조 하십시오는 **활성화 64 비트 빌드의 Xamarin.iOS 앱** 의 [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64.md#enable-64) 64 비트를 사용 하도록 설정 하면 자세한 지침에 대 한 문서를 작성 합니다.

# <a name="summary"></a>요약

이제 버전 1.3.1으로 Xamarin.Forms 응용 프로그램을 업데이트 해야 하 고 iOS 앱 (iOS 플랫폼에서 64 비트 아키텍처를 지원)이 표시 하는 통합 API로 마이그레이션.

위에서 언급 했 듯이 Xamarin.Forms 앱에 사용자 지정 렌더러와 같은 네이티브 코드 또는 이러한도 업데이트 되어야 하는 새 형식을 사용 하도록 다음 종속성 서비스 [통합 API에 도입 된](~/cross-platform/macios/index.md)합니다.


## <a name="related-links"></a>관련 링크

- [IOS 앱을 업데이트합니다.](~/cross-platform/macios/unified/updating-apps.md)
- [IOS 앱을 업데이트합니다.](~/cross-platform/macios/unified/updating-ios-apps.md)
- [플랫폼 간 앱에서 네이티브 형식 사용](~/cross-platform/macios/native-types-cross-platform.md)
- [팁 업데이트](~/cross-platform/macios/unified/updating-tips.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
