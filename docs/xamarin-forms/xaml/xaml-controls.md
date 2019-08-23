---
title: XAML 컨트롤
description: Xamarin.ios에 정의 된 모든 뷰는 XAML 파일에서 참조할 수 있습니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2019
ms.openlocfilehash: 9d9d07388c2dc32e82fb780d0ed09e586e401662
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648651"
---
# <a name="xaml-controls"></a>XAML 컨트롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

뷰는 레이블, 단추 및 일반적으로 이라고 하는 슬라이더와 같은 사용자 인터페이스 개체 *컨트롤* 또는 *위젯* 다른 그래픽 프로그래밍 환경에서입니다. 파생 되는 모든 Xamarin.Forms에서 지 원하는 뷰를 [ `View` ](xref:Xamarin.Forms.View) 클래스입니다.

Xamarin.ios에 정의 된 모든 뷰는 XAML 파일에서 참조할 수 있습니다.

## <a name="views-for-presentation"></a>프레젠테이션 보기

|     |     |
| --- | --- |
| <h3>BoxView</h3>특정 색의 사각형을 표시 합니다.<p align="center">![BoxView의 스크린샷](xaml-controls-images/BoxView.png "Boxview")</p>[API](xref:Xamarin.Forms.BoxView) / [가이드](~/xamarin-forms/user-interface/boxview.md) | <pre valign="center">&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>이미지</h3>비트맵을 표시 합니다.<p align="center">![이미지 스크린샷](xaml-controls-images/Image.png "이미지")</p>[API](xref:Xamarin.Forms.Image) / [가이드](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>레이블</h3>하나 이상의 텍스트 줄을 표시 합니다.<p align="center">![레이블의 스크린샷](xaml-controls-images/Label.png "레이블")</p>[API](xref:Xamarin.Forms.Label) / [가이드](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>지도</h3>지도를 표시 합니다.<p align="center">![지도의 스크린샷](xaml-controls-images/Map.png "지도")</p>[API](xref:Xamarin.Forms.Maps.Map) / [가이드](~/xamarin-forms/user-interface/map.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>WebView</h3>웹 페이지 또는 HTML 콘텐츠를 표시 합니다.<p align="center">![웹 보기의 스크린샷](xaml-controls-images/WebView.png "웹 보기")</p>[API](xref:Xamarin.Forms.WebView) / [가이드](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>명령을 시작 하는 뷰

|     |     |
| --- | --- |
| <h3>단추</h3>사각형 개체에 텍스트를 표시 합니다.<p align="center">![단추의 스크린샷](xaml-controls-images/Button.png "단추")</p>[API](xref:Xamarin.Forms.Button) / [가이드](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>사각형 개체에 이미지를 표시 합니다.<p align="center">![ImageButton의 스크린샷](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton) / [가이드](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>SearchBar</h3>검색을 수행 하는 검색 창을 표시 합니다.<p align="center">![SearchBar의 스크린샷](xaml-controls-images/SearchBar.png "Searchbar")</p>[API](xref:Xamarin.Forms.SearchBar) | <p valign="center"><pre>&lt;SearchBar Placeholder="Xamarin.Forms Property"<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>값을 설정 하는 것에 대 한 보기

|     |     |
| --- | --- |
| <h3>CheckBox</h3>`boolean` 값을 선택할 수 있습니다.<p align="center">![CheckBox의 스크린샷](xaml-controls-images/CheckBox.png "확인란")</p> [도움이](~/xamarin-forms/user-interface/checkbox.md) | <p valign="center"><pre>&lt;CheckBox IsChecked="true"<br />          HorizontalOptions="Center"<br />          VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>슬라이더</h3>연속 범위의 `double` 값을 선택할 수 있습니다.<p align="center">![슬라이더의 스크린샷](xaml-controls-images/Slider.png "슬라이더")</p>[API](xref:Xamarin.Forms.Slider) / [가이드](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Stepper</h3>증분 범위에서 `double` 값을 선택할 수 있습니다.<p align="center">![스텝 퍼의 스크린샷](xaml-controls-images/Stepper.png "스텝 퍼")</p>[API](xref:Xamarin.Forms.Stepper) / [가이드](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>스위치</h3>`boolean` 값을 선택할 수 있습니다.<p align="center">![스위치 스크린샷](xaml-controls-images/Switch.png "스위치")</p>[API](xref:Xamarin.Forms.Switch) / [가이드](~/xamarin-forms/user-interface/switch.md)| <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>날짜를 선택할 수 있습니다.<p align="center">![DatePicker 스크린샷](xaml-controls-images/DatePicker.png "DatePicker")</p>[API](xref:Xamarin.Forms.DatePicker) / [가이드](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>시간을 선택할 수 있습니다.<p align="center">![TimePicker의 스크린샷](xaml-controls-images/TimePicker.png "Timepicker")</p>[API](xref:Xamarin.Forms.TimePicker) / [가이드](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>텍스트 편집에 대 한 보기

|     |     |
| --- | --- |
| <h3>입력</h3>한 줄의 텍스트를 입력 하 고 편집할 수 있습니다.<p align="center">![항목의 스크린샷](xaml-controls-images/Entry.png "항목")</p>[API](xref:Xamarin.Forms.Entry) / [가이드](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>편집기</h3>여러 줄의 텍스트를 입력 하 고 편집할 수 있습니다.<p align="center">![편집기의 스크린샷](xaml-controls-images/Editor.png "레이블")</p>[API](xref:Xamarin.Forms.Editor) / [가이드](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>작업을 나타내기 위해 뷰

|     |     |
| --- | --- |
| <h3>ActivityIndicator</h3>진행률을 표시 하지 않고 응용 프로그램이 긴 작업에서 사용 되 고 있음을 보여 주는 애니메이션을 표시 합니다.<p align="center">![ActivityIndicator의 스크린샷](xaml-controls-images/ActivityIndicator.png "Activityindicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>응용 프로그램에서 시간이 오래 걸리는 작업을 진행 하 고 있음을 보여 주는 애니메이션을 표시 합니다.<p align="center">![ProgressBar의 스크린샷](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API](xref:Xamarin.Forms.ProgressBar) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>컬렉션을 표시 하는 보기

|     |     |
| --- | --- |
| <h3>CollectionView</h3>다른 레이아웃 사양을 사용 하 여 선택 가능한 데이터 항목의 스크롤 가능한 목록을 표시 합니다.<p align="center">![CollectionView 스크린샷](xaml-controls-images/CollectionView.png "CollectionView")</p>[도움이](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />    &lt;CollectionView.ItemsLayout&gt;<br />       &lt;GridItemsLayout Orientation="Vertical"<br />                        Span="2" /&gt;<br />    &lt;/CollectionView.ItemsLayout&gt;<br />&lt;/CollectionView/&gt;</pre></p> |
| <h3>ListView</h3>스크롤할 수 있는 선택 가능한 데이터 항목의 목록을 표시 합니다.<p align="center">![ListView의 스크린샷](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView) / [가이드](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>선택기</h3>텍스트 문자열 목록에서 선택 항목을 표시 합니다.<p align="center">![선택의 스크린샷](xaml-controls-images/Picker.png "선택")</p>[API](xref:Xamarin.Forms.Picker) / [가이드](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&lt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>대화형 행 목록을 표시 합니다.<p align="center">![TableView 스크린샷](xaml-controls-images/TableView.png "TableView")</p>[API](xref:Xamarin.Forms.TableView) / [가이드](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms FormsGallery 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API 설명서](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
