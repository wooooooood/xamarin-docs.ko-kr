---
title: 라이브 애플리케이션 검사
description: 이 문서에서는 Xamarin Inspector를 사용 하 여 응용 프로그램을 검사 하는 방법을 설명 합니다. 또한 Xamarin Inspector 도구의 제한에 대해서도 설명 합니다.
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 2bd68def0a29d4bb94f8cc66c8cbfa00add1700d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60948157"
---
# <a name="inspecting-live-applications"></a>라이브 애플리케이션 검사

라이브 앱 검사는 엔터프라이즈 고객이 사용할 수 있습니다.

1. 열 [앱 프로젝트를 지 원하는](~/tools/inspector/install.md#supported-platforms) Mac 또는 Visual Studio 용 Visual Studio에서.
1. 디버그 모드에서 앱을 실행합니다.
1. 클릭 합니다 **검사** IDE 도구 모음의 단추 (Visual Studio에서의 **검사 현재 앱....**  메뉴 항목에서 제공 됩니다. 합니다 **도구** 하거나 **디버그** 메뉴).

[![](inspect-images/mac-heres-the-button.png "IDE 도구 모음의 검사 단추를 클릭 합니다.")](inspect-images/mac-heres-the-button.png#lightbox)

새 REPL 프롬프트와 함께 새 Xamarin Inspector 클라이언트 창이 열립니다.

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "새 Xamarin Inspector 클라이언트 창이 열리고, 새 REPL 프롬프트와 함께")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

대화형 해야이 창이 표시 되 면 C# 실행 하 고 평가 하는 데 사용할 수 있는 프롬프트 C# 문 및 식입니다. 이 고유한 되어 대상 프로세스의 컨텍스트에서 코드를 계산을 있습니다. 이 경우에 표시 하 여 iOS 응용 프로그램에 대해 실행 되는 코드를 표시 됩니다.

응용 프로그램의 상태에 수행한 모든 변경 내용을 대상 프로세스에서 발생 실제로 사용할 수 있도록 C# 변경 하려면 응용 프로그램을 라이브로 라이브 응용 프로그램의 상태를 검사할 수 있습니다.

예를 들어, iOS에서 수 하고자 (여기서는 많은 응용 프로그램 상태 저장) 하는 기본 드라이버는 UIApplication 대리자 클래스를 찾을:

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(참고 제출할 때마다 여러 줄 편집기에서 발생 하 합니다. `Shift + Enter` 새 줄을 만들어집니다 및 `Cmd + Enter` (`Ctrl + Enter` Windows에서) 평가 대 한 코드를 제출 합니다. `Enter` 자동으로 때 제출 것이 안전 합니다.)

응용 프로그램의 시각적 요소를 이동 하는 보다 편리한 방법을 "검사" 단추를 사용 하는 것입니다. 이 작업을 누르면 응용 프로그램을 클릭 하 여 UI 요소를 선택할 수 있습니다. 변수의 `selectedView` 화면에서 실제 요소에 할당 됩니다. 위의 스크린샷에서 보면 어떻게 액세스 하 고 편집한 `selectedView.BarTintColor` 에 `UISearchBar` 선택 했다면 합니다.

라이브 시각적 트리도 매우 유용 합니다. 뷰 계층의 현재 스냅숏을 나타냅니다. 행 집합을 선택할 수 있습니다 `selectedView` REPL 및 보기의 속성 값을 참조 하세요. Mac에서 계층화 된 뷰의 쪼개진된 3D 시각화를 조작할 수 있습니다. Windows에서 보기의 속성 값을 시각적으로 편집할 수 있습니다.

## <a name="known-limitations"></a>알려진된 제한 사항

 - 뷰 선택은 기본 디스플레이에 지원 됩니다.
 - 속성 표에서 편집 Mac을 사용할 수 없는 하 고 Windows에서 몇 가지 데이터 형식으로 제한 됩니다. 더 강력한 편집에 대 한 REPL을 사용 합니다.
 - 검사기 추가 기능을 확장을 설치 및 IDE에서 사용 하도록 설정 하기만 디버그 모드에서 시작 될 때마다 앱에 코드를 삽입는 했습니다. 앱에서 이상 현상을 표시 하는 경우에 해제 또는 검사기 추가 기능/확장을 제거 하 고, IDE를 다시 시작 하 고 재확인 하세요. 보시고 [버그](~/tools/inspector/install.md#reporting-bugs) 알려주세요!
 - 그래도 변경으로 인해 UI 요소를 검사, 하세요 [알려주세요](~/tools/inspector/install.md#reporting-bugs)처럼이 버그를 나타낼 수 있습니다.

