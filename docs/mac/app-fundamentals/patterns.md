---
title: 공통 패턴 및 관용구 xamarin.mac
description: 이 문서에서는 Xamarin.Mac 앱을 빌드할 때 사용 하는 일반적인 디자인 패턴을 설명 합니다. 모델-뷰-컨트롤러 패턴, 데이터 원본 및 대리자 패턴 및 프로토콜에 설명 합니다.
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 06/17/2016
ms.openlocfilehash: b4266582ce0cec522e207cd06987f2d0eeff8b2d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61090891"
---
# <a name="common-patterns-and-idioms-in-xamarinmac"></a>공통 패턴 및 관용구 xamarin.mac

전체 C#을 통해 노출 되는 Apple Api를 특정 코드 관용구와 패턴 표시 반복 해 서. Xamarin.iOS 사용 하 여 프로그래밍 경험이 있다면 이러한 익숙해 보일 수 있습니다. 설명서는 자주 참조 이러한 패턴 및 관용구를 반복적으로의 확고 한 이해가 통해 확인할 수 있습니다 설명서의 의미 있도록 합니다.

## <a name="mvc---model-view-controller"></a>MVC-모델 뷰 컨트롤러

모델 뷰 컨트롤러 또는 줄여서 MVC Cocoa 전반에서 매우 일반적인 패턴입니다. 자세한 내용은이 문서의 범위를 벗어납니다. 이지만 간단히 구성 요소를 응용 프로그램을 구성 하는 방법.

- **모델** 개체 나타내고 원본으로 사용 되는 데이터를 볼 조작 (예: 주소록에 주소)
- **보기** 화면의 지정된 된 개체의 그리기 및 사용자 상호 작용 (화면에서 주소를 표시 하는 텍스트 필드)를 처리 개체 처리
- **컨트롤러** 개체 모델과 뷰 간의 상호 작용을 처리 합니다. 모델 변경 내용을 적용 하기 보기 업데이트 및 사용자가 UI에서 변경할 때 뷰에서 "down" 변경 내용 푸시를 "최대"입니다.

WPF와 같은 다른 라이브러리에서 MVVM (Model View ViewModel)를 사용 하 여 잘 알고 있다면 컨트롤러 ViewModel에 유사한 역할 수 있지만 특정 UI 요소에 더욱 긴밀 하 게 바인딩된 경우가 많습니다.

자세한 내용은 여기에서 찾을 수 있습니다.

- [Apple 웹 사이트에서 MVC 학습](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Objective C에서 모델 뷰 컨트롤러](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Windows 작업](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>데이터 원본 / 대리자 / 서브클래싱

Cocoa에서 또 다른 매우 일반적인 패턴 처리 UI 요소에 데이터를 제공 하 고 사용자 상호 작용에 반응 합니다. 사용 하 여 `NSTableView` 예를 들어 어떤 이유로 든 각 행에 대 한 데이터를 제공 해야, 일부 행을 나타내는 UI 요소에는 일부 설정 사용자 상호 작용 및 가능한 경우 약간의 사용자 지정에 반응 하는 동작의 집합입니다. 데이터 원본 및 대리자 패턴 하위 클래스에 의존 하지 않고도 대부분의 경우를 처리할 수 `NSTableView` 직접.

- 합니다 `DataSource` 속성의 사용자 지정 서브 클래스의 인스턴스가 할당 되 `NSTableViewDataSource` 데이터로 테이블을 채우는 데 이라고 (통해 `GetRowCount` 및 `GetObjectValue`).

- 합니다 `Delegate` 속성의 사용자 지정 서브 클래스의 인스턴스가 할당 되 `NSTableViewDelegate` 지정 된 모델 개체에 대 한 보기를 제공 하는 (통해 `GetViewForItem`) UI 상호 작용을 처리 하 고 (통해 `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`등).

경우에 따라 대리자 또는 데이터 원본에 지정 된 후크 이외의 방식으로 컨트롤을 사용자 지정 하려는 하 고 서브 클래스 뷰를 직접 수 있습니다. 그러나 주의 대부분의 기본값을 재정의 동작 다음 해야 사용자가 직접 해당 동작을 모두 처리할 수 있습니다 (선택 동작을 사용자 지정 해야 할 수 있습니다 선택 동작의 모든 사용자가 직접 구현).

Xamarin.iOS에서 일부 Api에서와 같은 `UITableView` 대리자와 데이터 소스를 모두 구현 하는 속성을 사용 하 여 확장 되었습니다 (`UITableViewSource`). 이 it 일반적인 제한 사항을 해결 하는 단일 C# 클래스는 하나의 기본 클래스를 하나만 사용할 수 있습니다 및 프로토콜을 표시 하는 기본 클래스를 통해 수행 됩니다.

Xamarin.Mac 응용 프로그램의 테이블 뷰를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [테이블 뷰](~/mac/user-interface/table-view.md) 설명서.

## <a name="protocols"></a>프로토콜

Objective-c에서 프로토콜의 인터페이스를 비교할 수 있습니다 C#, 대부분의 경우에서 이와 유사한 상황에서 사용 됩니다. 예를 들어를 `NSTableView` 위의 예에서 대리자와 데이터 소스는 실제로 프로토콜입니다. Xamarin.Mac을 재정의할 수 있습니다 가상 메서드를 사용 하 여 이러한 기본 클래스를 노출 합니다. 주요 차이점 C# 인터페이스와 Objective-c 프로토콜은 프로토콜의 일부 메서드를 구현 하려면 생략할 수 있습니다. 설명서 및/또는 선택적 결정 API의 정의 확인 해야 합니다.

자세한 내용은 참조 하십시오 우리의 [대리자, 프로토콜 및 이벤트](~/ios/app-fundamentals/delegates-protocols-and-events.md) 설명서.



## <a name="related-links"></a>관련 링크

- [테이블 뷰](~/mac/user-interface/table-view.md)
- [창 작업](~/mac/user-interface/window.md)
- [대리자, 프로토콜 및 이벤트](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
