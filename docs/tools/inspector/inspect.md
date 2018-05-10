---
title: 라이브 응용 프로그램을 검사합니다.
description: 시각화 하 고 라이브 앱 디버그
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: 8bcdc5ac50a0f03086ad9e9c3265e3ce86b06704
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="inspecting-live-applications"></a>라이브 응용 프로그램을 검사합니다.

기업 고객에 대 한 라이브 앱 검사 ´ ù입니다.


1. [Xamarin 통합 문서 및 관리자를 설치 합니다.](~/tools/inspector/install.md)

1. 아무 것 이나 열고 [지원 응용 프로그램 프로젝트](~/tools/inspector/install.md#supported-platforms) Mac 또는 Visual Studio 용 Visual Studio에서.
1. 디버그 모드에서 앱을 실행합니다.
1. 클릭는 **검사** IDE 도구 모음 단추 (Visual Studio에서의 **검사 현재 앱....**  메뉴 항목에서 제공 됩니다.는 **도구** 또는 **디버그** 메뉴).



[![](inspect-images/mac-heres-the-button.png "IDE의 도구 모음에서 검사 단추를 클릭 합니다.")](inspect-images/mac-heres-the-button.png#lightbox)

새 REPL 프롬프트와 함께 새 Xamarin 검사기 클라이언트 창이 열립니다.

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "새 Xamarin 검사기 클라이언트 창이 열리고, 새로운 REPL 프롬프트")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

이 창 표시 될 때 대화형 C# 프롬프트를 실행 하 고 C# 문 및 식 평가 하는 데 사용할 수 있는 해야 합니다. 가 있기 때문에이 고유 코드 대상 프로세스의 컨텍스트에서 계산 됩니다. 이 경우 표시 되는 iOS 응용 프로그램에 대해 실행 되는 코드를 나와 있습니다.

대상 프로세스에서 응용 프로그램의 상태 변경 내용을 실제로 발생 하 고, 사용할 수 있도록 응용 프로그램을 변경 하는 데 C#: 라이브, 또는 라이브 응용 프로그램의 상태를 검사할 수 있습니다.

예를 들어 ios, 우리의 UIApplication 대리자 클래스는 기본 드라이버 (여기서 많은 응용 프로그램 상태 저장) 즉를 찾으려고 할 수 있습니다.

    var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
    del.Database.GetAllCustomers ()
    ...
    del.Database.AddCustomer (...)

(각 제출 여러 줄 편집기에서 발생 하는지 확인 합니다. `Shift + Enter` 새 줄을 만들어집니다 및 `Cmd + Enter` (`Ctrl + Enter` Windows에서) 평가 위해 코드를 전송 하 게 합니다. `Enter` 자동으로 때 제출는 것이 안전 합니다.)

응용 프로그램의 시각적 요소를 시작 하는 편리한 방법을 "검사" 단추를 사용 하 여 됩니다. 이 키를 누르는 경우 응용 프로그램을 클릭 하 여 UI 요소를 선택할 수 있습니다. 변수 `selectedView` 화면에 실제 요소를 가리키도록에 할당 됩니다. 또한 위의 스크린 샷에서 액세스 및 편집 되는지에 볼 수 있습니다 `selectedView.BarTintColor` 에 `UISearchBar` 선택 했다면 합니다.

라이브 시각적 트리는 매우 유용 합니다. 계층 구조 보기의 현재 스냅숏을 나타냅니다. 설정 하는 행을 선택할 수 `selectedView` REPL에서 및 보기의 속성 값을 표시 합니다. Mac, 계층화 된 보기의 3 차원 쪼개진된 시각화와 상호 작용할 수 있습니다. Windows에서 보기의 속성 값을 시각적으로 편집할 수 있습니다.

## <a name="known-limitations"></a>알려진된 제한 사항

 - 기본 디스플레이에서 보기 선택만 지원 됩니다.
 - 속성 표에서 편집, Mac을 사용할 수 없으면 하 고 Windows에서 몇 가지 데이터 형식으로 제한 됩니다. 보다 강력한 편집용 REPL을 사용 합니다.
 - 으로 검사기 추가 기능/확장 설치 및 IDE에서 사용 하도록 설정 된 디버그 모드에서 시작 될 때마다 앱에 코드를 삽입는 했습니다. 앱에서 모든 비정상적인 동작을 발견 하는 경우 try 사용 하지 않도록 설정 하거나 검사기 추가 기능/확장, IDE, 다시 시작 및 다시 검사 하십시오. 및 하십시오 [버그](~/tools/inspector/install.md#reporting-bugs) 를 보내 주세요!
 - UI 요소를 검사에서 계속 변경으로 인해, 하세요 [알려주세요](~/tools/inspector/install.md#reporting-bugs)와 마찬가지로이 버그를 나타낼 수 있습니다.

