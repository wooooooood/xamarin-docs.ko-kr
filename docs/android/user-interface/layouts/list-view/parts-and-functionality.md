---
title: ListView 파트 및 기능
ms.prod: xamarin
ms.assetid: ABA40FED-FF68-C0B0-BC43-C748CEE585D8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2017
ms.openlocfilehash: b8fd44a70f4c7ecdcf7919dec1c81461200b35bf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028861"
---
# <a name="xamarinandroid-listview-parts-and-functionality"></a>Xamarin Android ListView 파트 및 기능

`ListView`는 다음과 같은 부분으로 구성 됩니다.

- **행** 은 목록에 있는 데이터의 표시 된 표현을 &ndash; 합니다.

- **어댑터** &ndash; 데이터 소스를 목록 뷰에 바인딩하는 비시각적 클래스입니다.

- 사용자가 목록 길이를 스크롤할 수 있도록 하는 핸들 &ndash; **빠른 스크롤** 입니다.

- **섹션 인덱스** &ndash; 스크롤 행 위로 이동 하 여 현재 행을 목록에서 찾을 위치를 나타내는 사용자 인터페이스 요소입니다.

이러한 스크린샷은 기본적인 `ListView` 컨트롤을 사용 하 여 스크롤 및 섹션 인덱스의 렌더링 속도를 보여 줍니다.

[일반 이전 행, 빠른 스크롤 및 섹션 인덱스를 사용 하 여 앱의![스크린샷](parts-and-functionality-images/listviewparts.png)](parts-and-functionality-images/listviewparts.png#lightbox)

`ListView`를 구성 하는 요소에 대해서는 아래에서 자세히 설명 합니다.

## <a name="rows"></a>행

각 행에는 고유한 `View`있습니다. 뷰는 `Android.Resources`에 정의 된 기본 제공 뷰나 사용자 지정 보기 중 하나일 수 있습니다. 각 행은 동일한 뷰 레이아웃을 사용 하거나 모두 다를 수 있습니다. 이 문서에서는 기본 제공 레이아웃을 사용 하는 방법과 사용자 지정 레이아웃을 정의 하는 방법을 설명 하는 예제를 제공 합니다.

## <a name="adapter"></a>어댑터

`ListView` 컨트롤에는 각 행에 대해 서식이 지정 된 `View`를 제공 하는 `Adapter` 필요 합니다. Android에는 기본 제공 어댑터와 보기를 사용할 수 있거나 사용자 지정 클래스를 만들 수 있습니다.

## <a name="fast-scrolling"></a>빠른 스크롤

`ListView`에 많은 데이터 행이 포함 되어 있는 경우 사용자가 목록의 일부분으로 이동 하는 데 도움이 되도록 사용 하도록 설정할 수 있습니다. 빠른 스크롤 ' 스크롤 막대 '는 선택적으로 사용 하도록 설정할 수 있으며 API 수준 11 이상에서 사용자 지정할 수 있습니다.

## <a name="section-index"></a>섹션 인덱스

긴 목록을 스크롤할 때 선택적 섹션 인덱스는 사용자에 게 현재 보고 있는 목록 부분에 대 한 피드백을 제공 합니다. 일반적으로 빠른 스크롤과 함께 긴 목록에만 적합 합니다.

## <a name="classes-overview"></a>클래스 개요

`ListViews`를 표시 하는 데 사용 되는 기본 클래스는 다음과 같습니다.

[ListView와 관련 클래스 간의 관계를 보여 주는 UML 다이어그램![](parts-and-functionality-images/image2.png)](parts-and-functionality-images/image2.png#lightbox)

각 클래스의 용도는 아래에 설명 되어 있습니다.

- **ListView** &ndash; 스크롤 가능한 행 컬렉션을 표시 하는 사용자 인터페이스 요소입니다. 휴대폰에서는 일반적으로 전체 화면을 사용 합니다 .이 경우에는 `ListActivity` 클래스를 사용할 수 있습니다. 또는 휴대폰 이나 태블릿 장치에서 더 큰 레이아웃의 일부일 수 있습니다.

- Android에서 **보기 &ndash; 보기** 는 모든 사용자 인터페이스 요소가 될 수 있지만 `ListView` 컨텍스트에서는 각 행에 `View`를 제공 해야 합니다.

- `ListView`를 데이터 원본에 바인딩하는 어댑터 구현에 대 한 **baseadapter** &ndash; 기본 클래스입니다.

- **Arrayadapter** &ndash; 기본 제공 어댑터 클래스는 문자열 배열을 표시 하기 위해 `ListView`에 바인딩합니다. 제네릭 `ArrayAdapter<T>`는 다른 형식에 대해서도 동일 하 게 수행 됩니다.

- **CursorAdapter** &ndash; `CursorAdapter` 또는 `SimpleCursorAdapter`를 사용 하 여 SQLite 쿼리를 기반으로 데이터를 표시 합니다.

이 문서에는 `ArrayAdapter`를 사용 하는 간단한 예제 뿐만 아니라 `BaseAdapter` 또는 `CursorAdapter`의 사용자 지정 구현이 필요한 복잡 한 예제가 포함 되어 있습니다.
