---
title: 라이브 애플리케이션 검사
description: 이 문서에서는 Xamarin Inspector를 사용 하 여 응용 프로그램을 검사 하는 방법을 설명 합니다. 또한 Xamarin Inspector 도구의 제한 사항을 설명 합니다.
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: bbb1e21139b5f073e2cc7e3d4781e8bc38334449
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006300"
---
# <a name="inspecting-live-applications"></a>라이브 애플리케이션 검사

라이브 앱 검사는 기업 고객에 게 제공 됩니다.

1. Mac용 Visual Studio 또는 Visual Studio에서 [지원 되는 모든 응용 프로그램 프로젝트](~/tools/inspector/install.md#supported-platforms) 를 엽니다.
1. 디버그 모드에서 앱을 실행합니다.
1. IDE 도구 모음에서 **검사** 단추를 클릭 합니다 (Visual Studio에서 **현재 앱 검사** 메뉴 항목은 **도구** 또는 **디버그** 메뉴 에서도 사용할 수 있음).

[![](inspect-images/mac-heres-the-button.png "Click the Inspect button in the IDE toolbar")](inspect-images/mac-heres-the-button.png#lightbox)

새 REPL 프롬프트가 표시 되 고 새 Xamarin Inspector 클라이언트 창이 열립니다.

[![](inspect-images/inspector-0.7.0-map-inspect-small.png "A new Xamarin Inspector client window will open, with a fresh REPL prompt")](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

이 창이 표시 되 면 문과 식을 실행 하 C# 고 평가 C# 하는 데 사용할 수 있는 대화형 프롬프트가 표시 됩니다. 이를 고유 하 게 만드는 것은 코드가 대상 프로세스의 컨텍스트에서 평가 된다는 것입니다. 이 경우 표시 되는 iOS 응용 프로그램에 대해 실행 되는 코드를 보여 줍니다.

응용 프로그램의 상태에 대 한 모든 변경 내용은 실제로 대상 프로세스에서 발생 하므로를 사용 C# 하 여 응용 프로그램을 실시간으로 변경 하거나 응용 프로그램의 상태를 실시간으로 검사할 수 있습니다.

예를 들어 iOS에서는 응용 프로그램 상태를 많이 저장 하는 기본 드라이버란 UIApplication delegate 클래스를 찾으려고 할 수 있습니다.

```csharp
var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
del.Database.GetAllCustomers ()
...
del.Database.AddCustomer (...)
```

각 제출은 여러 줄 편집기에서 발생 합니다. `Shift + Enter`는 새 줄을 만들고 `Cmd + Enter` (Windows`Ctrl + Enter`)는 평가를 위해 코드를 제출 합니다. `Enter` 안전 하면 자동으로 전송 됩니다.)

"검사" 단추를 사용 하 여 응용 프로그램의 시각적 요소를 보다 편리 하 게 가져올 수 있습니다. 이 단추를 누르면 응용 프로그램을 클릭 하 여 UI 요소를 선택할 수 있습니다. 화면에서 실제 요소를 가리키도록 `selectedView` 변수가 할당 됩니다. 위의 스크린샷에서는 선택한 `UISearchBar`에 대 한 `selectedView.BarTintColor` 액세스 한 후 편집 된 방법을 볼 수 있습니다.

라이브 시각적 트리도 매우 유용 합니다. 뷰 계층의 현재 스냅숏을 나타냅니다. 행을 선택 하 여 REPL에 `selectedView`을 설정 하 고 뷰의 속성 값을 볼 수 있습니다. Mac에서는 계층화 된 뷰의 3D 쪼개진 시각화와 상호 작용할 수 있습니다. Windows에서는 보기의 속성 값을 시각적으로 편집할 수 있습니다.

## <a name="known-limitations"></a>알려진 제한 사항

- 보기 선택은 기본 표시 에서만 지원 됩니다.
- Mac에서는 속성 그리드를 편집할 수 없으며 Windows에서는 몇 가지 데이터 형식으로 제한 됩니다. 더 강력한 편집을 위해 REPL을 사용 합니다.
- 검사기 추가/확장이 IDE에서 설치 되 고 사용 하도록 설정 되어 있으면 디버그 모드에서 시작 될 때마다 앱에 코드를 삽입 합니다. 앱에서 이상한 동작이 발생 하는 경우 검사기 추가/확장을 사용 하지 않도록 설정 하거나 제거 하 고, IDE를 다시 시작 하 고, rechecking를 시도 하세요. [버그](~/tools/inspector/install.md#reporting-bugs) 를 알려 주세요.
- UI 요소를 검사 하 여 해당 요소를 변경 하는 경우 버그를 나타낼 수 [있으므로 알려주세요.](~/tools/inspector/install.md#reporting-bugs)
