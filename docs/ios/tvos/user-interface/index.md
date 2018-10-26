---
title: TvOS Xamarin 사용 하 여 사용자 인터페이스 구축
description: UI (사용자 인터페이스) 컨트롤을 포함 하는 일반 사용자 환경 (UX) 검사 Xamarin.tvOS를 사용 하 여 작업 하는 경우 Xcode의 Interface Builder 및 UX 디자인 원칙을 사용 합니다.
ms.prod: xamarin
ms.assetid: 8CF80705-B36A-42D6-B66B-52BC8586FA5A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/07/2016
ms.openlocfilehash: a519af9e4bddb949b6e0547387d804f1437deb21
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104611"
---
# <a name="building-tvos-user-interfaces-with-xamarin"></a>TvOS Xamarin 사용 하 여 사용자 인터페이스 구축

_UI (사용자 인터페이스) 컨트롤을 포함 하는 일반 사용자 환경 (UX) 검사 Xamarin.tvOS를 사용 하 여 작업 하는 경우 Xcode의 Interface Builder 및 UX 디자인 원칙을 사용 합니다._

작업할 때 C# Objective-c 또는 Swift 및 Xcode에서 작업 하는 개발자가 동일한 사용자 인터페이스 컨트롤에 액세스할 수 있는 Xamarin 기반 tvOS에.NET 및. Xcode의 Interface Builder를 만들고 사용자 인터페이스를 유지 관리할 수 (또는 필요에 따라 직접 만들 C# 코드).

아래 나열 된 가이드 Xamarin.tvOS 앱에서 tvOS UI 요소를 사용 하는 방법에 대 한 자세한 정보를 제공 합니다. 것이 가장 좋습니다 진행 하는 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md) 주요 개념 및 모든 문서에서를 사용 하는 기술을 설명 하는 대로 먼저 합니다.

## <a name="working-with-alertsiostvosuser-interfacealertsmd"></a>[경고 사용](~/ios/tvos/user-interface/alerts.md)

이 문서에서는 사용 `UIAlertController` Xamarin.tvOS에서 사용자에 게 경고 메시지를 표시 합니다.

## <a name="working-with-buttonsiostvosuser-interfacebuttonsmd"></a>[단추를 사용 하 여 작업](~/ios/tvos/user-interface/buttons.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 단추를 사용 하 여 작업을 다룹니다.

## <a name="working-with-collection-viewsiostvosuser-interfacecollection-viewsmd"></a>[컬렉션 뷰 작업](~/ios/tvos/user-interface/collection-views.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 컬렉션 뷰를 사용 하 여 작업을 설명 합니다.

## <a name="working-with-navigation-barsiostvosuser-interfacenavigation-barsmd"></a>[탐색 모음을 사용 하 여 작업](~/ios/tvos/user-interface/navigation-bars.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 탐색 모음을 사용 하 여 작업을 다룹니다.

## <a name="working-with-page-controlsiostvosuser-interfacepage-controlsmd"></a>[페이지 컨트롤 사용](~/ios/tvos/user-interface/page-controls.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 페이지 컨트롤을 사용 하 여 작업을 다룹니다.

## <a name="working-with-progress-indicatorsiostvosuser-interfaceprogress-indicatorsmd"></a>[진행률 표시기를 사용 하 여 작업](~/ios/tvos/user-interface/progress-indicators.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 진행률 표시기를 사용 하 여 작업을 설명 합니다.

## <a name="working-with-segmented-controlsiostvosuser-interfacesegmented-controlsmd"></a>[분할 된 컨트롤 사용](~/ios/tvos/user-interface/segmented-controls.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 분할 된 컨트롤을 사용 하 여 작업을 다룹니다.

## <a name="working-with-split-view-controllersiostvosuser-interfacesplit-viewsmd"></a>[분할 뷰 컨트롤러를 사용 하 여 작업](~/ios/tvos/user-interface/split-views.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 분할 뷰 컨트롤러를 사용 하 여 작업을 설명 합니다.

## <a name="working-with-stack-viewsiostvosuser-interfacestacked-viewsmd"></a>[스택 뷰 작업](~/ios/tvos/user-interface/stacked-views.md)

이 문서에서는 디자인 및 스택 뷰 Xamarin.tvOS 앱 내에서 작업을 설명 합니다.

## <a name="working-with-tab-barsiostvosuser-interfacetab-barsmd"></a>[탭 표시줄 사용](~/ios/tvos/user-interface/tab-bars.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 탭 표시줄을 사용 하 여 작업을 다룹니다.

## <a name="working-with-table-viewsiostvosuser-interfacetable-viewsmd"></a>[테이블 뷰 작업](~/ios/tvos/user-interface/table-views.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 테이블 뷰 및 테이블 뷰 컨트롤러를 사용 하 여 작업을 설명 합니다.

## <a name="working-with-text-and-search-fieldsiostvosuser-interfacetext-fields-and-searchmd"></a>[텍스트 및 검색 필드를 사용 하 여 작업](~/ios/tvos/user-interface/text-fields-and-search.md)

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 텍스트 및 검색 필드를 사용 하 여 작업을 다룹니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
- [Xamarin (비디오)를 사용 하 여 tvOS에 대 한 앱 빌드](https://university.xamarin.com/lightninglectures/tvos-with-xamarin)
