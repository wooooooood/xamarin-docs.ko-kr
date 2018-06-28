---
title: Xamarin.Essentials 시작
description: Xamarin.Essentials iOS, Android 또는 UWP 작동 하는 단일 플랫폼 API를 제공 합니다. 응용 프로그램에서 액세스할 수 있는 사용자 인터페이스를 만드는 방법을 관계 없이 코드를 공유 합니다.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a42086f70eb81a761358655b3effb9f8f934c8d4
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067320"
---
# <a name="get-started-with-xamarinessentials"></a>Xamarin.Essentials 시작

![시험판 NuGet](~/media/shared/pre-release.png)

Xamarin.Essentials iOS, Android 또는 UWP 작동 하는 단일 플랫폼 API를 제공 합니다. 응용 프로그램에서 액세스할 수 있는 사용자 인터페이스를 만드는 방법을 관계 없이 코드를 공유 합니다.

## <a name="platform-support"></a>플랫폼 지원

Xamarin.Essentials 다음 플랫폼 및 운영 체제를 지원합니다.

| 플랫폼 | 버전 |
| --- | --- |
| Android | 4.4 (API 19) 이상 |
| iOS |10.0 이상이 |
| UWP | 10.0.16299.0 이상 |

## <a name="installation"></a>설치

Visual Studio를 사용 하 여 기존 또는 새 프로젝트에 추가할 수 있는 NuGet 패키지로 Xamarin.Essentials ´ ù.

1. 다운로드 및 설치 [Visual Studio](http://visualstudio.com) 와 [Xamarin 용 도구 Visual Studio](~/cross-platform/get-started/installation/index.md)합니다.

2. 기존 프로젝트를 열거나에서 비어 있는 앱 템플릿을 사용 하 여 새 프로젝트를 만들 **Visual Studio C#** (Android, iPhone 및 iPad 또는 플랫폼 간). **중요 한**: 16299 또는 더 높은 빌드 프로젝트 속성에 설정 된 경우 UWP 프로젝트에 추가 확인 합니다.

3. 추가 **Xamarin.Essentials** 각 프로젝트에 NuGet 패키지:

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    솔루션 탐색기 창에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **NuGet 패키지 관리**합니다. 검색할 **Xamarin.Essentials** 에 패키지를 설치 하 고 **모든** Android, iOS, UWP, 및 표준.NET 라이브러리를 포함 한 프로젝트입니다.

    > [!TIP]
    > 확인의 **Include 시험판** 동안 상자는 [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) 는 미리 보기로 제공 합니다.

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    솔루션 탐색기 창에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > NuGet 패키지 추가 중...** . 검색할 **Xamarin.Essentials** 에 패키지를 설치 하 고 **모든** Android, iOS 및 표준.NET 라이브러리를 포함 한 프로젝트입니다.

    > [!TIP]
    > 확인은 **시험판 패키지를 표시** 동안 상자는 [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) 는 미리 보기로 제공 합니다.

    -----

4. Api를 참조 하기 위해 모든 C# 클래스에 Xamarin.Essentials에 대 한 참조를 추가 합니다.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials는 플랫폼별 설치를 필요합니다.

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials 4.4, API 수준 19, 해당 하는 최소 Android 버전을 지원 하지만 컴파일 대상 Android 버전 8.1 여야 합니다. 해당 API 수준 27 하 합니다. (Visual Studio에서 이러한 두 가지 버전은 설정 Android 매니페스트 탭에서 Android 프로젝트에 대 한 프로젝트 속성 대화 상자. Mac 용 Visual Studio에서 있습니다 하는 프로젝트 옵션 대화 상자에서 설정 Android 응용 프로그램 탭에서 Android 프로젝트에 대 한 합니다.) 
    
    Xamarin.Essentials는 27.0.2 필요한 Xamarin.Android.Support 라이브러리의 버전을 설치 합니다. 응용 프로그램에 필요한 다른 Xamarin.Android.Support 라이브러리 NuGet 패키지 관리자를 사용 하 여 27.0.2 버전도 업데이트 되어야 합니다. 응용 프로그램에서 사용 하는 모든 Xamarin.Android.Support 라이브러리 동일 해야 하며 보다 길거나 같아야 27.0.2 버전입니다. 참조는 [문제 해결 페이지](troubleshooting.md) Xamarin.Essentials NuGet을 추가 하거나 솔루션에서 NuGets 업데이트 문제가 있는 경우.

    Android 프로젝트의 `MainLauncher` 또는 모든 `Activity` 즉 시작된 Xamarin.Essentials에서 초기화 되어야 합니다는 `OnCreate` 메서드:

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Android에 대 한 런타임 권한의 처리 하려면 Xamarin.Essentials 받아야 모든 `OnRequestPermissionsResult`합니다. 다음 코드를 모두 추가 `Activity` 클래스:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    추가 설치 하지 않아도 됩니다.

    # <a name="uwptabuwp"></a>[UWP](#tab/uwp)

    추가 설치 하지 않아도 됩니다.

    -----

6. 에 따라는 [Xamarin.Essentials 가이드](index.md) 를 복사 하 고 각 기능에 대 한 코드 조각을 붙여 넣을 수 있도록 합니다.

## <a name="other-resources"></a>기타 리소스

Xamarin 방문 새 개발자 권장 [Xamarin 개발 시작](~/cross-platform/getting-started/index.md)합니다.

방문는 [Xamarin.Essentials GitHub 리포지토리](http://github.com/xamarin/Essentials) 보려면 현재 소스 코드, 향후 예정 사항 다음 샘플을 실행 하 고 저장소를 닫습니다. 커뮤니티 기여는 시작!

탐색의 [API 설명서](xref:Xamarin.Essentials) Xamarin.Essentials의 모든 기능에 대 한 합니다.
