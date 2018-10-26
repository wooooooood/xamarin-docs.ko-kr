---
title: RecyclerView 파트 및 기능
description: 개요 RecyclerView 레이아웃 관리자, 어댑터 및 뷰의 소유자입니다.
ms.prod: xamarin
ms.assetid: 54F999BE-2732-4BC7-A466-D17373961C48
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: 13678d3b1bca102e6f608ad1c11838db1f14cd08
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105001"
---
# <a name="recyclerview-parts-and-functionality"></a>RecyclerView 파트 및 기능


`RecyclerView` 하지만 내부적으로 (예: 스크롤 뷰 재활용), 일부 작업 처리는 기본적으로 컬렉션을 표시 하기 위한 도우미 클래스를 조정 하는 관리자. `RecyclerView` 다음 도우미 클래스를 작업을 위임 합니다.

-   **`Adapter`** &ndash; 항목 레이아웃 확장 (레이아웃 파일의 내용을 인스턴스화하) 내에서 표시 되는 보기에 데이터 바인딩합니다는 `RecyclerView`합니다. 어댑터는 또한 항목 클릭 이벤트를 보고합니다.

-   **`LayoutManager`** &ndash; 측정 하 고 배치 내에서 항목 보기를 `RecyclerView` 보기 재생에 대 한 정책을 관리 하 고 있습니다.

-   **`ViewHolder`** &ndash; 조회 하 고 뷰 참조를 저장 합니다. 뷰 소유자에도 도움이 됩니다 항목 보기 클릭을 검색 합니다.

-   **`ItemDecoration`** &ndash; 항목을 강조 표시, visual 그룹화 경계 사이의 구분선을 그리기에 대 한 특정 보기에 특별 한 그리기 및 레이아웃 오프셋을 추가 하는 앱을 허용 합니다.

-   **`ItemAnimator`** &ndash; 항목 작업 중 수행 하거나 변경 내용으로 어댑터에 적용 하는 애니메이션을 정의 합니다.

간의 관계는 `RecyclerView`, `LayoutManager`, 및 `Adapter` 클래스는 다음 다이어그램에 표시 됩니다.

![데이터 집합에 액세스 하려면 어댑터를 사용 하 여 LayoutManager 포함 RecyclerView 다이어그램](parts-and-functionality-images/01-recyclerview-diagram.png)

이 그림에서 알 수 있듯이, 합니다 `LayoutManager` 간의 중개자 생각할 수 있습니다 합니다 `Adapter` 및 `RecyclerView`합니다. 합니다 `LayoutManager` 를 호출 하는 `Adapter` 위임자 메서드는 `RecyclerView`합니다. 예를 들어, 합니다 `LayoutManager` 호출을 `Adapter` 메서드는 특정 항목 위치에 대 한 새 뷰를 만들 때는 `RecyclerView`. 합니다 `Adapter` 해당 항목에 대 한 레이아웃을 확장 하 고 만듭니다는 `ViewHolder` 인스턴스 (표시 되지 않음) 해당 위치에서 보기에 대 한 캐시 참조 합니다. 경우는 `LayoutManager` 호출을 `Adapter` 특정 항목을 데이터 집합에 바인딩할는 `Adapter` 해당 항목에 대 한 데이터를 찾고, 데이터 집합에서 데이터를 검색 하 고 관련된 항목 보기에 복사 합니다.

사용 하는 경우 `RecyclerView` 앱에 다음 클래스의 파생된 형식 만들기가 필요 합니다.

-   **`RecyclerView.Adapter`** &ndash; 항목 보기 내에서 표시 되는 앱의 데이터 집합 (앱에 특정 됨)에서 바인딩을 제공 합니다 `RecyclerView`합니다. 어댑터에서 각 항목 뷰 위치를 연결 하는 방법을 알고는 `RecyclerView` 데이터 원본의 특정 위치에 있습니다. 또한 어댑터는 각 개별 항목 보기 내에서 내용의 레이아웃을 처리 하 고 각 보기에 대 한 뷰 소유자를 만듭니다. 어댑터는 또한 항목 보기에서 검색 되는 항목 클릭 이벤트를 보고 합니다.

-   **`RecyclerView.ViewHolder`** &ndash; 리소스 조회 불필요 하 게 반복 되지 않도록 항목 레이아웃 파일의 보기에 대 한 참조를 캐시 합니다. 뷰 소유자는 또한 사용자가 보기 자리 표시자의 관련된 항목 보기를 탭 하면 어댑터에 전달 될 항목 클릭 이벤트에 대 한 정렬 합니다.

-   **`RecyclerView.LayoutManager`** &ndash; 내에서 항목을 배치 합니다 `RecyclerView`합니다. 몇 가지 미리 정의 된 레이아웃 관리자 중 하나를 사용 하거나 사용자 고유의 사용자 지정 레이아웃 관리자를 구현할 수 있습니다.
    `RecyclerView` 대리자 레이아웃 관리자, 중요 하지 않고도 다른 레이아웃 관리자에 연결할 수 있도록 하는 레이아웃 정책을 앱에 변경 됩니다.

다음 클래스의 모양과 느낌을 변경 하려면 필요에 따라 확장할 수는 또한 `RecyclerView` 앱에서:

-   **`RecyclerView.ItemDecoration`**
-   **`RecyclerView.ItemAnimator`**

확장 하지 않는 경우 `ItemDecoration` 하 고 `ItemAnimator`, `RecyclerView` 에서는 기본 구현입니다. 이 가이드는 사용자 지정을 만드는 방법을 설명 하지 않습니다 `ItemDecoration` 하 고 `ItemAnimator` 클래스를 이러한 클래스에 대 한 자세한 내용은 참조 하세요. [RecyclerView.ItemDecoration](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemDecoration.html) 고 [RecyclerView.ItemAnimator](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ItemAnimator.html).


<a name="recycling" />

### <a name="how-view-recycling-works"></a>Works 재활용을 보는 방법

`RecyclerView` 데이터 원본에 있는 모든 항목에 대 한 항목 보기를 할당 하지 않습니다. 대신, 화면에 맞게 항목 보기의 수만 할당 하 고 사용자 스크롤합니다도 해당 항목 레이아웃 다시 사용 합니다. 뷰를 보이지 않도록 먼저 스크롤하면 다음 그림에 나와 있는 재활용 프로세스를 거칩니다.

[![보기 재활용의 6 단계를 보여 주는 다이어그램](parts-and-functionality-images/02-view-recycling-sml.png)](parts-and-functionality-images/02-view-recycling.png#lightbox)

1.  이 뷰를 눈앞에서 스크롤을 더 이상 표시 됩니다는 *뷰를 삭제*합니다.

2.  스크랩 보기 풀에 배치 되 고이 *뷰를 재활용*합니다.
    이 풀에는 동일한 형식의 데이터를 표시 하는 보기의 캐시입니다.

3.  새 항목을 표시할 때 뷰를 다시 사용할 수 있도록 재활용 풀에서 가져옵니다. 이 보기 다시 바인딩해야 어댑터가 표시 하기 전에, 때문에 호출을 *부적절 한 보기*.

4.  커밋되지 않은 뷰는 재활용: 어댑터를 표시 하려면 다음 항목에 대 한 데이터를 찾아서이 항목에 대 한 보기에이 데이터를 복사 합니다. 이러한 보기에 대 한 참조는 재활용된 뷰와 연결 된 보기 소유자에서 검색 됩니다.

5.  재활용된 보기에서 항목의 목록에 추가 됩니다는 `RecyclerView` 화면의 이동 하려고 하는 합니다.

6.  재활용된 보기까지 스크롤하면으로 화면의 이동 된 `RecyclerView` 목록에서 다음 항목으로 합니다. 한편 다른 보기 보이지 않도록 스크롤합니다 이며 위의 단계에 따라 재순환 됩니다.

항목 보기 재사용 하는 것 외에도 `RecyclerView` 또한 다른 효율성 최적화를 사용 하 여: 소유자를 확인 합니다. A *뷰 소유자* 캐시 참조 보기는 간단한 클래스입니다. 어댑터 확장 항목 레이아웃 파일을 때마다 또한는 해당 뷰 소유자를 만듭니다. 뷰 소유자를 사용 하 여 `FindViewById` 배포용된 항목 레이아웃 파일 내에서 보기에 대 한 참조를 가져오려고 합니다. 이러한 참조는 레이아웃은 새 데이터를 표시할 재활용 될 때마다 새 데이터 보기를 로드 하려면 사용 됩니다.
 


### <a name="the-layout-manager"></a>레이아웃 관리자

레이아웃 관리자에서 항목 위치 지정 담당 합니다 `RecyclerView` 표시; 프레젠테이션 형식 (목록 또는 그리드), (항목이 표시 되는지 여부 가로 또는 세로로)는 방향 및 방향 항목을 표시할지 결정 (일반 순서 또는 반대 순서로). 레이아웃 관리자는 크기와 각 항목의 위치를 계산 하는 일을 담당 합니다 **RecycleView** 표시 합니다.

레이아웃 관리자 용도가 추가: 항목 보기 더 이상 사용자에 게 표시 되지 않는 재활용 하는 경우에 대 한 정책을 결정 합니다.
레이아웃 관리자 있는 뷰를 표시 합니다 (및 되지 않는) 인지, 이므로 뷰를 재활용할 수를 결정할 수 있는 최상의 위치에 있습니다. 뷰를 재순환 할 레이아웃 관리자 일반적으로 호출 하는 어댑터 재활용된 보기의 내용을 서로 다른 데이터를 바꾸려면 앞에서 설명한 대로 [재활용 작동 방식 보기](#recycling)합니다.

확장할 수 있습니다 `RecyclerView.LayoutManager` 레이아웃을 만들려면 관리자 하거나 사용할 수 있습니다 하는 미리 정의 된 레이아웃 관리자. `RecyclerView` 다음 미리 정의 된 레이아웃 관리자를 제공합니다.

-   **`LinearLayoutManager`** &ndash; 가로 방향으로 스크롤할 수 있는 행 또는 열에서 세로로 스크롤할 수 있는 항목을 정렬 합니다.

-   **`GridLayoutManager`** &ndash; 표에 항목을 표시합니다.

-   **`StaggeredGridLayoutManager`** &ndash; 다른 높이 및 너비에 일부 항목이 있는 전환이 표의 항목을 표시 합니다.

레이아웃 관리자를 지정 하려면 선택한 레이아웃 상사 인스턴스화하고에 전달 된 `SetLayoutManager` 메서드. 참고 했는지 *해야 합니다* 레이아웃 관리자를 지정 &ndash; `RecyclerView` 기본적으로 미리 정의 된 레이아웃 관리자를 선택 하지 않습니다.

레이아웃 관리자에 대 한 자세한 내용은 참조는 [RecyclerView.LayoutManager 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html)합니다.


### <a name="the-view-holder"></a>뷰 소유자

뷰 소유자는 뷰 참조를 캐시에 대 한 정의 하는 클래스입니다. 어댑터는 각 뷰에 해당 콘텐츠를 바인딩할 이러한 뷰 참조를 사용 합니다. 에 있는 모든 항목을 `RecyclerView` 인스턴스가 관련 된 보기 소유자는 항목에 대 한 뷰 참조를 캐시 하는 합니다. 뷰 소유자를 만들려면 정확한 항목당 뷰 집합이 포함 된 클래스를 정의 하려면 다음 단계를 사용 합니다.

1.  서브 클래스 `RecyclerView.ViewHolder`합니다.
2.  조회 하 고 보기 파일에 대 한 참조를 저장 하는 생성자를 구현 합니다.
3.  어댑터는 이러한 참조에 액세스 하는 데 사용할 수 있는 속성을 구현 합니다.

자세한 예는 `ViewHolder` 구현에 표시 됩니다 [는 기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)합니다.
에 대 한 자세한 내용은 `RecyclerView.ViewHolder`를 참조 합니다 [RecyclerView.ViewHolder 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)합니다.


### <a name="the-adapter"></a>어댑터

"힘든"의 대부분은 `RecyclerView` 통합 코드는 어댑터에서 수행 합니다. `RecyclerView` 파생 된 어댑터를 제공 해야 `RecyclerView.Adapter` 를 데이터 원본에 액세스할 데이터 원본에서 콘텐츠를 사용 하 여 각 항목을 채웁니다.
앱 별 데이터 원본 이므로 데이터에 액세스 하는 방법을 이해 하는 어댑터 기능을 구현 해야 합니다. 어댑터가 데이터 소스에서 정보를 추출 및 각 항목에 로드 된 `RecyclerView` 컬렉션입니다.

다음 그림과 어댑터의 각 행 항목 내에서 개별 뷰를 뷰의 소유자를 통해 데이터 원본에서 콘텐츠를 매핑하는 방법의 `RecyclerView`:

[![ViewHolders에 데이터 원본을 연결 하는 어댑터를 보여 주는 다이어그램](parts-and-functionality-images/03-recyclerviewer-adapter-sml.png)](parts-and-functionality-images/03-recyclerviewer-adapter.png#lightbox)

각 어댑터 로드 `RecyclerView` 특정 행 항목에 대 한 데이터가 포함 된 행입니다. 행 위치에 대 한 *P*, 어댑터 위치에 연결 된 데이터를 찾는 예를 들어 *P* 위치의 항목 행에이 데이터 내에서 데이터 원본 및 복사본 *P* 에 `RecyclerView` 컬렉션입니다.
위의 그림에서 예를 들어, 어댑터를 사용 하 여 뷰 소유자에 대 한 참조를 조회 합니다 `ImageView` 및 `TextView` 반복적으로 호출 하지 않아도 되므로 해당 위치에서 `FindViewById` 사용자로 이러한 보기에 대 한 스크롤 하는 컬렉션 및 뷰를 다시 사용합니다.

다음 어댑터를 구현할 때 재정의 해야 `RecyclerView.Adapter` 메서드:

-   **`OnCreateViewHolder`** &ndash; 항목 레이아웃 파일 및 뷰의 소유자를 인스턴스화합니다.

-   **`OnBindViewHolder`** &ndash; 지정 된 보기 소유자에 해당 참조가 저장 되는 뷰를 지정된 된 위치에 데이터를 로드 합니다.

-   **`ItemCount`** &ndash; 데이터 소스의 항목 수를 반환합니다.

레이아웃 관리자 내에서 항목을 배치 하는 동안 이러한 메서드를 호출 합니다 `RecyclerView`합니다. 



### <a name="notifying-recyclerview-of-data-changes"></a>데이터 변경 내용 알림 RecyclerView

`RecyclerView` 자동으로 표시를 업데이트 하지 해당 데이터의 내용을 소스 변경 하는 경우 어댑터에 알려야 `RecyclerView` 데이터 집합의 변경 내용이 있을 때. 여러 가지 방법으로;에서 데이터 집합을 변경할 수 있습니다. 예를 들어 항목 내에서 내용을 변경 하거나 데이터의 전체 구조를 변경할 수 있습니다.
`RecyclerView.Adapter` 다양 한 호출할 수 있는 방법 제공 하도록 `RecyclerView` 가장 효율적인 방식으로 데이터 변경에 응답 합니다.

-  **`NotifyItemChanged`** &ndash; 지정된 된 위치에 항목을 변경 하는 신호를 보냅니다.

-  **`NotifyItemRangeChanged`** &ndash; 위치의 지정된 된 범위에 있는 항목 변경 된 신호를 보냅니다.

-  **`NotifyItemInserted`** &ndash; 지정된 된 위치에 있는 항목에 새로 삽입 된 신호를 보냅니다.

-  **`NotifyItemRangeInserted`** &ndash; 위치의 지정된 된 범위에서 항목을 새로 삽입 된 신호를 보냅니다.

-  **`NotifyItemRemoved`** &ndash; 지정된 된 위치에서 항목 제거 되었음을 알립니다.

-  **`NotifyItemRangeRemoved`** &ndash; 위치의 지정된 된 범위에서 항목이 제거 되었는지는 신호를 보냅니다.

-  **`NotifyDataSetChanged`** &ndash; 데이터 집합에서 변경 된 신호 (전체 업데이트를 강제로).

데이터 집합에 변경 하는 방법을 정확히 알고 있는 경우 새로 고치려면 위의 적절 한 메서드를 호출할 수 있습니다 `RecyclerView` 가장 효율적인 방식입니다. 데이터 집합에 변경 하는 방법에 정확 하 게 잘 모르는 경우 호출할 수 있습니다 `NotifyDataSetChanged`, 훨씬 더 효율적 있으므로 `RecyclerView` 사용자에 게 표시 되는 모든 보기를 새로 고쳐야 합니다. 이러한 방법에 대 한 자세한 내용은 참조 하세요. [RecyclerView.Adapter](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)합니다.

다음 항목인 [는 기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md), 앱의 예는 위에 설명 된 특징과 파트의 실제 코드 예제를 보여 주기 위해 구현 됩니다.


## <a name="related-links"></a>관련 링크

- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
