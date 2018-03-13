---
title: "스토리 보드 빠른 시작"
description: "스토리 보드와 사용자 인터페이스 시작된 건물 macOS를 가져오는 중입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: fe3a93557509aba4b33b1470879cd2504ed0f2a2
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="starting-a-new-storyboard-based-project"></a>프로젝트를 기반으로 새 스토리 보드를 시작 합니다.

스토리 보드를 사용 하 여 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 정의 하려면 간략 한 소개를으로 새 Xamarin.Mac 프로젝트를 시작 하겠습니다. **Mac** > **앱** > **Cocoa 앱**을 선택하고 **다음** 단추를 클릭합니다.

[![](quickstart-images/qs01.png "새 Cocoa 앱 추가")](quickstart-images/qs01.png#lightbox)

사용 하 여는 **응용 프로그램 이름** 의 `MacStoryboard` 클릭는 **다음** 단추:

[![](quickstart-images/qs02.png "응용 프로그램 이름 설정")](quickstart-images/qs02.png#lightbox)

기본값을 사용 하 여 **프로젝트 이름** 및 **솔루션 이름** 클릭는 **만들기** 단추:

[![](quickstart-images/qs03.png "프로젝트 및 솔루션 이름")](quickstart-images/qs03.png#lightbox)

에 **솔루션 탐색기**, 두 번 클릭는 `Main.storyboard` Xcode의 인터페이스 작성기에서 편집을 위해 열 파일입니다.

[![](quickstart-images/qs04.png "Xcode에서 스토리 보드를 편집합니다.")](quickstart-images/qs04.png#lightbox)

위에서 볼 수 있듯이 기본 스토리 보드를 정의 응용 프로그램의 메뉴 모음과 함께 주 창을 모두 보기 컨트롤러와 뷰 합니다. 이 샘플 응용 프로그램에 대 한 것 이며 main이 작성 된 UI를 만드는 수 _콘텐츠 뷰_ 한쪽에 및 _관리자 보기_ 두 번째에서입니다.

하 여 스토리 보드와 함께 제공 되는 보기에서 작성기 인터페이스 및 키를 눌러 선택 하 고이 수행 하려면 먼저 기본 뷰-컨트롤러를 제거 해야 합니다는 **삭제** 키:

[![](quickstart-images/qs05.png "기본 보기 컨트롤러 제거 중")](quickstart-images/qs05.png#lightbox)

그런 다음 입력 `split` 에 **필터** 영역을 수직 분할 뷰 컨트롤러를 선택 하 고 끕니다는 _디자인 화면_:

[![](quickstart-images/qs06.png "분할 뷰 컨트롤러에 대 한 검색")](quickstart-images/qs06.png#lightbox)

컨트롤러에 자동으로 두 명의 자식 뷰 컨트롤러 (및 해당 관련 된 뷰) 유선 접속 분할 보기의 왼쪽과 오른쪽에 포함을 확인 합니다. 키를 눌러 부모 창으로 분할 뷰를 연결 하는 **제어** 키, 창 컨트롤러 (창 컨트롤러의 프레임에 파란색 원) 클릭 하 고 분할 뷰 컨트롤러 선을 끌 합니다. 선택 **창 내용** 팝업에서:

[![](quickstart-images/qs07.png "콘텐츠 뷰의 windows 설정")](quickstart-images/qs07.png#lightbox)

그러면 두 인터페이스 요소는 Segue를 사용 하 여 함께 연결 됩니다.

[![](quickstart-images/qs08.png "창와 내용 사이의 Segue")](quickstart-images/qs08.png#lightbox)

텍스트 보기 분할 보기의 왼쪽에 배치 하 고 창 또는 분할 보기 크기를 조정 하면 자동으로 사용할 수 있는 영역을 채우도록 하 게 하려고 합니다. 분할 뷰에 연결 된 뷰-컨트롤러 맨 위에 텍스트 보기를 끌어서 클릭는 **Pin** 자동 레이아웃 제약 조건 (디자인 화면 맨 아래에 오른쪽에서 두 번째 아이콘).

[![](quickstart-images/qs09.png "제약 조건을 구성")](quickstart-images/qs09.png#lightbox)

여기에서 네 가지 모두를 클릭 합니다는 **i-빔** 경계 제약 조건을 Popover 맨 위에 있는 상자 아이콘과 클릭는 **4 제약 조건 추가** 필요한 제약 조건을 추가 하려면 아래쪽 단추입니다.

Mac 용 Visual Studio로 되돌아가려면에서는 프로젝트를 실행 하는 경우 텍스트 보기 창 또는 분할으로 분할 보기의 왼쪽에 맞게 자동으로 크기가 조정 되는 공지 크기가 조정 됩니다.

[![](quickstart-images/qs10.png "실행 중인 앱의 예")](quickstart-images/qs10.png#lightbox)

검사기 영역으로 분할 보기의 오른쪽을 사용할 것 이므로 원하는 위치로 더 작은 크기를 축소할 수 있도록 허용 합니다. Xcode를 반환 하 고 디자인 화면에서 선택 하 고를 클릭 하 여 오른쪽에 대 한 보기를 편집할는 **크기 검사기**합니다. 여기에서 입력 한 **너비** 의 `250`:

[![](quickstart-images/qs11.png "너비 설정")](quickstart-images/qs11.png#lightbox)

다음 select는 오른쪽을 나타내는 분할 항목 더 높은 설정 **우선 순위를 보유** 클릭는 **사용자 수 축소** 확인란:

[![](quickstart-images/qs12.png "지주 우선 순위를 편집합니다.")](quickstart-images/qs12.png#lightbox)

Mac 용 Visual Studio로 반환 하 고 프로젝트를 지금 실행 오른쪽 유지 하는 더 작은 크기와 창 크기가 조정 됩니다.

[![](quickstart-images/qs13.png "실행 중인 앱의 예")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>Segue 프레젠테이션을 정의

선택한 텍스트의 속성에 대 한 관리자 역할을 분할 보기의 오른쪽 레이아웃 것 이며 합니다. 레이아웃 관리자의 UI에 일부 컨트롤 아래쪽 뷰로 끌면 합니다. 마지막 컨트롤에 대 한 사용자에서 4 개의 미리 설정 된 문자 스타일을 선택할 수 있는 popover를 표시 하려고 합니다.

단추 검사기와 디자인 화면에 뷰 컨트롤러를 추가 합니다. 크기로 뷰 컨트롤러 크기가 조정 됩니다 우리의 Popover 하 고 네 개의 단추를 추가 한다고 합니다. त ु म च 다음 **제어** 우리의 popover를 나타내는 보기 컨트롤러에 끌어서 키-검사기 보기에서 단추를 클릭 합니다.

[![](quickstart-images/qs14.png "드래그 하 여 새 segue 만들기")](quickstart-images/qs14.png#lightbox)

팝업 메뉴에서 선택 **Popover**: 

[![](quickstart-images/qs15.png "Segue 유형 선택")](quickstart-images/qs15.png#lightbox)

Segue 디자인 화면에서 선택 하 고 설정 합니다 마지막으로 **기본 가장자리** 를 **왼쪽**합니다. 그런 다음 줄을 끌면 됩니다는 **앵커 보기** 을 아래쪽에 연결 될 popover 원하는:

[![](quickstart-images/qs16.png "드래그 하 여 새 segue 만들기")](quickstart-images/qs16.png#lightbox)

Mac 용 Visual Studio 돌아가면 응용 프로그램을 실행 하 고 클릭는 **None** popover 검사기에 단추가 표시 됩니다.

[![](quickstart-images/qs17.png "실행 중인 segue의 예")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>기본 응용 프로그램 설정 만들기

대부분의 표준 macOS 앱이 제공 하는 _기본 설정 대화 상자_ 사용자 응용 프로그램, 모양 또는 사용자 계정 등의 다양 한 측면을 제어 하는 몇 가지 옵션을 정의할 수 있도록 합니다.

표준 기본 설정 대화 상자 창 정의 하려면 먼저 디자인 화면으로 탭 뷰 컨트롤러를 끌어옵니다.

[![](quickstart-images/qs18.png "Xcode에서 스토리 보드를 편집합니다.")](quickstart-images/qs18.png#lightbox)

다시, 컨트롤러 보기에 연결 된 두 명의 자식와 함께 자동으로 제공 됩니다이 있습니다. 예를 들어에서는 추가 하겠습니다 레이블을 안에 가운데 맞춤 됩니다 하는 각 보기에:

[![](quickstart-images/qs19.png "제약 조건 설정")](quickstart-images/qs19.png#lightbox)

사용자가을 선택할 때 기본 설정 창 표시 하고자 하는 다음으로 **기본 설정 중...**  메뉴 항목입니다. 메뉴 모음에서 기본 설정 메뉴 항목을 선택한 **제어** 우리의 탭 뷰-컨트롤러에 선을 끌어 마우스 클릭 키:

[![](quickstart-images/qs20.png "드래그 하 여 한 segue 만들기")](quickstart-images/qs20.png#lightbox)

선택 된 팝업 화면에서 **모달** 모달 대화 상자로이 창을 표시 하려면:

[![](quickstart-images/qs21.png "Segue 유형 선택")](quickstart-images/qs21.png#lightbox)

Mac 용 Visual Studio로 돌아가서이 변경 사항을 저장 하는 경우 응용 프로그램을 실행 하 고 선택 된 **기본 설정 중...**  메뉴 항목을 통해 새 기본 설정 대화 상자가 표시 됩니다.

[![](quickstart-images/qs22.png "실행 중인 segue의 예")](quickstart-images/qs22.png#lightbox)

표준 macOS 응용 프로그램 기본 설정 대화 상자 창 ो द ि 있는지 확인할 수 있습니다. Xamarin.Mac 응용 프로그램에서이 해결 하려면 두 이미지 파일을 포함 `Resources` 폴더에는 **솔루션 탐색기** Xcode의 인터페이스 작성기 돌아갑니다.

탭 뷰-컨트롤러 및 스위치를 선택 합니다. 해당 **스타일** 를 **도구 모음**: 

[![](quickstart-images/qs23.png "탭 모음 스타일 설정")](quickstart-images/qs23.png#lightbox)

각 탭을 선택 하 고는 **레이블** 를 나타내는 이미지 중 하나를 선택 합니다.

[![](quickstart-images/qs24.png "각 탭 Xcode에서 구성")](quickstart-images/qs24.png#lightbox)

Mac 용 Visual Studio로 돌아가서이 변경 사항을 저장 하는 경우 응용 프로그램을 실행 하 고 선택 된 **기본 설정 중...**  메뉴 항목 대화 상자 표준 macOS 앱 처럼 표시 됩니다.

[![](quickstart-images/qs25.png "실행 중인 기본 설정 창의 예")](quickstart-images/qs25.png#lightbox)

자세한 내용은 참조 하십시오 우리의 [이미지 작업](~/mac/app-fundamentals/image.md), [메뉴](~/mac/user-interface/menu.md), [Windows](~/mac/user-interface/window.md) 및 [대화 상자의](~/mac/user-interface/dialog.md) 설명서입니다.

## <a name="related-links"></a>관련 링크

- [MacStoryboard (샘플)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [창 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
