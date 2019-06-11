---
title: Xamarin.iOS에서 사용자 인터페이스 컨트롤
description: 이 문서는 다양 한 iOS 사용자 인터페이스 컨트롤에 설명 Xamarin.iOS 개발자가 사용할 수 있는 가이드에 연결 됩니다. 연결 된 콘텐츠 경고, 단추, 컬렉션 뷰, 이미지, 수동 카메라 컨트롤, maps, 레이블, 선택, 날짜 선택 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: fab653eb24b504fa17e6f2eac32915fd88580817
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827820"
---
# <a name="user-interface-controls-in-xamarinios"></a>Xamarin.iOS에서 사용자 인터페이스 컨트롤

이 문서에는 몇 가지 가장 일반적인 iOS 사용자 인터페이스 컨트롤 및 사용 하는 방법을 소개 합니다.

## <a name="alertsalertsmd"></a>[경고](alerts.md)

IOS 8 부터는 UIAlertController 완료 된 대체 UIActionSheet 있으며 UIAlertView는 이제 사용 되지 않습니다.

## <a name="buttonsbuttonsmd"></a>[단추](buttons.md)

UIButton 클래스는 다양 한 유형의 iOS 화면에서 단추를 나타내는 데 사용 됩니다. 이 섹션에서는 iOS에서 단추를 사용 하 여 작업에 대 한 다양 한 옵션을 소개 합니다.

## <a name="collection-viewsuicollectionviewmd"></a>[컬렉션 뷰](uicollectionview.md)

사용할 수 있는 컬렉션 뷰는 `UICollectionView` 클래스, ios 6 소개 레이아웃을 사용 하 여 화면에 여러 항목을 표시 하는 새로운 개념입니다. 데이터를 제공 하는 패턴을 `UICollectionView` 항목을 만들고 동일한 위임 및 데이터 원본 iOS 개발에 일반적으로 사용 되는 패턴에 해당 항목 따라 상호 작용 합니다.

## <a name="imagesimagemd"></a>[이미지](image.md)

앱에 이미지를 추가 하려면 두 단계가 필요 합니다: 먼저 프로젝트에 이미지 추가 컨트롤 및 화면에 표시 하 여 코드를 추가 합니다. 참조를 [이미지를 사용 하 여 작업](~/ios/app-fundamentals/images-icons/index.md) 검사 Xamarin.iOS에서 처리 하는 이미지의 자세한 문서.

## <a name="manual-camera-controlsintro-to-manual-camera-controlsmd"></a>[수동 카메라 컨트롤](intro-to-manual-camera-controls.md)

제공 하는 수동 카메라 컨트롤을 `AVFoundation Framework` ios 8에서 모바일 응용 프로그램이 iOS 장치의 카메라를 통해 완전 한 제어를 허용 합니다. 전문가 수준 카메라 응용 프로그램을 만들어 이미지 또는 비디오를 계속 수행 하는 동안 카메라의 매개 변수를 조정 하 여 아티스트 컴퍼지션을 제공이 세분화 된 수준의 제어를 사용할 수 있습니다.

## <a name="mapsios-mapsindexmd"></a>[지도](ios-maps/index.md)

Maps는 모든 최신 모바일 운영 체제의 일반적인 기능. iOS에는 Map 키트 프레임 워크를 통해 고유 하 게 매핑 지원을 제공합니다. Map 키트를 사용 하 여 응용 프로그램 풍부 하 고 대화형 지도 쉽게 추가할 수 있습니다. 이러한 맵은 다양 한 위치를 지도에 표시할 주석을 추가 임의 셰이프의 그래픽을 오버레이 등의 방법으로 사용자 지정할 수 있습니다. Map 키트도에 장치의 현재 위치를 표시 하기 위한 기본 제공 지원

## <a name="labelslabelsmd"></a>[레이블](labels.md)

`UILabel` 읽기 전용 텍스트를 단일 및 여러 줄을 표시 하기 위한 컨트롤을 사용 합니다.

## <a name="pickers-and-date-pickerspickermd"></a>[선택기 및 날짜 선택](picker.md)

선택 컨트롤 강조 표시 하 여 선택한 값으로 값의 스크롤 가능한 목록이 포함 된 ' 휠 형식 ' 컨트롤을 표시 합니다. 사용자가 휠을 원하는 옵션을 선택 합니다.

하나의 특정 사용자 경우 선택기에 대 한 날짜 및/또는 시간을 설정 합니다. 이 Apple에 제공 하는 UIDatePicker 호출 되는 UIPickerView 클래스의 사용자 지정 서브 클래스를 만들었습니다.

## <a name="progress-and-activity-indicatorsprogress-activity-indicatormd"></a>[진행률 및 활동 표시기](progress-activity-indicator.md)

iOS 앱에서 진행률을 나타내는 두 가지를 제공 합니다. 활동 표시기 (특정을 포함 하 여 _네트워크_ 활동 표시기) 및 진행률 표시줄입니다.

## <a name="search-barssearchbarmd"></a>[검색 막대](searchbar.md)

UISearchBar 값 목록을 검색 하는 데 사용 됩니다. 

## <a name="sliders-switches-and-segmented-controlsslider-switch-segmented-controlsmd"></a>[슬라이더, 스위치 및 분할된 컨트롤](slider-switch-segmented-controls.md)

슬라이더 컨트롤 범위 내에서 숫자 값의 간단한 선택할 수 있습니다. iOS를 사용 하는 `UISwitch` 부울 값을 입력 하는 다른 플랫폼에서 라디오 단추에 표시 될 수 있습니다. 분할 된 컨트롤은 체계적인된 방식으로 사용자가 적은 수의 옵션을 사용 하 여 상호 작용할 수 있도록 합니다.

## <a name="stack-viewuistackviewmd"></a>[스택 보기](uistackview.md)

스택 뷰 컨트롤 (`UIStackView`) 활용 자동 레이아웃 및 크기 클래스 가로 또는 세로로 하위의 스택을 관리 하는 화면 크기와 방향 iOS 장치에 동적으로 응답 합니다.

## <a name="tables-and-cellstablesindexmd"></a>[테이블 및 셀](tables/index.md)

그의 섹션을 만들고 테이블을 표시 하는 데 클래스를 소개 다음 Xamarin.iOS에서 사용 하는 방법의 예제를 제공 합니다. 테이블을 편집 하 고 Xamarin iOS 디자이너를 사용 하 여 테이블을 시각적으로 디자인을 구현 하 여 레이아웃을 사용자 지정에 대 한 기본 모양을 사용 하 여 적용 됩니다. 경우에 따라 표시를 분명히 행 (예: 음악 앱) 및 (예: 연락처 앱에는 대화에서 메시지 앱 편집) 테이블 컨트롤을 인식 하는 것이 어렵습니다 경우도 목록입니다.

## <a name="text-inputtext-inputmd"></a>[텍스트 입력](text-input.md)

사용자 텍스트 입력을 적용 하 여 수행 됩니다는 `UITextField` 줄으로 입력 하 고 여러 줄 편집 가능한 텍스트 UITextView에 대 한 합니다. 이러한 컨트롤의 화면으로 끌 수 있으며 초기 텍스트를 설정 하려면 두 번 클릭 수 있습니다.

## <a name="tab-bars-and-tab-bar-controllerscreating-tabbed-applicationsmd"></a>[탭 표시줄 및 탭 표시줄 컨트롤러](creating-tabbed-applications.md)

탭 탐색 UI를 사용 하 여 iOS 응용 프로그램 UITabBarController 클래스를 사용 하 여 빌드됩니다. 이 문서에서는 여러 컨트롤러와 뷰를 포함 하는 탭된 응용 프로그램을 설정 하는 방법을 살펴봅니다. 그런 다음 루트 컨트롤러와 같은 한 후에 로그인 화면을 하는 경우는 UITabBarController를 로드 하는 방법을 검사 합니다.

## <a name="web-viewsuiwebviewmd"></a>[웹 보기](uiwebview.md)

이 문서에서는 살펴봅니다 Apple에서 제공 하는 세 가지 웹 보기의 각: `UIWebView`, `WKWebview`, 및 `SFSafariViewController`등의 유사성 및 차이점을 어떻게 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/monotouch/Controls/)
