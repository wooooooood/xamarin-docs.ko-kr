---
title: ListView 파트 및 기능
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: ec9b6583cac9478957e14f709c7c7ee8eb273cbc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763984"
---
# <a name="listview-parts-and-functionality"></a>ListView 파트 및 기능


## <a name="overview"></a>개요

A `ListView` 다음과 같은 부분으로 구성 됩니다.

- **행** &ndash; 목록에서 데이터의 표시 표현 합니다.

- **어댑터** &ndash; 목록 보기를 데이터 소스에 바인딩하는 비시각적 클래스입니다.

- **빠른 스크롤** &ndash; 사용자가 목록의 길이 스크롤할 수 있는 핸들입니다.

- **인덱스 섹션** &ndash; 스크롤 위에 배치 되는 사용자 인터페이스 요소를 목록에서 현재 행 있는 위치를 나타내는 행 합니다.

다음이 스크린샷에서 기본 사용 하 여 `ListView` 컨트롤을 빨리 스크롤 및 섹션 인덱스 렌더링 하는 방법을 보여 줍니다.

[![오래 된 일반 행을 사용 하 여 앱의 스크린 샷을 빠른 스크롤 및 섹션 인덱스](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

구성 하는 요소는 `ListView` 아래에서 자세히 설명 합니다.


## <a name="rows"></a>행

각 행에는 자체 `View`합니다. 보기에 정의 된 기본 제공 뷰 중 하나일 수 있습니다 `Android.Resources`, 또는 사용자 지정 보기. 각 행 동일한 보기 레이아웃을 사용할 수 또는 수 모두 다를 수 있습니다. 기본 제공 레이아웃 및 사용자 지정 레이아웃을 정의 하는 방법을 설명 하는 다른 사용자를 사용 하 여이 문서에은 예입니다.


## <a name="adapter"></a>어댑터

`ListView` 컨트롤을 사용 하려면는 `Adapter` 는 형식이 지정 된 제공 `View` 각 행에 대 한 합니다. Android에 기본 제공 어댑터 및 사용할 수 있는 뷰 또는 사용자 지정 클래스를 만들 수 있습니다.


## <a name="fast-scrolling"></a>빠른 스크롤

경우는 `ListView` 많은 행이 포함 되어 데이터의 빠른 스크롤 하도록 할 수 있습니다는 목록의 모든 부분으로 이동 하는 사용자입니다. Fast-스크롤 '스크롤 막대' 필요에 따라 사용 하도록 설정 (및 사용자 지정 API 수준 11에서에서 되 고 더 높은) 수 있습니다.


## <a name="section-index"></a>섹션 인덱스

긴 목록을 통해 스크롤 하는 동안 선택적 섹션 인덱스 목록의 어떤 부분이 현재 보고에 사용자에 게 피드백을 제공 합니다. 일반적으로 빠른 스크롤와 함께에 긴 목록에서 적절 한 됩니다.


## <a name="classes-overview"></a>클래스 개요

표시 하는 데 사용 되는 기본 클래스 `ListViews` 여기에 표시 됩니다.

[![ListView 및 관련된 클래스 간의 관계를 보여 주는 UML 다이어그램](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

다음은 각 클래스의 용도 대 한 설명입니다.

- **ListView** &ndash; 행의 스크롤 가능한 컬렉션을 표시 하는 사용자 인터페이스 요소입니다. 에 phone 것 일반적으로 전체 화면을 사용 하 여 (이 경우에 `ListActivity` 클래스를 사용할 수 있습니다) 또는 휴대폰 또는 태블릿 장치에서 큰 레이아웃의 일부가 될 수 없습니다.

- **보기** &ndash; Android에서 보기는 모든 사용자 인터페이스 요소 수의 맥락에서는 `ListView` 필요는 `View` 를 각 행에 대해 제공할 수 있습니다.

- **BaseAdapter** &ndash; 바인딩할 어댑터 구현 하기 위한 기본 클래스는 `ListView` 데이터 원본에 있습니다.

- **ArrayAdapter** &ndash; 문자열의 배열에 바인딩하는 기본 제공 어댑터 클래스는 `ListView` 표시 합니다. 제네릭 `ArrayAdapter<T>` 다른 형식에 대해 동일한 작업을 수행 합니다.

- **CursorAdapter** &ndash; 사용 `CursorAdapter` 또는 `SimpleCursorAdapter` SQLite 쿼리를 기반으로 데이터를 표시 합니다.

이 문서를 사용 하는 간단한 예를 들어는 `ArrayAdapter` 의 사용자 지정 구현이 필요로 하는 더 복잡 한 예제 뿐만 아니라 `BaseAdapter` 또는 `CursorAdapter`합니다.

