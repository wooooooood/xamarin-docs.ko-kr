---
title: ListView의 사용자 지정
description: Xamarin.Forms ListView 세로 목록으로 데이터의 컬렉션을 표시 하는 뷰입니다. 이 문서에는 사용자 지정 렌더러를 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 캡슐화 하는 네이티브 목록 제어 성능 더욱 잘 제어할 수 있도록 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: b3b73d542faebdb8ab85c989d7812368f4f3ffac
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997492"
---
# <a name="customizing-a-listview"></a>ListView의 사용자 지정

_Xamarin.Forms ListView 세로 목록으로 데이터의 컬렉션을 표시 하는 뷰입니다. 이 문서에는 사용자 지정 렌더러를 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 캡슐화 하는 네이티브 목록 제어 성능 더욱 잘 제어할 수 있도록 만드는 방법을 보여 줍니다._

모든 Xamarin.Forms 보기에 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대 한는 함께 제공 되는 렌더러. 경우는 [ `ListView` ](xref:Xamarin.Forms.ListView) iOS에서 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `ListViewRenderer` 클래스가 인스턴스화되면 네이티브를 다시 인스턴스화하는 `UITableView` 컨트롤입니다. Android 플랫폼에는 `ListViewRenderer` 클래스에는 네이티브 인스턴스화합니다 `ListView` 컨트롤입니다. Windows 플랫폼 (UWP (유니버설), 합니다 `ListViewRenderer` 클래스 인스턴스화합니다 네이티브 `ListView` 컨트롤입니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 하세요. [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램 간의 관계는 [ `ListView` ](xref:Xamarin.Forms.ListView) 제어 및 구현 하는 해당 네이티브 컨트롤:

![](listview-images/listview-classes.png "ListView 컨트롤 구현 네이티브 컨트롤 간의 관계")

렌더링 프로세스를 수행할 수 활용에 대 한 사용자 지정 렌더러를 만들어 플랫폼별 사용자 지정을 구현 하는 [ `ListView` ](xref:Xamarin.Forms.ListView) 각 플랫폼에서 합니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_ListView_Control) Xamarin.Forms 사용자 지정 컨트롤입니다.
1. [사용할](#Consuming_the_Custom_Control) Xamarin.Forms에서 사용자 지정 컨트롤입니다.
1. [만들](#Creating_the_Custom_Renderer_on_each_Platform) 각 플랫폼에서 컨트롤에 대 한 사용자 지정 렌더러.

각 항목 이제 살펴봅니다를 구현 하는 `NativeListView` 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 활용 하는 렌더러. 이 시나리오는 다시 사용할 수 있는 셀 코드 및 목록을 포함 하는 기존 네이티브 앱을 이식 하는 경우에 유용 합니다. 또한 데이터 가상화와 같은 성능에 영향을 줄 수 있는 목록 컨트롤 기능 자세한 사용자 지정할 수 있습니다.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>사용자 지정 ListView 컨트롤 만들기

사용자 지정 [ `ListView` ](xref:Xamarin.Forms.ListView) 컨트롤을 서브클래싱하 여 생성할 수는 `ListView` 클래스에 다음 코드 예제 에서처럼:

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

`NativeListView` .NET Standard 라이브러리 프로젝트에서 생성 되 고 사용자 지정 컨트롤에 대 한 API를 정의 합니다. 이 컨트롤을 표시는 `Items` 채우는 데 사용 되는 속성을 `ListView` 데이터 및 수에 대 한에 바인딩된 데이터 표시 목적으로 합니다. 또한 노출는 `ItemSelected` 플랫폼 특정 네이티브 목록 컨트롤에서 항목을 선택할 때마다 발생 하는 이벤트입니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>사용자 지정 컨트롤 사용

`NativeListView` 사용자 지정 컨트롤 참조할 수 있습니다 Xaml의.NET Standard 라이브러리 프로젝트에서 해당 위치에 대 한 네임 스페이스를 선언 하 고 컨트롤에 네임 스페이스 접두사를 사용 합니다. 다음 코드 예제에서는 방법을 `NativeListView` XAML 페이지에서 사용자 지정 컨트롤을 사용할 수 있습니다.

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

`local` 원하는 네임 스페이스 접두사를 이름을 지정할 수 있습니다. 그러나 합니다 `clr-namespace` 고 `assembly` 값 사용자 지정 컨트롤의 세부 정보를 일치 해야 합니다. 네임 스페이스를 선언 하는 사용자 지정 컨트롤을 참조 하는 접두사가 사용 됩니다.

다음 코드 예제에서는 방법을 `NativeListView` C# 페이지에서 사용자 지정 컨트롤을 사용할 수 있습니다.

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

합니다 `NativeListView` 사용자 지정 컨트롤 플랫폼별 사용자 지정 렌더러를 사용 하 여 데이터를 통해 채워진 목록을 표시 하는 `Items` 속성입니다. 목록의 각 행에는 이름, 범주 및 이미지 파일 이름에는 데이터의 세 가지 항목이 있습니다. 목록의 각 행의 레이아웃은 플랫폼 고유의 사용자 지정 렌더러를 통해 정의 됩니다.

> [!NOTE]
> 때문에 `NativeListView` 스크롤 기능을 포함 하는 플랫폼 특정 목록 컨트롤을 사용 하 여 사용자 지정 컨트롤이 렌더링 될, 사용자 지정 컨트롤 호스팅해서는 안 스크롤할 수 있는 레이아웃 컨트롤의 예는 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView)합니다.

사용자 지정 렌더러는 이제 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 만들려면 각 응용 프로그램 프로젝트에 추가할 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든를 `ListViewRenderer` 사용자 지정 컨트롤을 렌더링 하는 클래스입니다.
1. 재정의 `OnElementChanged` 맞게 사용자 지정 컨트롤 및 쓰기 논리를 렌더링 하는 메서드. 이 메서드를 호출한 경우 해당 Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) 만들어집니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 사용자 지정 컨트롤을 렌더링 하 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용 하 여 사용자 지정 렌더러를 등록 하는 데 사용 됩니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하는 선택 사항입니다. 사용자 지정 렌더러를 등록 하지 않은 경우 셀의 기본 클래스에 대 한 기본 렌더러 사용 됩니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램에서 각 프로젝트의 책임을 보여 줍니다.

![](listview-images/solution-structure.png "NativeListView 사용자 지정 렌더러 프로젝트 책임")

합니다 `NativeListView` 사용자 지정 컨트롤에서 파생 되는 플랫폼별 렌더러 클래스에 의해 렌더링 되는 `ListViewRenderer` 각 플랫폼에 대 한 클래스입니다. 이 인해 각 `NativeListView` 다음 스크린샷과에서 같이 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 사용 하 여 렌더링 되는 사용자 지정 컨트롤:

![](listview-images/screenshots.png "각 플랫폼에서 NativeListView")

합니다 `ListViewRenderer` 노출 클래스는 `OnElementChanged` 해당 네이티브 컨트롤을 렌더링 하는 Xamarin.Forms 사용자 지정 컨트롤을 만들 때 호출 되는 메서드. 이 메서드는 `ElementChangedEventArgs` 매개 변수를 포함 하는 `OldElement` 및 `NewElement` 속성입니다. 이러한 속성은 Xamarin.Forms 요소를 나타냅니다는 렌더러 *되었습니다* 에 연결 및 Xamarin.Forms 요소는 렌더러 *는* 을 각각 연결 합니다. 샘플 응용 프로그램에서을 `OldElement` 속성이 `null` 및 `NewElement` 속성에 대 한 참조가 포함 됩니다는 `NativeListView` 인스턴스.

재정의 된 버전을 `OnElementChanged` 각 플랫폼별 렌더러 클래스에서 메서드는 네이티브 컨트롤 사용자 지정을 수행 하는 위치입니다. 플랫폼에서 사용 되는 네이티브 컨트롤에 대 한 형식화 된 참조를 통해 액세스할 수는 `Control` 속성입니다. 또한를 통해 렌더링 되는 Xamarin.Forms 컨트롤에 대 한 참조를 가져올 수 있습니다는 `Element` 속성입니다.

이벤트 처리기를 구독할 때는 주의 기울여야 합니다는 `OnElementChanged` 메서드를 다음 코드 예제에서 설명한 것 처럼:

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

네이티브 컨트롤 구성만 해야 하 고 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결 될 때 이벤트 처리기를 구독 합니다. 마찬가지로, 된 모든 이벤트 처리기를 취소 해야 수신을 렌더러가 변경 내용에 연결 된 경우에 합니다. 이 접근 방식을 채택 메모리 누수의 영향을 받지 않으며는 사용자 지정 렌더러를 만드는 데 도움이 됩니다.

재정의 된 버전을 `OnElementPropertyChanged` 각 플랫폼별 렌더러 클래스에서 메서드는 Xamarin.Forms 사용자 지정 컨트롤에 바인딩 가능한 속성 변경 내용에 응답할 수 있는 곳입니다. 변경 되는 속성에 대 한 검사를 항상 수 있어야를 따라이 재정의 여러 번 호출할 수 있습니다.

으로 데코 레이트 된 각 사용자 지정 렌더러 클래스는 `ExportRenderer` Xamarin.Forms를 사용 하 여 렌더러를 등록 하는 특성입니다. 특성 매개 변수 두 개 – 렌더링 되 고 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름 및 사용자 지정 렌더러의 형식 이름입니다. `assembly` 접두사 특성에 특성을 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 각 플랫폼별 사용자 지정 렌더러 클래스의 구현을 설명합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>IOS에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

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

합니다 `UITableView` 컨트롤의 인스턴스를 만들어 구성 된를 `NativeiOSListViewSource` 클래스는 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결 된 합니다. 이 클래스는 데이터를 제공 합니다 `UITableView` 재정의 하 여 컨트롤을 `RowsInSection` 및 `GetCell` 메서드를를 `UITableViewSource` 클래스를 노출 하는 `Items` 표시할 데이터의 목록을 포함 하는 속성입니다. 클래스도 제공 합니다.는 `RowSelected` 를 호출 하는 메서드 재정의 `ItemSelected` 에서 제공 하는 이벤트를 `NativeListView` 사용자 지정 컨트롤입니다. 메서드에 대 한 자세한 내용은 재정의 참조 하세요 [서브클래싱 UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)합니다. `GetCell` 메서드가 반환을 `UITableCellView` 목록의 각 행에 대 한 데이터로 채워집니다 하 고 다음 코드 예제에 표시 됩니다.

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

이 메서드가 만드는 `NativeiOSListViewCell` 화면에 표시할 데이터의 각 행에 대 한 인스턴스. `NativeiOSCell` 인스턴스는 각 셀을 셀의 데이터 레이아웃을 정의 합니다. 셀 스크롤로 인해 화면에서 사라지면 셀 제공 됩니다 다시 사용할 수 있도록 합니다. 있는지만 확인 하 여 메모리가 낭비를 방지 하는이 `NativeiOSCell` 인스턴스 목록에 있는 데이터의 모든가 대신 화면에 표시 되는 데이터에 대 한 합니다. 셀 재사용에 대 한 자세한 내용은 참조 하세요. [셀 재사용](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)합니다. `GetCell` 메서드 또한 읽습니다.는 `ImageFilename` 존재 하 고 이미지를 읽습니다를 있는 그대로 저장에 제공 된 데이터의 각 행의 속성을 `UIImage` 인스턴스를 업데이트 하기 전에 `NativeiOSListViewCell` 에 대 한 데이터 (이름, 범주 및 이미지)를 사용 하 여 인스턴스 행입니다.

`NativeiOSListViewCell` 클래스는 각 셀에 대 한 레이아웃을 정의 하 고 다음 코드 예제에 표시 됩니다.

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

이 클래스는 셀의 내용 및 해당 레이아웃을 렌더링 하는 데 사용 하는 컨트롤을 정의 합니다. 합니다 `NativeiOSListViewCell` 생성자의 인스턴스를 만듭니다 `UILabel` 및 `UIImageView` 컨트롤 및 모양을 초기화 합니다. 이러한 컨트롤은 하는 데 사용 하 여 각 행의 데이터를 표시 합니다 `UpdateCell` 이 데이터를 설정 하는 데 사용 되는 메서드를 `UILabel` 및 `UIImageView` 인스턴스. 이 인스턴스의 위치가 설정 재정의 된 `LayoutSubviews` 셀 내에서 해당 좌표를 지정 하 여 메서드.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>사용자 지정 컨트롤의 속성 변경에 응답

경우는 `NativeListView.Items` 속성에 추가 되는 항목으로 인해 변경 또는 목록에서 제거, 사용자 지정 렌더러가 변경 내용을 표시 하 여 응답 해야 합니다. 재정의 하 여이 작업을 수행할 수 있습니다는 `OnElementPropertyChanged` 메서드를 다음 코드 예제에 표시 됩니다.

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

메서드의 새 인스턴스를 만듭니다는 `NativeiOSListViewSource` 데이터를 제공 하는 클래스를 `UITableView` 컨트롤을 하는 바인딩 가능한 제공 `NativeListView.Items` 속성이 변경 합니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

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

네이티브 `ListView` 컨트롤은 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결할 때 제공한 구성 됩니다. 인스턴스를 만들고 이렇게이 구성 하면 합니다 `NativeAndroidListViewAdapter` 네이티브에 데이터를 제공 하는 클래스 `ListView` 컨트롤을 처리 하는 이벤트 처리기를 등록 합니다 `ItemClick` 이벤트. 이 처리기는 호출을 합니다 `ItemSelected` 에서 제공 하는 이벤트를 `NativeListView` 사용자 지정 컨트롤입니다. `ItemClick` Xamarin.Forms 렌더러가 변경 내용에 연결 되어 있으면 이벤트에서 구독 취소 합니다.

`NativeAndroidListViewAdapter` 에서 파생 되는 `BaseAdapter` 클래스 및 노출은 `Items` 재정의 뿐만 아니라 표시 되는 데이터의 목록을 포함 하는 속성을 `Count`, `GetView`, `GetItemId`, 및 `this[int]` 메서드. 이러한 메서드 재정의 대 한 자세한 내용은 참조 하세요. [는 ListAdapter 구현](~/android/user-interface/layouts/list-view/populating.md)합니다. `GetView` 메서드는 데이터로 채워진, 각 행에 대 한 뷰를 반환 하 고 다음 코드 예제에 표시 됩니다.

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

`GetView` 메서드는 렌더링할 셀에 반환할으로 `View`, 목록에서 데이터의 각 행에 대 한 합니다. 만듭니다는 `View` 모양의 사용 하 여 화면에 표시할 데이터의 각 행에 대 한 인스턴스는 `View` 레이아웃 파일에 정의 되는 인스턴스. 셀 스크롤로 인해 화면에서 사라지면 셀 제공 됩니다 다시 사용할 수 있도록 합니다. 있는지만 확인 하 여 메모리가 낭비를 방지 하는이 `View` 인스턴스 목록에 있는 데이터의 모든가 대신 화면에 표시 되는 데이터에 대 한 합니다. 뷰 다시 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [행 보기 재사용](~/android/user-interface/layouts/list-view/populating.md)합니다.

`GetView` 채웁니다 메서드를 `View` 에 지정 된 파일 이름에서 이미지 데이터를 읽는 비롯 한 데이터를 사용 하 여 인스턴스를 `ImageFilename` 속성입니다.

네이티브 하 여 각 셀 dispayed의 레이아웃 `ListView` 에 정의 된 합니다 `NativeAndroidListViewCell.axml` 레이아웃 파일을 더하게 됩니다는 `LayoutInflater.Inflate` 메서드. 다음 코드 예제에서는 레이아웃 정을 보여 줍니다.

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

이 레이아웃 지정 두 개의 `TextView` 컨트롤 및 `ImageView` 컨트롤은 셀의 내용을 표시 하는 데 사용 됩니다. 두 `TextView` 내에서 세로 방향 컨트롤은는 `LinearLayout` 내에 포함 되는 모든 컨트롤을 사용 하 여 컨트롤을 `RelativeLayout`.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>사용자 지정 컨트롤의 속성 변경에 응답

경우는 `NativeListView.Items` 속성에 추가 되는 항목으로 인해 변경 또는 목록에서 제거, 사용자 지정 렌더러가 변경 내용을 표시 하 여 응답 해야 합니다. 재정의 하 여이 작업을 수행할 수 있습니다는 `OnElementPropertyChanged` 메서드를 다음 코드 예제에 표시 됩니다.

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

메서드의 새 인스턴스를 만듭니다는 `NativeAndroidListViewAdapter` 네이티브에 데이터를 제공 하는 클래스 `ListView` 컨트롤을 하는 바인딩 가능한 제공 `NativeListView.Items` 속성이 변경 합니다.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP의 사용자 지정 렌더러 만들기

다음 코드 예제에서는 UWP에 대 한 사용자 지정 렌더러를 보여 줍니다.

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

네이티브 `ListView` 컨트롤은 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결할 때 제공한 구성 됩니다. 이렇게이 구성 하면 설정을 어떻게 네이티브 `ListView` 컨트롤 컨트롤, 모양 및 각 셀의 내용을 정의 하 고 를처리하는이벤트처리기를등록하여표시된데이터를채우는선택된항목에응답합니다`SelectionChanged` 이벤트입니다. 이 처리기는 호출을 합니다 `ItemSelected` 에서 제공 하는 이벤트를 `NativeListView` 사용자 지정 컨트롤입니다. `SelectionChanged` Xamarin.Forms 렌더러가 변경 내용에 연결 되어 있으면 이벤트에서 구독 취소 합니다.

각 네이티브의 내용과 모양을 `ListView` 셀에 의해 정의 됩니다는 `DataTemplate` 라는 `ListViewItemTemplate`합니다. 이 `DataTemplate` 응용 프로그램 수준 리소스 사전에 저장 되 고 다음 코드 예제에 표시 됩니다.

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

`DataTemplate` 셀 레이아웃 및 모양을의 내용을 표시 하는 데 사용 하는 컨트롤을 지정 합니다. 두 개의 `TextBlock` 컨트롤 및 `Image` 컨트롤은 데이터 바인딩을 통해 셀의 내용을 표시 하는 데 사용 됩니다. 또한의 인스턴스를 `ConcatImageExtensionConverter` 연결 하는 데 사용 되는 `.jpg` 파일 각 이미지 파일 이름 확장명입니다. 이렇게 하면 합니다 `Image` 컨트롤에서 로드 하 고 때 이미지를 렌더링할 수 `Source` 속성을 설정 합니다.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>사용자 지정 컨트롤의 속성 변경에 응답

경우는 `NativeListView.Items` 속성에 추가 되는 항목으로 인해 변경 또는 목록에서 제거, 사용자 지정 렌더러가 변경 내용을 표시 하 여 응답 해야 합니다. 재정의 하 여이 작업을 수행할 수 있습니다는 `OnElementPropertyChanged` 메서드를 다음 코드 예제에 표시 됩니다.

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

메서드가 네이티브를 다시 채웁니다 `ListView` 컨트롤이 변경된 된 데이터를 사용 하 여 제공 하는 바인딩 가능한 `NativeListView.Items` 속성이 변경 합니다.

## <a name="summary"></a>요약

이 문서에서는 사용자 지정 렌더러를 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 캡슐화 하는 네이티브 목록 제어 성능 더욱 잘 제어할 수 있도록 만드는 방법을 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererListView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
