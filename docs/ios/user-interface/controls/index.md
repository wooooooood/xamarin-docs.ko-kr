---
title: 컨트롤
description: Xamarin.iOS는 Apple에서 제공한 모든 기본 사용자 인터페이스 개체를 노출 합니다. IOS 디자이너 Xcode의 인터페이스 작성기를 사용 하 여 Xamarin.iOS 앱에 추가 될 쉽게 또는 프로그래밍 방식으로 합니다. Xamarin.iOS 선택 하는 방법에 관계 없이 모든 사용자 인터페이스 개체 속성 및 C#에서 메서드를 노출 합니다.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 82b2998319d4e78ee4f58a6d024032a509724537
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="controls"></a>컨트롤

_Xamarin.iOS는 Apple에서 제공한 모든 기본 사용자 인터페이스 개체를 노출 합니다. IOS 디자이너 Xcode의 인터페이스 작성기를 사용 하 여 Xamarin.iOS 앱에 추가 될 쉽게 또는 프로그래밍 방식으로 합니다. Xamarin.iOS 선택 하는 방법에 관계 없이 모든 사용자 인터페이스 개체 속성 및 C#에서 메서드를 노출 합니다._

이 문서에서는 가장 일반적인 iOS 사용자 인터페이스 컨트롤 및을 사용 하는 방법을 소개 합니다.

## <a name="alertsalertsmd"></a>[경고](alerts.md)

IOS 8 이상에서는 UIAlertController 완료 된 대체 UIActionSheet 개이고 UIAlertView는 이제 사용 되지 않습니다.

## <a name="buttonsbuttonsmd"></a>[단추](buttons.md)

UIButton 클래스는 다양 한 유형의 iOS 화면에서 단추를 나타내는 데 사용 됩니다. 이 섹션에서는 iOS의 단추를 사용 하기 위한 다양 한 옵션을 소개 합니다.

## <a name="collection-viewsuicollectionviewmd"></a>[컬렉션 뷰](uicollectionview.md)

사용할 수 있는 컬렉션 뷰는 `UICollectionView` 클래스는 레이아웃을 사용 하 여 화면에 여러 항목을 제시 소개 하는 iOS 6에서에서 새로운 개념입니다. 데이터를 제공 하는 것에 대 한 패턴을 `UICollectionView` 항목을 만들고 동일한 위임 및 데이터 원본 iOS 개발에 일반적으로 사용 되는 패턴 해당 항목에 따라 상호 작용을 합니다.

## <a name="imagesimagemd"></a>[이미지](image.md)

앱에 이미지를 추가 하려면 두 단계가 필요 합니다: 먼저 프로젝트;에 이미지를 추가 컨트롤 및 화면에 표시 하려면 코드를 추가 합니다. 참조는 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) Xamarin.iOS에서 처리 하는 이미지의 범위 보다 자세한 문서.

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[수동 카메라 컨트롤](intro-to-manual-camera-controls.md)

제공 하는 수동 카메라 컨트롤은 `AVFoundation Framework` iOS 8, iOS 장치의 카메라를 통해 완전히 제어 하는 모바일 응용 프로그램을 허용 합니다. 이 세분화 된 수준의 제어 여전히 이미지 나 비디오 촬영 하는 동안 카메라의 매개 변수를 조정 하 여 아티스트 컴포지션 설명과 전문 수준 카메라 응용 프로그램을 만드는 데 사용할 수 있습니다.

## <a name="mapsios-mapsindexmd"></a>[지도](ios-maps/index.md)

지도 모바일 운영 체제의 일반적인 기능입니다. iOS는 기본적으로 지도 키트 프레임 워크를 통해 매핑을 지원 합니다. 지도 키트 응용 프로그램 다양 하 고 대화형 지도 쉽게 추가할 수 있습니다. 다양 한 방법으로 지도에 위치를 표시 하는 주석을 추가 하 고 그래픽의 임의 셰이프를 겹치게 등 이러한 맵은 사용자 지정할 수 있습니다. 도 맵 키트에는 장치의 현재 위치를 표시 하는 데 기본적으로 지원 합니다.

## <a name="labelslabelsmd"></a>[레이블](labels.md)

`UILabel` 읽기 전용 텍스트 컨트롤은 단일 행 삽입과 다중 행을 표시 하기 위해 사용 됩니다.

## <a name="pickers-and-date-pickerspickermd"></a>[선택 및 날짜 선택](picker.md)

선택 컨트롤 강조 표시 되 고 선택한 값을 사용 하 여 값의 스크롤 가능한 목록을 포함 하는 ' 휠 형식 ' 컨트롤을 표시 합니다. 사용자가 휠을 원하는 옵션을 선택 합니다.

하나의 특정 사용자 대/소문자 선택에 대 한 날짜 및/또는 시간을 설정할 수 있습니다. 이 Apple에 제공 하 라는 UIDatePicker UIPickerView 클래스의 사용자 지정 하위를 생성 했습니다.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[진행률 및 활동 표시기](progress-activity-indicator.md)

iOS 응용 프로그램에서 진행률을 표시 하는 두 가지 주요 방법으로 제공: 활동 표시기 (특정을 포함 하 여 _네트워크_ 활동 표시기) 및 진행률 표시줄입니다.

## <a name="search-barssearchbarmd"></a>[검색 창](searchbar.md)

UISearchBar 값의 목록을 검색 하는 데 사용 됩니다. 

## <a name="sliders-steppers-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[슬라이더, 스텝, 및 세그먼트 컨트롤](slider-switch-segmented-controls.md)

범위 내에서 숫자 값의 간단한 선택에 대 한 슬라이더 컨트롤 수 있습니다. iOS를 사용 하 여는 `UISwitch` 부울 입력 다른 플랫폼에서 라디오 단추에 표시 될 수 있습니다. 분할 된 컨트롤은 사용자가 적은 수의 옵션 상호 작용할 수 있도록 하는 구성 된 방법.

## <a name="stack-viewuistackviewmd"></a>[스택 보기](uistackview.md)

스택 뷰 컨트롤 (`UIStackView`) 이용 하 여 자동 레이아웃 및 크기 클래스 가로 또는 세로로 하위 뷰가, 스택에서 관리 하는 iOS 장치의 방향 및 화면 크기에 동적으로 응답 합니다.

## <a name="tables-and-cellstablesindexmd"></a>[테이블 및 셀](tables/index.md)

그의 섹션을 만들고 테이블을 표시 하는 데 사용 되는 클래스에서는 다음 Xamarin.iOS에서 사용 하는 방법의 예제를 제공 합니다. Xamarin iOS 디자이너를 사용 하 여 테이블을 시각적으로 디자인 하 고 편집을 구현 하 여 레이아웃을 사용자 지정 테이블에 대 한 기본 모양을 사용 하 여 적용 됩니다. 경우에 따라 디스플레이 분명히 행 (예: 음악 응용 프로그램) 및 다른 이유 (예: 연락처 앱 또는 대화에서 메시지 앱에서 편집) 테이블 컨트롤을 인식 하는 것이 어렵습니다의 목록입니다.

## <a name="text-inputtext-inputmd"></a>[텍스트 입력](text-input.md)

사용자 텍스트 입력을 적용 하 여 수행 됩니다는 `UITextField` 입력과 UITextView 여러 줄 편집 가능한 텍스트를 한 줄에 대 한 합니다. 이러한 컨트롤의 화면으로 끌 수 있으며 초기 텍스트를 설정 하려면 두 번 클릭.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[탭 표시줄 및 탭 표시줄 컨트롤러](creating-tabbed-applications.md)

iOS 응용 프로그램 탭 탐색 UI를 사용 하 여 UITabBarController 클래스를 사용 하 여 작성 됩니다. 이 문서의 여러 컨트롤러와 뷰를 포함 하는 탭된 응용 프로그램을 설정 하는 방법을 살펴봅니다. 그런 다음는 UITabBarController 후에 루트 컨트롤러와 같은 로그인 화면이 때 로드 하는 방법을 검토 합니다.

## <a name="web-viewsuiwebviewmd"></a>[웹 보기](uiwebview.md)

이 문서에서는 살펴볼 것 각 Apple에서 제공한 세 가지 웹 보기: `UIWebView`, `WKWebview`, 및 `SFSafariViewController`등의 유사점 및 차이점 및 방법을 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
