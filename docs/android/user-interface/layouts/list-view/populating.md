---
title: 데이터로 ListView 채우기
ms.prod: xamarin
ms.assetid: AC4F95C8-EC3F-D960-7D44-8D55D0E4F1B6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2017
ms.openlocfilehash: 57c69223a01074ed15714026b7e9ec4e995808e0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61171314"
---
# <a name="populating-a-listview-with-data"></a>데이터로 ListView 채우기


## <a name="overview"></a>개요

행을 추가할를 `ListView` 레이아웃 및 구현에 추가 해야는 `IListAdapter` 메서드를 사용 하 여는 `ListView` 채우는 자체에 대 한 호출 합니다. Android 포함 기본 제공 `ListActivity` 고 `ArrayAdapter` 모든 사용자 지정 레이아웃 XML 또는 코드를 정의 하지 않고 사용할 수 있는 클래스입니다. 합니다 `ListActivity` 클래스를 자동으로 만듭니다는 `ListView` 노출를 `ListAdapter` 는 어댑터를 통해 표시할 행 보기를 제공 하는 속성입니다.

기본 제공 어댑터 보기 리소스 ID를 각 행에 대해 사용 되는 매개 변수로 사용 합니다. 같은 기본 제공 리소스를 사용할 수 있습니다 `Android.Resource.Layout` 있으므로 않아도 쓸 고유한 합니다.


## <a name="using-listactivity-and-arrayadapterltstringgt"></a>ListActivity 및 ArrayAdapter를 사용 하 여&lt;문자열&gt;

예제 **BasicTable/HomeScreen.cs** 표시할 이러한 클래스를 사용 하는 방법에 설명 된 `ListView` 코드 몇 줄만:

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


### <a name="handling-row-clicks"></a>가 행을 처리 합니다.

일반적으로 `ListView` (예: song을 재생 하는 연락처, 호출 또는 다른 화면을 보여 주는) 동작을 수행 하는 행을 터치 하 고 사용자를 허용할 수 됩니다. 응답할 하나 있어야 사용자 터치 메서드 구현에 `ListActivity` &ndash; `OnListItemClick` &ndash; 같이:

[![SimpleListItem 스크린샷](populating-images/simplelistitem1.png)](populating-images/simplelistitem1.png#lightbox)

```csharp
protected override void OnListItemClick(ListView l, View v, int position, long id)
{
   var t = items[position];
   Android.Widget.Toast.MakeText(this, t, Android.Widget.ToastLength.Short).Show();
}
```

이제 사용자는 행을 터치 수 및 `Toast` 알림이 표시 됩니다.

[![스크린 샷의 알림 행을 연결 하는 경우 표시 되는](populating-images/basictable2.png)](populating-images/basictable2.png#lightbox)


## <a name="implementing-a-listadapter"></a>ListAdapter 구현

`ArrayAdapter<string>` 훌륭한 때문이 클래스는 단순 하지만 매우 제한적입니다. 그러나 종종 경우 바인딩하려는 문자열일 뿐 아니라 비즈니스 엔터티, 컬렉션
예를 들어 데이터 Employee 클래스의 컬렉션으로 구성 된 경우 각 직원의 이름을 표시 하도록 목록을 사용할 수 있습니다. 동작을 사용자 지정 하는 `ListView` 표시 되는 데이터를 제어 하의 서브 클래스를 구현 해야 `BaseAdapter` 재정의 다음 네 개 항목:

-   **개수** &ndash; 얼마나 많은 행이 데이터에 컨트롤을 알려야 합니다.

-   **GetView** &ndash; 데이터로 채워진 각 행에 대 한 뷰를 반환 합니다.
    이 메서드는 매개 변수는 `ListView` 다시 사용 하기 위해 기존에 사용 되지 않는 행을 전달 합니다.

-   **GetItemId** &ndash; 행 식별자를 반환 합니다. (일반적으로 행 번호를 선택 하는 long 값을 사용할 수 있지만).

-   **이 [int]** 인덱서 &ndash; 특정 행 번호와 관련 된 데이터를 반환 합니다.

예제 코드 **BasicTableAdapter/HomeScreenAdapter.cs** 방법을 보여 줍니다 서브 클래스 하는 방법을 `BaseAdapter`:

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

사용자 지정 어댑터를 사용 하는 것은 기본 제공 비슷합니다 `ArrayAdapter`전달를 `context` 및 `string[]` 표시 하는 값:

```csharp
ListAdapter = new HomeScreenAdapter(this, items);
```

이 예에서는 동일한 행 레이아웃을 사용 하므로 (`SimpleListItem1`) 응용 프로그램 앞의 예와 동일 합니다.


### <a name="row-view-re-use"></a>행 보기 재사용

이 예제에서 6 개의 항목이 있습니다. 화면 8를 담을 수, 있으므로 필요 없는 행 다시 사용 합니다. 그러나 수백 또는 수천 개의 행을 표시할 때는 것이 수백 또는 수천 개의 만들기 위해 메모리를 낭비 `View` 8 개의 때 개체를 한 번에 화면에 맞게 합니다. 이 경우 행 뷰 다시 사용 하기 위해 큐에 배치 됩니다 화면에서 사라지면 방지 하려면. 사용자가 스크롤하면 합니다 `ListView` 호출 `GetView` 표시할 새 보기를 요청 하 &ndash; 사용 가능한 경우에 사용 되지 않은 보기를 전달 합니다 `convertView` 매개 변수입니다. 코드에 새 보기 인스턴스를 만들어야 합니다. 그런 다음이 값이 null 이면이 고, 그렇지 다시 해당 개체의 속성을 설정할를 다시 사용 합니다.

`GetView` 메서드 행 보기 재사용에이 패턴을 따라야 합니다.

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

사용자 지정 어댑터 구현 해야 *항상* 다시 사용 된 `convertView` 긴 목록을 표시 하는 경우 메모리에서 실행 되지 않을 수 있도록 새 뷰를 만들기 전에 개체입니다.

일부 어댑터 구현 (같은 합니다 `CursorAdapter`) 없는 `GetView` 메서드를 대신 두 가지 방법이 필요 `NewView` 및 `BindView` 의 책임을 구분 하 여 행을 다시 사용을 적용 하는 `GetView` 두 개로 메서드입니다. 한 `CursorAdapter` 문서 뒷부분에 나오는 예제입니다.


## <a name="enabling-fast-scrolling"></a>빠른 스크롤을 사용 하도록 설정

빠른 스크롤 추가 '하는 핸들' 역할을 목록의 일부에 직접 액세스 하는 스크롤 막대를 제공 하 여 긴 목록을 통해 스크롤할 수 있습니다. 이 스크린샷은 빠른 스크롤 핸들을 보여 줍니다.

[![스크롤 핸들을 사용 하 여 빠른 스크롤 하는 스크린샷](populating-images/fastscroll.png)](populating-images/fastscroll.png#lightbox)

발생 표시에 대 한 빠른 스크롤 핸들을 간단 하 게 설정 합니다 `FastScrollEnabled` 속성을 `true`:

```csharp
ListView.FastScrollEnabled = true;
```


### <a name="adding-a-section-index"></a>섹션 인덱스 추가

구간 인덱스 추가 피드백을 제공 하므로 사용자에 대 한 긴 목록을 통해 빠르게 스크롤할 때 &ndash; 스크롤 하는 'section' 보여 줍니다. 섹션 색인을 어댑터 서브 클래스를 구현 해야 하는 `ISectionIndexer` 표시 되는 행에 따라 인덱스 텍스트를 제공 하는 인터페이스:

[![스크린 샷의 H H로 시작 하는 구역 위에 표시](populating-images/sectionindex.png)](populating-images/sectionindex.png#lightbox)

구현 `ISectionIndexer` 어댑터에 세 가지 메서드를 추가 해야 합니다.

-   **GetSections** &ndash; 인덱스 제목 표시 될 수 있는 섹션의 전체 목록을 제공 합니다. 이 메서드에 코드를 만들 필요가 있으므로 Java 개체의 배열이 필요는 `Java.Lang.Object[]` .NET 컬렉션에서 합니다. 목록에서 초기 문자의 목록을 반환 합니다 예에서 `Java.Lang.String` 합니다.

-   **GetPositionForSection** &ndash; 지정한 구간 인덱스에 대 한 첫 번째 행 위치를 반환 합니다.

-   **GetSectionForPosition** &ndash; 지정된 된 행에 대해 표시할 섹션 인덱스를 반환 합니다.


이 예제에서는 `SectionIndex/HomeScreenAdapter.cs` 생성자에서 해당 메서드 및 일부 추가 코드를 구현 하는 파일입니다. 모든 행을 반복 하 고 (항목은 이미 정렬 해야이 작동 하려면) 제목의 첫 번째 문자를 추출 하 여 섹션 인덱스를 작성 하는 생성자입니다.

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

만들어지면 데이터 구조를 사용 하 여는 `ISectionIndexer` 방법이 매우 간단 합니다.

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

섹션 인덱스 제목을 실제 섹션 1:1 매핑할 필요가 없습니다. 이 인해는 `GetPositionForSection` 메서드가 있습니다.
`GetPositionForSection` 어떤 인덱스가 어떤 섹션 목록 보기에는 인덱스 목록에 매핑할 수가 있습니다. 예를 들어 인덱스에는 "z"를 사용 해야 하지만 모든 문자에 대 한 테이블 섹션 없을 수 있습니다, 25 또는 24, 또는 섹션 인덱스 "z" 매핑되어야 하므로에 매핑할 수 있습니다 26 "z" 매핑을, 하는 대신 합니다.



## <a name="related-links"></a>관련 링크

- [BasicTableAndroid (샘플)](https://developer.xamarin.com/samples/BasicTableAndroid/)
- [BasicTableAdapter (샘플)](https://developer.xamarin.com/samples/BasicTableAdapter/)
- [FastScroll (샘플)](https://developer.xamarin.com/samples/FastScroll/)
