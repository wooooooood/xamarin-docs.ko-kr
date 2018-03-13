---
title: "연습"
ms.topic: article
ms.prod: xamarin
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: e5c058f173f64efe4a5c777872e9ea67120115f0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough"></a>연습

다음 단계에서 기본 앱을 조각을 사용 하 여 만들어집니다. 첫 번째 단계는 Android 프로젝트에 대 한 새 Xamarin.Android를 만드는 것입니다.

## <a name="1-create-a-project"></a>1. 프로젝트 만들기

라는 새 Xamarin.Android 프로젝트 만들기 **FragmentSample**합니다. **최소 Android** 버전 설정할지를 Android 3.1 이상으로 아래 이미지에 나와 있는 것 처럼:

[![최소 Android 버전을 설정합니다.](walkthrough-images/00.png)](walkthrough-images/00.png#lightbox)


## <a name="2-create-the-mainactivity"></a>2. MainActivity 만들기

만들어야 한다는 다음으로 `MainActivity` 클래스입니다. 응용 프로그램에 대 한 시작 작업입니다. 이 활동의 화면 크기에 따라 하나 또는 두 부분을 호스트 합니다. `MainActivity` 에서는이 장치에 적합 한 레이아웃 파일을 로드 하 여 수행 합니다.

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```

> [!NOTE]
> `Note:` 조각 하위 클래스는 공용 기본 인수 없는 생성자를 있어야 합니다.

## <a name="3-create-the-layout-files"></a>3. 레이아웃 파일 만들기

두 개의 다른 화면 크기에는 두 개의 다른 레이아웃 파일이 필요 합니다. 이제 새 폴더를 만들 **리소스/레이아웃-대형**, 라는 새 레이아웃을 만들고 **activity_main.axml**합니다. 기본 레이아웃 파일으로 바꾼 또한 합니다 **Resources/Layout/activity_main.axml**합니다. 이러한 변경 된 후 레이아웃 폴더는 다음 스크린샷을 유사 해야 합니다.

[![IDE에서 레이아웃 폴더의 스크린샷](walkthrough-images/01.png)](walkthrough-images/01.png#lightbox)


모든 장치 로드 되 고 레이아웃 파일을 사용할 **리소스/레이아웃**합니다.
매우 간단한 레이아웃 표시 하는 한 `TitlesFragment`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_width="fill_parent"
           android:layout_height="fill_parent" />
</LinearLayout>
```

Android 장치를 큰 화면에 있는 경우에 대 한 레이아웃 파일에서 로드 됩니다 **리소스/레이아웃-대형**합니다. 태블릿에 대 한 레이아웃의 콘텐츠는 다음과 같습니다.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_weight="1"
           android:layout_width="0px"
           android:layout_height="match_parent" />
 <FrameLayout android:id="@+id/details"
              android:layout_weight="1"
              android:layout_width="0px"
              android:layout_height="match_parent" />
</LinearLayout>
```

큰 화면에 대 한 레이아웃 파일은 약간 다릅니다. 뿐만 아니라는 `TitlesFragment` 이 레이아웃 파일에 표시 되지만 `FrameLayout` 조각 바로 오른쪽에 추가 됩니다. 큰 화면에는 `DetailsFragment` 에 프로그래밍 방식으로 추가 됩니다 `MainActivity` 사용자가을 플레이 선택 합니다. 나중에 설명 자세히입니다.

Android 3.2 화면 레이아웃을 지정 하는 새로운 방법을 도입 되었습니다. 이러한 새 한정자의 화면 크기 보다 레이아웃에 필요한 공간의 크기를 지정 합니다. 이 응용 프로그램 이상 또는 Android 3.2 에서만 실행 하려는 되었습니다, 경우 만듭니다는 **리소스/레이아웃-sw600dp** 폴더 (폴더 대신 **리소스/레이아웃-대형**) 레이아웃 파일에 대 한  **activity_main.axml**합니다. 이 리소스 파일에서 600 밀도 독립적 픽셀의 최소 화면 너비에 설정한 모든 장치의 로드 됩니다. 그러나이 응용 프로그램으로 Android 3.1 대상으로 설정 되었거나 이전 리소스 한정자 사용 하 여 이상.

## <a name="4-create-the-titlesfragment"></a>4. TitlesFragment 만들기

`TitlesFragment` 다양 한 역할의 제목을 표시 되 면 프로젝트에 새 조각을 호출을 추가 하겠습니다. 따라서 `TitlesFragment`:

[![새로운 조각이 TitlesFragment 프로젝트에 추가](walkthrough-images/02.png)](walkthrough-images/02.png#lightbox)

후 `TitlesFragment` 추가 클래스에서 상속 되도록 변경 해야 `Android.App.ListFragment`합니다. `ListFragment` 목록 기능을 포함 하는 특수 한 조각 형식이입니다.
`TitlesFragment` 재정의 합니다 `OnActivityCreated` (다른 조각 수명 주기 메서드)를 제공 하 고는 `Adapter` 하 `ListFragment` 목록을 채우는 데 사용 합니다:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
   base.OnActivityCreated(savedInstanceState);
   var adapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemChecked, Shakespeare.Titles);
   ListAdapter = adapter;
   if (savedInstanceState != null)
   {
       _currentPlayId = savedInstanceState.GetInt("current_play_id", 0);
   }
   var detailsFrame = Activity.FindViewById<View>(Resource.Id.details);
   _isDualPane = detailsFrame != null && detailsFrame.Visibility == ViewStates.Visible;
   if (_isDualPane)
   {
       ListView.ChoiceMode = (int) ChoiceMode.Single;
       ShowDetails(_currentPlayId);
   }
}
```

위에서 언급 한 대로 응용 프로그램에 두 가지 레이아웃을 `MainActivity`합니다. 코드 `OnActivityCreated` 감지 하는 `FrameLayout` 레이아웃 파일 로드 되었는지 확인 합니다. 경우는 `FrameLayout` 레이아웃에 존재 하면 `_isDualPane` 플래그가로 설정 되어 `true`합니다. `_isDualPane` 플래그 사용 하는 다른 위치에서 활동에서 구체적으로 `ShowDetails` 메서드. `ShowDetails` 메서드 아래에 자세히 설명 합니다.

`TitlesFragment` 목록 및 목록에 사용자 선택에 응답 해야 합니다. 이렇게 하려면 `TitlesFragment` 메서드를 재정의 `OnListItemClick`합니다. 내부 `OnListItemClick`, 새 `DetailsFragment` 만들어지고에 표시 됩니다는 `FrameLayout`, 프로그래밍 방식입니다. 내에 있는 관련 코드 `TitlesFragment` 됩니다.

```csharp
public override void OnListItemClick(ListView l, View v, int position, long id)
{
   ShowDetails(position);
}
private void ShowDetails(int playId)
{
   _currentPlayId = playId;
   if (_isDualPane)
   {
       // We can display everything in place with fragments.
       // Have the list highlight this item and show the data.
       ListView.SetItemChecked(playId, true);
       // Check what fragment is shown, replace if needed.
       var details = FragmentManager.FindFragmentById(Resource.Id.details) as DetailsFragment;
       if (details == null || details.ShownPlayId != playId)
       {
           // Make new fragment to show this selection.
           details = DetailsFragment.NewInstance(playId);
           // Execute a transaction, replacing any existing
           // fragment with this one inside the frame.
           var ft = FragmentManager.BeginTransaction();
           ft.Replace(Resource.Id.details, details);
           ft.SetTransition(FragmentTransaction.TransitFragmentFade);
           ft.Commit();
       }
   }
   else
   {
       // Otherwise we need to launch a new Activity to display
       // the dialog fragment with selected text.
       var intent = new Intent();
       intent.SetClass(Activity, typeof (DetailsActivity));
       intent.PutExtra("current_play_id", playId);
       StartActivity(intent);
   }
}
```

장치에서 선택한 play에서 따옴표를 표시 하는 방법을 결정 하는 코드 합니다. 태블릿의 경우는 `_isDualPane` 플래그로 설정 됩니다 `true`, 및 견적에 옆에 표시 됩니다는 `TitlesFragment`합니다. 경우 선택한 재생 `id` 아직 표시 되지 않은 다음 새 `DetailsFragment` 생성 한 다음에 로드 되는 `FrameLayout` 활동에 있습니다. 대형 디스플레이 갖지 않는 다른 장치에 대 한 &ndash; 폰, 예를 들어 &ndash; `isDualPane` 로 설정 됩니다 `false` 하므로 새 `DetailsActivity` 시작 됩니다.


## <a name="5-create-the-detailsactivity"></a>5. DetailsActivity 만들기

`DetailsActivity` 표시는 `DetailsFragment` 되어 소형 장치에 대 한 합니다. 이 확인 하려면 먼저 합니다 새 활동에 추가 라는 프로젝트 `DetailsActivity`합니다. `DetailsActivity` 매우 간단한 작업입니다. 만들고 새을 호스트할 `DetailsFragment` 재생에 대 한 `id` 를 보냈습니다.

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("current_play_id", 0);

       var details = DetailsFragment.NewInstance(index); // DetailsFragment.NewInstance is a factory method to create a Details Fragment
       var fragmentTransaction = FragmentManager.BeginTransaction();
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

레이아웃 파일이 없는 하기 위해 로드 된 알림 `DetailsActivity`합니다. 대신, `DetailsFragment` 작업의 루트 보기에 로드 됩니다. 이 루트 뷰에 특수 ID `Android.Resource.Id.Content`합니다. 새 `DetailFragment` 생성 되어 다음 내부에이 루트 보기에 추가 `FragmentTransaction` 활동의에서 만들어진 `FragmentManager`합니다.


## <a name="6-create-the-detailsfragment"></a>6. DetailsFragment 만들기

이제 다른 조각을 라는 응용 프로그램에 추가 해 보겠습니다 `DetailsFragment`합니다. 이 조각에는 선택한 play에서 견적을 표시 됩니다. 다음 코드에서는 전체 `DetailsFragment`:

```csharp
internal class DetailsFragment : Fragment
{
   public static DetailsFragment NewInstance(int playId)
   {
       var detailsFrag = new DetailsFragment {Arguments = new Bundle()};
       detailsFrag.Arguments.PutInt("current_play_id", playId);
       return detailsFrag;
   }
   public int ShownPlayId
   {
       get { return Arguments.GetInt("current_play_id", 0); }
   }
   public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
   {
       if (container == null)
       {
           // Currently in a layout without a container, so no reason to create our view.
           return null;
       }
       var scroller = new ScrollView(Activity);
       var text = new TextView(Activity);
       var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
       text.SetPadding(padding, padding, padding, padding);
       text.TextSize = 24;
       text.Text = Shakespeare.Dialogue[ShownPlayId];
       scroller.AddView(text);
       return scroller;
   }
}
```

에 대 한 순서 대로 `DetailsFragment` 제대로 기능 하려면에서 선택 되어 있는 재생 인덱스 있어야는 `TitlesFragment`합니다. 이 값을 제공 하는 방법은 여러 가지가 `DetailsFragment`재생이 예에서 `Id` 번들에 번들의 인스턴스로의 인수 속성에 저장 되 고는 `DetailsFragment`합니다. 속성 `ShownPlayId` 편의상 제공 됩니다 &ndash; 의 인스턴스에서 `DetailsFragment` 번들에서 해당 값을 검색 합니다.

`OnCreateView` 때 호출 되는 조각 자체의 사용자 인터페이스를 그려야 하 고 반환 해야는 `Android.Views.View` 개체입니다. 이 대부분의 경우에는 `View` 기존 레이아웃 파일에서 확장 합니다. 위 예제의 경우 조각 표시에 사용 하는 뷰를 프로그래밍 방식으로 작성 합니다.

지금까지 이제 폼 팩터 전체 개발을 간소화 하기 위해 조각을 사용 하는 응용 프로그램을 만들었습니다.

에 [다음 섹션](supporting-pre-honeycomb.md), 3.0 사전 Android 장치에서 작동할 수 있도록이 응용 프로그램을 확장 하는 것입니다.

