---
title: Xamarin.Mac의 스토리 보드 소개
description: 이 문서에서는 Xamarin.Mac 앱에서 스토리 보드와 작업에 대해 소개 합니다. 스토리보드와 Xcode의 Interface Builder를 사용하여 앱의 UI를 만들고 유지 관리하는 내용을 다룹니다.
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 027998d6aff8aba4e5621b1cde51a24e18821ff9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792667"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Xamarin.Mac의 스토리 보드 소개

_이 문서에서는 Xamarin.Mac 앱에서 스토리 보드와 작업에 대해 소개 합니다. 만들기 및 스토리 보드와 Xcode의 인터페이스 작성기를 사용 하 여 응용 프로그램의 UI를 유지 관리 하는 내용을 다룹니다._

스토리 보드를 사용 하면 뿐만 아니라 창 정 및 컨트롤을 포함 하 고 또한 다른 창 간의 링크가 포함 되어 Xamarin.Mac 앱에 대 한 사용자 인터페이스를 개발할 수 (통해 segues) 상태를 확인 합니다.

[![](images/intro01.png "Xcode에서 UI 샘플")](images/intro01.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 정의 하려면 스토리 보드를 사용 하 여에 대 한 소개를 제공 합니다.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>스토리 보드는 무엇입니까?

스토리 보드를 사용 하 여 모든 Xamarin.Mac 응용 프로그램의 UI는 개별 요소와 사용자 인터페이스 간의 탐색의 모든 단일 위치에서 정의할 수 있습니다. Xamarin.Mac에 대 한 스토리 보드 Xamarin.iOS에 대 한 스토리 보드에 매우 유사한 방식으로 작동 합니다. 하지만 다른 집합을 포함 _Segue 형식_ 다른 인터페이스 관용구 때문입니다.

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>장면을 사용한 작업

스토리 보드 ui의 기능 개요로 분리 하는 지정된 된 앱에 대 한 모든 정의 하는 위에서 설명한 대로 해당 _컨트롤러 보기_합니다. Xcode의 인터페이스 작성기에서 각 이러한 컨트롤러에 거주 하 고 자체 _장면_합니다.

[![](images/intro02.png "예에서는 뷰 컨트롤러")](images/intro02.png#lightbox)

각 장면 각 장면 따라서 해당 관계를 보여 주는 UI에 연결 하는 줄 (Segues 라고 함)의 집합과 지정 된 보기 및 보기 컨트롤러 쌍을 나타냅니다. 일부 Segues 정의 어떻게 뷰-컨트롤러 하나 이상의 자식 뷰 또는 컨트롤러 보기를 포함 합니다. 다른 Segues (예: popover 또는 대화 상자를 표시 하는) 뷰-컨트롤러 간의 변환을 정의 합니다. 

[![](images/intro03.png "샘플 segue")](images/intro03.png#lightbox)

가장 중요 한 참고 사항은 각 Segue 일종의 응용 프로그램의 UI의 특정된 요소 간의 데이터 흐름을 나타냅니다.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>컨트롤러 보기 작업

컨트롤러 보기 Mac 응용 프로그램 내에서 정보가 지정된 된 보기와 해당 정보를 제공 하는 데이터 모델 간의 관계를 정의 합니다. 스토리 보드의 각 최상위 장면이 Xamarin.Mac 앱 코드에서 하나의 보기 컨트롤러를 나타냅니다.

[![](images/intro04.png "예로 뷰-컨트롤러 지연")](images/intro04.png#lightbox)

이러한 방식으로 각 뷰-컨트롤러 정보의 시각적 표시 (뷰) 및 표시 하 고 해당 정보를 제어 하는 논리가의 자체 포함 하 고 재사용 가능한 쌍입니다.

지정 된 한 장면에 일반적으로 처리 하 던 개별적으로 하는 것의 모두 수행할 수 있습니다 `.xib` 파일: 

 - 전체 subviews 및 (예: 단추 및 텍스트 상자)을 제어 합니다.
 - 요소 위치 및 auto 레이아웃 제약 조건을 정의 합니다.
 - 연결 작업 및 콘센트 UI 요소 코드를 노출입니다.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>작업을 Segues

위에서 설명 했 듯이 Segues 간의 모든 응용 프로그램의 UI를 정의 하는 백그라운드에서 관계를 제공 합니다. IOS에서 스토리 보드에서 작업에 익숙한 것을 알고 있다면 Segues iOS에는 일반적으로 전체 화면 보기 간 전환을 정의 대 한 합니다. 이 점에서 차이가 macOS 등 Segues 일반적으로 "포함" (한 장면은 장면 부모의 자식은)를 정의 하는 경우.

MacOS 등의 대부분의 응용 프로그램의 뷰 같은 분할 뷰와 탭 UI 요소를 사용 하 여 동일한 창 내에 함께 그룹화 하는 경향이 있습니다. IOS, 뷰 및 화면 해제로 전환할 수 필요한 달리 제한 된 물리적으로 인해 공간을 표시 합니다.

경우 제약으로 macOS의 경향 들어 있는 _프레젠테이션 Segues_ 모달 창, 시트 뷰 Popovers 등 사용 됩니다.

프레젠테이션 Segues를 사용 하는 경우 재정의할 수 있습니다는 `PrepareForSegue` 초기화 프레젠테이션 및 변수에 대 한 부모 뷰-컨트롤러의 메서드 및 제공 되는 보기 컨트롤러에 모든 데이터를 제공 합니다.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>디자인 및 실행된 시간

디자인 타임에 (때 레이아웃 UI Xcode의 인터페이스 작성기에 out), 응용 프로그램의 UI의 각 요소를 구성 항목으로 분리 됩니다.

- **장면** -어떤 구성 됩니다.
    - **컨트롤러 보기** -뷰와 지 원하는 데이터 간의 관계를 정의 하는 합니다.
    - **보기와 하위** -사용자 인터페이스를 구성 하는 실제 요소입니다.
    - **Containment Segues** -장면 간의 부모-자식 관계를 정의 하는 합니다.
- **프레젠테이션 Segues** -개별 프레젠테이션 모드를 정의 하는 합니다. 

이러한 방식으로 각 요소를 정의 하 여 있습니다 각 요소의 지연 로드에 대 한 런타임 동안 필요할 때에. 전체 프로세스는 최소한의 기능만 작동 하도록 코드를 백업 해야 하는 복잡 하 고 유연한 사용자 인터페이스를 만드는 개발자를 허용 하도록 설계 된 macOS 등의 가능한 시스템 리소스와 효율적 되는 한편 합니다.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>스토리 보드 빠른 시작

에 [스토리 보드 빠른 시작](~/mac/platform/storyboards/quickstart.md) 가이드, 스토리 보드를 사용자 인터페이스를 만드는 작업의 주요 개념을 소개 하는 간단한 Xamarin.Mac 앱을 만들 것입니다. 샘플 응용 프로그램은 포함 하는 나누기 뷰 구성 되며는 _콘텐츠 영역_ 및 _검사기 영역_ 간단한 기본 설정 대화 상자 창이 제시 합니다. Segues 결합 하는 모든 사용자 인터페이스 요소를 사용해 보겠습니다.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>스토리 보드와 작업

이 섹션에서는의 자세한 정보를 다룹니다 [스토리 보드와 작업](~/mac/platform/storyboards/indepth.md) Xamarin.Mac 응용 프로그램에서입니다. 장면 및 컨트롤러 보기와 보기의 구성 되어 있는 방법에 대해 심층적을 수행 합니다. 그런 다음 장면 Segues 함께 연결 되는 방법을 살펴보겠습니다를 이동 합니다. 마지막으로, 사용자 지정 Segue 형식 작업 살펴보겠습니다를 이동 합니다. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>복잡 한 스토리 보드 예제

예를 보려면 Xamarin.Mac 응용 프로그램에서 스토리 보드와 작업의 복잡 한 예제를 참조 하십시오는 [SourceWriter 샘플 응용 프로그램](https://developer.xamarin.com/samples/mac/SourceWriter/)합니다. SourceWriter는 코드 완성 및 간단한 구문 강조 기능을 제공하는 간단한 소스 코드 편집기입니다.

SourceWriter 코드는 완벽하게 주석 처리되어 있으며, 가능한 경우 Xamarin.Mac 지침 설명서에 핵심 기술 또는 메서드부터 관련 정보까지 다양한 링크가 제공됩니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서 개요 Xamarin.Mac 앱에서 스토리 보드로 작업을 수행 했습니다. 스토리 보드를 사용 하 여 새 앱을 만드는 방법 및 사용자 인터페이스를 정의 하는 방법에 살펴보았습니다. 또한 다른 창 간을 탐색 하는 방법에 살펴보았습니다 및 사용 하 여 보기 상태 segues 합니다.


## <a name="related-links"></a>관련 링크

- [Hello, Mac(샘플)](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [창 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
