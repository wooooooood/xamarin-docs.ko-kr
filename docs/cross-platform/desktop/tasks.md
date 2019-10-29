---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: 일반 작업 비교
description: 이 문서에서는 WPF 및 Xamarin.ios에서 다양 한 일반 작업을 수행 하는 방법을 비교 합니다. 단추, 타이머, 글꼴 크기, URI 열기 및 작업 시트 표시를 확인 합니다.
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: d1f1430d8044f94c17a19d747334b5ed8a1441cf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016426"
---
# <a name="common-tasks-comparison"></a>일반 작업 비교

| 작업 | WPF | Xamarin.Forms |
|--- |--- |--- |
|화면에 단추가 있는 메시지를 표시 합니다.|`MessageBox`|`Page.DisplayAlert`|
|타이머 만들기|`DispatcherTimer` 클래스|`Device.StartTimer` 정적 메서드|
|기본 글꼴 크기 가져오기|정적 클래스 `SystemFonts`|`Device.GetNamedSize` 정적 메서드|
|URI/URL 열기|`Process.Start`|`Device.OpenUri`|
|작업 시트 표시 (단추 목록)|N/A|`Page.DisplayActionSheet`|
