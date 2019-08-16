---
title: ListView 모양 사용자 지정
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: a2487fd0f7d90b70ec0dc1fb1978ca06a3108822
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522608"
---
# <a name="customizing-a-listviews-appearance-with-xamarinandroid"></a>Xamarin.ios를 사용 하 여 ListView의 모양 사용자 지정

ListView의 모양은 표시 되는 행의 레이아웃에 따라 결정 됩니다. 의 `ListView`모양을 변경 하려면 다른 행 레이아웃을 사용 합니다.


## <a name="built-in-row-views"></a>기본 제공 행 뷰

**Android. Layout**을 사용 하 여 참조할 수 있는 기본 제공 보기는 12 가지가 있습니다.

- **Testlistitem** &ndash; 최소 서식 지정이 적용 된 한 줄의 텍스트입니다.

- **SimpleListItem1** &ndash; 한 줄의 텍스트입니다.

- **SimpleListItem2** &ndash; 두 줄의 텍스트입니다.

- **SimpleSelectableListItem** &ndash; 단일 또는 여러 항목 선택 (API 수준 11에서 추가)을 지 원하는 텍스트 한 줄입니다.

- **SimpleListItemActivated1** &ndash; SimpleListItem1와 비슷하지만 배경 색은 행이 선택 된 경우 (API 수준 11에서 추가 됨)를 나타냅니다.

- **SimpleListItemActivated2** &ndash; SimpleListItem2와 비슷하지만 배경 색은 행이 선택 된 경우 (API 수준 11에서 추가 됨)를 나타냅니다.

- **SimpleListItemChecked** &ndash; 선택 영역을 나타내는 확인 표시를 표시 합니다.

- **SimpleListItemMultipleChoice** &ndash; 여러 선택 항목을 표시 하는 확인란을 표시 합니다.

- **SimpleListItemSingleChoice** &ndash; 상호 배타적인 선택을 나타내는 라디오 단추를 표시 합니다.

- **Twol(listitem** ) &ndash; 두 줄의 텍스트입니다.

- **Activitylistitem** &ndash; 이미지를 사용 하는 한 줄의 텍스트입니다.

- **SimpleExpandableListItem** &ndash; 는 범주별로 행을 그룹화 하 고 각 그룹은 확장 하거나 축소할 수 있습니다.

기본 제공 되는 각 행 뷰에는 연결 된 기본 제공 스타일이 있습니다. 이러한 스크린샷에는 각 보기가 표시 되는 방식이 나와 있습니다.

[![TestListItem, SimpleSelectableListItem, SimpleListitem1 및 SimpleListItem2의 스크린샷](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![SimpleListItemActivated1, SimpleListItemActivated2, SimpleListItemChecked 및 SimpleListItemMultipleChecked의 스크린샷](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![SimpleListItemSingleChoice, Twol를 Listitem, ActivityListItem 및 SimpleExpandableListItem의 스크린샷](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

**BuiltInViews/HomeScreenAdapter** 샘플 파일 ( **BuiltInViews** 솔루션)에는 확장 가능 하지 않은 목록 항목 화면을 생성 하는 코드가 포함 되어 있습니다. 뷰는 `GetView` 메서드에 다음과 같이 설정 됩니다.

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

그런 다음 표준 `Text1`컨트롤 식별자 `Icon` 를 참조 하 여 뷰의 속성을 설정할 수 있으며 `Text2` , 아래 `Android.Resource.Id` (뷰에 포함 되지 않은 속성을 설정 하지 않거나 예외가 throw 됨).

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter** 샘플 파일 ( **BuiltInViews** 솔루션)에는 SimpleExpandableListItem 화면을 생성 하는 코드가 포함 되어 있습니다. 그룹 보기는 `GetGroupView` 메서드에 다음과 같이 설정 됩니다.

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

자식 뷰는 `GetChildView` 메서드에 다음과 같이 설정 됩니다.

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

그러면 위에서 설명한 대로 표준 `Text1` 및 `Text2` 컨트롤 식별자를 참조 하 여 그룹 보기 및 자식 뷰에 대 한 속성을 설정할 수 있습니다. SimpleExpandableListItem 스크린 샷 (위에 표시 됨)에서는 한 줄 그룹 보기 (SimpleExpandableListItem1) 및 2 줄 자식 보기 (SimpleExpandableListItem2)의 예를 제공 합니다. 또는 두 줄 (SimpleExpandableListItem2)에 대해 그룹 보기를 구성 하 고 자식 보기를 한 줄 (SimpleExpandableListItem1)에 대해 구성 하거나, 두 그룹 보기와 자식 보기 모두의 줄 수를 지정할 수 있습니다. 



## <a name="accessories"></a>Accessories

행에는 선택 상태를 나타내는 보조 프로그램이 보기의 오른쪽에 추가 될 수 있습니다.

- **SimpleListItemChecked** &ndash; 확인을 표시기로 사용 하 여 단일 선택 목록을 만듭니다.

- **SimpleListItemSingleChoice** &ndash; 하나만 선택할 수 있는 라디오 단추 형식 목록을 만듭니다.

- **SimpleListItemMultipleChoice** &ndash; 여러 항목을 선택할 수 있는 checkbox 유형 목록을 만듭니다.

앞서 언급 한 액세서리는 다음 화면에서 해당 순서로 설명 됩니다.

[![액세서리를 사용 하는 SimpleListItemChecked, SimpleListItemSingleChoice 및 SimpleListItemMultipleChoice의 스크린샷](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

이러한 액세서리 중 하나를 표시 하려면 필요한 레이아웃 리소스 ID를 어댑터에 전달 하 고, 필요한 행에 대 한 선택 상태를 수동으로 설정 합니다. 이 코드 줄에서는 다음 레이아웃 중 하나를 사용 하 `Adapter` 여를 만들고 할당 하는 방법을 보여 줍니다.

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

자체 `ListView` 는 표시 되는 액세서리에 관계 없이 다양 한 선택 모드를 지원 합니다. 혼동을 피하려면 `SingleChoice` 보조 프로그램 `Single` 에서 선택 모드를 사용 하 `Checked` 고 `Multiple` `MultipleChoice` 스타일을 사용 하 여 또는 모드를 사용 합니다. 선택 모드는 `ChoiceMode` `ListView`의 속성에 의해 제어 됩니다.


### <a name="handling-api-level"></a>API 수준 처리

이전 버전의 Xamarin Android에서는 열거형을 정수 속성으로 구현 했습니다. 최신 버전에는 잠재적 옵션을 보다 쉽게 검색할 수 있도록 하는 적절 한 .NET 열거형 형식이 도입 되었습니다.

대상으로 `ChoiceMode` 하는 API 수준에 따라은 정수 또는 열거형 중 하나입니다. **AccessoryViews/HomeScreen** 샘플 파일에는 Gingerbread API를 대상으로 하려는 경우 주석 처리 된 블록이 있습니다.

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

' 선택 ' 된 항목을 `SetItemChecked` 메서드를 사용 하 여 수동으로 설정 (여러 번 선택 하는 경우 여러 번 호출 될 수 있음):

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

또한 코드는 여러 선택 항목과 다르게 단일 선택을 검색 해야 합니다. 모드에서 `Single` 선택 된 행을 확인 하려면 정수 속성을 `CheckedItemPosition` 사용 합니다.

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

모드에서 `Multiple` 선택 된 행을 확인 하려면를 반복 `CheckedItemPositions` `SparseBooleanArray`해야 합니다. 스파스 배열은 값이 변경 된 항목만 포함 하는 사전과 유사 하므로 다음 코드 조각에 나와 있는 것 처럼 목록에서 선택 된 항목을 `true` 확인 하려면 값을 검색 하는 전체 배열을 트래버스 해야 합니다.

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```


## <a name="creating-custom-row-layouts"></a>사용자 지정 행 레이아웃 만들기

네 개의 기본 제공 행 뷰는 매우 간단 합니다. 더 복잡 한 레이아웃을 표시 하려면 (예: 메일 목록, 트 윗 또는 연락처 정보) 사용자 지정 보기가 필요 합니다. 일반적으로 사용자 지정 뷰는 **리소스/레이아웃** 디렉터리에서 axml 파일로 선언 된 다음 사용자 지정 어댑터에서 해당 리소스 Id를 사용 하 여 로드 됩니다. 보기에는 사용자 지정 색, 글꼴 및 레이아웃을 사용 하 여 여러 표시 클래스 (예: TextViews, ImageViews 및 기타 컨트롤)를 포함할 수 있습니다.

이 예는 다양 한 방법으로 이전 예제와 다릅니다.

- 는이 `Activity` 아니라 `ListActivity` 에서 상속 됩니다. 모든 `ListView` 항목에 대 한 행을 사용자 지정할 수 있지만 다른 컨트롤은 `Activity` 레이아웃 (예: 머리글, 단추 또는 기타 사용자 인터페이스 요소)에 포함 될 수도 있습니다. 이 예제에서는 `ListView` 을 설명 하기 위해 위의 제목을 추가 합니다.

- 화면에 대 한 AXML 레이아웃 파일이 필요 합니다. 이전 예제에서에는 `ListActivity` 레이아웃 파일이 필요 하지 않습니다. 이 axml에는 `ListView` 컨트롤 선언이 포함 되어 있습니다.

- 각 행을 렌더링 하려면 AXML 레이아웃 파일이 필요 합니다. 이 AXML 파일에는 사용자 지정 글꼴 및 색 설정을 사용 하는 텍스트 및 이미지 컨트롤이 포함 되어 있습니다.

- 선택적 사용자 지정 선택기 XML 파일을 사용 하 여 행이 선택 될 때 표시 되는 모양을 설정 합니다.

- `Adapter` 구현은 재정의`GetView` 에서 사용자 지정 레이아웃을 반환 합니다.

- `ItemClick`는 다르게 선언 해야 합니다. 이벤트 처리기는에서 `ListView.ItemClick` `ListActivity`재정의 `OnListItemClick` 되는 대신에 연결 됩니다.


이러한 변경 내용은 활동의 뷰 및 사용자 지정 행 뷰를 만든 다음 어댑터 및이를 렌더링 하기 위한 작업에 대 한 수정 사항을 포함 하 여 아래에 자세히 설명 되어 있습니다.


### <a name="adding-a-listview-to-an-activity-layout"></a>활동 레이아웃에 ListView 추가

에서는 `HomeScreen` 더 이상 `ListActivity` 상속 되지 않으므로 기본 뷰가 없으므로 HomeScreen 보기에 대 한 레이아웃 axml 파일을 만들어야 합니다. 이 예의 경우 보기에는 `TextView` `ListView` 를 사용 하 여 데이터를 표시 하는 머리글이 포함 됩니다. 레이아웃은 다음에 표시 된 **Resources/layout/HomeScreen** 파일에서 정의 됩니다.

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

사용자 지정 레이아웃을 사용 `Activity` 하 여를 `ListActivity`사용 하는 경우의 장점 (대신)은이 예의 머리글과 `TextView` 같이 화면에 컨트롤을 더 추가할 수 있습니다.


### <a name="creating-a-custom-row-layout"></a>사용자 지정 행 레이아웃 만들기

또 다른 XML 레이아웃 파일은 목록 뷰에 표시 되는 각 행에 대 한 사용자 지정 레이아웃을 포함 해야 합니다. 이 예제에서 행은 녹색 배경, 밤색 텍스트 및 오른쪽 맞춤 이미지를 갖게 됩니다. 이 레이아웃을 선언 하는 Android XML 태그는 **Resources/layout/CustomView에서 설명 합니다. axml**:

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

사용자 지정 행 레이아웃에는 다양 한 컨트롤이 포함 될 수 있지만 스크롤 성능은 복잡 한 디자인 및 이미지 (특히 네트워크를 통해 로드 해야 하는 경우)의 영향을 받을 수 있습니다. 스크롤 성능 문제 해결에 대 한 자세한 내용은 Google의 문서를 참조 하세요.


### <a name="referencing-a-custom-row-view"></a>사용자 지정 행 뷰 참조

사용자 지정 어댑터 예의 구현은에 `HomeScreenAdapter.cs`있습니다. 키 메서드 `GetView` 는 리소스 ID `Resource.Layout.CustomView`를 사용 하 여 사용자 지정 axml을 로드 한 다음 반환 하기 전에 뷰의 각 컨트롤에 대 한 속성을 설정 하는 위치입니다. 전체 어댑터 클래스가 표시 됩니다.

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


### <a name="referencing-the-custom-listview-in-the-activity"></a>활동에서 사용자 지정 ListView 참조

이제 클래스 `HomeScreen` 는에서 `Activity`상속 되므로 axml에 선언 된 컨트롤에 대 한 참조를 포함 하기 위해 클래스에서 필드가선언됩니다.`ListView`

```csharp
ListView listView;
```

그런 다음 클래스는 메서드를 `SetContentView` 사용 하 여 작업의 사용자 지정 레이아웃 axml을 로드 해야 합니다. 그런 다음 레이아웃에서 컨트롤 `ListView` 을 찾은 다음 어댑터를 만들어 할당 하 고 클릭 처리기를 할당 합니다. OnCreate 메서드에 대 한 코드가 다음과 같이 표시 됩니다.

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

마지막으로 `ItemClick` 처리기를 정의 해야 합니다 .이 경우에는 다음과 같이 `Toast` 메시지를 표시 하기만 하면 됩니다.

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

결과 화면은 다음과 같습니다.

[![결과 CustomRowView의 스크린샷](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



### <a name="customizing-the-row-selector-color"></a>행 선택기 색 사용자 지정

행이 작업 될 때 사용자 의견에 대해 강조 표시 되어야 합니다. 사용자 지정 보기가 **Customview** 의 배경색으로 지정 된 경우에는 선택 강조 표시도 재정의 됩니다. 이 코드 줄은 **Customview에 있습니다. axml** 은 배경을 연한 녹색으로 설정 하지만 행이 작업 될 때 시각적 표시기가 없음을 의미 하기도 합니다.

```xml
android:background="#FFDAFF7F"
```

강조 표시 동작을 다시 사용 하도록 설정 하 고 사용 되는 색을 사용자 지정 하려면 배경 특성을 사용자 지정 선택기로 설정 합니다. 이 선택기는 기본 배경색과 강조 색을 모두 선언 합니다. File **Resources/그릴 때/CustomSelector xml** 에는 다음 선언이 포함 됩니다.

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

사용자 지정 선택기를 참조 하려면 **Customview. axml** 에서 다음과 같이 background 특성을 변경 합니다.

```xml
android:background="@drawable/CustomSelector"
```

선택한 행과 해당 `Toast` 메시지는 이제 다음과 같이 표시 됩니다.

[![선택한 행의 이름을 표시 하는 알림 메시지가 포함 된 주황색의 선택 된 행](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>사용자 지정 레이아웃에 대 한 깜박임 방지

Android는 레이아웃 정보를 캐싱하여 `ListView` 스크롤 성능을 향상 시 키 려 고 합니다. 데이터의 스크롤 목록이 긴 경우 작업의 axml 정의에서 `android:cacheColorHint` `ListView` 선언에 대 한 속성을 사용자 지정 행 레이아웃의 배경과 동일한 색 값으로 설정 해야 합니다. 사용자가 사용자 지정 행 배경색이 있는 목록을 스크롤할 때이 힌트를 포함 하지 않으면 ' 깜박임 '이 발생할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [BuiltInViews (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/builtinviews)
- [AccessoryViews (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/accessoryviews)
- [CustomRowView (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/customrowview)
