---
title: ListView 모양 사용자 지정
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: fef81fb5e5d2de79508b43a5612bf56af68d0772
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61178488"
---
# <a name="customizing-a-listviews-appearance"></a>ListView 모양 사용자 지정


## <a name="overview"></a>개요

ListView의 모양은 표시 되는 행의 레이아웃에 따라 결정 됩니다. 모양을 변경 하는 `ListView`, 다른 행 레이아웃을 사용 합니다.


## <a name="built-in-row-views"></a>기본 제공 행 보기

사용 하 여 참조할 수 있는 기본 제공 뷰 12 가지 **Android.Resource.Layout**:

- **TestListItem** &ndash; 한 최소한의 서식 있는 텍스트의 줄.

- **SimpleListItem1** &ndash; 한 줄 텍스트입니다.

- **SimpleListItem2** &ndash; 두 줄 텍스트입니다.

- **SimpleSelectableListItem** &ndash; 한 줄 텍스트를 지 원하는 단일 또는 여러 항목 선택 (API 레벨 11에서에서 추가 됨).

- **SimpleListItemActivated1** &ndash; SimpleListItem1, 비슷하지만 배경색을 나타내는 행을 선택 하는 경우 (API 레벨 11에서에서 추가).

- **SimpleListItemActivated2** &ndash; SimpleListItem2, 비슷하지만 배경색을 나타내는 행을 선택 하는 경우 (API 레벨 11에서에서 추가).

- **SimpleListItemChecked** &ndash; 선택 영역을 나타내는 확인 표시를 표시 합니다.

- **SimpleListItemMultipleChoice** &ndash; 여러 선택 항목을 나타내는 확인란을 표시 합니다.

- **SimpleListItemSingleChoice** &ndash; 표시 라디오 단추를 나타내는 상호 배타적인 선택 합니다.

- **TwoLineListItem** &ndash; 두 줄 텍스트입니다.

- **ActivityListItem** &ndash; 한 이미지를 사용 하 여 텍스트의 줄.

- **SimpleExpandableListItem** &ndash; 범주 및 각 그룹에서 행 그룹을 확장 하거나 축소할 수 있습니다.

각 기본 제공 행 보기에 연결 된 기본 제공된 스타일입니다. 다음이 스크린샷에서 각 보기에 표시 되는 방식을 보여 줍니다.

[![TestListItem, SimpleSelectableListItem, SimpleListitem1, 및 SimpleListItem2의 스크린샷](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![SimpleListItemActivated1, SimpleListItemActivated2, SimpleListItemChecked, 및 SimpleListItemMultipleChecked의 스크린샷](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![SimpleListItemSingleChoice, TwoLineListItem, ActivityListItem, 및 SimpleExpandableListItem의 스크린샷](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

합니다 **BuiltInViews/HomeScreenAdapter.cs** 샘플 파일 (에 **BuiltInViews** 솔루션) 확장할 수 없는 목록 항목 화면을 생성 하는 코드를 포함 합니다. 보기에서 설정 됩니다는 `GetView` 다음과 같이 메서드:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

컨트롤 표준 식별자를 참조 하 여 해당 뷰의 속성을 설정할 수 있습니다 `Text1`, `Text2` 하 고 `Icon` 아래의 `Android.Resource.Id` (뷰가 없거나 예외가 throw 됩니다 하는 속성을 설정 하지 않으면):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

합니다 **BuiltInExpandableViews/ExpandableScreenAdapter.cs** 샘플 파일 (에 **BuiltInViews** 솔루션) SimpleExpandableListItem 화면을 생성 하는 코드를 포함 합니다. 그룹 보기에서 설정 됩니다는 `GetGroupView` 다음과 같이 메서드:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

에 설정 된 자식 뷰가 `GetChildView` 다음과 같이 메서드:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

그룹 보기 및 자식 뷰에 대 한 속성을 표준을 참조 하 여 설정할 수 있습니다 `Text1` 고 `Text2` 위에 표시 된 것 처럼 식별자를 제어 합니다. 위와 같이 SimpleExpandableListItem 스크린 샷 줄 그룹 보기 (SimpleExpandableListItem1) 및 두 줄 자식 뷰 (SimpleExpandableListItem2)의 예를 제공 합니다. 또는 두 줄 (SimpleExpandableListItem2)에 대 한 그룹 보기를 구성할 수 있습니다 (SimpleExpandableListItem1)를 한 줄에 대 한 자식 뷰를 구성할 수 있습니다 또는 둘 다 그룹화 보기 및 자식 뷰는 동일한 개수의 줄을 가질 수 있습니다. 



## <a name="accessories"></a>Accessories

행 선택 상태를 나타내기 위해 뷰의 오른쪽에 추가 하는 보조 프로그램을 포함할 수 있습니다.

- **SimpleListItemChecked** &ndash; 표시기는 검사를 사용 하 여 단일 선택 목록을 만듭니다.

- **SimpleListItemSingleChoice** &ndash; 하나 밖에 가능 라디오 단추 형식 목록을 만듭니다.

- **SimpleListItemMultipleChoice** &ndash; 여러 항목을 선택할 수 있는 checkbox 형식 목록을 만듭니다.

앞서 언급 한 accessories, 해당 순서 대로 다음 화면에 설명 되어 있습니다.

[![SimpleListItemChecked의 스크린샷, SimpleListItemSingleChoice, 및 액세서리를 사용 하 여 SimpleListItemMultipleChoice](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

Accessories 통과 중 하나를 표시 하려면 어댑터에 필요한 레이아웃 리소스 ID를 수동으로 설정한 필요한 행에 대 한 선택 상태입니다. 이 코드 줄을 만들고 할당 하는 방법을 보여 줍니다는 `Adapter` 이러한 레이아웃 중 하나를 사용 합니다.

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView` 자체는 표시 되 고 접근자에 관계 없이 다른 선택 모드를 사용 하 고 지원 합니다. 혼동을 피하기 위해 사용 하 여 `Single` 선택 모드 `SingleChoice` 보조 프로그램 및 `Checked` 또는 `Multiple` 사용 하 여 모드를 `MultipleChoice` 스타일입니다. 선택 모드에 의해 제어 됩니다 합니다 `ChoiceMode` 의 속성을 `ListView`입니다.


### <a name="handling-api-level"></a>처리 API 수준

이전 버전의 Xamarin.Android 열거형 정수 속성으로 구현 됩니다. 최신 버전에는 훨씬 쉽게 검색 가능한 옵션에 적절 한.NET 열거형 형식을 도입 되었습니다.

API 수준에 따라 대상으로 하는, `ChoiceMode` 정수 또는 열거형입니다. 샘플 파일 **AccessoryViews/HomeScreen.cs** 에 블록을 주석으로 처리 Gingerbread API를 대상으로 하려는 경우:

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = 1; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```


### <a name="selecting-items-programmatically"></a>프로그래밍 방식으로 항목 선택

항목을 수동으로 설정 '선택' 작업을 완료 합니다 `SetItemChecked` 메서드 (호출 될 수 여러 번 여러 선택):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

코드는 또한 다중 선택 다르게에서 단일 선택 항목을 검색 해야 합니다. 행을 선택한 결정할 `Single` 모드 사용을 `CheckedItemPosition` 정수 속성:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

행에서 선택 되었는지 확인 하려면 `Multiple` 반복 해야 하는 모드를 `CheckedItemPositions` `SparseBooleanArray`합니다. 스파스 배열 이므로 항목을 포함 하는 사전 처럼 값이 변경 위치를 찾고 전체 배열을 트래버스해야 `true` 어떤 선정 된 목록에서 다음 코드 조각 에서처럼 알아야 하는 값:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```


## <a name="creating-custom-row-layouts"></a>사용자 지정 행 레이아웃 만들기

4 가지 기본 제공 행 보기는 매우 간단 합니다. 더 복잡 한 레이아웃 (예: 이메일을 트 윗 또는 연락처 정보 목록)을 표시 하려면 사용자 지정 보기를가 필요 합니다. 사용자 지정 보기에 AXML 파일로 일반적으로 선언 된 합니다 **리소스/레이아웃** 디렉터리 및 해당 리소스 Id는 사용자 지정 어댑터를 사용 하 여 로드 합니다. 보기는 사용자 지정 색, 글꼴 및 레이아웃을 사용 하 여 원하는 수의 표시 클래스 (예: TextViews, ImageViews 및 기타 컨트롤)를 포함할 수 있습니다.

이 예제에서는 다양 한 방법으로의 이전 예제에서 다릅니다.

-  상속 `Activity` 이 아니라 `ListActivity` 합니다. 에 대 한 행을 사용자 지정할 수 있습니다 `ListView` 이지만 다른 컨트롤에 포함 될 수도 있습니다는 `Activity` 레이아웃 (예: 제목, 단추 또는 기타 사용자 인터페이스 요소). 위 머리글을 추가 하는이 예제는 `ListView` 보여 줍니다.

-  AXML 레이아웃 파일을 화면에 대 한 필요 위의 예제에는 `ListActivity` 레이아웃 파일을 사용할 필요가 없습니다. 이 AXML 포함을 `ListView` 선언을 제어 합니다.

-  각 행을 렌더링 하는 AXML 레이아웃 파일에 필요 합니다. 이 AXML 파일 사용자 지정 글꼴 및 색 설정을 사용 하 여 텍스트 및 이미지 컨트롤을 포함합니다.

-  선택 된 행의 모양을 설정 하는 선택적 사용자 지정 선택기 XML 파일을 사용 합니다.

-  합니다 `Adapter` 구현에서 사용자 지정 레이아웃을 반환 합니다 `GetView` 재정의 합니다.

-  `ItemClick` 다르게 선언 되어야 합니다 (이벤트 처리기에 연결 된 `ListView.ItemClick` 를 재정의 하는 대신 `OnListItemClick` 에서 `ListActivity`).


이러한 변경 작업의 보기 및 사용자 지정 행 뷰 만들기 및 렌더링 하도록 어댑터 및 작업에 수정 내용을 다루는 다음부터 아래 자세히 나와 있습니다.


### <a name="adding-a-listview-to-an-activity-layout"></a>작업 레이아웃에는 ListView 추가

때문에 `HomeScreen` 에서 더 이상 상속 `ListActivity` HomeScreen의 뷰에 대 한 AXML 레이아웃 파일을 만들어야 하므로 기본 보기가 없는 합니다. 예를 들어 뷰 머리글을 갖습니다 (사용 하 여는 `TextView`) 및 `ListView` 데이터를 표시 합니다. 레이아웃에 정의 된 **Resources/Layout/HomeScreen.axml** 여기 표시 된 파일:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent">
    <TextView android:id="@+id/Heading"
        android:text="Vegetable Groups"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="#00000000"
        android:textSize="30dp"
        android:textColor="#FF267F00"
        android:textStyle="bold"
        android:padding="5dp"
    />
    <ListView android:id="@+id/List"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:cacheColorHint="#FFDAFF7F"
    />
</LinearLayout>
```

사용 하는 이점은 `Activity` 사용자 지정 레이아웃을 사용 하 여 (대신를 `ListActivity`) 제목 같은 화면에 추가 컨트롤을 추가할 수 있다는 점 `TextView` 이 예제입니다.


### <a name="creating-a-custom-row-layout"></a>사용자 지정 행 레이아웃 만들기

다른 AXML 레이아웃 파일은 목록 보기에 표시 되는 각 행에 대 한 사용자 지정 레이아웃을 포함 해야 합니다. 이 예에서 행 면 녹색 배경을, 밤색 텍스트 및 이미지 오른쪽 정렬 해야 합니다. 이 레이아웃을 선언 하려면 Android XML 태그에 설명 되어 **Resources/Layout/CustomView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout  xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:background="#FFDAFF7F"
   android:padding="8dp">
    <LinearLayout android:id="@+id/Text"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="10dip">
        <TextView
         android:id="@+id/Text1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textColor="#FF7F3300"
         android:textSize="20dip"
         android:textStyle="italic"
         />
        <TextView
         android:id="@+id/Text2"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textSize="14dip"
         android:textColor="#FF267F00"
         android:paddingLeft="100dip"
         />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout >
```

사용자 지정 행 레이아웃을 여러 다른 컨트롤을 포함할 수 있지만 복잡 한 디자인 하 여 스크롤 성능이 저하 될 수 있습니다 및 (특히 해야 할 경우 네트워크를 통해 로드할) 이미지를 사용 하 여 합니다. 스크롤 성능 문제를 해결 대 한 자세한 내용은 Google의 문서를 참조 하십시오.


### <a name="referencing-a-custom-row-view"></a>사용자 지정 행 뷰를 참조

이 예제에서는 사용자 지정 어댑터의 구현은 `HomeScreenAdapter.cs`합니다. 핵심 메서드는 `GetView` 리소스 ID를 사용 하 여 사용자 지정 AXML 로드 여기서 `Resource.Layout.CustomView`, 다음 각 반환 하기 전에 뷰에 있는 컨트롤에서 속성을 설정 합니다. 전체 어댑터 클래스를 표시 됩니다.

```csharp
public class HomeScreenAdapter : BaseAdapter<TableItem> {
   List<TableItem> items;
   Activity context;
   public HomeScreenAdapter(Activity context, List<TableItem> items)
       : base()
   {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
   {
       return position;
   }
   public override TableItem this[int position]
   {
       get { return items[position]; }
   }
   public override int Count
   {
       get { return items.Count; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       var item = items[position];
       View view = convertView;
       if (view == null) // no view to re-use, create new
           view = context.LayoutInflater.Inflate(Resource.Layout.CustomView, null);
       view.FindViewById<TextView>(Resource.Id.Text1).Text = item.Heading;
       view.FindViewById<TextView>(Resource.Id.Text2).Text = item.SubHeading;
       view.FindViewById<ImageView>(Resource.Id.Image).SetImageResource(item.ImageResourceId);
       return view;
   }
}
```


### <a name="referencing-the-custom-listview-in-the-activity"></a>작업에서 사용자 지정 ListView를 참조합니다.

때문에 합니다 `HomeScreen` 이제 클래스에서 상속 되 `Activity`, `ListView` 필드는 AXML에 선언 된 컨트롤에 대 한 참조를 보유할 클래스에서 선언 됩니다.

```csharp
ListView listView;
```

클래스 AXML 레이아웃을 사용자 지정 하는 작업의 다음 로드 해야 합니다를 사용 하 여 `SetContentView` 메서드. 다음으로 찾을 수는 `ListView` 컨트롤과 레이아웃에서 다음 만듭니다 어댑터를 할당 및 클릭 처리기를 할당 합니다. OnCreate 메서드에 대 한 코드는 다음과 같습니다.

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

마지막 합니다 `ItemClick` 처리기를 정의 해야 합니다;이 경우에 표시 됩니다는 `Toast` 메시지:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

결과 화면은 다음과 같습니다.

[![결과 CustomRowView 스크린샷](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



### <a name="customizing-the-row-selector-color"></a>행 선택기 색을 사용자 지정

행을 연결 하는 경우 사용자 피드백에 대 한 강조 해야 합니다. 사용자 지정 보기 배경색으로 지정 하는 경우 **CustomView.axml** 는 또한 재정의 선택 강조 표시 합니다. 이 코드의이 줄 **CustomView.axml** 하지만 연한 녹색 배경이 있는 시각적 표시가 없으므로 행을 연결 하는 경우 의미 집합:

```xml
android:background="#FFDAFF7F"
```

다시 강조 표시 동작을 사용 하도록 설정 하는 데도 사용 되는 색을 사용자 지정, 배경 특성을 대신 사용자 지정 선택기에 설정 합니다. 기본 배경색을 뿐만 아니라 강조 색 선택기를 선언 합니다. 파일 **Resources/Drawable/CustomSelector.xml** 다음 선언을 포함 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="false"
  android:state_selected="false"
  android:drawable="@color/cellback" />
<item android:state_pressed="true" >
  <shape>
     <gradient
      android:startColor="#E77A26"
        android:endColor="#E77A26"
        android:angle="270" />
  </shape>
</item>
<item android:state_selected="true"
  android:state_pressed="false"
  android:drawable="@color/cellback" />
</selector>
```

사용자 지정 선택기를 참조 하려면에 배경 특성을 변경 **CustomView.axml** 하려면:

```xml
android:background="@drawable/CustomSelector"
```

선택한 행과 해당 `Toast` 메시지 이제 다음과 같습니다.

[![선택한 행의 이름을 표시 하는 알림 메시지를 사용 하 여 주황색에서 선택한 행](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>사용자 지정 레이아웃의 깜박임을 방지

Android의 성능을 개선 하려고 `ListView` 레이아웃 정보를 캐시 하 여 스크롤. 긴 데이터의 목록을 스크롤 하는 것이 있는 경우 설정 해야 합니다 `android:cacheColorHint` 속성을는 `ListView` (하기 위해 사용자 지정 행 레이아웃의 배경으로 동일한 색 값) 활동의 AXML 정의에서 선언 합니다. 이 힌트를 포함 하는 오류는 '깜박임'까지 스크롤하면으로 사용자 지정 행의 배경색을 사용 하 여 목록을 통해 발생할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [BuiltInViews (샘플)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (샘플)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (샘플)](https://developer.xamarin.com/samples/CustomRowView/)
