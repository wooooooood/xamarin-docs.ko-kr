---
title: 도구 모음
description: 이 문서에서는 작업 Xamarin.Mac 응용 프로그램에서 도구 모음을 설명 합니다. Xcode 및 코드에 노출 하 고 이러한 작업을 프로그래밍 방식으로 인터페이스 작성기에서 작성 및 유지 관리 도구 모음을 설명 합니다.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 729c5c69d80c52047585d1026d7c675f3267f34e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="toolbars"></a>도구 모음

_이 문서에서는 작업 Xamarin.Mac 응용 프로그램에서 도구 모음을 설명 합니다. Xcode 및 코드에 노출 하 고 이러한 작업을 프로그래밍 방식으로 인터페이스 작성기에서 작성 및 유지 관리 도구 모음을 설명 합니다._

Mac 용 Visual Studio와 함께 작업 하는 Xamarin.Mac 개발자 권한이 동일한 UI 컨트롤의 도구 모음 컨트롤을 포함 하 여 Xcode 작업 macOS 개발자에 게 제공 합니다. 없기 때문에 Xamarin.Mac Xcode에 직접 통합, Xcode의 인터페이스 작성기 만들고 도구 모음 항목을 유지 관리를 사용 하 합니다. C#에서 이러한 도구 모음 항목을 만들 수도 있습니다.

MacOS에서 도구 모음 창의 맨 위 섹션에 추가 되 고 해당 기능에 관련 된 명령에 쉽게 액세스할 합니다. 도구 모음 숨겨진, 표시 하거나 응용 프로그램의 사용자가 사용자 지정할 수 있으며 다양 한 방법으로 도구 모음 항목을 제공할 수 있습니다.

이 문서에서는 도구 모음 및 도구 모음 항목 Xamarin.Mac 응용 프로그램에서 작업의 기본 사항을 설명 합니다. 

통해 읽기 시작 하기 전에 [Hello, Mac](~/mac/get-started/hello-mac.md) 문서-구체적으로 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션-그대로 이 가이드 전체에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

또한을 살펴보세요는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 문서. 설명의 `Register` 및 `Export` Objective-c 클래스에 C# 클래스를 연결 하는 데 사용 되는 특성입니다.

## <a name="introduction-to-toolbars"></a>도구 모음에는 소개

MacOS 응용 프로그램의 모든 창 도구 모음을 포함할 수 있습니다.

![도구 모음으로는 예제 창이](toolbar-images/info01.png "도구 모음으로는 예제 창")

응용 프로그램의 사용자가 신속 하 게 중요 한를 액세스할 수 또는 자주 사용 하는 기능에 대 한는 손쉬운 방법을 제공 하는 도구 모음입니다. 예를 들어 문서 편집 응용 프로그램 텍스트 색을 설정, 글꼴을 변경 하거나 현재 문서 인쇄를 위한 도구 모음 항목을 제공할 수 있습니다.

도구 모음 세 가지 방법으로 항목을 표시할 수 있습니다.

1. **아이콘 및 텍스트** 

     ![도구 모음 아이콘 및 텍스트는](toolbar-images/info02.png "아이콘 및 텍스트는 도구 모음")

2. **만 아이콘** 

     ![아이콘 으로만 이동 가능한 도구 모음](toolbar-images/info03.png "는 아이콘 으로만 이동 가능한 도구 모음")

3. **텍스트 전용** 

     ![텍스트 전용 도구 모음](toolbar-images/info04.png "텍스트 전용 도구 모음")

도구 모음을 마우스 오른쪽 단추로 클릭 하 고 상황에 맞는 메뉴에서 디스플레이 모드를 선택 하 여 이러한 모드 사이 전환 합니다.

![도구 모음에 대 한 상황에 맞는 메뉴](toolbar-images/info05.png "도구 모음에 대 한 상황에 맞는 메뉴")

동일한 메뉴를 사용 하 여 더 작은 크기에 도구 모음을 표시 하려면:

![작은 아이콘에 있는 도구 모음을](toolbar-images/info06.png "작은 아이콘에 있는 도구 모음")

메뉴 도구 모음을 사용자 지정할 수도 있습니다.

![도구 모음을 사용자 지정 하는 데 사용 하 여 대화 상자](toolbar-images/info07.png "도구 모음을 사용자 지정 하는 데 사용 하 여 대화 상자")

Xcode의 인터페이스 구성 관리자에서 도구 모음을 설정할 때 개발자는 기본 구성의 일부분이 아닌 추가 도구 모음 항목을 제공할 수 있습니다. 그런 다음 응용 프로그램의 사용자는 필요에 따라 이러한 미리 정의 된 항목 추가 및 제거 도구 모음을 지정할 수 있습니다. 물론, 도구 모음을 기본 구성으로 재설정할 수 있습니다.

도구 모음에서 자동으로 연결 하는 **보기** 사용자가 숨길을 표시 하 고 사용자 지정할 수 있는 메뉴:

![보기 메뉴의 도구 모음 관련 항목](toolbar-images/info08.png "보기 메뉴의 도구 모음 관련 항목")

참조는 [기본 제공 메뉴 기능](~/mac/user-interface/menu.md) 자세한 세부 정보에 대 한 설명서입니다.

또한 도구 모음 인터페이스 작성기에서 제대로 구성 되 면 하는 경우 응용 프로그램은 여러 실행 하는 경우 응용 프로그램의 간에 도구 모음 사용자 지정을 자동으로 유지 됩니다.

이 가이드의 다음 섹션에서는 만들고 Xcode의 인터페이스 작성기 도구 모음을 유지 관리 하는 방법 및 코드에서 사용 하는 방법에 설명 합니다.

## <a name="setting-a-custom-main-window-controller"></a>주 창 사용자 지정 컨트롤러 설정

UI 요소 콘센트 및 작업, Xamarin.Mac 응용 프로그램을 통해 C# 코드를 노출 하는 사용자 지정 창 컨트롤러를 사용 해야 합니다.

1. Xcode의 인터페이스 작성기에서 응용 프로그램의 스토리 보드를 엽니다.
2. 디자인 화면의 창 컨트롤러를 선택 합니다.
3. 전환 하는 **Identity 관리자** 으로 "WindowController"를 입력 하 고는 **클래스 이름**: 

    [![설정 창 컨트롤러에 대 한 사용자 지정 클래스 이름](toolbar-images/windowcontroller01.png "설정 창 컨트롤러에 대 한 사용자 지정 클래스 이름")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. 변경 내용을 저장 하 고 동기화 하는 Mac에 대 한 Visual Studio로 돌아갑니다.
5. A **WindowController.cs** 파일을 프로젝트에 추가 됩니다는 **솔루션 패드** Mac 용 Visual Studio에서: 

    ![솔루션 패드 WindowController.cs을 선택 하면](toolbar-images/windowcontroller02.png "WindowController.cs 솔루션 패드에서 선택")

6. Xcode의 인터페이스 작성기에서 스토리 보드를 다시 엽니다.
7. **WindowController.h** 파일은 사용 하기 위해 사용할 수 있습니다. 

    [![WindowController.h 파일](toolbar-images/windowcontroller03.png "The WindowController.h 파일")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>만들기 및 도구 모음 Xcode에서 유지 관리

도구 모음 생성 및 Xcode의 인터페이스 작성기와 함께 유지 됩니다. 응용 프로그램의 주 스토리 보드를 편집 도구 모음에 응용 프로그램을 추가 하려면 (이 경우 **Main.storyboard**)에서 두 번 클릭 하 여는 **솔루션 패드**:

![Main.storyboard 솔루션 패드에서 열면](toolbar-images/edit01.png "Main.storyboard 솔루션 패드에서 열기")

**라이브러리 관리자**에 "도구"를 입력는 **검색 상자** 쉽게 사용 가능한 도구 모음 항목을 모두 볼 수 있도록 합니다.

![라이브러리 관리자 도구 모음 항목을 표시 하도록 필터링](toolbar-images/edit02.png "도구 모음 항목을 표시 하도록 필터링 The 라이브러리 관리자")

창에는 도구 모음을 끌어는 **인터페이스 편집기**합니다. 선택한 도구 모음을 사용에서 속성을 설정 하 여 해당 동작을 구성할는 **특성 검사기**:

![도구 모음에 대 한 특성 검사기](toolbar-images/edit04.png "도구 모음에 대 한 The 특성 관리자")

사용할 수 있는 속성은 다음과 같습니다.

1. **디스플레이** -도구 모음 아이콘, 텍스트 또는 둘 다 표시 여부를 제어 합니다.
2. **시작 시 표시** -을 선택한 경우에 도구 모음에서 기본적으로 표시 됩니다.
3. **사용자 지정 가능한** -사용자가 편집 하 고 도구 모음을 사용자 지정할 수를 선택 합니다.
4. **구분 기호** -내용 창에서에서 도구 모음 씬 가로 줄으로 구분을 선택 합니다.
5. **크기** -도구 모음의 크기 설정
6. **자동 저장** -응용 프로그램 사용자의 도구 모음에 대 한 구성 변경 내용을 실행 하는 경우 응용 프로그램에서 계속 유지 됩니다을 선택 합니다.

선택 된 **자동 저장** 옵션 및 다른 모든 속성을 기본 설정으로 둡니다. 

도구 모음에서 연 후는 **인터페이스 계층 구조**, 도구 모음 항목을 선택 하 여 사용자 지정 대화 상자를 표시 합니다.

![도구 모음 사용자 지정](toolbar-images/edit05.png "도구 모음 사용자 지정")

이 대화 상자를 사용 하 여 디자인 된 응용 프로그램에 대 한 기본 도구 모음을 도구 모음에 이미 속해 있는 항목에 대 한 속성을 설정 하 고 사용자가 도구 모음을 사용자 지정할 때 선택할 수에 대 한 추가 도구 모음 항목을 제공 하기. 항목 도구 모음을 추가 하려면 끌어 오는 **라이브러리 관리자**:

![라이브러리 관리자](toolbar-images/edit06.png "라이브러리 관리자")

다음 도구 모음 항목을 추가할 수 있습니다.

- **도구 모음 항목을 이미지** -아이콘으로 사용자 지정 이미지를 사용 하는 도구 모음 항목입니다.
- **유연한 공간 도구 모음 항목** -유연한 공간 후속 도구 모음 항목을 발생 하는 데 사용 합니다. 예를 들어 유연한 공간 도구 모음 항목 앞에 오는 하나 이상의 도구 모음 항목 및 다른 도구 모음 항목 고정 도구 모음 오른쪽에 있는 마지막 항목입니다.
- **도구 모음 항목 공간** -도구 모음에 있는 항목 사이 공백을 고정
- **구분 기호 도구 모음 항목** -그룹화를 위해 두 개 이상의 도구 모음 항목 사이 구분 기호 표시
- **도구 모음 항목을 사용자 지정** -사용자가 도구 모음을 사용자 지정할 수 있도록
- **도구 모음 항목을 인쇄** -사용자가 열려 있는 문서를 인쇄 하도록 허용
- **색 도구 모음 항목 표시** -표준 시스템 색 선택을 표시: 

     ![시스템 색 선택](toolbar-images/edit07.png "시스템 색 선택")

- **글꼴 도구 모음 항목 표시** -표준 시스템 글꼴 대화 상자를 표시 합니다. 

     ![글꼴 선택 기가](toolbar-images/edit08.png "글꼴 선택기")

> [!IMPORTANT]
> 나중에 표시 됩니다, 검색 필드, 세그먼트 컨트롤과 가로 슬라이더와 같은 많은 표준 Cocoa UI 컨트롤 도구 모음에 추가할 수도 있습니다.

### <a name="adding-an-item-to-a-toolbar"></a>도구 모음에 항목 추가

도구 모음 항목을 추가 하려면 도구 모음에서을 선택는 **인터페이스 계층 구조** 사용자 지정 대화 상자를 표시를 일으키는 포함 된 항목 중 하나를 클릭 합니다. 새 항목을 끌어 다음으로 **라이브러리 검사기** 에 **도구 모음 항목 허용** 영역:

![도구 모음 사용자 지정 대화 상자에서 허용 하는 도구 모음 항목](toolbar-images/add01.png "도구 모음 사용자 지정 대화 상자에서 허용 된 도구 모음 항목")

새 항목을 기본 도구 모음의 일부 인지를 끌어는 **기본 도구 모음 항목** 영역: 

![도구 모음 항목을 끌어서 순서 바꾸기](toolbar-images/add02.png "끌어 도구 모음 항목을 다시 정렬")

기본 도구 모음 항목의 순서를 끌어 왼쪽 또는 오른쪽입니다.

를 사용 하 여는 **특성 검사기** 항목에 대 한 기본 속성을 설정 하려면:

![속성 검사자를 사용 하 여 도구 모음 항목을 사용자 지정](toolbar-images/add03.png "특성 관리자를 사용 하 여 도구 모음 항목을 사용자 지정")

사용할 수 있는 속성은 다음과 같습니다.

- **이미지 이름** -항목에 대 한 아이콘으로 사용할 이미지
- **레이블** -도구 모음에서 항목에 대해 표시할 텍스트
- **색상표 레이블** -에 있는 항목에 표시할 텍스트를는 **도구 모음 항목 허용** 영역
- **태그** -코드에서 항목을 식별할 수 있는 옵션, 고유 식별자입니다.
- **식별자** -정의 된 도구 모음 항목 유형입니다. 코드에서 도구 모음 항목을 선택 하는 사용자 지정 값을 사용할 수 있습니다.
- **선택 가능한** -항목 켜기/끄기 단추는 같은 역할을 선택 하는 경우 합니다.

> [!IMPORTANT]
> 항목을 추가 **도구 모음 항목 허용** 영역 하지만 사용자에 대 한 사용자 지정 옵션을 제공 하도록 기본 도구 모음 없습니다. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>도구 모음으로 다른 UI 컨트롤을 추가합니다.

와 같은 여러 가지 Cocoa UI 요소 필드를 검색 하 고 도구 모음에 컨트롤 세그먼트를 추가할 수도 있습니다.

이 실행 하려면 도구 모음에서 열고는 **인터페이스 계층 구조** 사용자 지정 대화 상자를 열려면 도구 모음 항목을 선택 합니다. 끌어서는 **검색 필드** 에서 **라이브러리 검사기** 에 **도구 모음 항목 허용** 영역:

![도구 모음 사용자 지정 대화 상자를 사용 하 여](toolbar-images/add05.png "도구 모음 사용자 지정 대화 상자를 사용 하 여")

여기에서 인터페이스 작성기를 사용 하 여 검색 필드를 구성 하 고 작업 또는 콘센트 통해 코드에 노출 합니다.

## <a name="built-in-toolbar-item-support"></a>기본 제공 도구 모음 항목을 지 원하

여러 Cocoa UI 요소는 기본적으로 표준 도구 모음의 항목과 상호 작용 합니다. 예를 들어는 **텍스트 보기** 응용 프로그램의 창으로 끌고 콘텐츠 영역을 채우는 배치:

[![응용 프로그램에 텍스트 뷰 추가](toolbar-images/edit09.png "텍스트 보기 응용 프로그램에 추가")](toolbar-images/edit09-large.png#lightbox)

문서를 반환 하 여 Xcode와 동기화, 응용 프로그램을 실행, 텍스트를 입력, 선택, 클릭 Mac 용 Visual Studio로 저장 된 **색** 도구 모음 항목입니다. 텍스트 보기 색 선택 자동으로 작동 하는지 확인 합니다.

![텍스트 보기 및 색상 선택기를 사용 하는 기본 제공 도구 모음 기능](toolbar-images/edit10.png "텍스트 보기 및 색상 선택기를 사용 하는 기본 제공 도구 모음 기능")

## <a name="using-images-with-toolbar-items"></a>이미지를 사용 하 여 도구 모음 항목

사용 하는 **이미지 도구 모음 항목**, 모든 비트맵 이미지에 추가 **리소스** 폴더 (의 빌드 작업을 지정 하 고 **번들 리소스**)를 아이콘으로 도구 모음에 표시 될 수:

1. Mac에 대 한 Visual Studio에서의 **솔루션 패드**를 마우스 오른쪽 단추로 클릭는 **리소스** 폴더를 선택 **추가** > **파일 추가** .
2. **파일 추가** 대화 상자 원하는 이미지로 이동, 선택한 클릭는 **열려** 단추: 

    [![이미지를 추가 하려면 선택](toolbar-images/edit11.png "이미지를 추가 하려면 선택")](toolbar-images/edit11-large.png#lightbox)

3. 선택 **복사**, 확인 **동일한 작업을 사용 하 여 선택한 모든 파일에 대 한**를 클릭 하 고 **확인**:

    ![추가 된 이미지에 대 한 복사 작업을 선택 하면](toolbar-images/edit12.png "추가 된 이미지에 대 한 복사 작업을 선택 하면")

4. 에 **솔루션 패드**를 두 번 클릭 **MainWindow.xib** Xcode에서 엽니다.

5. 도구 모음에서 선택 된 **인터페이스 계층 구조** 사용자 지정 대화 상자를 열려면 해당 항목 중 하나를 클릭 합니다.

6. 끌어서는 **이미지 도구 모음 항목** 에서 **라이브러리 검사기** 의 도구 모음 **도구 모음 항목 허용** 영역: 

    ![도구 모음 항목 허용 된 영역에 추가 이미지 도구 모음 항목](toolbar-images/edit14.png "이미지 도구 모음 항목 도구 모음 항목 허용 된 영역에 추가")

7. 에 **특성 검사기**, Mac 용 Visual Studio에서 방금 추가 된 이미지 선택: 

    ![도구 모음 항목에 대 한 사용자 지정 이미지 설정](toolbar-images/edit15.png "도구 모음 항목에 대 한 사용자 지정 이미지를 설정 합니다.")

8. 설정의 **레이블** "휴지통"으로 및 **색상표 레이블** "문서를 지우지": 

    ![레이블 및 색상표 레이블 항목 도구 모음 설정](toolbar-images/edit16.png "레이블 및 색상표 레이블 항목 도구 모음 설정")

9. 끌어서는 **구분 기호 도구 모음 항목** 에서 **라이브러리 검사기** 의 도구 모음 **도구 모음 항목 허용** 영역: 

    [![도구 모음 항목 허용 된 영역에 추가 구분 기호 도구 모음 항목](toolbar-images/edit17.png "A 구분 기호 도구 모음 항목 도구 모음 항목 허용 된 영역에 추가")](toolbar-images/edit17-large.png#lightbox)

10. 구분 기호 항목 및 "휴지통" 항목을 끌어는 **기본 도구 모음 항목** 영역 및 집합에서 항목을 도구 모음 순서 왼쪽에서 오른쪽 (색, 글꼴, 구분 기호, 휴지통, 유연한 공간, 인쇄)를 다음과 같이 합니다. 

    ![기본 도구 모음 항목](toolbar-images/edit18.png "기본 도구 모음 항목")

11. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

기본적으로 새 도구 모음이 표시 되는지 확인 하려면 응용 프로그램을 실행 합니다.

![사용자 지정 된 기본 항목이 있는 도구 모음](toolbar-images/edit19.png "사용자 지정 된 기본 항목이 있는 도구 모음")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>도구 모음 항목 콘센트 및 동작을 노출

도구 모음 또는 코드에서 도구 모음 항목에 액세스 하려면 콘센트 또는 작업에 연결 해야 합니다.

1. 에 **솔루션 패드**를 두 번 클릭 **Main.storyboard** Xcode에서 엽니다.
2. "WindowController"의 주 창 컨트롤러에 할당 된 사용자 지정 클래스를 확인 하 고 **Identity 관리자**:

    [![창 컨트롤러에 대 한 사용자 지정 클래스를 설정 하 여 Identity 관리자를 사용 하 여](toolbar-images/edit20a.png "Identity 관리자를 사용 하 여 창 컨트롤러에 대 한 사용자 지정 클래스를 설정 하려면")](toolbar-images/edit20a-large.png#lightbox)

3. 도구 모음 항목을 다음으로, 선택는 **인터페이스 계층 구조**: 

    ![인터페이스 계층 구조에서 도구 모음 항목을 선택 하면](toolbar-images/edit20.png "인터페이스 계층 구조에서 도구 모음 항목을 선택 합니다.")  

4. 열기는 **도우미 보기**, 선택는 **WindowController.h** 파일 및 도구 모음 항목의 컨트롤 끌기를 **WindowController.h** 파일입니다.
5. 설정는 **연결** 형식을 **동작**, "trashDocument"에 대 한 입력는 **이름**를 클릭 하 고는 **연결** 단추: 

    [![도구 모음 항목에 대 한 작업 구성](toolbar-images/edit23.png "도구 모음 항목에 대 한 작업 구성")](toolbar-images/edit23-large.png#lightbox)

6. 노출 된 **텍스트 보기** 콘센트에서 "documentEditor" 호출로 **ViewController.h** 파일: 

    [![텍스트 보기에 대 한 콘센트 구성](toolbar-images/edit24.png "콘센트 텍스트 보기에 대 한 구성")](toolbar-images/edit24-large.png#lightbox)

7. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

Mac 용 Visual Studio에서 편집 된 **ViewController.cs** 파일을 다음 코드를 추가 합니다.

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

다음에 편집는 **WindowController.cs** 파일 맨 아래에 다음 코드를 추가 하는 `WindowController` 클래스:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

응용 프로그램을 실행 하는 경우는 **휴지통** 도구 모음 항목이 활성화 됩니다.

![활성 휴지통 항목 도구 모음](toolbar-images/edit25.png "활성 휴지통 항목 도구 모음")

에 **휴지통** 텍스트를 삭제 하려면 도구 모음 항목 이제 사용할 수 있습니다.

## <a name="disabling-toolbar-items"></a>도구 모음 항목 사용 안 함

도구 모음에서 항목을 사용 하지 않으려면 사용자 지정 만들기 `NSToolbarItem` 클래스 및 재정의 `Validate` 메서드. 그런 다음 인터페이스 작성기에서 사용자 지정 형식을 설정/해제 하려는 항목에 할당 합니다.

사용자 지정 만들어 `NSToolbarItem` 클래스 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 파일...** . 선택 **일반** > **빈 클래스**, "ActivatableItem"에 대 한 입력의 **이름**를 클릭 하 고는 **새로** 단추: 

![Mac 용 Visual Studio에서 빈 클래스 추가](toolbar-images/custom01.png "Mac 용 Visual Studio에서 빈 클래스 추가")

다음에 편집는 **ActivatableItem.cs** 다음과 같이 읽을 파일입니다.

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

두 번 클릭 **Main.storyboard** Xcode에서 엽니다. 선택 된 **휴지통** 도구 모음 항목 위에서 만든 하 고 해당 클래스로 "ActivatableItem"에 **Identity 관리자**:

![도구 모음 항목에 대 한 사용자 지정 클래스 설정](toolbar-images/custom02.png "도구 모음 항목에 대 한 사용자 지정 클래스를 설정 합니다.")

호출 콘센트 만들기 `trashItem` 에 대 한는 **휴지통** 도구 모음 항목입니다. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다. 마지막으로, 열 **MainWindow.cs** 하 고 업데이트는 `AwakeFromNib` 메서드를 다음과 같이 합니다.

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

응용 프로그램을 실행 하 고는 **휴지통** 항목 도구 모음에서 이제 비활성화 됩니다.

![비활성 휴지통 항목 도구 모음](toolbar-images/custom03.png "비활성 휴지통 항목 도구 모음")

## <a name="summary"></a>요약

이 문서에는 도구 모음 및 도구 모음 항목 Xamarin.Mac 응용 프로그램에서 작업에 대해 자세히를 수행 했습니다. 만들고 Xcode의 인터페이스 작성기의 도구 모음을 유지 관리 하는 방법, 일부 UI 컨트롤 도구 모음 항목 함께 자동으로 작동 방법, C# 코드에서는 도구 모음을 사용 하는 방법 및 도구 모음 항목을 사용 하지 않도록 설정 하는 방법을 설명 합니다.


## <a name="related-links"></a>관련 링크

- [MacToolbar (샘플)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [도구 모음에 대 한 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [도구 모음에는 소개](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
