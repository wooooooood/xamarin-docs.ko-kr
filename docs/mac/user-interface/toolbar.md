---
title: Xamarin.ios의 도구 모음
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 도구 모음을 사용 하는 방법을 설명 합니다. Xcode 및 Interface Builder에서 도구 모음을 만들고 유지 관리 하 고, 코드에 노출 하 고, 프로그래밍 방식으로 작업 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: cd2490bfad880d128f5eaeebd4aac58ad3a4d8fa
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772719"
---
# <a name="toolbars-in-xamarinmac"></a>Xamarin.ios의 도구 모음

_이 문서에서는 Xamarin.ios 응용 프로그램에서 도구 모음을 사용 하는 방법을 설명 합니다. Xcode 및 Interface Builder에서 도구 모음을 만들고 유지 관리 하 고, 코드에 노출 하 고, 프로그래밍 방식으로 작업 하는 방법에 대해 설명 합니다._

Mac용 Visual Studio 작업 하는 xamarin.ios 개발자는 도구 모음 컨트롤을 비롯 하 여 Xcode으로 작업 하는 macOS 개발자가 사용할 수 있는 것과 동일한 UI 컨트롤에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 Interface Builder를 사용 하 여 도구 모음 항목을 만들고 유지 관리할 수 있습니다. 이러한 도구 모음 항목은에서 C#만들 수도 있습니다.

MacOS의 도구 모음은 창의 위쪽 섹션에 추가 되 고 해당 기능과 관련 된 명령에 쉽게 액세스할 수 있도록 합니다. 응용 프로그램 사용자가 도구 모음을 숨기 거 나 표시 하거나 사용자 지정할 수 있으며, 도구 모음 항목을 다양 한 방식으로 제공할 수 있습니다.

이 문서에서는 Xamarin.ios 응용 프로그램에서 도구 모음 및 도구 모음 항목을 사용 하는 기본 사항을 설명 합니다. 

계속 하기 전에이 가이드 전체에서 사용 되는 주요 개념 및 기술에 대해 설명 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 문서 (특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션 소개)를 참조 하세요.

또한 [Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드 C# 를 목표로 노출-C](~/mac/internals/how-it-works.md) 섹션을 살펴보세요. 클래스를 목표 `Register` - `Export` C 클래스에 연결 C# 하는 데 사용 되는 및 특성을 설명 합니다.

## <a name="introduction-to-toolbars"></a>도구 모음 소개

MacOS 응용 프로그램의 모든 창에는 도구 모음이 포함 될 수 있습니다.

![도구 모음이 있는 예제 창](toolbar-images/info01.png "도구 모음이 있는 예제 창")

도구 모음은 응용 프로그램 사용자가 중요 하거나 일반적으로 사용 되는 기능에 빠르게 액세스할 수 있는 쉬운 방법을 제공 합니다. 예를 들어 문서 편집 응용 프로그램은 텍스트 색을 설정 하거나 글꼴을 변경 하거나 현재 문서를 인쇄 하기 위한 도구 모음 항목을 제공할 수 있습니다.

도구 모음은 다음과 같은 세 가지 방법으로 항목을 표시할 수 있습니다.

1. **아이콘 및 텍스트** 

     ![아이콘 및 텍스트를 포함 하는 도구 모음](toolbar-images/info02.png "아이콘 및 텍스트를 포함 하는 도구 모음")

2. **아이콘만** 

     ![아이콘 전용 도구 모음](toolbar-images/info03.png "아이콘 전용 도구 모음")

3. **텍스트만** 

     ![텍스트 전용 도구 모음](toolbar-images/info04.png "텍스트 전용 도구 모음")

도구 모음을 마우스 오른쪽 단추로 클릭 하 고 상황에 맞는 메뉴에서 디스플레이 모드를 선택 하 여 이러한 모드 간을 전환 합니다.

![도구 모음에 대 한 상황에 맞는 메뉴](toolbar-images/info05.png "도구 모음에 대 한 상황에 맞는 메뉴")

동일한 메뉴를 사용 하 여 도구 모음을 더 작은 크기로 표시 합니다.

![작은 아이콘을 포함 하는 도구 모음](toolbar-images/info06.png "작은 아이콘을 포함 하는 도구 모음")

메뉴를 사용 하 여 도구 모음을 사용자 지정할 수도 있습니다.

![도구 모음을 사용자 지정 하는 데 사용 되는 대화 상자](toolbar-images/info07.png "도구 모음을 사용자 지정 하는 데 사용 되는 대화 상자")

Xcode의 Interface Builder에서 도구 모음을 설정 하면 개발자가 기본 구성의 일부가 아닌 추가 도구 모음 항목을 제공할 수 있습니다. 그러면 응용 프로그램 사용자가 도구 모음을 사용자 지정 하 고 필요에 따라 미리 정의 된 항목을 추가 및 제거할 수 있습니다. 물론 도구 모음을 기본 구성으로 다시 설정할 수 있습니다.

도구 모음은 **보기** 메뉴에 자동으로 연결 되어 사용자가 해당 메뉴를 숨기고 표시 하 고 사용자 지정할 수 있습니다.

![보기 메뉴의 도구 모음 관련 항목](toolbar-images/info08.png "보기 메뉴의 도구 모음 관련 항목")

자세한 내용은 [기본 제공 메뉴 기능](~/mac/user-interface/menu.md) 설명서를 참조 하세요.

또한 Interface Builder에서 도구 모음이 올바르게 구성 된 경우 응용 프로그램은 응용 프로그램의 여러 시작에서 도구 모음 사용자 지정을 자동으로 유지 합니다.

이 가이드의 다음 섹션에서는 Xcode의 Interface Builder 사용 하 여 도구 모음을 만들고 유지 관리 하는 방법 및 코드에서 작업 하는 방법을 설명 합니다.

## <a name="setting-a-custom-main-window-controller"></a>사용자 지정 주 창 컨트롤러 설정

콘센트 및 작업을 통해 C# 코드에 UI 요소를 노출 하려면 xamarin.ios 앱에서 사용자 지정 창 컨트롤러를 사용 해야 합니다.

1. Xcode의 Interface Builder에서 앱의 스토리 보드를 엽니다.
2. 디자인 화면에서 창 컨트롤러를 선택 합니다.
3. **Id 검사자** 로 전환 하 고 **클래스 이름**으로 "windowcontroller"를 입력 합니다. 

    [![창 컨트롤러에 대 한 사용자 지정 클래스 이름 설정](toolbar-images/windowcontroller01.png "창 컨트롤러에 대 한 사용자 지정 클래스 이름 설정")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. 변경 내용을 저장 하 고 동기화 할 Mac용 Visual Studio로 돌아갑니다.
5. **WindowController.cs** 파일은 Mac용 Visual Studio의 **Solution Pad** 에서 프로젝트에 추가 됩니다. 

    ![Solution Pad에서 WindowController.cs 선택](toolbar-images/windowcontroller02.png "Solution Pad에서 WindowController.cs 선택")

6. Xcode의 Interface Builder에서 storyboard를 다시 엽니다.
7. **Windowcontroller .h** 파일을 사용할 수 있습니다. 

    [![WindowController .h 파일](toolbar-images/windowcontroller03.png "WindowController .h 파일")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Xcode에서 도구 모음 만들기 및 유지 관리

Xcode의 Interface Builder를 사용 하 여 도구 모음이 생성 및 유지 관리 됩니다. 응용 프로그램에 도구 모음을 추가 하려면 **Solution Pad**에서 두 번 클릭 하 여 앱의 기본 storyboard (이 경우 **주. storyboard**)를 편집 합니다.

![Solution Pad에서 주. 스토리 보드 열기](toolbar-images/edit01.png "Solution Pad에서 주. 스토리 보드 열기")

**라이브러리 검사기**에서 **검색 상자** 에 "도구"를 입력 하 여 사용 가능한 모든 도구 모음 항목을 쉽게 볼 수 있도록 합니다.

![도구 모음 항목을 표시 하도록 필터링 된 라이브러리 검사기](toolbar-images/edit02.png "도구 모음 항목을 표시 하도록 필터링 된 라이브러리 검사기")

도구 모음을 **인터페이스 편집기**의 창으로 끌어 놓습니다. 도구 모음이 선택 된 상태에서 **특성 검사자**에서 속성을 설정 하 여 동작을 구성 합니다.

![도구 모음에 대 한 특성 검사자](toolbar-images/edit04.png "도구 모음에 대 한 특성 검사자")

사용할 수 있는 속성은 다음과 같습니다.

1. **Display** -도구 모음에서 아이콘, 텍스트 또는 둘 다를 표시할지 여부를 제어 합니다.
2. **시작 시 표시** -선택 하는 경우 도구 모음이 기본적으로 표시 됩니다.
3. 사용자 **지정 가능** -선택 하는 경우 사용자가 도구 모음을 편집 하 고 사용자 지정할 수 있습니다.
4. **구분 기호** -이 상자를 선택 하면 세로 막대형 가로 줄이 창 내용과 도구 모음을 구분 합니다.
5. **크기** -도구 모음 크기를 설정 합니다.
6. **자동 저장** -선택 하는 경우 응용 프로그램은 응용 프로그램 시작 시 사용자의 도구 모음 구성 변경 내용을 유지 합니다.

**자동 저장** 옵션을 선택 하 고 나머지 속성은 모두 기본 설정으로 그대로 둡니다. 

**인터페이스 계층 구조**에서 도구 모음을 열고 도구 모음 항목을 선택 하 여 사용자 지정 대화 상자를 엽니다.

![도구 모음 사용자 지정](toolbar-images/edit05.png "도구 모음 사용자 지정")

이 대화 상자를 사용 하 여 도구 모음에 이미 포함 된 항목의 속성을 설정 하 고, 응용 프로그램의 기본 도구 모음을 디자인 하 고, 사용자가 도구 모음을 사용자 지정할 때 선택할 수 있는 추가 도구 모음 항목을 제공할 수 있습니다. 도구 모음에 항목을 추가 하려면 **라이브러리 검사자**에서 항목을 끌어 옵니다.

![라이브러리 검사기](toolbar-images/edit06.png "라이브러리 검사기")

다음 도구 모음 항목을 추가할 수 있습니다.

- **이미지 도구 모음 항목** -사용자 지정 이미지를 아이콘으로 사용 하는 도구 모음 항목입니다.
- **유연한 공간 도구 모음 항목** -후속 도구 모음 항목을 정렬 하는 데 사용 되는 유연한 공간입니다. 예를 들어 하나 이상의 도구 모음 항목 뒤에 유연한 공간 도구 모음 항목과 다른 도구 모음 항목이 나오면 마지막 항목을 도구 모음의 오른쪽에 고정 합니다.
- **공간 도구 모음 항목** -도구 모음에서 항목 사이의 고정 간격
- **구분 도구 모음 항목** -그룹화를 위한 두 개 이상의 도구 모음 항목 사이에 표시 되는 구분 기호
- 사용자 **지정 도구 모음 항목** -사용자가 도구 모음을 사용자 지정할 수 있습니다.
- **인쇄 도구 모음 항목** -사용자가 열린 문서를 인쇄할 수 있습니다.
- **색 표시 도구 모음 항목** -표준 시스템 색 선택을 표시 합니다. 

     ![시스템 색 선택](toolbar-images/edit07.png "시스템 색 선택")

- **글꼴 도구 모음 항목 표시** -표준 시스템 글꼴 대화 상자를 표시 합니다. 

     ![글꼴 선택기] 입니다. (toolbar-images/edit08.png "글꼴 선택기") 입니다.

> [!IMPORTANT]
> 나중에 볼 수 있듯이 검색 필드, 분할 된 컨트롤 및 가로 슬라이더와 같은 많은 표준 Cocoa UI 컨트롤을 도구 모음에 추가할 수도 있습니다.

### <a name="adding-an-item-to-a-toolbar"></a>도구 모음에 항목 추가

도구 모음에 항목을 추가 하려면 **인터페이스 계층 구조** 에서 도구 모음을 선택 하 고 해당 항목 중 하나를 클릭 하 여 사용자 지정 대화 상자를 표시 합니다. 다음으로 **라이브러리 검사기** 에서 **허용 되는 도구 모음 항목** 영역으로 새 항목을 끌어 옵니다.

![도구 모음 사용자 지정 대화 상자에서 허용 되는 도구 모음 항목](toolbar-images/add01.png "도구 모음 사용자 지정 대화 상자에서 허용 되는 도구 모음 항목")

새 항목이 기본 도구 모음의 일부 인지 확인 하려면 **기본 도구 모음 항목** 영역으로 끕니다. 

![드래그 하 여 도구 모음 항목 다시 정렬](toolbar-images/add02.png "드래그 하 여 도구 모음 항목 다시 정렬")

기본 도구 모음 항목을 다시 정렬 하려면 왼쪽 또는 오른쪽으로 끌어 놓습니다.

그런 다음 **특성 검사자** 를 사용 하 여 항목의 기본 속성을 설정 합니다.

![특성 검사자를 사용 하 여 도구 모음 항목 사용자 지정](toolbar-images/add03.png "특성 검사자를 사용 하 여 도구 모음 항목 사용자 지정")

사용할 수 있는 속성은 다음과 같습니다.

- **이미지 이름** -항목에 대 한 아이콘으로 사용할 이미지
- **레이블** -도구 모음에서 항목에 대해 표시할 텍스트입니다.
- **색상표 레이블** - **허용 되는 도구 모음 항목** 영역의 항목에 대해 표시할 텍스트입니다.
- **Tag** -코드에서 항목을 식별 하는 데 도움이 되는 선택적 고유 식별자입니다.
- **Identifier** -도구 모음 항목 형식을 정의 합니다. 사용자 지정 값을 사용 하 여 코드에서 도구 모음 항목을 선택할 수 있습니다.
- **선택** 가능-선택 하는 경우 항목은 설정/해제 단추 처럼 동작 합니다.

> [!IMPORTANT]
> **허용 되는 도구 모음 항목** 영역에 항목을 추가 하지만 기본 도구 모음이 아닌 사용자에 대 한 사용자 지정 옵션을 제공 합니다. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>도구 모음에 다른 UI 컨트롤 추가

검색 필드 및 분할 된 컨트롤과 같은 여러 Cocoa UI 요소를 도구 모음에 추가할 수도 있습니다.

이를 시도 하려면 **인터페이스 계층 구조** 에서 도구 모음을 열고 도구 모음 항목을 선택 하 여 사용자 지정 대화 상자를 엽니다. **라이브러리 검사기** 에서 **허용 되는 도구 모음 항목** 영역으로 **검색 필드** 를 끌어 옵니다.

![도구 모음 사용자 지정 대화 상자 사용](toolbar-images/add05.png "도구 모음 사용자 지정 대화 상자 사용")

여기에서 Interface Builder 사용 하 여 검색 필드를 구성 하 고 작업 또는 콘센트를 통해 코드에 노출 합니다.

## <a name="built-in-toolbar-item-support"></a>기본 제공 도구 모음 항목 지원

여러 Cocoa UI 요소는 기본적으로 표준 도구 모음 항목과 상호 작용 합니다. 예를 들어 **텍스트 뷰** 를 응용 프로그램 창으로 끌어 콘텐츠 영역을 채우도록 배치 합니다.

[![응용 프로그램에 텍스트 뷰 추가](toolbar-images/edit09.png "응용 프로그램에 텍스트 뷰 추가")](toolbar-images/edit09-large.png#lightbox)

문서를 저장 하 고, Xcode와 동기화 하기 위해 Mac용 Visual Studio로 돌아가서, 응용 프로그램을 실행 하 고, 텍스트를 입력 하 고, **색** 도구 모음 항목을 클릭 합니다. 텍스트 뷰는 색 선택과 함께 자동으로 작동 합니다.

![텍스트 보기 및 색 선택을 사용 하는 기본 제공 도구 모음 기능](toolbar-images/edit10.png "텍스트 보기 및 색 선택을 사용 하는 기본 제공 도구 모음 기능")

## <a name="using-images-with-toolbar-items"></a>도구 모음 항목과 함께 이미지 사용

**이미지 도구 모음 항목**을 사용 하 여 **리소스** 폴더 (및 **번들 리소스**의 빌드 작업 지정)에 추가 된 비트맵 이미지를 도구 모음에 아이콘으로 표시할 수 있습니다.

1. Mac용 Visual Studio의 **Solution Pad**에서 **리소스** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **추가** > **파일**추가를 선택 합니다.
2. **파일 추가** 대화 상자에서 원하는 이미지로 이동 하 여 선택 하 고 **열기** 단추를 클릭 합니다. 

    [![추가할 이미지 선택](toolbar-images/edit11.png "추가할 이미지 선택")](toolbar-images/edit11-large.png#lightbox)

3. **복사**를 선택 하 고 **선택한 모든 파일에 동일한 작업 사용**을 선택 하 고 **확인**을 클릭 합니다.

    ![추가 된 이미지에 대 한 복사 작업 선택](toolbar-images/edit12.png "추가 된 이미지에 대 한 복사 작업 선택")

4. **Solution Pad**에서 xib를 두 번 클릭 하 여 Xcode에서 엽니다 **mainwindow.xaml.**

5. **인터페이스 계층 구조** 에서 도구 모음을 선택 하 고 해당 항목 중 하나를 클릭 하 여 사용자 지정 대화 상자를 엽니다.

6. **라이브러리 검사기** 의 **이미지 도구 모음 항목** 을 도구 모음의 **허용 되는 도구 모음 항목** 영역으로 끌어 옵니다. 

    ![허용 되는 도구 모음 항목 영역에 추가 된 이미지 도구 모음 항목](toolbar-images/edit14.png "허용 되는 도구 모음 항목 영역에 추가 된 이미지 도구 모음 항목")

7. **특성 검사자**에서 방금 Mac용 Visual Studio에 추가 된 이미지를 선택 합니다. 

    ![도구 모음 항목에 대 한 사용자 지정 이미지 설정](toolbar-images/edit15.png "도구 모음 항목에 대 한 사용자 지정 이미지 설정")

8. **레이블을** "휴지통"으로 설정 하 고 **색상표 레이블을** "문서 지우기"로 설정 합니다. 

    ![도구 모음 항목 레이블 및 색상표 레이블 설정](toolbar-images/edit16.png "도구 모음 항목 레이블 및 색상표 레이블 설정")

9. **라이브러리 검사기** 에서 도구 모음의 **허용 되는 도구 모음 항목** 영역으로 **구분 도구 모음 항목** 을 끌어 옵니다. 

    [![허용 되는 도구 모음 항목 영역에 추가 된 구분 도구 모음 항목](toolbar-images/edit17.png "허용 되는 도구 모음 항목 영역에 추가 된 구분 도구 모음 항목")](toolbar-images/edit17-large.png#lightbox)

10. 구분 기호 항목 및 "휴지통" 항목을 **기본 도구 모음 항목** 영역으로 끌고, 다음과 같이 도구 모음 항목의 순서를 왼쪽에서 오른쪽으로 설정 합니다 (색, 글꼴, 구분 기호, 휴지통, 유연한 공간, 인쇄). 

    ![기본 도구 모음 항목](toolbar-images/edit18.png "기본 도구 모음 항목")

11. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

응용 프로그램을 실행 하 여 기본적으로 새 도구 모음이 표시 되는지 확인 합니다.

![사용자 지정 된 기본 항목을 포함 하는 도구 모음](toolbar-images/edit19.png "사용자 지정 된 기본 항목을 포함 하는 도구 모음")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>콘센트 및 작업을 사용 하 여 도구 모음 항목 노출

코드의 도구 모음 또는 도구 모음 항목에 액세스 하려면 콘센트 또는 작업에 연결 해야 합니다.

1. **Solution Pad**에서 **주. storyboard** 를 두 번 클릭 하 여 Xcode에서 엽니다.
2. 사용자 지정 클래스 "WindowController"가 **Id 검사자**에서 주 창 컨트롤러에 할당 되었는지 확인 합니다.

    [![Identity Inspector를 사용 하 여 창 컨트롤러에 대 한 사용자 지정 클래스 설정](toolbar-images/edit20a.png "Identity Inspector를 사용 하 여 창 컨트롤러에 대 한 사용자 지정 클래스 설정")](toolbar-images/edit20a-large.png#lightbox)

3. 다음으로 **인터페이스 계층 구조**에서 도구 모음 항목을 선택 합니다. 

    ![인터페이스 계층 구조에서 도구 모음 항목 선택](toolbar-images/edit20.png "인터페이스 계층 구조에서 도구 모음 항목 선택")  

4. **길잡이 뷰**를 열고 **windowcontroller .h** 파일을 선택한 다음 도구 모음 항목에서 **windowcontroller .h** 파일로 컨트롤을 끕니다.
5. **연결** 유형을 **작업**으로 설정 하 고 **이름**으로 "Trashdocument"를 입력 한 다음 **연결** 단추를 클릭 합니다. 

    [![도구 모음 항목에 대 한 작업 구성](toolbar-images/edit23.png "도구 모음 항목에 대 한 작업 구성")](toolbar-images/edit23-large.png#lightbox)

6. **Viewcontroller .h** 파일에서 "documenteditor" 라는 콘센트로 **텍스트 뷰** 를 노출 합니다. 

    [![텍스트 보기에 대 한 유출 구성](toolbar-images/edit24.png "텍스트 보기에 대 한 유출 구성")](toolbar-images/edit24-large.png#lightbox)

7. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

Mac용 Visual Studio에서 **ViewController.cs** 파일을 편집 하 고 다음 코드를 추가 합니다.

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

그런 다음 **WindowController.cs** 파일을 편집 하 고 `WindowController` 클래스의 맨 아래에 다음 코드를 추가 합니다.

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

응용 프로그램을 실행할 때 **휴지통** 도구 모음 항목이 활성화 됩니다.

![활성 휴지통 항목이 있는 도구 모음](toolbar-images/edit25.png "활성 휴지통 항목이 있는 도구 모음")

이제 **휴지통** 도구 모음 항목을 사용 하 여 텍스트를 삭제할 수 있습니다.

## <a name="disabling-toolbar-items"></a>도구 모음 항목 사용 안 함

도구 모음에서 항목을 사용 하지 않도록 설정 하려면 사용자 `NSToolbarItem` 지정 클래스를 만들고 `Validate` 메서드를 재정의 합니다. 그런 다음 Interface Builder에서 사용/사용 하지 않을 항목에 사용자 지정 형식을 할당 합니다.

사용자 지정 `NSToolbarItem` 클래스를 만들려면 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 파일**...을 선택 합니다. **일반**빈 클래스를 선택 하 고 이름에 "ActivatableItem"를 입력 한 다음 새로 만들기 단추를 클릭 합니다. >  

![Mac용 Visual Studio에서 빈 클래스 추가](toolbar-images/custom01.png "Mac용 Visual Studio에서 빈 클래스 추가")

그런 다음 **ActivatableItem.cs** 파일을 다음과 같이 편집 합니다.

```csharp
using System;

using Foundation;
using AppKit;

namespace MacToolbar
{
    [Register("ActivatableItem")]
    public class ActivatableItem : NSToolbarItem
    {
        public bool Active { get; set;} = true;

        public ActivatableItem ()
        {
        }

        public ActivatableItem (IntPtr handle) : base (handle)
        {
        }

        public ActivatableItem (NSObjectFlag  t) : base (t)
        {
        }

        public ActivatableItem (string title) : base (title)
        {
        }

        public override void Validate ()
        {
            base.Validate ();
            Enabled = Active;
        }
    }
}
```

Xcode에서 열려면 **주 storyboard** 를 두 번 클릭 합니다. 위에서 만든 **휴지통** 도구 모음 항목을 선택 하 고 **Identity Inspector**에서 해당 클래스를 "ActivatableItem"로 변경 합니다.

![도구 모음 항목에 대 한 사용자 지정 클래스 설정](toolbar-images/custom02.png "도구 모음 항목에 대 한 사용자 지정 클래스 설정")

휴지통 도구 모음 항목 `trashItem` 에 대해 호출 되는 콘센트를 만듭니다. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다. 마지막으로 **MainWindow.cs** 를 열고 다음과 같이 `AwakeFromNib` 메서드를 업데이트 하 여 읽습니다.

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

응용 프로그램을 실행 하 고, 도구 모음에서 **휴지통** 항목이 사용 하지 않도록 설정 되어 있는지 확인 합니다.

![비활성 휴지통 항목이 있는 도구 모음](toolbar-images/custom03.png "비활성 휴지통 항목이 있는 도구 모음")

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램의 도구 모음 및 도구 모음 항목 작업에 대해 자세히 살펴봅니다. Xcode의 Interface Builder에서 도구 모음을 만들고 유지 관리 하는 방법, 일부 UI 컨트롤이 도구 모음 항목에서 자동으로 작동 하는 방법, 코드 C# 에서 도구 모음을 사용 하도록 설정 및 해제 하는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [MacToolbar (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/mactoolbar)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [도구 모음에 대 한 인간 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [도구 모음 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
