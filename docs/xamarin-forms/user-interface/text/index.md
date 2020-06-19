---
title: 텍스트 텍스트Xamarin.Forms
description: Xamarin.Forms에는 텍스트 작업을 위한 세 가지 기본 보기가 있으며,이 문서에서는이를 사용 하 여 응용 프로그램에 텍스트를 입력 하 고 표시 하는 방법을 설명 합니다 Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b44a0b3e3542638874ee366a86967d73c0d53652
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573796"
---
# <a name="text-in-xamarinforms"></a>텍스트 텍스트Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_을 사용 하 여 Xamarin.Forms 텍스트를 입력 하거나 표시 합니다._

Xamarin.Forms에는 텍스트 작업을 위한 세 가지 기본 뷰가 있습니다.

- **[레이블](#label)** &mdash; 단일 또는 여러 줄 텍스트를 표시 하는입니다. 같은 줄에 여러 서식 옵션을 사용 하 여 텍스트를 표시할 수 있습니다.
- **[항목](#entry)** &mdash; 텍스트를 한 줄만 입력할 수 있습니다. 항목에 암호 모드가 있습니다.
- **[편집기](#editor)** &mdash; 텍스트를 입력 하는 경우 두 줄 이상을 사용할 수 있습니다.

기본 제공 스타일 또는 사용자 지정 [스타일](#styles) 을 사용 하 여 텍스트 모양을 변경할 수 있으며 일부 컨트롤은 사용자 지정 [글꼴](#fonts)을 지원 합니다.

## <a name="label"></a>[레이블](label.md)

`Label`뷰는 텍스트를 표시 하는 데 사용 됩니다. 여러 줄의 텍스트 또는 한 줄의 텍스트를 표시할 수 있습니다. `Label`인라인에서 사용 되는 여러 서식 옵션을 사용 하 여 텍스트를 표시할 수 있습니다. 레이블 보기는 텍스트를 한 줄에 맞출 수 없는 경우 텍스트를 래핑하고 자를 수 있습니다.

![레이블 예](images/label.png)

자세한 내용은 [레이블](label.md) 문서를 참조 하세요.

레이블에 사용 되는 글꼴을 사용자 지정 하는 방법에 대 한 자세한 내용은 [글꼴](fonts.md)을 참조 하세요.

## <a name="entry"></a>[항목](entry.md)

`Entry`는 한 줄 텍스트 입력을 허용 하는 데 사용 됩니다. `Entry`색 및 글꼴에 대 한 제어를 제공 합니다. `Entry`암호 모드를 가지 며 텍스트를 입력할 때까지 자리 표시자 텍스트를 표시할 수 있습니다.

![항목 예제](images/entry.png)

자세한 내용은 [항목](entry.md) 문서를 참조 하세요.

와 달리에는 `Label` `Entry` 사용자 지정 글꼴 설정을 사용할 수 없습니다.

## <a name="editor"></a>[편집기](editor.md)

`Editor`는 여러 줄 텍스트 입력을 허용 하는 데 사용 됩니다. `Editor`색 및 글꼴에 대 한 제어를 제공 합니다.

![편집기 예제](images/editor.png)

자세한 내용은 [편집기](editor.md) 문서를 참조 하세요.

## <a name="fonts"></a>[Fonts](fonts.md)

많은 컨트롤은 각 플랫폼의 기본 제공 글꼴이 나 앱에 포함 된 사용자 지정 글꼴을 사용 하 여 다른 글꼴 설정을 지원 합니다. 자세한 내용은 [글꼴](fonts.md) 문서를 참조 하세요.

## <a name="styles"></a>[스타일](styles.md)

여러 컨트롤에서 적용 되는 글꼴, [색](~/xamarin-forms/user-interface/colors.md)및 기타 표시 속성을 설정 하는 방법을 알아보려면 [스타일 작업](~/xamarin-forms/user-interface/styles/index.md) 을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [텍스트 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
