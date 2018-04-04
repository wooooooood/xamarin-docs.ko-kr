---
title: macOS 사용자 인터페이스
description: 이 문서는 다양 한 macOS UI 컨트롤에 설명 하는 지침에 연결 합니다.
ms.prod: xamarin
ms.assetid: 876B6EC2-E158-43F2-B9C9-03F54F3D2A49
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: d40faa29f2fe278377bf4eae42a032f3dc9086ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="macos-user-interface"></a>macOS 사용자 인터페이스

_이 문서는 다양 한 macOS UI 컨트롤에 설명 하는 지침에 연결 합니다._

를 사용할 때 C# 및.NET Xamarin.Mac 응용 프로그램에서 액세스할 수 있는 동일한 사용자 인터페이스를 제어 하에서 작업을 수행할 *Objective-c* 및 *Xcode* 않습니다. Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 만들기 및 사용자 인터페이스를 유지 관리 (또는 C# 코드에서 직접 만들 필요에 따라).

아래에 나열 된 가이드 macOS UI 요소 Xamarin.Mac 응용 프로그램에서 작업 하는 방법에 대 한 자세한 정보가 표시 됩니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 그대로 섹션에서는 모든 문서에 사용할 예정 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md#exposing-c-classes--methods-to-objective-c) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명으로 문서는 `Register` 및 `Export` 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 사용 되는 특성입니다.

## <a name="windowsmacuser-interfacewindowmd"></a>[Windows](~/mac/user-interface/window.md)

이 문서에서는 windows와 패널 Xamarin.Mac 응용 프로그램에서 작업을 수행 합니다. 만들기 및 windows와 Xcode 및 windows를 로드 하는 인터페이스 작성기의 패널 및 패널.storyboard 또는.xib 파일에서 유지 관리, windows를 사용 하 여 C# 코드에서 windows에 응답을 포함 합니다.

## <a name="dialogsmacuser-interfacedialogmd"></a>[대화 상자](~/mac/user-interface/dialog.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 모달 창 및 대화 상자를 사용 합니다. 만들고 Xcode 및 표준 대화 상자를 사용 하 고 표시 하 고 C# 코드에서 windows에 응답 인터페이스 작성기에서 모달 창을 유지 관리 하는 내용을 다룹니다.

## <a name="alertsmacuser-interfacealertmd"></a>[경고](~/mac/user-interface/alert.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 알림이 있는 작업을 설명합니다. 만들기 및 C# 코드에서 발생 한 경고를 표시 하 고, 경고에 응답 하는 내용을 다룹니다.

## <a name="menusmacuser-interfacemenumd"></a>[메뉴](~/mac/user-interface/menu.md)

메뉴는 Mac 응용 프로그램의 사용자 인터페이스의 다양 한 부분에 사용 됩니다. 응용 프로그램의 주 메뉴에서 팝업 메뉴와 창에서 아무 곳 이나 표시 될 수 있는 상황에 맞는 메뉴에 화면 맨 위에 있는 합니다. 메뉴는 Mac 응용 프로그램 사용자 경험의 필수적인 부분입니다. 이 문서에서는 Cocoa 메뉴 Xamarin.Mac 응용 프로그램에서 작업에 설명 합니다.

## <a name="standard-controlsmacuser-interfacestandard-controlsmd"></a>[표준 컨트롤](~/mac/user-interface/standard-controls.md)

단추, 레이블, 텍스트 필드, 확인란, Xamarin.Mac 응용 프로그램에서 세그먼트 컨트롤 등 표준 AppKit 컨트롤을 사용 합니다. 이 가이드에서는 Xcode의 인터페이스 작성기에서 사용자 인터페이스 디자인에 추가한 콘센트 및 동작을 통해 코드에 노출 하 고 C# 코드에서 AppKit 컨트롤 작업을 설명 합니다.

## <a name="toolbarsmacuser-interfacetoolbarmd"></a>[도구 모음](~/mac/user-interface/toolbar.md)

이 문서에 작업 Xamarin.Mac 응용 프로그램에서 도구 모음에 설명합니다. 만들기 및 관리 도구 모음 Xcode 및 인터페이스 작성기에서 다루고 콘센트 및 동작을 사용 하 여, 설정 및 도구 모음 항목 사용 안 함 및 C# 코드에서 도구 모음 항목에 마지막으로 응답 코드를 도구 모음 항목을 노출 하는 방법입니다.

## <a name="table-viewsmacuser-interfacetable-viewmd"></a>[테이블 뷰](~/mac/user-interface/table-view.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 테이블 뷰를 사용 하는 작업을 설명합니다. 만들기 및 Xcode 및 인터페이스 작성기에서 테이블 뷰를 유지 관리 설명 테이블 뷰를 노출 하려면 콘센트 및 동작을 사용 하 여 코드 항목, 테이블 뷰를 채우는 하는 방법과 테이블 C# 코드 항목 보기에 응답 합니다.

## <a name="outline-viewsmacuser-interfaceoutline-viewmd"></a>[개요 보기](~/mac/user-interface/outline-view.md)

이 문서에서는 Xamarin.Mac 응용 프로그램의 개요 보기와 작업을 설명합니다. 만들기 및 Xcode 및 인터페이스 작성기의 개요 보기를 유지 관리 설명 개요 뷰를 채우는 방법 개요 보기를 노출 하려면 콘센트 및 동작을 사용 하 여 코드 항목, 응답 C# 코드에서 항목 보기에 간략하게 설명 하 고 있습니다.

## <a name="source-listsmacuser-interfacesource-listmd"></a>[소스 목록](~/mac/user-interface/source-list.md)

이 문서에서는 소스 목록 Xamarin.Mac 응용 프로그램에서 사용 하 여 작업을 설명합니다. 만들기 및 Xcode 및 인터페이스 작성기에서 소스 목록을 유지 관리 설명 콘센트 및 동작을 사용 하 여, 원본 목록 채우기 및 C# 코드의 소스 목록 항목에 응답 하는 코드를 소스 목록 항목을 노출 하는 방법입니다.

## <a name="collection-viewsmacuser-interfacecollection-viewmd"></a>[컬렉션 뷰](~/mac/user-interface/collection-view.md)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 컬렉션 뷰를 사용 합니다. 만들기 및 Xcode 및 인터페이스 작성기에 대 한 컬렉션 뷰를 유지 관리 설명 컬렉션 뷰를 노출 하려면 콘센트 및 동작을 사용 하 여 코드 항목, 컬렉션 뷰를 채우는 방법과 C# 코드에 대 한 컬렉션 뷰를 응답 하지 않습니다.

## <a name="creating-custom-controlsmacuser-interfacecustom-controlsmd"></a>[사용자 지정 컨트롤 만들기](~/mac/user-interface/custom-controls.md)

이 문서에서는 사용자 지정 사용자 인터페이스 컨트롤 만들기 (에서 상속 하 여 `NSControl`), 사용자 지정 인터페이스는 컨트롤에 대 한 그리기 및 Xcode의 인터페이스 작성기와 함께 사용할 수 있는 사용자 지정 동작 만들기.

## <a name="mac-samples-gallery"></a>Mac 샘플 갤러리

에 제안 된 [Mac 샘플 갤러리](https://developer.xamarin.com/samples/mac/all/)합니다. 다양 한 단계 Xamarin.Mac 프로젝트를 빠르게 시작 하는 데 도움이 되는 즉시 사용할 코드를 포함 합니다.

## <a name="related-links"></a>관련된 링크

- [Human Interface Guidelines macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
