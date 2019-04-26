---
title: ListView 파트 및 기능
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: 248baa5daceff6db01098a155600ea204547e845
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61319746"
---
# <a name="listview-parts-and-functionality"></a>ListView 파트 및 기능


## <a name="overview"></a>개요

`ListView` 다음 부분으로 구성 됩니다.

- **행** &ndash; 표시 목록에서 데이터 표현입니다.

- **어댑터** &ndash; 목록 뷰의 데이터 원본에 바인딩되는 비 가시적 클래스입니다.

- **빠른 스크롤** &ndash; 목록의 길이 스크롤할 수 있는 핸들입니다.

- **인덱스 섹션** &ndash; 스크롤 위에 떠 있는 사용자 인터페이스 요소를 나타내는 목록에서 현재 행 있는 행입니다.

이러한 스크린샷은 기본 사용 `ListView` 컨트롤을 빠른 스크롤 및 섹션 인덱스 렌더링 하는 방법을 보여 줍니다.

[![오래 된 일반 행을 사용 하 여 앱의 스크린 샷을 빠른 스크롤 및 섹션 인덱스](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

구성 하는 요소는 `ListView` 아래에서 자세히 설명 되어 있습니다.


## <a name="rows"></a>행

각 행에는 자체 `View`합니다. 보기에 정의 된 기본 제공 뷰 중 하나가 될 수 있습니다 `Android.Resources`, 또는 사용자 지정 보기. 각 행 동일한 보기 레이아웃 사용 하거나 이러한 모든 다를 수 있습니다. 기본 제공 레이아웃 및 사용자 지정 레이아웃을 정의 하는 방법을 설명 하는 다른 사용자를 사용 하 여이 문서의 예제 있습니다.


## <a name="adapter"></a>어댑터

합니다 `ListView` 컨트롤에 필요한를 `Adapter` 형식이 지정 된 제공 `View` 각 행에 대 한 합니다. Android에 기본 제공 어댑터 및 사용할 수 있는 뷰 또는 사용자 지정 클래스를 만들 수 있습니다.


## <a name="fast-scrolling"></a>빠른 스크롤

경우는 `ListView` 많은 행이 포함 데이터의 빠른 스크롤 가능 사용자 목록의 일부가로 이동 하는 데 도움이 됩니다. Fast-스크롤 '스크롤 막대' 필요에 따라 사용 하도록 설정 (API 레벨 11에서에서 사용자 지정 및 더 높은) 수 있습니다.


## <a name="section-index"></a>구간 인덱스

긴 목록을 통해 스크롤 하는 동안 선택적인 섹션 인덱스는 목록 부분 현재 보고에 피드백을 사용 하 여 사용자를 제공 합니다. 또한 빠른 스크롤와 함께에서 일반적으로 긴 목록에만 적합합니다.


## <a name="classes-overview"></a>클래스 개요

기본 클래스를 표시 하는 데 `ListViews` 여기에 표시 됩니다.

[![ListView 및 관련된 클래스 간의 관계를 보여 주는 UML 다이어그램](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

각 클래스의 용도 대 한 설명은 다음과 같습니다.

- **ListView** &ndash; 행의 스크롤할 수 있는 컬렉션을 표시 하는 사용자 인터페이스 요소입니다. 에 휴대폰이 일반적으로 전체 화면을 사용 하 여 (경우에를 `ListActivity` 클래스를 사용할 수 있습니다) 또는 휴대폰 또는 태블릿 장치에서 더 큰 레이아웃의 일부일 수 없습니다.

- **뷰** &ndash; Android에서 뷰를 모든 사용자 인터페이스 요소 수의 맥락에서는 `ListView` 필요를 `View` 각 행에 대해 제공 해야 합니다.

- **BaseAdapter** &ndash; 바인딩할 어댑터 구현 위한 기본 클래스를 `ListView` 데이터 원본에 있습니다.

- **ArrayAdapter** &ndash; 문자열의 배열에 바인딩하는 기본 제공 어댑터 클래스를 `ListView` 표시 합니다. 제네릭 `ArrayAdapter<T>` 다른 형식에 대해 동일한 작업을 수행 합니다.

- **CursorAdapter** &ndash; 사용 하 여 `CursorAdapter` 또는 `SimpleCursorAdapter` SQLite 쿼리를 기반으로 데이터를 표시 합니다.

이 문서에 사용 하는 간단한 예제는 `ArrayAdapter` 의 사용자 지정 구현이 필요로 하는 더 복잡 한 예제 뿐만 아니라 `BaseAdapter` 또는 `CursorAdapter`합니다.

