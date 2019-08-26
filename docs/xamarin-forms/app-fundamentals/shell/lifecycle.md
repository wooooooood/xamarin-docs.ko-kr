---
title: Xamarin.Forms 셸 수명 주기
description: 셸 애플리케이션은 Xamarin.Forms 수명 주기를 기준으로 하며, Appearing 이벤트는 페이지가 화면에 표시되려고 할 때 발생하고, Disappearing 이벤트는 페이지가 화면에서 사라지려고 할 때 발생합니다.
ms.prod: xamarin
ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2019
ms.openlocfilehash: c008cc29d2ceae073459766597040af653329d71
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69893959"
---
# <a name="xamarinforms-shell-lifecycle"></a>Xamarin.Forms 셸 수명 주기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

셸 애플리케이션은 Xamarin.Forms 수명 주기를 기준으로 하며, `Appearing` 이벤트는 페이지가 화면에 표시되려고 할 때 발생하고, `Disappearing` 이벤트는 페이지가 화면에서 사라지려고 할 때 발생합니다. 이 이벤트는 페이지로 전파되며 페이지에서 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 또는 [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 메서드를 재정의하여 처리할 수 있습니다.

> [!NOTE]
> 셸 애플리케이션에서 `Appearing` 및 `Disappearing` 이벤트는 페이지를 표시하거나 화면에서 페이지를 제거하는 플랫폼 코드 전에 플랫폼 간 코드에서 발생합니다.

Xamarin.Forms 앱 수명 주기에 대한 자세한 내용 [Xamarin.Forms 앱 수명 주기](~/xamarin-forms/app-fundamentals/app-lifecycle.md)를 참조하세요.

## <a name="hierarchical-navigation"></a>계층적 탐색

셸 애플리케이션에서 페이지를 탐색 스택에 푸시하면 현재 표시되는 `ShellContent` 개체와 해당 페이지 콘텐츠에서 `Disappearing` 이벤트가 발생합니다. 마찬가지로 탐색 스택의 마지막 페이지가 표시되면 새로 표시되는 `ShellContent` 개체와 해당 페이지 콘텐츠에서 `Appearing` 이벤트가 발생합니다.

계층적 탐색에 대한 자세한 내용은 [Xamarin.Forms 계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)을 참조하세요.

## <a name="modal-navigation"></a>모달 탐색

셸 애플리케이션에서 모달 페이지를 모달 탐색 스택에 푸시하면 표시되는 모든 셸 개체에서 `Disappearing` 이벤트가 발생합니다. 마찬가지로 모달 탐색 스택의 마지막 모달 페이지가 표시되면 표시되는 모든 셸 개체에서 `Appearing` 이벤트가 발생합니다.

모달 탐색에 대한 자세한 내용은 [Xamarin.Forms 모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)를 참조하세요.

## <a name="related-links"></a>관련 링크

- [Xaminals(샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
- [Xamarin.Forms 앱 수명 주기](~/xamarin-forms/app-fundamentals/app-lifecycle.md)
- [Xamarin.Forms 모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)
