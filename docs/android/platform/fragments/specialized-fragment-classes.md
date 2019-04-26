---
title: 특수화된 조각 클래스
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 75d95d630415cdaa4c0c1ed3b8ddebb32b8e3c4d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60948054"
---
# <a name="specialized-fragment-classes"></a>특수화된 조각 클래스

조각 API 응용 프로그램의 일반적인 기능 중 일부를 캡슐화 하는 다른 하위 클래스를 제공 합니다. 이러한 서브 클래스 다음과 같습니다.

-   **ListFragment** &ndash; 배열이 나 커서 같은 데이터 원본에 바인딩된 항목 목록을 표시 하려면이 조각을 사용 됩니다.

-   **DialogFragment** &ndash; 이 조각 대화 주위에서 래퍼로 사용 됩니다. 조각에는 해당 활동을 기반으로 대화 상자를 표시 됩니다.

-   **PreferenceFragment** &ndash; 이 조각은 기본 설정 개체 목록으로 표시 하는 데 사용 됩니다.



## <a name="the-listfragment"></a>ListFragment

합니다 `ListFragment` 개념 및 기능을 매우 비슷합니다는 `ListActivity`; 래퍼를 호스트 하는 것을 `ListView` 조각에 합니다. 아래 이미지는 `ListFragment` 태블릿 및 휴대폰에서 실행 합니다.

[![스크린 샷을의 ListFragment 태블릿 및 휴대폰](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)


### <a name="binding-data-with-the-listadapter"></a>ListAdapter 사용 하 여 데이터 바인딩

`ListFragment` 클래스는 이미 기본 레이아웃을를 제공 있으므로 재정의할 필요가 없습니다 `OnCreateView` 의 내용을 표시 하는 `ListFragment`합니다. 합니다 `ListView` 를 사용 하 여 데이터에 바인딩되지 않은 `ListAdapter` 구현 합니다. 다음 예제는 간단한 문자열 배열을 사용 하 여 수행할 수 있습니다 하는 방법을 보여 줍니다.

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

설정 하는 경우는 `ListAdapter`, 사용 해야 합니다 `ListFragment.ListAdapter` 속성을 아니라는 `ListView.ListAdapter` 속성. 사용 하 여 `ListView.ListAdapter` 하면 중요 한 초기화 코드가 생략 됩니다.



### <a name="responding-to-user-selection"></a>사용자 선택에 응답

사용자 선택에 응답 하려면 응용 프로그램 재정의 해야 합니다는 `OnListItemClick` 메서드. 다음 예제에서는 이러한 한 가지 가능성을 보여 줍니다.

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

사용자가 항목을 선택 하는 경우 위의 코드에는 `ListFragment`, 선택 된 항목에 대 한 자세한 정보를 보여 주는 호스팅 작업에서 새 조각이 표시 됩니다.



## <a name="dialogfragment"></a>DialogFragment

합니다 *DialogFragment* 는 활동의 창 위에 떠 있는 조각 내에서 대화 상자 개체를 표시 하는 데 사용 되는 조각입니다. 관리 되는 대화 상자 (Android 3.0부터) Api를 대체 하는 것이 것입니다. 다음 스크린샷은 예제를 `DialogFragment`:

[![스크린 샷의 DialogFragment 새 차량 입력란 추가 표시 합니다.](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

`DialogFragment` 확인 대화 상자와 부분 간의 상태 일관성을 유지 하 합니다. 대화 상자 개체의 모든 컨트롤과 상호 작용을 통해 수행 되어야 합니다 `DialogFragment` API 및 대화 상자 개체에 대 한 직접 호출을 사용 하 여 설정할 수 없습니다. 합니다 `DialogFragment` 사용 하 여 각 인스턴스를 제공 하는 API는 `Show()` 조각을 표시 하는 데 사용 되는 메서드. 두 가지 방법으로 조각 제거 하려면:

-  호출 `DialogFragment.Dismiss()` 에 `DialogFragment` 인스턴스. 

-  다른 표시 `DialogFragment`합니다.

만들려는 `DialogFragment`, 클래스에서 상속 `Android.App.DialogFragment,` 후 다음 두 가지 방법 중 하나를 재정의 하 고:

- **OnCreateView** &ndash; 이 만들고 뷰를 반환 합니다.

- **OnCreateDialog** &ndash; 이 사용자 지정 대화 상자를 만듭니다. 일반적으로 표시 하는 데는 *AlertDialog*합니다. 재정의 하는 데 필요한 것이 메서드를 재정의할 때 `OnCreateView` 합니다.



### <a name="a-simple-dialogfragment"></a>간단한 DialogFragment

다음 스크린샷은 간단한 `DialogFragment` 있는 `TextView` 두 개의 `Button`s:

[![TextView를 및 두 개의 단추를 사용 하 여 예제 DialogFragment](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

합니다 `TextView` 사용자가 단추 하나를 클릭 하는 횟수를 표시 됩니다는 `DialogFragment`반면 다른 단추를 클릭 하면 조각 닫힙니다. 에 대 한 코드 `DialogFragment` 됩니다.

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


### <a name="displaying-a-fragment"></a>조각 표시

모든 조각이 같은 `DialogFragment` 의 컨텍스트에서 표시 되는 `FragmentTransaction`합니다.

`Show()` 메서드를 `DialogFragment` 사용을 `FragmentTransaction` 및 `string` 입력으로 합니다. 활동에 추가할 대화 상자 및 `FragmentTransaction` 커밋된 합니다.

다음 코드에서는 활동을 사용할 수 있습니다 하는 한 가지 방법을 합니다 `Show()` 표시할 메서드를 `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```


### <a name="dismissing-a-fragment"></a>조각 해제

호출 `Dismiss()` 인스턴스에서 `DialogFragment` 조각을 활동에서 제거할 시키고 해당 트랜잭션을 커밋합니다.
관련 조각 소멸 된 표준 조각 수명 주기 메서드 호출 됩니다.


### <a name="alert-dialog"></a>경고 대화 상자

재정의 하는 대신 `OnCreateView`, a `DialogFragment` 대신 재정의 될 수 있습니다 `OnCreateDialog`합니다. 이렇게 하면 응용 프로그램을 만들 수 있습니다.는 [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/) 조각을 통해 관리 되는 합니다. 다음 코드는 사용 하는 예제는 `AlertDialog.Builder` 만들려면를 `Dialog`:

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



## <a name="preferencefragment"></a>PreferenceFragment

조각 API는 기본 설정을 관리 하려면 다음을 제공 합니다.는 `PreferenceFragment` 하위 클래스입니다. 합니다 `PreferenceFragment` 비슷합니다는 [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/) &ndash; 계층을 사용자에 게 기본 설정 조각에 표시 됩니다. 사용자가 기본 설정에 따라 자동으로 저장 됩니다 하 [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html)합니다.
Android 3.0 또는 더 높은 응용 프로그램에서 사용 하 여는 `PreferenceFragment` 응용 프로그램에서 기본 설정을 사용 하 여 처리 합니다. 다음 그림의 예를 보여 줍니다.는 `PreferenceFragment`:

[![인라인, 대화 상자 및 시작 기본 설정을 사용 하 여 예제 PreferencesFragment](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)


### <a name="create-a-preference-fragment-from-a-resource"></a>리소스에서 기본 설정 조각 만들기

조각을 사용 하 여 XML 리소스 파일에서 팽창 될 수 있습니다 기본 설정 된 [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/) 메서드. 에 있는 논리 위치 조각의 수명 주기에서이 메서드를 호출 하는 것은 `OnCreate` 메서드.

`PreferenceFragment` 그림 위의 XML에서 리소스를 로드 하 여 생성 되었습니다. 리소스 파일은:

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

기본 설정 조각에 대 한 코드는 다음과 같습니다.

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



### <a name="querying-activities-to-create-a-preference-fragment"></a>쿼리 작업 기본 설정 조각을 만들려면

만들기 위한 다른 기법을 `PreferenceFragment` 활동 쿼리 합니다. 각 작업에 사용할 수는 [메타 데이터\_키\_기본 설정](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/) XML 리소스 파일을 가리키는 특성입니다. Xamarin.android에서 사용 하 여 활동을 표시 하면 됩니다는 `MetaDataAttribute`를 사용 하려면 리소스 파일을 지정 하 고 있습니다. `PreferenceFragment` 메서드를 제공 하는 클래스 [AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/))이 XML 리소스를 찾아에 대 한 기본 설정 계층을 확장 하기 위한 작업 쿼리를 사용할 수 있는 합니다.

이 프로세스의 예는 다음 코드 조각에서 제공 됩니다 `AddPreferencesFromIntent` 만들려면를 `PreferenceFragment`:

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

Android 클래스 살펴봅니다 `MyActivityWithPreference`합니다. 클래스를 사용 하 여 표시할 수 해야는 `MetaDataAttribute,` 다음 코드 조각과에서 같이:

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

합니다 `MetaDataAttribute` 는 XML 리소스 파일을 선언 합니다 `PreferenceFragment` 기본 계층을 확장 하는 데 사용할 합니다. 경우는 `MetatDataAttribute` 를 제공 하지 않으면 다음 런타임 시 예외가 throw 됩니다. 이 코드를 실행 하는 경우는 `PreferenceFragment` 다음 스크린샷과 같이 표시 됩니다.

[![PreferenceFragment 표시 된 예제 앱 스크린샷](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
