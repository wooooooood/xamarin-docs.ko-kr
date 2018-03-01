---
title: "특수화 된 조각 클래스"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: e7b4349ee2664a94ef6dff3c6a58d5f8f97682a1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="specialized-fragment-classes"></a>특수화 된 조각 클래스

조각 API의 일부 응용 프로그램에 일반적인 기능을 캡슐화 하는 다른 하위 클래스를 제공 합니다. 이 하위 클래스는.

-   **ListFragment** &ndash; 이 조각은 배열 또는 커서를 같은 데이터 소스에 바인딩된 항목의 목록을 표시 하는 데 사용 됩니다.

-   **DialogFragment** &ndash; 이 조각 대화 상자 주위에서 래퍼로 사용 됩니다. 조각에는 해당 활동 위에 대화 상자가 표시 됩니다.

-   **PreferenceFragment** &ndash; 이 조각은 기본 설정 개체 목록으로 표시 하는 데 사용 됩니다.


<a name="The_ListFragment" />

## <a name="the-listfragment"></a>ListFragment

`ListFragment` 는 개념 및 기능을 매우 비슷하지만 `ListActivity`;를 호스트 하는 래퍼는는 `ListView` 조각에서입니다. 다음 이미지는 `ListFragment` 태블릿 및 휴대폰에서 실행:

[![스크린 샷을의 ListFragment 태블릿 및 휴대폰](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png)

<a name="Binding_Data_With_The_ListAdapter" />

### <a name="binding-data-with-the-listadapter"></a>ListAdapter 사용 하 여 데이터 바인딩

`ListFragment` 재정의 되지 않도록 클래스는 기본 레이아웃을 이미 제공 `OnCreateView` 의 내용을 표시 하는 `ListFragment`합니다. `ListView` 를 사용 하 여 데이터에 바인딩되는 `ListAdapter` 구현 합니다. 다음 예제에서는 문자열의 단순 배열을 사용 하 여 수행할 수 있습니다 어떻게 보여 줍니다.

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    string[] values = new[] { "Android", "iPhone", "WindowsMobile",
                "Blackberry", "WebOS", "Ubuntu", "Windows7", "Max OS X",
                "Linux", "OS/2" };
    this.ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleExpandableListItem1, values);
}
```

설정할 때는 `ListAdapter`, 사용 해야는 `ListFragment.ListAdapter` 속성 및 not는 `ListView.ListAdapter` 속성입니다. 사용 하 여 `ListView.ListAdapter` 중요 한 초기화 코드가을 건너뛸 수 있습니다.

<a name="Responding_to_User_Selection" />


### <a name="responding-to-user-selection"></a>사용자 선택에 응답

사용자 선택에 응답 하려면 응용 프로그램 재정의 해야는 `OnListItemClick` 메서드. 다음 예제에서는 이러한 한 가지를 보여 줍니다.

```csharp
public override void OnListItemClick(ListView l, View v, int index, long id)
{
    // We can display everything in place with fragments.
    // Have the list highlight this item and show the data.
    ListView.SetItemChecked(index, true);

    // Check what fragment is shown, replace if needed.
    var details = FragmentManager.FindFragmentById<DetailsFragment>(Resource.Id.details);
    if (details == null || details.ShownIndex != index)
    {
        // Make new fragment to show this selection.
        details = DetailsFragment.NewInstance(index);

        // Execute a transaction, replacing any existing
        // fragment with this one inside the frame.
        var ft = FragmentManager.BeginTransaction();
        ft.Replace(Resource.Id.details, details);
        ft.SetTransition(FragmentTransit.FragmentFade);
        ft.Commit();
    }
}
```

사용자가 항목을 선택할 때 위의 코드에는 `ListFragment`, 새 조각을 선택 된 항목에 대 한 자세한 정보를 보여 주는 호스팅 활동에 표시 됩니다.

<a name="DialogFragment" />


## <a name="dialogfragment"></a>DialogFragment

*DialogFragment* 활동의 창 맨 위에 배치할 됩니다 하는 조각 내에서 대화 상자 개체를 표시 하는 데 사용 되는 조각입니다. 이를 관리 되는 대화 상자 (Android 3.0부터) Api를 교체 합니다. 다음 스크린 샷에서 모양의 예제가 나와 `DialogFragment`:

[![스크린샷의 DialogFragment 추가 새 차량 편집 상자를 표시 합니다.](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png)

A `DialogFragment` 확인 대화 상자와 부분 간의 상태 일관 되 게 유지 합니다. 모든 상호 작용 및 제어 대화 상자 개체를 통해 수행 해야는 `DialogFragment` API를 만들지 대화 상자 개체에서 직접 호출 하 고 있습니다. `DialogFragment` API 제공 된 각 인스턴스는 `Show()` 조각을 표시 하는 데 사용 되는 메서드. 두 가지 방법으로 조각를 제거 하려면:

-  호출 `DialogFragment.Dismiss()` 에 `DialogFragment` 인스턴스. 

-  다른 표시 `DialogFragment`합니다.

만들려면는 `DialogFragment`, 클래스에서 상속 `Android.App.DialogFragment,` 다음 두 가지 방법 중 하나를 재정의 합니다.

- **OnCreateView** &ndash; 이 만들고 뷰를 반환 합니다.

- **OnCreateDialog** &ndash; 이 사용자 지정 대화 상자를 만듭니다. 표시는 일반적으로 *AlertDialog*합니다. 재정의할 필요 하지 않습니다는이 메서드를 재정의할 때 `OnCreateView` 합니다.


<a name="A_Simple_DialogFragment" />

### <a name="a-simple-dialogfragment"></a>간단한 DialogFragment

다음 스크린샷은 간단한 `DialogFragment` 올려진는 `TextView` 와 두 개의 `Button`s:

[![예제 DialogFragment TextView와 두 개의 단추](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png)

`TextView` 사용자에의 한 개의 단추를 클릭 한 시간 수가 표시 됩니다는 `DialogFragment`반면 조각에서 다른 단추를 클릭 하면 닫힙니다. 에 대 한 코드 `DialogFragment` 됩니다.

```csharp
public class MyDialogFragment : DialogFragment
{
    private int _clickCount;
    public override void OnCreate(Bundle savedInstanceState)
    {
        _clickCount = 0;
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState)
        
        var view = inflater.Inflate(Resource.Layout.dialog_fragment_layout, container, false);
        var textView = view.FindViewById<TextView>(Resource.Id.dialog_text_view);
            
        view.FindViewById<Button>(Resource.Id.dialog_button).Click += delegate
            {
                textView.Text = "You clicked the button " + _clickCount++ + " times.";
            };

        // Set up a handler to dismiss this DialogFragment when this button is clicked.
        view.FindViewById<Button>(Resource.Id.dismiss_dialog_button).Click += (sender, args) => Dismiss();
        return view;
        }
    }
}
```

<a name="Displaying_a_Fragment" />

### <a name="displaying-a-fragment"></a>조각 표시

마찬가지로 모든 조각을 한 `DialogFragment` 의 컨텍스트에서 표시 되는 `FragmentTransaction`합니다.

`Show()` 에서 메서드는 `DialogFragment` 사용는 `FragmentTransaction` 및 `string` 입력으로 합니다. 대화 상자는 활동에 추가할 및 `FragmentTransaction` 커밋된 합니다.

다음 코드에서는 활동을 사용할 수는 한 가지 방법을 `Show()` 를 표시 하는 `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```

<a name="Dismissing_a_Fragment" />

### <a name="dismissing-a-fragment"></a>조각을 해제합니다.

호출 `Dismiss()` 인스턴스에 `DialogFragment` 하면 활동에서 제거할 조각 및 해당 트랜잭션을 커밋합니다.
관련 조각 소멸 된 표준 조각 수명 주기 메서드 호출 됩니다.

<a name="Alert_Dialog" />

### <a name="alert-dialog"></a>경고 대화 상자

재정의 하는 대신 `OnCreateView`, `DialogFragment` 대신 재정의할 수 있습니다 `OnCreateDialog`합니다. 이렇게 하면 응용 프로그램을 만들 수 있습니다.는 [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/) 관리 하는 조각입니다. 다음 코드는 사용 하는 예제는 `AlertDialog.Builder` 만들려는 `Dialog`:

```csharp
public class AlertDialogFragment : DialogFragment
{
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        EventHandler<DialogClickEventArgs> okhandler;
        var builder = new AlertDialog.Builder(Activity)
            .SetMessage("This is my dialog.")
            .SetPositiveButton("Ok", (sender, args) =>
                                         {
                                             // Do something when this button is clicked.
                                         })
            .SetTitle("Custom Dialog");
        return builder.Create();
    }
}
```

 <a name="PreferenceFragment" />


## <a name="preferencefragment"></a>PreferenceFragment

조각 API는 기본 설정 관리 하려면 다음을 제공 합니다.는 `PreferenceFragment` 하위 클래스입니다. `PreferenceFragment` 는 비슷합니다는 [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/
) &ndash; 사용자에 게 기본 설정의 계층 구조는 조각에 표시 됩니다. 사용자 기본 설정와 상호 작용 하은 자동으로 저장 될 [SharedPreferences](http://developer.android.com/reference/android/content/SharedPreferences.html)합니다.
Android 3.0 또는 더 높은 응용 프로그램에서 사용 하 여는 `PreferenceFragment` 기본 설정과 응용 프로그램에서 처리 하도록 합니다. 다음 그림은 예를 보여 줍니다.는 `PreferenceFragment`:

[![예제 PreferencesFragment 인라인, 대화 상자에서 및 실행할 때 환경 설정](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png)

<a name="Create_A_Preference_Fragment_from_a_Resource" />

### <a name="create-a-preference-fragment-from-a-resource"></a>리소스에서 기본 설정 조각을 만들려면

기본 설정을 사용 하 여 조각 XML 리소스 파일에서 확장 될 수 있습니다는 [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/) 메서드. 논리부터 조각의 수명 주기에서이 메서드를 호출 하는 것에 `OnCreate` 메서드.

`PreferenceFragment` 과 XML에서 리소스를 로드 하 여 위에서 만든 합니다. 리소스 파일은:

```xml
<?xml version="1.0" encoding="utf-8"?>

<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">

  <PreferenceCategory android:title="Inline Preferences">
    <CheckBoxPreference android:key="checkbox_preference"
                        android:title="Checkbox Preference Title"
                        android:summary="Checkbox Preference Summary" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Dialog Based Preferences">

    <EditTextPreference android:key="edittext_preference"
                        android:title="EditText Preference Title"
                        android:summary="EditText Preference Summary"
                        android:dialogTitle="Edit Text Preferrence Dialog Title" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Launch Preferences">

    <!-- This PreferenceScreen tag serves as a screen break (similar to page break
             in word processing). Like for other preference types, we assign a key
             here so it is able to save and restore its instance state. -->
    <PreferenceScreen android:key="screen_preference"
                      android:title="Title Screen Preferences"
                      android:summary="Summary Screen Preferences">

      <!-- You can place more preferences here that will be shown on the next screen. -->

      <CheckBoxPreference android:key="next_screen_checkbox_preference"
                          android:title="Next Screen Toggle Preference Title"
                          android:summary="Next Screen Toggle Preference Summary" />

    </PreferenceScreen>

    <PreferenceScreen android:title="Intent Preference Title"
                      android:summary="Intent Preference Summary">

      <intent android:action="android.intent.action.VIEW"
              android:data="http://www.android.com" />

    </PreferenceScreen>

  </PreferenceCategory>

</PreferenceScreen>
```

조각 기본 설정에 대 한 코드는 다음과 같습니다.

```csharp
public class PrefFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        AddPreferencesFromResource(Resource.Xml.preferences);
    }
}
```

 <a name="Querying_Activities_to_Create_a_Preference_Fragment" />


### <a name="querying-activities-to-create-a-preference-fragment"></a>기본 설정 조각을 만드는 쿼리 작업

만들기 위한 또 다른 방법은 한 `PreferenceFragment` 활동을 쿼리 하는 포함 합니다. 각 작업에 사용할 수는 [메타 데이터\_키\_PREFERENCE](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/) 특성을 XML 리소스 파일을 가리킵니다. Xamarin.Android에서 작업을 표시 하 여 수행 됩니다는 `MetaDataAttribute`, 사용 하도록 리소스 파일을 지정 하 고 있습니다. `PreferenceFragment` 클래스는 메서드를 제공 [AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/)) 쿼리를이 XML 리소스를 찾아 그에 대 한 기본 설정 계층을 확장할 작업에 사용할 수 있는 합니다.

사용 하 여 다음 코드 조각에서이 프로세스의 예제를 보려면 `AddPreferencesFromIntent` 만들려는 `PreferenceFragment`:

```csharp
public class MyPreferenceFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        var intent = new Intent(this.Activity, typeof (MyActivityWithPreferences));
        AddPreferencesFromIntent(intent);
    }
}
```

Android 클래스 살펴봅니다 `MyActivityWithPreference`합니다. 와 클래스를 표시 해야 합니다는 `MetaDataAttribute,` 다음 코드 조각에 나와 있는 것 처럼:

```csharp
[Activity(Label = "My Activity with Preferences")]
[MetaData(PreferenceManager.MetadataKeyPreferences, Resource = "@xml/preference_from_intent")]
public class MyActivityWithPreferences : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        // This is deliberately blank
    }
}
```

`MetaDataAttribute` XML 리소스 파일을 선언는 `PreferenceFragment` 을 기본 설정 계층을 확장할 사용 합니다. 경우는 `MetatDataAttribute` 를 제공 하지 않으면 다음 런타임 시 예외가 throw 됩니다. 이 코드를 실행 하는 경우는 `PreferenceFragment` 다음 스크린 샷에서 같이 나타납니다.

[![PreferenceFragment 표시 된 예제 응용 프로그램의 스크린 샷](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)
