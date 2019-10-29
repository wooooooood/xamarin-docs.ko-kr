---
title: RecyclerView 파트 및 기능
description: RecyclerView 레이아웃 관리자, 어댑터 및 보기 소유자의 개요입니다.
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: 823215b7cc1bac8a8f6bffc6e480400135e88ce8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028830"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView 파트 및 기능

`RecyclerView`는 내부적으로 일부 작업 (예: 뷰의 스크롤 및 재생)을 처리 하지만, 기본적으로 도우미 클래스를 조정 하 여 컬렉션을 표시 하는 관리자입니다. `RecyclerView`는 다음 도우미 클래스에 작업을 위임 합니다.

- **`Adapter`** &ndash; 늘어납니다 항목 레이아웃 (레이아웃 파일의 콘텐츠를 인스턴스화) 하 고 데이터를 `RecyclerView`내에 표시 되는 뷰에 바인딩합니다. 또한 어댑터는 항목 클릭 이벤트를 보고 합니다.

- &ndash; **`LayoutManager`** 는 `RecyclerView` 내에서 항목 보기를 측정 및 배치 하 고 뷰 재활용에 대 한 정책을 관리 합니다.

- **`ViewHolder`** &ndash; 조회 하 고 뷰 참조를 저장 합니다. 뷰 보유자는 항목 보기 클릭을 검색 하는 데도 도움이 됩니다.

- **`ItemDecoration`** &ndash;를 사용 하면 앱에서 항목, 강조 표시 및 시각적 그룹화 경계 사이의 분할자를 그리기 위해 특정 보기에 특수 그리기 및 레이아웃 오프셋을 추가할 수 있습니다.

- **`ItemAnimator`** &ndash;는 항목 작업 중에 발생 하거나 어댑터를 변경 하는 동안 발생 하는 애니메이션을 정의 합니다.

`RecyclerView`, `LayoutManager`및 `Adapter` 클래스 간의 관계는 다음 다이어그램에 나와 있습니다.

![어댑터를 사용 하 여 데이터 집합에 액세스 하는 LayoutManager를 포함 하는 RecyclerView의 다이어그램](parts-and-functionality-images/01-recyclerview-diagram.png)

이 그림에서 설명 하는 것 처럼 `LayoutManager`은 `Adapter`와 `RecyclerView`의 중개자로 간주할 수 있습니다. `LayoutManager`는 `RecyclerView`를 대신 하 여 `Adapter` 메서드를 호출 합니다. 예를 들어 `LayoutManager`는 `RecyclerView`에서 특정 항목 위치에 대 한 새 뷰를 만들 때 `Adapter` 메서드를 호출 합니다. `Adapter`는 해당 항목의 레이아웃을 늘어납니다 하 고 `ViewHolder` 인스턴스 (표시 되지 않음)를 만들어 해당 위치의 뷰에 대 한 참조를 캐시 합니다. `LayoutManager`에서 `Adapter`를 호출 하 여 특정 항목을 데이터 `Adapter` 집합에 바인딩하는 경우 해당 항목에 대 한 데이터를 찾아 데이터 집합에서 검색 한 다음 연결 된 항목 뷰로 복사 합니다.

앱에서 `RecyclerView`를 사용 하는 경우 다음 클래스의 파생 형식 만들기가 필요 합니다.

- **`RecyclerView.Adapter`** &ndash; 앱의 데이터 집합 (앱에만 해당)에서 `RecyclerView`내에 표시 되는 항목 뷰로의 바인딩을 제공 합니다. 어댑터는 `RecyclerView`의 각 항목 뷰 위치를 데이터 원본의 특정 위치에 연결 하는 방법을 알고 있습니다. 또한 어댑터는 각 개별 항목 보기 내에서 콘텐츠의 레이아웃을 처리 하 고 각 보기에 대 한 보기 홀더를 만듭니다. 또한 어댑터는 항목 보기에서 검색 된 항목 클릭 이벤트를 보고 합니다.

- **`RecyclerView.ViewHolder`** &ndash;는 리소스 조회가 불필요 하 게 반복 되지 않도록 항목 레이아웃 파일의 뷰에 대 한 참조를 캐시 합니다. 또한 보기 소유자는 사용자가 보기 소유자의 연결 된 항목 보기를 누를 때 어댑터에 전달 될 항목 클릭 이벤트를 정렬 합니다.

- `RecyclerView`내에서 항목 &ndash; 위치를 **`RecyclerView.LayoutManager`** 합니다. 미리 정의 된 여러 레이아웃 관리자 중 하나를 사용 하거나 고유한 사용자 지정 레이아웃 관리자를 구현할 수 있습니다.
    `RecyclerView` 레이아웃 정책을 레이아웃 관리자에 위임 하므로, 앱을 크게 변경할 필요 없이 다른 레이아웃 관리자에 연결할 수 있습니다.

또한 필요에 따라 다음 클래스를 확장 하 여 앱에서 `RecyclerView`의 모양과 느낌을 변경할 수 있습니다.

- **`RecyclerView.ItemDecoration`**
- **`RecyclerView.ItemAnimator`**

`ItemDecoration` 및 `ItemAnimator`를 확장 하지 않는 경우 `RecyclerView` 기본 구현을 사용 합니다. 이 가이드에서는 사용자 지정 `ItemDecoration` 및 `ItemAnimator` 클래스를 만드는 방법에 대해서는 설명 하지 않습니다. 이러한 클래스에 대 한 자세한 내용은 [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) 및 [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html)를 참조 하세요.

<a name="recycling" />

## <a name="how-view-recycling-works"></a>보기 재활용 작동 방법

`RecyclerView`는 데이터 원본의 모든 항목에 대해 항목 뷰를 할당 하지 않습니다. 대신 화면에 맞는 항목 보기의 수만 할당 하 고 사용자가 스크롤하면 해당 항목 레이아웃을 다시 사용 합니다. 뷰가 먼저 표시 되지 않는 경우 다음 그림에 설명 된 재생 프로세스를 진행 합니다.

[뷰 재활용의 6 단계를 보여 주는![다이어그램](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1. 뷰가 보이지 않게 스크롤될 때 더 이상 표시 되지 않으면 *스크랩 뷰가*됩니다.

2. 스크랩 보기는 풀에 배치 되 고 *재활용 뷰가*됩니다.
    이 풀은 동일한 형식의 데이터를 표시 하는 보기의 캐시입니다.

3. 새 항목이 표시 되 면 재활용 풀에서 뷰를 다시 사용 합니다. 이 뷰는 표시 되기 전에 어댑터에 의해 다시 바인딩되어야 하므로 *더티 보기*라고 합니다.

4. 더티 뷰가 재활용 됩니다. 어댑터는 표시 될 다음 항목에 대 한 데이터를 찾고이 항목에 대 한 보기에이 데이터를 복사 합니다. 이러한 보기에 대 한 참조는 재활용 된 뷰와 연결 된 뷰 보유자에서 검색 됩니다.

5. 화면에 표시 되는 `RecyclerView`의 항목 목록에 재활용 된 보기가 추가 됩니다.

6. 사용자가 `RecyclerView`를 목록의 다음 항목으로 스크롤하면 재활용 된 보기가 화면에 표시 됩니다. 한편, 다른 보기는 위 단계에 따라 표시 되지 않으며 재활용 됩니다.

항목 뷰 다시 사용 외에도 `RecyclerView` 다른 효율성 최적화를 사용 합니다. 뷰 소유자. *뷰 보유자* 는 뷰 참조를 캐시 하는 간단한 클래스입니다. 어댑터는 항목 레이아웃 파일을 늘어납니다 때마다 해당 하는 뷰 보유자를 만듭니다. 뷰 보유자는 `FindViewById`을 사용 하 여 팽창 된 항목 레이아웃 파일 내의 뷰에 대 한 참조를 가져옵니다. 이러한 참조는 새 데이터를 표시 하기 위해 레이아웃을 재활용할 때마다 새 데이터를 뷰에 로드 하는 데 사용 됩니다.

## <a name="the-layout-manager"></a>레이아웃 관리자

레이아웃 관리자는 `RecyclerView` 표시에서 항목의 위치를 지정 하는 일을 담당 합니다. 이는 프레젠테이션 유형 (목록 또는 그리드), 방향 (항목이 세로로 또는 가로로 표시 되는지 여부) 및 표시 되는 방향 항목 (보통 순서 또는 역순)을 결정 합니다. 또한 레이아웃 관리자는 **RecycleView** 디스플레이에서 각 항목의 크기와 위치를 계산 하는 일을 담당 합니다.

레이아웃 관리자는 추가 목적이 있습니다 .이는 사용자에 게 더 이상 표시 되지 않는 항목 보기를 재활용 하는 시기에 대 한 정책을 결정 합니다.
레이아웃 관리자는 표시 되는 보기와 표시 되지 않는 뷰를 인식 하므로 뷰를 재활용할 수 있는 시기를 결정 하는 것이 가장 좋습니다. 뷰를 재생 하기 위해 레이아웃 관리자는 일반적으로 [보기 재활용의 작동 방식](#recycling)에 설명 된 대로 일반적으로 어댑터를 호출 하 여 재활용 된 보기의 내용을 다른 데이터로 바꿉니다.

`RecyclerView.LayoutManager` 확장 하 여 사용자 고유의 레이아웃 관리자를 만들거나 미리 정의 된 레이아웃 관리자를 사용할 수 있습니다. `RecyclerView`는 다음과 같은 미리 정의 된 레이아웃 관리자를 제공 합니다.

- **`LinearLayoutManager`** &ndash; 세로로 스크롤할 수 있는 열 또는 가로로 스크롤할 수 있는 행의 항목을 정렬 합니다.

- &ndash; **`GridLayoutManager`** 표에 항목을 표시 합니다.

- **`StaggeredGridLayoutManager`** &ndash;는 지그재그형 표에 항목을 표시 합니다. 여기서 일부 항목의 높이와 너비는 다릅니다.

레이아웃 관리자를 지정 하려면 선택한 레이아웃 관리자를 인스턴스화하고 `SetLayoutManager` 메서드에 전달 합니다. 기본적으로 미리 정의 된 레이아웃 관리자를 선택 하지 `RecyclerView` &ndash; 레이아웃 관리자를 지정 *해야* 합니다.

레이아웃 관리자에 대 한 자세한 내용은 [RecyclerView manager 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html)를 참조 하세요.

## <a name="the-view-holder"></a>보기 홀더

뷰 보유자는 뷰 참조를 캐시 하기 위해 정의 하는 클래스입니다. 어댑터는 이러한 뷰 참조를 사용 하 여 각 뷰를 해당 내용에 바인딩합니다. `RecyclerView`의 모든 항목에는 해당 항목에 대 한 뷰 참조를 캐시 하는 연결 된 뷰 홀더 인스턴스가 있습니다. 뷰 홀더를 만들려면 다음 단계를 사용 하 여 항목당 정확한 뷰 집합을 보유할 클래스를 정의 합니다.

1. 서브 클래스 `RecyclerView.ViewHolder`입니다.
2. 뷰 참조를 조회 하 고 저장 하는 생성자를 구현 합니다.
3. 어댑터에서 이러한 참조에 액세스 하는 데 사용할 수 있는 속성을 구현 합니다.

`ViewHolder` 구현의 자세한 예는 [기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)에 나와 있습니다.
`RecyclerView.ViewHolder`에 대 한 자세한 내용은 [ViewHolder 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)를 참조 하세요.

## <a name="the-adapter"></a>어댑터

`RecyclerView` 통합 코드의 대부분은 어댑터에서 수행 됩니다. `RecyclerView` 하려면 `RecyclerView.Adapter`에서 파생 된 어댑터를 제공 하 여 데이터 원본에 액세스 하 고 각 항목을 데이터 원본의 콘텐츠로 채워야 합니다.
데이터 원본은 응용 프로그램 마다 다르기 때문에 데이터에 액세스 하는 방법을 이해 하는 어댑터 기능을 구현 해야 합니다. 어댑터는 데이터 원본에서 정보를 추출 하 여 `RecyclerView` 컬렉션의 각 항목에 로드 합니다.

다음 그림에서는 어댑터가 뷰 소유자를 통해 데이터 원본의 콘텐츠를 `RecyclerView`의 각 행 항목 내에 있는 개별 뷰에 매핑하는 방법을 보여 줍니다.

[ViewHolders에 데이터 원본을 연결 하는 어댑터를 보여 주는![다이어그램](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

어댑터는 특정 행 항목의 데이터를 사용 하 여 각 `RecyclerView` 행을 로드 합니다. 예를 들어 행 위치 *p*의 경우 어댑터는 데이터 원본 내의 위치 *p* 에서 연결 된 데이터를 찾고이 데이터를 `RecyclerView` 컬렉션의 *p* 위치에 있는 행 항목에 복사 합니다.
예를 들어 위의 그리기에서 어댑터는 보기 소유자를 사용 하 여 `ImageView`에 대 한 참조를 조회 하 고 해당 위치에서 `TextView` 사용자가 컬렉션을 스크롤하고 뷰를 다시 사용할 때 해당 뷰에 대해 `FindViewById`를 반복적으로 호출할 필요가 없습니다.

어댑터를 구현 하는 경우 다음 `RecyclerView.Adapter` 메서드를 재정의 해야 합니다.

- 항목 레이아웃 파일 및 보기 홀더를 인스턴스화하는 **`OnCreateViewHolder`** &ndash;입니다.

- **`OnBindViewHolder`** &ndash; 지정 된 위치에 있는 데이터를 참조가 지정 된 뷰 보유자에 저장 된 뷰로 로드 합니다.

- **`ItemCount`** &ndash;는 데이터 원본에 있는 항목 수를 반환 합니다.

레이아웃 관리자는 `RecyclerView`내에서 항목의 위치를 지정 하는 동안 이러한 메서드를 호출 합니다.

## <a name="notifying-recyclerview-of-data-changes"></a>RecyclerView 데이터 변경 알림

데이터 원본의 내용이 변경 되 면 `RecyclerView`에서 자동으로 표시를 업데이트 하지 않습니다. 어댑터는 데이터 집합이 변경 되 면 `RecyclerView`에 알려야 합니다. 데이터 집합은 여러 가지 방법으로 변경 될 수 있습니다. 예를 들어 항목 내의 내용이 변경 되거나 전체 데이터 구조가 변경 될 수 있습니다.
`RecyclerView.Adapter`은 `RecyclerView`가 가장 효율적인 방식으로 데이터 변경 내용에 응답 하도록 호출할 수 있는 여러 메서드를 제공 합니다.

- **`NotifyItemChanged`** &ndash; 지정 된 위치에 있는 항목이 변경 되었음을 신호로 보냅니다.

- **`NotifyItemRangeChanged`** &ndash; 지정 된 위치 범위의 항목이 변경 되었음을 신호로 보냅니다.

- **`NotifyItemInserted`** &ndash; 지정 된 위치의 항목이 새로 삽입 되었음을 신호로 알립니다.

- **`NotifyItemRangeInserted`** &ndash; 지정 된 위치 범위의 항목이 새로 삽입 되었음을 신호로 보냅니다.

- **`NotifyItemRemoved`** &ndash; 지정 된 위치에 있는 항목이 제거 되었음을 신호로 알립니다.

- **`NotifyItemRangeRemoved`** &ndash; 지정 된 위치 범위의 항목이 제거 되었음을 신호로 보냅니다.

- **`NotifyDataSetChanged`** &ndash; 데이터 집합이 변경 되었음을 신호로 보냅니다 (전체 업데이트 강제 적용).

데이터 집합이 변경 된 방식을 정확히 알고 있는 경우 위의 적절 한 메서드를 호출 하 여 가장 효율적인 방법으로 `RecyclerView`를 새로 고칠 수 있습니다. 데이터 집합이 변경 된 방식을 정확 하 게 알 수 없는 경우 `NotifyDataSetChanged`를 호출할 수 있습니다. `RecyclerView`는 사용자에 게 표시 되는 모든 보기를 새로 고쳐야 하기 때문에 훨씬 효율적입니다. 이러한 메서드에 대 한 자세한 내용은 [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)를 참조 하세요.

다음 항목인 [기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)에서는 위에 설명 된 파트 및 기능의 실제 코드 예제를 보여 주기 위해 예제 앱을 구현 합니다.

## <a name="related-links"></a>관련 링크

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
