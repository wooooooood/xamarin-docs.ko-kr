---
title: 특수화된 조각 클래스
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: b004fbf121374a2bb3bf5d85f45d8cae293573bf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027310"
---
# <a name="specialized-fragment-classes"></a>특수화된 조각 클래스

조각 API는 응용 프로그램에서 찾을 수 있는 몇 가지 일반적인 기능을 캡슐화 하는 다른 서브 클래스를 제공 합니다. 이러한 서브 클래스는 다음과 같습니다.

- 이 **조각이 &ndash; 배열** 또는 커서와 같은 데이터 원본에 바인딩된 항목의 목록을 표시 하는 데 사용 됩니다.

- 이 조각을 사용 하는 **dialogfragment** &ndash; 대화 상자의 래퍼로 사용 됩니다. 조각은 활동의 맨 위에 대화 상자를 표시 합니다.

- **PreferenceFragment** &ndash;이 조각은 기본 설정 개체를 목록으로 표시 하는 데 사용 됩니다.

## <a name="the-listfragment"></a>ListFragment

`ListFragment`는 개념과 기능에서 `ListActivity`와 매우 비슷합니다. 조각에서 `ListView`를 호스팅하는 래퍼입니다. 아래 이미지는 태블릿 및 휴대폰에서 실행 되는 `ListFragment`을 보여줍니다.

[태블릿 및 휴대폰에서 ListFragment의![스크린샷](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)

### <a name="binding-data-with-the-listadapter"></a>ListAdapter를 사용 하 여 데이터 바인딩

`ListFragment` 클래스는 이미 기본 레이아웃을 제공 하므로 `ListFragment`내용을 표시 하기 위해 `OnCreateView`를 재정의할 필요가 없습니다. `ListView`는 `ListAdapter` 구현을 사용 하 여 데이터에 바인딩됩니다. 다음 예제에서는 간단한 문자열 배열을 사용 하 여이 작업을 수행 하는 방법을 보여 줍니다.

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

`ListAdapter`를 설정 하는 경우 `ListView.ListAdapter` 속성이 아닌 `ListFragment.ListAdapter` 속성을 사용 하는 것이 중요 합니다. `ListView.ListAdapter`를 사용 하면 중요 한 초기화 코드를 건너뛸 수 있습니다.

### <a name="responding-to-user-selection"></a>사용자 선택에 응답

사용자 선택 항목에 응답 하려면 응용 프로그램이 `OnListItemClick` 메서드를 재정의 해야 합니다. 다음 예에서는 이러한 가능성을 보여 줍니다.

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

위의 코드에서 사용자가 `ListFragment`항목을 선택 하면 호스팅 활동에 새 조각이 표시 되어 선택한 항목에 대 한 세부 정보가 표시 됩니다.

## <a name="dialogfragment"></a>DialogFragment

*Dialogfragment* 는 활동의 창 위에 배치 될 조각 내부에 대화 상자 개체를 표시 하는 데 사용 되는 조각입니다. 이는 관리 되는 대화 상자 Api (Android 3.0에서 시작)를 대체 하기 위한 것입니다. 다음 스크린샷은 `DialogFragment`의 예를 보여 줍니다.

[새 차량 추가 입력란을 표시 하는 DialogFragment의![스크린샷](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

`DialogFragment`는 조각과 대화 상자 간의 상태를 일관 되 게 유지 합니다. 대화 상자 개체에 대 한 모든 상호 작용 및 제어는 `DialogFragment` API를 통해 수행 되어야 하며 대화 상자 개체에 대 한 직접 호출로 생성 되지 않습니다. `DialogFragment` API는 각 인스턴스에 조각을 표시 하는 데 사용 되는 `Show()` 메서드를 제공 합니다. 다음 두 가지 방법으로 조각을 제거할 수 있습니다.

- `DialogFragment` 인스턴스에서 `DialogFragment.Dismiss()`를 호출 합니다. 

- 다른 `DialogFragment`를 표시 합니다.

`DialogFragment`를 만들려면 클래스가 `Android.App.DialogFragment,`에서 상속 된 후 다음 두 메서드 중 하나를 재정의 합니다.

- **Oncreateview** &ndash; 뷰를 만들어 반환 합니다.

- **Oncreatedialog** &ndash; 사용자 지정 대화 상자를 만듭니다. 일반적으로 *Alertdialog*를 표시 하는 데 사용 됩니다. 이 메서드를 재정의 하는 경우 `OnCreateView`를 재정의할 필요가 없습니다.

### <a name="a-simple-dialogfragment"></a>간단한 DialogFragment

다음 스크린샷은 `TextView`와 두 개의 `Button`를 포함 하는 간단한 `DialogFragment`을 보여 줍니다.

[TextView 및 두 개의 단추를 사용 하는 DialogFragment![예제](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

`TextView`는 사용자가 `DialogFragment`에서 한 단추를 클릭 한 횟수를 표시 하 고 다른 단추를 클릭 하면 조각이 닫힙니다. `DialogFragment` 코드는 다음과 같습니다.

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

모든 조각과 마찬가지로 `DialogFragment` `FragmentTransaction`컨텍스트에 표시 됩니다.

`DialogFragment`에 대 한 `Show()` 메서드는 `FragmentTransaction` 및 `string`를 입력으로 사용 합니다. 대화 상자가 활동에 추가 되 고 `FragmentTransaction` 커밋됩니다.

다음 코드에서는 작업에서 `Show()` 메서드를 사용 하 여 `DialogFragment`를 표시 하는 한 가지 방법을 보여 줍니다.

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```

### <a name="dismissing-a-fragment"></a>조각 해제

`DialogFragment` 인스턴스에서 `Dismiss()`를 호출 하면 조각이 작업에서 제거 되 고 해당 트랜잭션이 커밋됩니다.
조각의 소멸과 관련 된 표준 조각 수명 주기 메서드가 호출 됩니다.

### <a name="alert-dialog"></a>경고 대화 상자

`OnCreateView`를 재정의 하는 대신 `DialogFragment` `OnCreateDialog`를 재정의할 수 있습니다. 이렇게 하면 응용 프로그램에서 조각으로 관리 되는 [Alertdialog](xref:Android.App.AlertDialog) 를 만들 수 있습니다. 다음 코드는 `AlertDialog.Builder`를 사용 하 여 `Dialog`을 만드는 예입니다.

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

기본 설정 관리를 돕기 위해 조각 API는 `PreferenceFragment` 하위 클래스를 제공 합니다. `PreferenceFragment`는 &ndash; [PreferenceActivity](xref:Android.Preferences.PreferenceActivity) 와 비슷하며 사용자에 게 기본 설정의 계층 구조가 표시 됩니다. 사용자가 기본 설정과 상호 작용 하면 자동으로 [Sharedpreferences 설정](https://developer.android.com/reference/android/content/SharedPreferences.html)에 저장 됩니다.
Android 3.0 이상의 응용 프로그램에서 `PreferenceFragment`를 사용 하 여 응용 프로그램에서 기본 설정을 처리 합니다. 다음 그림에서는 `PreferenceFragment`의 예를 보여 줍니다.

[인라인, 대화 상자 및 시작 기본 설정을 사용 하는![예제 PreferencesFragment](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)

### <a name="create-a-preference-fragment-from-a-resource"></a>리소스에서 기본 설정 조각 만들기

기본 설정 조각은 [PreferenceFragment. AddPreferencesFromResource](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromResource*) 메서드를 사용 하 여 XML 리소스 파일에서 팽창 수 있습니다. 조각의 수명 주기에서이 메서드를 호출 하는 논리적 위치가 `OnCreate` 메서드에 있습니다.

위의 `PreferenceFragment` XML에서 리소스를 로드 하 여 만들었습니다. 리소스 파일은 다음과 같습니다.

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

### <a name="querying-activities-to-create-a-preference-fragment"></a>작업을 쿼리하여 기본 설정 조각 만들기

`PreferenceFragment`를 만드는 다른 방법에는 작업 쿼리가 포함 됩니다. 각 활동은 XML 리소스 파일을 가리키는 [메타 데이터\_KEY\_기본 설정](xref:Android.Preferences.PreferenceManager.MetadataKeyPreferences) 특성을 사용할 수 있습니다. Xamarin.ios에서 `MetaDataAttribute`작업을 표시할 하 고 사용할 리소스 파일을 지정 하 여이 작업을 수행 합니다. `PreferenceFragment` 클래스는 활동을 쿼리하여이 XML 리소스를 찾고이에 대 한 기본 설정 계층을 확장 하는 데 사용할 수 있는 [AddPreferenceFromIntent](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromIntent*) 메서드를 제공 합니다.

이 프로세스의 예제는 다음 코드 조각에서 제공 됩니다 .이 코드 조각에서는 `AddPreferencesFromIntent`를 사용 하 여 `PreferenceFragment`를 만듭니다.

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

Android는 `MyActivityWithPreference`클래스를 확인 합니다. 클래스는 다음 코드 조각과 같이 `MetaDataAttribute,` 표시 되어야 합니다.

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

`MetaDataAttribute`은 `PreferenceFragment` 기본 설정 계층을 확장 하는 데 사용 하는 XML 리소스 파일을 선언 합니다. `MetatDataAttribute` 제공 되지 않으면 런타임에 예외가 throw 됩니다. 이 코드가 실행 되 면 `PreferenceFragment` 다음 스크린샷에 표시 됩니다.

[PreferenceFragment가 표시 된 예제 앱의![스크린샷](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
