---
title: Xamarin.Forms 지역화
description: Xamarin.Forms 사용한 플랫폼 간 다국어 응용 프로그램을 빌드하는 기본 제공.net 지역화를 사용할 수 있습니다. 텍스트 및 이미지 지역화할 수 있는 및 응용 프로그램에는 오른쪽에서 왼쪽 흐름 방향 지원할 수 있습니다.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
ms.openlocfilehash: 71033e935a2d3a4be88dbcc5d975938771484640
ms.sourcegitcommit: b60a37587aad8a0bfa8a522d88d22fa672002443
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285575"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms 지역화

_Xamarin.Forms 사용한 플랫폼 간 다국어 응용 프로그램을 빌드하는 기본 제공.net 지역화를 사용할 수 있습니다._

## <a name="string-and-image-localizationtextmd"></a>[문자열 및 이미지 지역화](text.md)

.NET 응용 프로그램 사용의 지역화를 위한 기본 제공 메커니즘이 [RESX 파일](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files) 및의 클래스는 `System.Resources` 및 `System.Globalization` 네임 스페이스입니다. RESX 파일 번역 된 문자열을 포함 하는 번역에 대 한 강력한 형식의 액세스를 제공 하는 컴파일러에서 생성 된 클래스와 함께 Xamarin.Forms 어셈블리에 포함 됩니다. 코드에서 번역 된 텍스트를 검색할 수 있습니다.

## <a name="right-to-left-localizationright-to-leftmd"></a>[오른쪽에서 왼쪽으로 지역화](right-to-left.md)

흐름 방향은 눈으로 페이지의 UI 요소를 검색 하는 방향입니다. 오른쪽에서 왼쪽 지역화 Xamarin.Forms 응용 프로그램에 오른쪽에서 왼쪽 흐름 방향에 대 한 지원을 추가합니다.
