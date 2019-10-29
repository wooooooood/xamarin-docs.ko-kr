---
title: RecyclerView
description: RecyclerView는 컬렉션을 표시 하기 위한 뷰 그룹입니다. ListView 및 GridView와 같은 오래 된 뷰 그룹을 보다 유연 하 게 바꿀 수 있도록 설계 되었습니다.  이 가이드에서는 Xamarin Android 응용 프로그램에서 RecyclerView를 사용 하 고 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/03/2018
ms.openlocfilehash: bce89be8bec512ac70ca40015521c7d56f3460d3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028807"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView는 컬렉션을 표시 하기 위한 뷰 그룹입니다. ListView 및 GridView와 같은 오래 된 뷰 그룹을 보다 유연 하 게 바꿀 수 있도록 설계 되었습니다.  이 가이드에서는 Xamarin Android 응용 프로그램에서 RecyclerView를 사용 하 고 사용자 지정 하는 방법을 설명 합니다._

## <a name="recyclerview"></a>RecyclerView

많은 앱이 동일한 유형 (예: 메시지, 연락처, 이미지 또는 노래)의 컬렉션을 표시 해야 합니다. 이 컬렉션이 너무 커서 화면에 맞지 않는 경우가 많으므로 컬렉션의 모든 항목을 매끄럽게 스크롤할 수 있는 작은 창에 컬렉션이 표시 됩니다.
`RecyclerView` 목록 또는 표에 항목 컬렉션을 표시 하 여 사용자가 컬렉션을 스크롤할 수 있도록 하는 Android 위젯입니다. 다음은 `RecyclerView`를 사용 하 여 전자 메일 받은 편지함 내용을 세로 스크롤 목록으로 표시 하는 예제 앱의 스크린샷입니다.

[RecyclerView를 사용 하 여 수신함 메시지를 나열 하는 예제 앱![](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView`는 다음과 같은 두 가지 강력한 기능을 제공 합니다.

- 기본 구성 요소를 연결 하 여 해당 동작을 수정할 수 있는 유연한 아키텍처가 있습니다.

- 항목 뷰를 다시 사용 하 고 뷰 *소유자* 를 사용 하 여 뷰 참조를 캐시 하기 때문에 규모가 많은 컬렉션이 효율적입니다.

이 가이드에서는 Xamarin Android 응용 프로그램에서 `RecyclerView`를 사용 하는 방법을 설명 합니다. Xamarin Android 프로젝트에 `RecyclerView` 패키지를 추가 하는 방법에 대해 설명 하 고 일반적인 응용 프로그램에서 `RecyclerView` 기능을 설명 합니다. 응용 프로그램에 `RecyclerView`를 통합 하는 방법, 항목-뷰 클릭을 구현 하는 방법 및 기본 데이터가 변경 될 때 `RecyclerView`를 새로 고치는 방법을 보여 주는 실제 코드 예제가 제공 됩니다. 이 가이드에서는 사용자가 Xamarin Android 개발에 대해 잘 알고 있다고 가정 합니다.

### <a name="requirements"></a>요구 사항

`RecyclerView`는 Android 5.0 롤리팝과 연결 되는 경우가 많지만 API 수준 7 (Android 2.1) 이상을 대상으로 하는 앱에서 작동 하는 `RecyclerView` &ndash; 지원 라이브러리로 제공 됩니다. Xamarin 기반 응용 프로그램에서 `RecyclerView`를 사용 하려면 다음이 필요 합니다.

- Visual Studio 또는 Mac용 Visual Studio를 사용 하 여 **xamarin.ios &ndash; xamarin. android 4.20** 이상 버전을 설치 하 고 구성 해야 합니다.

- 앱 프로젝트는 **RecyclerView** 패키지를 포함 해야 합니다. NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 [연습: 프로젝트에 Nuget 포함](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)을 참조 하세요.

### <a name="overview"></a>개요

`RecyclerView`은 Android에서 `ListView` 및 `GridView` 위젯의 대체로 간주할 수 있습니다. 선행 작업과 마찬가지로 `RecyclerView` 작은 창에 많은 데이터 집합을 표시 하도록 설계 되었지만 `RecyclerView`는 레이아웃 옵션을 더 많이 제공 하 고 많은 컬렉션을 표시 하는 데 최적화 되어 있습니다. `ListView`에 대해 잘 알고 있는 경우 `ListView`와 `RecyclerView`간에는 몇 가지 중요 한 차이점이 있습니다.

- `RecyclerView`를 사용 하는 것은 약간 더 복잡 합니다. `ListView`와 비교 하 여 `RecyclerView`를 사용 하려면 더 많은 코드를 작성 해야 합니다.

- `RecyclerView`는 미리 정의 된 어댑터를 제공 하지 않습니다. 데이터 원본에 액세스 하는 어댑터 코드를 구현 해야 합니다. 그러나 Android에는 `ListView` 및 `GridView`에서 작동 하는 몇 가지 미리 정의 된 어댑터가 포함 되어 있습니다.

- 사용자가 항목을 탭 할 때 항목 클릭 이벤트를 제공 하지 `RecyclerView` 대신, 항목 클릭 이벤트는 도우미 클래스에서 처리 됩니다. 반면 `ListView`는 항목 클릭 이벤트를 제공 합니다.

- `RecyclerView` 뷰를 재생 하 고, 뷰 소유자 패턴을 적용 하 여 성능을 향상 시킵니다 .이는 불필요 한 레이아웃 리소스 조회를 제거 합니다. `ListView`에서 뷰 소유자 패턴의 사용은 선택 사항입니다.

- `RecyclerView`은 사용자 지정을 용이 하 게 하는 모듈식 디자인을 기반으로 합니다. 예를 들어 응용 프로그램에 중요 한 코드를 변경 하지 않고도 다른 레이아웃 정책을 연결할 수 있습니다.
    이와 대조적으로 `ListView`는 구조적으로 상대적으로 모놀리식.

- `RecyclerView`는 항목 추가 및 제거에 대 한 기본 제공 애니메이션을 포함 합니다. `ListView` 애니메이션에는 앱 개발자의 일부에 대 한 몇 가지 추가 노력이 필요 합니다.

### <a name="sections"></a>섹션

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

이 항목에서는 `Adapter`, `LayoutManager`및 `ViewHolder`를 도우미 클래스로 함께 사용 하 여 `RecyclerView`를 지 원하는 방법에 대해 설명 합니다.
이러한 도우미 클래스에 대 한 개략적인 개요를 제공 하 고 앱에서 사용 하는 방법을 설명 합니다.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

이 항목에서는 다양 한 `RecyclerView` 요소를 구현 하 여 실제 사진 검색 앱을 빌드하는 방법에 대 한 실제 코드 예제를 제공 하 여 [RecyclerView 부분과 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) 에 제공 된 정보를 기반으로 합니다.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

이 항목에서는 [기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) 에 제공 된 예제 앱에 추가 코드를 추가 하 여 기본 데이터 소스가 변경 될 때 항목 클릭 이벤트를 처리 하 고 `RecyclerView`를 업데이트 하는 방법을 보여 줍니다.

### <a name="summary"></a>요약

이 가이드에서는 Android `RecyclerView` 위젯을 소개 했습니다. Xamarin Android 프로젝트에 `RecyclerView` 지원 라이브러리를 추가 하는 방법, 보기 `RecyclerView` 재활용 하는 방법, 효율성을 위해 보기 소유자 패턴을 적용 하는 방법, 컬렉션을 표시 하는 `RecyclerView` 공동 작업을 수행 하는 다양 한 도우미 클래스에 대해 설명 했습니다. `RecyclerView`를 응용 프로그램에 통합 하는 방법을 보여 주는 예제 코드를 제공 하 고, 다른 레이아웃 관리자를 연결 하 여 `RecyclerView`의 레이아웃 정책을 조정 하는 방법을 설명 하 고, 항목 클릭 이벤트를 처리 하 고 데이터 `RecyclerView` 알리는 방법을 설명 했습니다. 원본 변경.

`RecyclerView`에 대 한 자세한 내용은 [RecyclerView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [RecyclerViewer (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [롤리팝 소개](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
