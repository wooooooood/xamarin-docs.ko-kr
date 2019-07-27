---
title: Xamarin Android ListView를 데이터로 채우기
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: dff2efe687fde16903df19fefad2e2589c888086
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510116"
---
# <a name="populating-a-xamarinandroid-listview-with-data"></a>Xamarin Android ListView를 데이터로 채우기

에 행을 추가 하려면 `ListView` 레이아웃에 행을 추가 하 고 자신을 채우기 위해 `IListAdapter` 를 `ListView` 호출 하는 메서드를 사용 하 여를 구현 해야 합니다. Android에는 사용자 지정 `ListActivity` 레이아웃 `ArrayAdapter` XML 또는 코드를 정의 하지 않고 사용할 수 있는 기본 제공 및 클래스가 포함 되어 있습니다. 클래스 `ListActivity` 는를 `ListView` 자동으로 만들고 속성을 `ListAdapter` 노출 하 여 어댑터를 통해 표시할 행 뷰를 제공 합니다.

기본 제공 어댑터는 각 행에 사용 되는 매개 변수로 뷰 리소스 ID를 사용 합니다. 기본 제공 리소스 (예: `Android.Resource.Layout` )를 사용할 수 있으므로 사용자가 직접 작성 하지 않아도 됩니다.

## <a name="using-listactivity-and-arrayadapterltstringgt"></a>Listactivity 및 arrayadapter&lt;문자열 사용&gt;

다음 예 **에서는** 이러한 클래스를 사용 하 여 몇 줄의 코드만으로를 `ListView` 표시 하는 방법을 보여 줍니다.

```csharp
[Activity(Label = "BasicTable", MainLauncher = true, Icon = "@drawable/icon")]
public class HomeScreen : ListActivity {
   string[] items;
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       items = new string[] { "Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers" };
       ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItem1, items);
   }
   protected override void OnListItemClick(ListView l, View v, int position, long id)
}
```


### <a name="handling-row-clicks"></a>행 클릭 처리

일반적으로 `ListView` 는 사용자가 특정 작업 (예: 노래 재생, 연락처 호출 또는 다른 화면 표시)을 수행 하기 위해 행을 터치 할 수 있습니다. 사용자 터치에 응답 하려면 다음과 같이에서 구현 된 메서드를 하나 이상 사용 `ListActivity` 해야 합니다. &ndash; `OnListItemClick` &ndash;

[![SimpleListItem 스크린샷](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

이제 사용자가 행을 터치 할 수 있으며 `Toast` 경고가 표시 됩니다.

[![행이 작업 될 때 표시 되는 알림 스크린샷](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>ListAdapter 구현

`ArrayAdapter<string>`는 간단 하기 때문에 좋지만 매우 제한적입니다. 그러나 바인딩하려는 문자열이 아닌 비즈니스 엔터티 컬렉션이 있는 경우가 종종 있습니다.
예를 들어 데이터가 Employee 클래스 컬렉션으로 구성 된 경우 목록에서 각 직원의 이름만 표시 하는 것이 좋습니다. 표시 되는 데이터 `ListView` 를 제어 하기 위해의 동작을 사용자 지정 하려면 다음 네 가지 항목을 `BaseAdapter` 재정의 하는 서브 클래스를 구현 해야 합니다.

-   **개수** &ndash; 데이터에 있는 행 수를 컨트롤에 알리기 위해

-   **Getview** &ndash; 각 행에 대 한 뷰를 반환 하려면 데이터로 채워집니다.
    이 메서드에는 `ListView` 다시 사용 하기 위해 사용 되지 않는 기존 행을 전달 하기 위해에 대 한 매개 변수가 있습니다.

-   **Getitemid** &ndash; 행 식별자를 반환 합니다 (원하는 긴 값이 될 수 있지만 일반적으로 행 번호).

-   **이 [int]** 인덱서 &ndash; 를 사용 하 여 특정 행 번호와 연결 된 데이터를 반환 합니다.

**Basictableadapter/HomeScreenAdapter** 의 예제 코드는 다음을 서브 클래스 `BaseAdapter`하는 방법을 보여 줍니다.

```csharp
public class HomeScreenAdapter : BaseAdapter<string> {
   string[] items;
   Activity context;
   public HomeScreenAdapter(Activity context, string[] items) : base() {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
  {
       return position;
   }
   public override string this[int position] {  
       get { return items[position]; }
   }
   public override int Count {
       get { return items.Length; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       View view = convertView; // re-use an existing view, if one is available
      if (view == null) // otherwise create a new one
           view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
       view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
       return view;
   }
}
```


### <a name="using-a-custom-adapter"></a>사용자 지정 어댑터 사용

사용자 지정 어댑터를 사용 하는 것은 기본 제공 `ArrayAdapter`된 `context` 와 비슷하며, 표시할 값 `string[]` 의 및를 전달 합니다.

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

이 예제에서는 동일한 행 레이아웃 (`SimpleListItem1`)을 사용 하기 때문에 결과 응용 프로그램은 이전 예제와 동일 하 게 보입니다.


### <a name="row-view-re-use"></a>행 뷰 다시 사용

이 예제에는 6 개의 항목만 있습니다. 화면은 8에 맞출 수 있으므로 행을 다시 사용 하지 않아도 됩니다. 그러나 수백 또는 수천 개의 행을 표시 하는 경우에는 한 번에 8 개만 화면에 맞출 때 메모리가 `View` 낭비 될 수 있습니다. 이러한 상황을 방지 하기 위해 행이 화면에서 사라질 때 해당 뷰는 다시 사용 하기 위해 큐에 배치 됩니다. 사용자가 스크롤하면 새 보기를 `ListView` &ndash; 요청 `GetView` 하는 호출에서 `convertView` 사용할 수 있는 경우 매개 변수에서 사용 되지 않는 뷰를 전달 합니다. 이 값이 null 이면 코드에서 새 뷰 인스턴스를 만들어야 합니다. 그렇지 않으면 해당 개체의 속성을 다시 설정 하 고 다시 사용할 수 있습니다.

메서드 `GetView` 는 다음 패턴에 따라 행 뷰를 다시 사용 해야 합니다.

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

사용자 지정 어댑터 구현은  긴 목록을 표시할 때 메모리 `convertView` 부족을 방지 하기 위해 새 뷰를 만들기 전에 항상 개체를 다시 사용 해야 합니다.

일부 어댑터 구현 ( `CursorAdapter`예:)에는 `GetView` 메서드가 없으므로의 `GetView` 역할을 두 가지로 구분 하 `NewView` 여 `BindView` 행을 다시 사용 해야 합니다. 메서드. 문서의 뒷부분에 예제가 있습니다. `CursorAdapter`


## <a name="enabling-fast-scrolling"></a>빠른 스크롤 사용

빠른 스크롤을 사용 하면 목록의 일부에 직접 액세스 하는 스크롤 막대 역할을 하는 추가 ' 핸들 '을 제공 하 여 긴 목록을 스크롤할 수 있습니다. 이 스크린샷은 빠른 스크롤 핸들을 보여 줍니다.

[![스크롤 핸들을 사용한 빠른 스크롤 스크린샷](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

빠른 스크롤 핸들을 표시 하는 것은 `FastScrollEnabled` 속성을로 설정 하 `true`는 것 처럼 간단 합니다.

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>섹션 인덱스 추가

섹션 인덱스는 긴 목록을 &ndash; 통해 빠르게 스크롤할 때 사용자에 대 한 추가 피드백을 제공 합니다. 섹션 인덱스를 표시 하려면 어댑터 하위 클래스가 표시 되는 행에 `ISectionIndexer` 따라 인덱스 텍스트를 제공 하기 위해 인터페이스를 구현 해야 합니다.

[![H로 시작 하는 앞의 섹션에 표시 되는 H 스크린샷](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

을 구현 `ISectionIndexer` 하려면 어댑터에 세 개의 메서드를 추가 해야 합니다.

-   **Getsections** &ndash; 표시할 수 있는 섹션 인덱스 타이틀의 전체 목록을 제공 합니다. 이 메서드는 Java 개체 배열을 필요로 하므로 코드가 .net 컬렉션에서를 `Java.Lang.Object[]` 만들어야 합니다. 이 예제에서는 목록의 첫 문자 목록을로 `Java.Lang.String` 반환 합니다.

-   **Getpositionforsection** &ndash; 지정 된 섹션 인덱스의 첫 번째 행 위치를 반환 합니다.

-   **Get섹션 Forposition** &ndash; 지정 된 행에 대해 표시할 섹션 인덱스를 반환 합니다.


예제 `SectionIndex/HomeScreenAdapter.cs` 파일은 이러한 메서드를 구현 하 고 생성자에 몇 가지 추가 코드를 구현 합니다. 생성자는 모든 행을 반복 하 고 제목의 첫 번째 문자를 추출 하 여 섹션 인덱스를 작성 합니다 .이 작업을 수행 하려면 항목이 이미 정렬 되어 있어야 합니다.

```csharp
alphaIndex = new Dictionary<string, int>();
for (int i = 0; i < items.Length; i++) { // loop through items
   var key = items[i][0].ToString();
   if (!alphaIndex.ContainsKey(key))
       alphaIndex.Add(key, i); // add each 'new' letter to the index
}
sections = new string[alphaIndex.Keys.Count];
alphaIndex.Keys.CopyTo(sections, 0); // convert letters list to string[]

// Interface requires a Java.Lang.Object[], so we create one here
sectionsObjects = new Java.Lang.Object[sections.Length];
for (int i = 0; i < sections.Length; i++) {
   sectionsObjects[i] = new Java.Lang.String(sections[i]);
}
```

데이터 구조가 생성 되 면 메서드는 `ISectionIndexer` 매우 간단 합니다.

```csharp
public Java.Lang.Object[] GetSections()
{
   return sectionsObjects;
}
public int GetPositionForSection(int section)
{
   return alphaIndexer[sections[section]];
}
public int GetSectionForPosition(int position)
{   // this method isn't called in this example, but code is provided for completeness
    int prevSection = 0;
    for (int i = 0; i < sections.Length; i++)
    {
        if (GetPositionForSection(i) > position)
        {
            break;
        }
        prevSection = i;
    }
    return prevSection;
}
```

섹션 인덱스 제목은 실제 섹션에 1:1을 매핑할 필요가 없습니다. 이는 메서드가 존재 `GetPositionForSection` 하는 이유입니다.
`GetPositionForSection`는 인덱스 목록에 있는 모든 인덱스를 목록 뷰에 있는 섹션에 매핑할 수 있는 기회를 제공 합니다. 예를 들어 인덱스에 "z"가 있지만 모든 문자에 대 한 테이블 섹션이 없을 수 있으므로 26에 매핑되는 "z" 대신 25 또는 24로 매핑하거나 "z"가 매핑될 모든 섹션 인덱스를 매핑할 수 있습니다.



## <a name="related-links"></a>관련 링크

- [BasicTableAndroid (샘플)](https://developer.xamarin.com/samples/monodroid/BasicTableAndroid/)
- [BasicTableAdapter (샘플)](https://developer.xamarin.com/samples/monodroid/BasicTableAdapter/)
- [FastScroll (샘플)](https://developer.xamarin.com/samples/monodroid/FastScroll/)
