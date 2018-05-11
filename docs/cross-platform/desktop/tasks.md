---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: 일반적인 작업 비교
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b4d45ae61c5d7a7093d51c4440d11dd883c157a4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="common-tasks-comparison"></a>일반적인 작업 비교

| 작업 | WPF | Xamarin.Forms |
|--- |--- |--- |
|단추가 있는 화면에 메시지를 표시 합니다.|`MessageBox`|`Page.DisplayAlert`|
|타이머 만들기|`DispatcherTimer` 클래스|`Device.StartTimer` 정적 메서드|
|기본 글꼴 크기 가져오기|`SystemFonts` 정적 클래스|`Device.GetNamedSize` 정적 메서드|
|URI/URL 열기|`Process.Start`|`Device.OpenUri`|
|작업 시트 (단추 목록)을 표시 합니다.|N/A|`Page.DisplayActionSheet`|
