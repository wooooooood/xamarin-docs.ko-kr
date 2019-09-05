---
title: 기존 Xamarin Forms 앱 업데이트
description: 이 문서에서는 Xamarin. Forms 앱을 Classic API에서 Unified API 업데이트 하기 위해 따라야 하는 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 1820dfa1fb756ede6076fb61ad5eb4f6c9926fe8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280717"
---
# <a name="updating-existing-xamarinforms-apps"></a>기존 Xamarin Forms 앱 업데이트

_Unified API 사용 하 고 버전 1.3.1으로 업데이트 하려면 다음 단계를 수행 하 여 기존 Xamarin.ios 앱을 업데이트 합니다._

> [!IMPORTANT]
> Xamarin.ios 1.3.1는 Unified API을 지 원하는 첫 번째 릴리스입니다 때문에 iOS 앱을 통합으로 마이그레이션하는 동시에 최신 버전을 사용 하도록 전체 솔루션을 업데이트 해야 합니다. 즉, 통합 지원을 위해 iOS 프로젝트를 업데이트 하는 것 외에도 솔루션의 _모든_ 프로젝트에서 코드를 편집 해야 합니다.

업데이트는 다음 두 단계로 수행 됩니다.

1. 마이그레이션 도구에서 Mac용 Visual Studio의 빌드를 사용 하 여 iOS 앱을 Unified API로 마이그레이션합니다.

    - 마이그레이션 도구를 사용 하 여 프로젝트를 자동으로 업데이트 합니다.

    - [Ios 앱을 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md) 하는 지침에 설명 된 대로 Ios 기본 api를 업데이트 합니다 (특히 사용자 지정 렌더러 또는 종속성 서비스 코드).

2. 전체 솔루션을 Xamarin.ios 버전 1.3으로 업데이트 합니다.

    1. Xamarin.ios 1.3.1 NuGet 패키지를 설치 합니다.

    2. 공유 코드 `App` 에서 클래스를 업데이트 합니다.

    3. IOS 프로젝트 `AppDelegate` 에서를 업데이트 합니다.

    4. Android 프로젝트 `MainActivity` 에서를 업데이트 합니다.

    5. Windows Phone 프로젝트 `MainPage` 에서를 업데이트 합니다.

## <a name="1-ios-app-unified-migration"></a>1. iOS 앱 (통합 마이그레이션)

마이그레이션의 일부로 Xamarin.ios를 Unified API를 지 원하는 버전 1.3으로 업그레이드 해야 합니다. 올바른 어셈블리 참조를 만들려면 먼저 Unified API를 사용 하도록 iOS 프로젝트를 업데이트 해야 합니다.

### <a name="migration-tool"></a>마이그레이션 도구

IOS 프로젝트를 클릭 하 여 선택 하 고 프로젝트를 선택 하 **> xamarin.ios로 마이그레이션 Unified API** 하 고 표시 되는 경고 메시지에 동의 합니다.

![](updating-xamarin-forms-apps-images/beta-tool1.png "프로젝트 > 선택 하 여 Xamarin.ios Unified API로 마이그레이션 ... 표시 되는 경고 메시지에 동의 합니다.")

그러면 다음이 자동으로 수행 됩니다.

- 통합 64 비트 API를 지원 하도록 프로젝트 형식을 변경 합니다.
- 프레임 워크 참조를 **xamarin.ios** 로 변경 합니다 (이전 **monotouch.dialog** 참조 대체).
- 코드에서 네임 스페이스 참조를 변경 하 여 `MonoTouch` 접두사를 제거 합니다.
- Unified API에 대 한 올바른 빌드 대상을 사용 하도록 **.csproj** 파일을 업데이트 합니다.

프로젝트를 **정리** 하 고 **빌드하여** 수정할 다른 오류가 없는지 확인 합니다. 추가 작업은 필요 하지 않습니다. 이러한 단계는 [Unified API 문서](~/cross-platform/macios/unified/updating-ios-apps.md)에 자세히 설명 되어 있습니다.

### <a name="update-native-ios-apis-if-required"></a>네이티브 iOS Api 업데이트 (필요한 경우)

추가 iOS 네이티브 코드 (예: 사용자 지정 렌더러 또는 종속성 서비스)를 추가한 경우 추가 수동 코드 수정을 수행 해야 할 수 있습니다. 앱을 다시 컴파일하고, 필요할 수 있는 변경 내용에 대 한 자세한 내용은 [기존 IOS 앱 업데이트 지침](~/cross-platform/macios/unified/updating-ios-apps.md) 을 참조 하세요. 또한 [이러한 팁](~/cross-platform/macios/unified/updating-tips.md) 은 필요한 변경 내용을 식별 하는 데 도움이 됩니다.

## <a name="2-xamarinforms-131-update"></a>2. Xamarin.ios 1.3.1 업데이트

IOS 앱이 Unified API 업데이트 된 후에는 나머지 솔루션을 Xamarin.ios 버전 1.3.1으로 업데이트 해야 합니다. 다음을 포함합니다.

- 각 프로젝트에서 Xamarin.ios NuGet 패키지를 업데이트 합니다.
- 새 xamarin.ios `Application`, `FormsApplicationDelegate` (iOS), `FormsApplicationActivity` (Android) 및 `FormsApplicationPage` (Windows Phone) 클래스를 사용 하도록 코드를 변경 합니다.

이러한 단계는 아래에 설명 되어 있습니다.

### <a name="21-update-nuget-in-all-projects"></a>2.1 모든 프로젝트에서 NuGet 업데이트

솔루션에 있는 모든 프로젝트에 대해 NuGet 패키지 관리자를 사용 하 여 1.3.1 시험판으로 Xamarin.ios를 업데이트 합니다. PCL (있는 경우), iOS, Android 및 Windows Phone. 버전 1.3으로 업데이트 하려면 Xamarin.ios NuGet 패키지를 **삭제 하 고 다시 추가** 하는 것이 좋습니다.

> [!NOTE]
> Xamarin.ios 버전 1.3.1 현재 *시험판*에 있습니다. 즉, 최신 시험판 버전을 보려면 NuGet (Mac용 Visual Studio의 틱 상자를 통해 또는 Visual Studio의 드롭다운 목록)에서 **시험판** 옵션을 선택 해야 합니다.

> [!IMPORTANT]
> Visual Studio를 사용 하는 경우 최신 버전의 NuGet 패키지 관리자가 설치 되어 있는지 확인 합니다. Visual Studio의 이전 버전의 NuGet은 1.3.1의 통합 버전을 제대로 설치 하지 않습니다. **도구 > 확장 및 업데이트 ...** 로 이동 하 고 **설치** 된 목록을 클릭 하 여 **Visual Studio 용 NuGet 패키지 관리자** 가 버전 2.8.5 이상 인지 확인 합니다. 이전 버전인 경우 **업데이트** 목록을 클릭 하 여 최신 버전을 다운로드 합니다.

NuGet 패키지를 1.3.1로 업데이트 한 후에는 각 프로젝트에서 다음과 같이 변경 하 여 새 `Xamarin.Forms.Application` 클래스로 업그레이드 해야 합니다.

### <a name="22-portable-class-library-or-shared-project"></a>2.2 이식 가능한 클래스 라이브러리 (또는 공유 프로젝트)

**App.cs** 파일을 다음과 같이 변경 합니다.

- 클래스가 이제에서 `Application`상속 됩니다. `App`
- `MainPage` 속성은 표시할 첫 번째 콘텐츠 페이지로 설정 됩니다.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

`GetMainPage` 메서드를 완전히 제거 하 고 대신 `Application` 서브 클래스에서 *속성* 을 `MainPage` 설정 합니다.

이 새로운 `Application` 기본 클래스는 응용 프로그램의 `OnSleep`수명 주기 `OnResume` 를 관리 하는 데 도움이 되는 `OnStart`, 및 재정의도 지원 합니다.

이 클래스는 아래에 설명 된 대로 `LoadApplication` 각 응용 프로그램 프로젝트의 새 메서드에 전달 됩니다. `App`

### <a name="23-ios-app"></a>2.3 iOS 앱

**AppDelegate.cs** 파일을 다음과 같이 변경 합니다.

- 클래스는 `UIApplicationDelegate` 이전이 `FormsApplicationDelegate` 아닌에서 상속 됩니다.
- `LoadApplication`는의 `App`새 인스턴스를 사용 하 여 호출 됩니다.

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

### <a name="23-android-app"></a>2.3 Android 앱

**MainActivity.cs** 파일을 다음과 같이 변경 합니다.

- 클래스는 `FormsActivity` 이전이 `FormsApplicationActivity` 아닌에서 상속 됩니다.
- `LoadApplication`는의 새 인스턴스를 사용 하 여 호출 됩니다.`App`

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

**Mainpage** (XAML 및 codebehind 모두)를 업데이트 해야 합니다.

다음과 같이 **Mainpage .xaml** 파일을 변경 합니다.

- 루트 XAML 요소는 이어야 `winPhone:FormsApplicationPage`합니다.
- 특성을로 변경 해야 합니다. `xmlns:phone``xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

업데이트 된 예제는 다음과 같습니다. 이러한 항목을 편집 하기만 하면 됩니다. 나머지 특성은 동일 하 게 유지 되어야 합니다.

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

**MainPage.xaml.cs** 파일을 다음과 같이 변경 합니다.

- 클래스는 `PhoneApplicationPage` 이전이 `FormsApplicationPage` 아닌에서 상속 됩니다.
- `LoadApplication`는 xamarin.ios `App` 클래스의 새 인스턴스를 사용 하 여 호출 됩니다. Windows Phone 자체 `App` 클래스가 이미 정의 되어 있으므로이 참조를 정규화 해야 할 수도 있습니다.

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

Xamarin.ios NuGet 패키지를 업데이트 한 후에도 이와 유사한 오류가 표시 되는 경우가 있습니다. NuGet 업데이트 프로그램이 **.csproj** 파일에서 이전 버전에 대 한 참조를 완전히 제거 하지 않을 때 발생 합니다.

>YOUR\_PROJECT.csproj: 오류: 이 프로젝트는이 컴퓨터에 없는 NuGet 패키지를 참조 합니다. NuGet 패키지 복원을 사용 하도록 설정 하 여 다운로드 합니다.  자세한 내용은 http://go.microsoft.com/fwlink/?LinkID=322105 을 참조하세요. 누락 된 파일은입니다. /.. /packages/Xamarin.Forms.1.2.3.6257/build/portable-win + net45 + wp80 + MonoAndroid10 + MonoTouch10/Xamarin.ios. \_(프로젝트)

이러한 오류를 해결 하려면 텍스트 편집기에서 **.csproj** 파일을 열고 아래에 표시 된 `<Target` 요소와 같은 이전 버전의 xamarin.ios를 참조 하는 요소를 찾습니다. 이 전체 요소를 **.csproj** 파일에서 수동으로 삭제 하 고 변경 내용을 저장 해야 합니다.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

이러한 이전 참조가 제거 되 면 프로젝트가 성공적으로 빌드됩니다.

## <a name="considerations"></a>고려 사항

응용 프로그램이 하나 이상의 구성 요소나 NuGet 패키지를 사용 하는 경우 기존 Xamarin.ios 프로젝트를 Classic API에서 새 Unified API로 변환할 때 다음 사항을 고려해 야 합니다.

### <a name="components"></a>구성 요소

응용 프로그램에 포함 된 모든 구성 요소를 Unified API 업데이트 해야 합니다. 그렇지 않으면 컴파일을 시도할 때 충돌이 발생 합니다. 포함 된 모든 구성 요소에 대해 현재 버전을 Unified API를 지 원하는 Xamarin 구성 요소 저장소의 새 버전으로 바꾸고 새로 빌드를 수행 합니다. 작성자가 아직 변환 하지 않은 구성 요소는 구성 요소 저장소에 32 비트만 표시 합니다.

### <a name="nuget-support"></a>NuGet 지원

Unified API 지원 작업을 수행 하기 위해 NuGet에 변경 내용을 적용 하는 동안 nuget의 새로운 릴리스가 출시 되지 않았기 때문에 NuGet을 가져와 새 Api를 인식 하는 방법을 평가 하 고 있습니다.

이 시점까지 구성 요소와 마찬가지로 프로젝트에 포함 된 NuGet 패키지를 통합 Api를 지 원하는 버전으로 전환 하 고 나중에 정리 빌드를 수행 해야 합니다.

> [!IMPORTANT]
> _"오류 3"에 ' monotouch.dialog ' 및 ' monotouch.dialog '가 모두 포함 될 수 없습니다. ' xamarin.ios '는 명시적으로 참조 되는 반면 ' '는 ' xxx, Version = 0.0.000, Culture =에 의해 참조 됩니다. 중립, PublicKeyToken = null ' "_ 응용 프로그램을 통합 된 api로 변환한 후에는 일반적으로 Unified API로 업데이트 되지 않은 구성 요소 또는 NuGet 패키지가 프로젝트에 포함 되어 있기 때문입니다. 기존 component/NuGet을 제거 하 고, 통합 Api를 지 원하는 버전으로 업데이트 하 고, 클린 빌드를 수행 해야 합니다.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.ios 앱의 64 비트 빌드 사용

Unified API로 변환 된 Xamarin.ios 모바일 응용 프로그램의 경우 개발자는 응용 프로그램의 옵션에서 64 비트 컴퓨터용 응용 프로그램 빌드를 계속 사용 하도록 설정 해야 합니다. 64 비트 빌드를 사용 하는 방법에 대 한 자세한 내용은 [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md#enable-64) 문서의 **64 비트 빌드 사용** 을 참조 하세요.

## <a name="summary"></a>요약

이제 Xamarin.ios 응용 프로그램이 버전 1.3.1으로 업데이트 되 고 iOS 앱이 Unified API (iOS 플랫폼에서 64 비트 아키텍처 지원)로 마이그레이션 되었습니다.

위에서 설명한 것 처럼 Xamarin.ios 앱에 사용자 지정 렌더러 또는 종속성 서비스와 같은 네이티브 코드가 포함 된 경우 [Unified API에 도입](~/cross-platform/macios/index.md)된 새 형식을 사용 하기 위해 업데이트 해야 할 수도 있습니다.

## <a name="related-links"></a>관련 링크

- [IOS 앱 업데이트](~/cross-platform/macios/unified/updating-apps.md)
- [IOS 앱 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md)
- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
- [팁 업데이트](~/cross-platform/macios/unified/updating-tips.md)
- [클래식 vs Unified API 차이점](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
