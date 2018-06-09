---
title: Xamarin.Forms 밝은 테마
description: 이 문서에서는 앱에서 Xamarin.Forms 밝은 테마를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 7f40e375d653acec60f8848627234ab46fcce8de
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245249"
---
# <a name="xamarinforms-light-theme"></a>Xamarin.Forms 밝은 테마

![](~/media/shared/preview.png "이 API는 현재 미리 보기")

> [!NOTE]
> 테마는 Xamarin.Forms 2.3 미리 보기 버전에 필요 합니다. 확인 된 [문제 해결 팁](~/xamarin-forms/user-interface/themes/index.md) 오류가 발생 한 경우.

밝은 테마를 사용 합니다 하:

## <a name="1-add-nuget-packages"></a>1. Nuget 패키지 추가

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2. 리소스 사전에 추가

에 **App.xaml** 파일에 새 사용자 지정 추가 `xmlns` 는 테마에 대 한 응용 프로그램의 리소스 사전을 사용 하 여 병합 테마의 리소스를 확인 합니다.
XAML 파일의 예는 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:light="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light">
    <Application.Resources>
        <ResourceDictionary MergedWith="light:LightThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3. 테마 클래스를 로드 합니다.

이 따라 [단계 문제 해결](~/xamarin-forms/user-interface/themes/index.md) iOS 및 Android 응용 프로그램 프로젝트에 필요한 코드를 추가 합니다.

## <a name="4-use-styleclass"></a>4. StyleClass 사용

단추와 레이블의 생성 하는 태그와 함께 밝은 테마의 예를 들면 다음과 같습니다.

[![](light-images/light-theme-sml.png "단추 및 밝은 테마의 레이블")](light-images/light-theme.png#lightbox "단추 및 밝은 테마의 레이블")

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

[기본 제공 클래스의 전체 목록은](~/xamarin-forms/user-interface/themes/index.md) 몇 가지 공용 컨트롤에 대 한 사용할 수 있는 스타일을 보여 줍니다.
