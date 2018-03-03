---
title: "네이티브 참조"
description: "네이티브 참조 Xamarin.iOS 또는 Xamarin.Mac 프로젝트 또는 바인딩 프로젝트에 네이티브 프레임 워크를 포함 하는 기능을 제공 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 29b405c61b745c26c74318243f75e5809ecfdd7d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="native-references"></a>네이티브 참조

_네이티브 참조 Xamarin.iOS 또는 Xamarin.Mac 프로젝트 또는 바인딩 프로젝트에 네이티브 프레임 워크를 포함 하는 기능을 제공 합니다._


IOS 8.0 이후 응용 프로그램 확장과 Xcode에서 주 앱 간에 코드를 공유 하는 포함 된 프레임 워크를 만들 수 있었지만 했습니다. 네이티브 참조 기능 사용를 Xamarin.iOS에 이러한 포함 된 프레임 워크 (Xcode를 사용 하 여 만든)을 사용할 수 있습니다.
 
> [!IMPORTANT]
> **참고:** Xamarin.iOS 또는 Xamarin.Mac 프로젝트의 모든 형식에서 포함 된 프레임 워크를 만들 수 없습니다, 네이티브 참조만 허용 하며 기존 네이티브 (Objective-c) 프레임 워크의 소비 합니다.




<a name="Terminology" />

## <a name="terminology"></a>용어

8 이상, iOS에서 **포함 된 프레임 워크** 정적으로 연결 된 및 동적으로 연결 된 프레임 워크도 포함 될 수 있습니다. 적절 하 게 분산 하, 있도록 만들어야을 모두 포함 하는 "fat" 프레임 워크에 자신의 _분할 영역_ 응용 프로그램과 함께 지원 하 고 각 장치 아키텍처에 대 한 합니다.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>정적 포트 대 동적 프레임 워크

**정적 프레임 워크** 컴파일 타임에 연결 되어 있는 **동적 프레임 워크** 런타임 시 연결 되 고 따라서 다시 링크 하지 않고 수정할 수 있습니다. 사용 하는 iOS 8 하기 전에 모든 타사 프레임 워크를 사용한 경우는 **정적 프레임 워크** 컴파일된 앱에 있습니다. Apple의 참조 [동적 라이브러리 프로그래밍](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) 자세한 세부 정보에 대 한 설명서입니다.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>포함 된 vs입니다. 시스템 프레임 워크

**프레임 워크 포함** 앱 번들에 포함 되어 있고 해당 샌드박스를 통해 특정 응용 프로그램에 액세스할 수 있습니다. **시스템 프레임 워크** 운영 체제 수준에서 저장 되 고 장치에서 모든 앱에 사용할 수 있습니다. 현재만 Apple에 운영 체제 수준 프레임 워크를 만들 수 있습니다.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>씬 vs입니다. Fat 프레임 워크

**프레임 워크 씬** 의 컴파일된 코드는 특정 시스템 아키텍처를 포함 합니다. 여기서 **Fat 프레임 워크** 여러 아키텍처에 대 한 코드를 포함 합니다. 각 아키텍처 관련 코드 베이스는 프레임 워크 컴파일될 라고는 _조각_합니다. 따라서 예를 들어 두 개의 iOS 시뮬레이터 아키텍처 (i386 및 X86_64)에 대해 컴파일된는 프레임 워크, 있을 경우 포함 두 슬라이스입니다.

이 샘플 프레임 워크 응용 프로그램과 함께 배포 하려는 시뮬레이터에서 올바르게 실행는 프레임 워크는 iOS 장치에 대 한 코드 관련 조각을 포함 되어 있지 않는 장치에서 실패 합니다. 모든 경우에는 프레임 워크 작동 하는지 되도록 게도 필요 arm64, armv7 및 armv7s 같은 장치 관련 조각을 포함 하도록 합니다.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>포함 된 프레임 워크 사용

Xamarin.iOS 또는 Xamarin.Mac 앱에 포함 된 프레임 워크에서 작동 하기 위해 완료 해야 하는 두 단계가: Fat 프레임 워크를 만들고 프레임 워크를 포함 합니다.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Fat 프레임 워크 만들기

위에서 설명한 대로 응용 프로그램에 포함 된 프레임 워크를 사용 하려면 모든 앱에서 실행 되는 장치에 대 한 시스템 아키텍처 분할 영역을 포함 하는 Fat 프레임 워크 이어야 합니다.

프레임 워크와 사용 중인 응용 프로그램 같은 Xcode 프로젝트의 경우이 문제가 되지 않습니다는 프레임 워크와 동일한 빌드 설정을 사용 하 여 앱을 모두 Xcode 빌드 됩니다. Xamarin 앱은 포함 된 프레임 워크를 만들 수 없는 때문에이 기술을 사용할 수 없습니다.

이 문제를 해결 하는 `lipo` 모든 필요한 조각이 포함 된 하나의 Fat 프레임 워크에 둘 이상의 프레임 워크를 병합 하는 명령줄 도구를 사용할 수 있습니다. 작업에 대 한 자세한 내용은 `lipo` 명령를 참조 하십시오 우리의 [네이티브 라이브러리 연결](~/ios/platform/native-interop.md) 설명서입니다.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>프레임 워크를 포함합니다.

다음 단계는 기본 참조를 사용 하 여 Xamarin.iOS 또는 Xamarin.Mac 프로젝트에는 프레임 워크를 포함 해야 합니다.

1. 새로 만들거나 기존 Xamarin.iOS, Xamarin.Mac 또는 바인딩 프로젝트를 엽니다.
2. 에 **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **네이티브 참조 추가**: 

    [ ![](native-references-images/ref01.png "솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 네이티브 참조 추가 선택 합니다.")](native-references-images/ref01.png)
3. **열려** 대화 상자를 포함 하 고 클릭 하려는 네이티브 프레임 워크의 이름을 선택은 **열려** 단추: 

    [ ![](native-references-images/ref02.png "포함 하 고 열기 단추를 클릭 합니다. 네이티브 프레임 워크의 이름을 선택합니다")](native-references-images/ref02.png)
4. 프레임 워크는 프로젝트의 트리에 추가 됩니다. 

    [ ![](native-references-images/ref03.png "프로젝트 트리에 추가할 프레임 워크")](native-references-images/ref03.png)

프로젝트를 컴파일할 때 네이티브 프레임 워크는 앱의 번들에 포함 됩니다.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>응용 프로그램 확장 및 포함 된 프레임 워크

Xamarin.iOS 모노 런타임 프레임 워크와 연결 하려면이 기능을 걸릴 수 있습니다는 내부적으로 (배포 대상의 경우 > = iOS 8.0), 크게 확장을 사용 하 여 앱에 대 한 응용 프로그램 크기를 줄임으로써 (이후 모노 런타임에 대 한 한 번만 포함 됩니다 전체 앱 번들을 각 확장에 대해 한 번씩과 컨테이너 앱에 대 한 번이 아니라)입니다.

모든 확장 iOS 8.0 필요 하기 때문에 프레임 워크를 확장 모노 런타임에 연결 됩니다.

앱에 없는 확장 프로그램과 해당 대상 iOS 

<a name="Summary" />

## <a name="summary"></a>요약

이 문서와 통합 하였습니다 자세히 살펴보고 Xamarin.iOS 또는 Xamarin.Mac 응용 프로그램으로 네이티브 프레임 워크를 포함 합니다.

