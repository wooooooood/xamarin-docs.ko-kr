---
title: Xamarin.Mac의 스토리 보드 소개
description: 이 문서에서는 Xamarin.Mac 앱에서 스토리 보드를 작업을 소개 합니다. 스토리보드와 Xcode의 Interface Builder를 사용하여 앱의 UI를 만들고 유지 관리하는 내용을 다룹니다.
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 8a5a2f87c16a5dd040cefb2fbc615b01431ebcf5
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832295"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Xamarin.Mac의 스토리 보드 소개

_이 문서에서는 Xamarin.Mac 앱에서 스토리 보드를 작업을 소개 합니다. 만들기 및 스토리 보드와 Xcode의 Interface Builder를 사용 하 여 앱의 UI를 유지 관리 하는 내용을 다룹니다._

스토리 보드를 사용 하면 뿐만 아니라 창 정 및 컨트롤을 포함 하지만 다른 windows 간의 링크를 포함 하는 Xamarin.Mac 앱에 대 한 사용자 인터페이스를 개발 (통해 segue) 상태를 확인 합니다.

[![](images/intro01.png "Xcode에서 UI 샘플")](images/intro01.png#lightbox)

이 문서에서는 Xamarin.Mac 앱의 사용자 인터페이스를 정의 하려면 스토리 보드를 사용 하 여 소개를 제공 합니다.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>Storyboard 란?

스토리 보드를 사용 하 여 Xamarin.Mac 앱의 UI의 모든 모든 해당 개별 요소와 사용자 인터페이스 간의 탐색을 사용 하 여 단일 위치에서 정의할 수 있습니다. Xamarin.Mac에 대 한 스토리 보드는 Xamarin.iOS에 대 한 스토리 보드는 매우 비슷한 방식으로 작동 합니다. 하지만 다른 집합을 포함 _Segue 형식_ 다른 인터페이스 관용구 때문입니다.

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>백그라운드에서 작업

스토리 보드의 기능 개요를 분해할 지정된 된 앱에 대 한 UI의 모든 정의 하는 위에서 설명한 대로 해당 _뷰 컨트롤러_합니다. Xcode의 Interface Builder에서 각 이러한 컨트롤러에 거주 하 고 자체 _장면_합니다.

[![](images/intro02.png "예제 보기 컨트롤러를")](images/intro02.png#lightbox)

각 장면은 일련의 각 장면은 있으므로 관계를 보여 주는 UI에 연결 하는 선 (고 Segues 라고 함)를 사용 하 여 지정 된 보기 및 보기 컨트롤러 쌍을 나타냅니다. 몇 가지 고 Segues 정의 방법을 뷰 컨트롤러 하나 이상의 자식 뷰 또는 뷰 컨트롤러를 포함 합니다. 다른 고 Segues 뷰 컨트롤러 (예: 팝 오버 또는 대화 상자를 표시) 간의 전환을 정의 합니다. 

[![](images/intro03.png "샘플 segue")](images/intro03.png#lightbox)

가장 중요 한 점은 각 Segue 흐름 앱 UI의 특정된 요소 간에 데이터의 일부 폼을 나타내는입니다.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>뷰 컨트롤러를 사용 하 여 작업

뷰 컨트롤러는 지정 된 Mac 앱 내에서 정보 보기 및 해당 정보를 제공 하는 데이터 모델 간의 관계를 정의 합니다. 스토리 보드의 각 최상위 장면 Xamarin.Mac 앱의 코드에서 한 뷰 컨트롤러를 나타냅니다.

[![](images/intro04.png "예로 낮아지는 뷰 컨트롤러")](images/intro04.png#lightbox)

이 이렇게 하면 각 뷰 컨트롤러 정보의 시각적 표현 (뷰)와 제공 하 고 해당 정보를 제어 논리를 자체 포함 된, 재사용 가능한 쌍입니다.

지정 된 한 장면에 일반적으로 처리 하 던 개별적으로는 모두 수행할 수 있습니다 `.xib` 파일: 

- 장소와 subviews (예: 단추 및 텍스트 상자)을 제어 합니다.
- 요소 위치 및 자동 레이아웃 제약 조건을 정의 합니다.
- 연결 작업 및 출 선을 코드에 UI 요소를 노출 합니다.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>작업 Segue

위에서 설명한 대로 고 Segues 모든 앱의 UI를 정의 하는 백그라운드에서 간의 관계를 제공 합니다. IOS 스토리 보드에서 작업에 익숙한 경우 알고 Segue iOS에는 일반적으로 전체 화면 뷰 간의 전환 정의 대 한 합니다. 이 위치 고 Segues 일반적으로 "포함" (한 장면은 장면 부모의 자식)을 정의 하는 경우 macOS에서 다릅니다.

MacOS의 경우 대부분의 앱 분할 뷰와 탭과 같은 UI 요소를 사용 하 여 동일한 창 내에서 해당 뷰를 그룹화 하는 경우가 많습니다. 여기서 뷰에 화면 밖으로 전환 해야, iOS와 달리 제한 된 물리적으로 인해 공간을 표시 합니다.

포함으로 macOS의 성향을 들어 경우가 위치 _프레젠테이션 Segue_ 모달 Windows, 시트 뷰 Popovers 등 사용 됩니다.

프레젠테이션 Segue를 사용 하는 경우 재정의할 수 있습니다는 `PrepareForSegue` 초기화 하는 프레젠테이션 및 변수에 대 한 부모 뷰 컨트롤러의 메서드에 제공 되는 보기 컨트롤러에 모든 데이터를 제공 합니다.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>디자인 및 실행된 시간

디자인 타임 (때 한 Xcode의 Interface Builder에서 UI 레이아웃), 앱의 UI의 각 요소에 구성 항목으로 세분화 됩니다.

- **장면** -어떤 구성 됩니다.
    - **컨트롤러 보기** -뷰 및 지 원하는 데이터 간의 관계를 정의 하는 합니다.
    - **뷰와 하위 뷰** -사용자 인터페이스를 구성 하는 실제 요소입니다.
    - **포함 고 Segues** -장면 간의 부모-자식 관계를 정의 하는 합니다.
- **프레젠테이션 고 Segues** -개별 프레젠테이션 모드를 정의 하는 합니다. 

이러한 방식으로 각 요소를 정의 하 여 허용 각 요소의 지연 로딩 런타임에 필요할 때에 합니다. MacOS 개발자 실정입니다 작동 하도록 코드를 백업 해야 하는 복잡 하 고 유연한 사용자 인터페이스를 만들 수 있도록 전체 프로세스가 설계 되 가능한 시스템 리소스를 사용 하 여 효율적으로 하는 동안 모든 합니다.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>스토리 보드 빠른 시작

에 [스토리 보드 빠른 시작](~/mac/platform/storyboards/quickstart.md) 가이드를 만들어 사용자 인터페이스 만들기에 스토리 보드를 사용 하는 주요 개념을 소개 하는 간단한 Xamarin.Mac 앱. 샘플 앱을 포함 하는 나누기 보기 구성 되는 _콘텐츠 영역_ 및 _검사기 영역_ 간단한 기본 설정 대화 상자 창이 제공 하 고 합니다. 모든 사용자 인터페이스 요소를 연결 하 고 Segues 사용 하겠습니다 했습니다.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>Storyboards 작업

이 섹션에서는 자세한 세부 정보를 다룹니다 [스토리 보드를 작업](~/mac/platform/storyboards/indepth.md) Xamarin.Mac 앱에서. 장면 및 뷰 컨트롤러 및 보기 구성 됩니다 하는 방법을 자세히 배우려면을 수행 합니다. 그런 다음 고 Segues 함께 자동으로 연결 되는 방법을 살펴보겠습니다를 알아보겠습니다. 마지막으로 Segue 형식 사용자 지정 작업 살펴보겠습니다를 알아보겠습니다. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>복잡 한 스토리 보드 예제

Xamarin.Mac 앱에서 스토리 보드를 사용 하 여 작업의 복잡 한 예의 예제를 참조 하십시오 합니다 [SourceWriter 샘플 앱](https://developer.xamarin.com/samples/mac/SourceWriter/)합니다. SourceWriter는 코드 완성 및 간단한 구문 강조 기능을 제공하는 간단한 소스 코드 편집기입니다.

SourceWriter 코드는 완벽하게 주석 처리되어 있으며, 가능한 경우 Xamarin.Mac 지침 설명서에 핵심 기술 또는 메서드부터 관련 정보까지 다양한 링크가 제공됩니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 앱에서 스토리 보드를 작업에 빠르게 확인을 수행 했습니다. 스토리 보드를 사용 하 여 새 앱을 만드는 방법 및 사용자 인터페이스를 정의 하는 방법에 살펴보았습니다. 또한 다른 창 간에 탐색 하는 방법에 살펴보았습니다 및 segue를 사용 하 여 보기 상태입니다.


## <a name="related-links"></a>관련 링크

- [Hello, Mac(샘플)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
