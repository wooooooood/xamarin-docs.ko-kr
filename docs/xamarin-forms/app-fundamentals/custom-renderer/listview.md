---
title: ListView 사용자 지정
description: Xamarin.Forms ListView는 데이터의 컬렉션을 세로 목록으로 표시하는 보기입니다. 이 문서에서는 네이티브 목록 컨트롤 성능을 보다 효과적으로 제어할 수 있도록 플랫폼별 리스트 컨트롤과 네이티브 셀 레이아웃을 캡슐화하는 사용자 지정 렌더러를 만드는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 39ba281f036b9c57f85629390f5ba76377c99dd8
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054213"
---
# <a name="customizing-a-listview"></a>ListView 사용자 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)

_Xamarin.Forms ListView는 데이터의 컬렉션을 세로 목록으로 표시하는 보기입니다. 이 문서에서는 네이티브 목록 컨트롤 성능을 보다 효과적으로 제어할 수 있도록 플랫폼별 리스트 컨트롤과 네이티브 셀 레이아웃을 캡슐화하는 사용자 지정 렌더러를 만드는 방법을 보여줍니다._

모든 Xamarin.Forms 보기에는 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대해 함께 제공되는 렌더러가 있습니다. iOS의 Xamarin.Forms 애플리케이션에서 [`ListView`](xref:Xamarin.Forms.ListView)를 렌더링하면 `ListViewRenderer` 클래스가 인스턴스화되며, 차례로 네이티브 `UITableView` 컨트롤을 인스턴스화합니다. Android 플랫폼에서 `ListViewRenderer` 클래스는 네이티브 `ListView` 컨트롤을 인스턴스화합니다. UWP(유니버설 Windows 플랫폼)에서 `ListViewRenderer` 클래스는 네이티브 `ListView` 컨트롤을 인스턴스화합니다. Xamarin.Forms 컨트롤에 매핑되는 렌더러 및 네이티브 컨트롤 클래스에 대한 자세한 내용은 [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)을 참조하세요.

다음 다이어그램은 [`ListView`](xref:Xamarin.Forms.ListView) 컨트롤 및 이를 구현하는 해당 네이티브 컨트롤 간의 관계를 보여줍니다.

![](listview-images/listview-classes.png "ListView 컨트롤과 네이티브 컨트롤 구현 간의 관계")

렌더링 프로세스는 각 플랫폼에서 [`ListView`](xref:Xamarin.Forms.ListView)에 대한 사용자 지정 렌더러를 만들어 플랫폼별 사용자 지정을 구현하는 데 활용할 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin.Forms 사용자 지정 컨트롤을 [만듭니다](#Creating_the_Custom_ListView_Control).
1. Xamarin.Forms에서 사용자 지정 컨트롤을 [사용합니다](#Consuming_the_Custom_Control).
1. 각 플랫폼의 컨트롤에 대한 사용자 지정 렌더러를 [만듭니다](#Creating_the_Custom_Renderer_on_each_Platform).

이제 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 활용하는 `NativeListView` 렌더러를 구현하도록 각 항목을 차례로 설명합니다. 이 시나리오는 다시 사용할 수 있는 목록 및 셀 코드를 포함하는 기존 네이티브 앱을 이식하는 경우에 유용합니다. 또한 데이터 가상화와 같은 성능에 영향을 줄 수 있는 목록 컨트롤 기능의 자세한 사용자 지정을 허용합니다.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>사용자 지정 ListView 컨트롤 만들기

다음 코드 예제와 같이 `ListView` 클래스를 서브클래스하여 사용자 지정 [`ListView`](xref:Xamarin.Forms.ListView) 컨트롤을 만들 수 있습니다.

```csharp
public class NativeListView : ListView
{
  public static readonly BindableProperty ItemsProperty =
    BindableProperty.Create ("Items", typeof(IEnumerable<DataSource>), typeof(NativeListView), new List<DataSource> ());

  public IEnumerable<DataSource> Items {
    get { return (IEnumerable<DataSource>)GetValue (ItemsProperty); }
    set { SetValue (ItemsProperty, value); }
  }

  public event EventHandler<SelectedItemChangedEventArgs> ItemSelected;

  public void NotifyItemSelected (object item)
  {
    if (ItemSelected != null) {
      ItemSelected (this, new SelectedItemChangedEventArgs (item));
    }
  }
}
```

`NativeListView`는 .NET 표준 라이브러리 프로젝트에서 생성되고 사용자 지정 컨트롤에 대한 API를 정의합니다. 이 컨트롤은 데이터로 `ListView`를 채우는 데 사용되는 `Items` 속성을 노출하며, 표시 목적을 위해 바인딩되는 데이터가 될 수 있습니다. 또한 항목이 플랫폼별 네이티브 목록 컨트롤에서 선택될 때마다 발생하는 `ItemSelected` 이벤트를 노출합니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>사용자 지정 컨트롤 사용

`NativeListView` 사용자 지정 컨트롤은 해당 위치의 네임스페이스를 선언하고 컨트롤의 네임스페이스 접두사를 사용하여 .NET 표준 라이브러리 프로젝트의 Xaml에서 참조할 수 있습니다. 다음 코드 예제에서는 `NativeListView` 사용자 지정 컨트롤을 XAML 페이지에서 사용하는 방법을 보여줍니다.

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*" />
            </Grid.RowDefinitions>
          <Label Text="{x:Static local:App.Description}" HorizontalTextAlignment="Center" />
            <local:NativeListView Grid.Row="1" x:Name="nativeListView" ItemSelected="OnItemSelected" VerticalOptions="FillAndExpand" />
          </Grid>
      </ContentPage.Content>
</ContentPage>
```

`local` 네임스페이스 접두사는 원하는 이름으로 지정할 수 있습니다. 그러나 `clr-namespace` 및 `assembly` 값은 사용자 지정 컨트롤의 세부 정보와 일치해야 합니다. 네임스페이스가 선언되면 사용자 지정 컨트롤을 참조하는 데 접두사가 사용됩니다.

다음 코드 예제에서는 C# 페이지에서 `NativeListView` 사용자 지정 컨트롤을 사용하는 방법을 보여줍니다.

```csharp
public class MainPageCS : ContentPage
{
    NativeListView nativeListView;

    public MainPageCS()
    {
        nativeListView = new NativeListView
        {
            Items = DataSource.GetList(),
            VerticalOptions = LayoutOptions.FillAndExpand
        };

        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
                Padding = new Thickness(0, 20, 0, 0);
                break;
            case Device.Android:
            case Device.UWP:
                Padding = new Thickness(0);
                break;
        }

        Content = new Grid
        {
            RowDefinitions = {
                new RowDefinition { Height = GridLength.Auto },
                new RowDefinition { Height = new GridLength (1, GridUnitType.Star) }
            },
            Children = {
                new Label { Text = App.Description, HorizontalTextAlignment = TextAlignment.Center },
                nativeListView
            }
        };
        nativeListView.ItemSelected += OnItemSelected;
    }
    ...
}
```

`NativeListView` 사용자 지정 컨트롤은 `Items` 속성을 통해 채워지는 데이터의 목록을 표시하는 데 플랫폼별 사용자 지정 렌더러를 사용합니다. 목록의 각 행에는 이름, 범주 및 이미지 파일 이름의 세 가지 데이터 항목이 있습니다. 목록에서 각 행의 레이아웃은 플랫폼별 사용자 지정 렌더러를 통해 정의됩니다.

> [!NOTE]
> `NativeListView` 사용자 지정 컨트롤은 스크롤 기능을 포함하는 플랫폼별 목록 컨트롤을 사용하여 렌더링되므로 사용자 지정 컨트롤은 [`ScrollView`](xref:Xamarin.Forms.ScrollView)와 같은 스크롤할 수 있는 레이아웃 컨트롤에서 호스트되어서는 안 됩니다.

이제 각 애플리케이션 프로젝트에 사용자 지정 렌더러를 추가하여 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 만들 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 사용자 지정 컨트롤을 렌더링하는 `ListViewRenderer` 클래스의 서브클래스를 만듭니다.
1. 사용자 지정 컨트롤을 렌더링하고 이를 사용자 지정하기 위한 논리를 작성하는 `OnElementChanged` 메서드를 재정의합니다. 이 메서드는 해당 Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView)가 생성될 때 호출됩니다.
1. 사용자 지정 렌더러 클래스에 `ExportRenderer` 특성을 추가하여 Xamarin.Forms 사용자 지정 컨트롤을 렌더링하는 데 사용하도록 지정합니다. 이 특성은 사용자 지정 랜더러를 Xamarin.Forms에 등록하는 데 사용됩니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공하는 것은 선택 사항입니다. 사용자 지정 렌더러가 등록되지 않은 경우 셀의 기본 클래스에 대한 기본 렌더러가 사용됩니다.

다음 다이어그램은 샘플 애플리케이션에서 각 프로젝트의 책임과 이들 간의 관계를 보여줍니다.

![](listview-images/solution-structure.png "NativeListView 사용자 지정 렌더러 프로젝트 책임")

`NativeListView` 사용자 지정 컨트롤은 각 플랫폼의 `ListViewRenderer` 클래스에서 모두 파생되는 플랫폼별 렌더러 클래스에 의해 렌더링됩니다. 그러면 다음 스크린샷과 같이 각 `NativeListView` 사용자 지정 컨트롤이 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃으로 렌더링됩니다.

![](listview-images/screenshots.png "각 플랫폼의 NativeListView")

`ListViewRenderer` 클래스는 해당 네이티브 컨트롤을 렌더링하기 위해 Xamarin.Forms 사용자 지정 컨트롤이 생성될 때 호출되는 `OnElementChanged` 메서드를 노출합니다. 이 메서드는 `OldElement` 및 `NewElement` 속성이 포함된 `ElementChangedEventArgs` 매개 변수를 가져옵니다. 이러한 속성은 랜더러가 연결*된* Xamarin.Forms 요소와 렌더러가 연결*되는* Xamarin.Forms 요소를 각각 나타냅니다. 샘플 애플리케이션에서 `OldElement` 속성은 `null`이고, `NewElement` 속성은 `NativeListView` 인스턴스에 대한 참조를 포함합니다.

각 플랫폼별 렌더러 클래스에서 `OnElementChanged` 메서드의 재정의된 버전은 네이티브 컨트롤 사용자 지정을 수행하는 위치입니다. 플랫폼에서 사용되는 네이티브 컨트롤에 대한 형식화된 참조는 `Control` 속성을 통해 액세스할 수 있습니다. 또한 랜더링되는 Xamarin.Forms 컨트롤에 대한 참조는 `Element` 속성을 통해 얻을 수 있습니다.

`OnElementChanged` 메서드에서 이벤트 처리기를 구독할 때는 다음 코드 예제와 같이 주의해야 합니다.

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결된 경우 네이티브 컨트롤만 구성해야 합니다. 마찬가지로, 구독된 모든 이벤트 처리기는 렌더러가 연결된 요소가 변경될 때만 구독을 취소해야 합니다. 이러한 방식을 채택하면 메모리 누수가 없는 사용자 지정 렌더러를 만들 수 있습니다.

각 플랫폼별 렌더러 클래스에서 `OnElementPropertyChanged` 메서드의 재정의된 버전은 Xamarin.Forms 사용자 지정 컨트롤의 바인딩 가능한 속성 변경 내용에 응답하는 위치입니다. 이 재정의는 여러 번 호출될 수 있으므로 변경되는 속성에 대한 검사는 항상 수행되어야 합니다.

각 사용자 지정 렌더러 클래스는 랜더러를 Xamarin.Forms에 등록하는 `ExportRenderer` 속성으로 데코레이트됩니다. 이 특성은 렌더링되는 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름과 지정 렌더러의 형식 이름이라는 두 가지 매개 변수를 사용합니다. 특성의 `assembly` 접두사는 특성이 전체 어셈블리에 적용되도록 지정합니다.

다음 섹션에서는 각 플랫폼별 사용자 지정 렌더러 클래스의 구현을 설명합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>iOS에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼용 사용자 지정 렌더러를 보여줍니다.

```csharp
[assembly: ExportRenderer (typeof(NativeListView), typeof(NativeiOSListViewRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSListViewRenderer : ListViewRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null) {
                // Unsubscribe
            }

            if (e.NewElement != null) {
                Control.Source = new NativeiOSListViewSource (e.NewElement as NativeListView);
            }
        }
    }
}
```

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 `UITableView` 컨트롤은 `NativeiOSListViewSource` 클래스의 인스턴스를 만들어서 구성됩니다. 이 클래스는 `UITableViewSource` 클래스에서 `RowsInSection` 및 `GetCell` 메서드를 재정의하고, 표시되는 데이터의 목록을 포함하는 `Items` 속성을 노출하여 `UITableView` 컨트롤에 데이터를 제공합니다. 클래스는 `NativeListView` 사용자 지정 컨트롤에서 제공하는 `ItemSelected` 이벤트를 호출하는 `RowSelected` 메서드 재정의도 제공합니다. 메서드 재정의에 대한 자세한 내용은 [UITableViewSource 서브클래싱](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)을 참조하세요. `GetCell` 메서드는 목록의 각 행에 대한 데이터로 채워지는 `UITableCellView`를 반환하며, 다음 코드 예제에 표시됩니다.

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
  // request a recycled cell to save memory
  NativeiOSListViewCell cell = tableView.DequeueReusableCell (cellIdentifier) as NativeiOSListViewCell;

  // if there are no cells to reuse, create a new one
  if (cell == null) {
    cell = new NativeiOSListViewCell (cellIdentifier);
  }

  if (String.IsNullOrWhiteSpace (tableItems [indexPath.Row].ImageFilename)) {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , null);
  } else {
    cell.UpdateCell (tableItems [indexPath.Row].Name
      , tableItems [indexPath.Row].Category
      , UIImage.FromFile ("Images/" + tableItems [indexPath.Row].ImageFilename + ".jpg"));
  }

  return cell;
}
```

이 메서드는 화면에 표시될 데이터의 각 행에 대한 `NativeiOSListViewCell` 인스턴스를 만듭니다. `NativeiOSCell` 인스턴스는 각 셀의 레이아웃 및 셀의 데이터를 정의합니다. 셀이 스크롤로 인해 화면에서 사라지면 셀은 재사용을 위해 사용할 수 있도록 생성됩니다. 이렇게 하면 목록에 있는 모든 데이터가 아니라 화면에 표시되는 데이터에 대해 `NativeiOSCell` 인스턴스만 있는 것을 보장하여 메모리 낭비를 방지합니다. 셀 재사용에 대한 자세한 내용은 [셀 재사용](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)을 참조하세요. `GetCell` 메서드는 데이터의 각 행의 `ImageFilename` 속성을 읽고(존재하는 경우), 이미지를 읽고, 행에 대한 데이터(이름, 범주 및 이미지)로 `NativeiOSListViewCell` 인스턴스를 업데이트하기 전에 `UIImage` 인스턴스로 저장합니다.

`NativeiOSListViewCell` 클래스는 각 셀에 대한 레이아웃을 정의하며, 다음 코드 예제에 표시됩니다.

```csharp
public class NativeiOSListViewCell : UITableViewCell
{
  UILabel headingLabel, subheadingLabel;
  UIImageView imageView;

  public NativeiOSListViewCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
  {
    SelectionStyle = UITableViewCellSelectionStyle.Gray;

    ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);

    imageView = new UIImageView ();

    headingLabel = new UILabel () {
      Font = UIFont.FromName ("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB (127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    subheadingLabel = new UILabel () {
      Font = UIFont.FromName ("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB (38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add (headingLabel);
    ContentView.Add (subheadingLabel);
    ContentView.Add (imageView);
  }

  public void UpdateCell (string caption, string subtitle, UIImage image)
  {
    headingLabel.Text = caption;
    subheadingLabel.Text = subtitle;
    imageView.Image = image;
  }

  public override void LayoutSubviews ()
  {
    base.LayoutSubviews ();

    headingLabel.Frame = new CoreGraphics.CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
    subheadingLabel.Frame = new CoreGraphics.CGRect (100, 18, 100, 20);
    imageView.Frame = new CoreGraphics.CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

이 클래스는 셀의 콘텐츠 및 해당 레이아웃을 렌더링하는 데 사용하는 컨트롤을 정의합니다. `NativeiOSListViewCell` 생성자는 `UILabel` 및 `UIImageView` 컨트롤의 인스턴스를 만들고, 모양을 초기화합니다. 이러한 컨트롤은 `UILabel` 및 `UIImageView` 인스턴스에서 이 데이터를 설정하는 데 사용되는 `UpdateCell` 메서드와 함께 각 행의 데이터를 표시하는 데 사용됩니다. 이러한 인스턴스의 위치는 재정의된 `LayoutSubviews` 메서드 및 셀 내의 해당 좌표를 지정하여 설정됩니다.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>사용자 지정 컨트롤의 속성 변경에 응답

`NativeListView.Items` 속성이 목록에 추가되거나 목록에서 제거되는 항목으로 인해 변경되는 경우 사용자 지정 렌더러는 변경 내용을 표시하여 응답해야 합니다. 다음 코드 예제에 표시되는 `OnElementPropertyChanged` 메서드를 재정의하여 이 작업을 수행할 수 있습니다.

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

메서드는 바인딩 가능한 `NativeListView.Items` 속성이 변경되는 경우 `UITableView` 컨트롤에 데이터를 제공하는 `NativeiOSListViewSource` 클래스의 새 인스턴스를 만듭니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼용 사용자 지정 렌더러를 보여줍니다.

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeAndroidListViewRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidListViewRenderer : ListViewRenderer
    {
        Context _context;

        public NativeAndroidListViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // unsubscribe
                Control.ItemClick -= OnItemClick;
            }

            if (e.NewElement != null)
            {
                // subscribe
                Control.Adapter = new NativeAndroidListViewAdapter(_context as Android.App.Activity, e.NewElement as NativeListView);
                Control.ItemClick += OnItemClick;
            }
        }
        ...

        void OnItemClick(object sender, Android.Widget.AdapterView.ItemClickEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(((NativeListView)Element).Items.ToList()[e.Position - 1]);
        }
    }
}
```

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 네이티브 `ListView` 컨트롤이 구성됩니다. 이 구성은 네이티브 `ListView` 컨트롤에 데이터를 제공하는 `NativeAndroidListViewAdapter` 클래스의 인스턴스를 만들고, `ItemClick` 이벤트를 처리하는 이벤트 처리기를 등록하는 작업을 포함합니다. 그런 다음, 이 처리기는 `NativeListView` 사용자 지정 컨트롤에서 제공하는 `ItemSelected` 이벤트를 호출합니다. `ItemClick` 이벤트는 렌더러가 변경 내용에 연결된 Xamarin.Forms 요소에서만 구독이 취소됩니다.

`NativeAndroidListViewAdapter`는 `BaseAdapter` 클래스에서 파생되며 `Count`, `GetView`, `GetItemId` 및 `this[int]` 메서드 재정의 뿐만 아니라 표시되는 데이터의 목록을 포함하는 `Items` 속성을 노출합니다. 이러한 메서드 재정의에 대한 자세한 내용은 [ListAdapter 구현](~/android/user-interface/layouts/list-view/populating.md)을 참조하세요. `GetView` 메서드는 데이터로 채워지는 각 행에 대한 보기를 반환하며, 다음 코드 예제에 표시됩니다.

```csharp
public override View GetView (int position, View convertView, ViewGroup parent)
{
  var item = tableItems [position];

  var view = convertView;
  if (view == null) {
    // no view to re-use, create new
    view = context.LayoutInflater.Inflate (Resource.Layout.NativeAndroidListViewCell, null);
  }
  view.FindViewById<TextView> (Resource.Id.Text1).Text = item.Name;
  view.FindViewById<TextView> (Resource.Id.Text2).Text = item.Category;

  // grab the old image and dispose of it
  if (view.FindViewById<ImageView> (Resource.Id.Image).Drawable != null) {
    using (var image = view.FindViewById<ImageView> (Resource.Id.Image).Drawable as BitmapDrawable) {
      if (image != null) {
        if (image.Bitmap != null) {
          //image.Bitmap.Recycle ();
          image.Bitmap.Dispose ();
        }
      }
    }
  }

  // If a new image is required, display it
  if (!String.IsNullOrWhiteSpace (item.ImageFilename)) {
    context.Resources.GetBitmapAsync (item.ImageFilename).ContinueWith ((t) => {
      var bitmap = t.Result;
      if (bitmap != null) {
        view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (bitmap);
        bitmap.Dispose ();
      }
    }, TaskScheduler.FromCurrentSynchronizationContext ());
  } else {
    // clear the image
    view.FindViewById<ImageView> (Resource.Id.Image).SetImageBitmap (null);
  }

  return view;
}
```

`GetView` 메서드는 목록에서 데이터의 각 행에 대해 `View`로 렌더링되는 셀을 반환하도록 호출됩니다. 레이아웃 파일에서 정의되는 `View` 인스턴스의 모양으로 화면에 표시될 데이터의 각 행에 대한 `View` 인스턴스를 만듭니다. 셀이 스크롤로 인해 화면에서 사라지면 셀은 재사용을 위해 사용할 수 있도록 생성됩니다. 이렇게 하면 목록에 있는 모든 데이터가 아니라 화면에 표시되는 데이터에 대해 `View` 인스턴스만 있는 것을 보장하여 메모리 낭비를 방지합니다. 보기 재사용에 대한 자세한 내용은 [행 보기 재사용](~/android/user-interface/layouts/list-view/populating.md)을 참조하세요.

`GetView` 메서드는 또한 `ImageFilename` 속성에 지정된 파일 이름에서 이미지 데이터 읽기를 포함하여 데이터로 `View` 인스턴스를 채웁니다.

네이티브 `ListView`로 표시되는 각 셀의 레이아웃은 `LayoutInflater.Inflate` 메서드로 확장되는 `NativeAndroidListViewCell.axml` 레이아웃 파일에서 정의됩니다. 다음 코드 예제에서는 레이아웃 정의를 보여줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:padding="8dp"
    android:background="@drawable/CustomSelector">
    <LinearLayout
        android:id="@+id/Text"
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
            android:textStyle="italic" />
        <TextView
            android:id="@+id/Text2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="14dip"
            android:textColor="#FF267F00"
            android:paddingLeft="100dip" />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout>
```

이 레이아웃은 두 개의 `TextView` 컨트롤 및 `ImageView` 컨트롤이 셀의 콘텐츠를 표시하는 데 사용되는 것을 지정합니다. 두 개의 `TextView` 컨트롤은 `RelativeLayout` 내에 포함되는 모든 컨트롤을 사용하여 `LinearLayout` 컨트롤 내에서 세로 방향으로 설정됩니다.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>사용자 지정 컨트롤의 속성 변경에 응답

`NativeListView.Items` 속성이 목록에 추가되거나 목록에서 제거되는 항목으로 인해 변경되는 경우 사용자 지정 렌더러는 변경 내용을 표시하여 응답해야 합니다. 다음 코드 예제에 표시되는 `OnElementPropertyChanged` 메서드를 재정의하여 이 작업을 수행할 수 있습니다.

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

메서드는 바인딩 가능한 `NativeListView.Items` 속성이 변경되는 경우 네이티브 `ListView` 컨트롤에 데이터를 제공하는 `NativeAndroidListViewAdapter` 클래스의 새 인스턴스를 만듭니다.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP에서 사용자 지정 렌더러 만들기

다음 코드 예제는 UWP용 사용자 지정 렌더러를 보여줍니다.

```csharp
[assembly: ExportRenderer(typeof(NativeListView), typeof(NativeUWPListViewRenderer))]
namespace CustomRenderer.UWP
{
    public class NativeUWPListViewRenderer : ListViewRenderer
    {
        ListView listView;

        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.ListView> e)
        {
            base.OnElementChanged(e);

            listView = Control as ListView;

            if (e.OldElement != null)
            {
                // Unsubscribe
                listView.SelectionChanged -= OnSelectedItemChanged;
            }

            if (e.NewElement != null)
            {
                listView.SelectionMode = ListViewSelectionMode.Single;
                listView.IsItemClickEnabled = false;
                listView.ItemsSource = ((NativeListView)e.NewElement).Items;             
                listView.ItemTemplate = App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
                // Subscribe
                listView.SelectionChanged += OnSelectedItemChanged;
            }  
        }

        void OnSelectedItemChanged(object sender, SelectionChangedEventArgs e)
        {
            ((NativeListView)Element).NotifyItemSelected(listView.SelectedItem);
        }
    }
}
```

사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 네이티브 `ListView` 컨트롤이 구성됩니다. 이 구성은 네이티브 `ListView` 컨트롤이 선택되는 항목에 응답하는 방법 설정, 컨트롤로 표시되는 데이터 채우기, 각 셀의 모양 및 콘텐츠 정의 및 `SelectionChanged` 이벤트를 처리하는 이벤트 처리기 등록 작업을 포함합니다. 그런 다음, 이 처리기는 `NativeListView` 사용자 지정 컨트롤에서 제공하는 `ItemSelected` 이벤트를 호출합니다. `SelectionChanged` 이벤트는 렌더러가 변경 내용에 연결된 Xamarin.Forms 요소에서만 구독이 취소됩니다.

각 네이티브 `ListView` 셀의 모양 및 콘텐츠는 `ListViewItemTemplate`이라는 `DataTemplate`에 의해 정의됩니다. 이 `DataTemplate`은 애플리케이션 수준 리소스 사전에 저장되며, 다음 코드 예제에 표시됩니다.

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="#DAFF7F">
        <Grid.Resources>
            <local:ConcatImageExtensionConverter x:Name="ConcatImageExtensionConverter" />
        </Grid.Resources>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.40*" />
            <ColumnDefinition Width="0.40*"/>
            <ColumnDefinition Width="0.20*" />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.ColumnSpan="2" Foreground="#7F3300" FontStyle="Italic" FontSize="22" VerticalAlignment="Top" Text="{Binding Name}" />
        <TextBlock Grid.RowSpan="2" Grid.Column="1" Foreground="#267F00" FontWeight="Bold" FontSize="12" VerticalAlignment="Bottom" Text="{Binding Category}" />
        <Image Grid.RowSpan="2" Grid.Column="2" HorizontalAlignment="Left" VerticalAlignment="Center" Source="{Binding ImageFilename, Converter={StaticResource ConcatImageExtensionConverter}}" Width="50" Height="50" />
        <Line Grid.Row="1" Grid.ColumnSpan="3" X1="0" X2="1" Margin="30,20,0,0" StrokeThickness="1" Stroke="LightGray" Stretch="Fill" VerticalAlignment="Bottom" />
    </Grid>
</DataTemplate>
```

`DataTemplate`은 셀의 콘텐츠, 해당 레이아웃 및 모양을 표시하는 데 사용되는 컨트롤을 지정합니다. 두 개의 `TextBlock` 컨트롤 및 `Image` 컨트롤은 데이터 바인딩을 통해 셀의 콘텐츠를 표시하는 데 사용됩니다. 또한 `ConcatImageExtensionConverter`의 인스턴스는 각 이미지 파일 이름에 `.jpg` 파일 확장을 연결하는 데 사용됩니다. 이렇게 하면 `Image` 컨트롤은 해당 `Source` 속성이 설정될 때 이미지를 로드하고 렌더링할 수 있습니다.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>사용자 지정 컨트롤의 속성 변경에 응답

`NativeListView.Items` 속성이 목록에 추가되거나 목록에서 제거되는 항목으로 인해 변경되는 경우 사용자 지정 렌더러는 변경 내용을 표시하여 응답해야 합니다. 다음 코드 예제에 표시되는 `OnElementPropertyChanged` 메서드를 재정의하여 이 작업을 수행할 수 있습니다.

```csharp
protected override void OnElementPropertyChanged(object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
    base.OnElementPropertyChanged(sender, e);

    if (e.PropertyName == NativeListView.ItemsProperty.PropertyName)
    {
        listView.ItemsSource = ((NativeListView)Element).Items;
    }
}
```

바인딩 가능한 `NativeListView.Items` 속성이 변경되는 경우 메서드는 변경된 데이터로 네이티브 `ListView`컨트롤을 다시 채웁니다.

## <a name="summary"></a>요약

이 문서에서는 네이티브 목록 컨트롤 성능을 보다 효과적으로 제어할 수 있도록 플랫폼별 리스트 컨트롤과 네이티브 셀 레이아웃을 캡슐화하는 사용자 지정 렌더러를 만드는 방법을 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererListView(샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
