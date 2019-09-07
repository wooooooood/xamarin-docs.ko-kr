---
title: Xamarin.ios의 일반적인 패턴 및 관용구
description: 이 문서에서는 Xamarin.ios 앱을 빌드할 때 사용 되는 일반적인 디자인 패턴에 대해 설명 합니다. 모델-뷰-컨트롤러 패턴, 데이터 소스 및 대리자 패턴 및 프로토콜에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 06/17/2016
ms.openlocfilehash: b4934fa82d862ad2e8ab53579137873ed9e4bcca
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770174"
---
# <a name="common-patterns-and-idioms-in-xamarinmac"></a>Xamarin.ios의 일반적인 패턴 및 관용구

를 통해 C#노출 되는 Apple api 전체에서 특정 관용구 패턴은 다시 발생 합니다. Xamarin.ios를 사용 하 여 프로그래밍 하는 경험이 있다면 이러한 작업을 잘 살펴볼 수 있습니다. 설명서는 종종 이러한 패턴을 반복적으로 참조 하 고 관용구을 확실 하 게 이해 하 게 되 면 찾은 설명서를 이해 하는 데 도움이 됩니다.

## <a name="mvc---model-view-controller"></a>MVC 모델 뷰 컨트롤러

모델 뷰 컨트롤러 또는 short 용 MVC는 Cocoa 전체에서 발견 되는 매우 일반적인 패턴입니다. 자세한 논의는이 문서에서 다루지 않지만 응용 프로그램을 구성 요소로 구성 하는 방법에 대 한 간략 한 설명입니다.

- **모델** 개체는 표시 되 고 조작 되는 기본 데이터를 나타냅니다 (주소록의 주소와 같이).
- 개체 **보기** 화면에서 지정 된 개체의 그리기를 처리 하 고 사용자 상호 작용을 처리 합니다 (화면에 주소를 표시 하는 텍스트 필드).
- **컨트롤러** 개체는 모델과 뷰 간의 상호 작용을 처리 합니다. 사용자가 UI를 변경할 때 뷰를 업데이트 하 고 뷰를 "아래로" 변경 하 여 뷰를 업데이트 합니다.

WPF와 같은 다른 라이브러리에서 MVVM (모델 뷰 ViewModel)에 대해 잘 알고 있는 경우 컨트롤러는 ViewModel과 비슷하지만 특정 UI 요소에 더 가깝게 바인딩되어 있는 경우가 많습니다.

자세한 내용은 다음을 참조 하세요.

- [Apple 웹 사이트에서 MVC 학습](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [목표의 모델 뷰 컨트롤러-C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Windows 작업](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>데이터 원본/대리자/서브클래싱

Cocoa의 또 다른 매우 일반적인 패턴은 UI 요소에 데이터를 제공 하 고 사용자 상호 작용에 반응 하는 것을 다룹니다. 예 `NSTableView` 를 들어를 사용 하 여 각 행에 대 한 데이터, 해당 행을 나타내는 일부 UI 요소 집합, 사용자 상호 작용에 반응 하는 일부 동작 집합 및 일부 사용자 지정 항목을 제공 해야 합니다. 데이터 소스 및 대리자 패턴을 사용 하면 사용자를 서브클래싱 `NSTableView` 하지 않고도 대부분의 경우를 처리할 수 있습니다.

- 속성 `DataSource` 에는 `GetRowCount` 및 `NSTableViewDataSource`를통해테이블 에데이터를채우도록호출되는사용자지정하위클래스의인스턴스가할당됩니다.`GetObjectValue`

- 속성 `Delegate` 에는 지정 된 모델 개체 `GetViewForItem`에 대 한 뷰 `NSTableViewDelegate` 를 제공 하 고, 등을 `MouseDownInHeaderOfTableColumn`통해 `DidClickTableColumn`UI 상호 작용을 처리 하는의 사용자 지정 서브 클래스 인스턴스가 할당 됩니다.

경우에 따라 대리자 또는 데이터 소스에 지정 된 후크를 벗어나 컨트롤을 사용자 지정 하 고 뷰를 직접 서브 클래스 할 수 있습니다. 그러나 대부분의 경우 기본 동작을 재정의 하려면 모든 동작을 직접 처리 해야 합니다. 선택 동작을 사용자 지정 하려면 모든 선택 동작을 직접 구현 해야 할 수도 있습니다.

Xamarin.ios에서와 `UITableView` 같은 일부 api는 대리자와 데이터 소스 (`UITableViewSource`)를 모두 구현 하는 속성으로 확장 되었습니다. 이를 통해 단일 C# 클래스가 기본 클래스를 하나만 가질 수 있는 일반적인 제한 사항을 해결할 수 있으며 기본 클래스를 통해 프로토콜을 쉽게 확인할 수 있습니다.

Xamarin.ios 응용 프로그램에서 테이블 뷰로 작업 하는 방법에 대 한 자세한 내용은 [테이블 보기](~/mac/user-interface/table-view.md) 설명서를 참조 하세요.

## <a name="protocols"></a>프로토콜

목표-C의 프로토콜은의 C#인터페이스와 비교할 수 있으며, 대부분의 경우 비슷한 상황에서 사용 됩니다. 예를 들어 `NSTableView` 위의 예제에서 대리자와 데이터 소스는 모두 실제로 프로토콜입니다. Xamarin.ios는 재정의할 수 있는 가상 메서드를 사용 하 여 이러한 클래스를 기본 클래스로 노출 합니다. C# 인터페이스와 객관적인 프로토콜의 주요 차이점은 프로토콜의 일부 메서드를 구현 하는 것이 선택 사항 일 수 있다는 것입니다. 선택 사항인 항목을 확인 하려면 API의 설명서 및/또는 정의를 확인 해야 합니다.

자세한 내용은 [대리인, 프로토콜 및 이벤트 설명서를](~/ios/app-fundamentals/delegates-protocols-and-events.md) 참조 하세요.

## <a name="related-links"></a>관련 링크

- [테이블 보기](~/mac/user-interface/table-view.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [대리자, 프로토콜 및 이벤트](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
