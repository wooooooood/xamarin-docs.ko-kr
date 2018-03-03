---
title: RecyclerView
description: "RecyclerView는 컬렉션을 표시 하기 위한 뷰 그룹 ListView 및 GridView와 같은 이전 보기 그룹에 대 한 유연성을 대체할 수 있도록 설계 되었습니다.  이 가이드에는 사용 및 RecyclerView Xamarin.Android 응용 프로그램에서 사용자 지정 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/03/2018
ms.openlocfilehash: ec8b3a4655c8e8d9e492c9f7a1807dd64ecc6ae7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView는 컬렉션을 표시 하기 위한 뷰 그룹 ListView 및 GridView와 같은 이전 보기 그룹에 대 한 유연성을 대체할 수 있도록 설계 되었습니다.  이 가이드에는 사용 및 RecyclerView Xamarin.Android 응용 프로그램에서 사용자 지정 하는 방법을 설명 합니다._

## <a name="recyclerview"></a>RecyclerView

많은 응용 프로그램 (예: 메시지, 연락처, 이미지 또는 노래); 같은 유형의 컬렉션을 표시 해야 합니다. 대개이 컬렉션은 너무 커서 화면에 맞게 컬렉션은 컬렉션의 모든 항목을 통해 원활 하 게 스크롤할 수 있는 작은 창에 표시 됩니다.
`RecyclerView` 목록 또는 그리드로 컬렉션을 스크롤할 수 있도록 항목의 컬렉션을 표시 하는 Android 위젯 됩니다. 다음은 사용 하는 예제 앱의 스크린샷을 `RecyclerView` 세로 스크롤 목록에 전자 메일 받은 편지함 내용을 표시 합니다.

[ ![RecyclerView 목록 받은 편지함 메시지를 사용 하 여 예제 응용 프로그램](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png)

`RecyclerView` 두 개의 강력한 기능을 제공합니다.

-  기본 구성 요소에서 연결 하 여 해당 동작을 수정할 수 있는 유연한 아키텍처 있음

-  항목 보기를 다시 사용 하 고 사용 해야 하기 때문에 대규모 컬렉션으로 효율적입니다 *소유자에 게 보기* 캐시 뷰 참조에 대 한 합니다.

이 가이드를 사용 하는 방법에 설명 `RecyclerView` Xamarin.Android 응용 프로그램에 추가 하는 방법을 설명는 `RecyclerView` Xamarin.Android 프로젝트를 패키지에 설명 방법을 `RecyclerView` 일반적인 응용 프로그램의 함수입니다. 통합 하는 방법을 보여 주는 실제 코드 예제가 제공 됩니다 `RecyclerView` 응용 프로그램, 항목 보기 클릭을 구현 하는 방법 및 새로 고치는 방법으로 `RecyclerView` 원본 데이터 변경 되는 경우. 이 가이드에서는 Xamarin.Android 개발에 익숙한 가정 합니다.


### <a name="requirements"></a>요구 사항

하지만 `RecyclerView` 는 지원 라이브러리도 제공 종종와 연결 된 Android 5.0 롤리팝 &ndash; `RecyclerView` 작동 하는 앱과 해당 대상 API 수준 (Android 2.1) 7 이상. 다음을 사용 하려면 필요 `RecyclerView` Xamarin 기반 응용 프로그램에서:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 이상을 설치 하 고 Mac.에 대 한 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야

-  응용 프로그램 프로젝트를 포함 해야 합니다는 **Xamarin.Android.Support.v7.RecyclerView** 패키지 합니다. NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 [연습: 프로젝트에 포함 하는 NuGet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)합니다.


### <a name="overview"></a>개요

`RecyclerView` 에 대 한 대체 값으로 생각할 수 있습니다는 `ListView` 및 `GridView` Android에서 위젯의 합니다. 이전 버전과 마찬가지로 `RecyclerView` 작은 창에서 큰 데이터 집합을 표시 하도록 디자인 되었지만 `RecyclerView` 더 많은 레이아웃 옵션이 제공 하 고 보다 광범위 한 모음을 표시 하기 위해 최적화 됩니다. 에 익숙한 경우 `ListView`, 몇 가지 중요 한 차이점이 간의 `ListView` 및 `RecyclerView`:

-   `RecyclerView` 사용 하도록 약간 더 복잡:를 사용 하려면 더 많은 코드를 작성 해야 `RecyclerView` 에 비해 `ListView`합니다.

-   `RecyclerView` 미리 정의 된 어댑터; 제공 하지 않습니다. 데이터 원본에 액세스 하는 경우 어댑터 코드를 구현 해야 합니다. Android를 사용 하는 몇 가지 미리 정의 된 어댑터를 포함 하는 반면 `ListView` 및 `GridView`합니다.

-   `RecyclerView` 사용자가 항목;를 누를 때 항목 클릭 이벤트를 제공 하지 않습니다. 대신, 항목 클릭 이벤트는 도우미 클래스에 의해 처리 됩니다. 반면, `ListView` 항목 클릭 이벤트를 제공 합니다.

-   `RecyclerView` 뷰를 재활용 하 고 불필요 한 레이아웃 조회를 제거 하는 뷰 소유자 패턴을 적용 하 여 성능을 향상 시킵니다. 뷰 소유자 패턴을 사용 하는의 선택적 `ListView`합니다.

-   `RecyclerView` 기반으로 쉽게 사용자 지정할 수 있는 모듈형 디자인 합니다. 예를 들어 앱에 중요 한 코드를 변경 하지 않고 다른 레이아웃 정책에 연결할 수 있습니다.
    반면, `ListView` 구조에 상대적으로 모놀리식 됩니다.

-   `RecyclerView` 항목 추가 및 제거에 대 한 기본 제공 애니메이션을 포함 합니다. `ListView` 애니메이션의 몇 가지 추가 작업 앱 개발자 부담도 필요합니다.


### <a name="sections"></a>섹션

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

이 항목에서는 설명 방법을 `Adapter`, `LayoutManager`, 및 `ViewHolder` 지원 하기 위해 도우미 클래스와 함께 작업 `RecyclerView`합니다.
이러한 도우미 클래스의 각 상위 수준 개요를 제공 하 고 사용법을 앱에 설명 합니다.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

이 항목에 제공 된 정보에 빌드 [RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) 방법의 실제 코드 예제를 제공 하 여 다양 한 `RecyclerView` 요소 실제 사진 검색 앱을 빌드하기 위해 구현 됩니다.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

이 항목에 제공 된 예제 응용 프로그램에 추가 코드를 추가 [기본 RecyclerView 예제에서는](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) 항목 클릭 이벤트를 처리 하 고 업데이트 하는 방법을 보여 주기 위해 `RecyclerView` 때 기본 데이터 소스가 변경 합니다.


### <a name="summary"></a>요약

이 가이드는 Android 도입 `RecyclerView` 위젯; 추가 하는 방법을 설명 하는 `RecyclerView` Xamarin.Android 프로젝트에 라이브러리를 어떻게 지원 `RecyclerView` 하 고 효율성을 높이기 위해 뷰 소유자 패턴을 적용 하는 방법 보기, 재활용 다양 한 구성 하는 도우미 클래스 `RecyclerView` 컬렉션을 표시 하도록 공동 작업을 수행 합니다. 이 제공 되는 방법을 보여 주는 예제 코드가 방법을 `RecyclerView` 통합 된 응용 프로그램으로 구성 하는 방법을 설명 하기 `RecyclerView`의 서로 다른 레이아웃 관리자에 연결 하 여 레이아웃 정책 항목을 처리 하는 방법을 설명 이벤트를 클릭 하 고 알림`RecyclerView`데이터 원본 변경 합니다.

에 대 한 자세한 내용은 `RecyclerView`, 참조는 [RecyclerView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)합니다.


## <a name="related-links"></a>관련 링크

- [RecyclerViewer (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [롤리팝 소개](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
