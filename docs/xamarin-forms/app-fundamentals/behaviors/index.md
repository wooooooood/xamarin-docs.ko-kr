---
title: Xamarin.Forms 동작
description: 동작(Behavior)을 사용하면 서브클래스 없이도 사용자 인터페이스 컨트롤에 기능을 추가할 수 있습니다. 동작은 코드로 작성되고 XAML 또는 코드 내의 컨트롤에 추가됩니다.
ms.prod: xamarin
ms.assetid: 42E32AD7-8E3B-48B3-B402-E75B758DA913
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: df0a767976247166205ae8a3d70fd59c521646f6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997161"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms 동작

_동작(Behavior)을 사용하면 서브클래스 없이도 사용자 인터페이스 컨트롤에 기능을 추가할 수 있습니다. 동작은 코드로 작성되고 XAML 또는 코드 내의 컨트롤에 추가됩니다._

## <a name="introduction-to-behaviorsintroductionmd"></a>[동작 소개](introduction.md)

동작을 사용하면 코드가 컨트롤에 간결하게 첨부될 수 있도록 컨트롤의 API와 직접 상호 작용하기 때문에, 대개 코드 숨김으로 작성해야 하는 코드를 구현할 수 있습니다. 이 문서에는 동작을 소개합니다.

## <a name="attached-behaviorsattachedmd"></a>[연결된 동작](attached.md)

첨부된 동작은 첨부된 속성이 하나 이상 있는 `static` 클래스입니다. 이 문서에서는 첨부된 동작을 만들고 사용하는 방법을 보여줍니다.

## <a name="xamarinforms-behaviorscreatingmd"></a>[Xamarin.Forms 동작](creating.md)

Xamarin.Forms 동작은 [`Behavior`](xref:Xamarin.Forms.Behavior) 또는 [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) 클래스에서 파생되어 만들어집니다. 이 문서에서는 Xamarin.Forms 동작을 만들고 사용하는 방법을 보여줍니다.

## <a name="reusable-behaviorsreusableindexmd"></a>[재사용 가능한 동작](reusable/index.md)

동작은 둘 이상의 애플리케이션에서 다시 사용할 수 있습니다. 이 문서에서는 일반적으로 사용되는 기능을 수행하는 유용한 동작을 만드는 방법을 설명합니다.
