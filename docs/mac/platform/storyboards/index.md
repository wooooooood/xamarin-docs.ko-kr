---
title: Xamarin.ios의 스토리 보드 소개
description: 이 문서에서는 Xamarin.ios 앱에서 Storyboard를 사용 하는 방법을 소개 합니다. 스토리보드와 Xcode의 Interface Builder를 사용하여 앱의 UI를 만들고 유지 관리하는 내용을 다룹니다.
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: b27a8d65ebaca6009d8310931b9dac3a4d7e12f3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026143"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Xamarin.ios의 스토리 보드 소개

_이 문서에서는 Xamarin.ios 앱에서 Storyboard를 사용 하는 방법을 소개 합니다. 스토리 보드 및 Xcode의 Interface Builder를 사용 하 여 앱의 UI를 만들고 유지 관리 하는 방법을 다룹니다._

스토리 보드를 사용 하면 창 정의 및 컨트롤을 포함할 뿐만 아니라 다른 창 (segue를 통해) 및 뷰 상태 간의 링크도 포함 하는 Xamarin.ios 앱에 대 한 사용자 인터페이스를 개발할 수 있습니다.

[![](images/intro01.png "A sample UI in Xcode")](images/intro01.png#lightbox)

이 문서에서는 Storyboard를 사용 하 여 Xamarin.ios 앱의 사용자 인터페이스를 정의 하는 방법을 소개 합니다.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>스토리 보드 란?

Storyboard를 사용 하 여 모든 Xamarin.ios 앱의 UI는 개별 요소와 사용자 인터페이스 간의 모든 탐색을 사용 하 여 단일 위치에 정의할 수 있습니다. Xamarin.ios 용 storyboard는 Xamarin.ios 용 Storyboard와 매우 유사한 방식으로 작동 합니다. 그러나 다른 인터페이스 관용구 때문에 다른 _Segue 형식_ 집합을 포함 합니다.

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>장면 작업

위에서 설명한 것 처럼 Storyboard는 지정 된 앱에 대 한 모든 UI를 해당 _뷰 컨트롤러_의 기능 개요로 분할 하 여 정의 합니다. Xcode의 Interface Builder에서 이러한 각 컨트롤러는 자체 _장면_에 상주 합니다.

[![](images/intro02.png "An example view controller")](images/intro02.png#lightbox)

각 장면은 UI의 각 장면을 연결 하 여 해당 관계를 표시 하는 일련의 선 (Segue 이라고 함)과 함께 지정 된 뷰 및 뷰 컨트롤러 쌍을 나타냅니다. 일부 Segue는 하나 이상의 자식 뷰나 뷰 컨트롤러를 포함 하는 뷰 컨트롤러를 정의 합니다. 다른 Segue 뷰 컨트롤러 (예: 팝 오버 또는 대화 상자 표시) 간의 전환을 정의 합니다. 

[![](images/intro03.png "A sample segue")](images/intro03.png#lightbox)

가장 중요 한 점은 각 Segue는 응용 프로그램 UI의 지정 된 요소 간에 일부 형식의 데이터 흐름을 나타내는 것입니다.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>뷰 컨트롤러 작업

뷰 컨트롤러는 Mac 앱 내에서 지정 된 정보 보기와 해당 정보를 제공 하는 데이터 모델 간의 관계를 정의 합니다. 스토리 보드의 각 최상위 장면은 Xamarin.ios 앱의 코드에서 하나의 뷰 컨트롤러를 나타냅니다.

[![](images/intro04.png "An example slips view controller")](images/intro04.png#lightbox)

이러한 방식으로 각 뷰 컨트롤러는 정보의 시각적 표시 (보기)와 해당 정보를 표시 하 고 제어 하는 논리에 대 한 자체 포함 된 재사용 가능한 쌍입니다.

지정 된 장면 내에서 일반적으로 개별 `.xib` 파일에 의해 처리 되는 모든 작업을 수행할 수 있습니다. 

- 하위 뷰 및 컨트롤 (예: 단추 및 텍스트 상자)을 입력 합니다.
- 요소 위치 및 자동 레이아웃 제약 조건을 정의 합니다.
- UI 요소를 코드에 노출 하는 작업 및 콘센트를 연결 합니다.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>Segue 사용

위에서 설명한 것 처럼 Segue는 앱의 UI를 정의 하는 모든 장면 간의 관계를 제공 합니다. IOS의 스토리 보드에서 작업 하는 데 익숙한 경우 Segue for iOS는 일반적으로 전체 화면 보기 간의 전환을 정의 한다는 것을 알고 있습니다. 이는 Segue가 일반적으로 "포함"을 정의 하는 경우 macOS와 다릅니다. 여기서 한 장면은 부모 장면의 자식입니다.

MacOS에서 대부분의 앱은 분할 뷰 및 탭과 같은 UI 요소를 사용 하 여 동일한 창 내에서 보기를 함께 그룹화 하는 경향이 있습니다. IOS와 달리 보기는 물리적 표시 공간이 제한 되어 있으므로 화면을 전환 하거나 화면에서 해제 해야 합니다.

포함에 대 한 macOS 성향을 지정 된 경우 모달 창, 시트 보기 및 Popovers와 같은 _프레젠테이션 segue_ 이 사용 되는 경우가 있습니다.

프레젠테이션 Segue를 사용 하는 경우 표시 되는 부모 뷰 컨트롤러의 `PrepareForSegue` 메서드를 재정의 하 고, 변수를 초기화 하 고 표시 되는 뷰 컨트롤러에 데이터를 제공할 수 있습니다.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>디자인 및 실행 시간

디자인 타임 (Xcode의 Interface Builder UI를 레이아웃 하는 경우)에서 앱 UI의 각 요소는 해당 구성 항목으로 분류 됩니다.

- **장면** -다음으로 구성 됩니다.
  - **뷰 컨트롤러** -뷰 및 뷰를 지 원하는 데이터 간의 관계를 정의 합니다.
  - **Views 및 하위 뷰** -사용자 인터페이스를 구성 하는 실제 요소입니다.
  - **포함 segue** -장면 간 부모-자식 관계를 정의 합니다.
- **프레젠테이션 segue** -개별 프레젠테이션 모드를 정의 합니다. 

이러한 방식으로 각 요소를 정의 하 여 런타임 중에 필요할 때만 각 요소를 지연 로드할 수 있습니다. MacOS에서 전체 프로세스는 개발자가 작업을 수행 하기 위해 최소한의 지원 코드를 필요로 하는 복잡 하 고 유연한 사용자 인터페이스를 만들 수 있도록 설계 되었습니다 .이는 모두 시스템 리소스를 최대한 효율적으로 사용 합니다.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>스토리 보드 빠른 시작

[스토리 보드 빠른 시작](~/mac/platform/storyboards/quickstart.md) 가이드에서는 사용자 인터페이스를 만들기 위해 storyboard 작업의 주요 개념을 소개 하는 간단한 xamarin.ios 앱을 만듭니다. 샘플 앱은 _콘텐츠 영역_ 및 _검사기 영역이_ 포함 된 Spilt 뷰로 구성 되며 간단한 기본 설정 대화 상자 창을 제공 합니다. Segue를 사용 하 여 모든 사용자 인터페이스 요소를 함께 연결 합니다.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>Storyboards 작업

이 섹션에서는 Xamarin.ios 앱에서 [스토리 보드로 작업](~/mac/platform/storyboards/indepth.md) 하는 방법에 대 한 자세한 내용을 다룹니다. 이에 대 한 자세한 설명 및 보기 컨트롤러와 뷰로 구성 된 방법에 대해 자세히 살펴보겠습니다. 그런 다음 Segue와의 상호 연결 방식을 살펴보겠습니다. 마지막으로, 사용자 지정 Segue 형식으로 작업 하는 방법을 살펴보겠습니다. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>복잡 한 Storyboard 예

Xamarin.ios 앱에서 Storyboard를 사용 하는 복잡 한 예제에 대 한 예제는 [Sourcewriter 샘플 앱](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter)을 참조 하세요. SourceWriter는 코드 완성 및 간단한 구문 강조 기능을 제공하는 간단한 소스 코드 편집기입니다.

SourceWriter 코드는 완벽하게 주석 처리되어 있으며, 가능한 경우 Xamarin.Mac 지침 설명서에 핵심 기술 또는 메서드부터 관련 정보까지 다양한 링크가 제공됩니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 앱에서 Storyboard를 사용 하는 방법을 간략히 살펴보겠습니다. 스토리 보드를 사용 하 여 새 앱을 만드는 방법 및 사용자 인터페이스를 정의 하는 방법을 살펴보았습니다. 또한 segue를 사용 하 여 서로 다른 창 및 뷰 상태 사이를 탐색 하는 방법도 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [Hello, Mac(샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/hello-mac)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
