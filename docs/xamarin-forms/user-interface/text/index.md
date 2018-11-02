---
title: Xamarin.Forms에는 텍스트
description: Xamarin.Forms 텍스트를 사용 하기 위한 세 가지 기본 뷰가 있으며이 문서에서는 입력 및 Xamarin.Forms 응용 프로그램에서 표시 되는 텍스트를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2018
ms.openlocfilehash: 60dd54ff8ed06cbeea3a3e202e7058ea7747ea3d
ms.sourcegitcommit: 06a52ac36031d0d303ac7fc8163a59c178799c80
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50911569"
---
# <a name="text-in-xamarinforms"></a>Xamarin.Forms에는 텍스트

_Xamarin.Forms를 사용 하 여 텍스트를 입력 하거나 표시 합니다._

Xamarin.Forms에 텍스트를 사용 하 여 작업에 대 한 세 가지 기본 뷰가 있습니다.

- **[레이블을](#Label)**  &mdash; 단일 또는 여러 줄 텍스트를 제공 합니다. 같은 줄에서 여러 서식 옵션을 사용 하 여 텍스트를 표시할 수 있습니다.
- **[항목](#Entry)**  &mdash; 선이 하나만 있는 텍스트를 입력 합니다. 항목에는 암호 모드에 있습니다.
- **[편집기](#Editor)**  &mdash; 둘 이상의 행을 사용할 수 있는 텍스트를 입력 합니다.

기본 제공 또는 사용자 지정을 사용 하 여 텍스트 모양을 변경할 수 있습니다 [스타일](#Styles) 일부 컨트롤 사용자 지정 지원과 [글꼴](#Fonts)합니다.

<a name="Label" />

## <a name="labellabelmd"></a>[레이블](label.md)

`Label` 보기가 사용 되는 텍스트를 표시 합니다. 여러 줄의 텍스트 또는 텍스트 한 줄 표시할 수 있습니다. `Label` 인라인에 사용 되는 여러 서식 옵션을 사용 하 여 텍스트를 표시할 수 있습니다. 레이블 뷰가 줄 바꿈 하거나 한 줄에 표시할 수 없을 때 텍스트를 자를 수 있습니다.

![](images/label.png "레이블 예제")

참조 된 [레이블](label.md) 자세한 문서.

레이블에 사용 된 글꼴을 사용자 지정에 대 한 자세한 내용은 [글꼴](fonts.md)합니다.

<a name="Entry" />

## <a name="entryentrymd"></a>[항목](entry.md)

`Entry` 한 줄 텍스트 입력을 허용 하는 데 사용 됩니다. `Entry` 제품 색 및 글꼴 제어 합니다. `Entry` 암호 모드 있으며 텍스트를 입력할 때까지 자리 표시자 텍스트를 표시할 수 있습니다.

![](images/entry.png "항목 예")

참조를 [항목이](entry.md) 자세한 문서.

이때 달리 `Label`, `Entry` 사용자 지정 글꼴 설정을 사용할 수 없습니다.

<a name="Editor" />

## <a name="editoreditormd"></a>[편집기](editor.md)

`Editor` 여러 줄 텍스트 입력을 허용 하는 데 사용 됩니다. `Editor` 제품 색 및 글꼴 제어 합니다.

![](images/editor.png "편집기 예제")

참조 된 [편집기](editor.md) 자세한 문서.

<a name="Fonts" />

## <a name="fontsfontsmd"></a>[글꼴](fonts.md)

`Label` 컨트롤은 기본 제공 글꼴을 사용 하 여 각 플랫폼에 사용자 지정 글꼴을 앱에 포함 된 다른 글꼴 설정을 지원 합니다. 참조 된 [글꼴](fonts.md) 자세한 문서.

<a name="Styles" />

## <a name="stylesstylesmd"></a>[스타일](styles.md)

가리킵니다 [스타일을 사용 하 여 작업](~/xamarin-forms/user-interface/styles/index.md) 글꼴을 설정 하는 방법을 알아보려면 [색](~/xamarin-forms/user-interface/colors.md), 다른 표시 하는 속성과 여러 컨트롤에 적용 합니다.

## <a name="related-links"></a>관련 링크

- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
