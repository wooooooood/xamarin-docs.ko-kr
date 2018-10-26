---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: bbc764adc204a1f5b9ef4674a183473995be55c1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115161"
---
# <a name="gridlayout"></a>GridLayout

합니다 `GridLayout` 새로운 `ViewGroup` 아래와 같이 HTML 테이블을 비슷하게 2D 표 형태로 보기 레이아웃을 지 원하는 하위 클래스입니다.

 [![네 개의 셀을 표시 하는 GridLayout을 자릅니다.](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` 여기서 자식 뷰 모눈을 설정 위치로 행과 열 이어야 합니다를 지정 하 여 계층 구조는 플랫 보기를 사용 하 여 작동 합니다. 이러한 방식으로 *GridLayout* 하지 않고도 모든 중간 보기는 테이블 구조를 제공 하는 TableLayout에 사용 되는 테이블 행에 표시 된 대로 같은 눈금에서 보기를 배치 해야 할 수 있습니다. 플랫 계층 유지 관리 함으로써 *GridLayout* 더 좋게 레이아웃 조치가 해당 자식 뷰. 이 개념 실제로 의미 코드에서 설명 하는 예제를 살펴보겠습니다.


## <a name="creating-a-grid-layout"></a>표 레이아웃 만들기

다음 XML 몇 개 추가 `TextView` 컨트롤을 *GridLayout*합니다.

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

레이아웃 다음 다이어그램에 설명 된 대로 셀에는 해당 콘텐츠를 맞출 수 있도록 행 및 열 크기를을 조정 합니다.

 [![두 셀 오른쪽에 보다 작은 왼쪽에 표시 하는 레이아웃 다이어그램](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

이 인해 응용 프로그램에서 실행 될 때 다음 사용자 인터페이스:

 [![네 개의 셀을 표시 하는 스크린 샷의 GridLayoutDemo 앱](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>방향을 지정합니다.

위의 XML에서 각 확인 `TextView` 행 또는 열을 지정 하지 않습니다. 이 지정 되지 않은 경우는 `GridLayout` 방향에 따라 순서 대로 각 자식 뷰를 할당 합니다. 예를 들어에 이런 세로, 가로 기본값에서 변경 GridLayout의 방향을 보겠습니다.

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

이제는 `GridLayout` 아래와 같이 왼쪽에서 오른쪽으로 대신 각 열에서 아래쪽에 위쪽에서 셀을 배치 합니다.

 [![셀이 세로 방향으로 배치 되는 방식을 보여 주는 다이어그램](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

이 인해 런타임 시 다음 사용자 인터페이스:

 [![세로 방향으로 배치 하는 셀을 사용 하 여 GridLayoutDemo의 스크린 샷](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>명시적 위치를 지정합니다.

자식 뷰 위치를 명시적으로 제어 하고자 하는 경우는 `GridLayout`를 설정할 수 있습니다 해당 `layout_row` 및 `layout_column` 특성입니다. 예를 들어, 다음 XML 레이아웃 스크린샷에 표시 된 첫 번째 (위에 표시 됨), 방향에 관계 없이 발생 합니다.

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



### <a name="specifying-spacing"></a>간격 지정

간격 자식 뷰를 제공 하는 옵션의 경우 몇 가지를 `GridLayout`입니다. 사용할 수 있습니다는 `layout_margin` 여백을 설정 하려면 각 자식 뷰를 직접 아래와 같이 특성

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

또한 Android 4의 새 범용 간격 라는 보기 `Space` 출시 되었습니다. 를 사용 하려면 단순히 자식 뷰를 추가 합니다.
예를 들어, 아래 XML에 추가 행이 추가 `GridLayout` 설정 하 여 해당 `rowcount` 3 추가 `Space` 사이의 간격을 제공 하는 보기는 `TextViews`합니다.

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

이 XML의 공백은 만듭니다는 `GridLayout` 아래와 같이:

 [![스크린 샷의 GridLayoutDemo 간격을 사용 하 여 더 큰 셀을 보여 주는](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

새 사용 하는 이점은 `Space` 뷰는 간격을 허용 하 고 모든 자식 보기에서 특성을 설정할 필요 하지 않습니다.



### <a name="spanning-columns-and-rows"></a>열 및 행에 걸쳐

`GridLayout` 도 여러 열과 행에 걸쳐 있는 셀을 지원 합니다. 예를 들어, 단추를 포함 하는 다른 행 추가 `GridLayout` 아래와 같이:

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

그러면 첫 번째 열은 `GridLayout` 여기서 볼 수 있듯이 단추의 크기에 맞게 확장 되 고:

[![첫 번째 열만 확장 단추를 사용 하 여 GridLayoutDemo의 스크린 샷](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

첫 번째 열 확장 되지 않도록 하려면 다음과 같이 해당 columnspan를 설정 하 여 두 열에 걸쳐 단추를 설정할 수 있습니다.

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

레이아웃에 대 한 결과이 작업을 수행 합니다 `TextViews` 맨 아래에 추가한 단추를 사용 하 여 이전에 했습니다 레이아웃 비슷한는 `GridLayout` 아래와 같이:

 [![두 열에 걸친 단추를 사용 하 여 GridLayoutDemo의 스크린 샷](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>관련 링크

- [GridLayoutDemo (샘플)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Ice Cream Sandwich 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
