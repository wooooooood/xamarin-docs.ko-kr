---
title: Xamarin.Forms 어두운 테마
description: 이 문서에서는 앱에서 Xamarin.Forms 어두운 테마를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 1fc329f506afde04b0dc59dc637d999865aafbe1
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38853247"
---
# <a name="xamarinforms-dark-theme"></a>Xamarin.Forms 어두운 테마

![](~/media/shared/preview.png "이 API는 현재 미리 보기")

> [!NOTE]
> 테마는 Xamarin.Forms 2.3 미리 보기 릴리스의 경우에 필요 합니다. 확인 합니다 [문제 해결 팁](~/xamarin-forms/user-interface/themes/index.md) 오류가 발생 한 경우.

어두운 테마를 사용 합니다 하:

## <a name="1-add-nuget-packages"></a>1. Nuget 패키지 추가

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2. 리소스 사전에 추가

에 **App.xaml** 파일을 새 사용자 지정 추가 `xmlns` 테마에 대 한 테마의 리소스는 응용 프로그램의 리소스 사전과 병합을 확인 합니다.
XAML 파일의 예는 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:dark="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Dark">
    <Application.Resources>
        <ResourceDictionary MergedWith="dark:DarkThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. 테마 클래스를 로드 합니다.

이 따라 [문제 해결 단계](~/xamarin-forms/user-interface/themes/index.md) iOS 및 Android 응용 프로그램 프로젝트에서 필요한 코드를 추가 합니다.

## <a name="4-use-styleclass"></a>4. 사용 하 여 StyleClass

단추 및 레이블을 생성 하는 태그와 함께 어두운 테마에서의 예는 다음과 같습니다.

[![](dark-images/dark-theme-sml.png "단추 및 어두운 테마에서 레이블을")](dark-images/dark-theme.png#lightbox "단추 및 어두운 테마의 레이블")

```xaml
<StackLayout Padding="20">
    <Button Text="Button Default" />
    <Button Text="Button Class Default" StyleClass="Default" />
    <Button Text="Button Class Primary" StyleClass="Primary" />
    <Button Text="Button Class Success" StyleClass="Success" />
    <Button Text="Button Class Info" StyleClass="Info" />
    <Button Text="Button Class Warning" StyleClass="Warning" />
    <Button Text="Button Class Danger" StyleClass="Danger" />
    <Button Text="Button Class Link" StyleClass="Link" />

    <Button Text="Button Class Default Small" StyleClass="Small" />
    <Button Text="Button Class Default Large" StyleClass="Large" />
</StackLayout>
```

합니다 [기본 제공 클래스의 전체 목록은](~/xamarin-forms/user-interface/themes/index.md) 어떤 스타일은 일부 공용 컨트롤을 보여 줍니다.
