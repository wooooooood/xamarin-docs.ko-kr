---
title: Xamarin.Essentials 시작
description: 모든 iOS, Android 또는 UWP 작동 하는 단일 플랫폼 간 API를 제공 하는 Xamarin.Essentials에서 액세스할 수 있는 응용 프로그램 사용자 인터페이스를 만든 방법에 관계 없이 코드를 공유 합니다.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e371a6125d223d354b75ce7e09dcc28efb3dffa
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855236"
---
# <a name="get-started-with-xamarinessentials"></a>Xamarin.Essentials 시작

![시험판 버전 NuGet](~/media/shared/pre-release.png)

모든 iOS, Android 또는 UWP 작동 하는 단일 플랫폼 간 API를 제공 하는 Xamarin.Essentials에서 액세스할 수 있는 응용 프로그램 사용자 인터페이스를 만든 방법에 관계 없이 코드를 공유 합니다.

## <a name="platform-support"></a>플랫폼 지원

Xamarin.Essentials은 다음 플랫폼과 운영 체제를 지원합니다.

| 플랫폼 | 버전 |
| --- | --- |
| Android | 4.4 (API 19) 이상 |
| iOS |10.0 이상 |
| UWP | 10.0.16299.0 이상 |

## <a name="installation"></a>설치

Xamarin.Essentials는 Visual Studio를 사용 하 여 모든 기존 또는 새 프로젝트에 추가할 수 있는 NuGet 패키지로 사용할 수 있습니다.

1. 다운로드 및 설치 [Visual Studio](http://visualstudio.com) 사용 하 여 합니다 [Visual Studio tools for Xamarin](~/cross-platform/get-started/installation/index.md)합니다.

2. 아래에 있는 비어 있는 앱 템플릿을 사용 하 여 새 프로젝트를 만들거나 기존 프로젝트를 엽니다 **Visual Studio C#** (Android, iPhone 및 iPad 또는 플랫폼 간). **중요 한**: UWP 프로젝트에 추가 확인 빌드 16299 이상 프로젝트 속성에서 설정 됩니다.

3. 추가 된 **Xamarin.Essentials** 각 프로젝트에 NuGet 패키지:

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    솔루션 탐색기 창에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 관리**합니다. 검색할 **Xamarin.Essentials** 패키지를 설치 하 고 **모든** Android, iOS, UWP 및.NET Standard 라이브러리를 포함 한 프로젝트입니다.

    > [!TIP]
    > 확인 합니다 **시험판 포함** 하는 동안 상자를 [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) 는 미리 보기로 제공 합니다.

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    솔루션 탐색기 창에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > NuGet 패키지 추가... **. 검색할 **Xamarin.Essentials** 패키지를 설치 하 고 **모든** Android, iOS 및.NET Standard 라이브러리를 포함 한 프로젝트입니다.

    > [!TIP]
    > 확인 합니다 **시험판 패키지 표시** 하는 동안 상자 합니다 [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) 미리 보기 상태인 합니다.

    -----

4. Api를 참조 하려면 모든 C# 클래스에서 Xamarin.Essentials에 대 한 참조를 추가 합니다.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials는 플랫폼별 설치를 필요합니다.

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials 4.4, API 수준 19, 해당 하는 최소 Android 버전을 지원 하지만 컴파일 대상 Android 버전 8.1 해야 합니다. API 레벨 27에 해당 합니다. (Visual Studio에서 이러한 두 가지 버전 설정이 Android 매니페스트 탭에서 Android 프로젝트의 프로젝트 속성 대화 상자. Mac 용 Visual Studio에서 이러한 설정을 완료 했습니다 Android 응용 프로그램 탭에서 Android 프로젝트의 프로젝트 옵션 대화 상자에서.) 
    
    Xamarin.Essentials는 27.0.2.1 필요한 Xamarin.Android.Support 라이브러리의 버전을 설치 합니다. 응용 프로그램에 필요한 다른 Xamarin.Android.Support 라이브러리 NuGet 패키지 관리자를 사용 하 여 27.0.2.1 버전도 업데이트 되어야 합니다. 응용 프로그램에서 사용 하는 모든 Xamarin.Android.Support 라이브러리 동일 여야 하며 최소 27.0.2.1 버전입니다. 참조 된 [문제 해결 페이지](troubleshooting.md) Xamarin.Essentials NuGet를 추가 하거나 솔루션의 Nuget 업데이트에 문제가 있는 경우.

    Android 프로젝트의 `MainLauncher` 또는 임의의 `Activity` 즉 시작된 Xamarin.Essentials에서 초기화 되어야 합니다는 `OnCreate` 메서드:

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    를 처리 하기 위해 android 런타임 권한에 Xamarin.Essentials 받아야 모든 `OnRequestPermissionsResult`합니다. 다음 코드를 모두 추가할 `Activity` 클래스:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    추가 설정이 필요 없습니다.

    # <a name="uwptabuwp"></a>[UWP](#tab/uwp)

    추가 설정이 필요 없습니다.

    -----

6. 에 따라 합니다 [Xamarin.Essentials 가이드](index.md) 복사 하 고 각 기능에 대 한 코드 조각에 붙여넣을 수 있도록 합니다.

## <a name="other-resources"></a>기타 리소스

Xamarin 방문 접하는 개발자 것이 좋습니다 [Xamarin 개발을 시작 하기](~/cross-platform/getting-started/index.md)합니다.

방문 합니다 [Xamarin.Essentials GitHub 리포지토리](http://github.com/xamarin/Essentials) 보려면 현재 소스 코드를 다음 단계를 샘플을 실행 하 고 저장소를 닫습니다. 커뮤니티 기여는 시작!

탐색 합니다 [API 설명서](xref:Xamarin.Essentials) Xamarin.Essentials의 모든 기능에 대 한 합니다.
