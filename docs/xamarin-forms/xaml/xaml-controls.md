---
title: XAML 컨트롤
description: 에서 정의 된 모든 뷰는 Xamarin.Forms XAML 파일에서 참조할 수 있습니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/09/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fd871b883d843fae039f6bdabac5812e90c451ca
ms.sourcegitcommit: cd0c0999b53e825b60471bfbfd4144cfcd783587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225573"
---
# <a name="xaml-controls"></a>XAML 컨트롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

뷰는 다른 그래픽 프로그래밍 환경에서 일반적으로 *컨트롤이* 나 *위젯* 으로 알려진 레이블, 단추 및 슬라이더와 같은 사용자 인터페이스 개체입니다. 모두에서 지원 되는 뷰는 Xamarin.Forms 클래스에서 파생 [`View`](xref:Xamarin.Forms.View) 됩니다.

에서 정의 된 모든 뷰는 Xamarin.Forms XAML 파일에서 참조할 수 있습니다.

## <a name="views-for-presentation"></a>프레젠테이션 뷰

|     |     |
| --- | --- |
| <h3>BoxView</h3>특정 색의 사각형을 표시 합니다.<p align="center">![BoxView의 스크린샷](xaml-controls-images/BoxView.png "BoxView")</p>[API](xref:Xamarin.Forms.BoxView)  /  [가이드](~/xamarin-forms/user-interface/boxview.md) | <p valign="center"><pre>&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>타원</h3>타원 또는 원을 표시 합니다.<p align="center">![타원의 스크린샷](xaml-controls-images/Ellipse.png "타원")</p>[API](xref:Xamarin.Forms.Shapes.Ellipse)  /  [가이드](~/xamarin-forms/user-interface/shapes/ellipse.md) | <p valign="center"><pre>&lt;Ellipse Fill="Red"<br />         WidthRequest="150"<br />         HeightRequest="50"<br />         HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Expander</h3>모든 콘텐츠를 호스트 하는 확장 가능한 컨테이너를 제공 합니다.<p align="center">![확장기의 스크린샷](xaml-controls-images/Expander.png "Expander")</p>[도움이](~/xamarin-forms/user-interface/expander.md) | <pre>&lt;Expander&gt;<br />    &lt;Expander.Header&gt;<br />        &lt;Label Text="Baboon" /&gt;<br />    &lt;/Expander.Header&gt;<br />    &lt;Image Source="Baboon.png"<br />           Aspect="AspectFill" /&gt;<br />&lt;/Expander&gt;</pre></p> |
| <h3>이미지</h3>비트맵을 표시 합니다.<p align="center">![이미지 스크린샷](xaml-controls-images/Image.png "이미지")</p>[API](xref:Xamarin.Forms.Image)  /  [가이드](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>레이블</h3>하나 이상의 텍스트 줄을 표시 합니다.<p align="center">![레이블의 스크린샷](xaml-controls-images/Label.png "레이블")</p>[API](xref:Xamarin.Forms.Label)  /  [가이드](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>꺾은선형</h3>선을 표시 합니다.<p align="center">![선의 스크린샷](xaml-controls-images/Line.png "꺾은선형")</p>[API](xref:Xamarin.Forms.Shapes.Line)  /  [가이드](~/xamarin-forms/user-interface/shapes/line.md) | <p valign="center"><pre>&lt;Line X1="40"<br />      Y1="0"<br />      X2="0"<br />      Y2="120"<br />      Stroke="Red"<br />      HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>맵</h3>지도를 표시 합니다.<p align="center">![지도의 스크린샷](xaml-controls-images/Map.png "맵")</p>[API](xref:Xamarin.Forms.Maps.Map)  /  [가이드](~/xamarin-forms/user-interface/map/index.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>MediaElement</h3>비디오 또는 오디오를 재생 합니다.<p align="center">![MediaElement의 스크린샷](xaml-controls-images/MediaElement.png "MediaELement")</p>[API](xref:Xamarin.Forms.MediaElement)  /  [가이드](~/xamarin-forms/user-interface/mediaelement.md) | <p valign="center"><pre>&lt;MediaElement Source="https://sec.ch9.ms/ch9/XamarinShow_mid.mp4"<br />              AutoPlay="True"<br />              ShowsPlaybackControls="True" /&gt;</pre></p> |
| <h3>경로</h3>곡선 및 복잡 한 셰이프를 표시 합니다.<p align="center">![경로의 스크린샷](xaml-controls-images/Path.png "경로")</p>[API](xref:Xamarin.Forms.Shapes.Path)  /  [가이드](~/xamarin-forms/user-interface/shapes/path.md) | <p valign="center"><pre>&lt;Path Stroke="Black"<br />      Aspect="Uniform"<br />      HorizontalOptions="Center"<br />      HeightRequest="100"<br />      WidthRequest="100"<br />      Data="M13.9,16.2<br />            L32,16.2 32,31.9 13.9,30.1Z<br />            M0,16.2<br />            L11.9,16.2 11.9,29.9 0,28.6Z<br />            M11.9,2<br />            L11.9,14.2 0,14.2 0,3.3Z<br />            M32,0<br />            L32,14.2 13.9,14.2 13.9,1.8Z" /&gt;</pre></p> |
| <h3>다각형</h3>다각형을 표시 합니다.<p align="center">![다각형의 스크린샷](xaml-controls-images/Polygon.png "다각형")</p>[API](xref:Xamarin.Forms.Shapes.Polygon)  /  [가이드](~/xamarin-forms/user-interface/shapes/polygon.md) | <p valign="center"><pre>&lt;Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96,<br/>                 50 96, 48 192, 150 200 144 48"<br />         Fill="Blue"<br />         Stroke="Red"<br />         StrokeThickness="3"<br />         HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>폴리라인</h3>일련의 연결 된 직선을 표시 합니다.<p align="center">![다중선의 스크린샷](xaml-controls-images/Polyline.png "폴리라인")</p>[API](xref:Xamarin.Forms.Shapes.Polyline)  /  [가이드](~/xamarin-forms/user-interface/shapes/Polyline.md) | <p valign="center"><pre>&lt;Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0<br />                  43,60 48,30 100,30"<br />          Stroke="Red"<br />          HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>사각형</h3>사각형 또는 사각형을 표시 합니다.<p align="center">![사각형의 스크린샷](xaml-controls-images/Rectangle.png "사각형")</p>[API](xref:Xamarin.Forms.Shapes.Rectangle)  /  [가이드](~/xamarin-forms/user-interface/shapes/rectangle.md) | <p valign="center"><pre>&lt;Rectangle Fill="Red"<br />           WidthRequest="150"<br />           HeightRequest="50"<br />           HorizontalOptions="Center" /&gt;</pre></p> |  
| <h3>WebView</h3>웹 페이지 또는 HTML 콘텐츠를 표시 합니다.<p align="center">![웹 보기의 스크린샷](xaml-controls-images/WebView.png "WebView")</p>[API](xref:Xamarin.Forms.WebView)  /  [가이드](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>명령 시작 뷰

|     |     |
| --- | --- |
| <h3>단추</h3>사각형 개체에 텍스트를 표시 합니다.<p align="center">![단추의 스크린샷](xaml-controls-images/Button.png "단추")</p>[API](xref:Xamarin.Forms.Button)  /  [가이드](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>사각형 개체에 이미지를 표시 합니다.<p align="center">![ImageButton의 스크린샷](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton)  /  [가이드](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>RadioButton</h3>집합에서 하나의 옵션을 선택할 수 있습니다.<p align="center">![RadioButton의 스크린샷](xaml-controls-images/RadioButton.png "RadioButton")</p>[도움이](~/xamarin-forms/user-interface/radiobutton.md) | <p valign="center"><pre>&lt;RadioButton Text="Pineapple"<br/>             CheckedChanged="OnRadioButtonCheckedChanged" /&gt;</pre></p> |
| <h3>RefreshView</h3>스크롤할 수 있는 콘텐츠를 위한 끌어오기-새로 고침 기능을 제공 합니다.<p align="center">![RefreshView의 스크린샷](xaml-controls-images/RefreshView.png "RefreshView")</p>[도움이](~/xamarin-forms/user-interface/refreshview.md) | <p valign="center"><pre>&lt;RefreshView IsRefreshing="{Binding IsRefreshing}"<br />             Command="{Binding RefreshCommand}" &gt;<br />    &lt;!-- Scrollable control goes here --&gt;<br />&lt;/RefreshView&gt;</pre></p> |
| <h3>SearchBar</h3> 검색을 수행 하는 데 사용 하는 사용자 입력을 허용 합니다.<p align="center">![SearchBar의 스크린샷](xaml-controls-images/SearchBar.png "SearchBar")</p>[도움이](~/xamarin-forms/user-interface/searchbar.md) | <p valign="center"><pre>&lt;SearchBar Placeholder="Enter search term"<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
| <h3>SwipeView</h3> 살짝 밀기 제스처로 표시 되는 상황에 맞는 메뉴 항목을 제공 합니다.<p align="center">![SwipeView 스크린샷](xaml-controls-images/SwipeView.png "SwipeView")</p>[도움이](~/xamarin-forms/user-interface/swipeview.md) | <p valign="center"><pre>&lt;SwipeView&gt;<br />    &lt;SwipeView.LeftItems&gt;<br />        &lt;SwipeItems&gt;<br />            &lt;SwipeItem Text="Delete"<br />                       IconImageSource="delete.png"<br />                       BackgroundColor="LightPink"<br />                       Invoked="OnDeleteInvoked" /&gt;<br />        &lt;/SwipeItems&gt;<br />    &lt;/SwipeView.LeftItems&gt;<br />    &lt;!-- Content --&gt;<br />&lt;/SwipeView&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>값 설정 뷰

|     |     |
| --- | --- |
| <h3>CheckBox</h3>값을 선택할 수 있습니다 `boolean` .<p align="center">![CheckBox의 스크린샷](xaml-controls-images/CheckBox.png "CheckBox")</p> [도움이](~/xamarin-forms/user-interface/checkbox.md) | <p valign="center"><pre>&lt;CheckBox IsChecked="true"<br />          HorizontalOptions="Center"<br />          VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>슬라이더</h3>연속 범위의 값을 선택할 수 있습니다 `double` .<p align="center">![슬라이더의 스크린샷](xaml-controls-images/Slider.png "슬라이더")</p>[API](xref:Xamarin.Forms.Slider)  /  [가이드](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Stepper</h3>`double`증분 범위에서 값을 선택할 수 있습니다.<p align="center">![스텝 퍼의 스크린샷](xaml-controls-images/Stepper.png "Stepper")</p>[API](xref:Xamarin.Forms.Stepper)  /  [가이드](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>스위치</h3>값을 선택할 수 있습니다 `boolean` .<p align="center">![스위치 스크린샷](xaml-controls-images/Switch.png "스위치")</p>[API](xref:Xamarin.Forms.Switch)  /  [가이드](~/xamarin-forms/user-interface/switch.md)| <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>날짜를 선택할 수 있습니다.<p align="center">![DatePicker 스크린샷](xaml-controls-images/DatePicker.png "DatePicker")</p>[API](xref:Xamarin.Forms.DatePicker)  /  [가이드](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>시간을 선택할 수 있습니다.<p align="center">![TimePicker의 스크린샷](xaml-controls-images/TimePicker.png "TimePicker")</p>[API](xref:Xamarin.Forms.TimePicker)  /  [가이드](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>텍스트 편집 뷰

|     |     |
| --- | --- |
| <h3>입력</h3>한 줄의 텍스트를 입력 하 고 편집할 수 있습니다.<p align="center">![항목의 스크린샷](xaml-controls-images/Entry.png "입력")</p>[API](xref:Xamarin.Forms.Entry)  /  [가이드](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>편집기</h3>여러 줄의 텍스트를 입력 하 고 편집할 수 있습니다.<p align="center">![편집기의 스크린샷](xaml-controls-images/Editor.png "레이블")</p>[API](xref:Xamarin.Forms.Editor)  /  [가이드](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>작업 표시 뷰

|     |     |
| --- | --- |
| <h3>ActivityIndicator</h3>진행률을 표시 하지 않고 응용 프로그램이 긴 작업에서 사용 되 고 있음을 보여 주는 애니메이션을 표시 합니다.<p align="center">![ActivityIndicator의 스크린샷](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator)  /  [가이드](~/xamarin-forms/user-interface/activityindicator.md) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>응용 프로그램에서 시간이 오래 걸리는 작업을 진행 하 고 있음을 보여 주는 애니메이션을 표시 합니다.<p align="center">![ProgressBar의 스크린샷](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API](xref:Xamarin.Forms.ProgressBar)  /  [가이드](~/xamarin-forms/user-interface/progressbar.md) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>컬렉션 표시 뷰

|     |     |
| --- | --- |
| <h3>CarouselView</h3>스크롤 가능한 데이터 항목 목록을 표시 합니다.<p align="center">![CarouselView 스크린샷](xaml-controls-images/CarouselView.png "CarouselView")</p>[도움이](~/xamarin-forms/user-interface/carouselview/index.md) | <p valign="center"><pre>&lt;CarouselView ItemsSource="{Binding Monkeys}"&gt;<br/>              ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p>|
| <h3>CollectionView</h3>다른 레이아웃 사양을 사용 하 여 선택 가능한 데이터 항목의 스크롤 가능한 목록을 표시 합니다.<p align="center">![CollectionView 스크린샷](xaml-controls-images/CollectionView.png "CollectionView")</p>[도움이](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />    &lt;CollectionView.ItemsLayout&gt;<br />       &lt;GridItemsLayout Orientation="Vertical"<br />                        Span="2" /&gt;<br />    &lt;/CollectionView.ItemsLayout&gt;<br />&lt;/CollectionView/&gt;</pre></p> |
| <h3>IndicatorView</h3>의 항목 수를 나타내는 표시기를 표시 `CarouselView` 합니다.<p align="center">![IndicatorView의 스크린샷](xaml-controls-images/IndicatorView.png "IndicatorView")</p>[도움이](~/xamarin-forms/user-interface/indicatorview.md) | <p valign="center"><pre>&lt;IndicatorView x:Name="indicatorView"<br />               IndicatorColor="LightGray"<br />               SelectedIndicatorColor="DarkGray" /&gt;</pre></p> |
| <h3>ListView</h3>스크롤할 수 있는 선택 가능한 데이터 항목의 목록을 표시 합니다.<p align="center">![ListView의 스크린샷](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView)  /  [가이드](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>선택기</h3>텍스트 문자열 목록에서 선택 항목을 표시 합니다.<p align="center">![선택의 스크린샷](xaml-controls-images/Picker.png "선택기")</p>[API](xref:Xamarin.Forms.Picker)  /  [가이드](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&gt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>대화형 행 목록을 표시 합니다.<p align="center">![TableView 스크린샷](xaml-controls-images/TableView.png "TableView")</p>[API](xref:Xamarin.Forms.TableView)  /  [가이드](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
