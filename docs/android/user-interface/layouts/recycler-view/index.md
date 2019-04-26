---
title: RecyclerView
description: RecyclerView는 컬렉션을 표시 하기 위한 뷰 그룹 ListView 및 GridView와 같은 이전 보기 그룹에 대 한 유연성을 대체할 수 있도록 설계 되었습니다.  이 가이드에는 사용 및 RecyclerView Xamarin.Android 응용 프로그램에서 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/03/2018
ms.openlocfilehash: 1332a7ef5b8e5bb2f1178582bcf058123f1e177c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61308763"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView는 컬렉션을 표시 하기 위한 뷰 그룹 ListView 및 GridView와 같은 이전 보기 그룹에 대 한 유연성을 대체할 수 있도록 설계 되었습니다.  이 가이드에는 사용 및 RecyclerView Xamarin.Android 응용 프로그램에서 사용자 지정 하는 방법을 설명 합니다._

## <a name="recyclerview"></a>RecyclerView

동일한 형식 (예: 메시지, 연락처, 이미지 또는 노래); 컬렉션을 표시 하는 데 필요한 대부분의 앱 종종이 컬렉션은 너무 커서 화면에 맞게 컬렉션은 컬렉션의 모든 항목을 통해 원활 하 게 스크롤할 수 있는 작은 창에 표시 됩니다.
`RecyclerView` 목록 또는 컬렉션을 스크롤할 수 있도록 모눈 항목의 컬렉션을 표시 하는 프로그램 Android 위젯 됩니다. 다음은 사용 하는 예제 앱 스크린샷 `RecyclerView` 세로 스크롤 목록에 전자 메일 받은 편지함 내용을 표시 합니다.

[![RecyclerView 목록 받은 편지함 메시지를 사용 하 여 앱의 예](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` 두 가지 매력적인 기능을 제공 합니다.

-  기본 구성 요소에 연결 하 여 해당 동작을 수정할 수 있는 유연한 아키텍처를 있습니다.

-  항목 보기를 다시 사용 하 고 사용 해야 하므로 큰 컬렉션을 사용 하 여 효율적인 *소유자 보기* 뷰 참조를 캐시 하 합니다.

이 가이드를 사용 하는 방법에 설명 `RecyclerView` Xamarin.Android 응용 프로그램에 추가 하는 방법을 설명 합니다 `RecyclerView` Xamarin.Android 프로젝트를 패키지에 설명 하는 방법을 `RecyclerView` 일반적인 응용 프로그램의 함수입니다. 통합 하는 방법을 보여 줍니다 실제 코드 예제가 제공 됩니다 `RecyclerView` 새로 고치는 방법, 응용 프로그램 및 항목 보기 클릭을 구현 하는 방법에 `RecyclerView` 해당 기본 데이터가 변경 될 때입니다. 이 가이드에서는 Xamarin.Android 개발을 사용 하 여 잘 알고 있다고 가정 합니다.


### <a name="requirements"></a>요구 사항

하지만 `RecyclerView` 는 지원 라이브러리로 제공 됩니다 Android 5.0 Lollipop와 사용 하 여에 연결 하는 경우가 많습니다 &ndash; `RecyclerView` 해당 대상 API 수준 (Android 2.1) 7 앱을 사용 하 여 작동 이상. 사용 하기 위해 다음 사항은 `RecyclerView` Xamarin 기반 응용 프로그램에서:

-  **Xamarin.Android** &ndash; 4.20 이상 Xamarin.Android를 설치 하 고 mac 용 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야

-  앱 프로젝트를 포함 해야 합니다 **Xamarin.Android.Support.v7.RecyclerView** 패키지 있습니다. NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 하세요. [연습: 프로젝트에서 NuGet을 포함 하 여](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)입니다.


### <a name="overview"></a>개요

`RecyclerView` 대체 생각할 수 있습니다 합니다 `ListView` 및 `GridView` Android의 위젯입니다. 이전 버전과 마찬가지로 `RecyclerView` 작은 창에서 큰 데이터 집합을 표시 하도록 설계 되어 있지만 `RecyclerView` 더욱 다양 한 레이아웃 옵션 최적화 되 고 더 큰 컬렉션을 표시 합니다. 에 대해 잘 알고 있다면 `ListView`, 몇 가지 중요 한 차이점이 있습니다 사이의 `ListView` 및 `RecyclerView`:

-   `RecyclerView` 사용 하 여 약간 더 복잡: 데 더 많은 코드를 작성 해야 할 `RecyclerView` 비교할 `ListView`합니다.

-   `RecyclerView` 미리 정의 된 어댑터를 제공 하지 않습니다. 데이터 원본에 액세스 하는 어댑터 코드를 구현 해야 합니다. Android 사용 하는 몇 가지 미리 정의 된 어댑터를 포함 하는 반면 `ListView` 고 `GridView`입니다.

-   `RecyclerView` 사용자가 항목을 누르면 항목 클릭 이벤트를 제공 하지 않습니다. 대신, 항목 클릭 이벤트는 도우미 클래스에 의해 처리 됩니다. 반면, `ListView` 항목 클릭 이벤트를 제공 합니다.

-   `RecyclerView` 뷰를 재활용 하 여 및 불필요 한 레이아웃 리소스 조회를 제거 하는 보기 자리 표시자 패턴을 적용 하 여 성능을 향상 시킵니다. 뷰 소유자 패턴을 사용 하는 선택 사항 `ListView`합니다.

-   `RecyclerView` 쉽게 사용자 지정할 수 있도록 하는 모듈식 설계를 기반 합니다. 예를 들어, 앱에 중요 한 코드 변경 없이 다른 레이아웃 정책에 연결할 수 있습니다.
    반면, `ListView` 구조의 상대적으로 모놀리식 됩니다.

-   `RecyclerView` 항목 추가 및 제거에 대 한 기본 제공 애니메이션을 포함 합니다. `ListView` 애니메이션에는 앱 개발자의 몇 가지 추가 작업이 필요합니다.


### <a name="sections"></a>섹션

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

이 항목에 설명 하는 방법을 `Adapter`, `LayoutManager`, 및 `ViewHolder` 지원 하기 위한 도우미 클래스와 함께 작동 `RecyclerView`합니다.
이러한 도우미 클래스의 각 상위 수준 개요를 제공 하 고 사용 하는 방법으로 앱에 설명 합니다.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

제공 하는 정보를 기반으로 한이 항목에서는 [RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) 방법의 실제 코드 예제를 제공 하 여 다양 한 `RecyclerView` 요소 실제 사진을 검색 앱을 빌드하 구현 됩니다.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

나오는 예제에서는 앱에 코드를 추가 하는이 항목에서는 [는 기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) 항목 클릭 이벤트를 처리 하 고 업데이트 하는 방법을 보여 주기 위해 `RecyclerView` 때 기본 데이터 원본 변경 합니다.


### <a name="summary"></a>요약

이 가이드에서는 Android 소개 했습니다 `RecyclerView` 위젯; 추가 하는 방법을 설명 하는 `RecyclerView` Xamarin.Android 프로젝트에 라이브러리를 어떻게 지원 `RecyclerView` 효율성을 높이기 위해 보기 자리 표시자 패턴 적용 방법 및 뷰를 재활용 다양 한 구성 하는 도우미 클래스 `RecyclerView` 컬렉션을 표시 하도록 공동 작업을 수행 합니다. 방법을 보여 주는 예제 코드가 제공 하는 방법 `RecyclerView` 통합 조정 하는 방법을 설명 하는 응용 프로그램에 `RecyclerView`다른 레이아웃 관리자에 연결 하 여 레이아웃 정책 설명의 항목을 처리 하는 방법을 클릭 이벤트 및 알림 `RecyclerView`데이터 원본 변경 내용입니다.

에 대 한 자세한 내용은 `RecyclerView`를 참조 합니다 [RecyclerView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)합니다.


## <a name="related-links"></a>관련 링크

- [RecyclerViewer (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [롤리팝 소개](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
