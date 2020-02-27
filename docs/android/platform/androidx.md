---
title: AndroidX
description: Xamarin.ios를 사용 하 여 AndroidX 앱 개발을 시작 하는 방법입니다.
ms.assetid: CC21BD28-EF67-4132-8C0D-CF25B78BA78B
author: JonDouglas
ms.author: jodou
ms.date: 02/20/2020
ms.openlocfilehash: ad6ea2f68fc01183f7ed42e85094f6be5fb3d9f9
ms.sourcegitcommit: 2836f2003a5b745b042ee6003a3d6a11b9139e44
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618913"
---
# <a name="androidx-with-xamarin"></a>Xamarin을 사용 하는 AndroidX

_Xamarin.ios를 사용 하 여 AndroidX 앱 개발을 시작 하는 방법입니다._

AndroidX는 더 이상 유지 관리 되지 않는 원래 Android 지원 라이브러리의 주요 개선 사항입니다. **Androidx** 패키지는 android 응용 프로그램에서 사용할 수 있는 기능 패리티와 새 라이브러리를 제공 하 여 Android 지원 라이브러리를 완전히 대체 합니다.

AndroidX에는 다음과 같은 기능이 포함 되어 있습니다.

- 이제 AndroidX 내의 모든 패키지에는 `androidx`에서 시작 하는 일관 된 네임 스페이스가 있습니다. 즉, 모든 Android 지원 라이브러리 패키지는 해당 `androidx.*` 패키지에 매핑됩니다.
- `androidx` 패키지는 개별적으로 유지 관리 및 업데이트 됩니다. 즉, AndroidX 라이브러리를 서로 독립적으로 업데이트할 수 있습니다.
- Android 지원 라이브러리의 v28을 통해 릴리스가 더 이상 제공 되지 않습니다. 대신 모든 개발이 `androidx`에 포함 됩니다.

![AndroidX 로고](~/android/platform/androidx-images/AndroidXLogo.png)

## <a name="requirements"></a>요구 사항

다음 목록은 Xamarin 기반 앱에서 AndroidX 기능을 사용 하는 데 필요 합니다.

- Visual **studio에서** visual studio 2019 버전 16.4 이상으로 업데이트 합니다. MacOS에서 Mac 용 Visual Studio 2019 버전 8.4 이상으로 업데이트 합니다.
- **Xamarin android** -xamarin. android 10.0 이상이 Visual Studio와 함께 설치 되어야 합니다. Xamarin은 Windows에서 .net 작업을 **사용 하 여 모바일 개발** 의 일부로 자동으로 설치 되 고 **Mac용 Visual Studio 설치 관리자**의 일부로 설치 됩니다.
- **Java 개발자 키트** -Xamarin Android 10.0 개발에는 JDK 8이 필요 합니다. Microsoft의 OpenJDK 배포는 자동으로 Visual Studio의 일부로 설치 됩니다.
- **Android SDK** -Android SDK API 28 이상은 Android SDK Manager를 통해 설치 해야 합니다.

## <a name="get-started"></a>시작

Android 프로젝트 내에 [Androidx NuGet 패키지](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22) 를 포함 하 여 androidx를 시작할 수 있습니다. [Visual Studio](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio) 또는 [Mac용 Visual Studio](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac) 에서 패키지를 설치 하 고 사용 하는 방법에 대해 자세히 알아보세요.

## <a name="behavior-changes"></a>동작 변경

AndroidX는 Android 지원 라이브러리를 다시 디자인 하기 때문에 Android 지원 라이브러리를 사용 하 여 빌드된 Android 응용 프로그램에 영향을 주는 마이그레이션 단계를 포함 합니다.

### <a name="package-name-change"></a>패키지 이름 변경
이전 패키지와 새 패키지 간에 패키지 이름이 변경 되었습니다. 아래에서 이러한 변경의 예를 확인할 수 있습니다.

| 원래의                    | 새로 만들기                    |
| ---------------------- | ---------------------- |
| android. 지원. * *     | androidx. @             |
| android. 디자인. * *      | com. google. android. @ |
| android. 지원. * * | androidx. 테스트. @       |
| android. 아치. * *        | androidx. @             |
| android. 지 속성 * * | androidx 공간. @ |
| android. 지 속성. * * | androidx. sqlite. @ |

패키지 이름 지정에 대 한 자세한 [내용은 다음 설명서를 참조](https://developer.android.com/jetpack/androidx/migrate#artifact_mappings)하세요.

## <a name="migration-tooling"></a>마이그레이션 도구

응용 프로그램에 대해 알고 싶은 마이그레이션 단계는 세 가지가 있습니다.

1. 응용 프로그램에 **Android 지원 라이브러리 네임 스페이스가 포함 되어 있고이를 AndroidX 네임 스페이스로 마이그레이션하려면** **androidx** IDE 도구를 사용 하 여 대부분의 네임 스페이스 시나리오를 처리할 수 있습니다. 

Visual Studio 2019 내에서 **Xamarin > Android 설정 > 도구 > 옵션** 을 통해 **Androidx Migrator** 를 사용 하도록 설정 합니다 (Mac용 Visual Studio에서이 단계를 건너뛸 수 있음).

![AndroidX Migrator 사용](~/android/platform/androidx-images/EnableAndroidXMigrator.png)

프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **AndroidX로 마이그레이션합니다**.

![AndroidX로 마이그레이션](~/android/platform/androidx-images/MigrateToAndroidX.png)

> [!NOTE] 
> 도구가 다루지 않는 시나리오에 대 한 몇 가지 수동 네임 스페이스를 변경 해야 합니다. 올바른 패키지를 매핑하는 동안 프로젝트 마이그레이션에 도움이 되는 공식 [아티팩트 매핑](https://developer.android.com/jetpack/androidx/migrate/artifact-mappings) 및 [클래스 매핑을](https://developer.android.com/jetpack/androidx/migrate/class-mappings) 살펴보는 것이 좋습니다.

2. 응용 프로그램에 **androidx 네임 스페이스로 마이그레이션되지 않은 종속성**이 포함 된 경우 [Android 지원 라이브러리를 사용 하 여 Androidx 마이그레이션 패키지](https://www.nuget.org/packages/Xamarin.AndroidX.Migration) 를 사용 해야 합니다.
3. 응용 프로그램에 **androidx 네임 스페이스 마이그레이션이 필요한 종속성이 포함 되어 있지**않으면 [현재 NuGet에서 androidx 라이브러리](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22)를 사용할 수 있습니다.

## <a name="troubleshooting"></a>문제 해결

- AndroidX 내의 특정 아키텍처 패키지는 지원 라이브러리 버전과 충돌 합니다. 이 문제를 해결 하려면 이러한 패키지의 AndroidX 버전을 사용 하 고 지원 라이브러리 버전을 제거 해야 합니다. 예를 들어 프로젝트에서 `Xamarin.Android.Arch.Work.Runtime` 참조 하는 경우 새로 추가 된 `AndroidX.Work` 패키지의 형식과 충돌 합니다.

## <a name="summary"></a>요약

이 문서에서는 AndroidX를 소개 하 고 AndroidX를 사용 하 여 Xamarin.ios 개발용 최신 도구 및 패키지를 설치 하 고 구성 하는 방법을 설명 했습니다. AndroidX의 개요를 제공 했습니다. AndroidX를 사용 하 여 앱을 만드는 작업을 시작 하는 데 도움이 되는 API 설명서 및 Android 개발자 항목에 대 한 링크가 포함 되어 있습니다. 또한 가장 중요 한 AndroidX 동작 변경 내용과 기존 앱에 영향을 줄 수 있는 문제 해결 항목도 강조 표시 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [AndroidX 소개 | Xamarin Show](https://www.youtube.com/watch?v=M_l3RjTev5A)
- [AndroidX](https://developer.android.com/jetpack/androidx)
- [Xamarin AndroidX GitHub 리포지토리](https://github.com/xamarin/AndroidX)
- [Xamarin AndroidX 마이그레이션 GitHub 리포지토리](https://github.com/xamarin/XamarinAndroidXMigration)
