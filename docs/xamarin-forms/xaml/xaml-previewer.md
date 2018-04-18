---
title: Xamarin.Forms에 대 한 XAML 미리 보기
description: 입력할 때 렌더링 Xamarin.Forms 레이아웃을 참조 하세요!
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 02/24/2017
ms.openlocfilehash: d23f89ed8ad7956f7a366280a14ccc12ba3dac0c
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2018
---
# <a name="xaml-previewer-for-xamarinforms"></a>Xamarin.Forms에 대 한 XAML 미리 보기

_입력할 때 렌더링 Xamarin.Forms 레이아웃을 참조 하세요!_

## <a name="requirements"></a>요구 사항

프로젝트에서 실행 되도록 XAML 미리 보기에 대 한 최신 Xamarin.Forms NuGet 패키지를 필요 합니다. Android 앱을 미리 보기 필요 [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)합니다.

에 대 한 자세한 정보는 [릴리스 정보](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer)합니다.

## <a name="getting-started"></a>시작

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

사용 하 여는 **보기 > 다른 창 > Xamarin.Forms 미리 보기** 미리 보기 창을 열려면 Visual Studio에서 메뉴. 사용 하 여는 **창 > 새 세로 탭 그룹** 메뉴-나란히 배치 합니다.

[![Visual Studio에서 ListView 컨트롤의 미리 보기](xaml-previewer-images/xamlp-list-vs-sml.png "Visual Studio의 Forms 미리 보기")](xaml-previewer-images/xamlp-list-vs.png#lightbox "Visual Studio의 Forms 미리 보기")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**미리 보기** 단추 XAML 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 편집기에 표시 될 수 **연결 프로그램 > 미리 보기 양식**합니다. 미리 보기 창에 다음 표시 하거나 숨길 수 키를 눌러는 **미리 보기** 모든 XAML 문서 창의 오른쪽 위 모퉁이의 단추:

[![Mac 용 Visual Studio에서 ListView 컨트롤 preview](xaml-previewer-images/xamlp-list-sml.png "Mac 용 Visual Studio의 Forms 미리 보기")](xaml-previewer-images/xamlp-list.png#lightbox "Mac 용 Visual Studio의 Forms 미리 보기")

-----

## <a name="xaml-preview-options"></a>XAML 미리 보기 옵션

미리 보기 창의 맨 위에 옵션이 있습니다.

* **전화** -전화 크기 화면에 렌더링 합니다.
* **태블릿** – 태블릿 크기 화면에 렌더링할 (확대/축소 컨트롤 창의 오른쪽 아래에 있는 참고)
* **Android** – Android 버전의 화면 표시
* **iOS** – 화면 iOS 버전이 표시 됨
* (아이콘)-Portait 세로 방향을 사용 하 여 미리 보기
* 가로 (아이콘)-사용 하 여 가로 방향 미리 보기

## <a name="adding-design-time-data"></a>디자인 타임 데이터를 추가합니다.

일부 레이아웃 없이 사용자 인터페이스 컨트롤에 바인딩된 모든 데이터를 시각화 하기 어려울 수 있습니다. 미리 보기를 보다 유용 하도록 하려면 몇 가지 정적 데이터 컨트롤에 할당 하드 코딩 하 여 바인딩 컨텍스트 (코드 숨김 또는 XAML을 사용 하 여 하나).

James Montemagno의를 참조 [디자인 타임 데이터를 추가 하는 방법에 대 한 블로그 게시물](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) XAML에서 정적 ViewModel에 바인딩하는 방법을 볼 수 있습니다.

## <a name="troubleshooting"></a>문제 해결

아래 문제 확인 및 [Xamarin 포럼](https://forums.xamarin.com/categories/xamarin-forms), 문제가 발생 합니다.

### <a name="xaml-preview-isnt-showing"></a>XAML 미리 보기 항목이 표시 되지 않습니다.

다음을 확인하십시오.

* 프로젝트를 빌드해야 (컴파일된) XAML 파일을 미리 볼 시도 하기 전에.
* 디자이너 에이전트가 설치 처음 XAML 파일을 미리 볼 수 있어야-진행률 표시기가 준비 될 때까지 진행 메시지와 함께 미리 보기에 표시 됩니다.
* 다시 여세요 XAML 파일을 다시 열어 합니다.

### <a name="invalid-xaml-the-android-project-needs-to-built-before-preview-can-be-created"></a>잘못 된 XAML: Android 프로젝트에 추가 해야 미리 보기를 만들기 전에 작성

XAML 미리 보기 페이지를 렌더링 하기 전에 프로젝트를 빌드할 수 있어야 합니다.
미리 보기 창의 위쪽에 아래 오류 표시, 응용 프로그램을 빌드 전 다시 시도 하십시오.

![오류 메시지: 프로젝트 먼저 빌드해야](xaml-previewer-images/error-not-built-sml.png "오류 메시지: 프로젝트를 다시 작성")
