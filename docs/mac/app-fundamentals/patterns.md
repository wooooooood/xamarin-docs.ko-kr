---
title: 일반적인 패턴 및 관용구
description: 이 문서에서는 모델-뷰-컨트롤러 패턴, 데이터 원본 및 대리자 패턴 및 프로토콜에 설명 합니다.
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/17/2016
ms.openlocfilehash: cf499c555ddbefbdb5a7fae3ae8929a17e0d0cd7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="common-patterns-and-idioms"></a>일반적인 패턴 및 관용구

C#을 통해 노출 되는 Apple Api 전체에서 특정 관용구와 패턴 대로 반복 됩니다. Xamarin.iOS 사용한 프로그래밍 경험이 있는 경우 이러한 친숙 한 보일 수 있습니다. 설명서 자주 참조 합니다 이러한 패턴 및 관용구를 반복 해 서 그중에서 확실 하 게 이해 하지 찾았으면 설명서의 결과 이해 하면 도움이 됩니다.

## <a name="mvc---model-view-controller"></a>MVC-모델 뷰 컨트롤러

모델 뷰 컨트롤러 또는 줄여서 MVC Cocoa 전체에서 발견 되는 매우 일반적인 패턴입니다. 자세한 내용은이 문서의 범위를 벗어납니다 이지만 간단히 구성 요소에 응용 프로그램의 구조를 지정 하는 방법:

- **모델** 개체 나타내고 되 고 있는 원본으로 사용 데이터 보고 조작 (예: 주소록에서 주소)
- **보기** 화면에 지정된 된 개체의 그리기 및 사용자 상호 작용 (화면에서 주소를 표시 하는 텍스트 필드)를 처리 개체 처리
- **컨트롤러** 개체 모델 및 뷰 간의 상호 작용을 처리 합니다. 모델 변경 내용을 밀어 뷰를 업데이트 및 사용자가 UI에서 변경할 때 보기에서 "아래" 변경 내용을 푸시 하려면 "최대"입니다.

WPF 등의 다른 라이브러리에서 MVVM (모델 뷰 ViewModel)에 잘 알고 있다면, 컨트롤러 역할 ViewModel 비슷합니다 되지만 더욱 긴밀 하 게 특정 UI 요소에 바인딩된 경우가 많습니다.

자세한 내용은 여기에서 확인할 수 있습니다.

- [Apple 웹 사이트에 MVC 학습](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Objective C의 모델 뷰 컨트롤러](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [창 작업](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>데이터 원본 / 위임 / 서브클래싱

Cocoa에 매우 일반적인 패턴을 다른 UI 요소에 데이터를 제공 및 사용자 상호 작용에 응답할를 다룹니다. 사용 하 여 `NSTableView` 예를 들어, 어떻게 하 든 각 행에 대 한 데이터를 제공 해야 할 해당 일부 행을 나타내는 UI 요소에는 일부 설정할 사용자 상호 작용 및 사용자 지정 일정 기간 수에 반응 하는 동작의 집합입니다. 데이터 원본 및 대리자 패턴 하면 대부분의 경우 하위 클래스에 의존 하지 않고도 `NSTableView` 직접 합니다.

- `DataSource` 속성의 사용자 지정 하위 클래스의 인스턴스를 할당 된 `NSTableViewDataSource` 데이터로 테이블을 채우는 데 사용 하는 호출 되 (통해 `GetRowCount` 및 `GetObjectValue`).

- `Delegate` 속성의 사용자 지정 하위 클래스의 인스턴스를 할당 된 `NSTableViewDelegate` 지정 된 모델 개체에 대 한 보기를 제공 하는 (통해 `GetViewForItem`) UI 상호 작용을 처리 하 고 (통해 `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`등).

일부 경우에 대리자 또는 데이터 원본에 지정 된 후크 외 방식으로 컨트롤을 사용자 지정 해야 하 고 직접 보기 서브클래싱 할 수 있습니다. 하지만 주의 해야, 대부분의 경우 기본 재정의 동작 해야 합니다 직접 해당 동작을 모두 처리할 수 있습니다 (선택 동작을 사용자 지정 해야 할 수 있습니다 선택 동작의 모든 사용자가 직접 구현)입니다.

Xamarin.iOS, 일부 Api에서에서와 같은 `UITableView` 대리자와 데이터 소스를 구현 하는 속성으로이 확장 되었습니다 (`UITableViewSource`). 하나의 기본 클래스 및 프로토콜에서 표시가 우리의 단일 C# 클래스가 가질 수 있는 일반적인 문제점을 해결 하도록 이렇게 기본 클래스를 통해 합니다.

표 보기 Xamarin.Mac 응용 프로그램에서 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [테이블 뷰](~/mac/user-interface/table-view.md) 설명서입니다.

## <a name="protocols"></a>프로토콜

Objective C의 C#에서 인터페이스에 비교할 수 프로토콜과 많은 경우에는 이와 유사한 상황에서 사용 됩니다. 예를 들어는 `NSTableView` 위 예제에서 대리자와 데이터 원본 프로토콜은 실제로 프로토콜입니다. Xamarin.Mac 가상 메서드를 재정의할 수 있습니다이 방법으로 기본 클래스를 노출 합니다. C#의 인터페이스 및 Objective-c 프로토콜의 주요 차이점은 프로토콜의 일부 메서드를 구현 하는 선택적 수 있습니다. 설명서 및/또는 선택적 결정 API의 정의를 확인 해야 합니다.

자세한 내용은 참조 하십시오 우리의 [대리자, 프로토콜 및 이벤트](~/ios/app-fundamentals/delegates-protocols-and-events.md) 설명서입니다.



## <a name="related-links"></a>관련 링크

- [테이블 뷰](~/mac/user-interface/table-view.md)
- [창 작업](~/mac/user-interface/window.md)
- [대리자, 프로토콜 및 이벤트](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
