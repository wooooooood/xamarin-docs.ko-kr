---
title: 네이티브 참조 iOS, Mac 및 바인딩 프로젝트
description: 네이티브 참조를 사용 하면 네이티브 프레임 워크를 Xamarin.ios, Xamarin.ios 또는 바인딩 프로젝트에 포함할 수 있습니다.
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 1ad7a98b92c34cf956e50ebc7a6cec73580f8f04
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765499"
---
# <a name="native-references-in-ios-mac-and-bindings-projects"></a>IOS, Mac 및 바인딩 프로젝트의 네이티브 참조

_네이티브 참조를 사용 하면 네이티브 프레임 워크를 Xamarin.ios 또는 Xamarin.ios 프로젝트 또는 바인딩 프로젝트에 포함할 수 있습니다._

IOS 8.0부터 앱 확장과 Xcode의 주 앱 간에 코드를 공유 하는 임베디드 프레임 워크를 만들 수 있습니다. 네이티브 참조 기능을 사용 하면 Xamarin.ios에서 이러한 포함 프레임 워크 (Xcode로 만든)를 사용할 수 있습니다.

> [!IMPORTANT]
> 모든 형식의 Xamarin.ios 또는 Xamarin.ios 프로젝트에서 포함 된 프레임 워크를 만들 수는 없습니다. 네이티브 참조는 기존 네이티브 (목적-C) 프레임 워크를 사용 하는 경우에만 사용할 수 있습니다.

<a name="Terminology" />

## <a name="terminology"></a>용어

IOS 8 (이상)에서 포함 된 **프레임 워크** 는 정적으로 연결 되 고 동적으로 연결 된 프레임 워크가 될 수 있습니다. 이를 제대로 배포 하려면 앱에서 지원 하려는 각 장치 아키텍처에 대해 모든 _조각을_ 포함 하는 "Fat" 프레임 워크로 만들어야 합니다.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>정적 포트 대 동적 프레임 워크

**정적 프레임** 워크는 런타임에 **동적 프레임 워크가** 연결 되 고 줄임으로써를 다시 연결 하지 않고도 수정할 수 있는 컴파일 시간에 연결 됩니다. IOS 8 이전에 타사 프레임 워크를 사용한 경우 응용 프로그램으로 컴파일된 **정적 프레임 워크** 를 사용 했습니다. 자세한 내용은 Apple의 [동적 라이브러리 프로그래밍](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) 설명서를 참조 하세요.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>포함 된 vs 시스템 프레임 워크

**포함 프레임 워크** 는 앱 번들에 포함 되며 해당 샌드박스를 통해 특정 앱에만 액세스할 수 있습니다. **시스템 프레임 워크** 는 운영 체제 수준에서 저장 되며 장치의 모든 앱에서 사용할 수 있습니다. 현재 Apple 에서만 운영 체제 수준 프레임 워크를 만들 수 있습니다.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>씬 vs Fat 프레임 워크

**씬 프레임 워크** 는 여러 아키텍처에 대 한 코드를 포함 하는 **Fat 프레임 워크** 의 특정 시스템 아키텍처에 대해 컴파일된 코드만 포함 합니다. 프레임 워크로 컴파일되는 각 아키텍처 관련 코드 베이스를 _조각_이라고 합니다. 예를 들어 두 iOS 시뮬레이터 아키텍처 (i386 및 X86_64)에 대해 컴파일된 프레임 워크가 있는 경우 두 개의 조각이 포함 됩니다.

앱을 사용 하 여이 샘플 프레임 워크를 배포 하려는 경우 시뮬레이터에서 제대로 실행 되지만 프레임 워크에 iOS 장치에 대 한 코드 별 조각이 포함 되어 있지 않으므로 장치에서 오류가 발생 합니다. 프레임 워크가 모든 인스턴스에서 작동 하도록 하려면 arm64, armv7 및 armv7s 용 thumb-2와 같은 장치 관련 조각만 포함 해야 합니다.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>포함 프레임 워크 사용

Xamarin.ios 또는 Xamarin.ios 앱에서 포함 된 프레임 워크를 사용 하려면 다음 두 단계를 완료 해야 합니다. Fat 프레임 워크를 만들고 프레임 워크를 포함 합니다.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Fat 프레임 워크 만들기

위에서 설명한 것 처럼 앱에서 포함 된 프레임 워크를 사용할 수 있으려면 앱이 실행 되는 장치에 대 한 모든 시스템 아키텍처 조각이 포함 된 Fat 프레임 워크 여야 합니다.

프레임 워크와 사용 중인 앱이 동일한 Xcode 프로젝트에 있는 경우 Xcode는 동일한 빌드 설정을 사용 하 여 프레임 워크와 앱을 모두 빌드하기 때문에 문제가 되지 않습니다. Xamarin 앱은 포함 된 프레임 워크를 만들 수 없기 때문에이 기술을 사용할 수 없습니다.

이 문제 `lipo` 를 해결 하기 위해 명령줄 도구를 사용 하 여 필요한 모든 조각을 포함 하는 하나의 Fat 프레임 워크로 둘 이상의 프레임 워크를 병합할 수 있습니다. `lipo` 명령 사용에 대 한 자세한 내용은 [네이티브 라이브러리 연결](~/ios/platform/native-interop.md) 설명서를 참조 하세요.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>프레임 워크 포함

다음 단계는 네이티브 참조를 사용 하 여 Xamarin.ios 또는 Xamarin.ios 프로젝트에 프레임 워크를 포함 하는 데 필요 합니다.

1. 새를 만들거나 기존 Xamarin.ios, Xamarin.ios 또는 바인딩 프로젝트를 엽니다.
2. **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 추가**네이티브 참조**추가 **를 선택** > 합니다. 

    [![](native-references-images/ref01.png "솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 네이티브 참조 추가를 선택 합니다.")](native-references-images/ref01.png#lightbox)
3. **열기** 대화 상자에서 포함 하려는 네이티브 프레임 워크의 이름을 선택 하 고 **열기** 단추를 클릭 합니다. 

    [![](native-references-images/ref02.png "포함할 네이티브 프레임 워크의 이름을 선택 하 고 열기 단추를 클릭 합니다.")](native-references-images/ref02.png#lightbox)
4. 프레임 워크는 프로젝트의 트리에 추가 됩니다. 

    [![](native-references-images/ref03.png "프레임 워크는 프로젝트 트리에 추가 됩니다.")](native-references-images/ref03.png#lightbox)

프로젝트가 컴파일될 때 네이티브 프레임 워크는 앱의 번들에 포함 됩니다.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>앱 확장 및 포함 프레임 워크

내부적으로 Xamarin.ios는이 기능을 활용 하 여 Mono 런타임을 프레임 워크로 연결 하 여 (배포 대상이 > = iOS 8.0 인 경우) 확장이 있는 앱에 대해 앱 크기를 크게 줄일 수 있습니다 (Mono 런타임이에 대해 한 번만 포함 됨). 컨테이너 앱에 대해 한 번이 아니라 각 확장에 대해 한 번이 아니라 전체 앱 번들입니다.

모든 확장에는 iOS 8.0가 필요 하기 때문에 확장은 Mono 런타임과 프레임 워크로 연결 됩니다.

IOS를 대상으로 하는 확장 및 앱이 없는 앱 

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 네이티브 프레임 워크를 Xamarin.ios 또는 Xamarin.ios 응용 프로그램에 포함 하는 방법을 자세히 살펴봅니다.
