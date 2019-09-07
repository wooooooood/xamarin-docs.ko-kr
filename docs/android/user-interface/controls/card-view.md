---
title: CardView
description: Cardview 위젯은 카드와 유사한 보기에서 텍스트 및 이미지 콘텐츠를 표시 하는 UI 구성 요소입니다. 이 가이드에서는 이전 버전의 Android와의 호환성을 유지 하면서 Xamarin Android 응용 프로그램에서 CardView를 사용 하 고 사용자 지정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CF12FE85-D03A-4E64-95D2-D7115061A500
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 2051c7c904dedf8b41f405d3ec7b9c1a003b7fd5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758790"
---
# <a name="xamarinandroid-cardview"></a>Xamarin Android CardView

_Cardview 위젯은 카드와 유사한 보기에서 텍스트 및 이미지 콘텐츠를 표시 하는 UI 구성 요소입니다. 이 가이드에서는 이전 버전의 Android와의 호환성을 유지 하면서 Xamarin Android 응용 프로그램에서 CardView를 사용 하 고 사용자 지정 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Android 5.0 (롤리팝)에 도입 된 위젯은카드와유사한보기에서텍스트및이미지콘텐츠를표시하는UI구성요소입니다.`Cardview` `CardView`는 모퉁이 및 그림자 `FrameLayout` 가 둥근 위젯에 구현 됩니다. 일반적으로는 `CardView` `ListView` 또는`GridView` 뷰 그룹에 단일 행 항목을 표시 하는 데 사용 됩니다. 예를 들어 다음 스크린샷은 스크롤할 수 있는 여행 대상 카드를 스크롤 `CardView` `ListView`가능으로 구현 하는 여행 예약 앱의 예입니다.

![각 여행 대상에 대해 CardView를 사용 하는 예제 앱](card-view-images/01-cardview-example.png)

이 가이드에서는 Xamarin Android 프로젝트에 `CardView` 패키지를 추가 하는 방법, 레이아웃에 추가 `CardView` 하는 방법 및 앱에서의 `CardView` 모양을 사용자 지정 하는 방법을 설명 합니다. 또한이 가이드에서는 android 5.0 롤리팝 이전의 android 버전 `CardView` 에서 사용 `CardView` 하는 데 도움이 되는 특성을 포함 하 여 변경할 수 있는 특성의 자세한 목록을 제공 합니다.

<a name="requirements" />

## <a name="requirements"></a>요구 사항

다음은 Xamarin 기반 앱에서 새로운 Android 5.0 이상 기능 (포함 `CardView`)을 사용 하는 데 필요 합니다.

- Visual Studio 또는 Mac용 Visual Studio를 사용 하 여 **xamarin android** &ndash; xamarin android 4.20 이상 버전을 설치 하 고 구성 해야 합니다.

- **Android SDK** &ndash; Android 5.0 (API 21) 이상을 Android SDK Manager를 통해 설치 해야 합니다.

- **JAVA JDK 1.8** &ndash; 특히 API 레벨 23 및 이전 버전을 대상으로 하는 경우 JDK 1.7을 사용할 수 있습니다. JDK 1.8은 [Oracle](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)에서 사용할 수 있습니다.

앱에도 패키지를 `Xamarin.Android.Support.v7.CardView` 포함 해야 합니다. Mac용 Visual Studio에서 패키지 `Xamarin.Android.Support.v7.CardView` 를 추가 하려면 다음을 수행 합니다.

1. 프로젝트를 열고 **패키지** 를 마우스 오른쪽 단추로 클릭 한 다음 **패키지 추가**를 선택 합니다.

2. **패키지 추가** 대화 상자에서 **CardView**를 검색 합니다.

3. **Xamarin 지원 라이브러리 V7 CardView**를 선택 하 고 **패키지 추가**를 클릭 합니다.

Visual Studio에서 `Xamarin.Android.Support.v7.CardView` 패키지를 추가 하려면 다음을 수행 합니다.

1. 프로젝트를 열고 **솔루션 탐색기** 창에서 **참조** 노드를 마우스 오른쪽 단추로 클릭 한 다음 **NuGet 패키지 관리 ...** 를 선택 합니다.

2. **NuGet 패키지 관리** 대화 상자가 표시 되 면 검색 상자에 **CardView** 를 입력 합니다.

3. **Xamarin Support Library V7 CardView** 가 표시 되 면 **설치**를 클릭 합니다.

Android 5.0 앱 프로젝트를 구성 하는 방법을 알아보려면 [android 5.0 프로젝트 설정](~/android/platform/lollipop.md)을 참조 하세요.
NuGet 패키지를 설치 하는 [방법에 대 한 자세한 내용은 연습: 프로젝트](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)에 NuGet을 포함 합니다.

## <a name="introducing-cardview"></a>CardView 소개

기본값 `CardView` 은 모퉁이가 둥근 모퉁이가 작은 흰색 카드와 약간의 그림자와 유사 합니다. 다음 예제 **Main. axml** 레이아웃은를 `CardView` `TextView`포함 하는 단일 위젯을 표시 합니다.

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

이 **xml을 사용**하 여 **MainActivity.cs** 의 기존 내용을 바꾸려면 이전 XML의 리소스를 참조 하는 코드를 주석으로 처리 해야 합니다.

이 레이아웃 예제에서는 다음 스크린샷에 `CardView` 표시 된 것 처럼 한 줄의 텍스트를 사용 하 여 기본을 만듭니다.

[![흰색 배경 및 텍스트 줄이 있는 CardView의 스크린샷](card-view-images/02-basic-cardview-sml.png)](card-view-images/02-basic-cardview.png#lightbox)

이 예제에서는 앱 스타일을 밝은 재질 테마 (`Theme.Material.Light`)로 설정 하 여 `CardView` 그림자와 가장자리를 더 쉽게 볼 수 있도록 합니다. Android 5.0 앱에 테마를 표시 하는 방법에 대 한 자세한 내용은 [재질 테마](~/android/user-interface/material-theme.md)를 참조 하세요. 다음 섹션에서는 응용 프로그램에 대해 사용자 지정 `CardView` 하는 방법을 배웁니다.

## <a name="customizing-cardview"></a>CardView 사용자 지정

기본 `CardView` 특성을 수정 하 여 앱 `CardView` 에서의 모양을 사용자 지정할 수 있습니다. 예를 들어의 `CardView` 상승을 늘려 더 큰 그림자를 캐스팅할 수 있습니다 (카드를 배경 위에 배치 하는 것 처럼 보이게 함). 또한 카드의 모퉁이가 더 둥글게 되도록 모퉁이 반경을 늘릴 수 있습니다.

다음 레이아웃 예제에서는 사용자 지정 `CardView` 된를 사용 하 여 인쇄 사진 ("스냅숏")의 시뮬레이션을 만듭니다. 는 이미지를 표시 `CardView` 하기 위해에 추가 `TextView` `ImageView`되고 는 이미지의 제목을 표시 하기 위해 아래에 배치 됩니다. `ImageView`
이 예제 레이아웃에서에는 `CardView` 다음과 같은 사용자 지정이 있습니다.

- 큰 그림자를 캐스트 하기 위해 가4dp로증가합니다.`cardElevation`
- 모퉁이가 더 둥근 모양으로 보이도록 가5dp로증가합니다.`cardCornerRadius`

는 `CardView` Android v7 지원 라이브러리에서 제공 되기 때문에 `android:` 네임 스페이스에서 해당 특성을 사용할 수 없습니다. 따라서 고유한 XML 네임 스페이스를 정의 하 고 해당 네임 스페이스 `CardView` 를 특성 접두사로 사용 해야 합니다. 아래 레이아웃 예제에서는이 줄을 사용 하 여 라는 `cardview`네임 스페이스를 정의 합니다.

```xml
    xmlns:cardview="http://schemas.android.com/apk/res-auto"
```

이 네임 스페이스 `card_view` 를 호출 `myapp` 하거나 (이 파일의 범위 내 에서만 액세스할 수 있음)를 선택할 수도 있습니다. 이 네임 스페이스를 호출 하도록 선택 하는 경우에는이 네임 스페이스 `CardView` 를 사용 하 여 수정 하려는 특성에 접두사를 사용 해야 합니다. 이 레이아웃 예제 `cardview` 에서 네임 스페이스는 및 `cardCornerRadius`에 대 한 `cardElevation` 접두사입니다.

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

이 레이아웃 예제를 사용 하 여 사진 보기 앱에서 이미지를 표시 하는 경우 `CardView` 에는 다음 스크린샷에 표시 된 것 처럼에 사진 스냅숏의 모양이 있습니다.

[![이미지 및 이미지가 아래에 있는 CardView](card-view-images/03-photo-cardview-sml.png)](card-view-images/03-photo-cardview.png#lightbox)

이 스크린샷은 `RecyclerView` 위젯을 사용 하 여 사진을 보기 위한 이미지의 `CardView` 스크롤 목록을 제공 하는 [RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer) 샘플 앱에서 가져옵니다. 에 대 한 `RecyclerView`자세한 내용은 [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md) guide를 참조 하세요.

는 `CardView` 콘텐츠 영역에 둘 이상의 자식 뷰를 표시할 수 있습니다. 예를 들어 위의 사진 보기 앱 예제에서 콘텐츠 영역은 `ListView` `ImageView` 및를 `TextView`포함 하는로 구성 됩니다. 인스턴스가 `CardView` 일반적으로 수직으로 정렬 되기는 하지만이를 가로로 정렬할 수도 있습니다 (예제 스크린샷에 대 한 [사용자 지정 보기 스타일 만들기](~/android/user-interface/material-theme.md#customview) 참조).

### <a name="cardview-layout-options"></a>CardView 레이아웃 옵션

`CardView`안쪽 여백, 높이, 모퉁이 반경 및 배경색에 영향을 주는 특성을 하나 이상 설정 하 여 레이아웃을 사용자 지정할 수 있습니다.

[![CardView 특성 다이어그램](card-view-images/04-attributes-sml.png)](card-view-images/04-attributes.png#lightbox)

각 특성은 대응 `CardView` 하는 메서드를 호출 하 여 동적으로 변경할 수도 있습니다. `CardView` 메서드에 대 한 자세한 내용은 [CardView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/CardView.html)를 참조 하세요.
이러한 특성 (배경색 제외)은 치수 값을 허용 하며,이 값은 10 진수이 고 그 뒤에 단위가 옵니다. 예를 들어 `11.5dp` 는 11.5 밀도 독립적 픽셀을 지정 합니다.

#### <a name="padding"></a>안쪽 여백

`CardView`는 카드 내에서 콘텐츠를 배치할 수 있는 5 가지 패딩 특성을 제공 합니다. 레이아웃 XML에서 설정 하거나 코드에서 유사한 메서드를 호출할 수 있습니다.

[![CardView 패딩 특성 다이어그램](card-view-images/05-padding-sml.png)](card-view-images/05-padding.png#lightbox)

패딩 특성에 대 한 설명은 다음과 같습니다.

- `contentPadding`의 자식 뷰와 `CardView` 카드의 모든 가장자리 사이의 안쪽 안쪽 여백입니다. &ndash;

- `contentPaddingBottom`&ndash; 의 자식뷰와카드의아래쪽가장자리사이의안쪽`CardView` 안쪽 여백입니다.

- `contentPaddingLeft`&ndash; 의 자식뷰와카드의왼쪽가장자리사이의안쪽`CardView` 안쪽 여백입니다.

- `contentPaddingRight`&ndash; 의 자식뷰와카드의오른쪽가장자리사이의안쪽`CardView` 안쪽 여백입니다.

- `contentPaddingTop`&ndash; 의 자식뷰와카드의위쪽가장자리사이의안쪽`CardView` 안쪽 여백입니다.

콘텐츠 패딩 특성은 콘텐츠 영역 내에 있는 지정 된 위젯이 아니라 콘텐츠 영역의 경계를 기준으로 합니다.
예를 들어 사진 `contentPadding` 보기 앱에서 충분히 늘어난 경우는 `CardView` 카드에 표시 된 이미지와 텍스트를 모두 자릅니다.

#### <a name="elevation"></a>검사용

`CardView`는 권한 상승을 제어 하는 두 가지 권한 상승 특성을 제공 하며, 그에 따라 해당 그림자의 크기를 제공 합니다.

[![CardView 권한 상승 특성 다이어그램](card-view-images/06-elevation-sml.png)](card-view-images/06-elevation.png#lightbox)

권한 상승 특성에 대 한 설명은 다음과 같습니다.

- `cardElevation`&ndash; 의상승(Z`CardView` 축 나타냄)입니다.

- `cardMaxElevation`&ndash; 상승`CardView`의 최대값입니다.

값 `cardElevation` 이 클수록 배경 위에 더 큰 부동으로 `CardView` 보이도록 그림자 크기가 증가 합니다. 특성 `cardElevation` 은 겹치는 보기의 그리기 순서를 결정 합니다. 즉 `CardView` , 높은 권한 상승을 사용 하는 다른 겹치는 뷰로 그리고 낮은 권한 설정으로 겹치는 보기 위에 그려집니다.
설정은 `cardMaxElevation` 앱에서 권한 상승을 동적으로 &ndash; 변경 하는 경우이 설정을 사용 하 여 정의 하는 한도를 초과 하 여 섀도를 확장할 수 없는 경우에 유용 합니다.

#### <a name="corner-radius-and-background-color"></a>모퉁이 반경 및 배경색

`CardView`는 모퉁이 반지름과 배경색을 제어 하는 데 사용할 수 있는 특성을 제공 합니다. 이러한 두 가지 속성을 통해의 `CardView`전체 스타일을 변경할 수 있습니다.

[![CardView 모퉁이 radious 및 배경색 특성 다이어그램](card-view-images/07-radius-bgcolor-sml.png)](card-view-images/07-radius-bgcolor.png#lightbox)

이러한 특성에 대 한 설명은 다음과 같습니다.

- `cardCornerRadius`의 모든 모퉁이에 대 한 모퉁이 반경입니다. `CardView` &ndash;

- `cardBackgroundColor`&ndash; 의`CardView`배경색입니다.

이 다이어그램 `cardCornerRadius` 에서는 보다 반올림 된 10dp로 설정 되 고 `cardBackgroundColor` 는 (연한 `"#FFFFCC"` 노랑)로 설정 됩니다.

## <a name="compatibility"></a>호환성

Android 5.0 롤리팝 `CardView` 이전 버전의 android에서 사용할 수 있습니다. 는 `CardView` android v7 지원 라이브러리의 일부 이기 때문에 android 2.1 ( `CardView` API 수준 7) 이상에서 사용할 수 있습니다.
그러나 위의 [요구 사항](#requirements)에 설명 `Xamarin.Android.Support.v7.CardView` 된 대로 패키지를 설치 해야 합니다.

`CardView`는 롤리팝 (API 레벨 21) 이전 장치에서 약간 다른 동작을 보여 주는 것입니다.

- `CardView`는 추가 안쪽 여백을 추가 하는 프로그래밍 그림자 구현을 사용 합니다.

- `CardView`는 모퉁이가 둥근 모퉁이와 `CardView`교차 하는 자식 뷰를 클리핑 하지 않습니다.

이러한 호환성 차이를 관리 하는 데 `CardView` 도움이 되도록에서는 레이아웃에서 구성할 수 있는 몇 가지 추가 특성을 제공 합니다.

- `cardPreventCornerOverlap`이전 Android 버전에서 `true` 앱을 실행 하는 경우이 특성을로 설정 합니다 (API 레벨 20 및 이전 버전). &ndash; 이 설정은 콘텐츠가 `CardView` 모퉁이가 둥근 모퉁이 `CardView`와 교차 하지 않도록 합니다.

- `cardUseCompatPadding`앱이 Android 버전 `true` 이상에서 실행 되는 경우에는이 특성을로 설정 하 여 패딩을 추가 합니다. &ndash; 롤리팝 이전 장치에서를 `CardView` 사용 하 고 롤리팝에서 동일 하 게 보이도록 하려면이 특성을로 `true`설정 합니다. 이 특성을 사용 하는 `CardView` 경우에는 미리 롤리팝 장치에서 실행 될 때 그림자를 그리기 위해 안쪽 여백을 추가 합니다. 이렇게 하면 이전 버전의 프로그래밍 그림자 구현이 적용 될 때 도입 된 패딩의 차이를 극복 하는 데 도움이 됩니다.

이전 버전의 Android와의 호환성을 유지 하는 방법에 대 한 자세한 내용은 [호환성 유지 관리](https://developer.android.com/training/material/compatibility.html)를 참조 하세요.

## <a name="summary"></a>요약

이 가이드에서는 Android 5.0 `CardView` (롤리팝)에 포함 된 새로운 위젯을 소개 했습니다. 기본 `CardView` 모양을 보여주고, 높이, 모퉁이 원형율, 콘텐츠 `CardView` 안쪽 여백 및 배경색을 변경 하 여 사용자 지정 하는 방법을 설명 했습니다. `CardView` 레이아웃 특성 (참조 다이어그램 포함)이 나열 되 고 android 5.0 롤리팝 이전의 android `CardView` 장치에서를 사용 하는 방법에 대해 설명 했습니다. 에 대 한 `CardView`자세한 내용은 [CardView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/CardView.html)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [RecyclerView (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [롤리팝 소개](~/android/platform/lollipop.md)
- [CardView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/CardView.html)
