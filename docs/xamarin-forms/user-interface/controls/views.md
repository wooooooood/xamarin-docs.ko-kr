---
title: "Xamarin.Forms 뷰"
description: "Xamarin.Forms 뷰는 플랫폼 간 모바일 사용자 인터페이스의 구성 요소입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: df8c8463b2556035c5369c70cb10dbc3dc6b6743
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms 뷰

_Xamarin.Forms 뷰는 플랫폼 간 모바일 사용자 인터페이스의 구성 요소입니다._

<style>.tableimg {최대 너비: 없음! 중요;을 (를)</style>

## <a name="views"></a>보기

Xamarin.Forms는 단어 *보기* 단추, 레이블 또는 텍스트 입력 상자 위젯 컨트롤로 일반적으로 알려져 있습니다-같은 시각적 개체를 가리킵니다.

이러한 UI 요소는 일반적으로 하위 클래스의 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)합니다.

<br clear="right" />

Xamarin.Forms를 지원합니다.

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Type</strong>
    </th>
    <th>
      <strong>설명</strong>
    </th>
    <th style="min-width:400px">
      <strong>스크린 샷</strong>
    </th>

  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a>
    </td>
    <td valign="top">
진행 중인는 점을 나타내는 데 사용 하는 시각적 컨트롤입니다. 이 컨트롤 항목 상황을 진행 하는 방법에 대 한 정보 없이 사용자에 게 시각적 실마리를 제공 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ActivityIndicatorDemoPage.cs"><img src="views-images/ActivityIndicator.png" title="ActivityIndicator 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 단색 색이 지정 된 사각형을 그리는 데 사용 합니다. BoxView는 이미지 또는 사용자 지정 요소에 대 한 유용한 대리는 초기 프로토타입을 수행할 때. BoxView 40 x 40의 기본 크기 요청을 있습니다. 다른 크기를 해야 하는 경우 할당 된 <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/">VisualElement.WidthRequest</a> 및 <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/">VisualElement.HeightRequest</a>합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/BoxViewDemoPage.cs"><img src="views-images/BoxView.png" title="BoxView 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">단추</a>
    </td>
    <td align="center" valign="top">
단추 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 터치 이벤트에 반응 하 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ButtonDemoPage.cs"><img src="views-images/Button.png" title="단추 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 날짜를 선택할 수 있도록 합니다. DatePicker의 시각적 표시는 중 하나를 매우 비슷합니다 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">항목</a>제외 하 고 키보드 대신 날짜를 선택 하는 데는 특수 컨트롤 표시 </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/DatePickerDemoPage.cs"><img src="views-images/DatePicker.png" title="DatePicker 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">편집기</a>
    </td>
    <td valign="top">
여러 줄의 텍스트를 편집할 수 있는 컨트롤입니다. 한 줄 항목에 대 한 참조 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">항목</a>합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EditorDemoPage.cs"><img src="views-images/Editor.png" title="편집기 예" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a>
    </td>
    <td valign="top">
한 줄 텍스트를 편집할 수 있는 컨트롤입니다. 항목은 한 줄 텍스트 입력 합니다. 가장 작은 개별 가지 사용자 이름 및 암호와 같은 정보를 수집 하기 위한 사용 됩니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="views-images/Entry.png" title="항목 예" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Image</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 이미지를 보유 하는 합니다.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">이미지 API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/images.md">이미지 작업</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageDemoPage.cs">데모 원본</a>
    </td>
    <td>
    <img src="views-images/Image.png" title="이미지의 예" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">레이블</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 텍스트를 표시 하는 읽기 전용 형식입니다. 레이블의 사용 하 여 한 줄 텍스트 요소 뿐만 아니라 여러 줄 텍스트 블록을 표시 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/LabelDemoPage.cs"><img src="views-images/Label.png" title="레이블 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a>
    </td>
    <td valign="top">
<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/">ItemView</a> 세로 목록으로 데이터의 컬렉션을 표시 하는 합니다.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/listview/index.md">ListView 설명서</a>
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ListViewDemoPage.cs"><img src="views-images/ListView.png" title="ListView 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/">OpenGLView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> OpenGL 콘텐츠를 표시 하는입니다.
    <ul>
      <li>IOS 및 Android 프로젝트 (Windows Phone 지원 없음)만 작동합니다.
      <li>에 대 한 참조는 <b>OpenTK 1.0</b> iOS 및 Android 프로젝트의 어셈블리.</li>
      <li>공유 프로젝트;에서 사용할에 가장 적합 한 PCL에 사용 되는 다음 한 DependencyService 필요한 수도 있습니다.</li>
    </ul>
    </td>
    <td>
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/"><img src="views-images/OpenGL.png" title="OpenGlView 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">선택</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 목록의 요소를 선택 하는 데 제어 합니다. 선택기를의 시각적 표시는 비슷합니다는 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">항목</a>, 키보드를 대신 선택 컨트롤을 표시 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/PickerDemoPage.cs"><img src="views-images/Picker.png" title="선택기 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 진행률을 나타내는 컨트롤입니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ProgressBarDemoPage.cs"><img src="views-images/ProgressBar.png" title="ProgressBar 예제 클래스 ="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 검색 상자를 제공 하는 컨트롤입니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SearchBarDemoPage.cs"><img src="views-images/SearchBar.png" title="SearchBar 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">슬라이더</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 컨트롤을 선형 값을 입력 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SliderDemoPage.cs"><img src="views-images/Slider.png" title="슬라이더 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">스텝 퍼</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 불연속 값을 입력 하는 제어 범위를 제한 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StepperDemoPage.cs"><img src="views-images/Stepper.png" title="스텝 퍼 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">스위치</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 채우기에 사용 되는 값을 제공 하는 컨트롤입니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchDemoPage.cs"><img src="views-images/Switch.png" title="스위치 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 의 행을 보유 하는 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">셀</a>s입니다.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/tableview.md">TableView 설명서</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TableViewFormDemoPage.cs">데모 원본</a>
    </td>
    <td>
    <img src="views-images/TableViewNewest.png" title="TableView 예제" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> 선택 하는 데 시간을 제공 하는 컨트롤입니다. TimePicker의 시각적 표시는 중 하나를 매우 비슷합니다 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">항목</a>한다는 점을 제외 하는 한 번 선택 하는 데 특별 한 컨트롤이 키보드 대신 나타납니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TimePickerDemoPage.cs"><img src="views-images/TimePicker.png" title="TimePicker 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">웹 보기</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a> HTML 콘텐츠를 표시 합니다.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/webview.md">웹 보기 설명서</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/WebViewDemoPage.cs">데모 원본</a>
    </td>
    <td>
    <img src="views-images/WebView.png" title="WebView 예제" class="tableimg">
    </td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms 갤러리 (샘플)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API 설명서](https://developer.xamarin.com/api/root/Xamarin.Forms/)
