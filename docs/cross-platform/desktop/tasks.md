---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: 일반적인 작업 비교
description: 이 문서에는 WPF 및 Xamarin.Forms에 다양 한 일반적인 작업을 수행 하는 방법을 비교 합니다. 단추, 글꼴 크기를 URI를 열고 작업 시트를 표시 합니다. 타이머에 살펴봅니다.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269597"
---
# <a name="common-tasks-comparison"></a>일반적인 작업 비교

| 작업 | WPF | Xamarin.Forms |
|--- |--- |--- |
|단추를 사용 하 여 화면에 메시지를 표시 합니다.|`MessageBox`|`Page.DisplayAlert`|
|타이머를 만듭니다|`DispatcherTimer` 클래스|`Device.StartTimer` 정적 메서드|
|기본 글꼴 크기 가져오기|`SystemFonts` 정적 클래스|`Device.GetNamedSize` 정적 메서드|
|URI/URL 열기|`Process.Start`|`Device.OpenUri`|
|작업 시트 (단추의 목록)를 표시 합니다.|N/A|`Page.DisplayActionSheet`|
