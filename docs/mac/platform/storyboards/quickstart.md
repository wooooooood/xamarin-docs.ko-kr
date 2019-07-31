---
title: Xamarin.ios의 storyboard – 빠른 시작
description: 이 문서에서는 Xamarin.ios에서 storyboard를 사용 하 여 macOS 사용자 인터페이스를 작성 하는 빠른 시작 소개를 제공 합니다. Segue을 만들고 기본 설정 창을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 20719B5D-8147-4E8A-A23C-8D575C7ACCEE
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: b93cc584a58d864e6dc7477dc7f76d4f59844d48
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652402"
---
# <a name="storyboards-in-xamarinmac-quick-start"></a>Xamarin.ios의 storyboard – 빠른 시작

Storyboard를 사용 하 여 Xamarin.ios 앱의 사용자 인터페이스를 정의 하는 방법에 대 한 간략 한 소개로, 새 Xamarin.ios 프로젝트를 시작 하겠습니다. **Mac** > **앱** > **Cocoa 앱**을 선택하고 **다음** 단추를 클릭합니다.

[![](quickstart-images/qs01.png "새 Cocoa 앱 추가")](quickstart-images/qs01.png#lightbox)

**앱 이름을** `MacStoryboard` 사용 하 고 **다음** 단추를 클릭 합니다.

[![](quickstart-images/qs02.png "앱 이름 설정")](quickstart-images/qs02.png#lightbox)

기본 **프로젝트 이름** 및 **솔루션 이름** 을 사용 하 고 **만들기** 단추를 클릭 합니다.

[![](quickstart-images/qs03.png "프로젝트 및 솔루션 이름")](quickstart-images/qs03.png#lightbox)

**솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 Xcode의 Interface Builder에서 편집할 수 있도록 엽니다.

[![](quickstart-images/qs04.png "Xcode에서 storyboard 편집")](quickstart-images/qs04.png#lightbox)

위에서 볼 수 있듯이 기본 Storyboard는 응용 프로그램의 메뉴 모음과 주 창 (보기 컨트롤러 및 보기)을 모두 정의 합니다. 샘플 앱의 경우 한 쪽에는 주 _콘텐츠 보기가_ 있고 두 번째는 _검사기 보기_ 를 포함 하는 UI를 만들 예정입니다.

이렇게 하려면 Interface Builder에서 선택한 다음 **Delete** 키를 눌러 Storyboard와 함께 제공 되는 기본 뷰 컨트롤러 및 뷰를 먼저 제거 해야 합니다.

[![](quickstart-images/qs05.png "기본 뷰 컨트롤러 제거")](quickstart-images/qs05.png#lightbox)

그런 다음 `split` **필터** 영역에를 입력 하 고, 수직 분할 뷰 컨트롤러를 선택 하 여 _Design Surface_로 끕니다.

[![](quickstart-images/qs06.png "분할 뷰 컨트롤러를 검색 하는 중")](quickstart-images/qs06.png#lightbox)

컨트롤러에는 분할 뷰의 왼쪽 및 오른쪽에 두 개의 자식 뷰 컨트롤러 (및 관련 뷰)가 자동으로 포함 되어 있습니다. 분할 뷰를 부모 창에 연결 하려면 **컨트롤** 키를 누르고 창 컨트롤러 (창 컨트롤러의 프레임에 있는 파란색 원)를 클릭 한 다음 분할 보기 컨트롤러로 줄을 끕니다. 팝업에서 **창 콘텐츠** 를 선택 합니다.

[![](quickstart-images/qs07.png "Windows 콘텐츠 뷰 설정")](quickstart-images/qs07.png#lightbox)

이렇게 하면 Segue를 사용 하 여 두 인터페이스 요소를 함께 연결 합니다.

[![](quickstart-images/qs08.png "창과 내용 간의 Segue")](quickstart-images/qs08.png#lightbox)

분할 뷰의 왼쪽에 텍스트 뷰를 놓고 창이 나 분할 뷰의 크기가 조정 될 때 사용 가능한 영역을 자동으로 채우도록 합니다. 분할 뷰에 연결 된 최상위 뷰 컨트롤러로 텍스트 뷰를 끌고 **고정** 자동 레이아웃 제약 조건 (Design Surface 아래쪽의 오른쪽에 있는 두 번째 아이콘)을 클릭 합니다.

[![](quickstart-images/qs09.png "제약 조건 구성")](quickstart-images/qs09.png#lightbox)

여기에서 팝 오버 제약 조건 위쪽의 경계 상자 주위에 있는 네 개의 **I 빔** 아이콘을 클릭 하 고 맨 아래에 있는 4 개의 **제약 조건 추가** 단추를 클릭 하 여 필수 제약 조건을 추가 합니다.

Mac용 Visual Studio로 돌아가서 프로젝트를 실행 하는 경우 창 또는 분할 크기가 조정 될 때 텍스트 뷰의 크기가 자동으로 조정 되어 분할 뷰의 왼쪽에 채워집니다.

[![](quickstart-images/qs10.png "실행 중인 앱의 예")](quickstart-images/qs10.png#lightbox)

분할 보기의 오른쪽을 Inspector 영역으로 사용 하기 때문에 크기가 작고 축소 되도록 하는 것이 좋습니다. Xcode로 돌아가서 Design Surface에서 선택한 다음 **크기 검사기**를 클릭 하 여 오른쪽에 대 한 보기를 편집 합니다. 여기에서 **너비** `250`를 입력 합니다.

[![](quickstart-images/qs11.png "너비 설정")](quickstart-images/qs11.png#lightbox)

그런 다음 오른쪽을 나타내는 분할 항목을 선택 하 고, 더 높은 **우선 순위** 를 설정 하 고, **사용자가 축소할 수 있음** 확인란을 클릭 합니다.

[![](quickstart-images/qs12.png "대기 우선 순위 편집")](quickstart-images/qs12.png#lightbox)

Mac용 Visual Studio로 돌아가서 지금 프로젝트를 실행 하는 경우 오른쪽에는 크기가 작고 창의 크기가 조정 되는 것을 알 수 있습니다.

[![](quickstart-images/qs13.png "실행 중인 앱의 예")](quickstart-images/qs13.png#lightbox)

<a name="Defining-a-Presentation-Segue" />

## <a name="defining-a-presentation-segue"></a>프레젠테이션 Segue 정의

분할 뷰의 오른쪽을 레이아웃 하 여 선택한 텍스트의 속성에 대 한 검사기 역할을 수행 합니다. Inspector의 UI를 레이아웃 하기 위해 일부 컨트롤을 아래쪽 보기로 끌어 옵니다. 마지막 컨트롤의 경우 사용자가 기본 설정 문자 스타일 4 개를 선택할 수 있는 팝 오버를 표시 하려고 합니다.

검사기에 단추를 추가 하 고 Design Surface에 뷰 컨트롤러를 추가 합니다. 보기 컨트롤러의 크기를 팝 오버 원하는 크기로 조정 하 고 4 개의 단추를 추가 합니다. 다음으로, 검사기 보기에서 단추를 클릭 하 고 팝 오버를 나타내는 뷰 컨트롤러로 끕니다.

[![](quickstart-images/qs14.png "끌어서 새 segue 만들기")](quickstart-images/qs14.png#lightbox)

팝업 메뉴에서 **팝 오버**을 선택 합니다. 

[![](quickstart-images/qs15.png "Segue 유형 선택")](quickstart-images/qs15.png#lightbox)

마지막으로 Design Surface에서 Segue를 선택 하 고 기본 설정 된 **가장자리** 를 **왼쪽**으로 설정 합니다. 그런 다음 **앵커 뷰에서** 팝 오버를 연결 하려는 단추로 선을 끌어 옵니다.

[![](quickstart-images/qs16.png "끌어서 새 segue 만들기")](quickstart-images/qs16.png#lightbox)

Mac용 Visual Studio으로 돌아가면 앱을 실행 하 고 검사기에서 **없음** 단추를 클릭 하면 팝 오버가 표시 됩니다.

[![](quickstart-images/qs17.png "실행 중인 segue의 예")](quickstart-images/qs17.png#lightbox)

<a name="Creating-App-Preferences" />

## <a name="creating-app-preferences"></a>앱 기본 설정 만들기

대부분의 표준 macOS 앱은 사용자가 응용 프로그램의 다양 한 측면 (예: 모양 또는 사용자 계정)을 제어 하는 여러 옵션을 정의할 수 있는 _기본 설정 대화 상자_ 를 제공 합니다.

표준 기본 설정 대화 상자 창을 정의 하려면 먼저 탭 뷰 컨트롤러를 Design Surface으로 끌어 옵니다.

[![](quickstart-images/qs18.png "Xcode에서 storyboard 편집")](quickstart-images/qs18.png#lightbox)

그러면 두 개의 자식 뷰 컨트롤러가 연결 된 상태로 자동으로 연결 됩니다. 예를 들어, 내에서 가운데에 표시 되는 각 뷰에 레이블을 추가 합니다.

[![](quickstart-images/qs19.png "제약 조건 설정")](quickstart-images/qs19.png#lightbox)

다음으로, 사용자가 **기본 설정 ...** 메뉴 항목을 선택할 때 기본 설정 창을 표시 하려고 합니다. 메뉴 모음에서 기본 설정 메뉴 항목을 선택 하 고 **키를** 클릭 한 다음 탭 보기 컨트롤러로 줄을 끕니다.

[![](quickstart-images/qs20.png "드래그 하 여 segue 만들기")](quickstart-images/qs20.png#lightbox)

팝업에서 **모달** 을 선택 하 여이 창을 모달 대화 상자로 표시 합니다.

[![](quickstart-images/qs21.png "Segue 유형 선택")](quickstart-images/qs21.png#lightbox)

변경 내용을 저장 하 고 Mac용 Visual Studio 돌아가서 앱을 실행 한 다음 **기본 설정 ...** 메뉴 항목을 선택 하면 새 기본 설정 대화 상자가 표시 됩니다.

[![](quickstart-images/qs22.png "실행 중인 segue의 예")](quickstart-images/qs22.png#lightbox)

표준 macOS 앱 기본 설정 대화 상자 창이 표시 되지 않는 것을 알 수 있습니다. 이 문제를 해결 하려면 `Resources` **솔루션 탐색기** 의 xamarin.ios 앱 폴더에 두 개의 이미지 파일을 포함 하 고 Xcode의 Interface Builder으로 돌아옵니다.

탭 뷰 컨트롤러를 선택 하 고 **스타일** 을 **도구 모음**으로 전환 합니다. 

[![](quickstart-images/qs23.png "탭 모음 스타일 설정")](quickstart-images/qs23.png#lightbox)

각 탭을 선택 하 고 **레이블을** 지정 하 고 이미지 중 하나를 선택 하 여 표시 합니다.

[![](quickstart-images/qs24.png "Xcode에서 각 탭 구성")](quickstart-images/qs24.png#lightbox)

변경 내용을 저장 하 고 Mac용 Visual Studio 돌아가서 앱을 실행 한 다음 **기본 설정 ...** 메뉴 항목을 선택 하면 대화 상자가 이제 표준 macos 앱 처럼 표시 됩니다.

[![](quickstart-images/qs25.png "실행 기본 설정 창의 예")](quickstart-images/qs25.png#lightbox)

자세한 내용은 이미지, [메뉴](~/mac/user-interface/menu.md), [창](~/mac/user-interface/window.md) 및 [대화 상자](~/mac/user-interface/dialog.md) [사용](~/mac/app-fundamentals/image.md)설명서를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
