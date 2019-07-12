---
title: 기존 Xamarin.Forms 앱 업데이트
description: 이 문서에는 통합 API로 클래식 API에서 Xamarin.Forms 앱 업데이트를 준수 해야 하는 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 36a4c6b66f7f724bfccc3c2a3b81c17f1d34a9c5
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829729"
---
# <a name="updating-existing-xamarinforms-apps"></a>기존 Xamarin.Forms 앱 업데이트

_기존 Xamarin.Forms 앱에 통합 API를 사용 및 업데이트 버전 1.3.1 업데이트 하려면 다음이 단계를 수행 합니다._

> [!IMPORTANT]
> Xamarin.Forms 1.3.1 통합 API를 지 원하는 첫 번째 릴리스 이기 때문에 전체 솔루션을 통합 하는 iOS 앱을 마이그레이션할 때와 동시에 최신 버전을 사용 하도록 업데이트 되어야 합니다. 즉,는 통합 지원에 대 한 iOS 프로젝트를 업데이트 하는 것 외에도 해야의 코드를 편집 _모든_ 솔루션의 프로젝트입니다.

업데이트는 두 단계로 수행 됩니다.

1. Visual Studio를 사용 하 여 마이그레이션 도구에서 Mac 빌드에 대 한 통합 API로 iOS 앱을 마이그레이션하십시오.

    - 마이그레이션 도구를 사용 하 여 프로젝트를 자동으로 업데이트 합니다.

    - 업데이트 iOS 네이티브 Api 지침에 설명 된 대로 [iOS 앱을 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md) (특히 사용자 지정 렌더러 또는 종속성 서비스 코드에서).

2. Xamarin.Forms 버전 1.3 전체 솔루션을 업데이트 합니다.

    1. 1\.3.1 Xamarin.Forms NuGet 패키지를 설치 합니다.

    2. 업데이트 된 `App` 공유 코드에는 클래스입니다.

    3. 업데이트 된 `AppDelegate` iOS 프로젝트에서.

    4. 업데이트 된 `MainActivity` Android 프로젝트에서입니다.

    5. 업데이트 된 `MainPage` Windows Phone 프로젝트에서.

## <a name="1-ios-app-unified-migration"></a>1입니다. iOS 앱 (Unified 마이그레이션)

마이그레이션 중 Xamarin.Forms 통합 API를 지 원하는 버전 1.3으로 업그레이드 해야 합니다. 올바른 어셈블리 참조를 만들 수에 대 한 순서로 먼저 통합 API를 사용 하 여 iOS 프로젝트를 업데이트 해야 합니다.

### <a name="migration-tool"></a>마이그레이션 도구

IOS 프로젝트에는 클릭을 선택 하면 선택한 **프로젝트 > Xamarin.iOS 통합 API로 마이그레이션 중...**  나타나는 경고 메시지에 동의 합니다.

![](updating-xamarin-forms-apps-images/beta-tool1.png "프로젝트 선택 > Xamarin.iOS 통합된 API로 마이그레이션 중... 표시 되는 경고 메시지에 동의")

이 자동으로:

- 프로젝트 유형을 변경 하 여 통합 64 비트 API를 지원 합니다.
- 프레임 워크를 변경할 **Xamarin.iOS** (이전 교체 **monotouch** 참조).
- 네임 스페이스 참조를 제거 하는 코드에서 변경 된 `MonoTouch` 접두사입니다.
- 업데이트를 **csproj** Unified API에 대 한 올바른 빌드 대상을 사용 하는 파일입니다.

**정리** 하 고 **빌드** 프로젝트 문제를 해결 하려면 다른 오류가 없는지 확인 합니다. 추가 작업이 없으므로 해야 합니다. 이러한 단계에 자세히 설명 되어는 [Unified API 문서](~/cross-platform/macios/unified/updating-ios-apps.md)합니다.

### <a name="update-native-ios-apis-if-required"></a>(필요한 경우) 네이티브 iOS Api 업데이트

추가 iOS 네이티브 코드 (예: 사용자 지정 렌더러 또는 종속성 서비스)를 추가한 경우에 추가 수동 코드 문제를 해결 해야 합니다. 다시 앱을 컴파일 및 참조를 [기존 업데이트 iOS 앱 지침](~/cross-platform/macios/unified/updating-ios-apps.md) 필요할 수 있는 변경 내용에 대 한 자세한 내용은 합니다. [이러한 팁](~/cross-platform/macios/unified/updating-tips.md) 필요한 변경 내용을 식별도 도움이 됩니다.

## <a name="2-xamarinforms-131-update"></a>2. Xamarin.Forms 1.3.1 업데이트

IOS 앱을 통합 API로 업데이트 후 솔루션의 나머지 부분에서는 Xamarin.Forms 버전 1.3.1 업데이트할 해야 합니다. 다음을 포함합니다.

- 각 프로젝트에 Xamarin.Forms NuGet 패키지를 업데이트 합니다.
- 새 Xamarin.Forms를 사용 하도록 코드를 변경 `Application`, `FormsApplicationDelegate` (iOS) `FormsApplicationActivity` (Android) 및 `FormsApplicationPage` (Windows Phone) 클래스입니다.

이러한 단계는 아래 설명 되어 있습니다.

### <a name="21-update-nuget-in-all-projects"></a>2.1 모든 프로젝트에서 NuGet을 업데이트

1\.3.1 Xamarin.Forms 업데이트 솔루션의 모든 프로젝트에 대 한 NuGet 패키지 관리자를 사용 하 여 시험판: PCL (있는 경우), iOS, Android 및 Windows Phone 것이 좋습니다 했는지 **삭제 하 고 다시 추가** Xamarin.Forms NuGet 패키지를 버전 1.3으로 업데이트 합니다.

> [!NOTE]
> 현재는 Xamarin.Forms 버전 1.3.1 *시험판*합니다. 즉, 선택 해야 합니다 **시험판** 최신 시험판 버전을 보려면 (눈금 상자를 Mac 용 Visual Studio에서) 또는 드롭 다운-목록 Visual Studio에서 NuGet 옵션.

> [!IMPORTANT]
> Visual Studio를 사용 하는 최신 버전의 NuGet 패키지 관리자를 설치 했는지 확인 합니다. 이전 버전의 Visual Studio에서 NuGet 통합 버전 1.3.1 Xamarin.Forms에 올바르게 설치 되지 않습니다. 로 **도구 > 확장 및 업데이트 하는 중...**  를 클릭 합니다 **설치 됨** 있는지 여부를 확인 하려면 목록을 **Visual Studio 용 NuGet 패키지 관리자** 이상이 2.8.5 버전. 이전 인 경우 클릭 합니다 **업데이트** 최신 버전을 다운로드 하는 목록입니다.

1\.3.1 Xamarin.Forms에 NuGet 패키지를 업데이트 한 후 새 업그레이드할 각 프로젝트에서 다음과 같이 변경할 `Xamarin.Forms.Application` 클래스입니다.

### <a name="22-portable-class-library-or-shared-project"></a>2.2 이식 가능한 클래스 라이브러리 (또는 공유 프로젝트)

변경 된 **App.cs** 파일 되도록 합니다.

- 합니다 `App` 클래스에서 이제 상속 `Application`합니다.
- `MainPage` 속성을 표시 하려는 첫 번째 콘텐츠 페이지로 설정 합니다.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

완전히 제거 했습니다 합니다 `GetMainPage` 메서드를 대신 설정 및는 `MainPage` *속성* 에 `Application` 하위 클래스입니다.

이 새로운 `Application` 기본 클래스도 지원 합니다 `OnStart`, `OnSleep`, 및 `OnResume` 응용 프로그램 수명 주기를 관리할 수 있도록 재정의 합니다.

합니다 `App` 클래스 전달 되어 새 `LoadApplication` 아래 설명 된 대로 각 앱 프로젝트에서 메서드:

### <a name="23-ios-app"></a>2.3 iOS 앱

변경 된 **AppDelegate.cs** 파일 되도록 합니다.

- 클래스에서 상속 `FormsApplicationDelegate` (대신 `UIApplicationDelegate` 이전).
- `LoadApplication` 새 인스턴스를 사용 하 여 라고 `App`합니다.

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

### <a name="23-android-app"></a>2.3 android 앱

변경 된 **MainActivity.cs** 파일 되도록 합니다.

- 클래스에서 상속 `FormsApplicationActivity` (대신 `FormsActivity` 이전).
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

### <a name="24-windows-phone-app"></a>2.4 Windows Phone 앱

업데이트 해야 합니다 **MainPage** -는 XAML 및 코드 숨김 파일입니다.

변경 된 **MainPage.xaml** 파일 되도록 합니다.

- 루트 XAML 요소 여야 `winPhone:FormsApplicationPage`합니다.
- 합니다 `xmlns:phone` 특성이 *변경* 를 `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

업데이트 된 예는 아래-(특성의 나머지는 동일 함) 이러한 작업을 편집만 있어야 합니다.

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

변경 된 **MainPage.xaml.cs** 파일 되도록 합니다.

- 클래스에서 상속 `FormsApplicationPage` (대신 `PhoneApplicationPage` 이전).
- `LoadApplication` Xamarin.Forms의 새 인스턴스를 사용 하 여 라고 `App` 클래스입니다. Windows Phone 있으므로이 참조를 정규화 해야 자신의 `App` 이미 정의 된 클래스입니다.

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

### <a name="troubleshooting"></a>문제 해결

경우에 따라 Xamarin.Forms NuGet 패키지를 업데이트 한 후 다음과 유사한 오류가 표시 됩니다. NuGet 업데이트 프로그램에서 이전 버전에 대 한 참조를 완전히 제거 하지 않습니다 하는 경우 발생 하 **csproj** 파일입니다.

>YOUR\_PROJECT.csproj: 오류: 이 프로젝트는이 컴퓨터에 누락 된 NuGet 패키지를 참조 합니다. 다운로드 하려면 NuGet 패키지 복원 사용 하도록 설정 합니다.  자세한 내용은 http://go.microsoft.com/fwlink/?LinkID=322105 을 참조하세요. 누락 된 파일은... /.. /packages/Xamarin.Forms.1.2.3.6257/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets 합니다. (사용자\_프로젝트)

이러한 오류를 해결 하려면 엽니다는 **csproj** 텍스트 편집기에서 파일을 검색할 `<Target` 아래에 표시 된 요소와 같은 이전 버전에서는 Xamarin.Forms의를 참조 하는 요소입니다. 이 전체 요소를 수동으로 삭제 해야 합니다 **csproj** 파일 및 변경 내용을 저장 합니다.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

이러한 이전 참조가 제거 되 면 프로젝트가 성공적으로 빌드되어야 합니다.

## <a name="considerations"></a>고려 사항

다음 고려 사항이 하나 이상의 구성 요소 또는 NuGet 패키지에서 해당 앱이 의존 하는 경우 클래식 API에서 기존 Xamarin.Forms 프로젝트를 새 통합 API로 변환 하는 경우 계정에 취해야 합니다.

### <a name="components"></a>구성 요소

응용 프로그램에 포함 된 모든 구성 요소는 통합 API로 업데이트 해야 할 또는 컴파일 하려고 할 때 충돌을 받습니다. 모든 포함 된 구성 요소에 대 한 통합 API를 지 원하는 Xamarin Component Store에서 새 버전을 사용 하 여 현재 버전을 교체 하 고 클린 빌드를 수행 합니다. 작성자에 의해 아직 전환 되지 않은 모든 구성 요소는 32 비트 구성 요소 저장소에만 경고를 표시 합니다.

### <a name="nuget-support"></a>NuGet 지원

Unified API 지원을 통해 작동 하도록 NuGet에 변경 내용을 제공한 것 동안 없었습니다 NuGet의 새 릴리스 하므로 새 Api를 인식 하는 NuGet을 가져오는 방법을 평가 하는 것입니다.

구성 요소와 마찬가지로 통합 Api를 지 원하는 버전으로 프로젝트에 포함 된 모든 NuGet 패키지를 전환 하는 클린 빌드를 나중에 수행 해야 합니다.

> [!IMPORTANT]
> 폼에 오류가 있는 경우 _"오류 3 동일한 Xamarin.iOS 프로젝트에서 'monotouch.dll' 및 'Xamarin.iOS.dll'를 포함할 수 없습니다-'Xamarin.iOS.dll'는 'monotouch.dll'에서 참조 하는 동안 명시적으로 참조 됩니다 ' xxx, 버전 = 0.0.000, Culture = neutral, PublicKeyToken = null'"_ Unified Api로 응용 프로그램으로 변환 후 일반적으로 통합 API로 업데이트 되지 않은 프로젝트에서 구성 요소 또는 NuGet 패키지 사용 하지 때문입니다. 기존 구성 요소/NuGet을 제거 하 고, 통합 Api를 지 원하는 버전으로 업데이트 하 고, 클린 빌드를 수행 해야 합니다.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS 앱의 빌드를 64 비트를 사용 하도록 설정

Xamarin.iOS 모바일 응용 프로그램의 통합 API로 변환 된 경우 개발자는 여전히 앱의 옵션에서 64 비트 컴퓨터에 대 한 응용 프로그램을 작성 하는 사용 하도록 설정 해야 합니다. 참조 하세요 합니다 **사용 하도록 설정 하면 64 비트 빌드의 Xamarin.iOS 앱** 의 [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md#enable-64) 64 비트를 사용 하도록 설정 하는 자세한 지침은 문서 작성 합니다.

## <a name="summary"></a>요약

이제 버전 1.3.1으로 Xamarin.Forms 응용 프로그램을 업데이트 해야 하 고 iOS 앱 (iOS 플랫폼에서 64 비트 아키텍처 지원)는 통합 API로 마이그레이션.

Xamarin.Forms 앱에 사용자 지정 렌더러와 같은 네이티브 코드 또는 종속성 서비스를 새 형식을 사용 하도록 업데이트 해야 할 수 있습니다 이러한 다음 위에서 설명한 것 처럼 [Unified API에 도입 된](~/cross-platform/macios/index.md)합니다.

## <a name="related-links"></a>관련 링크

- [IOS 앱 업데이트](~/cross-platform/macios/unified/updating-apps.md)
- [IOS 앱 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md)
- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
- [팁 업데이트](~/cross-platform/macios/unified/updating-tips.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
