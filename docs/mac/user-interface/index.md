---
title: "사용자 인터페이스"
description: "이 문서는 다양 한 macOS UI 컨트롤에 설명 하는 지침에 연결 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: a8cb9488849dafc2cd720ecf59d654009a9ad781
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="user-interface"></a>사용자 인터페이스

_이 문서는 다양 한 macOS UI 컨트롤에 설명 하는 지침에 연결 합니다._

C# 및.NET Xamarin.Mac 응용 프로그램에서에서 작업할 때는 동일 하 게 액세스할 수 있습니다 작업 하는 개발자는 사용자 인터페이스 컨트롤 *Objective-c* 및 *Xcode* 않습니다. Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 만들기 및 사용자 인터페이스를 유지 관리 (또는 C# 코드에서 직접 만들 필요에 따라). 

아래에 나열 된 가이드 macOS UI 요소 Xamarin.Mac 응용 프로그램에서 작업 하는 방법에 대 한 자세한 정보가 표시 됩니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 그대로 섹션에서는 모든 문서에 사용할 예정 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 명령 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 하는 데 사용 합니다.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 Windows 패널와 작업을 수행 합니다. 만들기 및 창과 패널 창과 패널에서 로드 Xcode 및 인터페이스 작성기에서 유지 관리 설명 `.storyboard` 또는 `.xib` 창을 사용 하 여 및 C# 코드에서 Windows 응답 파일입니다.

## <a name="dialogsmacuser-interfacedialogmd"></a>[대화 상자](~/mac/user-interface/dialog.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 대화 상자 및 모달 창 작업을 설명합니다. Xcode 및 Interface Builder에서 모달 창을 생성 및 유지 관리하고, 표준 대화 상자를 작업하고, C# 코드에서 창을 표시하고 응답하는 내용을 다룹니다.

## <a name="alertsmacuser-interfacealertmd"></a>[경고](~/mac/user-interface/alert.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 경고 작업을 설명합니다. C# 코드에서 경고를 생성 및 표시하고 경고에 응답하는 내용을 다룹니다.

## <a name="menusmacuser-interfacemenumd"></a>[메뉴](~/mac/user-interface/menu.md)

메뉴는 Mac 응용 프로그램의 사용자 인터페이스의 다양 한 부분에 사용 됩니다. 응용 프로그램의 주 메뉴에서 창에서 아무 곳 이나 표시 될 수 있는 팝업 하 고 상황에 맞는 메뉴에 화면 맨 위에 있는 합니다. 메뉴는 Mac 응용 프로그램의 사용자 경험에서 필수 요소입니다. 이 문서에서는 Xamarin.Mac 응용 프로그램에서 Cocoa 메뉴를 작업하는 내용을 다룹니다.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[표준 컨트롤](~/mac/user-interface/standard-controls.md)

Xamarin.Mac 응용 프로그램에서 단추, 레이블, 텍스트 필드, 확인란 컨트롤 세그먼트 등 표준 AppKit 컨트롤을 사용 합니다. 이 가이드에서는 Xcode의 인터페이스 작성기에서 사용자 인터페이스 디자인에 추가할 콘센트 및 동작을 통해 코드에 노출 하 고 C# 코드에서 AppKit 컨트롤 작업을 다룹니다.

 
## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[도구 모음](~/mac/user-interface/toolbar.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 작업 도구 모음을 설명합니다. Xcode 및 Interface Builder에서 도구 모음을 생성 및 유지 관리하고, 출선 및 작업을 사용하여 도구 모음 항목을 코드에 노출하고, 도구 모음을 설정 및 해제하고, 마지막으로 C# 코드에서 도구 모음 항목에 응답하는 내용을 다룹니다.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[테이블 보기](~/mac/user-interface/table-view.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 테이블 보기와 작업을 설명합니다. 만들기 및 Xcode 및 인터페이스 작성기에서 테이블 뷰를 유지 관리 설명 콘센트 및 동작을 사용 하 여, 표 보기 채우기 및 C# 코드에서 항목 보기 테이블에 마지막으로 응답 코드에 항목 테이블 보기를 노출 하는 방법입니다.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[개요 보기](~/mac/user-interface/outline-view.md)

이 문서에서는 개요 뷰는 Xamarin.Mac 응용 프로그램에서 작업을 설명합니다. Xcode 및 Interface Builder에서 출선 보기를 생성 및 유지 관리하고, 출선 및 작업을 사용하여 출선 보기 항목을 코드에 노출하고, 출선 항목을 채우고, 마지막으로 C# 코드에서 출선 보기 항목에 응답하는 내용을 다룹니다.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[원본 목록](~/mac/user-interface/source-list.md)

이 문서에서는 소스 목록 Xamarin.Mac 응용 프로그램에서 작업을 설명합니다. Xcode 및 Interface Builder에서 원본 목록을 생성 및 유지 관리하고, 출선 및 작업을 사용하여 원본 목록 항목을 코드에 노출하고, 원본 목록 항목을 채우고, 마지막으로 C# 코드에서 원본 목록 항목에 응답하는 내용을 다룹니다.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[컬렉션 뷰](~/mac/user-interface/collection-view.md)

이 문서에서는 컬렉션 뷰 Xamarin.Mac 응용 프로그램에서 작업을 설명합니다. Xcode 및 Interface Builder에서 컬렉션 보기를 생성 및 유지 관리하고, 출선 및 작업을 사용하여 컬렉션 보기 요소를 코드에 노출하고, 컬렉션 보기를 채우고, 마지막으로 C# 코드에서 컬렉션 보기에 응답하는 내용을 다룹니다.

## <a name="creating-custom-user-controlsmacuser-interfacecustom-controlsmd"></a>[사용자 지정 컨트롤 만들기](~/mac/user-interface/custom-controls.md)

이 문서에서는 사용자 지정 사용자 인터페이스 컨트롤 만들기 (에서 상속 하 여 `NSControl`) 컨트롤에 대 한 사용자 지정 인터페이스 그리고 Xcode의 인터페이스 작성기와 함께 사용할 수 있는 사용자 지정 동작 만들기.

## <a name="mac-samples-gallery"></a>Mac 샘플 갤러리

에 제안는 [Mac 샘플 갤러리](http://developer.xamarin.com/samples/mac/all/), 다양 한 단계 Xamarin.Mac 프로젝트를 빠르게 시작 하는 데 도움이 되는 즉시 사용할 코드를 포함 합니다.

## <a name="related-links"></a>관련 링크

- [Human Interface Guidelines macOS](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
