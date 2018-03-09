---
title: Xamarin.Forms Layouts
description: "Xamarin.Forms 레이아웃 논리 구조를으로 사용자 인터페이스 컨트롤을 구성 하는 데 사용 됩니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ecea0f55360fcde7a50c52bb33c45a2c5fff5eeb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms Layouts

_Xamarin.Forms 레이아웃 논리 구조를으로 사용자 인터페이스 컨트롤을 구성 하는 데 사용 됩니다._

<style>.tableimg {최대 너비: 없음! 중요;을 (를)</style>

## <a name="layouts"></a>레이아웃

[`Layout`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) Xamarin.Forms에는 클래스는 역할을 하는 컨테이너의 다른 뷰나 레이아웃 보기의 특수 한 하위 형식입니다. 일반적으로 Xamarin.Forms는 응용 프로그램에서 자식 요소의 크기와 위치를 설정 하는 논리가 포함 됩니다.

 [ ![](layouts-images/layouts-sml.png "Xamarin.Forms Layout Types")](layouts-images/layouts.png "Xamarin.Forms Layout Types")

<br clear="all" />

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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a>
    </td>
    <td valign="top">
템플릿 기반 뷰에 대 한 레이아웃 관리자입니다. 내에서 사용 되는 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/">ControlTemplate</a> 를 제공할 때는 내용을 표시할 위치를 표시 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/Templates/ControlTemplates/SimpleTheme/SimpleTheme/App.xaml"><img src="layouts-images/ContentPresenter.png" title="ContentPresenter 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a>
    </td>
    <td valign="top">
단일 콘텐츠가 있는 요소입니다. ContentView 거의 사용을 있습니다. 용도 사용자 정의 복합 보기에 대 한 기본 클래스로 사용할 것입니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentViewDemoPage.cs"><img src="layouts-images/ContentView.png" title="ContentView 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">프레임</a>
    </td>
    <td valign="top">
몇 가지 프레이밍 옵션이 단일를 자식으로 포함 하는 요소입니다. 프레임 기본값이 <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/">Xamarin.Forms.Layout.Padding</a> 20입니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/FrameDemoPage.cs"><img src="layouts-images/Frame.png" title="프레임 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>
    </td>
    <td valign="top">
요소 콘텐츠가 경우 스크롤 가능 필요 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ScrollViewDemoPage.cs"><img src="layouts-images/ScrollView.png" title="ScrollView 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a>
    </td>
    <td valign="top">
컨트롤 템플릿 및 기본 클래스를 사용 하 여 콘텐츠를 표시 하는 요소 <a href=""/api/type/Xamarin.Forms.ContentView/">ContentView</a>합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="layouts-images/TemplatedView.png" title="TemplatedView 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a>
    </td>
    <td valign="top">
요청 된 절대 위치에 자식 요소를 배치 합니다. 앵커와 범위에 할당 된 사용자는 컨트롤의 크기와 위치를 정의 합니다.
    </td>
    <td valign="top">
      <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/AbsoluteLayoutDemoPage.cs"><img src="layouts-images/AbsoluteLayout.png" title="AbsoluteLayout 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Grid</a>
    </td>
    <td valign="top">
행과 열으로 정렬 하는 보기를 포함 하는 레이아웃 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/GridDemoPage.cs"><img src="layouts-images/Grid.png" title="표 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/%601">레이아웃</a> 사용 하 여 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/">제약 조건</a>레이아웃으로 s 자식입니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/RelativeLayoutDemoPage.cs"><img src="layouts-images/RelativeLayout.png" title="RelativeLayout 예제" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/">레이아웃</a> 가로 또는 세로 방향으로 지정할 수 있는 한 줄에 자식 요소를 배치 하는 합니다. 이 레이아웃은 자식 범위 자동으로 설정 레이아웃 주기 중. 범위 지정 된 사용자 덮어쓰게 됩니다 및 따라서 설정 하지 않아야 자식 요소에 대해 사용자가 합니다.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StackLayoutDemoPage.cs"><img src="layouts-images/StackLayout.png" title="StackLayout 예제" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 소개](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms 갤러리 (샘플)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API 설명서](https://developer.xamarin.com/api/namespace/Xamarin.Forms)
