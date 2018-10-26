---
title: 빠른 시작-Xamarin.Mac의 스토리 보드
description: 이 문서에서는 macOS Xamarin.Mac의 스토리 보드를 사용 하 여 사용자 인터페이스를 작성에 대 한 빠른 시작 소개를 제공 합니다. Segue를 만들고 기본 설정 창 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: 7f7d23a01a3c3c6567d6bab45d0abbfb078fb512
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112574"
---
# <a name="storyboards-in-xamarinmac-quick-start"></a>빠른 시작-Xamarin.Mac의 스토리 보드

스토리 보드를 사용 하 여 Xamarin.Mac 앱의 사용자 인터페이스를 정의 하려면 빠른 소개의 경우와 새 Xamarin.Mac 프로젝트를 시작 해 보겠습니다. **Mac** > **앱** > **Cocoa 앱**을 선택하고 **다음** 단추를 클릭합니다.

[![](quickstart-images/qs01.png "새 Cocoa 앱 추가")](quickstart-images/qs01.png#lightbox)

사용 합니다 **앱 이름** 의 `MacStoryboard` 을 클릭 합니다 **다음** 단추:

[![](quickstart-images/qs02.png "앱 이름 설정")](quickstart-images/qs02.png#lightbox)

기본값을 사용 하 여 **프로젝트 이름** 및 **솔루션 이름** 을 클릭 합니다 **만들기** 단추:

[![](quickstart-images/qs03.png "프로젝트 및 솔루션 이름")](quickstart-images/qs03.png#lightbox)

에 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` Xcode의 Interface Builder에서 편집 하기 위해 열려는 파일:

[![](quickstart-images/qs04.png "Xcode에서 스토리 보드를 편집합니다.")](quickstart-images/qs04.png#lightbox)

위에 보이는 것 처럼 기본 스토리 보드 모두 앱의 메뉴 모음 및 정의 된 주 창 뷰 컨트롤러 및 보기. 샘플 앱에 로컬인 것 주에는 UI를 만들 _콘텐츠 보기_ 한쪽 및 _검사기 보기_ 1 초에서입니다.

이 작업을 수행 하려면 먼저 기본 뷰 컨트롤러를 제거 해야는 및에서 스토리 보드를 사용 하 여 제공 되는 보기에서 Interface Builder 및 키를 눌러 선택 합니다 **삭제** 키:

[![](quickstart-images/qs05.png "기본 뷰 컨트롤러를 제거합니다.")](quickstart-images/qs05.png#lightbox)

그런 다음 입력 `split` 에 **필터** 영역에서 세로 분할 뷰 컨트롤러를 선택 하 고 끕니다 합니다 _디자인 화면_:

[![](quickstart-images/qs06.png "분할 뷰 컨트롤러에 대 한 검색")](quickstart-images/qs06.png#lightbox)

컨트롤러에 자동으로 두 명의 자식 뷰 컨트롤러 (및 해당 관련된 뷰), 했으니 분할 보기의 왼쪽과 오른쪽을 포함 하는 알 수 있습니다. 키를 눌러 해당 부모 창에 나누기 보기 연결할 합니다 **컨트롤** 키 창 컨트롤러 (창 컨트롤러의 프레임에 파란색 원)를 클릭 하 고 분할 보기 컨트롤러에 선을 끕니다. 선택 **창 내용이** 팝업에서:

[![](quickstart-images/qs07.png "Windows 콘텐츠 보기 설정")](quickstart-images/qs07.png#lightbox)

그러면 Segue를 사용 하 여 두 가지 인터페이스 요소에 연결:

[![](quickstart-images/qs08.png "창 사이의 Segue")](quickstart-images/qs08.png#lightbox)

분할 뷰의 왼쪽에 텍스트 뷰를 배치 하 고 창 또는 분할 뷰 크기를 조정할 때 사용할 수 있는 영역을 자동으로 입력 되 게 하려고 합니다. 분할 뷰에 연결 된 뷰 컨트롤러 맨 위에 텍스트 뷰를 끌어서 클릭 합니다 **Pin** 자동 레이아웃 제약 조건 (디자인 화면의 맨 아래에서 오른쪽에서 두 번째 아이콘).

[![](quickstart-images/qs09.png "제약 조건 구성")](quickstart-images/qs09.png#lightbox)

여기에서 네 가지 모두를 클릭 합니다는 **i-빔** 경계 제약 조건 팝 오버의 맨 위에 있는 상자 아이콘과 클릭 합니다 **4 개 제약 조건 추가** 필요한 제약 조건을 추가 하려면 아래쪽 단추입니다.

Mac 용 Visual Studio로 돌아가서 고 프로젝트를 실행 하는 경우 텍스트 보기 창 또는 분할으로 분할 뷰의 왼쪽에 맞게 자동으로 조정 하는 크기가 조정 됩니다.

[![](quickstart-images/qs10.png "실행 중인 앱의 예")](quickstart-images/qs10.png#lightbox)

검사기 영역으로 분할 보기의 오른쪽을 사용할 것 이므로 원하는 작은 크기를 축소할 수 있도록 합니다. Xcode로 돌아가서 디자인 화면에서 선택 하 고 클릭 하 여 오른쪽에 대 한 보기를 편집 합니다 **크기 검사기**합니다. 여기에서 입력을 **너비** 의 `250`:

[![](quickstart-images/qs11.png "너비를 설정합니다.")](quickstart-images/qs11.png#lightbox)

다음 선택 오른쪽을 나타내는 분할 항목을 높게 설정 되어 있으면 **보유 우선 순위** 을 클릭 합니다 **사용자 수 축소** 확인란을 선택:

[![](quickstart-images/qs12.png "보관 우선 순위를 편집합니다.")](quickstart-images/qs12.png#lightbox)

Mac 용 Visual Studio로 반환 하 고 프로젝트를 지금 실행 오른쪽 유지 하는 더 작은 크기와 창 크기가 조정 됩니다.

[![](quickstart-images/qs13.png "실행 중인 앱의 예")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>Segue는 프레젠테이션을 정의

예정 레이아웃에 선택한 텍스트의 속성에 대해 검사기 역할을 분할 보기의 오른쪽입니다. 레이아웃 관리자의 UI 일부 컨트롤 아래쪽 보기 끌어 됩니다 했습니다. 마지막으로 컨트롤을 팝 오버에서 네 가지 기본 설정된 문자 스타일을 선택할 수 있는 표시 하려고 합니다.

단추 디자인 화면에 뷰 컨트롤러 검사기를 추가 합니다. 뷰 컨트롤러의 크기를 조정 하 여 네 개의 단추를 추가 하 여 팝 오버 한다고 합니다. 에서는 다음 **제어** 우리의 팝 오버 나타내는 보기 컨트롤러를 끌어서 키-검사기 보기에서 단추를 클릭 합니다.

[![](quickstart-images/qs14.png "끌어서 새 segue를 만들기")](quickstart-images/qs14.png#lightbox)

팝업 메뉴에서 선택 **팝 오버**: 

[![](quickstart-images/qs15.png "Segue 형식 선택")](quickstart-images/qs15.png#lightbox)

디자인 화면에서 Segue를 선택 하 고 설정 됩니다 마지막으로 **지 원하는** 하 **왼쪽**합니다. 줄을 다음 끌면 됩니다 합니다 **앵커 보기** 에 연결할 팝 오버 우리가 원하는 단추:

[![](quickstart-images/qs16.png "끌어서 새 segue를 만들기")](quickstart-images/qs16.png#lightbox)

Visual studio for Mac을 반환 하는 경우 앱을 실행 하 고 클릭 합니다 **None** 관리자 팝 오버에서에서 단추가 표시 됩니다.

[![](quickstart-images/qs17.png "실행 segue의 예")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>앱 기본 설정 만들기

대부분의 표준 macOS 앱 제공을 _기본 설정 대화 상자_ 사용자 모양이 나 사용자 계정과 같은 앱의 다양 한 측면을 제어 하는 몇 가지 옵션을 정의할 수 있도록 합니다.

표준 기본 설정 대화 상자 창이 정의 하려면 먼저 디자인 화면으로 탭 뷰 컨트롤러를 끌어옵니다.

[![](quickstart-images/qs18.png "Xcode에서 스토리 보드를 편집합니다.")](quickstart-images/qs18.png#lightbox)

다시 두 자식 뷰 컨트롤러 연결을 사용 하 여이 제공 하는 것은 자동으로. 예를 들어 드리지만 추가한 레이블을 내부 center는 각 보기로:

[![](quickstart-images/qs19.png "제약 조건 설정")](quickstart-images/qs19.png#lightbox)

사용자가 선택 하는 경우 기본 설정 창 표시 하고자 하는 다음으로 **기본 설정...**  메뉴 항목입니다. 메뉴 모음에서 기본 설정 메뉴 항목을 선택 **제어** 키 클릭 하 고 선 탭 뷰 컨트롤러를 드래그 합니다.

[![](quickstart-images/qs20.png "끌어서 segue를 만들기")](quickstart-images/qs20.png#lightbox)

팝업에서 선별해 **모달** 모달 대화 상자로이 창을 표시 하려면:

[![](quickstart-images/qs21.png "Segue 형식 선택")](quickstart-images/qs21.png#lightbox)

Mac 용 Visual Studio로 돌아가서이 변경 사항을 저장 하는 경우 앱을 실행 하 고 선택 된 **기본 설정...**  메뉴 항목을이 새 기본 설정 대화 상자가 표시 됩니다.

[![](quickstart-images/qs22.png "실행 segue의 예")](quickstart-images/qs22.png#lightbox)

표준 macOS 앱을 기본 설정 대화 상자 창 처럼 보이지는 확인할 수 있습니다. 이 해결 하려면 Xamarin.Mac 앱의 두 가지 이미지 파일을 포함 `Resources` 폴더에는 **솔루션 탐색기** Xcode의 Interface Builder를 반환 합니다.

탭 뷰 컨트롤러 및 스위치를 선택 합니다. 해당 **스타일** 하 **도구 모음**: 

[![](quickstart-images/qs23.png "탭 표시줄 스타일 설정")](quickstart-images/qs23.png#lightbox)

각 탭을 선택 하 고 지정 된 **레이블** 를 나타내는 이미지 중 하나를 선택 하 고:

[![](quickstart-images/qs24.png "Xcode에서 각 탭을 구성합니다.")](quickstart-images/qs24.png#lightbox)

Mac 용 Visual Studio로 돌아가서이 변경 사항을 저장 하는 경우 앱을 실행 하 고 선택 된 **기본 설정...**  메뉴 항목 대화 상자는 표준 macOS 앱 처럼 표시 됩니다.

[![](quickstart-images/qs25.png "실행 중인 기본 창의 예")](quickstart-images/qs25.png#lightbox)

자세한 내용은 참조 하십시오 우리의 [이미지를 사용 하 여 작업](~/mac/app-fundamentals/image.md), [메뉴](~/mac/user-interface/menu.md)를 [Windows](~/mac/user-interface/window.md) 및 [대화 상자](~/mac/user-interface/dialog.md) 설명서.

## <a name="related-links"></a>관련 링크

- [MacStoryboard (샘플)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
