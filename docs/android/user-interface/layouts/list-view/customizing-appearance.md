---
title: "ListView의 모양 사용자 지정"
ms.topic: article
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 18c53ed6428eff911420c696d45b341d8e0fa5c1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="customizing-a-listviews-appearance"></a>ListView의 모양 사용자 지정

<a name="overview" />

## <a name="overview"></a>개요

ListView의 모양은 표시 되는 행의 레이아웃에 따라 결정 됩니다. 모양을 변경 하는 `ListView`, 다른 행 레이아웃을 사용 합니다.

<a name="Built-in_Row_Views" />

## <a name="built-in-row-views"></a>기본 제공 행 보기

사용 하 여 참조 될 수 있는 기본 제공 12 개의 보기가 **Android.Resource.Layout**:

- **TestListItem** &ndash; 한 최소한의 서식 있는 텍스트 줄입니다.

- **SimpleListItem1** &ndash; 한 줄 텍스트입니다.

- **SimpleListItem2** &ndash; 두 줄 텍스트입니다.

- **SimpleSelectableListItem** &ndash; 한 줄 텍스트를 지 원하는 단일 또는 여러 항목 선택 (API 수준 11에서에서 추가 됨).

- **SimpleListItemActivated1** &ndash; SimpleListItem1, 유사 하지만 배경색 나타내는 행을 선택 (API 수준 11에서에서 추가 됨).

- **SimpleListItemActivated2** &ndash; SimpleListItem2, 유사 하지만 배경색 나타내는 행을 선택 (API 수준 11에서에서 추가 됨).

- **SimpleListItemChecked** &ndash; 선택을 나타냅니다에 확인 표시를 표시 합니다.

- **SimpleListItemMultipleChoice** &ndash; 여러 선택 항목을 나타내는 확인란을 표시 합니다.

- **SimpleListItemSingleChoice** &ndash; 표시 라디오 단추를 나타내는 상호 배타적인 선택 합니다.

- **TwoLineListItem** &ndash; 두 줄 텍스트입니다.

- **ActivityListItem** &ndash; 한 줄 텍스트 이미지와 함께 합니다.

- **SimpleExpandableListItem** &ndash; 범주 및 각 그룹에서 행 그룹을 확장 하거나 축소할 수 있습니다.

각 기본 제공 행 보기에 연결 된 기본 제공된 스타일입니다. 이러한 스크린샷 각 보기 표시 되는 방식을 보여 줍니다.

[![TestListItem, SimpleSelectableListItem, SimpleListitem1, 및 SimpleListItem2 스크린샷](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png)

[![SimpleListItemActivated1, SimpleListItemActivated2, SimpleListItemChecked, 및 SimpleListItemMultipleChecked 스크린샷](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png)

[![SimpleListItemSingleChoice, TwoLineListItem, ActivityListItem, 및 SimpleExpandableListItem 스크린샷](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png)

**BuiltInViews/HomeScreenAdapter.cs** 샘플 파일 (에 **BuiltInViews** 솔루션)를 확장할 수 없는 목록 항목 화면을 만드는 코드를 포함 합니다. 보기에 설정 된 `GetView` 다음과 같이 메서드:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

그런 다음 표준 제어 식별자를 참조 하 여 해당 뷰의 속성을 설정할 수 `Text1`, `Text2` 및 `Icon` 아래 `Android.Resource.Id` (보기가 포함 하지 않거나 예외가 throw 하는 속성 설정 안 함):

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter.cs** 샘플 파일 (에 **BuiltInViews** 솔루션) SimpleExpandableListItem 화면을 생성 하기 위해 코드를 포함 합니다. 에 설정 된 그룹 뷰가 `GetGroupView` 다음과 같이 메서드:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

에 설정 된 자식 뷰가 `GetChildView` 다음과 같이 메서드:

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

그런 다음 표준 참조 하 여 그룹 뷰와 자식 뷰에 대 한 속성을 설정할 수 `Text1` 및 `Text2` 위와 같이 식별자를 제어 합니다. 위의 SimpleExpandableListItem 스크린 샷 한 줄 그룹 보기 (SimpleExpandableListItem1) 및 두 줄 자식 뷰 (SimpleExpandableListItem2)의 예를 제공 합니다. 또는 두 줄 (SimpleExpandableListItem2)에 대 한 그룹 보기를 구성할 수 있습니다 (SimpleExpandableListItem1) 한 줄에 대 한 자식 뷰를 구성할 수 있습니다 또는 보기 그룹 둘 다 및 자식 뷰에 같은 줄 수를 포함할 수 있습니다. 


<a name="Accessories" />

## <a name="accessories"></a>보조 프로그램

행 선택 상태를 표시 하는 보기의 오른쪽에 추가 하는 액세서리를 가질 수 있습니다.

- **SimpleListItemChecked** &ndash; 지표로 확인을 사용 하는 단일 선택 목록을 만듭니다.

- **SimpleListItemSingleChoice** &ndash; 만듭니다 라디오 단추 형식 목록 하나만 선택이 가능 합니다.

- **SimpleListItemMultipleChoice** &ndash; 여러 선택 항목은 가능한 checkbox 형식 목록을 만듭니다.

앞에서 언급 한 accessories는 해당 순서 대로 다음 화면에서 확인할 수 있습니다.

[![SimpleListItemChecked 스크린샷, SimpleListItemSingleChoice, 및 액세서리와 SimpleListItemMultipleChoice](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png)

이러한 보조 프로그램 패스 중 하나를 표시 하려면 어댑터에 필요한 레이아웃 리소스 ID 다음 수동으로 설정 필요한 행에 대 한 선택 상태입니다. 이 줄의 코드를 만들고 할당 하는 방법을 보여 줍니다는 `Adapter` 이러한 레이아웃 중 하나를 사용 하 여:

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView` 자체에서는 표시 되 고 접근자에 관계 없이 다른 선택 모드를 사용 하 고 지원 합니다. 혼동을 피하기 위해 사용 하 여 `Single` 선택 모드와 `Checked` 및 `SingleChoice` 액세서리 및 `Multiple` 모드는 `MultipleChoice` 스타일입니다. 선택 모드에 의해 제어 되는 `ChoiceMode` 속성은 `ListView`합니다.

<a name="Handling_API_Level" />

### <a name="handling-api-level"></a>처리 API 레벨

이전 버전의 Xamarin.Android 정수 속성으로 열거형을 구현 합니다. 최신 버전에는 훨씬 쉽게 가능한 옵션을 검색 하려면 적절 한.NET 열거형 형식 소개 했습니다.

대상으로 API 수준 `ChoiceMode` 정수 또는 열거 됩니다. 샘플 파일 **AccessoryViews/HomeScreen.cs** 에 블록 주석으로 처리 인형 API를 대상으로 할 경우:

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```

<a name="Selecting_Items_Programmatically" />

### <a name="selecting-items-programmatically"></a>프로그래밍 방식으로 항목 선택

항목을 수동으로 설정 '선택' 작업을 완료 된 `SetItemChecked` 메서드 (것 수 여러 번 호출할 수에 대 한 다중 선택):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

코드는 또한 여러 선택 항목에서 다르게 단일 선택 항목을 검색 해야 합니다. 행에서 선택 하는 확인 하려면 `Single` 모드 사용은 `CheckedItemPosition` 정수 속성:

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

행에서 선택 되었는지 확인 하려면 `Multiple` 반복 해야 하는 모드는 `CheckedItemPositions` `SparseBooleanArray`합니다. 스파스 배열 이므로 항목을 포함 하는 사전 처럼 값이 변경 위치를 찾고 배열 전체를 통과 해야 `true` 가 되어 선택한 항목 목록에서 다음 코드 조각의 설명과 같이 알아야 하는 값:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```

<a name="Creating_Custom_Row_Layouts" />

## <a name="creating-custom-row-layouts"></a>행 사용자 지정 레이아웃 만들기

4 개의 기본 제공 행 보기는 매우 간단 합니다. 더 복잡 한 레이아웃 (예: 전자 메일, 또는 윗 또는 연락처 정보 목록)을 표시 하려면 사용자 지정 보기가 필요 합니다. 사용자 지정 보기에 AXML 파일로 선언 일반적으로 **리소스/레이아웃** 디렉터리 및 해당 리소스 Id를 사용자 지정 어댑터를 사용 하 여 다음 로드 합니다. 보기는 사용자 지정 색, 글꼴 및 레이아웃 표시 클래스 (예: TextViews, ImageViews 및 기타 컨트롤)을 개수에 관계 없이 포함 될 수 있습니다.

이 예제에서는 여러 가지 방법으로 이전 예제에 나오는 마다 다릅니다.

-  상속 `Activity` 이 아니라 `ListActivity` 합니다. 에 대 한 행을 사용자 지정할 수 있습니다 `ListView` 다른 컨트롤에 포함 될 수도 있지만는 `Activity` 레이아웃 (예: 제목, 단추 또는 다른 사용자 인터페이스 요소). 위의 머리글을 추가 하는이 예제는 `ListView` 을 보여 줍니다.

-  화면에 대 한 AXML 레이아웃 파일 필요 앞의 예제에는 `ListActivity` 레이아웃 파일은 필요 하지 않습니다. 이 AXML 포함 한 `ListView` 선언을 제어 합니다.

-  각 행을 렌더링 하는 AXML 레이아웃 파일이 필요 합니다. 이 AXML 파일 사용자 지정 글꼴 및 색 설정을 사용 하 여 텍스트 및 이미지 컨트롤을 포함합니다.

-  선택 된 행의 모양을 설정 하는 선택적 사용자 지정 선택기 XML 파일을 사용 합니다.

-  `Adapter` 구현에서 사용자 지정 레이아웃을 반환 합니다.는 `GetView` 재정의 합니다.

-  `ItemClick` 다르게 선언 되어야 합니다 (에 이벤트 처리기가 연결 `ListView.ItemClick` 는 재정의 하는 대신 `OnListItemClick` 에 `ListActivity`).


이러한 변경 내용은 아래에서 자세히, 활동의 뷰 및 사용자 지정 행 뷰 만들기 및 수정 프로그램 렌더링할 어댑터 및 활동에 설명한 다음으로 시작 합니다.

<a name="Adding_a_ListView_to_an_Activity_Layout" />

### <a name="adding-a-listview-to-an-activity-layout"></a>ListView 레이아웃에 추가 된 활동

때문에 `HomeScreen` 에서 더 이상 상속 받지 `ListActivity` HomeScreen의 보기에 대 한 레이아웃 AXML 파일을 만들어야 하므로 기본 보기 되어 있지 않습니다. 이 예제에서는 보기 제목 해야 합니다 (사용 하 여는 `TextView`) 및 `ListView` 데이터를 표시 합니다. 레이아웃에 정의 된 **Resources/Layout/HomeScreen.axml** 여기에 표시 된 파일:

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

사용 하는 장점은 `Activity` 사용자 지정 레이아웃 (대신는 `ListActivity`) 제목 등 화면에 추가 컨트롤을 추가할 수 있다는 점 `TextView` 이 예에서 합니다.

<a name="Creating_a_Custom_Row_Layout" />

### <a name="creating-a-custom-row-layout"></a>행 사용자 지정 레이아웃 만들기

다른 AXML 레이아웃 파일은 목록 보기에 표시 될 각 행에 대해 사용자 지정 레이아웃을 포함 해야 합니다. 이 예제에서 녹색 배경, 갈색 텍스트 및 이미지 오른쪽 맞춤 행 포함 됩니다. 이 레이아웃을 선언 하는 Android XML 태그에 설명 된 **Resources/Layout/CustomView.axml**:

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

사용자 지정 행 레이아웃 서로 다른 여러 컨트롤을 포함할 수 있는 동안 스크롤 성능 영향을 받을 수 복잡 한 디자인 및 (특히 경우 네트워크를 통해 로드 될 필요가) 이미지를 사용 하 여 합니다. 스크롤 성능 문제 해결에 자세한 내용은 Google의 문서를 참조 하십시오.

<a name="Referencing_a_Custom_Row_View" />

### <a name="referencing-a-custom-row-view"></a>사용자 지정 행 뷰를 참조

이 예제에서는 사용자 지정 어댑터 구현 `HomeScreenAdapter.cs`합니다. 핵심 메서드는 `GetView` 리소스 ID를 사용 하 여 사용자 지정 AXML 로드 여기서 `Resource.Layout.CustomView`, 다음에서 각 반환 하기 전에 뷰에서 컨트롤의 속성을 설정 합니다. 전체 어댑터 클래스 표시 됩니다.

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

<a name="Referencing_the_Custom_ListView_in_the_Activity" />

### <a name="referencing-the-custom-listview-in-the-activity"></a>사용자 지정 활동에서 ListView 참조

때문에 `HomeScreen` 에서 이제 클래스를 상속 `Activity`, `ListView` 필드가 AXML에 선언 된 컨트롤에 대 한 참조를 보유 하는 클래스에 정의 되었습니다.

```csharp
ListView listView;
```

클래스를 활동의 사용자 지정 레이아웃 AXML 로드 해야 합니다를 사용 하는 `SetContentView` 메서드. 찾을 수 있는 것은 `ListView` 레이아웃의 컨트롤 및 어댑터를 할당 하 만듭니다 클릭 처리기를 할당 합니다. OnCreate 메서드에 대 한 코드는 다음과 같습니다.

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

마지막으로 `ItemClick` 처리기를 정의 해야 합니다;이 경우에 표시 됩니다는 `Toast` 메시지:

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

결과 화면은 다음과 같습니다.

[![결과 CustomRowView의 스크린 샷](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png)


<a name="Customizing_the_Row_Selector_Color" />

### <a name="customizing-the-row-selector-color"></a>행 선택기 색을 사용자 지정

행에 접촉 될 때 사용자에 게 피드백에 대 한 강조 해야 합니다. 사용자 지정 보기 배경색으로 지정 하는 경우 **CustomView.axml** 않습니다, 또한 재정의 선택 강조 표시 합니다. 이 줄의 코드에서 **CustomView.axml** 연한 녹색 있지만 배경 행에 접촉 될 때 없는 시각적 표시기는 또한 의미 설정:

```xml
android:background="#FFDAFF7F"
```

다시 강조 동작을 사용 하도록 설정 하 고도 사용 되는 색 사용자 지정 하는 배경 특성을 설정 사용자 지정 선택기 대신에. 으로 기본 배경색의 강조 색 선택기 선언 됩니다. 파일 **Resources/Drawable/CustomSelector.xml** 선언이 있습니다.

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

사용자 지정 선택기를 참조 하려면에 배경 특성을 변경 **CustomView.axml** 에:

```xml
android:background="@drawable/CustomSelector"
```

선택한 행과 해당 `Toast` 메시지 지금은 다음과 같습니다.

[![선택한 행의 이름을 표시 하는 알림 메시지와 함께 주황색으로 선택 된 행](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png)


<a name="Preventing_Flickering_on_Custom_Layouts" />

### <a name="preventing-flickering-on-custom-layouts"></a>사용자 지정 레이아웃에 깜박이 방지

Android의 성능을 개선 하려고 `ListView` 레이아웃 정보를 캐시 하 여 스크롤입니다. 긴 데이터의 목록을 스크롤 있는 경우 설정 해야는 `android:cacheColorHint` 속성에는 `ListView` (하기 위해 사용자 지정 행 레이아웃의 배경으로 동일한 색 값) 활동의 AXML 정의에서 선언 합니다. 이 힌트를 포함 하는 오류는 '깜박임' 사용자 스크롤으로 목록이 사용자 지정 행의 배경색으로를 통해 발생할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [BuiltInViews (샘플)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (샘플)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (샘플)](https://developer.xamarin.com/samples/CustomRowView/)
