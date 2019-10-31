---
title: Xamarin.Android 앱 테스트, 최적화 및 배포
description: 이 섹션에는 애플리케이션을 테스트하고, 해당 성능을 최적화하고, 릴리스할 준비를 하고, 인증서를 사용하여 서명하고, 앱 스토어에 게시하는 방법을 설명하는 가이드가 포함되어 있습니다.
ms.prod: xamarin
ms.assetid: 568C0B85-EFF3-AF6F-5605-95055193D367
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
ms.openlocfilehash: 70d03e2ed35970835e0343bc416041845e0edb29
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028041"
---
# <a name="deployment-and-testing"></a>배포 및 테스트

이 섹션에는 애플리케이션을 테스트하고, 해당 성능을 최적화하고, 릴리스할 준비를 하고, 인증서를 사용하여 서명하고, 앱 스토어에 게시하는 방법을 설명하는 가이드가 포함되어 있습니다.

## <a name="application-package-sizesapp-package-sizemd"></a>[애플리케이션 패키지 크기](app-package-size.md)

이 아티클에서는 배포의 디버그 및 릴리스 단계에서 효율적인 패키지 배포에 사용할 수 있는 관련 전략과 Xamarin.Android 애플리케이션 패키지의 구성 요소를 살펴봅니다.

## <a name="building-appsbuilding-appsindexmd"></a>[앱 빌드](building-apps/index.md)

이 섹션에서는 빌드 프로세스의 작동 원리를 설명하고, ABI 관련 APK를 빌드하는 방법을 설명합니다.

## <a name="command-line-emulatorcommand-line-emulatormd"></a>[명령줄 에뮬레이터](command-line-emulator.md)

이 아티클에서는 명령줄을 통해 에뮬레이터를 시작하는 방법을 간략히 살펴봅니다.

## <a name="debuggingandroiddeploy-testdebuggingindexmd"></a>[디버깅](~/android/deploy-test/debugging/index.md)

섹션의 가이드를 통해 Android 에뮬레이터, 실제 Android 디바이스 및 디버그 로그를 사용하여 앱을 디버그할 수 있습니다.

## <a name="setting-the-debuggable-attributeandroiddeploy-testdebuggable-attributemd"></a>[디버깅 가능한 특성 설정](~/android/deploy-test/debuggable-attribute.md)

이 아티클에서는 `adb` 같은 도구가 JVM과 통신할 수 있도록 디버그 가능한 특성을 설정하는 방법을 설명합니다.

## <a name="environmentenvironmentmd"></a>[환경](environment.md)

이 아티클에서는 프로그램 실행에 영향을 주는 Android 시스템 속성과 Xamarin.Android 실행 환경을 설명합니다.

## <a name="gdbgdbmd"></a>[GDB](gdb.md)

이 아티클에서는 Xamarin.Android 애플리케이션에 `gdb`를 사용하는 방법을 설명합니다.

## <a name="installing-a-system-appinstall-system-appmd"></a>[시스템 앱 설치](install-system-app.md)

이 가이드에서는 Xamarin.Android 앱을 Android 디바이스의 시스템 애플리케이션이나 사용자 지정 ROM의 일부로 설치하는 방법을 설명합니다.

## <a name="linking-on-androidlinkermd"></a>[Android의 연결](linker.md)

이 아티클에서는 Xamarin.Android에서 애플리케이션의 최종 크기를 줄이기 위해 사용하는 연결 프로세스를 설명합니다. 수행 가능한 다양한 수준의 연결을 설명하고, 링커를 사용하면서 발생할 수 있는 오류를 완화하기 위한 몇 가지 지침과 문제 해결 관련 조언을 제공합니다.

## <a name="xamarinandroid-performanceandroiddeploy-testperformancemd"></a>[Xamarin.Android Performance](~/android/deploy-test/performance.md)

Xamarin.Android로 빌드된 애플리케이션의 성능을 높이기 위한 많은 기술이 있습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 애플리케이션에서 소비하는 메모리의 양을 크게 줄일 수 있습니다.

## <a name="profiling-android-appsandroiddeploy-testprofilingmd"></a>[Android 앱 프로파일링](~/android/deploy-test/profiling.md)

이 가이드에서는 프로파일러 도구를 사용하여 Android 앱의 성능 및 메모리 사용을 검사하는 방법을 설명합니다.

## <a name="preparing-an-application-for-releaseandroiddeploy-testrelease-prepindexmd"></a>[릴리스할 애플리케이션 준비](~/android/deploy-test/release-prep/index.md)

애플리케이션을 코딩하고 테스트한 후에 배포할 패키지를 준비해야 합니다. 이 패키지를 준비하는 첫 번째 작업은 릴리스할 애플리케이션을 빌드하는 것입니다. 여기에는 주로 일부 애플리케이션 특성을 설정하는 작업이 포함됩니다.

## <a name="signing-the-android-application-packageandroiddeploy-testsigningindexmd"></a>[Android 애플리케이션 패키지에 서명](~/android/deploy-test/signing/index.md)

Android 서명 ID를 만들고, Android 애플리케이션에 대한 새 서명 인증서를 만들고, 서명 인증서로 애플리케이션에 서명하는 방법을 설명합니다. 또한 이 항목에서는 *임시* 배포를 위해 디스크에 앱을 내보내는 방법을 설명합니다. 앱 스토어를 통하지 않고 Android 디바이스에 생성된 APK를 사이드로드할 수 있습니다.

## <a name="publishing-an-applicationandroiddeploy-testpublishingindexmd"></a>[애플리케이션 게시](~/android/deploy-test/publishing/index.md)

이 아티클 시리즈에서는 Xamarin.Android를 사용하여 만든 애플리케이션의 공용 배포에 대한 단계를 설명합니다. 배포는 이메일, 개인 웹 서버, Google Play 또는 Android용 Amazon 앱 스토어와 같은 채널을 통해 수행될 수 있습니다.
