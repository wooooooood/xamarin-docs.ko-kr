---
title: 'title: “Xamarin.Forms 셸 수명 주기” description: “셸 애플리케이션은 Xamarin.Forms 수명 주기를 기준으로 하며, Appearing 이벤트는 페이지가 화면에 표시되려고 할 때 발생하고, Disappearing 이벤트는 페이지가 화면에서 사라지려고 할 때 발생합니다.”'
description: 'ms.prod: xamarin ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 07/25/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3a7a46187d861098b61f638a3fb460d890b081dd
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84138725"
---
# <a name="xamarinforms-shell-lifecycle"></a>Xamarin.Forms Shell 수명 주기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

셸 애플리케이션은 Xamarin.Forms 수명 주기를 기준으로 하며, `Appearing` 이벤트는 페이지가 화면에 표시되려고 할 때 발생하고, `Disappearing` 이벤트는 페이지가 화면에서 사라지려고 할 때 발생합니다. 이 이벤트는 페이지로 전파되며 페이지에서 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 또는 [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 메서드를 재정의하여 처리할 수 있습니다.

> [!NOTE]
> 셸 애플리케이션에서 `Appearing` 및 `Disappearing` 이벤트는 페이지를 표시하거나 화면에서 페이지를 제거하는 플랫폼 코드 전에 플랫폼 간 코드에서 발생합니다.

Xamarin.Forms 앱 수명 주기에 대한 자세한 내용은 [Xamarin.Forms 앱 수명 주기](~/xamarin-forms/app-fundamentals/app-lifecycle.md)를 참조하세요.

## <a name="hierarchical-navigation"></a>계층적 탐색

셸 애플리케이션에서 페이지를 탐색 스택에 푸시하면 현재 표시되는 `ShellContent` 개체와 해당 페이지 콘텐츠에서 `Disappearing` 이벤트가 발생합니다. 마찬가지로 탐색 스택의 마지막 페이지가 표시되면 새로 표시되는 `ShellContent` 개체와 해당 페이지 콘텐츠에서 `Appearing` 이벤트가 발생합니다.

계층적 탐색에 대한 자세한 내용은 [Xamarin.Forms 계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

## <a name="modal-navigation"></a>모달 탐색

셸 애플리케이션에서 모달 페이지를 모달 탐색 스택에 푸시하면 표시되는 모든 셸 개체에서 `Disappearing` 이벤트가 발생합니다. 마찬가지로 모달 탐색 스택의 마지막 모달 페이지가 표시되면 표시되는 모든 셸 개체에서 `Appearing` 이벤트가 발생합니다.

모달 탐색에 대한 자세한 내용은 [Xamarin.Forms 모달 탐색](~/xamarin-forms/app-fundamentals/navigation/modal.md)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms 앱 수명 주기](~/xamarin-forms/app-fundamentals/app-lifecycle.md)
- [Xamarin.Forms 모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)
