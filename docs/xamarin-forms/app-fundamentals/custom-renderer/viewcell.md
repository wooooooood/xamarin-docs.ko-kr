---
title: ViewCell 사용자 지정
description: Xamarin.Forms ViewCell은 ListView 또는 개발자 정의 보기를 포함 하는 TableView에 추가할 수 있는 셀입니다. 이 문서에서는 Xamarin.Forms ListView 컨트롤 내에서 호스팅되는 ViewCell에 대 한 사용자 지정 렌더러를 만드는 방법을 보여줍니다. ListView 스크롤 하는 동안 반복 해 서 호출 Xamarin.Forms 레이아웃 계산 되 고에서 중지 합니다.
ms.prod: xamarin
ms.assetid: 61F378C9-6DEF-436B-ACC3-2324B25D404E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 3e5f4f2d4c4025cce21026cc611af650616e69e2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-a-viewcell"></a>ViewCell 사용자 지정

_Xamarin.Forms ViewCell은 ListView 또는 개발자 정의 보기를 포함 하는 TableView에 추가할 수 있는 셀입니다. 이 문서에서는 Xamarin.Forms ListView 컨트롤 내에서 호스팅되는 ViewCell에 대 한 사용자 지정 렌더러를 만드는 방법을 보여줍니다. ListView 스크롤 하는 동안 반복 해 서 호출 Xamarin.Forms 레이아웃 계산 되 고에서 중지 합니다._

모든 Xamarin.Forms 셀에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다. 경우는 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) ios에서 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `ViewCellRenderer` 네이티브 다시 인스턴스화하는 클래스를 인스턴스화할 `UITableViewCell` 제어 합니다. Android 플랫폼의 `ViewCellRenderer` 클래스 인스턴스화합니다 네이티브 `View` 제어 합니다. Windows Phone 및 유니버설 Windows 플랫폼 (UWP)에 `ViewCellRenderer` 클래스 인스턴스화합니다 네이티브 `DataTemplate`합니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 [렌더러 기본 클래스와 기본 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램에서는 간의 관계는 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 및 구현 하는 해당 네이티브 컨트롤:

![](viewcell-images/viewcell-classes.png "ViewCell 컨트롤과 구현 네이티브 컨트롤 간의 관계")

렌더링 프로세스 활용 하기 위해 취할 수에 대 한 사용자 지정 렌더러를 만들어 플랫폼 관련 사용자 지정을 구현는 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 각 플랫폼에 있습니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_Cell) Xamarin.Forms 사용자 지정 셀입니다.
1. [사용할](#Consuming_the_Custom_Cell) Xamarin.Forms에서 사용자 지정 셀입니다.
1. [만들](#Creating_the_Custom_Renderer_on_each_Platform) 각 플랫폼에 대 한 셀에 대 한 사용자 지정 렌더러 합니다.

각 항목 이제 살펴봅니다 차례로 구현 하는 `NativeCell` 는 Xamarin.Forms 내에서 호스팅되는 각 셀에 대 한 플랫폼별 레이아웃의 이점을 활용 하는 렌더러 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 제어 합니다. 이 인해 중지 하는 동안 반복적으로 호출 하지 못하도록 Xamarin.Forms 레이아웃 계산 `ListView` 스크롤입니다.

<a name="Creating_the_Custom_Cell" />

## <a name="creating-the-custom-cell"></a>사용자 지정 셀 만들기

사용자 지정 셀 컨트롤을 서브클래싱하 여 만들 수 있습니다는 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 다음 코드 예제와 같이 클래스:

```csharp
public class NativeCell : ViewCell
{
  public static readonly BindableProperty NameProperty =
    BindableProperty.Create ("Name", typeof(string), typeof(NativeCell), "");

  public string Name {
    get { return (string)GetValue (NameProperty); }
    set { SetValue (NameProperty, value); }
  }

  public static readonly BindableProperty CategoryProperty =
    BindableProperty.Create ("Category", typeof(string), typeof(NativeCell), "");

  public string Category {
    get { return (string)GetValue (CategoryProperty); }
    set { SetValue (CategoryProperty, value); }
  }

  public static readonly BindableProperty ImageFilenameProperty =
    BindableProperty.Create ("ImageFilename", typeof(string), typeof(NativeCell), "");

  public string ImageFilename {
    get { return (string)GetValue (ImageFilenameProperty); }
    set { SetValue (ImageFilenameProperty, value); }
  }
}
```
`NativeCell` 클래스 이식 가능한 클래스 라이브러리 PCL 프로젝트에 만들어지고 사용자 지정 셀에 대 한 API를 정의 합니다. 사용자 지정 셀 노출 `Name`, `Category`, 및 `ImageFilename` 데이터 바인딩을 통해 표시 될 수 있는 속성입니다. 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩 기본](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)을 참조하세요.

<a name="Consuming_the_Custom_Cell" />

## <a name="consuming-the-custom-cell"></a>사용자 지정 셀 사용

`NativeCell` 사용자 지정 셀 수 Xaml에서 참조할 수는 PCL 프로젝트에 해당 위치에 대 한 네임 스페이스를 선언 하 고 사용자 지정 셀 요소에 네임 스페이스 접두사를 사용 하 여 합니다. 다음 코드 예제는 방법을 `NativeCell` 사용자 지정 셀 XAML 페이지 에서도 사용할 수 있습니다.

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Xamarin.Forms native cell" HorizontalTextAlignment="Center" />
            <ListView x:Name="listView" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <local:NativeCell Name="{Binding Name}" Category="{Binding Category}" ImageFilename="{Binding ImageFilename}" />
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local` 원하는 네임 스페이스 접두사를 이름을 지정할 수 있습니다. 그러나는 `clr-namespace` 및 `assembly` 값에는 사용자 지정 컨트롤의 세부 정보과 일치 해야 합니다. 네임 스페이스 선언 되 면 사용자 지정 셀을 참조 하는 접두사가 사용 됩니다.

다음 코드 예제는 방법을 `NativeCell` 사용자 지정 셀 C# 페이지 에서도 사용할 수 있습니다.

```csharp
public class NativeCellPageCS : ContentPage
{
    ListView listView;

    public NativeCellPageCS()
    {
        listView = new ListView(ListViewCachingStrategy.RecycleElement)
        {
            ItemsSource = DataSource.GetList(),
            ItemTemplate = new DataTemplate(() =>
            {
                var nativeCell = new NativeCell();
                nativeCell.SetBinding(NativeCell.NameProperty, "Name");
                nativeCell.SetBinding(NativeCell.CategoryProperty, "Category");
                nativeCell.SetBinding(NativeCell.ImageFilenameProperty, "ImageFilename");

                return nativeCell;
            })
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

        Content = new StackLayout
        {
            Children = {
                new Label { Text = "Xamarin.Forms native cell", HorizontalTextAlignment = TextAlignment.Center },
                listView
            }
        };
        listView.ItemSelected += OnItemSelected;
    }
    ...
}
```

Xamarin.Forms는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 컨트롤을 사용을 통해 되는 데이터의 목록을 표시 하는 [ `ItemSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemsSource/) 속성입니다. [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 최소화 하려고 캐싱 전략은 `ListView` 목록 셀을 재활용 하 여 메모리 사용 공간, 실행 속도입니다. 자세한 내용은 참조 [캐싱 전략](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy)합니다.

목록에서 각 행에는 이름, 범주 및 이미지 파일 이름에는 데이터의 세 가지 항목이 있습니다. 목록에서 각 행의 레이아웃으로 정의 됩니다는 `DataTemplate` 통해 참조 되는 [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%601.ItemTemplate/) 바인딩 가능한 속성입니다. `DataTemplate` 목록에는 데이터의 각 행 되도록 정의 `NativeCell` 표시 하는 해당 `Name`, `Category`, 및 `ImageFilename` 데이터 바인딩을 통해 속성입니다. 에 대 한 자세한 내용은 `ListView` 제어, 참조 [ListView](~/xamarin-forms/user-interface/listview/index.md)합니다.

사용자 지정 렌더러는 이제 각 셀에 대 한 플랫폼별 레이아웃을 사용자 지정할 각 응용 프로그램 프로젝트에 추가할 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든는 `ViewCellRenderer` 사용자 지정 셀에 렌더링 하는 클래스입니다.
1. 사용자 지정 하는 논리를 작성 및 사용자 지정 셀을 렌더링 하는 플랫폼 특정 메서드를 재정의 합니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 사용자 지정 셀을 렌더링에 사용 되는 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용한 사용자 지정 렌더러를 등록 하려면 사용 합니다.

> [!NOTE]
> 대부분의 Xamarin.Forms 요소에 대 한 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하기 선택 사항입니다. 사용자 지정 렌더러 등록 되지 않은 경우 컨트롤의 기본 클래스에 대 한 기본 렌더러 사용 됩니다. 그러나 사용자 지정 렌더러 필요한 각 플랫폼 프로젝트에 렌더링 하는 경우는 [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 요소입니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램의 각 프로젝트의 책임을 보여줍니다.

![](viewcell-images/solution-structure.png "NativeCell 사용자 지정 렌더러 프로젝트 책임")

`NativeCell` 사용자 지정 셀이 모든에서 파생 되는 플랫폼 특정 렌더러 클래스에서 렌더링 되는 `ViewCellRenderer` 각 플랫폼에 대 한 클래스입니다. 이 인해 각 `NativeCell` 다음 스크린샷에서 같이 플랫폼 특정 레이아웃으로 렌더링 되는 사용자 지정 셀:

![](viewcell-images/screenshots.png "각 플랫폼에서 NativeCell")

`ViewCellRenderer` 클래스 사용자 지정 셀을 렌더링 하기 위한 플랫폼 관련 메서드를 노출 합니다. 이는 `GetCell` iOS 플랫폼에서 메서드는 `GetCellCore` Android 플랫폼, 메서드 및 `GetTemplate` Windows Phone 플랫폼에서 메서드.

각 사용자 지정 렌더러 클래스도 데코레이팅되 어는 `ExportRenderer` xamarin.forms 렌더러를 등록 하는 특성입니다. 속성에는 두 개의 매개 변수-렌더링 되 고 Xamarin.Forms 셀의 형식 이름 및 사용자 지정 렌더러의 유형 이름을 사용 합니다. `assembly` 특성에 대 한 접두사 특성이 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 각 사용자 지정 렌더러 플랫폼 특정 클래스의 구현에 설명 합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>Ios 사용자 지정 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeiOSCellRenderer))]
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        NativeiOSCell cell;

        public override UITableViewCell GetCell(Cell item, UITableViewCell reusableCell, UITableView tv)
        {
            var nativeCell = (NativeCell)item;

            cell = reusableCell as NativeiOSCell;
            if (cell == null)
                cell = new NativeiOSCell(item.GetType().FullName, nativeCell);
            else
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;
            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCell` 메서드를 표시 하기 위해 각 셀을 빌드하기 위해 호출 됩니다. 각 셀에는 한 `NativeiOSCell` 셀 및 데이터의 레이아웃을 정의 하는 인스턴스입니다. 작업은 `GetCell` 의존 하는 메서드는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 캐싱 전략:

- 경우는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 캐싱 전략은 [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCell` 각 셀에 대해 메서드가 호출 됩니다. A `NativeiOSCell` 각각에 대해 인스턴스를 만듭니다. `NativeCell` 화면에 처음 표시 되는 인스턴스. 사용자에 의해 스크롤될 때는 `ListView`, `NativeiOSCell` 인스턴스를 다시 사용 됩니다. IOS 셀 다시 사용 하는 방법에 대 한 자세한 내용은 참조 [셀 재사용](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)합니다.

  > [!NOTE]
  > 일부 셀 다시 사용 하 여이 사용자 지정 렌더러 코드에서 수행 하는 경우에는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 셀을 유지 하도록 설정 합니다.

  각각 표시 되는 데이터 `NativeiOSCell` 인스턴스를 새로 만들거나 다시 사용을 업데이트할지 각각의 데이터로 `NativeCell` 하 여 인스턴스에 `UpdateCell` 메서드.

  > [!NOTE]
  > `OnNativeCellPropertyChanged` 메서드 안 됩니다 될 때 호출 되는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 유지 셀으로 설정 된 캐싱 전략입니다.

- 경우는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 캐싱 전략은 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCell` 메서드가 처음 화면에 표시 되는 각 셀에 대해 호출 됩니다. A `NativeiOSCell` 각각에 대해 인스턴스를 만듭니다. `NativeCell` 화면에 처음 표시 되는 인스턴스. 각각 표시 되는 데이터 `NativeiOSCell` 인스턴스가 데이터를 업데이트는 `NativeCell` 하 여 인스턴스에 `UpdateCell` 메서드. 그러나는 `GetCell` 사용자 스크롤합니다도 메서드를 호출할 수 없습니다는 `ListView`합니다. 대신,는 `NativeiOSCell` 인스턴스를 다시 사용 됩니다. `PropertyChanged` 이벤트가 발생 수는 `NativeCell` 해당 데이터가 변경 될 때 인스턴스 및 `OnNativeCellPropertyChanged` 이벤트 처리기에서 다시 사용 되는 각 데이터를 업데이트 `NativeiOSCell` 인스턴스.

다음 코드 예제는 `OnNativeCellPropertyChanged` 가 호출 메서드는 `PropertyChanged` 이벤트가 발생 합니다.

```csharp
namespace CustomRenderer.iOS
{
    public class NativeiOSCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingLabel.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingLabel.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.CellImageView.Image = cell.GetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

이 메서드가 업데이트 하 여 표시 되는 데이터를 다시 사용 `NativeiOSCell` 인스턴스. 메서드가 여러 번 호출할 수 있습니다 때 변경 된 속성에 대 한 검사는 수행 됩니다.

`NativeiOSCell` 클래스는 각 셀에 대 한 레이아웃을 정의 하 고 다음 코드 예제에 표시 됩니다.

```csharp
internal class NativeiOSCell : UITableViewCell, INativeElementView
{
  public UILabel HeadingLabel { get; set; }
  public UILabel SubheadingLabel { get; set; }
  public UIImageView CellImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeiOSCell(string cellId, NativeCell cell) : base(UITableViewCellStyle.Default, cellId)
  {
    NativeCell = cell;

    SelectionStyle = UITableViewCellSelectionStyle.Gray;
    ContentView.BackgroundColor = UIColor.FromRGB(255, 255, 224);
    CellImageView = new UIImageView();

    HeadingLabel = new UILabel()
    {
      Font = UIFont.FromName("Cochin-BoldItalic", 22f),
      TextColor = UIColor.FromRGB(127, 51, 0),
      BackgroundColor = UIColor.Clear
    };

    SubheadingLabel = new UILabel()
    {
      Font = UIFont.FromName("AmericanTypewriter", 12f),
      TextColor = UIColor.FromRGB(38, 127, 0),
      TextAlignment = UITextAlignment.Center,
      BackgroundColor = UIColor.Clear
    };

    ContentView.Add(HeadingLabel);
    ContentView.Add(SubheadingLabel);
    ContentView.Add(CellImageView);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingLabel.Text = cell.Name;
    SubheadingLabel.Text = cell.Category;
    CellImageView.Image = GetImage(cell.ImageFilename);
  }

  public UIImage GetImage(string filename)
  {
    return (!string.IsNullOrWhiteSpace(filename)) ? UIImage.FromFile("Images/" + filename + ".jpg") : null;
  }

  public override void LayoutSubviews()
  {
    base.LayoutSubviews();

    HeadingLabel.Frame = new CGRect(5, 4, ContentView.Bounds.Width - 63, 25);
    SubheadingLabel.Frame = new CGRect(100, 18, 100, 20);
    CellImageView.Frame = new CGRect(ContentView.Bounds.Width - 63, 5, 33, 33);
  }
}
```

이 클래스는 셀의 내용 및 레이아웃과 렌더링 하는 데 사용 되는 컨트롤을 정의 합니다. 클래스가 구현 하는 [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) 때을 지정 해야 하는 인터페이스는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 사용 하 여는 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 캐싱 전략입니다. 이 인터페이스는 클래스를 구현 해야 지정는 [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) 속성을 재활용된 셀에 대 한 사용자 지정 셀 데이터를 반환 해야 합니다.

`NativeiOSCell` 생성자의 모양을 초기화는 `HeadingLabel`, `SubheadingLabel`, 및 `CellImageView` 속성입니다. 이러한 속성에 저장 된 데이터를 표시 하는 데 사용 됩니다는 `NativeCell` 인스턴스와 `UpdateCell` 메서드가 각 속성의 값을 설정 하기 위해 호출 되 고 있습니다. 또한,는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 사용 하 여는 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 캐싱 전략을 표시 하는 데이터는 `HeadingLabel`, `SubheadingLabel`, 및 `CellImageView` 속성 일 수 있습니다 에 의해 업데이트는 `OnNativeCellPropertyChanged` 에서 사용자 지정 렌더러 메서드.

셀 레이아웃에 의해 수행 됩니다는 `LayoutSubviews` 재정의의 좌표를 설정 하는 `HeadingLabel`, `SubheadingLabel`, 및 `CellImageView` 셀 내에서.

### <a name="creating-the-custom-renderer-on-android"></a>Android 사용자 지정 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeAndroidCellRenderer))]
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        NativeAndroidCell cell;

        protected override Android.Views.View GetCellCore(Cell item, Android.Views.View convertView, ViewGroup parent, Context context)
        {
            var nativeCell = (NativeCell)item;
            Console.WriteLine("\t\t" + nativeCell.Name);

            cell = convertView as NativeAndroidCell;
            if (cell == null)
            {
                cell = new NativeAndroidCell(context, nativeCell);
            }
            else
            {
                cell.NativeCell.PropertyChanged -= OnNativeCellPropertyChanged;
            }

            nativeCell.PropertyChanged += OnNativeCellPropertyChanged;

            cell.UpdateCell(nativeCell);
            return cell;
        }
        ...
    }
}
```

`GetCellCore` 메서드를 표시 하기 위해 각 셀을 빌드하기 위해 호출 됩니다. 각 셀에는 한 `NativeAndroidCell` 셀 및 데이터의 레이아웃을 정의 하는 인스턴스입니다. 작업은 `GetCellCore` 의존 하는 메서드는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 캐싱 전략:

- 경우는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 캐싱 전략은 [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/), `GetCellCore` 각 셀에 대해 메서드가 호출 됩니다. A `NativeAndroidCell` 각각에 대해 만들 수 `NativeCell` 화면에 처음 표시 되는 인스턴스. 사용자에 의해 스크롤될 때는 `ListView`, `NativeAndroidCell` 인스턴스를 다시 사용 됩니다. Android 셀이 다시 사용 하는 방법에 대 한 자세한 내용은 참조 [행 보기 재사용할](~/android/user-interface/layouts/list-view/populating.md)합니다.

  > [!NOTE]
  > 참고 일부 셀 다시 사용 하 여이 사용자 지정 렌더러 코드 수행 되는 경우에는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 셀을 유지 하도록 설정 합니다.

  각각 표시 되는 데이터 `NativeAndroidCell` 인스턴스를 새로 만들거나 다시 사용을 업데이트할지 각각의 데이터로 `NativeCell` 하 여 인스턴스에 `UpdateCell` 메서드.

  > [!NOTE]
  > 하지만 `OnNativeCellPropertyChanged` 메서드 됩니다 될 때 호출 되는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 은 셀을 유지 하려면 설정, 업데이트 되지 것입니다는 `NativeAndroidCell` 속성 값.

- 경우는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 캐싱 전략은 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/), `GetCellCore` 메서드가 처음 화면에 표시 되는 각 셀에 대해 호출 됩니다. A `NativeAndroidCell` 각각에 대해 인스턴스를 만듭니다. `NativeCell` 화면에 처음 표시 되는 인스턴스. 각각 표시 되는 데이터 `NativeAndroidCell` 인스턴스가 데이터를 업데이트는 `NativeCell` 하 여 인스턴스에 `UpdateCell` 메서드. 그러나는 `GetCellCore` 사용자 스크롤합니다도 메서드를 호출할 수 없습니다는 `ListView`합니다. 대신,는 `NativeAndroidCell` 인스턴스를 다시 사용 됩니다.  `PropertyChanged` 이벤트가 발생 수는 `NativeCell` 해당 데이터가 변경 될 때 인스턴스 및 `OnNativeCellPropertyChanged` 이벤트 처리기에서 다시 사용 되는 각 데이터를 업데이트 `NativeAndroidCell` 인스턴스.

다음 코드 예제는 `OnNativeCellPropertyChanged` 가 호출 메서드는 `PropertyChanged` 이벤트가 발생 합니다.

```csharp
namespace CustomRenderer.Droid
{
    public class NativeAndroidCellRenderer : ViewCellRenderer
    {
        ...

        void OnNativeCellPropertyChanged(object sender, PropertyChangedEventArgs e)
        {
            var nativeCell = (NativeCell)sender;
            if (e.PropertyName == NativeCell.NameProperty.PropertyName)
            {
                cell.HeadingTextView.Text = nativeCell.Name;
            }
            else if (e.PropertyName == NativeCell.CategoryProperty.PropertyName)
            {
                cell.SubheadingTextView.Text = nativeCell.Category;
            }
            else if (e.PropertyName == NativeCell.ImageFilenameProperty.PropertyName)
            {
                cell.SetImage(nativeCell.ImageFilename);
            }
        }
    }
}
```

이 메서드가 업데이트 하 여 표시 되는 데이터를 다시 사용 `NativeAndroidCell` 인스턴스. 메서드가 여러 번 호출할 수 있습니다 때 변경 된 속성에 대 한 검사는 수행 됩니다.

`NativeAndroidCell` 클래스는 각 셀에 대 한 레이아웃을 정의 하 고 다음 코드 예제에 표시 됩니다.

```csharp
internal class NativeAndroidCell : LinearLayout, INativeElementView
{
  public TextView HeadingTextView { get; set; }
  public TextView SubheadingTextView { get; set; }
  public ImageView ImageView { get; set; }

  public NativeCell NativeCell { get; private set; }
  public Element Element => NativeCell;

  public NativeAndroidCell(Context context, NativeCell cell) : base(context)
  {
    NativeCell = cell;

    var view = (context as Activity).LayoutInflater.Inflate(Resource.Layout.NativeAndroidCell, null);
    HeadingTextView = view.FindViewById<TextView>(Resource.Id.HeadingText);
    SubheadingTextView = view.FindViewById<TextView>(Resource.Id.SubheadingText);
    ImageView = view.FindViewById<ImageView>(Resource.Id.Image);

    AddView(view);
  }

  public void UpdateCell(NativeCell cell)
  {
    HeadingTextView.Text = cell.Name;
    SubheadingTextView.Text = cell.Category;

    // Dispose of the old image
    if (ImageView.Drawable != null)
    {
      using (var image = ImageView.Drawable as BitmapDrawable)
      {
        if (image != null)
        {
          if (image.Bitmap != null)
          {
            image.Bitmap.Dispose();
          }
        }
      }
    }

    SetImage(cell.ImageFilename);
  }

  public void SetImage(string filename)
  {
    if (!string.IsNullOrWhiteSpace(filename))
    {
      // Display new image
      Context.Resources.GetBitmapAsync(filename).ContinueWith((t) =>
      {
        var bitmap = t.Result;
        if (bitmap != null)
        {
          ImageView.SetImageBitmap(bitmap);
          bitmap.Dispose();
        }
      }, TaskScheduler.FromCurrentSynchronizationContext());
    }
    else
    {
      // Clear the image
      ImageView.SetImageBitmap(null);
    }
  }
}
```

이 클래스는 셀의 내용 및 레이아웃과 렌더링 하는 데 사용 되는 컨트롤을 정의 합니다. 클래스가 구현 하는 [ `INativeElementView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INativeElementView/) 때을 지정 해야 하는 인터페이스는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 사용 하 여는 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 캐싱 전략입니다. 이 인터페이스는 클래스를 구현 해야 지정는 [ `Element` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INativeElementView.Element/) 속성을 재활용된 셀에 대 한 사용자 지정 셀 데이터를 반환 해야 합니다.

`NativeAndroidCell` 생성자 팽창는 `NativeAndroidCell` 레이아웃을 초기화 합니다는 `HeadingTextView`, `SubheadingTextView`, 및 `ImageView` 높여서 레이아웃의 컨트롤에는 속성입니다. 이러한 속성에 저장 된 데이터를 표시 하는 데 사용 됩니다는 `NativeCell` 인스턴스와 `UpdateCell` 메서드가 각 속성의 값을 설정 하기 위해 호출 되 고 있습니다. 또한,는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 사용 하 여는 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) 캐싱 전략을 표시 하는 데이터는 `HeadingTextView`, `SubheadingTextView`, 및 `ImageView` 속성 일 수 있습니다 에 의해 업데이트는 `OnNativeCellPropertyChanged` 에서 사용자 지정 렌더러 메서드.

다음 코드 예제에 대 한 레이아웃 정의 보여 줍니다.는 `NativeAndroidCell.axml` 레이아웃 파일:

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
            android:id="@+id/HeadingText"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FF7F3300"
            android:textSize="20dip"
            android:textStyle="italic" />
        <TextView
            android:id="@+id/SubheadingText"
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

### <a name="creating-the-custom-renderer-on-windows-phone-and-uwp"></a>Windows Phone에서 사용자 지정 렌더러 만들기 및 UWP

다음 코드 예제에서는 UWP 및 Windows Phone 대 한 사용자 지정 렌더러를 보여 줍니다.

```csharp
[assembly: ExportRenderer(typeof(NativeCell), typeof(NativeWinPhoneCellRenderer))]
namespace CustomRenderer.WinPhone81
{
    public class NativeWinPhoneCellRenderer : ViewCellRenderer
    {
        public override Windows.UI.Xaml.DataTemplate GetTemplate(Cell cell)
        {
            return App.Current.Resources["ListViewItemTemplate"] as Windows.UI.Xaml.DataTemplate;
        }
    }
}
```

`GetTemplate` 메서드 목록에는 데이터의 각 행에 대해 렌더링 해야 하는 셀을 반환 하기 위해 호출 됩니다. 생성 한 `DataTemplate` 각각에 대해 `NativeCell` 와 화면에 표시 되는 인스턴스는 `DataTemplate` 모양 및 셀의 내용을 정의 합니다.

`DataTemplate` 응용 프로그램 수준 리소스 사전에 저장 되 고 다음 코드 예제에 표시 됩니다.

```xaml
<DataTemplate x:Key="ListViewItemTemplate">
    <Grid Background="LightYellow">
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

## <a name="summary"></a>요약

이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 제시 합니다.는 [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 하는 Xamarin.Forms 내에서 호스팅되는 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 제어 합니다. 이 인해 중지 하는 동안 반복적으로 호출 하지 못하도록 Xamarin.Forms 레이아웃 계산 `ListView` 스크롤입니다.


## <a name="related-links"></a>관련 링크

- [ListView 성능](~/xamarin-forms/user-interface/listview/performance.md)
- [CustomRendererViewCell (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
