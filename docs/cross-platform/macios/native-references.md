---
title: 네이티브 참조 iOS, Mac 및 바인딩 프로젝트
description: 네이티브 참조는 Xamarin.iOS, Xamarin.Mac, 또는 바인딩 프로젝트에는 네이티브 프레임 워크를 포함 하는 기능을 제공 합니다.
ms.prod: xamarin
ms.assetid: E53185FB-CEF5-4AB5-94F9-CC9B57C52300
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3a497d0bb4674014b8063cb1fbc91eec6e7ae5ea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61201567"
---
# <a name="native-references-in-ios-mac-and-bindings-projects"></a>네이티브 iOS, Mac 및 바인딩 프로젝트 참조

_네이티브 참조는 Xamarin.iOS 또는 Xamarin.Mac 프로젝트 또는 바인딩 프로젝트가 네이티브 프레임 워크를 포함 하는 기능을 제공 합니다._

IOS 8.0 이후 앱 확장 및 Xcode에서 기본 앱 간에 코드 공유 하는 포함 된 프레임 워크를 만들 수 있었습니다. 네이티브 참조 기능을 사용 하는 것이 포함 된 프레임 워크 (Xcode를 사용 하 여 만든) Xamarin.iOS에서 사용할 수 합니다.
 
> [!IMPORTANT]
> Xamarin.iOS 또는 Xamarin.Mac 프로젝트의 모든 형식에서 포함 된 프레임 워크를 만들 수 없습니다, 네이티브 참조만 허용 기존 네이티브 (Objective-c) 프레임 워크를 사용 합니다.

<a name="Terminology" />

## <a name="terminology"></a>용어

8 (이상), ios에서 **포함 된 프레임 워크** 정적으로 연결 하 고 동적으로 연결 된 프레임 워크를 포함 하는 둘 다를 수 있습니다. 제대로 배포를 확인 해야 합니다을 모두 포함 하는 "fat" 프레임 워크에 해당 _조각_ 앱을 사용 하 여 지원 하려는 각 장치 아키텍처에 대 한 합니다.

<a name="Static-vs-Dynamic-Frameworks" />

### <a name="static-vs-dynamic-frameworks"></a>정적 포트 대 동적 프레임 워크

**정적 프레임 워크** 컴파일 타임에 연결 된 위치 **동적 프레임 워크** 런타임 시 연결 되 고 따라서 다시 링크 하지 않고 수정할 수 있습니다. 사용한 iOS 8 이전의 모든 타사 프레임 워크를 사용한 경우는 **정적 Framework** 앱으로 컴파일된 합니다. Apple의를 참조 하세요 [동적 라이브러리 프로그래밍](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html#//apple_ref/doc/uid/TP40001873-SW1) 자세한 세부 정보에 대 한 설명서입니다.

<a name="Embedded-vs-System-Frameworks" />

### <a name="embedded-vs-system-frameworks"></a>포함 된 vs입니다. 시스템 프레임 워크

**프레임 워크를 포함** 에 앱 번들에 포함 되어 있고 해당 샌드박스를 통해 특정 앱에 액세스할 수만 있습니다. **시스템 프레임 워크** 운영 체제 수준에서 저장 되 고 장치의 모든 앱에 사용할 수 있습니다. 현재 Apple만에 운영 체제 수준 프레임 워크를 만들 수 있습니다.

<a name="Thin-vs-Fat-Frameworks" />

### <a name="thin-vs-fat-frameworks"></a>씬 vs입니다. Fat 프레임 워크

**프레임 워크를 씬** 의 컴파일된 코드는 특정 시스템 아키텍처를 포함 합니다. 여기서 **Fat 프레임 워크** 여러 아키텍처에 대 한 코드를 포함 합니다. 프레임 워크에 컴파일된 각 아키텍처 관련 코드 베이스 라고 하는 _조각_합니다. 따라서 예를 들어 두 개의 iOS 시뮬레이터 아키텍처 (i386 및 X86_64)에 대해 컴파일된 프레임 워크에 있으면이 포함 됩니다 두 조각.

앱을 사용 하 여이 샘플 프레임 워크를 배포 하려는 경우는 시뮬레이터에서 제대로 실행 있지만 프레임 워크에는 iOS 장치에 대 한 모든 관련 코드 조각 이므로 장치에서 실패 합니다. 사용할 수 있도록 프레임 워크는 모든 인스턴스에서 것도 해야 arm64, armv7 및 armv7s 같은 장치별 조각을 포함 합니다.

<a name="Working-with-Embedded-Frameworks" />

## <a name="working-with-embedded-frameworks"></a>포함 된 프레임 워크 사용

Xamarin.iOS 또는 Xamarin.Mac 앱에 포함 된 프레임 워크를 사용 하 여 작업을 완료 해야 하는 방법은 다음 두 단계가 있습니다. Fat 프레임 워크를 만들고 프레임 워크를 포함 합니다.

<a name="Overview" />

### <a name="creating-a-fat-framework"></a>Fat 프레임 워크 만들기

위에서 설명한 대로 앱에서 포함 된 프레임 워크를 사용 하려면 모든 앱에서 실행 되는 장치에 대 한 시스템 아키텍처 조각이 포함 된 Fat Framework 이어야 합니다.

프레임 워크 및 소비 앱 같은 Xcode 프로젝트의 경우이 문제가 되지 않습니다는 Xcode 프레임 워크와 동일한 빌드 설정을 사용 하 여 앱에 작성 됩니다. Xamarin 앱에서 포함 된 프레임 워크를 만들 수 없는 때문에이 기법을 사용할 수 없습니다.

이 문제를 해결 하는 `lipo` 필요한 조각 모두를 포함 하는 단일 Fat 프레임 워크에 둘 이상의 프레임 워크를 병합 하려면 명령줄 도구를 사용할 수 있습니다. 작업에 대 한 자세한 내용은 합니다 `lipo` 명령, 참조 하세요 우리의 [네이티브 라이브러리 연결](~/ios/platform/native-interop.md) 설명서.

<a name="Embedding-a-Framework" />

### <a name="embedding-a-framework"></a>프레임 워크를 포함합니다.

네이티브 참조를 사용 하 여 Xamarin.iOS 또는 Xamarin.Mac 프로젝트에서 프레임 워크를 포함 하려면 다음 단계는 필요 합니다.

1. 새 또는 기존 Xamarin.iOS, Xamarin.Mac 또는 바인딩 프로젝트를 엽니다.
2. 에 **솔루션 탐색기**, 프로젝트 이름을 마우스 오른쪽 단추로 **추가** > **네이티브 참조 추가**: 

    [![](native-references-images/ref01.png "솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 네이티브 참조 추가 선택")](native-references-images/ref01.png#lightbox)
3. **엽니다** 대화 상자를 포함 하 고 클릭 하려는 네이티브 프레임 워크의 이름을 선택 합니다 **열기** 단추: 

    [![](native-references-images/ref02.png "포함 열기 단추를 클릭 하 여 네이티브 프레임 워크의 이름을 선택합니다")](native-references-images/ref02.png#lightbox)
4. 프레임 워크는 프로젝트의 트리에 추가 됩니다. 

    [![](native-references-images/ref03.png "프로젝트 트리에 추가할 프레임 워크")](native-references-images/ref03.png#lightbox)

프로젝트를 컴파일할 때 네이티브 프레임 워크는 앱의 번들에 포함 됩니다.

<a name="App-Extensions-and-Embedded-Frameworks" />

## <a name="app-extensions-and-embedded-frameworks"></a>앱 확장 및 포함 된 프레임 워크

Xamarin.iOS 프레임 워크로 Mono 런타임과 함께 연결 하려면이 기능을 걸릴 수 있습니다는 내부적으로 (배포 대상의 경우 > = iOS 8.0), (에 대 한 Mono 런타임에 한 번만 포함 될 것 이므로 앱 크기 확장을 사용 하 여 앱에 대 한 크게 감소 전체 앱 번들에는 컨테이너 앱에 대해 한 번씩 각 확장에 대 한 한 번이 아니라).

확장 모든 확장에는 iOS 8.0 필요 하기 때문에 프레임 워크는 Mono 런타임과 함께 연결 합니다.

없는 확장과 앱 해당 대상 iOS 앱 

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.iOS 또는 Xamarin.Mac 응용 프로그램에 네이티브 프레임 워크를 포함 하는 자세히 살펴보려면을 걸렸습니다.

