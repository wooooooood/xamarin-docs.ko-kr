---
title: Xamarin.Mac – 관련 설명서
description: '이 문서에서는 Xamarin.Mac 개발자를 위한 관련 설명서: Xamarin.iOS 설명서, Apple의 Mac 개발자 센터 및 Xamarin.Mac으로 사용자 인터페이스를 작성하는 방법을 설명하는 다양한 설명서에 대한 연결을 제공합니다.'
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 12/02/2016
ms.openlocfilehash: ae1ac9b40a0e6a62076a6143e64fa8beb82df6c5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107319"
---
# <a name="xamarinmac-related-documentation"></a>Xamarin.Mac – 관련 설명서

[developer.xamarin.com](~/mac/get-started/index.md)의 Mac 섹션 외에도 Xamarin.Mac과 관련된 의문을 해결할 수 있는 세 가지 좋은 자료가 있습니다.

- [**Xamarin.iOS 설명서**](~/ios/get-started/index.md) - 여러 API(주로 AppKit/UIKit 외부의)에서 iOS와 macOS 버전이 약간 다를 뿐입니다. 특정 iOS API의 이름이 `UIFoo`인 경우 macOS에서 `NSFoo`라고 하는 이름의 비슷한 API를 찾을 수 있습니다. 이러한 예제는 일반적으로 C#에 이미 있습니다.

- **Apple의 [Mac 개발자 센터](https://developer.apple.com/devcenter/mac/)** - Objective-C에서 호출하는 API를 간단하게 C#으로 변환하는 예제를 자주 제공합니다. 자세한 방법은 [Mac API 이해](~/mac/app-fundamentals/mac-apis.md)를 참조하세요.

- [**스택 오버플로**](http://stackoverflow.com/) - ["NSOutlineView의 모든 노드를 자동으로 확장하려면 어떻게 해야 하나요"](http://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes)와 같은 간단한 일회성 질문에 대한 답을 제공하는 훌륭한 리소스입니다. 이러한 예제는 종종 Objective-C에서도 제공되며 C#으로 변환해야 합니다. 하지만 C#에는 답변의 하위 집합이 있습니다.

## <a name="user-interface"></a>사용자 인터페이스

Xamarin.Mac 응용 프로그램에서 C# 및 .NET을 작업할 때 개발자는 *Objective-C* 및 *Xcode*에서 작업하는 개발자와 동일한 사용자 인터페이스 컨트롤에 액세스할 수 있습니다. Xamarin.Mac이 Xcode와 직접 통합되므로 개발자는 Xcode의 _Interface Builder_를 사용하여 앱의 사용자 인터페이스를 만들고 유지 관리할 수 있습니다(또는 필요에 따라 C# 코드에서 바로 작성).

아래에 나열된 지침은 Xamarin.Mac 응용 프로그램에서 macOS 요소를 작업하는 자세한 방법을 설명합니다.

- [Windows](~/mac/user-interface/window.md)
- [대화 상자](~/mac/user-interface/dialog.md)
- [경고](~/mac/user-interface/alert.md)
- [메뉴](~/mac/user-interface/menu.md)
- [도구 모음](~/mac/user-interface/toolbar.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [원본 목록](~/mac/user-interface/source-list.md)
