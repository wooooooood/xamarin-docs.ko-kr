---
title: "ListView 데이터로 채우기"
ms.topic: article
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: 12197d238ddc6ddc2bd8f48f77aa15f5eff22a0a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="populating-a-listview-with-data"></a>ListView 데이터로 채우기


## <a name="overview"></a>개요

행을 추가 하는 `ListView` 레이아웃 및 구현에 추가 해야는 `IListAdapter` 메서드와 함께 하는 `ListView` 채울 자체에 대 한 호출 합니다. 기본 제공을 포함 하는 android `ListActivity` 및 `ArrayAdapter` 클래스를 모든 사용자 지정 레이아웃 XML 또는 코드를 정의 하지 않고 사용할 수 있습니다. `ListActivity` 클래스를 자동으로 만듭니다는 `ListView` 노출 한 `ListAdapter` 는 어댑터를 통해 표시할 행 보기를 제공 하는 속성입니다.

기본 제공 어댑터 보기 리소스 ID를 각 행에 사용 되는 매개 변수로 사용 합니다. 에 있는 기본 제공 리소스를 사용할 수 있습니다 `Android.Resource.Layout` 필요 하지 쓰려고 직접 합니다.


## <a name="using-listactivity-and-arrayadapterltstringgt"></a>ListActivity 및 ArrayAdapter 사용 하 여&lt;문자열&gt;

이 예제에서는 **BasicTable/HomeScreen.cs** 표시 하려면 이러한 클래스를 사용 하는 방법을 보여 줍니다.는 `ListView` 단 몇 줄의 코드에서:

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


### <a name="handling-row-clicks"></a>행 처리를 클릭합니다

일반적으로 `ListView` 은 일부 동작 (예:는 곡을 재생 또는 호출 하는 연락처, 또는 다른 화면을 표시)을 수행 하려면 행을 터치 사용자 수 있게 해줍니다. 에 응답 하려면 사용자 작업 하나 있어야 자세한 메서드 구현에 `ListActivity` &ndash; `OnListItemClick` &ndash; 다음과 같이 합니다.

[![SimpleListItem의 스크린 샷](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

이제 사용자는 행을 인접할 수 및 `Toast` 경고가 표시 됩니다.

[![스크린샷의 알림 메시지는 행에 접촉 될 때 표시 되는](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>ListAdapter 구현

`ArrayAdapter<string>` 매우 때문이 클래스는 단순 하지만 매우 제한적입니다. 그러나 종종 바인딩할 정당한 문자열 아니라 비즈니스 엔터티 집합이 있는 합니다.
예를 들어 데이터 직원 클래스의 컬렉션으로 구성 된 경우 각 직원의 이름을 표시 하려면 목록을 사용할 수 있습니다. 동작을 사용자 지정 하는 `ListView` 표시 되는 데이터를 제어할 수의 서브 클래스를 구현 해야 `BaseAdapter` 재정의 다음 네 개 항목:

-   **Count** &ndash; 얼마나 많은 행이 데이터에 컨트롤을 알 수 있습니다.

-   **영역의 GetView** &ndash; 각 행에 대 한 보기를 반환할 데이터로 채워집니다.
    이 메서드에 대 한 매개 변수는는 `ListView` 다시 사용에 대 한 기존, 사용 하지 않는 행에 전달할 수 있습니다.

-   **GetItemId** &ndash; 행 식별자를 반환 합니다. (일반적으로 행 번호를 사용 하고자 하는 long 값을 사용할 수 있지만).

-   **이 [int]** 인덱서 &ndash; 특정 행 번호와 관련 된 데이터를 반환 합니다.

예제 코드에서는 **BasicTableAdapter/HomeScreenAdapter.cs** 보여 줍니다 서브 클래스 하는 방법을 `BaseAdapter`:

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


### <a name="using-a-custom-adapter"></a>사용자 지정 어댑터를 사용 하 여

사용자 지정 어댑터를 사용 하는 기본 제공 비슷합니다 `ArrayAdapter`에 전달 하는 `context` 및 `string[]` 표시할 값:

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

이 예에서는 동일한 행 레이아웃을 사용 하므로 (`SimpleListItem1`) 결과 응용 프로그램은 앞의 예와 동일 하 게 보일 합니다.


### <a name="row-view-re-use"></a>행 보기 다시 사용

이 예제는 6 개의 항목이 있습니다. 화면 충족할 수는 8 자, 이후 필요 없는 행 다시 사용 합니다. 그러나 수백 또는 수천 개의 행을 표시할 때 것 수백 또는 수천 개의 만들기 위해 메모리를 낭비 `View` 때만 8 개의 개체가 한 번에 화면에 맞게 합니다. 행이 해당 뷰를 다시 사용 하도록 큐에 배치 됩니다 화면에서 사라지면 이러한 상황을 피해야 합니다. 사용자 스크롤하면는 `ListView` 호출 `GetView` 표시할 새 보기를 요청 하려면 &ndash; 사용할 수 있는 경우에 사용 되지 않은 뷰를 전달는 `convertView` 매개 변수입니다. 코드에 새 보기 인스턴스를 만들어야 합니다. 그런 다음이 값이 null 이면 그렇지 않으면 다시 해당 개체의 속성을 다시 사용 하 여 합니다.

`GetView` 메서드 행 뷰 다시 사용 하려면이 패턴을 따라야 합니다.

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

사용자 지정 어댑터 구현 해야 *항상* 다시 사용 된 `convertView` 긴 목록을 표시할 때 메모리가 부족 실행 하지 않도록 하려면 새 뷰를 만들기 전에 개체입니다.

일부 어댑터 구현 (같은 `CursorAdapter`) 없는 `GetView` 메서드를 두 가지 방법이 필요한 대신 `NewView` 및 `BindView` 의 업무를 분리 하 여 행이 다시 사용을 적용 하 `GetView` 두 개로 메서드를 합니다. 한 `CursorAdapter` 문서 뒷부분에 나오는 예제입니다.


## <a name="enabling-fast-scrolling"></a>빠른 스크롤을 사용 하도록 설정

빠른 스크롤을 통해 사용자는 추가 '하는 핸들' 역할을 목록의 일부에 직접 액세스할 스크롤 막대를 제공 하 여 긴 목록을 통해 스크롤할 수 있습니다. 이 스크린샷에서 빠른 스크롤 핸들을 보여 줍니다.

[![스크롤 핸들 사용 하 여 빠른 스크롤의 스크린샷](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

로 설정 하기만 하면은 표시에 대 한 빠른 스크롤 핸들을 일으키는 `FastScrollEnabled` 속성을 `true`:

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>섹션 인덱스 추가

섹션 인덱스 긴 목록을 통해 빠른 스크롤 있을 때 사용자에 대 한 추가 피드백을 제공 &ndash; 에 스크롤 'section' 보여 줍니다. 섹션 색인을 어댑터 하위 클래스를 구현 해야 하는 `ISectionIndexer` 인터페이스를 표시 되는 행에 따라 인덱스 텍스트를 입력 합니다.

[![H로 시작 하는 스크린 샷의 H 섹션 위에 나타납니다](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

구현 하려면 `ISectionIndexer` 를 어댑터에 세 가지 메서드를 추가 해야 합니다.

-   **GetSections** &ndash; 인덱스 제목은 표시할 수 있는 섹션의 전체 목록을 제공 합니다. 이 방법을 사용 하려면 Java 개체의 배열을 만드는 데 필요한 코드 하므로 한 `Java.Lang.Object[]` .NET 컬렉션에서 합니다. 예제에서에 따라 목록에서 초기 문자의 목록을 반환 `Java.Lang.String` 합니다.

-   **GetPositionForSection** &ndash; 지정한 구간 인덱스에 대 한 첫 번째 행 위치를 반환 합니다.

-   **GetSectionForPosition** &ndash; 지정된 된 행에 대해 표시 되는 섹션 인덱스를 반환 합니다.


이 예제에서는 `SectionIndex/HomeScreenAdapter.cs` 생성자에서 해당 메서드 및 몇 가지 추가 코드를 구현 하는 파일입니다. 생성자는 모든 행을 반복 하 고 제목 (항목 해야 하도록 정렬 되어이 수행)의 첫 번째 문자를 추출 하 여 섹션 인덱스를 작성 합니다.

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

을 만든 데이터 구조는 `ISectionIndexer` 메서드는 매우 간단 합니다.

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

사용자 섹션 인덱스 타이틀 1:1 실제 섹션에 매핑할 필요 하지 않습니다. 이 인해는 `GetPositionForSection` 메서드가 있습니다.
`GetPositionForSection` 모든 인덱스는 목록 보기에서 모든 섹션은 인덱스 목록에 매핑할 수가 있습니다. 예를 들어 인덱스에서 "z"를 할 수 있습니다 하지만 모든 문자에 대 한 테이블 섹션 없을 수 있습니다, 25 또는 24, 또는 어떤 섹션 "z" 인덱스에 매핑되는지를 매핑할 수 있습니다 "z" 매핑 26 사이 대신 하도록 합니다.



## <a name="related-links"></a>관련 링크

- [BasicTableAndroid (샘플)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (샘플)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (샘플)](https://developer.xamarin.com/samples/FastScroll/)
