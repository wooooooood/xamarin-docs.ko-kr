---
title: Xamarin.Mac의 도구 모음
description: 이 문서에서는 Xamarin.Mac 응용 프로그램의 도구 모음을 사용 하 여 작업을 설명 합니다. Xcode 및 Interface Builder를 코드에 노출 하 고 프로그래밍 방식으로 작업에서 만들고 유지 관리 도구 모음을 설명 합니다.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6cb17ae0f60390564a8aa6bdb64ea612aae51b55
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120257"
---
# <a name="toolbars-in-xamarinmac"></a>Xamarin.Mac의 도구 모음

_이 문서에서는 Xamarin.Mac 응용 프로그램의 도구 모음을 사용 하 여 작업을 설명 합니다. Xcode 및 Interface Builder를 코드에 노출 하 고 프로그래밍 방식으로 작업에서 만들고 유지 관리 도구 모음을 설명 합니다._

Mac 용 Visual Studio를 사용 하 여 작업 Xamarin.Mac 개발자가 동일한 UI 컨트롤 도구 모음 컨트롤을 포함 하 여 Xcode를 사용 하는 macOS 개발자에 게 제공 합니다. Xamarin.Mac이 Xcode와 직접 통합 되므로 Xcode의 Interface Builder를 사용 하 여 도구 모음 항목을 만들 수 있습니다. 또한 C#에서 이러한 도구 모음 항목을 만들 수 있습니다.

MacOS의 도구 모음 창의 맨 위 섹션에 추가 되 고 해당 기능과 관련 된 명령에 쉽게 액세스할 수 있습니다. 도구 모음 숨겨진, 표시 하거나 응용 프로그램의 사용자가 사용자 지정할 수 있으며 다양 한 방법으로 도구 모음 항목을 제공할 수 있습니다.

이 문서에서는 도구 모음 및 Xamarin.Mac 응용 프로그램에서 도구 모음 항목을 사용 하는 기본 사항을 다룹니다. 

읽고 진행 하기 전에 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 문서-특히 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션-하므로 주요 개념 및이 가이드 전체에서 사용 되는 기술을 설명 합니다.

볼 수 있습니다 합니다 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 문서. 에 대해 설명 합니다 `Register` 및 `Export` Objective-c 클래스를 C# 클래스를 연결 하는 데 사용 되는 특성입니다.

## <a name="introduction-to-toolbars"></a>도구 모음 소개

MacOS 응용 프로그램에서 모든 창 도구 모음을 포함할 수 있습니다.

![도구 모음으로는 예제 윈도우](toolbar-images/info01.png "도구 모음으로는 예제 윈도우")

도구 모음 중요에 빠르게 액세스 하려면 응용 프로그램의 사용자가 일반적으로 사용 되는 기능에는 쉬운 방법을 제공 합니다. 예를 들어, 문서 편집 응용 프로그램을 도구 모음 항목의 텍스트 색을 설정, 글꼴을 변경 하거나 현재 문서 인쇄를 제공할 수 있습니다.

도구 모음 세 가지 방법으로 항목을 표시할 수 있습니다.

1. **아이콘 및 텍스트** 

     ![아이콘 및 텍스트를 사용 하 여 도구 모음](toolbar-images/info02.png "아이콘 및 텍스트를 사용 하 여 도구 모음")

2. **만 아이콘** 

     ![프로그램 아이콘 전용 도구 모음](toolbar-images/info03.png "프로그램 아이콘 전용 도구 모음")

3. **텍스트 전용** 

     ![텍스트 전용 도구 모음](toolbar-images/info04.png "텍스트 전용 도구 모음")

도구 모음을 마우스 오른쪽 단추로 클릭 하 고 상황에 맞는 메뉴에서 디스플레이 모드를 선택 하 여 이러한 모드를 전환 합니다.

![도구 모음에 대 한 상황에 맞는 메뉴](toolbar-images/info05.png "도구 모음에 대 한 상황에 맞는 메뉴")

동일한 메뉴를 사용 하 여 작은 크기로 도구 모음을 표시 합니다.

![작은 아이콘을 사용 하 여 도구 모음](toolbar-images/info06.png "작은 아이콘을 사용 하 여 도구 모음")

메뉴 도구 모음 사용자 지정 하기 위한 수도 있습니다.

![도구 모음 사용자 지정 하는 데 사용 하 여 대화 상자](toolbar-images/info07.png "도구 모음 사용자 지정 하는 데 사용 하 여 대화 상자")

Xcode의 Interface Builder에서 도구 모음을 설정 하는 경우 개발자는 기본 구성의 일부분이 아닌 추가 도구 모음 항목을 제공할 수 있습니다. 응용 프로그램의 사용자가 사용자 지정할 수 도구 모음, 필요에 따라 이러한 미리 정의 된 항목 추가 및 제거 합니다. 물론, 도구 모음을 해당 기본 구성으로 재설정할 수 있습니다.

도구 모음에 자동으로 연결 합니다 **보기** 숨기지를 표시 하 고 사용자 지정할 수 있는 메뉴에서:

![보기 메뉴에서 도구 모음-관련 항목](toolbar-images/info08.png "보기 메뉴에서 도구 모음-관련 항목")

참조를 [기본 제공 메뉴 기능](~/mac/user-interface/menu.md) 자세한 세부 정보에 대 한 설명서입니다.

또한 Interface Builder에서 도구 모음이 제대로 구성 하는 경우 응용 프로그램은 응용 프로그램의 여러 실행 간에 도구 모음 사용자 지정을 자동으로 유지 됩니다.

이 가이드의 다음 섹션에는 만들고 Xcode의 Interface Builder를 사용 하 여 도구 모음을 유지 관리 하는 방법 및 코드에서 사용 하는 방법을 설명 합니다.

## <a name="setting-a-custom-main-window-controller"></a>사용자 지정 주 창의 컨트롤러 설정

출 선 및 작업, Xamarin.Mac 앱을 통해 C# 코드에 UI 요소를 노출 하는 사용자 지정 창 컨트롤러를 사용 해야 합니다.

1. Xcode의 Interface Builder에서 앱의 스토리 보드를 엽니다.
2. 디자인 화면의 창 컨트롤러를 선택 합니다.
3. 전환할 합니다 **검사기** "WindowController"으로 입력 합니다 **클래스 이름**: 

    [![창 컨트롤러에 대 한 사용자 지정 클래스 이름을 설정할](toolbar-images/windowcontroller01.png "창 컨트롤러에 대 한 사용자 지정 클래스 이름 설정")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. 변경 내용을 저장 하 고 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.
5. A **WindowController.cs** 에서 프로젝트에 추가할 파일을 **Solution Pad** Mac 용 Visual Studio에서: 

    ![Solution Pad에서 WindowController.cs 선택](toolbar-images/windowcontroller02.png "WindowController.cs Solution Pad에서 선택")

6. Xcode의 Interface Builder 스토리 보드를 다시 엽니다.
7. 합니다 **WindowController.h** 파일 용도로 제공 됩니다. 

    [![WindowController.h 파일](toolbar-images/windowcontroller03.png "The WindowController.h 파일")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>만들기 및 Xcode의 도구 모음을 유지 관리

도구 모음 생성 및 Xcode의 Interface Builder를 사용 하 여 유지 관리 됩니다. 도구 모음에 응용 프로그램에 추가 하려면 앱의 주 스토리 보드를 편집 (이 예제의 **Main.storyboard**)에서 두 번 클릭 합니다 **Solution Pad**:

![Main.storyboard 솔루션 패드에서 열기](toolbar-images/edit01.png "Main.storyboard 솔루션 패드에서 열기")

에 **라이브러리 검사기**에 "도구"를 입력 합니다 **검색 상자** 쉽게 사용할 수 있는 도구 모음 항목을 모두 볼 수 있도록:

![라이브러리 검사기 도구 모음 항목을 표시 하도록 필터링](toolbar-images/edit02.png "도구 모음 항목을 표시 하도록 필터링은 라이브러리 검사기")

도구 모음에서 창을 끌 합니다 **인터페이스 편집기**합니다. 선택한 도구 모음을 사용 하 여에서 속성을 설정 하 여 해당 동작을 구성 합니다 **특성 검사기**:

![도구 모음에 대 한 특성 검사기](toolbar-images/edit04.png "도구 모음에 대 한 특성 검사기")

사용할 수 있는 속성은 다음과 같습니다.

1. **표시** -도구 모음 아이콘, 텍스트 또는 둘 다 표시 되는지 여부를 제어 합니다.
2. **시작 시 표시** -를 선택한 경우에 도구 모음에서 기본적으로 표시 됩니다.
3. **사용자 지정 가능한** -사용자가 편집 하 고 도구 모음 사용자 지정 수를 선택 하는 경우.
4. **구분 기호** -씬 가로줄 창의 내용에서 도구 모음을 구분을 선택 합니다.
5. **크기** -도구 모음 크기를 설정 합니다.
6. **자동 저장** -응용 프로그램 사용자의 도구 모음에 대 한 구성 변경 내용을 응용 프로그램 실행 간에 유지 됩니다을 선택 합니다.

선택 된 **자동 저장** 옵션 및 해당 기본 설정을 그대로 두고 다른 모든 속성. 

도구 모음을 연 후 합니다 **인터페이스 계층 구조**, 도구 모음 항목을 선택 하 여 사용자 지정 대화 상자를 엽니다.

![도구 모음 사용자 지정](toolbar-images/edit05.png "도구 모음 사용자 지정")

이 대화 상자를 사용 하 여 이미 도구 모음, 응용 프로그램에 대 한 기본 도구 모음의 디자인의 일부인 항목에 대 한 속성을 설정 하 고 도구 모음 사용자 지정 하는 시기를 선택 하려면 사용자에 대 한 추가 도구 모음 항목을 제공 합니다. 항목 도구 모음에 추가할에서 끌어온 합니다 **라이브러리 검사기**:

![라이브러리 검사기](toolbar-images/edit06.png "라이브러리 검사기")

다음 도구 모음 항목을 추가할 수 있습니다.

- **도구 모음 항목 이미지** -아이콘으로 사용자 지정 이미지를 사용 하 여 도구 모음 항목입니다.
- **유연한 공간 도구 모음 항목** -유연한 공간 후속 도구 모음 항목을 정당화 하는 데 사용 합니다. 예를 들어, 유연한 공간 도구 모음 항목 뒤에 하나 이상의 도구 모음 항목 및 다른 도구 모음 항목 도구 모음 오른쪽에 있는 마지막 항목 고정 하는 것입니다.
- **도구 모음 항목 공간** -도구 모음에서 항목 사이의 간격을 고정
- **구분 기호 도구 모음 항목** -그룹화에 대 한 두 개 이상의 도구 모음 항목 간의 구분 기호 표시
- **도구 모음 항목을 사용자 지정** -사용자는 도구 모음을 사용자 지정할 수 있습니다.
- **도구 모음 항목을 인쇄** -사용자가 열려 있는 문서를 인쇄 하도록 허용
- **색 도구 모음 항목 표시** -표준 시스템 색 선택이 표시 됩니다. 

     ![시스템 색 선택기](toolbar-images/edit07.png "시스템 색 선택")

- **글꼴 도구 모음 항목 표시** -표준 시스템 글꼴 대화 상자를 표시 합니다. 

     ![글꼴 선택기](toolbar-images/edit08.png "글꼴 선택기")

> [!IMPORTANT]
> 것과 나중에 검색 필드, 분할 된 컨트롤 및 가로 슬라이더와 같은 표준 많은 Cocoa UI 컨트롤 도구 모음에도 추가할 수 있습니다.

### <a name="adding-an-item-to-a-toolbar"></a>도구 모음에 항목을 추가합니다.

항목 도구 모음에 추가 하려면 도구 모음을 선택 합니다 **인터페이스 계층 구조** 원인이 사용자 지정 대화 상자를 표시 하 고 항목을 중 하나를 클릭 합니다. 다음으로에서 새 항목을 끌어 합니다 **라이브러리 검사기** 에 **도구 모음 항목 허용** 영역:

![도구 모음 사용자 지정 대화 상자에서 허용 하는 도구 모음 항목](toolbar-images/add01.png "도구 모음 사용자 지정 대화 상자에서 허용 된 도구 모음 항목")

새 항목을 기본 도구 모음의 부분 인지를 확인, 끕니다 합니다 **기본 도구 모음 항목** 영역: 

![도구 모음 항목을 끌어 다시 정렬](toolbar-images/add02.png "도구 모음 항목을 끌어 다시 정렬")

기본 도구 모음 항목의 순서를 끌어 왼쪽 또는 오른쪽.

다음을 사용 하는 **특성 검사기** 항목에 대 한 기본 속성을 설정 하려면:

![특성 검사기를 사용 하 여 도구 모음 항목을 사용자 지정](toolbar-images/add03.png "특성 검사기를 사용 하 여 도구 모음 항목을 사용자 지정")

사용할 수 있는 속성은 다음과 같습니다.

- **이미지 이름** -항목에 대 한 아이콘으로 사용할 이미지
- **레이블** -도구 모음에서 항목에 대해 표시할 텍스트
- **색상표 레이블을** -항목에 대해 표시할 텍스트를 **도구 모음 항목 허용** 영역
- **태그** -코드에서 항목을 식별 하는 데 도움이 되는 선택적 고유 식별자입니다.
- **식별자** -도구 모음 항목 형식을 정의 합니다. 코드에서 도구 모음 항목을 선택 하려면 사용자 지정 값을 사용할 수 있습니다.
- **선택 가능한** -항목 켜기/끄기 단추를 같은 역할을 선택 하는 경우 합니다.

> [!IMPORTANT]
> 항목을 추가 합니다 **도구 모음 항목 허용** 영역 하지만 사용자에 대 한 사용자 지정 옵션을 제공 하려면 기본 도구 모음의 없습니다. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>도구 모음에 다른 UI 컨트롤을 추가합니다.

와 같은 여러 Cocoa UI 요소 필드를 검색 하 고 분할 된 컨트롤 도구 모음에 추가할 수도 있습니다.

이 사용 하려면 도구 모음을 엽니다는 **인터페이스 계층 구조** 사용자 지정 대화 상자를 열려면 도구 모음 항목을 선택 합니다. 끌어서를 **검색 필드** 에서 합니다 **라이브러리 검사기** 에 **도구 모음 항목 허용** 영역:

![도구 모음 사용자 지정 대화 상자를 사용 하 여](toolbar-images/add05.png "도구 모음 사용자 지정 대화 상자를 사용 하 여")

여기에서 Interface Builder를 사용 하 여 검색 필드를 구성 하 여 작업 또는 콘센트를 통해 코드에 노출 합니다.

## <a name="built-in-toolbar-item-support"></a>기본 제공 도구 모음 항목 지원

기본적으로 표준 도구 모음 항목을 사용 하 여 여러 Cocoa UI 요소가 상호 작용 합니다. 예를 들어 끌어를 **텍스트 뷰** 응용 프로그램의 창으로 놓습니다 콘텐츠 영역을 채우는:

[![응용 프로그램에 텍스트 뷰를 추가](toolbar-images/edit09.png "응용 프로그램에 텍스트 뷰를 추가 합니다.")](toolbar-images/edit09-large.png#lightbox)

Xcode와 동기화, 응용 프로그램을 실행, 텍스트 입력, 선택 및 클릭 Mac 용 Visual Studio로 돌아가서 문서를 저장 합니다 **색** 도구 모음 항목입니다. 텍스트 뷰 색 선택기를 사용 하 여 자동으로 작동 하는지 확인 합니다.

![텍스트 뷰 및 색상 선택기를 사용 하 여 기본 제공 도구 모음 기능](toolbar-images/edit10.png "텍스트 뷰 및 색상 선택기를 사용 하 여 기본 제공 도구 모음 기능")

## <a name="using-images-with-toolbar-items"></a>도구 모음 항목을 사용 하 여 이미지를 사용 하 여

사용 하 여는 **이미지 도구 모음 항목**, 비트맵 이미지에 추가 합니다 **리소스** 폴더 (빌드 작업을 지정 하 고 **번들 리소스**) 도구 모음 아이콘으로 표시할 수 있습니다:

1. Mac 용 Visual Studio에서에 **Solution Pad**를 마우스 오른쪽 단추로 클릭 합니다 **리소스** 폴더를 선택 **추가** > **파일 추가** .
2. **파일 추가** 대화 상자에서 원하는 이미지를 이동, 모두 선택 하 고 클릭 합니다 **열려** 단추: 

    [![추가할 이미지 선택](toolbar-images/edit11.png "추가할 이미지 선택")](toolbar-images/edit11-large.png#lightbox)

3. 선택 **복사본**, 확인 **동일한 작업을 사용 하 여 선택한 모든 파일에 대 한**를 클릭 하 고 **확인**:

    ![추가 이미지에 대 한 복사 작업을 선택 하](toolbar-images/edit12.png "추가 이미지에 대 한 복사 작업을 선택 합니다.")

4. 에 **Solution Pad**를 두 번 클릭 **MainWindow.xib** Xcode에서 엽니다.

5. 도구 모음을 선택 합니다 **인터페이스 계층 구조** 사용자 지정 대화 상자를 열려면 해당 항목 중 하나를 클릭 합니다.

6. 끌어서를 **이미지 도구 모음 항목** 에서 합니다 **라이브러리 검사기** 는 도구 모음에 **허용 하는 도구 모음 항목** 영역: 

    ![이미지의 도구 모음 항목을 허용 하는 도구 모음 항목 영역에 추가](toolbar-images/edit14.png "허용 도구 모음 항목 영역에는 이미지의 도구 모음 항목 추가")

7. 에 **특성 검사기**, Mac 용 Visual Studio의 방금 추가 된 이미지를 선택 합니다. 

    ![도구 모음 항목에 대 한 사용자 지정 이미지를 설정](toolbar-images/edit15.png "도구 모음 항목에 대 한 사용자 지정 이미지를 설정 합니다.")

8. 설정 합니다 **레이블** "휴지통" 및 **팔레트 레이블** "문서 지울": 

    ![레이블 및 색상표 레이블 항목 도구 모음을 설정](toolbar-images/edit16.png "레이블 및 색상표 레이블 항목 도구 모음 설정")

9. 끌어서를 **도구 모음 항목 구분 기호** 에서 합니다 **라이브러리 검사기** 를 도구 모음 **도구 모음 항목 허용** 영역: 

    [![구분 기호 도구 모음 항목을 허용 하는 도구 모음 항목 영역에 추가](toolbar-images/edit17.png "허용 도구 모음 항목 영역에는 구분 기호 도구 모음 항목 추가")](toolbar-images/edit17-large.png#lightbox)

10. 구분 기호 항목 및 "휴지통" 항목을 끌어 합니다 **도구 모음 항목 기본** 영역 및 도구 모음 순서에서 항목 집합 왼쪽에서 오른쪽 (색, 글꼴, 구분 기호, 휴지통, 유연한 공간, 인쇄) 같이: 

    ![기본 도구 모음 항목](toolbar-images/edit18.png "기본 도구 모음 항목")

11. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

기본적으로 새 도구 모음이 표시 되는지 확인 하려면 응용 프로그램을 실행 합니다.

![지정 된 기본 항목을 사용 하 여 도구 모음](toolbar-images/edit19.png "사용자 지정 된 기본 항목을 사용 하 여 도구 모음")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>출 선 및 작업을 사용 하 여 도구 모음 항목 표시

도구 모음 또는 코드에서 도구 모음 항목에 액세스 하려면 출 선 또는 액션을 연결 해야 합니다.

1. 에 **Solution Pad**를 두 번 클릭 **Main.storyboard** Xcode에서 엽니다.
2. "WindowController"에 있는 주 창 컨트롤러에 할당 된 사용자 지정 클래스 있는지 확인 합니다 **검사기**:

    [![검사기를 사용 하 여 창 컨트롤러에 대 한 사용자 지정 클래스를 설정할](toolbar-images/edit20a.png "검사기를 사용 하 여 창 컨트롤러에 대 한 사용자 지정 클래스를 설정 하려면")](toolbar-images/edit20a-large.png#lightbox)

3. 다음으로, 도구 모음 항목을 선택 합니다 **인터페이스 계층 구조**: 

    ![인터페이스 계층 구조에서 도구 모음 항목을 선택 하면](toolbar-images/edit20.png "인터페이스 계층 구조에서 도구 모음 항목을 선택 합니다.")  

4. 열기는 **도우미 보기**를 선택 합니다 **WindowController.h** 파일 및 도구 모음 항목의 컨트롤 끌기를 **WindowController.h** 파일.
5. 설정를 **연결** 형식을 **작업**, "trashDocument"을 입력 합니다 **이름**, 클릭를 **연결** 단추: 

    [![도구 모음 항목에 대 한 동작을 구성](toolbar-images/edit23.png "도구 모음 항목에 대 한 액션 구성")](toolbar-images/edit23-large.png#lightbox)

6. 노출 된 **텍스트 뷰** 에서 "documentEditor" 이라는 출 선으로는 **ViewController.h** 파일: 

    [![텍스트 보기에 대 한 콘센트를 구성](toolbar-images/edit24.png "텍스트 보기에 대 한 콘센트를 구성 합니다.")](toolbar-images/edit24-large.png#lightbox)

7. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

Mac 용 Visual Studio에서 편집 합니다 **ViewController.cs** 파일과 다음 코드를 추가 합니다.

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

다음에 편집 합니다 **WindowController.cs** 파일 및 맨 아래에 다음 코드를 추가 합니다 `WindowController` 클래스:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

응용 프로그램을 실행 하는 경우는 **휴지통** 도구 모음 항목 활성화 됩니다.

![활성 휴지통 항목을 사용 하 여 도구 모음](toolbar-images/edit25.png "active 휴지통 항목을 사용 하 여 도구 모음")

다음에 유의 합니다 **휴지통** 텍스트를 삭제 하려면 도구 모음 항목 이제 사용할 수 있습니다.

## <a name="disabling-toolbar-items"></a>도구 모음 항목을 사용 하지 않도록 설정

사용자 지정 만들기 도구 모음에서 항목을 사용 하지 않으려면 `NSToolbarItem` 클래스를 재정의 합니다 `Validate` 메서드. 그런 다음 Interface Builder에서 사용자 지정 형식을 설정/해제 하려는 항목에 할당 합니다.

사용자 지정을 만들려면 `NSToolbarItem` 클래스는 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** . 선택 **일반** > **빈 클래스**, "ActivatableItem"을 입력 합니다 **이름**, 클릭 합니다 **새로 만들기** 단추: 

![Mac 용 Visual Studio에서 빈 클래스를 추가](toolbar-images/custom01.png "Mac 용 Visual Studio에서 빈 클래스 추가")

다음으로 편집 합니다 **ActivatableItem.cs** 를 다음과 같이 파일:

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

두 번 클릭 **Main.storyboard** Xcode에서 엽니다. 선택 된 **휴지통** 도구 모음 항목 위에서 만든 및 해당 클래스에서 "ActivatableItem" 변경 합니다 **검사기**:

![도구 모음 항목에 대 한 사용자 지정 클래스를 설정](toolbar-images/custom02.png "도구 모음 항목에 대 한 사용자 지정 클래스를 설정 합니다.")

호출 아웃렛 만들기 `trashItem` 에 대 한 합니다 **휴지통** 도구 모음 항목입니다. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다. 마지막으로, 열 **MainWindow.cs** 업데이트 및는 `AwakeFromNib` 메서드를 다음과 같이 합니다.

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

응용 프로그램을 실행 하 고는 합니다 **휴지통** 항목 이제 도구 모음에서 사용할 수 없습니다.

![비활성 휴지통 항목을 사용 하 여 도구 모음](toolbar-images/custom03.png "비활성 휴지통 항목을 사용 하 여 도구 모음")

## <a name="summary"></a>요약

이 문서에서는 자세히 살펴보고 도구 모음 및 Xamarin.Mac 응용 프로그램에서 도구 모음 항목을 사용 하 여 작업을 수행 했습니다. 만들고 Xcode의 Interface Builder에서 도구 모음을 유지 관리 하는 방법, 일부 UI 컨트롤 도구 모음 항목을 사용 하 여 자동으로 작동 하는 방법, C# 코드에서는 도구 모음을 사용 하는 방법 및 사용 하도록 설정 하 고 도구 모음 항목을 사용 하지 않도록 설정 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [MacToolbar (샘플)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [도구 모음에 대 한 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [도구 모음 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
