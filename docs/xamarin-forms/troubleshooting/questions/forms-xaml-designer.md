---
title: Visual Studio XAML 디자이너가 Xamarin.Forms XAML 파일에 대해 작동하지 않는 이유는 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: cab2eefb-c52f-4d81-866e-8f1feabbdd64
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: ce318faab1af07b95a7769d81a506979b37a34e4
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842997"
---
# <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-files"></a>Visual Studio XAML 디자이너가 Xamarin.Forms XAML 파일에 대해 작동하지 않는 이유는 무엇인가요?

Xamarin.ios는 현재 XAML 파일에 대 한 비주얼 디자이너를 지원 하지 않습니다. 이로 인해 Visual Studio의 *XAML Ui 디자이너* 또는 *인코딩을 사용 하는 xaml Ui 디자이너*에서 Forms xaml 파일을 열려고 하면 다음과 같은 오류 메시지가 throw 됩니다.

> "선택한 편집기를 사용 하 여 파일을 열 수 없습니다. 다른 편집기를 선택 하십시오. "

이러한 제한 사항은 [XAMARIN.IOS XAML 기본 사항](~/xamarin-forms/xaml/xaml-basics/index.md) 가이드에 설명 되어 있습니다.

> "Xamarin.ios 응용 프로그램에서 XAML을 생성 하기 위한 비주얼 디자이너는 아직 없지만 모든 XAML을 직접 작성 해야 합니다."

그러나 Xamarin.ios XAML 미리 보기는 **다른 창 > > 보기** 를 선택 하 여 표시할 수 있습니다. 폼의 미리 보기 메뉴 옵션입니다.
