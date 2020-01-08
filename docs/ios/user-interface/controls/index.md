---
title: Xamarin.ios의 사용자 인터페이스 컨트롤
description: 이 문서는 Xamarin.ios 개발자가 사용할 수 있는 다양 한 iOS 사용자 인터페이스 컨트롤을 설명 하는 가이드로 연결 됩니다. 연결 된 콘텐츠는 경고, 단추, 컬렉션 보기, 이미지, 수동 카메라 컨트롤, 지도, 레이블, 선택, 날짜 선택기 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: a4bf0b89a9ab336bf47ddcd104760211d912f423
ms.sourcegitcommit: bad1ab3f78d7f94d48511666626b54f8ba155689
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663436"
---
# <a name="user-interface-controls-in-xamarinios"></a>Xamarin.ios의 사용자 인터페이스 컨트롤

이 문서에서는 가장 일반적인 iOS 사용자 인터페이스 컨트롤 중 일부를 소개 하 고 사용 하는 방법을 소개 합니다.

## <a name="alertsalertsmd"></a>[경고](alerts.md)

IOS 8부터 UIAlertController는 이제 더 이상 사용 되지 않는 Uialertcontroller 및 Uialertcontroller를 모두 완료 했습니다.

## <a name="buttonsbuttonsmd"></a>[단추](buttons.md)

UIButton 클래스는 iOS 화면에서 다양 한 종류의 단추를 표시 하는 데 사용 됩니다. 이 섹션에서는 iOS의 단추 작업에 대 한 다양 한 옵션을 소개 합니다.

## <a name="collection-viewsuicollectionviewmd"></a>[컬렉션 뷰](uicollectionview.md)

`UICollectionView` 클래스에서 제공 되는 컬렉션 뷰는 레이아웃을 사용 하 여 화면에 여러 항목을 표시 하는 iOS 6의 새로운 개념입니다. 항목을 만들고 해당 항목과 상호 작용 하는 `UICollectionView`에 데이터를 제공 하는 패턴은 iOS 개발에 일반적으로 사용 되는 것과 동일한 위임 및 데이터 소스 패턴을 따릅니다.

## <a name="imagesimagemd"></a>[이미지](image.md)

앱에 이미지를 추가 하려면 먼저 두 단계가 필요 합니다. 먼저 프로젝트에 이미지를 추가 합니다. 그런 다음 컨트롤 및 코드를 추가 하 여 화면에 표시 합니다. Xamarin.ios에서 이미지를 처리 하는 방법에 대 한 자세한 내용은 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 문서를 참조 하세요.

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[수동 카메라 컨트롤](intro-to-manual-camera-controls.md)

IOS 8의 `AVFoundation Framework`에서 제공 하는 수동 카메라 컨트롤을 사용 하 여 모바일 응용 프로그램이 iOS 장치의 카메라를 완전히 제어할 수 있습니다. 이 세분화 된 제어 수준을 사용 하면 스틸 이미지 또는 비디오를 가져오는 동안 카메라의 매개 변수를 조정 하 여 전문가 수준의 카메라 응용 프로그램을 만들고 음악가 컴퍼지션을 제공할 수 있습니다.

## <a name="mapsios-mapsindexmd"></a>[지도](ios-maps/index.md)

맵은 모든 최신 모바일 운영 체제에서 일반적으로 사용할 수 있는 기능입니다. iOS는 맵 키트 프레임 워크를 통해 기본적으로 매핑 지원을 제공 합니다. 지도 키트를 사용 하면 응용 프로그램에서 풍부한 대화형 지도를 쉽게 추가할 수 있습니다. 이러한 맵은 지도의 위치를 표시 하는 주석을 추가 하 고 임의 모양의 그래픽을 표시 하는 등의 다양 한 방법으로 사용자 지정할 수 있습니다. 지도 키트에는 장치의 현재 위치를 표시 하는 기본 지원도 있습니다.

## <a name="labelslabelsmd"></a>[레이블](labels.md)

`UILabel` 컨트롤은 단일 및 여러 줄 읽기 전용 텍스트를 표시 하는 데 사용 됩니다.

## <a name="pickers-and-date-pickerspickermd"></a>[선택 및 날짜 선택기](picker.md)

선택 컨트롤은 선택 된 값이 강조 표시 된 스크롤 가능한 값 목록을 포함 하는 ' 휠 유사 ' 컨트롤을 표시 합니다. 사용자가 휠을 회전 하 여 원하는 옵션을 선택 합니다.

에서 날짜 및/또는 시간을 설정 하도록 선택 하는 한 가지 특정 사용자 사례가 있습니다. 이 Apple에를 제공 하기 위해 UIDatePicker 라는 UIPickerView 클래스의 사용자 지정 하위 클래스를 만들었습니다.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[진행률 및 활동 표시기](progress-activity-indicator.md)

iOS는 앱에서 진행률을 나타내는 두 가지 주요 방법, 즉 활동 표시기 (특정 _네트워크_ 활동 표시기 포함) 및 진행률 표시줄을 제공 합니다.

## <a name="search-barssearchbarmd"></a>[검색 막대](searchbar.md)

UISearchBar는 값 목록을 검색 하는 데 사용 됩니다. 

## <a name="sliders-switches-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[슬라이더, 스위치 및 분할된 컨트롤](slider-switch-segmented-controls.md)

슬라이더 컨트롤을 사용 하면 범위 내에서 숫자 값을 간단히 선택할 수 있습니다. iOS는 다른 플랫폼에서 라디오 단추로 표현할 수 있는 부울 입력으로 `UISwitch`를 사용 합니다. 분할 된 컨트롤은 사용자가 적은 수의 옵션을 조작할 수 있도록 구성 된 방법입니다.

## <a name="stack-viewuistackviewmd"></a>[스택 보기](uistackview.md)

`UIStackView`(Stack View control)는 Auto Layout 및 Size 클래스의 강력한 기능을 활용 하 여 iOS 장치의 방향 및 화면 크기에 동적으로 응답 하는 가로 또는 세로로 하위 뷰 스택을 관리 합니다.

## <a name="tables-and-cellstablesindexmd"></a>[테이블 및 셀](tables/index.md)

이 섹션에서는 테이블을 만들고 표시 하는 데 사용 되는 클래스를 소개 하 고 Xamarin.ios에서 사용 하는 방법에 대 한 예제를 제공 합니다. 테이블에 대 한 기본 모양을 사용 하 고, 레이아웃을 사용자 지정 하며, 편집을 구현 하 고 Xamarin iOS Designer를 사용 하 여 테이블을 시각적으로 디자인 하는 방법을 다룹니다. 경우에 따라 표시 되는 행의 목록 (예: 음악 앱) 및 테이블 컨트롤을 인식 하기 어려운 다른 시간 (예: 연락처 앱에서 편집 또는 메시지의 대화)이 표시 됩니다.

## <a name="text-inputtext-inputmd"></a>[텍스트 입력](text-input.md)

사용자 텍스트를 허용 하는 것은 여러 줄의 편집 가능 텍스트에 대 한 한 줄 입력 및 UITextView의 `UITextField` 사용 하 여 수행 됩니다. 이러한 컨트롤 중 하나를 화면으로 끌어 온 다음 두 번 클릭 하 여 초기 텍스트를 설정할 수 있습니다.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[탭 표시줄 및 탭 표시줄 컨트롤러](creating-tabbed-applications.md)

탭 탐색 UI를 사용 하는 iOS 응용 프로그램은 UITabBarController 클래스를 사용 하 여 빌드됩니다. 이 문서에서는 여러 컨트롤러 및 뷰를 포함 하는 탭 응용 프로그램을 설정 하는 방법을 안내 합니다. 그런 다음 로그인 화면 후와 같이 루트 컨트롤러가 아닌 Uitab바 컨트롤러를 로드 하는 방법을 살펴보겠습니다.

## <a name="web-viewswebviewmd"></a>[웹 보기](webview.md)

이 문서에서는 Apple에서 제공 하는 웹 보기,`WKWebview` 및 `SFSafariViewController`(유사성 및 차이점) 및이를 사용 하는 방법을 살펴봅니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
