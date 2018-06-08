---
title: ListView에 사용자 지정
description: Xamarin.Forms ListView 세로 목록으로 데이터의 컬렉션을 표시 하는 뷰입니다. 이 문서를 만드는 사용자 지정 렌더러 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 캡슐화 하는 네이티브 목록 컨트롤 성능에 비해 더 많은 제어를 허용 하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 2FBCB8C8-4F32-45E7-954F-63AD29D5F1B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 69640c1cdea6d7dbe3ec82dacbc77991c7b28c99
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847402"
---
# <a name="customizing-a-listview"></a>ListView에 사용자 지정

_Xamarin.Forms ListView 세로 목록으로 데이터의 컬렉션을 표시 하는 뷰입니다. 이 문서를 만드는 사용자 지정 렌더러 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 캡슐화 하는 네이티브 목록 컨트롤 성능에 비해 더 많은 제어를 허용 하는 방법을 보여줍니다._

모든 Xamarin.Forms 보기에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다. 경우는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ios에서 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `ListViewRenderer` 네이티브 다시 인스턴스화하는 클래스를 인스턴스화할 `UITableView` 제어 합니다. Android 플랫폼의 `ListViewRenderer` 클래스 인스턴스화합니다 네이티브 `ListView` 제어 합니다. 에 플랫폼 UWP (유니버설 Windows)는 `ListViewRenderer` 클래스 인스턴스화합니다 네이티브 `ListView` 제어 합니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 [렌더러 기본 클래스와 기본 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램에서는 간의 관계는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 제어 및 구현 하는 해당 네이티브 컨트롤:

![](listview-images/listview-classes.png "ListView 컨트롤과 구현 네이티브 컨트롤 간의 관계")

렌더링 프로세스 활용 하기 위해 취할 수에 대 한 사용자 지정 렌더러를 만들어 플랫폼 관련 사용자 지정을 구현는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 각 플랫폼에 있습니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_ListView_Control) Xamarin.Forms 사용자 지정 컨트롤입니다.
1. [사용할](#Consuming_the_Custom_Control) Xamarin.Forms에서 사용자 지정 컨트롤입니다.
1. [만들](#Creating_the_Custom_Renderer_on_each_Platform) 각 플랫폼에서 컨트롤에 대 한 사용자 지정 렌더러 합니다.

각 항목 이제 살펴봅니다 차례로 구현 하는 `NativeListView` 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃의 이점을 활용 하는 렌더러 합니다. 이 시나리오는 목록을 포함 하는 기존 네이티브 앱 및 다시 사용 될 수 있는 셀 코드를 이식할 때 유용 합니다. 또한 데이터 가상화 등 성능에 영향을 줄 수 있는 목록 컨트롤 기능 세부 사용자 지정할 수 있습니다.

<a name="Creating_the_Custom_ListView_Control" />

## <a name="creating-the-custom-listview-control"></a>사용자 지정 ListView 컨트롤 만들기

사용자 지정 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 서브클래싱하 여 컨트롤을 만들 수 있습니다는 `ListView` 다음 코드 예제와 같이 클래스:

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

`NativeListView` .NET 표준 라이브러리 프로젝트에서 생성 하 고 사용자 지정 컨트롤에 대 한 API를 정의 합니다. 이 컨트롤을 노출 한 `Items` 채우는 데 사용 되는 속성의 `ListView` 데이터 및 수 있는 데이터에 대 한 바인딩된 표시 목적으로 합니다. 또한 노출 된 `ItemSelected` 플랫폼 특정 네이티브 목록 컨트롤에서 항목을 선택할 때마다 발생 하는 이벤트입니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>사용자 지정 컨트롤 사용

`NativeListView` 사용자 지정 컨트롤 수 Xaml에서 참조할 수.NET 표준 라이브러리 프로젝트에서 해당 위치에 대 한 네임 스페이스를 선언 하 고 컨트롤에 네임 스페이스 접두사를 사용 합니다. 다음 코드 예제는 방법을 `NativeListView` XAML 페이지에서 사용자 지정 컨트롤을 사용할 수 있습니다.

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

`local` 원하는 네임 스페이스 접두사를 이름을 지정할 수 있습니다. 그러나는 `clr-namespace` 및 `assembly` 값에는 사용자 지정 컨트롤의 세부 정보과 일치 해야 합니다. 네임 스페이스 선언 되 면 사용자 지정 컨트롤을 참조 하는 접두사가 사용 됩니다.

다음 코드 예제는 방법을 `NativeListView` C# 페이지 사용자 지정 컨트롤을 사용할 수 있습니다.

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

`NativeListView` 사용자 지정 컨트롤 플랫폼 특정 사용자 지정 렌더러를 사용 하 여 통해 되는 데이터의 목록을 표시 하는 `Items` 속성입니다. 목록에서 각 행에는 이름, 범주 및 이미지 파일 이름에는 데이터의 세 가지 항목이 있습니다. 목록에서 각 행의 레이아웃 플랫폼 특정 사용자 지정 렌더러를 통해 정의 됩니다.

> [!NOTE]
> 때문에 `NativeListView` 사용자 지정 컨트롤 호스팅해서는 안 스크롤할 수 있는 레이아웃 컨트롤에서와 같은, 스크롤 기능을 포함 하는 플랫폼 특정 목록 컨트롤을 사용 하 여 사용자 지정 컨트롤이 렌더링 되는 [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)합니다.

사용자 지정 렌더러는 이제 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 만드는 각 응용 프로그램 프로젝트에 추가할 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든는 `ListViewRenderer` 사용자 지정 컨트롤을 렌더링 하는 클래스입니다.
1. 재정의 `OnElementChanged` 사용자 지정 하는 사용자 지정 권한 및 쓰기 논리를 렌더링 하는 메서드입니다. 이 메서드를 호출한 경우 해당 Xamarin.Forms [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 만들어집니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 사용자 지정 컨트롤을 렌더링 하 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용한 사용자 지정 렌더러를 등록 하려면 사용 합니다.

> [!NOTE]
> 이 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하는 선택 사항. 사용자 지정 렌더러 등록 되지 않은 셀의 기본 클래스에 대 한 기본 렌더러 사용 됩니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램의 각 프로젝트의 책임을 보여줍니다.

![](listview-images/solution-structure.png "NativeListView 사용자 지정 렌더러 프로젝트 책임")

`NativeListView` 모든에서 파생 되는 플랫폼 특정 렌더러 클래스에서 사용자 지정 컨트롤이 렌더링 되는 `ListViewRenderer` 각 플랫폼에 대 한 클래스입니다. 이 인해 각 `NativeListView` 다음 스크린샷에서 같이 플랫폼 특정 목록 컨트롤과 네이티브 셀 레이아웃, 렌더링 하는 사용자 지정 컨트롤:

![](listview-images/screenshots.png "각 플랫폼에서 NativeListView")

`ListViewRenderer` 클래스가 노출은 `OnElementChanged` 메서드를 해당 네이티브 컨트롤을 렌더링 하는 Xamarin.Forms 사용자 지정 컨트롤을 만들 때 호출 됩니다. 이 메서드는 `ElementChangedEventArgs` 포함 하는 매개 변수 `OldElement` 및 `NewElement` 속성입니다. Xamarin.Forms 요소를 나타내야 하는 이러한 속성 하는 렌더러 *되었습니다* 에, 연결 된 및 Xamarin.Forms 요소는 렌더러 *은* 에, 각각 연결 된 합니다. 샘플 응용 프로그램에서의 `OldElement` 속성이 `null` 및 `NewElement` 속성에 대 한 참조에 포함 됩니다는 `NativeListView` 인스턴스.

재정의 된 버전의 `OnElementChanged` 각 플랫폼별 렌더러 클래스에서 메서드는 네이티브 컨트롤 사용자 지정을 수행할 위치 합니다. 플랫폼에서 사용 되는 네이티브 컨트롤에 대 한 형식화 된 참조를 통해 액세스할 수는 `Control` 속성입니다. 또한 통해 렌더링 되는 Xamarin.Forms 컨트롤에 대 한 참조를 가져올 수 있습니다는 `Element` 속성입니다.

이벤트 처리기를 구독할 때는 주의 기울여야 합니다는 `OnElementChanged` 메서드를 다음 코드 예제에서와 같이:

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

기본 컨트롤 구성만 해야 하 고 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 될 때 이벤트 처리기를 구독 합니다. 마찬가지로, 구독 했던 모든 이벤트 처리기는 없어야에서 등록 된 렌더러 요소 변경 내용에 연결 된 경우에 합니다. 이 방법을 채택 메모리 누수에서 발생 하지 않는 사용자 지정 렌더러를 만드는 데 도움이 됩니다.

재정의 된 버전의 `OnElementPropertyChanged` 각 플랫폼별 렌더러 클래스의 메서드에 Xamarin.Forms 사용자 지정 컨트롤에 바인딩 가능한 속성 변경 내용에 응답 하는 위치입니다. 변경 된 속성에 대 한 확인을 항상을 설정 해야으로이 재정의 여러 번 호출할 수 있습니다.

각 사용자 지정 렌더러 클래스도 데코레이팅되 어는 `ExportRenderer` xamarin.forms 렌더러를 등록 하는 특성입니다. 속성에는 두 개의 매개 변수-렌더링 되 고 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름 및 사용자 지정 렌더러의 유형 이름을 사용 합니다. `assembly` 특성에 대 한 접두사 특성이 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 각 사용자 지정 렌더러 플랫폼 특정 클래스의 구현에 설명 합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>Ios 사용자 지정 렌더러 만들기

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

`UITableView` 컨트롤은의 인스턴스를 만들어 구성는 `NativeiOSListViewSource` 클래스 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우. 이 클래스는 데이터를 제공는 `UITableView` 재정의 하 여 제어는 `RowsInSection` 및 `GetCell` 에서 메서드는 `UITableViewSource` 클래스를 노출 하는 `Items` 데이터를 표시할 수의 목록을 포함 하는 속성입니다. 클래스 제공는 `RowSelected` 메서드 재정의 호출 하는 `ItemSelected` 에서 제공 하는 이벤트는 `NativeListView` 사용자 지정 컨트롤입니다. 메서드에 대 한 자세한 내용은 재정의 대 한 참조 [서브클래싱 UITableViewSource](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)합니다. `GetCell` 메서드가 반환 되는 `UITableCellView` 목록에서 각 행에 대 한 데이터로 채워지는 하 고 다음 코드 예제에 표시 됩니다.

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

이 메서드가 만드는 `NativeiOSListViewCell` 화면에 표시 되는 데이터의 각 행에 대 한 인스턴스. `NativeiOSCell` 인스턴스는 각 셀과 셀의 데이터의 레이아웃을 정의 합니다. 셀 스크롤 때문 화면에서 사라지면 셀 걸 수 다시 사용할 수 있습니다. 이렇게 하면 피할 수만 있는지 확인 하 여 메모리가 낭비 `NativeiOSCell` 인스턴스 목록에서 데이터를 모두가 아닌 화면에 표시 되는 데이터에 대 한 합니다. 셀 재사용 하는 방법에 대 한 자세한 내용은 참조 [셀 재사용](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)합니다. `GetCell` 읽는다 메서드는 `ImageFilename` 존재 하 고 이미지를 읽고 하로 저장에 제공 된 데이터의 각 행의 속성을 `UIImage` 인스턴스를 업데이트 하기 전에 `NativeiOSListViewCell` 인스턴스 (이름, 범주 및 이미지) 데이터에 대 한 행입니다.

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

이 클래스는 셀의 내용 및 레이아웃과 렌더링 하는 데 사용 되는 컨트롤을 정의 합니다. `NativeiOSListViewCell` 생성자의 인스턴스를 만드는 `UILabel` 및 `UIImageView` 컨트롤과 모양은 초기화 합니다. 이러한 컨트롤은와 각 행의 데이터를 표시 하는 데 사용 됩니다는 `UpdateCell` 에이 데이터를 설정 하는 데 사용 되는 메서드는 `UILabel` 및 `UIImageView` 인스턴스. 이러한 인스턴스 위치가 설정의 재정의 된 `LayoutSubviews` 셀의 좌표를 지정 하 여 메서드.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>사용자 지정 컨트롤의 속성 변경에 응답

경우는 `NativeListView.Items` 변경 내용을 표시 하 여 응답 해야 하는 사용자 지정 렌더러, 속성, 5d; 변경 항목에 추가 되 고 또는 목록에서 제거 합니다. 재정의 하 여이 작업을 수행할 수 있습니다는 `OnElementPropertyChanged` 메서드를 다음 코드 예제에 표시 됩니다.

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Source = new NativeiOSListViewSource (Element as NativeListView);
  }
}
```

메서드가의 새 인스턴스를 만듭니다.는 `NativeiOSListViewSource` 데이터를 제공 하는 클래스는 `UITableView` 을 제어 하는 바인딩 가능한 제공 `NativeListView.Items` 속성이 변경 합니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android 사용자 지정 렌더러 만들기

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

네이티브 `ListView` 컨트롤은 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우 구성 됩니다. 이 구성의 인스턴스를 만들고 포함 됩니다는 `NativeAndroidListViewAdapter` 는 네이티브 데이터를 제공 하는 클래스 `ListView` 컨트롤을 처리 하는 이벤트 처리기를 등록 하는 중는 `ItemClick` 이벤트입니다. 이 처리기는 호출에 `ItemSelected` 에서 제공 하는 이벤트는 `NativeListView` 사용자 지정 컨트롤입니다. `ItemClick` Xamarin.Forms 요소 렌더러는 변경 내용에 연결 된 경우 이벤트를 구독 취소 합니다.

`NativeAndroidListViewAdapter` 에서 파생 되는 `BaseAdapter` 클래스와 노출은 `Items` 데이터를 표시할 수의 목록을 포함 하는 속성으로 재정의 `Count`, `GetView`, `GetItemId`, 및 `this[int]` 메서드. 이러한 메서드 재정의 대 한 자세한 내용은 참조 [는 ListAdapter 구현](~/android/user-interface/layouts/list-view/populating.md)합니다. `GetView` 메서드 데이터로 채워진 각 행에 대 한 뷰를 반환 하 고 다음 코드 예제에 표시 됩니다.

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

`GetView` 메서드는 렌더링할 수를 셀로 `View`, 목록에는 데이터의 각 행에 대 한 합니다. 생성 한 `View` 의 모양에 화면에 표시 되는 데이터의 각 행에 대 한 인스턴스는 `View` 되 고 레이아웃 파일에 정의 된 인스턴스. 셀 스크롤 때문 화면에서 사라지면 셀 걸 수 다시 사용할 수 있습니다. 이렇게 하면 피할 수만 있는지 확인 하 여 메모리가 낭비 `View` 인스턴스 목록에서 데이터를 모두가 아닌 화면에 표시 되는 데이터에 대 한 합니다. 보기 다시 사용할 수 있도록 하는 방법에 대 한 자세한 내용은 참조 [행 보기 재사용할](~/android/user-interface/layouts/list-view/populating.md)합니다.

`GetView` 채웁니다 메서드는 `View` 에 지정 된 파일 이름에서 이미지 데이터 읽기 등의 데이터를 사용 하 여 인스턴스는 `ImageFilename` 속성입니다.

네이티브 하 여 각 셀 dispayed의 레이아웃 `ListView` 에 정의 된는 `NativeAndroidListViewCell.axml` 레이아웃 파일에서을 확장 하는 `LayoutInflater.Inflate` 메서드. 다음 코드 예제에서는 레이아웃 정을 보여 줍니다.

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

이 레이아웃 지정 두 `TextView` 컨트롤 및 `ImageView` 컨트롤은 셀의 내용을 표시 하는 데 사용 됩니다. 두 `TextView` 컨트롤은 세로 방향 내에서 한 `LinearLayout` 내에 포함 되 고 모든 컨트롤의 한 제어는 `RelativeLayout`합니다.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>사용자 지정 컨트롤의 속성 변경에 응답

경우는 `NativeListView.Items` 변경 내용을 표시 하 여 응답 해야 하는 사용자 지정 렌더러, 속성, 5d; 변경 항목에 추가 되 고 또는 목록에서 제거 합니다. 재정의 하 여이 작업을 수행할 수 있습니다는 `OnElementPropertyChanged` 메서드를 다음 코드 예제에 표시 됩니다.

```csharp
protected override void OnElementPropertyChanged (object sender, System.ComponentModel.PropertyChangedEventArgs e)
{
  base.OnElementPropertyChanged (sender, e);

  if (e.PropertyName == NativeListView.ItemsProperty.PropertyName) {
    Control.Adapter = new NativeAndroidListViewAdapter (_context as Android.App.Activity, Element as NativeListView);
  }
}
```

메서드가의 새 인스턴스를 만듭니다.는 `NativeAndroidListViewAdapter` 는 네이티브 데이터를 제공 하는 클래스 `ListView` 을 제어 하는 바인딩 가능한 제공 `NativeListView.Items` 속성이 변경 합니다.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP에 사용자 지정 렌더러 만들기

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

네이티브 `ListView` 컨트롤은 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우 구성 됩니다. 이 구성을 설정 해야 어떻게 네이티브 `ListView` 컨트롤은 컨트롤의 모양 및 각 셀의 내용을 정의 하 고는 를처리하는이벤트처리기를등록하는중에표시데이터를채우는항목을선택에응답합니다`SelectionChanged` 이벤트입니다. 이 처리기는 호출에 `ItemSelected` 에서 제공 하는 이벤트는 `NativeListView` 사용자 지정 컨트롤입니다. `SelectionChanged` Xamarin.Forms 요소 렌더러는 변경 내용에 연결 된 경우 이벤트를 구독 취소 합니다.

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

`DataTemplate` 는 셀의 레이아웃 및 모양을의 내용을 표시 하는 데 사용 하는 컨트롤을 지정 합니다. 두 개의 `TextBlock` 컨트롤 및 `Image` 컨트롤은 데이터 바인딩을 통해 셀의 내용을 표시 하는 데 사용 됩니다. 또한의 인스턴스는 `ConcatImageExtensionConverter` 을 연결 하는 데 사용 되는 `.jpg` 파일을 각 이미지 파일 이름 확장명입니다. 이렇게 하면는 `Image` 컨트롤에서 로드 하 고 되었을 때 이미지를 렌더링할 수 `Source` 속성을 설정 합니다.

#### <a name="responding-to-a-property-change-on-the-custom-control"></a>사용자 지정 컨트롤의 속성 변경에 응답

경우는 `NativeListView.Items` 변경 내용을 표시 하 여 응답 해야 하는 사용자 지정 렌더러, 속성, 5d; 변경 항목에 추가 되 고 또는 목록에서 제거 합니다. 재정의 하 여이 작업을 수행할 수 있습니다는 `OnElementPropertyChanged` 메서드를 다음 코드 예제에 표시 됩니다.

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

메서드는 네이티브를 다시 채웁니다 `ListView` 컨트롤 변경 된 데이터를 제공 하는 바인딩 가능한 `NativeListView.Items` 속성이 변경 합니다.

## <a name="summary"></a>요약

이 문서를 만드는 사용자 지정 렌더러 플랫폼별 목록 컨트롤 및 네이티브 셀 레이아웃을 캡슐화 하는 네이티브 목록 컨트롤 성능에 비해 더 많은 제어를 허용 하는 방법을 보여 주었습니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererListView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/listview/)
