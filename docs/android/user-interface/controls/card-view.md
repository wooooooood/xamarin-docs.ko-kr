---
title: CardView
description: Cardview 위젯은 뷰에서 카드 유사한 텍스트 및 이미지 콘텐츠를 표시 하는 UI 구성 요소입니다. 이 가이드를 사용 하 고 이전 버전의 Android와 이전 버전과 호환성을 유지 하면서 CardView Xamarin.Android 응용 프로그램에서 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 21e2a2e8ef04936664344cb4fb758bc2af3b4d05
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771836"
---
# <a name="cardview"></a>CardView

_Cardview 위젯은 뷰에서 카드 유사한 텍스트 및 이미지 콘텐츠를 표시 하는 UI 구성 요소입니다. 이 가이드를 사용 하 고 이전 버전의 Android와 이전 버전과 호환성을 유지 하면서 CardView Xamarin.Android 응용 프로그램에서 사용자 지정 하는 방법을 설명 합니다._


## <a name="overview"></a>개요

`Cardview` Android 5.0 (롤리팝)에 도입 된 위젯은 뷰에서 카드 유사한 텍스트 및 이미지 콘텐츠를 표시 하는 UI 구성 요소입니다. `CardView` 로 구현 됩니다는 `FrameLayout` 둥근된 모서리 및 그림자 위젯입니다. 일반적으로 `CardView` 의 단일 행 항목을 표시에 사용 되는 `ListView` 또는 `GridView` 보기 그룹입니다. 예를 들어 다음 스크린 샷에서 구현 하는 각 여행 예약 응용 프로그램의 예시 `CardView`-여행 대상 카드에는 스크롤 가능한 기반 `ListView`:

![CardView를 사용 하 여 각 여행 대상에 대 한 예제 응용 프로그램](card-view-images/01-cardview-example.png)

이 가이드에서는 추가 하는 방법에 설명 된 `CardView` 패키지를 프로그램 Xamarin.Android 프로젝트를 추가 하는 방법 `CardView` 의 모양을 사용자 지정 하는 방법 및 레이아웃을 `CardView` 응용 프로그램에서 합니다. 이 가이드의 세부 목록을 제공 하는 또한 `CardView` 특성을 사용 하는 특성을 포함 하 여, 변경할 수 있는 `CardView` Android 5.0 롤리팝 보다 이전 버전의 Android에 있습니다.

<a name="requirements" />

## <a name="requirements"></a>요구 사항

다음은 새 Android 5.0 및 이후 기능을 사용 해야 (포함 하 여 `CardView`) Xamarin 기반 앱에서:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 이상을 설치 하 고 Mac.에 대 한 Visual Studio 또는 Visual Studio를 사용 하 여 구성 해야

-  **Android SDK** &ndash; Android 5.0 (API 21) 나중에 설치 되어 있어야 Android SDK Manager를 통해 합니다.

-  **Java JDK 1.8** &ndash; JDK 1.7 구체적으로 대상으로 하는 API 수준 23 및 이전 버전의 경우 사용할 수 있습니다. 사용할 수 있는 JDK 1.8은 [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)합니다.

앱도 포함 해야는 `Xamarin.Android.Support.v7.CardView` 패키지 합니다. 추가 하 여 `Xamarin.Android.Support.v7.CardView` Mac 용 Visual Studio에서 패키지:

1. 프로젝트를 열고 마우스 오른쪽 단추로 클릭 **패키지** 선택 **패키지 추가**합니다.

2. 에 **패키지 추가** 대화 상자에서 검색에 대 한 **CardView**합니다.

3. 선택 **Xamarin 지원 라이브러리 v7 CardView**, 클릭 **패키지 추가**합니다.

추가 하 여 `Xamarin.Android.Support.v7.CardView` Visual Studio에서 패키지:

1. 프로젝트를 열고 마우스 오른쪽 단추로 클릭는 **참조** 노드 (에 **솔루션 탐색기** 창)을 선택 하 고 **NuGet 패키지 관리...** .

2. 경우는 **NuGet 패키지 관리** 대화 상자가 표시 됩니다, 입력 **CardView** 검색 상자에 있습니다.

3. 때 **Xamarin 지원 라이브러리 v7 CardView** 나타나면 클릭 **설치**합니다.

Android 5.0 응용 프로그램 프로젝트를 구성 하는 방법을 알아보려면 참조 [설정을 Android 5.0 프로젝트](~/android/platform/lollipop.md)합니다.
NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 [연습: 프로젝트에 포함 하는 NuGet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)합니다.


## <a name="introducing-cardview"></a>CardView 소개

기본 `CardView` 최소 둥근된 모서리 및 약간의 그림자 흰색 카드와 유사 합니다. 다음 예제에서는 **Main.axml** 레이아웃 표시 되는 단일 `CardView` 위젯을 포함 하는 `TextView`:

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

이 XML을 사용 하 여의 기존 내용을 대체 하는 경우 **Main.axml**의 모든 코드를 주석 처리 해야 **MainActivity.cs** 이전 XML의 리소스에 참조 합니다.

이 레이아웃 예제는 기본 만듭니다 `CardView` 다음 화면에 표시 된 대로 텍스트의 한 줄으로:

[![흰색 배경 및 텍스트 줄을 사용 하 여 CardView의 스크린 샷](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

이 예제에서는 앱 스타일이 있는 밝은 테마 자료로 설정 됩니다 (`Theme.Material.Light`) 하는 `CardView` 그림자 및 가장자리는 쉽게 볼 수 있습니다. 테마 설정 Android 5.0 앱에 대 한 자세한 내용은 참조 [자료 테마](~/android/user-interface/material-theme.md)합니다. 다음 섹션에서는 사용자 지정 하는 방법을 알아봅니다에서는 `CardView` 응용 프로그램에 대 한 합니다.


## <a name="customizing-cardview"></a>CardView 사용자 지정

기본을 수정할 수 있습니다 `CardView` 특성의 모양을 사용자 지정 하는 `CardView` 응용 프로그램에서 합니다. 예를 들어 상승은 `CardView` (낮추는, 배경 위에 더 높은 float로 울 카드) 더 큰 별다른 영향을 늘릴 수 있습니다. 또한 반올림 자세한 카드의 모서리를 둥글게 만드는 모서리 반지름을 늘릴 수 있습니다.

다음 예에서 레이아웃을 사용자 지정 `CardView` 인쇄 사진 ("스냅숏")의 시뮬레이션을 만드는 데 사용 됩니다. `ImageView` 에 추가 되는 `CardView` 이미지를 표시 하기 위한 및 `TextView` 아래에 배치 됩니다는 `ImageView` 이미지의 제목이 표시에 대 한 합니다.
이 예제에서는 레이아웃의 `CardView` 에 다음과 같은 사용자 지정:

-  `cardElevation` 더 큰 그림자 4dp 개로 증가 되었습니다.
-  `cardCornerRadius` 표시 더 둥근 모서리를 둥글게 만드는 5dp 개로 증가 되었습니다.

때문에 `CardView` Android v7 지원 라이브러리에서 해당 특성에서 사용할 수 없는 제공 되는 `android:` 네임 스페이스입니다. XML 네임 스페이스를 정의 하 고 해당 네임 스페이스를 사용 하 여 해야 따라서는 `CardView` 특성 접두사입니다. 아래 레이아웃 예제에서는를 사용 하 여이 줄 라는 네임 스페이스를 정의 `cardview`:

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

이 네임 스페이스를 호출할 수 있습니다 `card_view` 또는 심지어 `myapp` 선택 하는 경우 (이 파일의 범위 내 에서만 액세스할 수 있는 경우). 이 네임 스페이스를 호출 하려는 무엇이 든, 접두사를 사용 해야는 `CardView` 특성을 수정 합니다. 이 예에서 레이아웃의 `cardview` 네임 스페이스에 대 한 접두사는 `cardElevation` 및 `cardCornerRadius`:

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

이 레이아웃 예제 사진 보기 응용 프로그램에서 이미지를 표시 하는 사용 되는 경우는 `CardView` 다음 스크린샷에 표시 된 것 처럼 사진 스냅숏의 모양이:

[![이미지와 이미지 아래 캡션이 있는 CardView](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

가져온 것이 스크린 샷은 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer) 샘플 응용 프로그램을 사용 하 여는 `RecyclerView` 스크롤 목록으로 표시 하는 위젯 `CardView` 사진을 보기 위한 이미지입니다. 에 대 한 자세한 내용은 `RecyclerView`, 참조는 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) 가이드입니다.

에 `CardView` 콘텐츠 영역에 둘 이상의 자식 뷰를 표시할 수 있습니다. 예를 들어 위의 사진 앱 예제 보기의 콘텐츠 영역으로 이루어진는 `ListView` 를 포함 하는 `ImageView` 및 `TextView`합니다. 하지만 `CardView` 인스턴스 세로로 정렬 종종 됩니다, 가로로도 정렬할 수 (참조 [만드는 사용자 지정 보기 스타일](~/android/user-interface/material-theme.md#customview) 예제 스크린샷에 대 한).


### <a name="cardview-layout-options"></a>CardView 레이아웃 옵션

`CardView` 안쪽 여백, 권한 상승, 모퉁이 반경 및 배경 색에 영향을 주는 하나 이상의 특성을 설정 하 여 레이아웃을 사용자 지정할 수 있습니다.

[![CardView 특성의 다이어그램](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

각 특성 해당 하는 도구를 호출 하 여 동적으로 변경할 수도 있습니다 `CardView` 메서드 (대 한 자세한 내용은 `CardView` 메서드 참조는 [CardView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/CardView.html)).
참고 (제외 배경색) 이러한 특성은 다음의 단위를 10 진수 사용 되는 차원 값의 허용 합니다. 예를 들어 `11.5dp` 11.5 밀도 독립적 픽셀로 지정 합니다.


#### <a name="padding"></a>안쪽 여백
`
CardView` 카드 내에서 콘텐츠를 배치 5 안쪽 여백 특성을 제공 합니다. XML 레이아웃에서 설정할 수 있습니다 또는 유사한 메서드가 코드에서 호출할 수 있습니다.

[![안쪽 여백 특성 CardView의 다이어그램](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

안쪽 여백 특성을 다음과 같이 설명 합니다.

-  `contentPadding` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 및 카드의 모든 가장자리입니다.

-  `contentPaddingBottom` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 카드의 아래쪽 가장자리와 합니다.

-  `contentPaddingLeft` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 와 카드의 왼쪽된 가장자리입니다.

-  `contentPaddingRight` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 와 카드의 오른쪽 가장자리입니다.

-  `contentPaddingTop` &ndash; 자식 뷰 사이의 안쪽 여백 내부는 `CardView` 와 카드의 위쪽 가장자리입니다.

콘텐츠 영역 내에 있는 모든 특정된 위젯에 아닌 콘텐츠 영역의 경계를 기준으로 콘텐츠 패딩 특성이 됩니다.
예를 들어 경우 `contentPadding` 사진 보기 응용 프로그램에서 충분히 증가할는 `CardView` 카드에 표시 되는 텍스트와 이미지 자르기 것입니다.



#### <a name="elevation"></a>권한 상승

`CardView` 해당 승격을 제어 하는 두 개의 권한 상승 특성을 제공 하 고, 결과적으로 해당 그림자의 크기:

[![CardView 상승 특성의 다이어그램](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

권한 상승 특성을 다음과 같이 설명 합니다.

-  `cardElevation` &ndash; 상승 된 `CardView` (Z 축을 나타냄).

-  `cardMaxElevation` &ndash; 최대값은 `CardView`의 상승 합니다.

값이 클수록 `cardElevation` 있도록 그림자 크기를 늘리려면 `CardView` float 배경 위에 더 높은 것 같습니다. `cardElevation` 특성도 뷰 겹치는의 그리기 순서를 결정 합니다; 즉, `CardView` 상승 설정이 낮은 겹치는 모든 뷰 이상과 더 높은 권한 상승 설정과 함께 다른 겹치는 보기 아래에 그려집니다.
`cardMaxElevation` 설정은 변경 될 때 응용 프로그램 권한 상승 동적으로 하는 데 유용 &ndash; 그림자의 값이 설정으로 정의 하는 개발자가 수 없습니다.


#### <a name="corner-radius-and-background-color"></a>모퉁이 반경 및 배경색

`CardView` 모서리 반지름 및 배경 색을 제어 하는 데 사용할 수 있는 특성을 제공 합니다. 이러한 두 속성의 전체 스타일을 바꾸는 데 사용할 수는 `CardView`:

[![Radious CardView 모퉁이 및 배경 색 특성의 다이어그램](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

이러한 특성은 다음과 같이 설명 되어 있습니다.

-  `cardCornerRadius` &ndash; 모든 모서리의 모서리 반지름은 `CardView`합니다.

-  `cardBackgroundColor` &ndash; 배경색은 `CardView`합니다.

이 다이어그램에서 `cardCornerRadius` 더 둥근된 10dp로 설정 된 및 `cardBackgroundColor` 로 설정 된 `"#FFFFCC"` (연한 노랑)입니다.


## <a name="compatibility"></a>호환성

사용할 수 있습니다 `CardView` Android 5.0 롤리팝 보다 이전 버전의 Android에 있습니다. 때문에 `CardView` 일부인 Android v7 지원 라이브러리의 사용할 수 있습니다 `CardView` (API 수준 7) Android 2.1 이상.
그러나 설치 해야는 `Xamarin.Android.Support.v7.CardView` 에 설명 된 대로 패키지 [요구 사항](#requirements)위의 합니다.

`CardView` 롤리팝 (API 수준 21) 하기 전에 장치에 약간 다른 동작을 나타냅니다.

-  `CardView` 추가 안쪽 여백을 추가 하는 프로그래밍 방식으로 그림자 구현을 사용 합니다.

-  `CardView` 와 교차 하는 자식 뷰를 클리핑는 `CardView`의 모서리가 둥글게 변합니다.

이러한 호환성 차이 관리 하려면 `CardView` 레이아웃에서 구성할 수 있는 몇 가지 추가 특성을 제공 합니다.

-   `cardPreventCornerOverlap` &ndash; 이 특성을 설정 `true` 에 응용 프로그램은 이전 Android 버전 (API 수준 20 및 이전 버전)에서 실행 되는 패딩을 추가 합니다. 이 설정은 방지 `CardView` 와 교차에서 콘텐츠는 `CardView`의 모서리가 둥글게 변합니다.

-   `cardUseCompatPadding` &ndash; 이 특성을 설정 `true` 채움 문자를 추가 응용 프로그램에서 Android API 수준 21 보다 이상 버전에서 실행 중인 경우. 사용 하려는 경우 `CardView` 사전 롤리팝 장치에서이 특성을 설정, 롤리팝 (또는 이상)에서 동일한 모양을 유지 하 게 하 고 `true`합니다. 이 특성을 사용 하면 `CardView` 사전 롤리팝 장치에서 실행 될 경우 그림자를 그릴 추가 패딩을 추가 합니다. 이렇게 하면 사전 롤리팝 프로그래밍 그림자 구현에 적용 되는 경우 도입 된 패딩의 차이점을 해결 합니다.

Android의 이전 버전과 호환성을 유지 하는 방법에 대 한 자세한 내용은 참조 [호환성을 유지](https://developer.android.com/training/material/compatibility.html)합니다.


## <a name="summary"></a>요약

이 가이드는 새 도입 `CardView` 위젯 Android 5.0 (롤리팝)에 포함 합니다. 기본값을 설명 `CardView` 모양 사용자 지정 하는 방법을 설명 하 고 `CardView` 해당 권한 상승, 둥근 모서리를 변경 하 여 안쪽 여백, 콘텐츠 및 배경색입니다. 나열 된 `CardView` 레이아웃 특성 참조 다이어그램) (포함 사용 하는 방법을 설명 하 고 `CardView` Android 5.0 롤리팝 이전의 Android 장치에서 합니다. 에 대 한 자세한 내용은 `CardView`, 참조는 [CardView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/CardView.html)합니다.


## <a name="related-links"></a>관련 링크

- [RecyclerView (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [롤리팝 소개](~/android/platform/lollipop.md)
- [CardView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
