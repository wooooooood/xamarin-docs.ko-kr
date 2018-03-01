---
title: GridLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 013db64add615e94ef3494f14bc82fc17ec2dca1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="gridlayout"></a>GridLayout

`GridLayout` 는 새로운 `ViewGroup` 아래와 같이 뷰는 HTML 테이블과 비슷한 2D 눈금에 배치를 지 원하는 서브 클래스:

 [ ![잘린 GridLayout 네 개의 셀을 표시 합니다.](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png)

 `GridLayout` 자식 뷰 ו 위치로 표에 행과에 있어야 하는 열을 지정 하 여 계층 구조는 플랫 보기 함께 작동 합니다. 이러한 방식으로 *GridLayout* 중간 모든 뷰는 테이블 구조를 제공 등의 TableLayout에 사용 된 테이블 행에서 볼 필요 없이 표에서 뷰 위치를 지정할 수 있습니다. 플랫 계층 유지 관리 함으로써 *GridLayout* 더 빨리 레이아웃 수 해당 자식 뷰. 이 개념 실제로 의미 코드에서 설명 하는 예제를 살펴보겠습니다.

<a name="Creating_a_Grid_Layout" />

## <a name="creating-a-grid-layout"></a>격자 레이아웃 만들기

다음 XML 몇 개 추가 `TextView` 컨트롤을 한 *GridLayout*합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip" />
</GridLayout>
```

레이아웃 다음 다이어그램에서와 같이 셀에는 콘텐츠를 넣을 수 있도록 행 및 열 크기를 조정 합니다.

 [ ![두 개의 셀 오른쪽에 보다 작은 왼쪽에 표시 하는 레이아웃 다이어그램](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png)

이 인해 응용 프로그램에서 실행 될 경우 다음과 같은 사용자 인터페이스:

 [ ![네 개의 셀을 표시 하는 GridLayoutDemo 스크린샷 응용 프로그램](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png)

 <a name="Specifying_Orientation" />


## <a name="specifying-orientation"></a>방향 지정

각 위의 XML에 대 한 것을 알 `TextView` 행 또는 열을 지정 하지 않습니다. 이 지정 되지 않은 경우는 `GridLayout` 방향에 따라 순서 대로 각 자식 뷰를 할당 합니다. 예를 들어 바꿔보겠습니다 GridLayout의 방향을 수평, 다음과 같이 세로로 기본값과:

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

이제는 `GridLayout` 아래와 같이 왼쪽에서 오른쪽으로 대신 각 열에서 아래쪽으로 위쪽에서 셀을 배치 합니다.

 [ ![셀이 세로 방향으로 배치 되는 방식을 보여 주는 다이어그램](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png)

이 인해 런타임 시 다음과 같은 사용자 인터페이스:

 [ ![세로 방향에 배치 하는 셀과 GridLayoutDemo의 스크린 샷](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png)

 <a name="Specifying_Explicit_Position" />


### <a name="specifying-explicit-position"></a>명시적 위치를 지정합니다.

자식 뷰 위치를 명시적으로 제어 하려는 경우는 `GridLayout`, 설정할 수 있습니다는 `layout_row` 및 `layout_column` 특성입니다. 예를 들어 다음 XML 방향에 관계 없이 (위에 표시), 첫 번째 스크린 샷에 표시 된 레이아웃에 발생 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="1" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="1"  />
</GridLayout>
```

 <a name="Specifying_spacing" />


### <a name="specifying-spacing"></a>간격 지정

두 가지 자식 사이의 간격의 뷰를 제공 하는 옵션이 있는 `GridLayout`합니다. 사용할 수는 `layout_margin` 여백을 설정 하려면 각 자식 뷰를 직접 아래와 같이 특성

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

또한 Android 4의 새로운 범용 간격 라는 보기 `Space` ´ ù. 를 사용 하려면 단순히 자식 뷰를 추가 합니다.
예를 들어 다음 XML에 추가 행이 추가 `GridLayout` 설정 하 여 해당 `rowcount` 를 3으로 추가 하 고는 `Space` 사이의 간격을 제공 하는 보기는 `TextViews`합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="3"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"         
            android:layout_height="50dp" />    
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="1" />
</GridLayout>
```

이 XML에는 공백을 만듭니다는 `GridLayout` 아래와 같이:

 [ ![스크린샷의 GridLayoutDemo 간격으로 더 큰 셀을 보여 주기 위해](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png)

새 사용 하는 장점은 `Space` 보기는 간격을 허용 하 고 않아도 모든 자식 보기에서 특성을 설정 합니다.

 <a name="Spanning_Columns_and_Rows" />


### <a name="spanning-columns-and-rows"></a>행과 열 확장

`GridLayout` 도 여러 열과 행에 걸쳐 있는 셀을 지원 합니다. 예를 들어, 단추를 포함 하는 다른 행이 추가 우리는 `GridLayout` 아래와 같이:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="4"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"        
            android:layout_height="50dp" />   
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"        
            android:layout_row="2"        
            android:layout_column="1" />
     <Button
            android:id="@+id/myButton"
            android:text="@string/hello"        
            android:layout_row="3"
            android:layout_column="0" />
</GridLayout>
```

이렇게 하면 첫 번째 열에는 `GridLayout` 여기 볼 수 있듯이 단추의 크기에 맞게 확대 되:

[ ![첫 번째 열만 스패닝 단추와 GridLayoutDemo의 스크린 샷](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png)

확장에서 첫 번째 열을 유지 하려면 해당 columnspan 다음과 같이 설정 하 여 두 개의 열으로 확장에 있는 단추를 설정할 수 있습니다.

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

에 대 한 레이아웃으로 인해이 작업은 `TextViews` 는 레이아웃 했습니다. 이전 버전에서의 맨 아래에 추가 하는 단추와 유사한는 `GridLayout` 아래와 같이:

 [ ![두 열을 확장 하는 단추와 GridLayoutDemo의 스크린 샷](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png)


## <a name="related-links"></a>관련 링크

- [GridLayoutDemo (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
