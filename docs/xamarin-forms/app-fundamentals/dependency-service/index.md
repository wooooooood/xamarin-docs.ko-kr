---
title: Xamarin.Forms DependencyService
description: Xamarin.Forms DependencyService 클래스는 Xamarin.Forms 애플리케이션이 공유 코드에서 네이티브 플랫폼 기능을 호출할 수 있도록 하는 서비스 로케이터입니다.
ms.prod: xamarin
ms.assetid: 403479F2-6751-41F2-ADCE-3AF595062FE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
ms.openlocfilehash: ea259d1ee9dc4a94322c38b3e96bee654197bb87
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650449"
---
# <a name="xamarinforms-dependencyservice"></a>Xamarin.Forms DependencyService

## <a name="introductionintroductionmd"></a>[소개](introduction.md)

[`DependencyService`](xref:Xamarin.Forms.DependencyService) 클래스는 Xamarin.Forms 애플리케이션이 공유 코드에서 네이티브 플랫폼 기능을 호출할 수 있도록 하는 서비스 로케이터입니다.

## <a name="registration-and-resolutionregistration-and-resolutionmd"></a>[등록 및 확인](registration-and-resolution.md)

플랫폼 구현을 호출하려면 [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 등록한 후 공유 코드에서 확인해야 합니다.

## <a name="picking-a-photo-from-the-libraryphoto-pickermd"></a>[라이브러리에서 사진 선택](photo-picker.md)

이 문서에서는 Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) 클래스를 사용하여 휴대폰의 사진 라이브러리에서 사진을 선택하는 방법을 설명합니다.
