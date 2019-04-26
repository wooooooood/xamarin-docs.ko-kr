---
title: Xamarin.Mac의 macOS 사용자 인터페이스 컨트롤
description: 이 문서는 Xamarin.Mac 개발자에 게 사용할 수 있는 다양 한 사용자 인터페이스 컨트롤을 설명 하는 지침에 연결 합니다. 연결 된 콘텐츠는 windows, 대화 상자, 경고, 메뉴, 도구 모음, 테이블 뷰, 개요 보기 및 자세히 살펴보겠습니다.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/27/2018
ms.openlocfilehash: a12553cf0b7b9584bb8ff7bc04ed326ad4a7ad2a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61281599"
---
# <a name="macos-user-interface-controls-in-xamarinmac"></a>Xamarin.Mac의 macOS 사용자 인터페이스 컨트롤

_이 문서는 다양 한 macOS UI 컨트롤을 설명 하는 가이드에 연결 합니다._

Xamarin.Mac 응용 프로그램에서 C# 및.NET을 사용 하 여 작업에 액세스할 수 있습니다 동일한 사용자 인터페이스 컨트롤에서 작업을 수행할 *Objective-c* 하 고 *Xcode* 않습니다. Xcode의 Xamarin.Mac이 Xcode와 직접 통합 되므로 사용할 수 있습니다 _Interface Builder_ 만들기 및 사용자 인터페이스를 유지 관리 (또는 필요에 따라 C# 코드에서 바로 작성).

아래 나열 된 가이드 Xamarin.Mac 응용 프로그램에 macOS UI 요소를 사용 하는 방법에 대 한 자세한 정보를 제공 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및 모든 문서에서를 사용 하는 기술을 설명 합니다.

확인 하려는 경우는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서화를 `Register` 및 `Export` 특성을 사용 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소입니다.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

이 문서에서는 windows 및 Xamarin.Mac 응용 프로그램에서 패널을 사용 하 여 작업을 다룹니다. 만들고 유지 관리 및 패널 Xcode 및 Interface Builder를 로드 하는 windows에서 창과 패널.storyboard 또는.xib 파일에서 windows를 사용 하 여 C# 코드에서 windows로 응답을 설명 합니다.

## <a name="dialogsmacuser-interfacedialogmd"></a>[대화 상자](~/mac/user-interface/dialog.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 대화 상자 및 모달 사용. 만들고 Xcode 및 Interface Builder 및 표준 대화 상자를 사용 하 고, 표시 하 고, C# 코드에서 windows로 응답에서 모달 창을 유지 관리 하는 내용을 다룹니다.

## <a name="alertsmacuser-interfacealertmd"></a>[경고](~/mac/user-interface/alert.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 경고를 사용 하 여 작업을 설명 합니다. 만들기 및 C# 코드에서 발생 한 경고를 표시 하 고, 경고에 응답 하는 내용을 다룹니다.

## <a name="menusmacuser-interfacemenumd"></a>[메뉴](~/mac/user-interface/menu.md)

메뉴는 Mac 응용 프로그램의 사용자 인터페이스의 여러 부분에 사용 됩니다. 응용 프로그램의 주 메뉴에서 팝업 메뉴 및 상황에 맞는 메뉴 창에서 아무 곳 이나 표시 될 수 있는 화면 맨 위에 있는 합니다. 메뉴는 Mac 애플리케이션의 사용자 환경에서 필수 요소입니다. 이 문서에서는 Xamarin.Mac 응용 프로그램에서 Cocoa 메뉴를 사용 하 여 작업을 설명 합니다.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[표준 컨트롤](~/mac/user-interface/standard-controls.md)

단추, 레이블, 텍스트 필드, 확인란 및 Xamarin.Mac 응용 프로그램에서 분할 된 컨트롤 같은 표준 AppKit 컨트롤을 사용합니다. 이 가이드에서는 Xcode의 Interface Builder에서 사용자 인터페이스 디자인을 추가, 출 선 및 작업을 통해 코드에 노출 하 및 C# 코드에서 AppKit 컨트롤을 사용 하 여 작업을 다룹니다.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[도구 모음](~/mac/user-interface/toolbar.md)

이 문서에서는 Xamarin.Mac 응용 프로그램의 도구 모음을 사용 하 여 작업을 설명 합니다. 만들기 및 Xcode 및 Interface Builder에서 도구 모음을 유지 관리 설명 도구 모음 항목을 출 선 및 작업을 사용 하 여, 사용 하도록 설정 하 고 도구 모음 항목을 사용 하지 않도록 설정 및 마지막으로 C# 코드에서 도구 모음 항목에 응답 하는 코드에 노출 하는 방법입니다.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[테이블 뷰](~/mac/user-interface/table-view.md)

이 문서에서는 Xamarin.Mac 응용 프로그램의 테이블 뷰를 사용 하 여 작업을 설명 합니다. 만들기 및 Xcode 및 Interface Builder에서 테이블 뷰를 유지 관리 설명 테이블 보기를 노출 하려면 출 선 및 작업을 사용 하 여 코드 항목, 테이블 뷰를 채우는 및 C# 코드에서 테이블 보기 항목에 응답 합니다.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[개요 보기](~/mac/user-interface/outline-view.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에 대 한 개요 보기를 사용 하 여 작업을 설명 합니다. 만들기 및 개요 뷰가 Xcode 및 Interface Builder에서 유지 관리 설명 개요 보기를 노출 하려면 출 선 및 작업을 사용 하 여 코드 항목, 개요 보기 채우기 및 C# 코드에서 보기 항목을 간략하게 설명에 응답 합니다.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[원본 목록](~/mac/user-interface/source-list.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 원본 목록 사용 하 여 작업을 설명 합니다. 만들기 및 Xcode 및 Interface Builder에서 원본 목록을 유지 관리 설명 원본 목록 항목을 출 선 및 작업을 사용 하 여, 원본 목록 채우기 및 C# 코드에서 원본 목록 항목에 응답 하는 코드를 노출 하는 방법입니다.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[컬렉션 뷰](~/mac/user-interface/collection-view.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 컬렉션 뷰를 사용 하 여 작업을 설명 합니다. 만들기 및 Xcode 및 Interface Builder에서 컬렉션 뷰를 유지 관리 설명 컬렉션 뷰를 노출 하려면 출 선 및 작업을 사용 하 여 코드 항목, 컬렉션 보기를 채우고 및 C# 코드에서 컬렉션 보기에 응답 합니다.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[사용자 지정 컨트롤 만들기](~/mac/user-interface/custom-controls.md)

이 문서에서는 사용자 지정 사용자 인터페이스 컨트롤을 만들고 (에서 상속 하 여 `NSControl`), 컨트롤에 대 한 인터페이스를 사용자 지정 그리기 및 Xcode의 Interface Builder를 사용 하 여 사용할 수 있는 사용자 지정 작업 만들기.

## <a name="mac-samples-gallery"></a>Mac 샘플 갤러리

살펴보는 것이 좋습니다 합니다 [Mac 샘플 갤러리](https://developer.xamarin.com/samples/mac/all/)합니다. 다양 한 순조로운 Xamarin.Mac 프로젝트를 빠르게 도움이 될 수 있는 준비-사용 가능한 코드를 포함 합니다.

## <a name="related-links"></a>관련 링크

- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
