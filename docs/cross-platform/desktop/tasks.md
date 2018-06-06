---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: 일반적인 작업 비교
description: 이 문서에는 WPF 및 Xamarin.Forms에 다양 한 일반적인 작업을 수행 하는 방법을 비교 합니다. 단추, 타이머, URI를 열고 표시 작업 시트 글꼴 크기에 살펴봅니다.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780478"
---
# <a name="common-tasks-comparison"></a>일반적인 작업 비교

| 작업 | WPF | Xamarin.Forms |
|--- |--- |--- |
|단추가 있는 화면에 메시지를 표시 합니다.|`MessageBox`|`Page.DisplayAlert`|
|타이머 만들기|`DispatcherTimer` 클래스|`Device.StartTimer` 정적 메서드|
|기본 글꼴 크기 가져오기|`SystemFonts` 정적 클래스|`Device.GetNamedSize` 정적 메서드|
|URI/URL 열기|`Process.Start`|`Device.OpenUri`|
|작업 시트 (단추 목록)을 표시 합니다.|N/A|`Page.DisplayActionSheet`|
