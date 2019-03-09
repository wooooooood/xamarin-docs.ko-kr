---
title: CardView
description: Cardview 위젯은 카드와 비슷한 뷰에서 텍스트 및 이미지 콘텐츠를 제공 하는 UI 구성 요소입니다. 이 가이드에 사용 하 여 이전 버전의 Android 사용 하 여 이전 버전과 호환성을 유지 하면서 CardView Xamarin.Android 응용 프로그램에서 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: cdb75207bff3f15a54d0cdd90fa0833da9c145e6
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670651"
---
# <a name="cardview"></a>CardView

_Cardview 위젯은 카드와 비슷한 뷰에서 텍스트 및 이미지 콘텐츠를 제공 하는 UI 구성 요소입니다. 이 가이드에 사용 하 여 이전 버전의 Android 사용 하 여 이전 버전과 호환성을 유지 하면서 CardView Xamarin.Android 응용 프로그램에서 사용자 지정 하는 방법을 설명 합니다._


## <a name="overview"></a>개요

`Cardview` (Lollipop) Android 5.0에 도입 된 위젯을 카드와 비슷한 뷰에서 텍스트 및 이미지 콘텐츠를 제공 하는 UI 구성 요소입니다. `CardView` 으로 구현 되는 `FrameLayout` 둥근된 모서리와 그림자를 사용 하 여 위젯. 일반적으로 `CardView` 의 단일 행 항목을 제공 하는 데 사용 되는 `ListView` 또는 `GridView` 보기 그룹입니다. 예를 들어 다음 스크린 샷에서를 구현 하는 여행 예약 응용 프로그램의 예로 `CardView`-여행 대상 카드를 스크롤할 수 있는 기반 `ListView`:

![CardView를 사용 하 여 각 여행 대상에 대 한 앱의 예](card-view-images/01-cardview-example.png)

이 가이드를 추가 하는 방법에 설명 합니다 `CardView` 패키지에 xamarin.android 프로젝트를 추가 하는 방법 `CardView` 여 레이아웃 및 모양을 사용자 지정 하는 방법에 `CardView` 앱에서. 이 가이드의 자세한 목록을 제공 하는 또한 `CardView` 사용할 수 있도록 특성을 변경할 수 있는 특성 `CardView` Android 5.0 Lollipop 이전 버전의 Android입니다.

<a name="requirements" />

## <a name="requirements"></a>요구 사항

다음 새 Android 5.0 및 이후 기능을 사용 해야 합니다. (포함 하 여 `CardView`) Xamarin 기반 앱에서:

-  **Xamarin.Android** &ndash; 4.20 이상 Xamarin.Android를 설치 하 고 mac 용 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야

-  **Android SDK** &ndash; Android 5.0 (API 21) 나중에 설치 해야 Android SDK Manager를 통해 또는 합니다.

-  **Java JDK 1.8** &ndash; 특별히 대상으로 하는 API 수준 23 및 이전 하려는 경우 JDK 1.7을 사용할 수 있습니다. JDK 1.8에서 제공 됩니다 [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)합니다.

앱도 포함 해야 합니다는 `Xamarin.Android.Support.v7.CardView` 패키지 있습니다. 추가 하 여 `Xamarin.Android.Support.v7.CardView` Mac 용 Visual Studio에서 패키지:

1. 프로젝트를 열고 마우스 오른쪽 단추로 클릭 **패키지** 선택한 **패키지 추가**합니다.

2. 에 **패키지 추가** 대화 상자에서 검색 **CardView**합니다.

3. 선택 **Xamarin 지원 라이브러리 v7 CardView**, 클릭 **패키지 추가**합니다.

추가 하 여 `Xamarin.Android.Support.v7.CardView` Visual Studio에서 패키지:

1. 프로젝트를 열고 마우스 오른쪽 단추로 클릭 합니다 **참조** 노드 (에 **솔루션 탐색기** 창) 선택한 **NuGet 패키지 관리...** .

2. 경우는 **NuGet 패키지 관리** 대화 상자가 표시 됩니다, 입력 **CardView** 검색 상자에 있습니다.

3. 때 **Xamarin 지원 라이브러리 v7 CardView** 나타나면 클릭 **설치**합니다.

Android 5.0 앱 프로젝트를 구성 하는 방법에 알아보려면 참조 [설정 하는 Android 5.0 프로젝트](~/android/platform/lollipop.md)합니다.
NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 하세요. [연습: 프로젝트에서 NuGet을 포함 하 여](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)입니다.


## <a name="introducing-cardview"></a>CardView 소개

기본 `CardView` 최소 둥근된 모서리와 약간의 그림자를 사용 하 여 흰색 카드와 유사 합니다. 다음 예제에서는 **Main.axml** 레이아웃 표시 단일 `CardView` 위젯을 포함 하는 `TextView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal">
        <TextView
            android:text="Basic CardView"
            android:layout_marginTop="0dp"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center"
            android:layout_centerVertical="true"
            android:layout_alignParentRight="true"
            android:layout_alignParentEnd="true" />
    </android.support.v7.widget.CardView>
</LinearLayout>
```

기존 내용을 바꿉니다이 XML을 사용 하는 경우 **Main.axml**, 모든 코드의 주석 처리 해야 **MainActivity.cs** 위의 xml 리소스를 참조 하는 합니다.

이 레이아웃 예제는 기본 만듭니다 `CardView` 다음 스크린 샷과 같이 텍스트 한 줄을 사용 하 여:

[![흰색 배경 및 텍스트 줄을 사용 하 여 CardView의 스크린 샷](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

이 예제에서는 앱 스타일 빛 재질 테마에 설정 됩니다 (`Theme.Material.Light`) 있도록는 `CardView` 그림자 및에 지는 쉽게 확인할 수 있습니다. Android 5.0 테마 앱에 대 한 자세한 내용은 참조 하세요. [재질 테마](~/android/user-interface/material-theme.md)합니다. 다음 섹션에서는 사용자 지정 하는 방법을 알아봅니다에서는 `CardView` 응용 프로그램에 대 한 합니다.


## <a name="customizing-cardview"></a>사용자 지정 CardView

기본을 수정할 수 있습니다 `CardView` 모양을 사용자 지정 특성을 `CardView` 앱에서. 예를 들어, 상승 된 `CardView` (그러면, 배경 위에 더 높은 float 같습니다 카드)는 더 큰 섀도 캐스팅을 늘릴 수 있습니다. 또한 모퉁이 반경은 반올림 자세한 카드의 모서리를 둥글게 만드는 늘릴 수 있습니다.

다음 예에서 레이아웃을 사용자 지정 `CardView` 인쇄 사진 ("스냅숏")의 시뮬레이션을 만드는 데 사용 됩니다. `ImageView` 에 추가 됩니다 합니다 `CardView` 이미지를 표시 하기 위한 및 `TextView` 아래에 배치 됩니다는 `ImageView` 이미지의 제목을 표시 합니다.
이 예제에서는 레이아웃을 `CardView` 에 다음과 같은 사용자 지정:

-  `cardElevation` 4dp 큰 섀도 캐스팅 하는 증가 합니다.
-  `cardCornerRadius` 표시 더 둥근 모서리를 둥글게 만드는 5dp 증가 됩니다.

때문에 `CardView` Android v7 지원 라이브러리를 통해 해당 특성은에서 사용할 수 있는 제공 된 `android:` 네임 스페이스입니다. XML 네임 스페이스를 정의 하 고 해당 네임 스페이스를 사용 해야 하므로 `CardView` 특성 접두사입니다. 아래 레이아웃 예제에서는 사용 하 여이 줄 라는 네임 스페이스를 정의 하려면 `cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

이 네임 스페이스를 호출할 수 있습니다 `card_view` 심지어 `myapp` 않으려는 경우 (이 파일의 범위 내 에서만 액세스할 수 있는 경우). 이 네임 스페이스를 선택 하 든 간에, 앞에 붙는 접두사로 사용 해야 합니다는 `CardView` 수정 하고자 하는 특성입니다. 이 예에서 레이아웃 합니다 `cardview` 네임 스페이스에 대 한 접두사인 `cardElevation` 및 `cardCornerRadius`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:gravity="center_horizontal"
    android:padding="5dp">
    <android.support.v7.widget.CardView
        android:layout_width="fill_parent"
        android:layout_height="245dp"
        android:layout_gravity="center_horizontal"
        cardview:cardElevation="4dp"
        cardview:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="fill_parent"
            android:layout_height="240dp"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="fill_parent"
                android:layout_height="190dp"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="fill_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Photo Title"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="5dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</LinearLayout>
```

이 레이아웃 예제 사진 보기 응용 프로그램에서 이미지를 표시 하는 경우는 `CardView` 다음 스크린샷에 표시 된 것 처럼 사진 스냅숏으로 모양이:

[![이미지와 이미지 아래의 캡션을 CardView](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

이 스크린샷에서에서 가져온 것를 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer) 를 사용 하는 샘플 앱을 `RecyclerView` 의 스크롤 목록을 표시 하는 위젯 `CardView` 사진을 보기 위한 이미지입니다. 에 대 한 자세한 내용은 `RecyclerView`를 참조 합니다 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) 가이드입니다.

에 `CardView` 콘텐츠 영역에 둘 이상의 자식 뷰를 표시할 수 있습니다. 예를 들어, 앱 예제를 확인 하는 위의 사진의 콘텐츠 영역으로 구성 됩니다는 `ListView` 를 포함 하는 `ImageView` 및 `TextView`합니다. 하지만 `CardView` 인스턴스를 세로로 정렬 자주, 가로로도 정렬할 수 있습니다 (참조 [사용자 지정 보기 스타일 만들기](~/android/user-interface/material-theme.md#customview) 스크린 샷 예가 대 한).


### <a name="cardview-layout-options"></a>CardView 레이아웃 옵션

`CardView` 레이아웃 안쪽 여백, 권한 상승, 모퉁이 반경 및 배경 색상에 영향을 주는 하나 이상의 특성을 설정 하 여 사용자 지정할 수 있습니다.

[![CardView 특성의 다이어그램](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

각 특성은 대응 사용자를 호출 하 여 동적으로 변경할 수도 있습니다 `CardView` 메서드 (대 한 자세한 내용은 `CardView` 메서드를 참조 합니다 [CardView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
(배경색) 제외 하 고 이러한 특성은 단위 뒤에 10 진수는 차원 값을 그대로 사용 하는 참고 합니다. 예를 들어 `11.5dp` 11.5 밀도 독립적 픽셀을 지정 합니다.


#### <a name="padding"></a>안쪽 여백
`
CardView` 카드 내에서 콘텐츠를 배치 하도록 5 안쪽 여백 특성을 제공 합니다. XML 레이아웃에서 설정할 수 있습니다 또는 유사한 메서드가 코드에서 호출할 수 있습니다.

[![안쪽 여백 특성 CardView 다이어그램](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

안쪽 여백 특성을 다음과 같이 설명 됩니다.

-  `contentPadding` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 및 카드의 모든 가장자리입니다.

-  `contentPaddingBottom` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 카드의 아래쪽 가장자리와 합니다.

-  `contentPaddingLeft` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 와 카드의 왼쪽된 가장자리입니다.

-  `contentPaddingRight` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 와 카드의 오른쪽 가장자리입니다.

-  `contentPaddingTop` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 와 카드의 위쪽 가장자리입니다.

콘텐츠 안쪽 여백 특성 대신 콘텐츠 영역 내에 있는 모든 지정 된 위젯 콘텐츠 영역의 경계를 기준으로 합니다.
예를 들어 경우 `contentPadding` 사진 보기 응용 프로그램에서 충분히 증가 된는 `CardView` 카드에 표시 되는 텍스트와 이미지를 자르는 것입니다.



#### <a name="elevation"></a>권한 상승

`CardView` 해당 높이 제어 하는 두 가지 권한 상승 특성을 제공 하 고, 결과적으로 해당 그림자 크기:

[![CardView 권한 상승 특성의 다이어그램](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

권한 상승 특성은 다음과 같이 설명 됩니다.

-  `cardElevation` &ndash; 상승 된 `CardView` (Z 축을 나타냅니다).

-  `cardMaxElevation` &ndash; 최대값을 `CardView`의 권한 상승 합니다.

값이 클수록 `cardElevation` 있도록 그림자 크기를 늘릴 `CardView` float 배경 위에 더 높은 것 같습니다. 합니다 `cardElevation` 특성은 또한 뷰 겹치는의 그리기 순서를 결정입니다. 즉, `CardView` 낮은 권한 상승 설정 사용 하 여 겹치는 모든 보기 이상 더 높은 권한 상승 설정을 사용 하 여 다른 겹치는 보기 아래에서 가져오게 됩니다.
합니다 `cardMaxElevation` 설정이 변경 되 면 앱 권한 상승 동적으로 유용 &ndash; 그림자에서이 설정을 사용 하 여 정의 하는 한도 초과 확장 되지 않습니다.


#### <a name="corner-radius-and-background-color"></a>모퉁이 반경 및 배경색

`CardView` 모서리 반지름 및 배경 색을 제어 하는 데 사용할 수 있는 특성을 제공 합니다. 이러한 두 속성의 전체 스타일을 바꾸는 데 사용할 수는 `CardView`:

[![다이어그램 radious CardView 모서리와 배경 색 특성](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

이러한 특성은 다음과 같이 설명 됩니다.

-  `cardCornerRadius` &ndash; 모든 모퉁이의 모퉁이 반경을 `CardView`합니다.

-  `cardBackgroundColor` &ndash; 배경색을 `CardView`입니다.

이 다이어그램과 `cardCornerRadius` 더 둥근된 10dp로 설정 됩니다 및 `cardBackgroundColor` 로 설정 되어 `"#FFFFCC"` (연한 노랑)입니다.


## <a name="compatibility"></a>호환성

사용할 수 있습니다 `CardView` Android 5.0 Lollipop 이전 버전의 Android입니다. 때문에 `CardView` 일부인 Android v7 지원 라이브러리를 사용할 수 있습니다 `CardView` (API 수준 7) Android 2.1 이상.
그러나 설치 해야 합니다는 `Xamarin.Android.Support.v7.CardView` 에 설명 된 대로 패키지 [요구 사항을](#requirements)위의 합니다.

`CardView` 롤리팝 (API 수준 21) 하기 전에 장치에서 약간 다른 동작을 수행 합니다.

-  `CardView` 추가 안쪽 여백을 추가 하는 프로그래밍 방식으로 섀도 구현을 사용 합니다.

-  `CardView` 와 교차 하는 자식 뷰를 잘라지 않습니다는 `CardView`모서리가 둥근의 합니다.

이러한 호환성 차이 관리 하는 데 `CardView` 레이아웃에서 구성할 수 있는 몇 가지 추가 특성을 제공 합니다.

-   `cardPreventCornerOverlap` &ndash; 이 특성을 설정 `true` 앱은 이전 Android 버전 (API 레벨 20 및 이전 버전)에서 실행 중인 경우 안쪽 여백을 추가 하려면. 이 설정은 방지 `CardView` 와 교차에서 콘텐츠를 `CardView`모서리가 둥근의 합니다.

-   `cardUseCompatPadding` &ndash; 이 특성을 설정 `true` 앱에서 Android API 수준 21 보다 이상 버전에서 실행 중인 경우 안쪽 여백을 추가 합니다. 사용 하려는 경우 `CardView` 사전 롤리팝 장치의 롤리팝 (또는 이상)에서 동일 하 게 표시,이 속성을 설정 하도록 `true`합니다. 이 특성을 사용 하면 `CardView` 사전 롤리팝 장치에서 실행 될 경우 그림자를 그릴 추가 패딩을 추가 합니다. 사전 롤리팝 프로그래밍 섀도 구현 될 때 도입 된 패딩의 차이 극복 하기 위해 이렇게 합니다.

Android의 이전 버전과 호환성을 유지 관리 하는 방법에 대 한 자세한 내용은 참조 하세요. [유지 관리 호환성](https://developer.android.com/training/material/compatibility.html)합니다.


## <a name="summary"></a>요약

이 가이드는 새 도입 `CardView` 위젯 Android 5.0 (Lollipop)에 포함 합니다. 기본값을 보여 줍니다 `CardView` 모양을 사용자 지정 하는 방법을 설명 하 고 `CardView` 해당 권한 상승, 모서리의 둥근 정도 변경 하 여 안쪽 여백, 콘텐츠 및 배경색입니다. 나열 합니다 `CardView` 레이아웃 특성 (참조 다이어그램)를 포함 하 고 사용 하는 방법을 설명 `CardView` Android 5.0 Lollipop 이전 Android 장치에서. 에 대 한 자세한 내용은 `CardView`를 참조 합니다 [CardView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/CardView.html)합니다.


## <a name="related-links"></a>관련 링크

- [RecyclerView (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [롤리팝 소개](~/android/platform/lollipop.md)
- [CardView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
