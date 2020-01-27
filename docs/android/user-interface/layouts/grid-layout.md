---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 13271b50353d95ecd2db40e25d549788111530f7
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725005"
---
# <a name="xamarinandroid-gridlayout"></a>Xamarin Android GridLayout

`GridLayout`은 아래와 같이 HTML 테이블과 비슷하게 2D 그리드에서 보기 레이아웃을 지 원하는 새로운 `ViewGroup` 하위 클래스입니다.

 [네 개의 셀을 표시 하는 ![잘린 GridLayout](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout`는 플랫 뷰 계층 구조와 함께 작동 합니다. 여기서 자식 뷰는 행과 열을 지정 하 여 표에 위치를 설정 합니다. 이러한 방식으로 *GridLayout* 는 중간 뷰가 TableLayout에서 사용 되는 테이블 행에 표시 되는 것과 같은 테이블 구조를 제공 하지 않고도 그리드에 뷰를 배치할 수 있습니다. *GridLayout* 는 플랫 계층을 유지 관리 하 여 자식 뷰를 더 빠르게 레이아웃 할 수 있습니다. 실제로 코드에서이 개념의 의미를 설명 하는 예제를 살펴보겠습니다.

## <a name="creating-a-grid-layout"></a>그리드 레이아웃 만들기

다음 XML은 *GridLayout*에 여러 개의 `TextView` 컨트롤을 추가 합니다.

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

다음 다이어그램에 표시 된 것 처럼 레이아웃은 행과 열 크기를 조정 하 여 셀이 해당 내용에 맞게 조정 됩니다.

 [왼쪽의 두 셀을 표시 하는 레이아웃의 ![오른쪽 보다 작음](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

그러면 응용 프로그램에서 실행 될 때 다음 사용자 인터페이스가 생성 됩니다.

 [4 개 셀을 표시 하는 GridLayoutDemo 앱의 ![스크린샷](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)

## <a name="specifying-orientation"></a>방향 지정

위의 XML에서 각 `TextView`는 행 이나 열을 지정 하지 않습니다. 이를 지정 하지 않으면 `GridLayout`는 방향에 따라 각 자식 뷰를 순서 대로 할당 합니다. 예를 들어 GridLayout의 방향을 기본값인 가로에서 다음과 같이 세로로 변경 하겠습니다.

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

이제 `GridLayout`은 아래와 같이 왼쪽에서 오른쪽이 아니라 각 열에서 위쪽에서 아래쪽으로 셀을 배치 합니다.

 [셀이 세로 방향으로 배치 되는 방식을 보여 주는 ![다이어그램](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

이렇게 하면 런타임 시 다음 사용자 인터페이스가 생성 됩니다.

 [세로 방향으로 배치 된 셀이 있는 GridLayoutDemo의 ![스크린샷](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)

### <a name="specifying-explicit-position"></a>명시적 위치 지정

`GridLayout`에서 자식 뷰의 위치를 명시적으로 제어 하려는 경우 `layout_row` 및 `layout_column` 특성을 설정할 수 있습니다. 예를 들어 다음 XML은 방향에 관계 없이 첫 번째 스크린샷 (위에 표시 됨)에 표시 되는 레이아웃을 생성 합니다.

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

`GridLayout`의 자식 보기 사이에 간격을 지정 하는 몇 가지 옵션이 있습니다. 다음과 같이 `layout_margin` 특성을 사용 하 여 각 자식 보기의 여백을 직접 설정할 수 있습니다.

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

또한 Android 4에서는 이제 `Space` 라는 새로운 범용 간격 보기를 사용할 수 있습니다. 이를 사용 하려면 자식 뷰로 추가 하기만 하면 됩니다.
예를 들어 아래 XML은 `rowcount`를 3으로 설정 하 여 `GridLayout`에 추가 행을 추가 하 고 `TextViews`사이 간격을 제공 하는 `Space` 뷰를 추가 합니다.

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

이 XML은 아래와 같이 `GridLayout` 간격을 만듭니다.

 [간격이 있는 큰 셀을 보여 주는 GridLayoutDemo의 ![스크린샷](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

새 `Space` 보기를 사용 하는 경우의 혜택은 간격을 허용 하 고 모든 자식 보기에서 특성을 설정 하지 않아도 된다는 것입니다.

### <a name="spanning-columns-and-rows"></a>열 및 행 확장

`GridLayout`는 여러 열 및 행에 걸쳐 있는 셀도 지원 합니다. 예를 들어 아래와 같이 단추를 포함 하는 다른 행을 `GridLayout`에 추가 한다고 가정 합니다.

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

그러면 다음과 같이 단추의 크기에 맞게 `GridLayout`의 첫 번째 열이 확장 됩니다.

[첫 번째 열만으로 확장 된 단추가 있는 GridLayoutDemo의 ![스크린샷](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

첫 번째 열을 스트레치 하지 않도록 하려면 다음과 같이 columnspan을 설정 하 여 두 열을 확장 하는 단추를 설정할 수 있습니다.

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

이렇게 하면 이전에 있던 레이아웃과 유사한 `TextViews` 레이아웃이 표시 되 고 아래와 같이 `GridLayout` 아래쪽에 단추가 추가 됩니다.

 [두 열을 모두 확장 하는 단추가 있는 GridLayoutDemo의 ![스크린샷](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)

## <a name="related-links"></a>관련 링크

- [GridLayoutDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/gridlayoutdemo)
