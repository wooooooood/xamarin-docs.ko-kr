---
title: 'title: “Xamarin.Essentials 시작” description: “Xamarin.Essentials는 사용자 인터페이스가 생성된 방식과 관계없이 공유 코드에서 액세스할 수 있는 모든 iOS, Android 또는 UWP 애플리케이션에서 작동하는 단일 플랫폼 간 API를 제공합니다.”'
description: 'ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9 author: jamesmontemagno ms.author: jamont ms.custom: video ms.date: 05/11/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.custom: video
ms.date: 05/11/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0c0052b35475522ffb3634ebe22f69f66fe3b22b
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84129313"
---
# <a name="get-started-with-xamarinessentials"></a>Xamarin.Essentials 시작하기

Xamarin.Essentials는 사용자 인터페이스가 생성된 방식과 관계없이 공유 코드에서 액세스할 수 있는 모든 iOS, Android 또는 UWP 애플리케이션에서 작동하는 단일 플랫폼 간 API를 제공합니다. 지원되는 운영 체제에 대한 자세한 내용은 [플랫폼 및 기능 지원 지침](platform-feature-support.md)을 참조하세요.

## <a name="installation"></a>설치

Xamarin.Essentials는 NuGet 패키지로 사용할 수 있으며 Visual Studio의 모든 새 프로젝트에 포함되어 있습니다. 다음 단계를 통해 Visual Studio를 사용하는 기존 프로젝트에 추가할 수도 있습니다.

1. [Visual Studio Tools for Xamarin](~/get-started/installation/index.md)을 사용하여 [Visual Studio](https://visualstudio.microsoft.com/)를 다운로드하고 설치합니다.

2. 기존 프로젝트를 열거나, **Visual Studio C#** 아래의 비어 있는 앱 템플릿(Android, iPhone 및 iPad, 플랫폼 간)을 사용하여 새 프로젝트를 만듭니다.

    > [!IMPORTANT]
    > UWP 프로젝트에 추가하려면 프로젝트 속성에 빌드 16299 이상이 설정되었는지 확인합니다.

3. [ **Xamarin.Essentials** ](https://www.nuget.org/packages/Xamarin.Essentials/) NuGet 패키지를 각 프로젝트에 추가합니다.

    <!--markdownlint-disable MD023 -->
    # <a name="visual-studio"></a>[Visual Studio](#tab/windows)

    솔루션 탐색기 패널에서 솔루션 이름을 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택합니다. **Xamarin.Essentials** 를 검색하고 Android, iOS, UWP 및 .NET Standard 라이브러리를 포함한 **모든** 프로젝트에 패키지를 설치합니다.

    # <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

    솔루션 탐색기 패널에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **추가 > NuGet 패키지 추가...** 를 선택합니다. **Xamarin.Essentials** 를 검색하고 Android, iOS 및 .NET Standard 라이브러리를 포함한 **모든** 프로젝트에 패키지를 설치합니다.

    -----

4. C# 클래스에서 Xamarin.Essentials에 대한 참조를 추가하여 API를 참조합니다.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials에는 플랫폼별 설정이 필요합니다.

    # <a name="android"></a>[Android](#tab/android)

    Xamarin.Essentials는 API 레벨 19에 해당하는 최소 Android 버전 4.4를 지원하지만, 컴파일 대상 Android 버전은 API 레벨 28 및 29에 해당하는 9.0 또는 10.0이어야 합니다. Visual Studio에서 이러한 두 버전은 Android 프로젝트에 대한 프로젝트 속성 대화 상자의 Android 매니페스트 탭에서 설정됩니다. Mac용 Visual Studio에서는 Android 프로젝트에 대한 프로젝트 옵션 대화 상자의 Android 애플리케이션 탭에서 설정됩니다.

    Android 9.0에 대해 컴파일할 때 Xamarin.Essentials는 필요한 Xamarin.Android.Support 라이브러리 버전 28.0.0.3을 설치합니다. NuGet 패키지 관리자를 사용하여 애플리케이션에 필요한 다른 모든 Xamarin.Android.Support 라이브러리도 버전 28.0.0.3으로 업데이트해야 합니다. 애플리케이션에서 사용하는 모든 Xamarin.Android.Support 라이브러리는 동일해야 하며, 적어도 28.0.0.3 버전이어야 합니다. Xamarin.Essentials NuGet을 추가하거나 솔루션의 NuGet을 업데이트하는 데 문제가 있는 경우 [문제 해결 페이지](troubleshooting.md)를 참조하세요.

    버전 1.5.0부터는 Android 10.0에 대해 컴파일할 때 Xamarin.Essentials는 필요한 AndroidX 지원 라이브러리를 설치합니다. 아직 전환하지 않은 경우에는 [AndroidX 설명서](https://docs.microsoft.com/xamarin/android/platform/androidx)를 참조하세요.

    Android 프로젝트의 `MainLauncher` 또는 시작된 `Activity`의 `OnCreate` 메서드에서 Xamarin.Essentials를 초기화해야 합니다.

    ```csharp
    protected override void OnCreate(Bundle savedInstanceState) {
        //...
        base.OnCreate(savedInstanceState);
        Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code, it may also be called: bundle
        //...
    ```

    Android에서 런타임 권한을 처리하려면 Xamarin.Essentials가 `OnRequestPermissionsResult`를 받아야 합니다. 모든 `Activity` 클래스에 다음 코드를 추가합니다.

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="ios"></a>[iOS](#tab/ios)

    추가 설정이 필요하지 않습니다.

    # <a name="uwp"></a>[UWP](#tab/uwp)

    추가 설정이 필요하지 않습니다.

    -----

6. 각 기능에 대한 코드 조각을 복사하여 붙여넣을 수 있는 [Xamarin.Essentials 가이드](index.md)를 따르세요.

## <a name="xamarinessentials---cross-platform-apis-for-mobile-apps-video"></a>Xamarin.Essentials - 모바일 앱용 플랫폼 간 API(비디오)

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Snack-Pack-XamarinEssentials-Cross-Platform-APIs-for-Mobile-Apps/player]

## <a name="other-resources"></a>기타 리소스

Xamarin을 처음 접하는 개발자의 경우 [Xamarin 개발 시작](~/cross-platform/getting-started/index.md)을 방문하는 것이 좋습니다.

현재 소스 코드 및 향후 제공될 기능을 확인하고, 샘플을 실행하고, 리포지토리를 복제하려면 [Xamarin.Essentials GitHub 리포지토리](https://github.com/xamarin/Essentials)를 방문하세요. 커뮤니티 기여 환영!

[API 문서](xref:Xamarin.Essentials)에서 Xamarin.Essentials의 모든 기능을 살펴보세요.
