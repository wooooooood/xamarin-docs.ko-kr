---
title: Xamarin.Forms Pages
description: "Xamarin.Forms 페이지 플랫폼 간 모바일 앱 화면을 나타냅니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 35822dbbb7d5694e7f1f0a3f35f10df404206af9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms Pages

_Xamarin.Forms 페이지 플랫폼 간 모바일 앱 화면을 나타냅니다._

<style>.tableimg {최대 너비: 없음! 중요;을 (를)</style>

## <a name="pages"></a>인쇄할 페이지

[ `Page` ](http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.Page) 클래스는 대부분 또는 전체 화면을 차지 하며 단일 자식을 포함 하는 시각적 요소입니다. A `Xamarin.Forms.Page` iOS에 있는 뷰 컨트롤러 또는 Windows Phone 페이지를 나타냅니다. Android에서 각 페이지 활동, 같은 화면을 차지 하지만 Xamarin.Forms 페이지는 *하지* 활동입니다.

 [ ![](pages-images/pages-sml.png "Xamarin.Forms 페이지 형식")](pages-images/pages.png "Xamarin.Forms 페이지 유형")

<br clear="all" />

Xamarin.Forms를 지원합니다.

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <tr>
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
  </thead></tr>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>
    </td>
    <td align="center" valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a> 표시 하는 단일 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">보기</a>, 종종와 같은 컨테이너는 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a> 또는 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentPageDemoPage.cs"><img src="pages-images/ContentPage.png" title="ContentPage 예제" class="tableimg">
    </a></td>
  </tr><tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">페이지</a> 정보의 두 개의 창이 관리 하 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/MasterDetailPageDemoPage.cs"><img src="pages-images/MasterDetailPage.png" title="MasterDetailPage 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">페이지</a> 탐색 및 사용자 경험의 다른 페이지 스택을 관리 합니다.  
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/NavigationPageDemoPage.cs"><img src="pages-images/NavigationPage.png" title="NavigationPage 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">페이지</a> 자식 간의 탐색 탭을 사용 하 여 페이지 수 있도록 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TabbedPageDemoPage.cs"><img src="pages-images/TabbedPage.png" title="TabbedPage 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">페이지</a> 컨트롤 템플릿 및 기본 클래스를 사용 하 여 전체 화면 콘텐츠를 표시 하는 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>합니다.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="pages-images/TemplatedPage.png" title="TemplatedPage 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">페이지</a> 살짝 제스처 갤러리와 같은 하위 페이지 사이 허용 합니다.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CarouselPageDemoPage.cs"><img src="pages-images/CarouselPage.png" title="CarouselPage 예제" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms 갤러리 (샘플)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API 설명서](http://iosapi.xamarin.com/?link=N%3aXamarin.Forms)
