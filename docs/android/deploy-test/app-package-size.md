---
title: 애플리케이션 패키지 크기
description: 이 아티클에서는 배포의 디버그 및 릴리스 단계에서 효율적인 패키지 배포에 사용할 수 있는 관련 전략과 Xamarin.Android 애플리케이션 패키지의 구성 요소를 살펴봅니다.
ms.prod: xamarin
ms.assetid: 8D70CDDD-3D3C-9949-8045-AB8F93D18E74
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 9bff233b5507e3456ba3620315bd967d0ac7018d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69525786"
---
# <a name="application-package-size"></a>애플리케이션 패키지 크기

_이 아티클에서는 배포의 디버그 및 릴리스 단계에서 효율적인 패키지 배포에 사용할 수 있는 관련 전략과 Xamarin.Android 애플리케이션 패키지의 구성 요소를 살펴봅니다._


## <a name="overview"></a>개요

Xamarin.Android는 다양한 메커니즘을 사용하여 패키지 크기를 최소화하는 한편 효율적인 디버그 및 릴리스 배포 프로세스를 유지합니다. 이 아티클에서는 Xamarin.Android 릴리스 및 디버그 배포 워크플로를 살펴보고, Xamarin.Android 플랫폼으로 소형 애플리케이션 패키지를 빌드하고 릴리스하하는 방법을 살펴봅니다.


## <a name="release-packages"></a>릴리스 패키지

완전히 포함된 애플리케이션을 제공하기 위해 패키지에는 애플리케이션, 관련 라이브러리, 콘텐츠, Mono 런타임 및 필요한 BCL(기본 클래스 라이브러리) 어셈블리가 포함되어야 합니다. 예를 들어 기본 "Hello World" 템플릿을 사용할 경우 전체 패키지 빌드의 콘텐츠는 다음과 같이 표시됩니다.

[![링커 실행 전 패키지 크기](app-package-size-images/hello-world-package-size-before-linker.png)](app-package-size-images/hello-world-package-size-before-linker.png#lightbox)

15.8MB는 예상보다 더 큰 다운로드 크기입니다. 문제는 BCL 라이브러리입니다. mscorlib, System 및 Mono.Android를 포함하고 있고, 애플리케이션을 실행하는 데 필요한 많은 구성 요소를 제공하기 때문입니다. 그러나 사용자가 애플리케이션에서 사용하지 않을 수 있는 기능도 제공하므로 이러한 구성 요소는 제외하는 것이 좋습니다.

배포용 애플리케이션을 빌드할 때는 애플리케이션을 살펴보고 직접 사용되지 않는 코드를 제거하는 연결이라고 하는 프로세스를 실행합니다. 이 프로세스는 [가비지 수집](~/android/internals/garbage-collection.md)에서 힙 할당 메모리에 제공하는 기능과 유사합니다. 단, 연결 프로세스는 개체를 통해 작동하지 않고 코드를 통해 작동합니다. 예를 들어 System.dll에 이메일을 주고받는 데 사용되는 완전한 네임스페이스가 있지만 애플리케이션에서 이 기능을 활용하지 않는 경우 해당 코드는 공간 낭비일 뿐입니다. Hello World 애플리케이션에서 링커를 실행한 후 패키지의 모습은 다음과 같습니다.

[![링커 실행 후 패키지 크기](app-package-size-images/hello-world-package-size-after-linker.png)](app-package-size-images/hello-world-package-size-after-linker.png#lightbox)

보시다시피 이는 사용하지 않는 BCL 공간을 상당 부분 제거해 줍니다. 최종 BCL 크기는 애플리케이션에서 실제로 사용하는 구성 요소에 따라 달라집니다. 예를 들어 ApiDemo라는 좀 더 견고한 샘플 애플리케이션을 살펴보면 ApiDemo가 Hello, World보다 BCL을 더 많이 사용하기 때문에 BCL 구성 요소의 크기가 증가한 것을 알 수 있습니다.

![연결 후 ApiDemo 패키지 크기](app-package-size-images/api-demo-package-size-after-linker.png)

여기에 설명된 것처럼 애플리케이션 패키지 크기는 일반적으로 애플리케이션과 종속성보다 큰 약 2.9MB가 됩니다.


## <a name="debug-packages"></a>디버그 패키지

디버그 빌드에서는 처리 방식이 약간 달라집니다. 디바이스에 반복적으로 재배포하는 경우 애플리케이션의 속도가 가능한 빨라야 하므로 디버그 패키지는 크기보다는 배포 속도에 맞게 최적화됩니다.

Android는 상대적으로 패키지를 복사하고 설치하는 속도가 느리므로 패키지 크기를 가능하면 작게 유지하는 것이 좋습니다. 위에서 설명한 것처럼 패키지 크기를 최소화할 수 있는 한 가지 방법은 링커입니다. 하지만 연결은 속도가 느리고, 일반적으로 마지막 배포 이후에 변경된 애플리케이션의 일부만 배포하는 것이 좋습니다. 이를 위해서는 핵심 Xamarin.Android 구성 요소와 애플리케이션을 구분합니다.

디바이스에서 처음 디버깅할 때는 *공유 런타임*과 *공유 플랫폼*이라는 대규모 패키지 두 개를 복사합니다. 공유 런타임에는 Mono Runtime 및 BCL이 포함되고, 공유 플랫폼에는 Android API 수준별 어셈블리가 포함됩니다.

[![공유 런타임 패키지 크기](app-package-size-images/shared-runtime-package-size.png)](app-package-size-images/shared-runtime-package-size.png#lightbox)

이러한 핵심 구성 요소를 복사하는 작업은 시간이 상당히 오래 걸리므로 한 번만 수행하며, 그래도 디버그 모드에서 실행 중인 모든 후속 애플리케이션이 이러한 구성 요소를 활용할 수 있습니다. 마지막으로, 작고 빠른 실제 애플리케이션을 복사합니다.

![실제 애플리케이션은 작습니다.](app-package-size-images/hello-world-debug-application-no-link.png)

### <a name="fast-assembly-deployment"></a>빠른 어셈블리 배포

*빠른 어셈블리 배포* 빌드 옵션은 애플리케이션의 패키지에 어셈블리를 포함하지 않고, 디바이스에 한 번만 바로 어셈블리를 설치하고, 마지막 배포 이후로 수정된 파일에만 복사하여 디버그 설치 패키지의 크기를 더 줄이는 데 사용할 수 있습니다.

*빠른 어셈블리 배포*를 활성화하려면 다음을 수행합니다.

1. 솔루션 탐색기에서 Android 프로젝트를 마우스 오른쪽 단추로 클릭하고 **옵션**을 선택합니다.

2. 프로젝트 옵션 대화 상자에서 **Android 빌드**를 선택합니다.  

    ![프로젝트 옵션 Android 빌드](app-package-size-images/fastdev0.png)

3. **공유 Mono 런타임 사용** 확인란과 **빠른 어셈블리 배포** 확인란을 선택합니다.  

    ![패키징 탭 아래에서 선택된 확인란](app-package-size-images/fastdev.png)

4. **확인** 단추를 클릭하여 변경 내용을 저장하고 프로젝트 옵션 대화 상자를 닫습니다.


다음에 애플리케이션이 디버그용으로 빌드될 때 어셈블리가 디바이스에 바로 설치되고(아직 설치되지 않은 경우) 소형 애플리케이션 패키지(어셈블리를 포함하지 않는)가 디바이스에 설치됩니다. 따라서 테스트를 위해 실행 중인 애플리케이션에 변경 내용이 적용되는 데 걸리는 시간이 단축됩니다.

공유 런타임 및 공유 플랫폼의 긴 첫 배포를 완료하면 애플리케이션을 변경할 때마다 새 버전을 쉽고 빠르게 배포할 수 있으므로 변경/배포/실행 주기가 단축됩니다.


## <a name="summary"></a>요약

이 아티클에서는 Xamarin.Android 릴리스 및 디버그 프로필 패키징의 패싯을 살펴보았습니다. 또한 Android 플랫폼용 Mono에서 배포의 디버그 및 릴리스 단계에서 효율적인 패키지 배포를 촉진하기 위해 사용하는 전략도 살펴보았습니다.
