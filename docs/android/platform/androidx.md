---
title: AndroidX
description: Xamarin.Android를 사용하여 AndroidX로 앱 개발을 시작하는 방법입니다.
ms.assetid: CC21BD28-EF67-4132-8C0D-CF25B78BA78B
author: JonDouglas
ms.author: jodou
ms.date: 02/20/2020
ms.openlocfilehash: ad6ea2f68fc01183f7ed42e85094f6be5fb3d9f9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "77618913"
---
# <a name="androidx-with-xamarin"></a>Xamarin을 사용하는 AndroidX

_Xamarin.Android를 사용하여 AndroidX로 앱 개발을 시작하는 방법입니다._

AndroidX는 원래 Android Support Library가 대대적으로 개선된 것으로, 기존 것은 더 이상 유지 관리되지 않습니다. **AndroidX** 패키지는 Android 애플리케이션에서 사용할 수 있는 기능 패리티와 새 라이브러리를 제공하여 Android Support Library를 완전히 대체합니다.

AndroidX에는 다음과 같은 기능이 포함됩니다.

- 이제 AndroidX 내의 모든 패키지에는 `androidx`로 시작되는 일관된 네임스페이스가 있습니다. 즉, 모든 Android Support Library 패키지는 해당 `androidx.*` 패키지에 매핑됩니다.
- `androidx` 패키지는 개별적으로 유지 관리되고 업데이트됩니다. 따라서 AndroidX 라이브러리를 개별적으로 업데이트할 수 있습니다.
- Android Support Library v28부터 릴리스가 더 이상 제공되지 않습니다. 대신 모든 개발이 `androidx`에 포함됩니다.

![AndroidX 로고](~/android/platform/androidx-images/AndroidXLogo.png)

## <a name="requirements"></a>요구 사항

Xamarin 기반 앱에서 AndroidX 기능을 사용하려면 다음 목록이 필요합니다.

- **Visual Studio** - Windows에서 Visual Studio 2019 버전 16.4 이상으로 업데이트합니다. macOS에서 Mac용 Visual Studio 2019 버전 8.4 이상으로 업데이트합니다.
- **Xamarin.Android** - Xamarin.Android 10.0 이상은 Visual Studio와 함께 설치되어야 합니다(Xamarin.Android는 Windows에서 **.NET을 이용한 모바일 개발** 워크로드의 일부로 자동 설치되고 **Mac용 Visual Studio 설치 관리자**의 일부로 설치됨).
- **Java Developer Kit** - Xamarin.Android 10.0 개발에는 JDK 8이 필요합니다. Microsoft의 OpenJDK 배포는 Visual Studio의 일부로 자동 설치됩니다.
- **Android SDK** - Android SDK API 28 이상은 Android SDK Manager를 통해 설치해야 합니다.

## <a name="get-started"></a>시작

Android 프로젝트 내에 [AndroidX NuGet 패키지](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22)를 포함하여 AndroidX를 시작할 수 있습니다. [Visual Studio](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio) 또는 [Mac용 Visual Studio](https://docs.microsoft.com/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac)에서 패키지를 설치하고 사용하는 방법에 대해 자세히 알아보세요.

## <a name="behavior-changes"></a>동작 변경

AndroidX는 Android Support Library를 다시 디자인한 것이기 때문에 Android Support Library를 사용하여 빌드된 Android 애플리케이션에 영향을 주는 마이그레이션 단계를 포함합니다.

### <a name="package-name-change"></a>패키지 이름 변경
이전 패키지와 새 패키지 간에 패키지 이름이 변경되었습니다. 아래에서 이러한 변경의 예를 확인할 수 있습니다.

| 이전                    | 단추를 사용하여 새                    |
| ---------------------- | ---------------------- |
| android.support.**     | androidx.@             |
| android.design.**      | com.google.android.material.@ |
| android.support.test.** | androidx.test.@       |
| android.arch.**        | androidx.@             |
| android.arch.persistence.room.** | androidx.room.@ |
| android.arch.persistence.** | androidx.sqlite.@ |

패키지 이름 지정에 대한 자세한 내용은 [다음 설명서를 참조하세요](https://developer.android.com/jetpack/androidx/migrate#artifact_mappings).

## <a name="migration-tooling"></a>마이그레이션 도구

애플리케이션에 대해 알아야 할 마이그레이션 단계 세 가지가 있습니다.

1. 애플리케이션에 **Android Support Library 네임스페이스가 포함되어 있고 이를 AndroidX 네임스페이스로 마이그레이션**하려는 경우 **AndroidX로 마이그레이션** IDE 도구를 사용하여 대부분의 네임스페이스 시나리오를 처리할 수 있습니다. 

Visual Studio 2019 내에서 **도구 > 옵션 > Xamarin > Android 설정**을 통해 **AndroidX Migrator**를 사용하도록 설정합니다(Mac용 Visual Studio에서는 이 단계를 건너뛸 수 있음).

![AndroidX Migrator 사용](~/android/platform/androidx-images/EnableAndroidXMigrator.png)

프로젝트를 마우스 오른쪽 단추로 클릭하고 **AndroidX로 마이그레이션**을 선택합니다.

![AndroidX로 마이그레이션](~/android/platform/androidx-images/MigrateToAndroidX.png)

> [!NOTE] 
> 이 도구가 다루지 않는 시나리오의 경우 몇 가지 수동 네임스페이스 변경을 수행해야 합니다. 올바른 패키지를 자동으로 매핑해 드리지만 프로젝트 마이그레이션에 도움이 되는 공식 [아티팩트 매핑](https://developer.android.com/jetpack/androidx/migrate/artifact-mappings) 및 [클래스 매핑](https://developer.android.com/jetpack/androidx/migrate/class-mappings)을 직접 확인하는 것이 좋습니다.

2. 애플리케이션에 **AndroidX 네임스페이스로 마이그레이션되지 않은 종속성**이 포함되어 있는 경우 [Android Support Library를 AndroidX로 마이그레이션 패키지](https://www.nuget.org/packages/Xamarin.AndroidX.Migration)를 사용해야 합니다.
3. 애플리케이션에 **AndroidX 네임스페이스 마이그레이션이 필요한 종속성이 포함되어 있지 않으면** [현재 NuGet의 AndroidX 라이브러리](https://www.nuget.org/packages?q=Tags%3A%22AndroidX%22+Authors%3A%22Microsoft%22)를 사용할 수 있습니다.

## <a name="troubleshooting"></a>문제 해결

- AndroidX 내의 특정 아키텍처 패키지는 Support Library 버전과 충돌합니다. 이 문제를 해결하려면 이러한 패키지의 AndroidX 버전을 사용하고 Support Library 버전을 제거해야 합니다. 예를 들어 프로젝트에서 `Xamarin.Android.Arch.Work.Runtime`을 참조하는 경우 새로 추가된 `AndroidX.Work` 패키지의 형식과 충돌합니다.

## <a name="summary"></a>요약

이 문서에서는 AndroidX를 소개하고 AndroidX를 사용하여 Xamarin.Android 개발을 위한 최신 도구 및 패키지를 설치 및 구성하는 방법을 설명했습니다. AndroidX의 개요를 제공했습니다. 여기에는 AndroidX를 사용하는 앱을 만드는 과정을 시작하는 데 도움이 되는 API 설명서 및 Android 개발자 항목에 대한 링크가 포함되어 있습니다. 기존 앱에 영향을 줄 수 있는 가장 중요한 AndroidX 동작 변경 내용 및 문제 해결 항목도 설명했습니다.

## <a name="related-links"></a>관련 링크

- [AndroidX 소개 | The Xamarin Show](https://www.youtube.com/watch?v=M_l3RjTev5A)
- [AndroidX](https://developer.android.com/jetpack/androidx)
- [Xamarin AndroidX GitHub 리포지토리](https://github.com/xamarin/AndroidX)
- [Xamarin AndroidX 마이그레이션 GitHub 리포지토리](https://github.com/xamarin/XamarinAndroidXMigration)
